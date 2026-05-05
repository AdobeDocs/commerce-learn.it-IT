---
title: 'Dividi pagamento POC: decisioni su architettura e progettazione'
description: Scopri in che modo il POC di pagamento frazionato mappa i passaggi di sincronizzazione dell’estrazione su Commerce e basati sull’I/O in App Builder, con attributi di estensione, REST e cinque casi limite dei plug-in.
feature: App Builder, Eventing, Extensibility, Paas, Payments, REST
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 293
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 9add0b4bfa1eba33ec359adaa766b64711df25ba
workflow-type: tm+mt
source-wordcount: '863'
ht-degree: 1%

---

# Dividi pagamento POC: decisioni su architettura e progettazione

In questa pagina vengono illustrate le scelte architettoniche alla base del concetto di split payment proof of concept. Leggilo prima di utilizzare i prompt di build in questa serie, in modo da comprendere come è strutturato ogni componente e come adattare i modelli nel tuo progetto.

## Principio fondamentale

La prova di concetto non riguarda l&#39;implementazione di split payment più elegante. Si tratta di mostrare **come iniziare a spostare la logica di Commerce in App Builder senza una riscrittura significativa**.

La regola applicata in è:

> **Se qualcosa deve essere eseguito in modo sincrono nel ciclo di richiesta di Commerce o deve richiamare API interne a Commerce che non hanno una superficie esterna pulita, rimane in PHP. Tutto il resto viene spostato in App Builder.**

## Cosa vive in Commerce (PHP) e perché

### &#x200B;1. Archivia richiesta di credito: `PlaceOrderPlugin`

Il credito dell&#39;archivio è applicato al carrello utilizzando `Magento\CustomerBalance\Api\BalanceManagementInterface::apply()`. Questo metodo funziona solo su un carrello **attivo**. Il carrello diventa inattivo nel momento in cui viene effettuato l’ordine. App Builder riceve l&#39;evento di I/O *dopo* che l&#39;ordine è stato effettuato, pertanto non è possibile applicare il credito dello store da App Builder.

**La lezione:** tutto ciò che deve cambiare lo stato del carrello prima che il posizionamento dell&#39;ordine venga eseguito in Commerce. Nessuna soluzione alternativa.

### &#x200B;2. Soglia guardia sincrona: `CheckoutPlugin`

Il controllo soglia ordine da $ 100 deve bloccare il cliente al passaggio di pagamento, prima che selezioni **[!UICONTROL Place Order]**. La risposta deve essere sincrona nel ciclo di richiesta di Commerce. App Builder è basato su eventi e asincrono, pertanto non può restituire un errore immediato in quel momento.

App Builder *also* convalida la soglia (come controllo di audit), ma l&#39;esperienza del cliente dipende dal controllo Commerce eseguito per primo.

### &#x200B;3. Endpoint REST personalizzati: `webapi.xml` e `SplitPaymentManagement`

I seguenti endpoint devono:

* Richiama `SplitInvoiceService` (fatture che utilizzano il servizio fatture interno di Commerce)
* Richiama `ShipOrder::execute()` (servizio di spedizione interno di Commerce)
* Aggiornare lo stato e lo stato dell&#39;ordine con la macchina dello stato dell&#39;ordine di Commerce

`/V1/split-payment/orders/:id/cash-received` e `/V1/split-payment/orders/:id/cash-decline`

Non esiste uno strato REST pubblico per questo comportamento, quindi Commerce espone gli endpoint. App Builder li chiama.

### &#x200B;4. Suddividere gli importi su preventivo e ordine: osservatori e plug-in

Commerce richiede gli importi suddivisi (`split_store_credit_amount`, `split_cash_amount`, `split_cash_status`) nell&#39;ordine, sia per le risposte REST lette da App Builder che per la visualizzazione ordine **[!UICONTROL Admin]**. Gli importi sono allegati con attributi di estensione e vengono copiati dal preventivo nell&#39;ordine in un osservatore su `sales_model_service_quote_submit_before`.

## Cosa vive in App Builder e perché

### &#x200B;1. Elaborazione ordine basato su eventi: `payment-orchestrator`

Dopo l&#39;attivazione di `sales_order_place_before`, App Builder riceve l&#39;evento. Riconvalida la soglia (come controllo), registra un commento *cash pending* sull&#39;ordine e aggiorna lo stato dell&#39;ordine. Niente di tutto questo richiede un nuovo PHP, solo REST in Commerce.

### &#x200B;2. Accettazione contanti: `payment-accept`

Quando un ERP (o un operatore nel dashboard) conferma la ricezione di contanti, `payment-accept` chiama `POST /V1/split-payment/orders/:id/cash-received`. Fattura, spedizione e stato degli ordini vengono gestiti in Commerce. App Builder è il trigger.

### &#x200B;3. Rifiuto contanti: `payment-decline`

`payment-decline` chiama `POST /V1/split-payment/orders/:id/cash-decline` e Commerce annulla l&#39;ordine. Stesso modello di accettazione di contante.

### &#x200B;4. Dashboard operatore: `demo-dashboard`

Dashboard HTML indipendente gestito da un’azione web App Builder. Recupera gli ordini in attesa di contante da Commerce REST e fornisce **[!UICONTROL Accept]** / **[!UICONTROL Decline]** azioni che richiamano le azioni App Builder di cui sopra. Commerce **[!UICONTROL Admin]** non è obbligatorio.

