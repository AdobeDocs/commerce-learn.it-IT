---
title: 'Dividi pagamento POC: App Builder Orchestrator AI prompt'
description: 'Scopri come utilizzare questo prompt per creare l’app per l’orchestrazione dei pagamenti divisi: eventi di I/O, orchestrazione dei pagamenti, azioni web, dashboard dimostrativa e distribuzione di app aio.'
feature: App Builder, Configuration, Eventing, Extensibility, Paas, REST
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 421
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: beb22335cec97141b46ddbbca97d21b216c55a80
workflow-type: tm+mt
source-wordcount: '927'
ht-degree: 0%

---

# Dividi pagamento POC: App Builder Orchestrator AI prompt

Utilizzare questa pagina per copiare il prompt completo che genera il progetto **split-payment-orchestrator**: le azioni Web **payment-orchestrator** I/O Event consumer, **payment-accept** e **payment-rifiuta**, **demo-dashboard** e il client REST di Commerce.

## Come utilizzare questo prompt

Copiare tutto da **PROMPT START** a **End of prompt** in Cursor (con Claude) o direttamente in Claude. Eseguire il prompt dalla directory `split-payment-orchestrator/` (directory principale del progetto App Builder).

## Prima dell’esecuzione

* Termina [Dividi POC pagamento: prerequisiti e configurazione dell&#39;ambiente](split-payment-poc-prerequisites-and-setup.md).
* Prepara il file [Dividi POC pagamento: variabili di ambiente di riferimento](split-payment-poc-env-reference.md) e `.env` nel progetto.


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
* Same Commerce credential inputs

**`demo-dashboard`**
* `web: "yes"`
* `runtime: nodejs:18`
* `require-adobe-auth: false` ← Dashboard is publicly accessible (protected only by `DEMO_UI_SECRET` if set)
* Inputs: all Commerce credentials + `DEMO_UI_SECRET`, `DEMO_UI_BASE_URL`

**Events registration** (under `events.registrations`):

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

Shared OAuth 1.0a REST client for Adobe Commerce. Implements:

**`createCommerceClient(params, logger)`** — returns `{ request, baseUrl }`

* Reads `COMMERCE_BASE_URL`, `COMMERCE_CONSUMER_KEY`, `COMMERCE_CONSUMER_SECRET`, `COMMERCE_ACCESS_TOKEN`, `COMMERCE_ACCESS_TOKEN_SECRET` from `params`
* Throws if any credential is missing
* Uses `oauth-1.0a` with `HMAC-SHA256` (Node.js `crypto.createHmac`)
* Uses `got@11` (not `got@12+` — the project uses CJS) with `prefixUrl = ${baseUrl}/rest/V1/`
* Adds `Authorization` header via `beforeRequest` hook
* `request(method, path, options)` — normalizes the path (strips leading `/`), returns `{ statusCode, body }`
* `throwHttpErrors: false` — never throws on 4xx/5xx; always returns the status code


### `actions/payment-orchestrator/threshold.js`

**`evaluateThreshold({ orderTotal, storeCreditAmount, cashAmount, params, logger })`** — returns `{ pass: boolean, reason: string }`

Logic:
1. Read `params.PAYMENT_THRESHOLD`; parse as float; default to `100` if missing, NaN, or ≤ 0
2. If `orderTotal > threshold`: return `{ pass: false, reason: 'PAYMENT_THRESHOLD_EXCEEDED' }`
3. If `Math.abs((storeCreditAmount + cashAmount) - orderTotal) > 0.02`: return `{ pass: false, reason: 'SPLIT_AMOUNT_MISMATCH' }`
4. Otherwise: return `{ pass: true, reason: '' }`


### `actions/payment-orchestrator/cash-payment.js`

**`recordCashPending({ commerce, orderId, cashAmount, logger })`** — returns `{ ok: boolean, error? }`

* Calls `POST orders/${orderId}/comments` with:

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

* Returns `{ ok: true }` on 2xx; returns `{ ok: false, error: { code, message } }` otherwise
* Wraps in try/catch; returns error object, never throws


### `actions/payment-orchestrator/order-update.js`

**`updateOrderAfterOrchestration({ commerce, orderId, success, detail, logger })`** — returns `{ ok: boolean, error? }`

* If `success`: posts a history comment `"Split payment orchestration completed. Order awaiting cash confirmation."`
* If `!success`: posts `"Payment could not be processed. Please try again or contact support."` — never include `detail` in the customer-visible comment; log `detail` internally only
* Returns `{ ok: boolean, error? }` — never throws


### `actions/payment-orchestrator/store-credit.js`

**`applyStoreCredit({ commerce, cartId, amount, logger })`** — deprecated no-op implementation

Include this file with a JSDoc `@deprecated` notice explaining:
> The orchestrator no longer applies store credit via REST. Store credit is applied at checkout in the Commerce PHP module (`PlaceOrderPlugin`) using `BalanceManagementInterface::apply()`, which requires an active cart. By the time App Builder receives the I/O Event, the cart is inactive. This file is kept for reference or for custom flows where store credit is applied post-order.

Keep a working implementation (same shape as other modules) so developers can study the pattern, but mark it clearly as not used in the current flow.


### `actions/payment-orchestrator/index.js`

I/O Event entry point. Implements `async function main(params)`.

**Payload extraction:**

Adobe Commerce I/O Events may deliver the payload in several shapes. Extract the order value object by checking these paths in order:
1. `params.__ow_body` (parse JSON string if needed) → `.event.data.value`
2. `params.data.value`
3. `params.value`
4. `params.body.event.data.value`
5. Fall back to `params` itself

**Field extraction from value:**
* `orderId = value.entity_id || value.id`
* `orderTotal = parseFloat(value.grand_total ?? value.base_grand_total ?? value.subtotal ?? '0')`
* Split amounts from `value.extension_attributes` (check both `extension_attributes` and `extensionAttributes`):
   * `storeCredit = parseFloat(ext.split_store_credit_amount ?? value.split_store_credit_amount ?? '0')`
   * `cash = parseFloat(ext.split_cash_amount ?? value.split_cash_amount ?? '0')`

**Flow:**
1. Extract value; if `orderId` is missing, log error and return `{ statusCode: 200, body: { ok: false, message: PUBLIC_ERROR } }`
2. Call `evaluateThreshold(...)` — if fails, log and return same 200 response
3. Call `createCommerceClient(params, logger)` — if fails, return 200 error
4. If `storeCredit > 0`, log that store credit was applied at checkout (no REST call needed)
5. Call `recordCashPending(...)` — if fails, call `updateOrderAfterOrchestration(..., success: false)` and return 200 error
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
