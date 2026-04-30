---
title: Guida all’implementazione passo per passo del POC di pagamento frazionato
description: Scopri come implementare il POC di pagamento frazionato in ordine dopo il completamento della configurazione, coprendo modulo, ambienti, orchestratore, verifica e interfaccia utente opzionale.
feature: App Builder, Eventing, Extensibility
topic: Commerce, Architecture, App Builder, Development
role: Developer
level: Intermediate
doc-type: Tutorial
duration: 480
jira: KT-20902
last-substantial-update: 2026-04-30T00:00:00Z
source-git-commit: 27811c05f066c95a60ac309472d6488295fb2c96
workflow-type: tm+mt
source-wordcount: '1150'
ht-degree: 0%

---

# Guida all’implementazione passo per passo del POC di pagamento frazionato

Questa pagina è la tua lista di controllo per l’implementazione. Si presuppone che tu abbia già guardato la [panoramica](./overview.md) e la [demo completa](./full-demo.md) e che il tuo sito Commerce e il progetto App Builder siano entrambi pronti. Le singole pagine dei tutorial contengono informazioni dettagliate su ogni argomento di seguito. Utilizzare questa pagina per sapere cosa fare e in quale ordine.

## Prima di iniziare

Prima di eseguire un prompt o un comando, verificare quanto segue. La pagina [prerequisiti e configurazione](./prerequisites-and-setup.md) descrive ogni elemento in dettaglio.

**Lato Commerce**

