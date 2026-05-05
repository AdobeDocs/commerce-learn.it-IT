---
title: 'Dividi pagamento POC: richiesta di IA per l’estensione dell’interfaccia utente di Experience Cloud'
description: 'Scopri come utilizzare questo prompt facoltativo per incorporare lo split payment in Commerce Admin: interfaccia di amministrazione di SDK, IMS, OAuth, accept and declinare e lo script di simulazione.'
feature: App Builder, Admin Workspace, Extensibility, Paas, REST
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 192
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 629bbb6fe26f128e346d85c857111c2f8dbb6d76
workflow-type: tm+mt
source-wordcount: '496'
ht-degree: 0%

---

# Dividi pagamento POC: richiesta di IA per l’estensione dell’interfaccia utente di Experience Cloud

Questo è il passaggio facoltativo che incorpora un pannello di ordini di pagamento frazionati nell&#39;Admin Shell (Experience Cloud) di **[!UICONTROL Adobe Commerce]** utilizzando il pattern `commerce-checkout-starter-kit` e `commerce-backend-ui-1`. Il dashboard [demo](./orchestrator-prompt.md) autonomo di App Builder Orchestrator copre lo stesso flusso di accettazione e rifiuto senza l&#39;integrazione di Admin Shell.

## Come utilizzare questo prompt

Copia tutto da **PROMPT START** a **End of prompt** in Cursor o Claude. Eseguire il prompt dalla directory `commerce-checkout-starter-kit/`.

## Prima dell’esecuzione

* Questo percorso necessita di **credenziali IMS** oltre ai valori OAuth (vedi [Dividi POC pagamento: riferimento alle variabili di ambiente](./env-reference.md) per le variabili `commerce-checkout-starter-kit`).
* Completa [Dividi POC pagamento: richiesta di IA di App Builder Orchestrator](./orchestrator-prompt.md) prima se vuoi confrontare lo stesso comportamento `payment-accept` e `payment-decline`; l&#39;estensione dell&#39;interfaccia utente riutilizza tale logica con i nomi env `COMMERCE_INTEGRATION_*`.


## Il prompt

**INIZIO RICHIESTA**


Stai generando l&#39;estensione SDK dell&#39;interfaccia utente di amministrazione `commerce-backend-ui-1` per il PoC per il pagamento frazionato. Questa estensione incorpora un dashboard degli ordini di pagamento frazionati in Adobe Commerce Admin Shell utilizzando i modelli `@adobe/aio-app-dev-toolkit` e `@adobe/commerce-backend-ui-1` di Commerce Checkout Starter Kit.

**Progetto di base:** `commerce-checkout-starter-kit/`
**Directory di estensione:** `commerce-checkout-starter-kit/commerce-backend-ui-1/`


### Cosa Fornisce Questa Estensione

1. **Pannello griglia ordini amministratore**: voce di menu personalizzata in Commerce Admin Shell che elenca gli ordini di pagamento frazionati con `split_cash_status = 'pending'`
2. **Visualizzazione dettagli ordine** — mostra il raggruppamento del pagamento frazionato (importo contanti, importo credito del negozio, stato) insieme all&#39;ordine
3. **Accetta/Rifiuta azioni** — pulsanti che chiamano le azioni App Builder `payment-accept` e `payment-decline` tramite OAuth 1.0a


### Struttura del file da generare

```
commerce-checkout-starter-kit/commerce-backend-ui-1/
├── ext.config.yaml
├── README.md
├── .env.simulation.example
├── actions/
│   ├── utils.js
│   ├── commerce/
│   │   └── index.js           ← IMS-based Commerce REST (order listing)
│   ├── payment-accept/
│   │   ├── commerce-client.js ← OAuth 1.0a (accept/decline)
│   │   └── index.js
│   ├── payment-decline/
│   │   └── index.js
│   └── registration/
│       └── index.js
├── scripts/
│   ├── README.md
│   └── simulate-split-payment.mjs
└── web-src/
    ├── index.html
    ├── package.json
    ├── .parcelrc
    └── src/
        ├── index.jsx
        ├── index.css
        ├── utils.js
        ├── exc-runtime.js
        ├── config.json
        ├── constants/
        │   └── extension.js
        ├── components/
        │   ├── App.jsx
        │   ├── ExtensionRegistration.jsx
        │   ├── MainPage.jsx
        │   ├── SplitPaymentDashboard.jsx
        │   └── SplitPaymentOrderDetail.jsx
        └── hooks/
            └── useSplitPaymentOrders.js
```


