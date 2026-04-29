---
title: 'Split payment POC: next steps after the proof of concept'
description: 'Learn how to move the split payment POC toward production: Experience Cloud UI, ERP hooks, API Mesh, PHP scope, App Builder workflows, and deploy checklist.'
feature: App Builder, API Mesh, Extensibility, Paas, REST, Eventing
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 269
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 1e2c7e0e6d0f2d174b88406ce3fb7c787676ecee
workflow-type: tm+mt
source-wordcount: '852'
ht-degree: 0%

---

# Split payment POC: next steps after the proof of concept

The demo dashboard and simulation script you built in this tutorial are intentionally rough. They exist to prove the pattern. This page describes a practical path from proof of concept to production-style App Builder development.


## Step 1 — Replace the Demo Dashboard with an Experience Cloud UI Extension

The `demo-dashboard` web action serves HTML from a string inside a Node.js function. It works, but it is not the production pattern.

The proper replacement is the `commerce-backend-ui-1` extension in the `commerce-checkout-starter-kit` — a React application embedded in the Commerce Admin Shell via the Adobe Admin UI SDK. See [Split payment POC: Experience Cloud UI extension AI prompt](split-payment-poc-experience-cloud-ui-prompt.md) for the generation prompt.

**What changes:**
* Operators log in through Commerce Admin Shell (IMS authentication instead of a shared secret)
* The order list uses the IMS token context — no need for a demo secret
* Accept/decline actions are scoped to the operator&#39;s IMS identity
* The UI is embedded in Commerce Admin at the URL Commerce administrators already know

**What stays the same:**
* `payment-accept` and `payment-decline` App Builder actions are unchanged
* Commerce REST endpoints are unchanged
* The PHP module is unchanged


## Step 2 — Connect a Real ERP

The accept/decline flow in this PoC is driven by a human clicking a button. In production, this is driven by your ERP, CRM, or payment processor.

**The integration pattern:**
1. Your ERP system captures cash and calls `POST /payment-accept` (the App Builder web action URL) with `{ orderId: <entity_id> }`
2. App Builder convalida la chiamata (token Bearer o chiave API — aggiungi middleware di autenticazione a `payment-accept`)
3. App Builder chiama Commerce REST `cash-received`
4. Fatture Commerce e spedizione dell&#39;ordine

Non sono richieste modifiche PHP. La superficie REST è già presente. L’azione App Builder richiede solo un chiamante sicuro invece di un pulsante del dashboard.

