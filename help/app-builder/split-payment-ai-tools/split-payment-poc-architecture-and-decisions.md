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
source-git-commit: 1e2c7e0e6d0f2d174b88406ce3fb7c787676ecee
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

* `split_store_credit_amount` (float)
* `split_cash_amount` (float)
* `split_cash_status` (string)

## I/O event payload fields

`observer.sales_order_place_before` is configured in `io_events.xml` to include the following in the event:

```xml
entity_id, quote_id, increment_id, subtotal,
split_store_credit_amount, split_cash_amount, split_cash_status
```

App Builder uses `entity_id` as the order ID and `split_store_credit_amount` and `split_cash_amount` for threshold validation.

## The five edge cases the proof of concept covers

### 1. `CapCustomerBalanceCollectPlugin`

Commerce’s native **[!UICONTROL Customer balance]** total collector can over-apply (it can see the full available balance, not the session-declared split amount). This plugin caps the amount to the value declared in the session.

### 2. `FixSplitPaymentGrandTotalPlugin`

After store credit is applied, the quote **[!UICONTROL Grand Total]** can drop to the cash-only amount. The checkout JavaScript must compute the order total for split validation *before* that change. The plugin runs after totals collection and corrects the display, while the JavaScript does not trust `grand_total` alone and reconstructs the value from subtotal segments.

### 3. `FixInvoiceCustomerBalanceAfterTotalsPlugin`

When invoice totals are recollected, store credit can be applied twice. This plugin corrects `customer_balance_amount` on invoices.

### 4. `SplitPaymentZeroTotalPlugin`

After store credit is applied, the cart **[!UICONTROL Grand Total]** can be $0 (full store credit order). Commerce’s **[!UICONTROL Zero subtotal checkout]** check can block COD in that case. This plugin allows COD when the session cash amount is greater than 0.

### 5. Quote recollection before `BalanceManagementInterface::apply()`

`apply()` checks the amount against the current **[!UICONTROL Grand Total]**. If the total is already the cash portion only, `apply()` can fail or cap. `PlaceOrderPlugin` temporarily suspends the grand-total fix while balance is applied, using a session flag (`beginBalanceApply` / `endBalanceApply`).


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
