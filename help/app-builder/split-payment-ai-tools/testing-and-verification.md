---
title: 'Dividi pagamento POC: guida al test e alla verifica'
description: Scopri come verificare il POC di pagamento frazionato. Installazione di Commerce, REST, pagamento, soglia, accettazione e rifiuto della simulazione, dashboard dimostrativa e registri di App Builder.
feature: App Builder, Configuration, Extensibility, Paas, Payments, REST, Orders
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 359
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 8dfbf2694378aae76c91afa11bfee7d93077d8ba
workflow-type: tm+mt
source-wordcount: '907'
ht-degree: 0%

---

# Dividi pagamento POC: guida al test e alla verifica

Questa guida illustra come verificare che ogni componente funzioni correttamente, nell’ordine in cui deve essere testato. Inizia dal basso (modulo Commerce) e continua (App Builder).


## Passaggio 1: verificare l&#39;installazione del modulo Commerce

Dopo l&#39;esecuzione di `bin/magento setup:upgrade` e `bin/magento setup:di:compile`:

```bash
# Confirm module is enabled
bin/magento module:status Client_SplitPayment

# Confirm db_schema columns were added
bin/magento doctrine:schema:validate
# or check directly:
mysql -u <user> -p <dbname> -e "DESCRIBE sales_order;" | grep split
```

Output previsto: quattro colonne a partire da `split_` visibili in `sales_order`.


## Passaggio 2 — Verificare la configurazione dell&#39;amministratore Commerce

In Amministrazione Commerce:
1. **[!UICONTROL Stores]** > **[!UICONTROL Configuration]** > **[!UICONTROL Sales]** > **[!UICONTROL Payment Methods]** — conferma che **[!UICONTROL Cash On Delivery]** è abilitato con il titolo `Cash`
2. **[!UICONTROL Stores]** > **[!UICONTROL Configuration]** > **[!UICONTROL Customers]** > **[!UICONTROL Customer Configuration]** > **[!UICONTROL Store Credit Options]** — conferma abilitata
3. Conferma che il cliente del test ha il credito dell&#39;archivio: **[!UICONTROL Customers]** > **[!UICONTROL All Customers]** > *[cliente]* > **[!UICONTROL Store Credit]**


## Passaggio 3 — Verificare che gli endpoint REST siano accessibili

Utilizza curl per confermare che gli endpoint rispondono (rifiuteranno le richieste non autorizzate, ma un errore 401 conferma che sono instradati correttamente):

```bash
# Should return 401 (not 404) — endpoint exists but requires auth
curl -X POST https://your-store.example.com/rest/V1/split-payment/orders/1/cash-received

# Should return 200 or session-based response — anonymous endpoint
curl -X POST https://your-store.example.com/rest/V1/split-payment/set \
  -H "Content-Type: application/json" \
  -d '{"storeCreditAmount": 0, "cashAmount": 0}'
```


## Passaggio 4 — Verificare l&#39;interfaccia utente Checkout

1. Accedi allo storefront come cliente di prova (che dispone di credito in negozio)
2. Aggiungi un prodotto al carrello (totale inferiore a $100 dopo la spedizione + imposte)
3. Procedi con il pagamento
4. Al passaggio di pagamento, seleziona **Contanti** (o Contanti alla consegna)
5. Verifica che vengano visualizzati i campi per il pagamento frazionato:
   * Saldo credito del tuo Negozio visualizzato
   * Campo Importo contanti precompilato con il totale dell&#39;ordine
   * Il campo &quot;Memorizza credito per questo ordine&quot; mostra $0,00 (poiché contanti = totale ordine completo)
6. Ridurre l&#39;importo in contanti (ad esempio, immettere 10 dollari per un ordine di 50 dollari)
7. Verifica che la quota di credito del negozio venga aggiornata a $ 40,00
8. Verificare che venga visualizzato il messaggio: `"The remaining $40.00 will automatically be applied from your store credit."`