**Opzioni di autenticazione per il chiamante ERP:**
* Adobe I/O Runtime supporta `require-adobe-auth: true` per i token IMS (se l&#39;ERP può ottenere un token IMS)
* Per i sistemi non Adobe: aggiungi una semplice verifica della chiave API nell&#39;azione `payment-accept` (confronta un&#39;intestazione con un segreto memorizzato nell&#39;env dell&#39;azione)


## Passaggio 3: aggiungere rete API come livello broker

Attualmente, App Builder chiama Commerce REST direttamente con le credenziali OAuth 1.0a. Per implementazioni più estese, Adobe API Mesh fornisce un livello gateway gestito tra App Builder e Commerce.

**Vantaggi:**
* Gestione centralizzata delle credenziali: API Mesh contiene le credenziali Commerce; App Builder chiama API Mesh
* Richiedi trasformazione: mappatura dei payload di App Builder alle forme Commerce REST senza modificare l&#39;azione App Builder
* Limitazione della velocità e caching: protezione di Commerce da elevati volumi di traffico di eventi

**Schema:**
* Sostituisci le chiamate di `createCommerceClient(params, logger)` con le chiamate all&#39;endpoint Mesh API
* API Mesh gestisce la firma OAuth verso Commerce


## Passaggio 4 — Ridurre l&#39;ingombro PHP

Il modulo PHP corrente gestisce cinque elementi che devono rimanere in elaborazione (vedere [Dividi POC di pagamento: decisioni su architettura e progettazione](split-payment-poc-architecture-and-decisions.md)). Con la maturazione della superficie API di Adobe Commerce, alcune di queste possono diventare mobili:

**Possibile spostamento in futuro:**
* Il credito store REST API è in evoluzione; le versioni future potrebbero supportare l’applicazione del credito post-ordine o ai carrelli inattivi
* Man mano che più operazioni Commerce diventano asincrone, la protezione di soglia può essere spostata su un risolutore Mesh API pre-ordine

**Not movable (architectural constraints):**
* Quote manipulation before `placeOrder()` will always require in-process code unless Commerce exposes a clean hook via an API-first extension point
* The REST endpoints (`/V1/split-payment/*`) are specific to this feature; they live in Commerce because they call Commerce-internal services


## Step 5 — Add More Workflow to App Builder

The current PoC does one thing: listen for order placement, then enable accept/decline. Natural extensions:

**Fraud scoring before accept:**
In `payment-orchestrator`, after recording cash pending, call a fraud scoring API before the orchestration result is considered final. If the score is above a threshold, auto-decline instead of waiting for operator action.

**Notification emails:**
When `payment-accept` succeeds, trigger an email (via Adobe Campaign, SendGrid, or any HTTPS API) notifying the customer that their cash payment was confirmed.

**Loyalty point awards:**
After cash is confirmed, call a loyalty API to award points. This is pure App Builder — no PHP required.

**Timeout handling:**
Add a scheduled App Builder action (using `cron` in `app.config.yaml`) that scans for orders with `split_cash_status = 'pending'` older than X days and auto-declines them.


## Step 6 — Deploy to Production

The PoC is configured for `Stage` workspace. Moving to production:

```bash
# Switch to production workspace
aio app use
# Select Production workspace

# Update .env for production
# (same credentials, but production Commerce base URL)

# Deploy
aio app deploy
```

**Checklist for production readiness:**
* [ ] `DEMO_UI_SECRET` set (or demo dashboard replaced with Experience Cloud UI)
* [ ] `LOG_LEVEL=warn` or `error` in production (not `debug`)
* [ ] `PAYMENT_THRESHOLD` matches Commerce production config
* [ ] Commerce Integration credentials in `.env` are for a dedicated production integration (not staging)
* [ ] Fastly IP allowlist updated with App Builder production egress IPs (Commerce Cloud)
* [ Registrazione dell&#39;evento di I/O ] confermata nell&#39;area di lavoro di produzione


## Diagramma dell’evoluzione dell’architettura

```
PoC (this tutorial)
┌─────────────────────────────────────────────────┐
│  Commerce                 App Builder            │
│  ┌─────────────┐         ┌──────────────────┐   │
│  │  PHP Module │◄────────│ payment-orchestr │   │
│  │  REST API   │────────►│ payment-accept   │   │
│  │  Checkout UI│         │ payment-decline  │   │
│  └─────────────┘         │ demo-dashboard   │   │
└─────────────────────────────────────────────────┘

Phase 2: Experience Cloud UI
┌─────────────────────────────────────────────────┐
│  Commerce                 App Builder            │
│  ┌─────────────┐         ┌──────────────────┐   │
│  │  PHP Module │◄────────│ payment-orchestr │   │
│  │  REST API   │────────►│ payment-accept   │   │
│  │  Checkout UI│         │ payment-decline  │   │
│  │             │         │ Admin UI ext.    │   │
│  └─────────────┘         └──────────────────┘   │
└─────────────────────────────────────────────────┘

Phase 3: Real ERP + API Mesh
┌──────────────────────────────────────────────────────────┐
│  ERP  ──►  App Builder  ──►  API Mesh  ──►  Commerce     │
│            (orchestrator)   (gateway)    (PHP module      │
│            (accept)                      REST surface)    │
└──────────────────────────────────────────────────────────┘
```


## Riferimenti chiave

* [Documentazione di Adobe App Builder](https://developer.adobe.com/app-builder/docs/overview/){target="_blank"}
* [Adobe I/O Events per Commerce](https://developer.adobe.com/commerce/extensibility/events/){target="_blank"}
* [Commerce Checkout Starter Kit](https://github.com/adobe/commerce-checkout-starter-kit){target="_blank"}
* [SDK interfaccia di amministrazione di Adobe](https://developer.adobe.com/commerce/extensibility/admin-ui-sdk/){target="_blank"}
* [Mesh API di Adobe](https://developer.adobe.com/graphql-mesh-gateway/){target="_blank"}
* [Adobe I/O Runtime (OpenWhisk)](https://developer.adobe.com/runtime/docs/){target="_blank"}



{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
