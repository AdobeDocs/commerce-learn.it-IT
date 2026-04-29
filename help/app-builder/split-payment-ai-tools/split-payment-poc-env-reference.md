---
title: 'Dividi pagamento POC: riferimento variabili ambiente'
description: Scopri come mappare Commerce OAuth, l’URL di base, la soglia di pagamento e le impostazioni demo facoltative ai file dell’orchestratore, dell’estensione dell’interfaccia utente e dell’ambiente di simulazione.
feature: App Builder, Configuration, Extensibility, Paas, REST, Security
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 115
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 1e2c7e0e6d0f2d174b88406ce3fb7c787676ecee
workflow-type: tm+mt
source-wordcount: '243'
ht-degree: 0%

---

# Dividi pagamento POC: riferimento variabili ambiente

Le stesse quattro credenziali OAuth di Commerce vengono utilizzate in ogni componente. In **[!UICONTROL Commerce Admin]**, creare un **[!UICONTROL Integration]**, quindi riutilizzare i quattro valori in ogni file `.env` seguente. (Vedi [Dividi POC pagamento: prerequisiti e configurazione dell&#39;ambiente](split-payment-poc-prerequisites-and-setup.md) per i passaggi di attivazione.)

## Le quattro credenziali OAuth (utilizzate ovunque)

| Variabile | Dove prenderlo |
| --- | --- |
| `COMMERCE_CONSUMER_KEY` | **[!UICONTROL Commerce Admin]** > **[!UICONTROL System]** > **[!UICONTROL Integrations]** > *[integrazione]* |
| `COMMERCE_CONSUMER_SECRET` | Come sopra — i valori vengono visualizzati solo al momento dell&#39;attivazione |
| `COMMERCE_ACCESS_TOKEN` | Come sopra |
| `COMMERCE_ACCESS_TOKEN_SECRET` | Come sopra |


## orchestratore App Builder

### `split-payment-orchestrator/.env`

Copia da `.env.example` nella directory di orchestrator. Non eseguire il commit di questo file.

```dotenv
# Commerce REST base URL — no trailing slash
COMMERCE_BASE_URL=https://your-store.example.com

# OAuth 1.0a integration credentials
COMMERCE_CONSUMER_KEY=
COMMERCE_CONSUMER_SECRET=
COMMERCE_ACCESS_TOKEN=
COMMERCE_ACCESS_TOKEN_SECRET=

# Must match split_payment/general/threshold in Commerce config (default: 100)
# Both Commerce and App Builder fall back to 100 if this is missing, non-numeric, or ≤ 0
PAYMENT_THRESHOLD=100

LOG_LEVEL=info

# Demo dashboard: if set, requires ?secret=<value> in URL or x-demo-secret header
# Leave empty for private staging only (anyone with the URL can list/accept orders)
DEMO_UI_SECRET=

# Optional: override the base URL used in dashboard action links (useful behind proxies)
DEMO_UI_BASE_URL=
```


## Estensione dell’interfaccia utente di Experience Cloud (commerce-checkout-starter-kit)

### `commerce-checkout-starter-kit/.env`

Questo componente utilizza due set di credenziali: IMS per l’inserimento nell’elenco degli ordini con l’interfaccia utente di **[!UICONTROL Admin]** SDK e OAuth 1.0a per le azioni di accettazione e rifiuto.

```dotenv
# IMS — used by CustomMenu/commerce-rest-api to list orders
# The Admin UI SDK provides the IMS token context; these set the Commerce base URL
COMMERCE_BASE_URL=https://your-store.example.com
OAUTH_CLIENT_ID=
OAUTH_CLIENT_SECRETS=
OAUTH_TECHNICAL_ACCOUNT_ID=
OAUTH_TECHNICAL_ACCOUNT_EMAIL=
OAUTH_SCOPES=
OAUTH_IMS_ORG_ID=
AIO_CLI_ENV=stage

# OAuth 1.0a — same four credentials, COMMERCE_INTEGRATION_ prefix
COMMERCE_INTEGRATION_BASE_URL=https://your-store.example.com
COMMERCE_INTEGRATION_CONSUMER_KEY=
COMMERCE_INTEGRATION_CONSUMER_SECRET=
COMMERCE_INTEGRATION_ACCESS_TOKEN=
COMMERCE_INTEGRATION_ACCESS_TOKEN_SECRET=
```


## Script di simulazione

### `commerce-backend-ui-1/.env.simulation`

Copia da `.env.simulation.example` nella stessa directory.

```dotenv
COMMERCE_BASE_URL=https://your-store.example.com
COMMERCE_CONSUMER_KEY=
COMMERCE_CONSUMER_SECRET=
COMMERCE_ACCESS_TOKEN=
COMMERCE_ACCESS_TOKEN_SECRET=
```


## Note

**`PAYMENT_THRESHOLD`** — Deve corrispondere a `split_payment/general/threshold` nella configurazione di sistema di **[!UICONTROL Commerce]**. Entrambi i lati hanno valore predefinito `100` se il valore è mancante, non numerico o minore o uguale a `0`. Se modifichi la soglia in **[!UICONTROL Commerce]**, aggiorna App Builder `.env` in modo che corrisponda.

**`DEMO_UI_SECRET`** — Facoltativo ma consigliato per qualsiasi distribuzione che non sia localhost. Chiunque disponga dell’URL del dashboard può elencare gli ordini ed eseguire Accetta e rifiuta se questo campo è vuoto. Per un ambiente di staging reale, imposta un segreto condiviso.

**`COMMERCE_BASE_URL`** — Non includere mai una barra finale. Il client REST di Commerce aggiunge automaticamente `/rest/V1/`.

**`AIO_CLI_ENV`** — Imposta su `stage` per l&#39;area di lavoro **[!UICONTROL Stage]**. Cambia in `prod` quando distribuisci in **[!UICONTROL Production]**.


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
