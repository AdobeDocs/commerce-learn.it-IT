---
title: 'Dividi pagamento POC: App Builder Orchestrator AI prompt'
description: Scopri come utilizzare questo prompt per creare l’app split-payment-orchestrator. Eventi di I/O, programma di gestione dei pagamenti, azioni Web, dashboard dimostrativa e distribuzione di app aio.
feature: App Builder, Configuration, Eventing, Extensibility, Paas, REST
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 421
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 8dfbf2694378aae76c91afa11bfee7d93077d8ba
workflow-type: tm+mt
source-wordcount: '927'
ht-degree: 0%

---

# Dividi pagamento POC: App Builder Orchestrator AI prompt

Utilizzare questa pagina per copiare il prompt completo che genera il progetto **split-payment-orchestrator**: le azioni Web **payment-orchestrator** I/O Event consumer, **payment-accept** e **payment-rifiuta**, **demo-dashboard** e il client REST di Commerce.

## Come utilizzare questo prompt

Copiare tutto da **PROMPT START** a **End of prompt** in Cursor (con Claude) o direttamente in Claude. Eseguire il prompt dalla directory `split-payment-orchestrator/` (directory principale del progetto App Builder).

## Prima dell’esecuzione

* Termina [Dividi POC pagamento: prerequisiti e configurazione dell&#39;ambiente](./prerequisites-and-setup.md).
* Prepara il file [Dividi POC pagamento: variabili di ambiente di riferimento](./env-reference.md) e `.env` nel progetto.


## Il prompt

**INIZIO RICHIESTA**


Stai generando un&#39;applicazione Adobe App Builder completa per orchestrare i pagamenti divisi in Adobe Commerce. Questa applicazione riceve eventi di I/O da Commerce, elabora decisioni di pagamento frazionato e richiama in Commerce tramite REST.

**Progetto:** `split-payment-orchestrator`
**Runtime:** Node.js 18
**Dipendenze chiave:** `@adobe/aio-sdk ^6.0.0`, `got ^11.8.6`, `oauth-1.0a ^2.2.6`

Genera tutti i file elencati di seguito. L&#39;applicazione deve funzionare con Adobe I/O Runtime (`aio app deploy`).


### Struttura del file da generare

```
split-payment-orchestrator/
├── package.json
├── app.config.yaml
├── .env.example
└── actions/
    ├── payment-orchestrator/
    │   ├── index.js         ← I/O Event entry point
    │   ├── commerce-client.js
    │   ├── threshold.js
    │   ├── cash-payment.js
    │   ├── order-update.js
    │   └── store-credit.js  ← Deprecated stub; kept for reference
    ├── payment-accept/
    │   └── index.js
    ├── payment-decline/
    │   └── index.js
    └── demo-dashboard/
        └── index.js
```


### `package.json`

```json
{
  "name": "split-payment-orchestrator",
  "version": "1.0.0",
  "private": true,
  "description": "Adobe App Builder action — split payment PoC orchestrator",
  "engines": { "node": ">=18" },
  "scripts": {
    "deploy": "NODE_OPTIONS=--disable-warning=DEP0040 aio app deploy",
    "aio": "NODE_OPTIONS=--disable-warning=DEP0040 aio"
  },
  "dependencies": {
    "@adobe/aio-sdk": "^6.0.0",
    "@adobe/exc-app": "^1.5.9",
    "got": "^11.8.6",
    "oauth-1.0a": "^2.2.6"
  }
}
```


### `app.config.yaml`

Definire quattro azioni nel pacchetto `split_payment_orchestrator`:

**`payment-orchestrator`**
* `web: "no"` (solo trigger evento I/O, non chiamabile direttamente tramite HTTP)
* `runtime: nodejs:18`
* `require-adobe-auth: true`, `final: true`
* Input da env: `LOG_LEVEL`, `COMMERCE_BASE_URL`, `COMMERCE_CONSUMER_KEY`, `COMMERCE_CONSUMER_SECRET`, `COMMERCE_ACCESS_TOKEN`, `COMMERCE_ACCESS_TOKEN_SECRET`, `PAYMENT_THRESHOLD`

**`payment-accept`**
* `web: "yes"` (azione Web HTTP, richiamabile dal dashboard o ERP)
* `runtime: nodejs:18`
* `require-adobe-auth: true`, `final: true`
* Stessi input delle credenziali Commerce (nessun `PAYMENT_THRESHOLD`)

**`payment-decline`**
* `web: "yes"`
* `runtime: nodejs:18`
* `require-adobe-auth: true`, `final: true`
* Stessi input delle credenziali Commerce

**`demo-dashboard`**
* `web: "yes"`
* `runtime: nodejs:18`
* `require-adobe-auth: false` ← dashboard è accessibile al pubblico (protetto solo da `DEMO_UI_SECRET` se impostato)
* Input: tutte le credenziali Commerce + `DEMO_UI_SECRET`, `DEMO_UI_BASE_URL`

**Registrazione eventi** (in `events.registrations`):

```yaml
Split payment — sales order place before:
  description: Invokes payment-orchestrator when an order is about to be placed
  events_of_interest:
    - provider_metadata: dx_commerce_events
      event_codes:
        - com.adobe.commerce.observer.sales_order_place_before
  runtime_action: split_payment_orchestrator/payment-orchestrator
```


### `actions/payment-orchestrator/commerce-client.js`

Client REST OAuth 1.0a condiviso per Adobe Commerce. Implementa:

**`createCommerceClient(params, logger)`** — restituisce `{ request, baseUrl }`

* Legge `COMMERCE_BASE_URL`, `COMMERCE_CONSUMER_KEY`, `COMMERCE_CONSUMER_SECRET`, `COMMERCE_ACCESS_TOKEN`, `COMMERCE_ACCESS_TOKEN_SECRET` da `params`
* Genera se mancano le credenziali
* Usa `oauth-1.0a` con `HMAC-SHA256` (Node.js `crypto.createHmac`)
* Usa `got@11` (non `got@12+` — il progetto utilizza CJS) con `prefixUrl = ${baseUrl}/rest/V1/`
* Aggiunge l&#39;intestazione `Authorization` tramite hook `beforeRequest`
* `request(method, path, options)` — normalizza il percorso (strisce iniziali di `/`), restituisce `{ statusCode, body }`
* `throwHttpErrors: false` — non genera mai su 4xx/5xx; restituisce sempre il codice di stato


### `actions/payment-orchestrator/threshold.js`

**`evaluateThreshold({ orderTotal, storeCreditAmount, cashAmount, params, logger })`** — restituisce `{ pass: boolean, reason: string }`

Logica:
1. Lettura di `params.PAYMENT_THRESHOLD`; analisi come float; impostazione predefinita `100` se mancante, NaN o ≤ 0
2. Se `orderTotal > threshold`: restituisce `{ pass: false, reason: 'PAYMENT_THRESHOLD_EXCEEDED' }`
3. Se `Math.abs((storeCreditAmount + cashAmount) - orderTotal) > 0.02`: restituisce `{ pass: false, reason: 'SPLIT_AMOUNT_MISMATCH' }`
4. In caso contrario: restituisce `{ pass: true, reason: '' }`


### `actions/payment-orchestrator/cash-payment.js`

**`recordCashPending({ commerce, orderId, cashAmount, logger })`** — restituisce `{ ok: boolean, error? }`

* Chiama `POST orders/${orderId}/comments` con:

  ```json
  {
    "statusHistory": {
      "comment": "Cash payment of $X.XX pending. Awaiting admin confirmation.",
      "entity_name": "order",
      "parent_id": "<orderId>",
      "is_visible_on_front": true,
      "is_customer_notified": false,
      "status": "pending_payment"
    }
  }
  ```

* Restituisce `{ ok: true }` su 2xx; restituisce `{ ok: false, error: { code, message } }` in caso contrario
* Esegue il wrapping in try/catch; restituisce un oggetto di errore, non genera mai


### `actions/payment-orchestrator/order-update.js`

**`updateOrderAfterOrchestration({ commerce, orderId, success, detail, logger })`** — restituisce `{ ok: boolean, error? }`

* Se `success`: pubblica un commento di cronologia `"Split payment orchestration completed. Order awaiting cash confirmation."`
* Se `!success`: pubblica `"Payment could not be processed. Please try again or contact support."` — non includere mai `detail` nel commento visibile al cliente; registra `detail` solo internamente
* Restituisce `{ ok: boolean, error? }` — non genera mai


### `actions/payment-orchestrator/store-credit.js`

**`applyStoreCredit({ commerce, cartId, amount, logger })`**: implementazione no-op obsoleta

Includi questo file con un avviso JSDoc `@deprecated` che spiega:
> L&#39;orchestratore non applica più il credito dello store tramite REST. Il credito dell&#39;archivio viene applicato al momento dell&#39;estrazione nel modulo PHP di Commerce (`PlaceOrderPlugin`) utilizzando `BalanceManagementInterface::apply()`, che richiede un carrello attivo. Quando App Builder riceve l’evento I/O, il carrello è inattivo. Questo file viene conservato come riferimento o per i flussi personalizzati in cui viene applicato il credito dell’archivio dopo l’ordine.

Mantenere un&#39;implementazione funzionante (stessa forma di altri moduli) in modo che gli sviluppatori possano studiare il pattern, ma contrassegnarlo chiaramente come non utilizzato nel flusso corrente.


### `actions/payment-orchestrator/index.js`

Punto di ingresso evento I/O. Implementa `async function main(params)`.

**Estrazione payload:**

Gli eventi di I/O di Adobe Commerce possono distribuire il payload in diverse forme. Estrai l’oggetto valore ordine controllando questi percorsi nell’ordine:
1. `params.__ow_body` (analizzare la stringa JSON se necessario) → `.event.data.value`
2. `params.data.value`
3. `params.value`
4. `params.body.event.data.value`
5. Torna a `params` stesso

**Estrazione campo dal valore:**
* `orderId = value.entity_id || value.id`
* `orderTotal = parseFloat(value.grand_total ?? value.base_grand_total ?? value.subtotal ?? '0')`
* Dividi gli importi da `value.extension_attributes` (controlla sia `extension_attributes` che `extensionAttributes`):
   * `storeCredit = parseFloat(ext.split_store_credit_amount ?? value.split_store_credit_amount ?? '0')`
   * `cash = parseFloat(ext.split_cash_amount ?? value.split_cash_amount ?? '0')`

**Flusso:**
1. Estrai valore; se `orderId` manca, registra l&#39;errore e restituisce `{ statusCode: 200, body: { ok: false, message: PUBLIC_ERROR } }`
2. Chiama `evaluateThreshold(...)` — in caso di errore, registra e restituisce la stessa risposta 200
3. Chiama `createCommerceClient(params, logger)` — se non riesce, restituisce l&#39;errore 200
4. Se `storeCredit > 0`, registra il credito dell&#39;archivio applicato al momento dell&#39;estrazione (nessuna chiamata REST necessaria)
5. Chiama `recordCashPending(...)` — se non riesce, chiama `updateOrderAfterOrchestration(..., success: false)` e restituisce l&#39;errore 200
6. Chiama `updateOrderAfterOrchestration(..., success: true)`
7. Restituisce `{ statusCode: 200, body: { ok: true, message: 'processed' } }`

**Importante:** restituire sempre `statusCode: 200`. I/O Runtime tenterà di rispondere senza 200, causando l&#39;elaborazione di ordini duplicati. Gli errori vengono segnalati nel corpo.

Costante **`PUBLIC_ERROR`:** `"Payment could not be processed. Please try again or contact support."` — utilizzata per tutti i messaggi di errore rivolti all&#39;esterno.


### `actions/payment-accept/index.js`

Azione web HTTP. Chiama `POST /V1/split-payment/orders/:orderId/cash-received`.

**Risoluzione ID ordine:** Controllare `params.orderId`, quindi `params.payload.orderId`, quindi `params.__ow_body` (JSON analizzato). Restituisci 400 se mancante.

**Flusso:**
1. Risolvi `orderId`; restituisce 400 se manca
2. Init Commerce client; restituisce 500 in caso di errore
3. Chiama `POST split-payment/orders/${orderId}/cash-received` con corpo JSON vuoto
4. Se 2xx: restituisce `{ statusCode: 200, body: { ok: true, orderId, message: 'accepted' } }`
5. In caso di errore: registrare e restituire `{ statusCode: 200, body: { ok: false, message: PUBLIC_ERROR } }`


### `actions/payment-decline/index.js`

Azione web HTTP. Stesso pattern di `payment-accept` ma chiama `POST /V1/split-payment/orders/:orderId/cash-decline`.

Restituisci `{ ok: true, orderId, message: 'declined' }` al completamento.


### `actions/demo-dashboard/index.js`

Dashboard dell’operatore demo autonomo. Fornisce un dashboard di HTML per elencare gli ordini di cassa in sospeso e attivare le azioni di accettazione/rifiuto. Si tratta di una singola azione web che serve sia l’interfaccia utente di HTML che un’API JSON.

**Sicurezza:**
* Controllo `DEMO_UI_SECRET` facoltativo: se impostato, richiede il parametro di query `?secret=<value>` o l&#39;intestazione `x-demo-secret` in tutte le richieste. Restituisce 401 se mancante/errato.
* Registra un avviso se `DEMO_UI_SECRET` non è impostato (dashboard non protetto)

**Indirizzamento (basato su metodo HTTP + percorso/corpo):**

L’azione riceve tutte le richieste. Determina intento da:
* `params.__ow_method` (GET/POST) e `params.__ow_path` o parametri azione
* `GET` senza alcuna azione → distribuire il dashboard di HTML
* `GET` con `action=list` parametro → restituire l&#39;elenco JSON degli ordini in sospeso
* Logica `POST` con `action=accept` e `orderId` → chiamata `payment-accept` (o chiamata REST Commerce in linea)
* `POST` con logica `action=decline` e `orderId` → chiamata `payment-decline`

**Recupero di ordini in sospeso:**
* Chiama `GET orders?searchCriteria[pageSize]=50` da Commerce REST
* Filtra gli ordini in cui `extension_attributes.split_cash_status === 'pending'` AND non si trova in uno stato terminale (`complete`, `closed`, `canceled`, `cancelled`)
* Ordina il primo più recente in memoria
* Restituisce fino a 20 (configurabile tramite il parametro `limit`)

**Dashboard HTML:**
Il dashboard è una stringa HTML restituita con `Content-Type: text/html`. Dovrebbe:
* Elencare gli ordini di cassa in sospeso in una tabella: n. ordine (increment_id), id_entità, nome cliente, importo contanti, importo del credito di magazzino, data
* Fornisci i pulsanti **Accetta** e **Rifiuta** per ogni ordine che chiama il proprio endpoint API
* Mostra risposte di successo/errore in linea
* Includi un pulsante di aggiornamento
* Essere formattato a sufficienza per essere utilizzabile (CSS minimo va bene; nessuna dipendenza CDN esterna per evitare problemi CORS in fase di esecuzione)
* Mostra un banner di avviso se `DEMO_UI_SECRET` non è impostato

**Errore nell&#39;interfaccia utente:**
Quando una chiamata REST di Commerce non riesce, includi lo stato HTTP e una breve descrizione del corpo dell’errore Commerce (bonifica, elimina i tag HTML, tronca a 500 caratteri). Non esporre le credenziali.

**Helper risposte:**

```javascript
function jsonResponse(statusCode, obj, extraHeaders = {}) { ... }
function htmlResponse(html) { ... }
```


### `.env.example`

```dotenv
# Commerce REST base URL — no trailing slash
COMMERCE_BASE_URL=

# OAuth 1.0a integration credentials (Admin → System → Integrations)
COMMERCE_CONSUMER_KEY=
COMMERCE_CONSUMER_SECRET=
COMMERCE_ACCESS_TOKEN=
COMMERCE_ACCESS_TOKEN_SECRET=

# PoC threshold — must match split_payment/general/threshold in Commerce (default: 100)
PAYMENT_THRESHOLD=100

LOG_LEVEL=info

# Demo dashboard optional shared secret
DEMO_UI_SECRET=
DEMO_UI_BASE_URL=
```


### Distribuisci, comando

Dopo aver generato tutti i file, dalla directory `split-payment-orchestrator/`:

```bash
npm install
cp .env.example .env
# Edit .env with your credentials
aio app deploy
```

Dopo la distribuzione, prendere nota degli URL di azione stampati da `aio app deploy`. L&#39;URL `demo-dashboard` è il punto in cui si accede al dashboard dell&#39;operatore.


### Fine del prompt


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