* [ ] Adobe Commerce 2.4.5 o versione successiva
* [ ] evento di I/O abilitato e distribuito (solo Commerce Cloud — richiede `ENABLE_EVENTING: true` in `.magento.env.yaml`)
* [ ] **Consegna in contanti** abilitata in Amministratore con titolo impostato esattamente su `Cash`
* [ Credito dell&#39;archivio ] abilitato in Amministratore
* [ Il cliente di prova ] ha un credito di almeno $50
* [ Integrazione di ] Commerce creata, attivata e tutti e quattro i valori OAuth salvati in modo sicuro

**Lato App Builder**

* [ Il progetto App Builder ] esiste in Adobe Developer Console
* [ Servizio Adobe I/O Events ] aggiunto all&#39;area di lavoro
* [ ] Commerce connesso come provider di eventi
* [ ] `aio login` completato; area di lavoro corretta selezionata con `aio app use`
* [ ] Node.js 18 o versione successiva installato; `aio` CLI installato (`npm install -g @adobe/aio-cli`)

Se manca qualcosa, fermati qui e completalo prima.

## Fase 1 — Creazione del modulo Commerce

**Funzionamento:** genera il modulo PHP `Client_SplitPayment` che gestisce l&#39;interfaccia utente di estrazione, l&#39;applicazione di credito dell&#39;archivio, gli endpoint REST e la sottoscrizione dell&#39;evento di I/O. Questa è la scheda di rete thin in-Commerce: tutto il flusso di lavoro dell&#39;operatore rimane in App Builder.

### Passaggio 1.1 — Personalizzare il prompt per il progetto

Apri la pagina [Prompt di IA del modulo Commerce](./commerce-module-prompt.md) e prendi nota di quanto segue prima di copiare il prompt:

* Sostituisci `Client` con il tuo vero nome fornitore nel prompt
* Sostituisci `SplitPayment` se vuoi un nome modulo diverso
* Se il tema non è Luma, potrebbe essere necessario regolare il percorso di iniezione `LayoutProcessorPlugin` e le mappature RequireJS
* Se il metodo Cash on Delivery utilizza un codice diverso da `cashondelivery`, aggiornare `payment-method-helper.js`

### Passaggio 1.2 — Eseguire il prompt

Copiare il prompt completo da **PROMPT START** a **End of prompt** e incollarlo nel cursore (con Claude) o direttamente in Claude. Eseguirlo dalla directory principale del progetto Commerce in modo che l&#39;IA possa creare file in `app/code/`.

### Passaggio 1.3 — Abilitare e installare il modulo

Esegui questi comandi dalla directory principale del progetto Commerce:

```bash
bin/magento module:enable Client_SplitPayment
bin/magento setup:upgrade
bin/magento setup:di:compile
bin/magento setup:static-content:deploy -f
bin/magento cache:flush
```

### Passaggio 1.4 — Verificare che il modulo sia installato correttamente

```bash
# Confirm the module is active
bin/magento module:status Client_SplitPayment

# Confirm the split_ columns were added to sales_order
mysql -u <user> -p <dbname> -e "DESCRIBE sales_order;" | grep split
```

Dovrebbero essere visualizzate cinque colonne: `split_store_credit_amount`, `split_cash_amount`, `split_cash_status`, `split_sc_invoice_id` e `split_cash_invoice_id`.

Conferma in un browser che i campi di pagamento frazionato vengono visualizzati al momento dell&#39;estrazione quando un cliente connesso con credito del negozio seleziona **Contanti** come metodo di pagamento.

## Fase 2: configurare le variabili di ambiente

**Funzionamento:** Prepara i file delle credenziali utilizzati dall&#39;orchestratore App Builder, lo script di simulazione e (facoltativamente) l&#39;estensione dell&#39;interfaccia utente di Experience Cloud. Gli stessi quattro valori OAuth dell&#39;integrazione Commerce vengono utilizzati in ogni file `.env`.

Per l&#39;elenco completo delle variabili per componente, fare riferimento a [riferimento alle variabili di ambiente](./env-reference.md).

### Passaggio 2.1 — Creare l&#39;orchestratore `.env`

Nella directory `split-payment-orchestrator/`:

```bash
cp .env.example .env
```

Inserisci ogni valore:

| Variabile | Dove prenderlo |
|---|---|
| `COMMERCE_BASE_URL` | L&#39;URL dello store non ha una barra finale |
| `COMMERCE_CONSUMER_KEY` | Amministratore Commerce > Sistema > Integrazioni |
| `COMMERCE_CONSUMER_SECRET` | Uguale — visualizzato solo al momento dell&#39;attivazione |
| `COMMERCE_ACCESS_TOKEN` | Uguale |
| `COMMERCE_ACCESS_TOKEN_SECRET` | Uguale |
| `PAYMENT_THRESHOLD` | Deve corrispondere a `split_payment/general/threshold` in Commerce (impostazione predefinita: `100`) |
| `DEMO_UI_SECRET` | Facoltativo — se impostato, obbligatorio come `?secret=` nell&#39;URL del dashboard |

### Passaggio 2.2 — Creare lo script di simulazione `.env`

```bash
cp commerce-backend-ui-1/.env.simulation.example commerce-backend-ui-1/.env.simulation
```

Inserisci `COMMERCE_BASE_URL` e i quattro valori OAuth (stesse credenziali di cui sopra).

## Fase 3 — Creazione dell&#39;orchestratore App Builder

**Funzionamento:** genera il progetto App Builder `split-payment-orchestrator`: il consumer dell&#39;evento di I/O `payment-orchestrator`, le azioni Web `payment-accept` e `payment-decline` e l&#39;interfaccia utente dell&#39;operatore `demo-dashboard`.

### Passaggio 3.1 — Eseguire il prompt di Orchestrator

Apri la pagina del [prompt di App Builder Orchestrator AI](./orchestrator-prompt.md). Copiare il prompt completo ed eseguirlo dalla directory `split-payment-orchestrator/`.

### Passaggio 3.2 — Installare le dipendenze e implementare

```bash
cd split-payment-orchestrator
npm install
# Confirm .env is filled in (from Phase 2, Step 2.1)
aio app deploy
```

Dopo la distribuzione, `aio app deploy` stampa gli URL dell&#39;azione. Prendere nota dell&#39;URL `demo-dashboard`, necessario nella fase 4.

### Passaggio 3.3 — Confermare la registrazione dell&#39;evento

In Adobe Developer Console, apri l’area di lavoro del progetto App Builder. In **Eventi**, verificare che la registrazione `Split payment — sales order place before` esista ed è associata all&#39;azione `payment-orchestrator`.

## Fase 4 — Prova del flusso completo

Elabora i passaggi di verifica in ordine. La [guida ai test e alla verifica](./testing-and-verification.md) contiene i comandi curl completi, l&#39;utilizzo degli script di simulazione e i riferimenti per la risoluzione dei problemi per ogni passaggio seguente.

### Passaggio 4.1 — Verificare che gli endpoint REST siano instradabili

```bash
# Expect 401, not 404 — the endpoint exists but requires auth
curl -X POST https://your-store.example.com/rest/V1/split-payment/orders/1/cash-received
```

### Passaggio 4.2 — Verificare l&#39;interfaccia utente di estrazione

1. Accedi allo store come cliente di prova (quello con il credito del negozio)
2. Aggiungi un prodotto al carrello per un totale inferiore a 100 dollari al netto di spese di spedizione e imposte
3. Al passaggio di pagamento, seleziona **Contanti**
4. Conferma la visualizzazione dei campi di pagamento frazionato: saldo visualizzato, contante precompilato, credito del negozio a $0
5. Riduci l&#39;importo in contanti e conferma che la parte di credito del negozio venga aggiornata correttamente

### Passaggio 4.3 — Verifica del dispositivo di protezione della soglia

Aggiungi prodotti per un totale di oltre $ 100, procedi al pagamento, seleziona Contanti e tenta di effettuare l&#39;ordine. L&#39;errore `"Payment could not be processed. Please try again or contact support."` deve essere visualizzato.

### Fase 4.4 — Trasferimento di un ordine di pagamento frazionato di prova

Crea un carrello inferiore a $100, inserisci una suddivisione del credito in contanti/negozio e fai l’ordine. In Commerce Admin, conferma:

* Lo stato dell&#39;ordine è `pending_payment`
* Nell’ordine sono visibili due commenti di App Builder:
   1. `"Cash payment of $X.XX pending. Awaiting admin confirmation."`
   2. `"Split payment orchestration completed. Order awaiting cash confirmation."`

Se non viene visualizzato alcun commento di App Builder, controlla i registri con `aio app logs` prima di continuare.

### Passaggio 4.5 — Test di accettazione tramite script di simulazione

```bash
# Find your order's entity_id
node commerce-backend-ui-1/scripts/simulate-split-payment.mjs list

# Accept the payment
node commerce-backend-ui-1/scripts/simulate-split-payment.mjs accept <entity_id>
```

In Commerce Admin, conferma:

* Lo stato dell&#39;ordine è cambiato in `processing`
* Commento cronologia: `"Cash payment of $X.XX received."`
* Fattura di cassa visibile nella scheda Fatture
* Spedizione visibile nella scheda Spedizioni (se applicabile)

### Passaggio 4.6 — Test del declino tramite script di simulazione

Esegui un altro ordine di test, quindi:

```bash
node commerce-backend-ui-1/scripts/simulate-split-payment.mjs decline <entity_id>
```

Confermare lo stato dell&#39;ordine: `canceled`; `split_cash_status`: `declined`.

### Passaggio 4.7 — Testare il dashboard demo

Apri l’URL del dashboard dal passaggio 3.2. Con un ordine in sospeso nel sistema:

1. Conferma la visualizzazione dell&#39;ordine nell&#39;elenco in sospeso
2. Fai clic su **Accetta** e conferma che l&#39;ordine verrà spostato in `processing` in Commerce
3. Ricolloca l&#39;ordine, torna alla dashboard e fai clic su **Rifiuta** — conferma annullamento

## Fase 5 (facoltativa): aggiunta dell’estensione dell’interfaccia utente di Experience Cloud

Questa fase incorpora il dashboard dell&#39;operatore nella shell di amministrazione di Commerce anziché utilizzare `demo-dashboard` standalone. Ignora questa fase se il dashboard autonomo soddisfa le tue esigenze.

### Passaggio 5.1 — Rivedere i prerequisiti

L’estensione dell’interfaccia utente di Experience Cloud richiede le credenziali IMS oltre ai valori di integrazione OAuth. Leggi la pagina del [prompt di IA per l&#39;estensione dell&#39;interfaccia utente di Experience Cloud](./experience-cloud-ui-prompt.md) prima di eseguire il prompt, prestando attenzione a `OAUTH_CLIENT_ID` e alle variabili IMS correlate.

### Passaggio 5.2 — Eseguire il prompt dell&#39;estensione dell&#39;interfaccia utente

Copiare il prompt completo da **PROMPT START** a **Fine del prompt** ed eseguirlo dalla directory `commerce-checkout-starter-kit/`.

### Passaggio 5.3 — Implementare

```bash
cd commerce-checkout-starter-kit
npm install
cp .env.dist .env
# Fill in IMS credentials and COMMERCE_INTEGRATION_* credentials
aio app deploy
```

## Guida rapida alla risoluzione dei problemi

| Sintomo | Prima cosa da verificare |
|---|---|
| I campi di pagamento frazionato non vengono visualizzati al momento del pagamento | Errori RequireJS nella console del browser; eseguire anche `bin/magento cache:flush` |
| `"Payment could not be processed"` al momento dell&#39;ordine | `PlaceOrderPlugin` o `BalanceManagementInterface` — aggiungere la registrazione di debug temporanea in `PlaceOrderPlugin` |
| Nessun commento di App Builder sull’ordine | Esegui `aio app logs` per verificare se l&#39;evento è stato generato e che cosa ha restituito l&#39;azione |
| `403` da Commerce REST in App Builder | Fastly IP inserita nell&#39;elenco Consentiti — verifica prima con lo script di simulazione; se funziona, il problema è il blocco dell’IP in uscita |
| `"The signature is invalid"` da Commerce | Conferma che `COMMERCE_BASE_URL` non ha una barra finale; conferma che l&#39;integrazione sia attivata |
| Gli ordini non vengono visualizzati nel dashboard demo | Verifica che `extension_attributes.split_cash_status` sia presente in una risposta diretta di `GET /rest/V1/orders` |

Per informazioni dettagliate sulla risoluzione dei problemi, vedere la [guida ai test e alla verifica](./testing-and-verification.md#common-issues-and-fixes).

{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