## La soglia: applicata due volte di proposito

```text
Customer at checkout
        |
        v
[Commerce: CheckoutPlugin]     <- Synchronous, blocks immediately, user sees error
        |
        |  (if somehow bypassed: direct API call, and so on)
        v
[Order placed] -> I/O Event -> [App Builder: payment-orchestrator]
                                        |
                                        v
                              [evaluateThreshold()]  <- Async audit, records failure comment
```

**Commerce è il proprietario della protezione rivolta all&#39;utente; App Builder è il proprietario del controllo post-posizionamento.** È intenzionale.

## Il credito del negozio: perché rimane in PHP

```text
What you might think would work (it does not):
  Order placed -> I/O Event -> App Builder -> PUT /V1/carts/:id/store-credit
  (Fails: cart is inactive after place order)

What actually works:
  AroundPlaceOrder plugin
  -> BalanceManagementInterface::apply($cartId, $amount)  <- cart is still active
  -> place order
  -> order placed
  -> I/O event: App Builder (store credit is already applied)
```

Il file `store-credit.js` nell&#39;orchestrator documenta questo. Si tratta di uno stub no-op con commenti che spiegano perché non viene utilizzato.

## Attributi di estensione: l’associazione

Gli importi suddivisi si spostano nel sistema sugli attributi dell&#39;estensione:

```text
Checkout JavaScript (Knockout)
    |  POST /V1/split-payment/set
    v
SplitPaymentSession (PHP session)
    |  AroundPlaceOrder reads the session
    v
CartInterface extension attributes
    |  `sales_model_service_quote_submit_before` observer
    v
OrderInterface extension attributes -> `sales_order` flat columns
    |  I/O event payload includes these fields
    v
App Builder `payment-orchestrator` reads the split amounts
```

## Modello dati

**`sales_order`colonne piatte aggiunte da questo modulo**

| Colonna | Tipo | Finalità |
| --- | --- | --- |
| `split_store_credit_amount` | galleggiare | Credito del Negozio applicato |
| `split_cash_amount` | galleggiare | Importo in contanti dovuto |
| `split_cash_status` | varchar | `pending`, `received` o `declined` |
| `split_sc_invoice_id` | int | ID entità della fattura di credito del punto vendita |
| `split_cash_invoice_id` | int | ID entità della fattura di cassa |

**Attributi di estensione** (in `CartInterface`, `OrderInterface` e `OrderPaymentInterface`)

* `split_store_credit_amount` (virgola mobile)
* `split_cash_amount` (virgola mobile)
* `split_cash_status` (stringa)

## Campi payload dell’evento di I/O

`observer.sales_order_place_before` è configurato in `io_events.xml` per includere quanto segue nell&#39;evento:

```xml
entity_id, quote_id, increment_id, subtotal,
split_store_credit_amount, split_cash_amount, split_cash_status
```

App Builder utilizza `entity_id` come ID ordine e `split_store_credit_amount` e `split_cash_amount` per la convalida della soglia.

## I cinque casi edge coperti dalla prova di concetto

### 1. `CapCustomerBalanceCollectPlugin`

L&#39;agente di raccolta totale **[!UICONTROL Customer balance]** nativo di Commerce può applicare un numero eccessivo di righe (può visualizzare l&#39;intero saldo disponibile, non l&#39;importo suddiviso dichiarato dalla sessione). Questo plug-in limita la quantità al valore dichiarato nella sessione.

### 2. `FixSplitPaymentGrandTotalPlugin`

Dopo l&#39;applicazione del credito del negozio, il preventivo **[!UICONTROL Grand Total]** può essere eliminato all&#39;importo solo contanti. Il JavaScript di estrazione deve calcolare il totale dell&#39;ordine per la convalida di suddivisione *prima* che cambi. Il plug-in viene eseguito dopo la raccolta dei totali e corregge la visualizzazione, mentre JavaScript non considera attendibile `grand_total` da solo e ricostruisce il valore dai segmenti dei subtotali.

### 3. `FixInvoiceCustomerBalanceAfterTotalsPlugin`

Quando vengono recuperati i totali delle fatture, il credito di magazzino può essere applicato due volte. Questo plug-in corregge `customer_balance_amount` sulle fatture.

### 4. `SplitPaymentZeroTotalPlugin`

Dopo aver applicato il credito dell&#39;archivio, il carrello **[!UICONTROL Grand Total]** può essere $0 (ordine di credito dell&#39;archivio completo). Il controllo **[!UICONTROL Zero subtotal checkout]** di Commerce può bloccare il COD in questo caso. Questo plug-in consente il contrassegno quando l’importo contanti della sessione è maggiore di 0.

### &#x200B;5. Recupero preventivo prima di `BalanceManagementInterface::apply()`

`apply()` controlla l&#39;importo rispetto all&#39;attuale **[!UICONTROL Grand Total]**. Se il totale è già solo la parte contante, `apply()` può non riuscire o raggiungere il limite massimo. `PlaceOrderPlugin` sospende temporaneamente la correzione del totale complessivo mentre viene applicato il saldo, utilizzando un flag di sessione (`beginBalanceApply` / `endBalanceApply`).


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
