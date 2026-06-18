---
title: Dividi POC pagamento — passaggi successivi alla prova di concetto
description: Scopri come spostare il POC di pagamento frazionato verso la produzione. Interfaccia utente di Experience Cloud, hook ERP, Mesh API, ambito PHP, flussi di lavoro di App Builder ed elenco di controllo per la distribuzione.
feature: App Builder, API Mesh, Extensibility, Paas, REST, Eventing
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, User
level: Intermediate
doc-type: Tutorial
duration: 269
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 63ac13d8c5a97ee81dcdd1f3785a9875aaf2a4db
workflow-type: tm+mt
source-wordcount: '852'
ht-degree: 0%

---

# Dividi pagamento POC: passaggi successivi alla prova di concetto

Il dashboard demo e lo script di simulazione creati in questa esercitazione sono intenzionalmente approssimativi. Esistono per dimostrare il modello. Questa pagina descrive un percorso pratico da proof of concept a sviluppo App Builder in stile produzione.


## Passaggio 1: sostituire Demo Dashboard con un’estensione dell’interfaccia utente di Experience Cloud

L&#39;azione Web `demo-dashboard` utilizza HTML da una stringa all&#39;interno di una funzione Node.js. Funziona, ma non è il modello di produzione.

La sostituzione corretta è l&#39;estensione `commerce-backend-ui-1` in `commerce-checkout-starter-kit` — un&#39;applicazione React incorporata in Commerce Admin Shell tramite Adobe Admin UI SDK. Per il prompt di generazione, consulta il [prompt di IA dell&#39;estensione dell&#39;interfaccia utente di Experience Cloud](./experience-cloud-ui-prompt.md) per dividere il POC dei pagamenti.

**Modifiche:**
* Gli operatori accedono tramite Commerce Admin Shell (autenticazione IMS anziché segreto condiviso)
* L’elenco degli ordini utilizza il contesto del token IMS, non è necessario un segreto demo
* L’ambito delle azioni di accettazione/rifiuto corrisponde all’identità IMS dell’operatore
* L’interfaccia utente è incorporata in Commerce Admin all’URL che gli amministratori di Commerce già conoscono

**Cosa rimane invariato:**
* `payment-accept` e `payment-decline` azioni App Builder sono invariate
* Gli endpoint REST di Commerce sono invariati
* Il modulo PHP rimane invariato


## Passaggio 2 — Collegare un ERP reale

Il flusso di accettazione/rifiuto in questo PoC è guidato da un utente che fa clic su un pulsante. In produzione, questo è determinato dal processore ERP, CRM o di pagamento.

**Schema di integrazione:**
1. Il tuo sistema ERP acquisisce contanti e chiama `POST /payment-accept` (l&#39;URL azione Web di App Builder) con `{ orderId: <entity_id> }`
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

Il modulo PHP corrente gestisce cinque elementi che devono rimanere in elaborazione (vedere [Dividi POC di pagamento: decisioni su architettura e progettazione](./architecture-and-decisions.md)). Con la maturazione della superficie API di Adobe Commerce, alcune di queste possono diventare mobili:

**Possibile spostamento in futuro:**
* Il credito store REST API è in evoluzione; le versioni future potrebbero supportare l’applicazione del credito post-ordine o ai carrelli inattivi
* Man mano che più operazioni Commerce diventano asincrone, la protezione di soglia può essere spostata su un risolutore Mesh API pre-ordine

**Non mobile (vincoli architetturali):**
* La manipolazione dei preventivi prima di `placeOrder()` richiederà sempre codice in-process a meno che Commerce non esprima un hook pulito tramite un punto di estensione API-first
* Gli endpoint REST (`/V1/split-payment/*`) sono specifici di questa funzionalità; risiedono in Commerce perché chiamano i servizi interni di Commerce


## Passaggio 5 — Aggiungere altro flusso di lavoro ad App Builder

Il PoC corrente esegue una delle seguenti operazioni: ascolta il posizionamento dell&#39;ordine, quindi abilita l&#39;opzione Accetta/rifiuta. Estensioni naturali:

**Punteggio frode prima dell&#39;accettazione:**
In `payment-orchestrator`, dopo la registrazione di contanti in sospeso, chiamare un&#39;API per il punteggio di frode prima che il risultato dell&#39;orchestrazione sia considerato finale. Se il punteggio è superiore a una soglia, rifiuta automaticamente invece di attendere l’azione dell’operatore.

**E-mail di notifica:**
Quando `payment-accept` ha esito positivo, attiva un&#39;e-mail (tramite Adobe Campaign, SendGrid o qualsiasi API HTTPS) per avvisare il cliente che il pagamento in contanti è stato confermato.

**Punti fedeltà:**
Una volta confermata la disponibilità liquida, chiama un’API fedeltà per assegnare punti. Questo è puro App Builder — non è richiesto alcun PHP.

**Gestione timeout:**
Aggiungi un&#39;azione App Builder pianificata (utilizzando `cron` in `app.config.yaml`) che analizza gli ordini con `split_cash_status = 'pending'` più vecchi di X giorni e li rifiuta automaticamente.


## Passaggio 6 — Implementare in produzione

PoC configurato per l&#39;area di lavoro `Stage`. Passaggio alla produzione:

```bash
# Switch to production workspace
aio app use
# Select Production workspace

# Update .env for production
# (same credentials, but production Commerce base URL)

# Deploy
aio app deploy
```

**Elenco di controllo per la preparazione alla produzione:**
* [ Set ] `DEMO_UI_SECRET` (o dashboard demo sostituito con l&#39;interfaccia utente di Experience Cloud)
* [ ] `LOG_LEVEL=warn` o `error` in produzione (non `debug`)
* [ ] `PAYMENT_THRESHOLD` corrisponde alla configurazione di produzione Commerce
* [ Le credenziali di integrazione di ] Commerce in `.env` sono per un&#39;integrazione di produzione dedicata (non per staging)
* [ ] Aggiornamento del inserisco nell&#39;elenco Consentiti di IP fastly con gli IP in uscita di produzione di App Builder (Commerce Cloud)
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