**Convalida test:**
* Inserire un importo in contanti maggiore del messaggio di errore → totale dell&#39;ordine
* Immettere un importo in contanti che richiede un credito del negozio maggiore del messaggio di errore → disponibile
* Inserire il contante = errore 0 → (o il credito dello store copre l&#39;intero ordine)


## Passaggio 5 — Soglia di controllo

1. Aggiungere prodotti per un totale superiore a 100 dollari (subtotale + spedizione + imposte > 100 dollari)
2. Procedi all&#39;estrazione, seleziona **Contanti**
3. Tentativo di effettuare l&#39;ordine
4. Verificare che venga visualizzato il messaggio di errore: `"Payment could not be processed. Please try again or contact support."`
5. Verifica che il carrello sia mantenuto (il cliente può ancora regolare il carrello e riprovare)


## Passaggio 6 — Inserimento di un ordine di pagamento frazionato di test

1. Crea un carrello inferiore a $100 (hai effettuato l’accesso al cliente con il credito del negozio)
2. Seleziona contanti al pagamento
3. Inserire un importo in contanti inferiore al totale dell&#39;ordine (ad esempio, $ 10 di un ordine di $ 45)
4. Conferma la visualizzazione del messaggio di credito del negozio
5. Fai clic su **Inserisci ordine**

Dopo il posizionamento dell’ordine, verifica in Commerce Admin:
* L&#39;ordine è nello stato `pending_payment`
* L’ordine ha due commenti sulla cronologia:
   1. `"Cash payment of $X.XX pending. Awaiting admin confirmation."` (da App Builder `payment-orchestrator`)
   2. `"Split payment orchestration completed. Order awaiting cash confirmation."` (da App Builder)
* Gli importi del pagamento frazionato sono visibili nel blocco di pagamento dell’ordine

> **Se non vengono visualizzati commenti di App Builder:** controllare i registri delle azioni di App Builder con `aio app logs`. L’evento non può essere stato attivato o l’azione può presentare un errore.


## Passaggio 7 — Testare l&#39;accettazione tramite lo script di simulazione

Lo script di simulazione è il modo più veloce per testare il flusso di accettazione/rifiuto senza l’interfaccia utente dell’operatore completa.

```bash
cd commerce-checkout-starter-kit
cp commerce-backend-ui-1/.env.simulation.example commerce-backend-ui-1/.env.simulation
# Edit .env.simulation with your credentials

# List recent orders (find your test order entity_id)
node commerce-backend-ui-1/scripts/simulate-split-payment.mjs list

# Show split payment fields for a specific order
node commerce-backend-ui-1/scripts/simulate-split-payment.mjs show 42

# Accept the cash payment
node commerce-backend-ui-1/scripts/simulate-split-payment.mjs accept 42
```

Dopo l’accettazione, verifica nella visualizzazione degli ordini di amministrazione di Commerce:
* Lo stato dell&#39;ordine è `processing`
* Commento cronologia: `"Cash payment of $X.XX received."`
* Fattura di cassa creata (visibile nella scheda Fatture)
* Spedizione creata (visibile nella scheda Spedizioni, se applicabile)
* Commento cronologia: `"Split payment: cash portion invoiced #XXXXXXXX."`
* Commento cronologia: `"Split payment: shipment created after cash was accepted (App Builder / API)."`


## Passaggio 8 — Test del rifiuto tramite script di simulazione

Eseguire un altro ordine di test (la stessa impostazione del passaggio 6), quindi:

```bash
node commerce-backend-ui-1/scripts/simulate-split-payment.mjs decline <orderId>
```

Dopo il rifiuto, verifica in Amministratore Commerce:
* Lo stato dell&#39;ordine è `canceled`
* Commento cronologia: `"Cash payment declined (simulated fraud check)."`
* `split_cash_status` = `declined`


## Passaggio 9 — Testare il dashboard demo

Dopo aver distribuito `split-payment-orchestrator`, `aio app deploy` stampa gli URL dell&#39;azione.

Apri l&#39;URL `demo-dashboard` in un browser:

```
https://[runtime-host]/api/v1/web/split_payment_orchestrator/demo-dashboard
```

Se `DEMO_UI_SECRET` è impostato:

```
https://[runtime-host]/api/v1/web/split_payment_orchestrator/demo-dashboard?secret=<your-secret>
```

Con un ordine in sospeso:
1. Il dashboard deve mostrare l’ordine nell’elenco in sospeso
2. Fai clic su **Accetta** → l&#39;ordine deve essere spostato su `processing` in Commerce
3. Inserisci un altro ordine; fai clic su **Rifiuta** → l&#39;ordine deve essere `canceled` in Commerce


## Passaggio 10 — Verifica dei registri di azioni di App Builder

```bash
# Follow logs in real-time
aio app logs --tail

# Or view last invocations
aio runtime activation list --limit 10
aio runtime activation logs <activation-id>
```

Per `payment-orchestrator`, cercare:

```
[INFO] Split payment orchestration finished { orderId: '42' }
```

Per `payment-accept` o `payment-decline`:

```
[INFO] Cash payment accepted on Commerce via REST { orderId: '42' }
```


## Problemi comuni e correzioni

### &quot;La firma non è valida&quot; da Commerce OAuth

**Causa:** sfasamento di orario tra App Builder Runtime e Commerce o bug della firma OAuth.

**Correzione:**
* Conferma `COMMERCE_BASE_URL` senza barra finale
* Conferma le quattro credenziali OAuth per un&#39;integrazione _attivato_
* Eseguire prima il test con lo script di simulazione. Se lo script funziona ma App Builder non lo fa, è probabile che non venga caricata una variabile di ambiente (verificare l&#39;output `aio app deploy` per verificare che non siano presenti variabili di ambiente)

### I campi di pagamento frazionato non vengono visualizzati nell&#39;estrazione

**Causa:** i percorsi di iniezione LayoutProcessorPlugin non corrispondono al layout di estrazione.

**Correzione:**
* Controlla la console del browser per individuare eventuali errori RequireJS durante il caricamento di `Client_SplitPayment/js/view/payment/split-payment`
* Verifica `bin/magento setup:static-content:deploy` completata
* Svuota cache: `bin/magento cache:flush`
* Se il tema ha un&#39;estrazione personalizzata, il percorso `LayoutProcessorPlugin` per inserire il componente potrebbe richiedere una regolazione

### Il credito del Negozio non si applica / &quot;Il pagamento non può essere elaborato&quot; al momento dell&#39;ordine

**Causa:** In genere uno dei plug-in per casi edge non funziona correttamente.

**Verifica:**
1. `SplitPaymentSession` restituisce gli importi corretti? Aggiungi registrazione di debug temporanea in `PlaceOrderPlugin`
2. `FixSplitPaymentGrandTotalPlugin` è in esecuzione e influisce sul totale delle offerte prima che venga chiamato `BalanceManagementInterface::apply()`? Il flag `beginBalanceApply()` deve essere eliminato.
3. Il modulo di bilanciamento cliente è abilitato in Commerce?

### L’azione App Builder riceve un evento ma orderId è mancante

**Causa:** L&#39;elenco dei campi `io_events.xml` non include `entity_id` oppure la forma del payload dell&#39;evento è stata modificata.

**Correzione:**
* Conferma `io_events.xml` include `entity_id` nell&#39;elenco campi
* Nell&#39;azione, registra temporaneamente `JSON.stringify(params)` per visualizzare la forma payload completo
* Verificare che la funzione `extractValue()` stia trovando il livello di nidificazione corretto

### Gli ordini non vengono visualizzati nel dashboard demo

**Causa:** i criteri di ricerca REST `orders` di Commerce non restituiscono ordini oppure il campo `split_cash_status` non è incluso nella risposta REST.

**Correzione:**
* Conferma che `OrderRepositoryPlugin` stia caricando correttamente gli attributi dell&#39;estensione
* Test diretto: `GET /rest/V1/orders?searchCriteria[pageSize]=5` e verifica se `extension_attributes.split_cash_status` compare nella risposta
* Verificare che `extension_attributes.xml` stia dichiarando correttamente l&#39;attributo `split_cash_status` in `OrderInterface`


## Elenco di controllo per la verifica

* [ ] `split_*` colonne visibili nella tabella `sales_order`
* [ ] endpoint REST restituiscono 401 (non 404) quando vengono chiamati senza autenticazione
* [ ] viene eseguito il rendering dell&#39;interfaccia utente di split payment all&#39;estrazione quando è selezionato Cash
* [ ] I messaggi di convalida funzionano (pagamento eccessivo, credito insufficiente)
* [ ] La soglia blocca gli ordini > $100
* [ ] L&#39;ordine inoltrato ha lo stato `pending_payment` e commenti App Builder
* [ ] `simulate-split-payment.mjs list` mostra l&#39;ordine di test con importi suddivisi
* [ ] `simulate-split-payment.mjs accept <id>` sposta l&#39;ordine in `processing` con fattura e spedizione
* [ ] `simulate-split-payment.mjs decline <id>` annulla l&#39;ordine
* [ ] Il dashboard demo elenca gli ordini in sospeso e accetta/rifiuta il lavoro dall&#39;interfaccia utente


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