### Azioni back-end

**`actions/commerce/index.js`** - REST Commerce autenticato da IMS
* Utilizza il token IMS fornito dal contesto SDK dell’interfaccia utente amministratore per chiamare Commerce REST
* Recupera l&#39;elenco degli ordini con il filtro `split_cash_status`
* Restituisce l’elenco degli ordini come JSON

**`actions/payment-accept/commerce-client.js`** - Client OAuth 1.0a
* Stessa implementazione di `split-payment-orchestrator/actions/payment-orchestrator/commerce-client.js`
* Utilizza `COMMERCE_INTEGRATION_*` variabili env con prefisso (per distinguere dalle credenziali IMS)

**`actions/payment-accept/index.js`** — Azione di accettazione
* Stessa logica di `split-payment-orchestrator/actions/payment-accept/index.js`
* Chiamate `POST /V1/split-payment/orders/:orderId/cash-received` tramite OAuth 1.0a

**`actions/payment-decline/index.js`** — Rifiuta azione
* Chiamate `POST /V1/split-payment/orders/:orderId/cash-decline`

**`actions/registration/index.js`** — Registrazione SDK interfaccia utente amministratore
* Registra l&#39;estensione con Commerce Admin Shell
* Aggiunge una voce di menu in Ordini per il dashboard del pagamento frazionato


### React - Componenti front-end

**`SplitPaymentDashboard.jsx`**
* Elenca gli ordini di pagamento frazionati in sospeso in una tabella in stile Spectrum
* Colonne: N. ordine (increment_id), Data, Cliente, Scadenza contanti, Credito negozio, Stato
* Pulsanti Accetta e Rifiuta per riga
* Chiama le azioni di backend tramite `web-src/src/utils.js` helper di recupero
* Mostra gli stati di caricamento/errore; si aggiorna al completamento dell&#39;azione

**`SplitPaymentOrderDetail.jsx`**
* Mostra dettagli pagamento frazionato per un singolo ordine
* Visualizzazioni: importo contanti, importo credito negozio, split_cash_status corrente

**`useSplitPaymentOrders.js`** — React hook
* Recupera gli ordini di pagamento frazionati da `actions/commerce/index.js`
* Restituisce `{ orders, loading, error, refresh }`


### Script di simulazione

**`scripts/simulate-split-payment.mjs`**

Uno script ESM Node.js per testare direttamente le chiamate REST di Commerce (senza passare attraverso App Builder). Utilizza la stessa firma OAuth 1.0a delle azioni di App Builder.

Comandi:
* `node simulate-split-payment.mjs help` — mostra utilizzo
* `node simulate-split-payment.mjs list` — elenca gli ordini recenti con dati di pagamento frazionati
* `node simulate-split-payment.mjs show <orderId>` — mostra i campi di pagamento frazionato per un ordine specifico (entity_id)
* `node simulate-split-payment.mjs accept <orderId>` — chiama endpoint `cash-received`
* `node simulate-split-payment.mjs decline <orderId>` — chiama endpoint `cash-decline`

Legge le credenziali da `commerce-backend-ui-1/.env.simulation` (copia da `.env.simulation.example`).

**`.env.simulation.example`:**

```dotenv
COMMERCE_BASE_URL=https://your-store.example.com
COMMERCE_CONSUMER_KEY=
COMMERCE_CONSUMER_SECRET=
COMMERCE_ACCESS_TOKEN=
COMMERCE_ACCESS_TOKEN_SECRET=
```


### `ext.config.yaml`

Configura l&#39;estensione con:
* Tipo di estensione `commerce-backend-ui-1`
* Le quattro azioni di backend (`commerce`, `payment-accept`, `payment-decline`, `registration`)
* `require-adobe-auth: true` per tutte le azioni eccetto `registration`
* Assegnazioni di input dall&#39;ambiente per le credenziali `COMMERCE_INTEGRATION_*`


### Dopo la generazione dei file

```bash
cd commerce-checkout-starter-kit
npm install
cp .env.dist .env
# Fill in IMS credentials and COMMERCE_INTEGRATION_* credentials
aio app deploy
```

### Fine del prompt


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
