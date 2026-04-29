---
title: 'Dividi POC pagamento: riferimento rapido tutorial'
description: 'Scopri la mappa del file POC per il pagamento diviso: quale pagina di Experience League corrisponde a ciascun prompt di IA, ordine di sezione suggerito e note dell’autore per il pagamento Luma.'
feature: App Builder, Extensibility, Paas, REST, Eventing
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 398
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 1e2c7e0e6d0f2d174b88406ce3fb7c787676ecee
workflow-type: tm+mt
source-wordcount: '1455'
ht-degree: 0%

---

# Dividi POC pagamento: riferimento rapido tutorial

In questa pagina viene riepilogato come è organizzata la serie di tutorial proof-of-concept per il pagamento frazionato. Mappa ogni file del prompt sorgente numerato alla relativa pagina Experience League pubblicata, elenca un ordine di sezione suggerito per i lettori e raccoglie note per autori e responsabili.


## Riferimento file per file

### [Creazione di un POC di pagamento frazionato: strumenti App Builder e AI](create-a-split-payment-poc-app-builder-and-ai-tools.md)

**Etichetta Source:** `00_TUTORIAL_OVERVIEW.md` (panoramica concettuale; la serie pubblicata si apre con questa pagina.)

**Scopo:** Introduzione e orientamento per l&#39;esercitazione.

**Perché è necessario:** offre agli sviluppatori il &quot;perché&quot; prima del &quot;come&quot;. Spiega cosa costruiranno, chi è il pubblico e, in modo critico, cosa devono personalizzare se il loro sito differisce da un’installazione Luma pulita. Funge anche da sommario per il resto della serie.

**Utilizzo esercitazione:** Apertura della sezione. Imposta il contesto prima di qualsiasi passaggio tecnico.


### [Dividi POC pagamento: decisioni su architettura e progettazione](split-payment-poc-architecture-and-decisions.md)


**Scopo:** spiegazione dettagliata di ogni decisione di architettura nel PoC.

**Perché è necessario:** Il punto di confusione più comune quando si lavora con questo PoC è capire *perché* qualcosa è in PHP rispetto ad App Builder. Senza questo contesto, gli sviluppatori tentano di spostare elementi che non possono essere spostati (come l’applicazione di credito all’archivio) e non riescono. Il presente documento previene tali carenze con una chiara motivazione.

**Argomenti chiave trattati:**

* La regola &quot;cosa deve rimanere in PHP&quot; e perché
* Schema di doppia applicazione della soglia
* Perché la richiesta di credito dell&#39;archivio non può essere asincrona
* I cinque casi edge di Commerce gestiti dai plug-in
* Flusso di dati dell&#39;attributo dell&#39;estensione dal checkout → preventivo → ordine → I/O Event → App Builder

**Utilizzo esercitazione:** sezione &quot;Architettura&quot; o &quot;Funzionamento&quot;. Può essere saltato da sviluppatori esperti di Commerce, ma è essenziale per i nuovi arrivati di App Builder.


### [Dividi POC pagamento: prerequisiti e configurazione dell&#39;ambiente](split-payment-poc-prerequisites-and-setup.md)


**Scopo:** completare l&#39;elenco di controllo prima della verifica prima della scrittura del codice o dell&#39;esecuzione delle richieste.

**Perché è necessario:** La fase di installazione presenta la più alta percentuale di errori nelle esercitazioni. Questo documento assicura che l’amministratore di Commerce sia configurato correttamente (titolo COD esatto, credito archiviato abilitato, test del saldo del cliente, creazione dell’integrazione), che il progetto App Builder disponga di eventi di I/O e che il provider di eventi Commerce sia connesso e che l’ambiente locale disponga delle versioni corrette.

**Elementi di enfasi:**

* Il titolo del contrassegno deve essere esattamente `Cash` (critico: JavaScript esegue la corrispondenza delle stringhe)
* L&#39;integrazione di Commerce deve essere *attivata* (non solo salvata) affinché le credenziali OAuth funzionino
* `entity_id` rispetto a `increment_id` qui spiegati per evitare confusione in tutto

**Utilizzo esercitazione:** sezione &quot;Prerequisiti&quot; o &quot;Guida introduttiva&quot;. Deve essere completato in modo interattivo, non solo letto.


### [Dividi POC pagamento: riferimento variabili ambiente](split-payment-poc-env-reference.md)


**Scopo:** tutte le variabili di ambiente per tutti e tre i componenti in un&#39;unica posizione.

**Perché è necessario:** Le stesse quattro credenziali OAuth vengono visualizzate in tre file diversi con nomi di variabili diversi (`COMMERCE_*` rispetto a `COMMERCE_INTEGRATION_*`). L&#39;utilizzo di un singolo riferimento impedisce il debug &quot;perché non funziona&quot; in cui un set di credenziali presenta un errore di battitura.

**Componenti coperti:**

* `split-payment-orchestrator/.env`
* `commerce-checkout-starter-kit/.env` (IMS + OAuth)
* `commerce-backend-ui-1/.env.simulation`

**Uso del tutorial:** Barra laterale di riferimento o sezione &quot;Configurazione&quot;. Utilizzato anche come complemento ai prompt di generazione.


### [Dividi POC pagamento: richiesta di IA modulo Commerce](split-payment-poc-commerce-module-prompt.md)


**Scopo:** richiesta di IA completa e autonoma per generare l&#39;intero modulo PHP `Client_SplitPayment`.

**Perché è necessario:** Questo è l&#39;artefatto primario della build AI per il lato PHP. È scritto in modo che uno sviluppatore senza esperienza PHP possa passarlo a Cursor (con Claude) e ottenere un modulo di lavoro. Vengono specificati tutti i file, viene definito ogni contratto comportamentale, vengono incluse le note critiche sull’implementazione e vengono forniti i comandi per abilitare il modulo in seguito.

**Coverage:**

* Complete file structure (30+ files)
* Database schema, extension attributes, REST endpoints, I/O Events config
* All PHP classes with full behavioral specs (not just signatures)
* KnockoutJS checkout component specs
* The five edge case plugins with explanations of why each exists
* Customization callouts for non-Luma themes

**Tutorial use:** &quot;Build the Commerce module&quot; section. The prompt itself is the artifact — developers copy it into their AI tool and run it.


### [Split payment POC: App Builder orchestrator AI prompt](split-payment-poc-app-builder-orchestrator-prompt.md)


**Purpose:** Complete, self-contained AI prompt to generate the `split-payment-orchestrator` App Builder application.

**Why it&#39;s needed:** The four App Builder actions (`payment-orchestrator`, `payment-accept`, `payment-decline`, `demo-dashboard`) are the core of the &quot;what moved out of PHP&quot; story. This prompt generates all of them with full behavioral specifications, correct `app.config.yaml` structure, and the event registration configuration.

**Coverage:**

* `app.config.yaml` with all four actions and the I/O Event registration
* `commerce-client.js` OAuth 1.0a shared client
* All four actions with complete logic specs
* The `store-credit.js` deprecated stub (with explanation of *why* it&#39;s not used — this is pedagogically important)
* The demo dashboard with HTML rendering, order filtering, and security
* `.env.example` with all variables

**Tutorial use:** &quot;Build the App Builder application&quot; section. Companion to the Commerce module prompt.


### [Split payment POC: Experience Cloud UI extension AI prompt](split-payment-poc-experience-cloud-ui-prompt.md)


**Purpose:** AI prompt to generate the optional Experience Cloud Admin UI SDK extension.

**Why it&#39;s needed:** The demo dashboard in the orchestrator prompt is intentionally rough — it is proof-of-concept, not production. This section shows developers the next step: embedding the split payment dashboard into the Commerce Admin Shell using the Admin UI SDK. It was missing entirely from the original prompts.

**Coverage:**

* `ext.config.yaml` for the Admin UI SDK extension
* React components for the order dashboard and order detail
* Backend actions using IMS auth for listing and OAuth for accept/decline
* The simulation script (also used for testing)

**Tutorial use:** Optional &quot;Going further&quot; or &quot;Production path&quot; section. Can be skipped if the tutorial focuses only on the PoC.


### [Split payment POC: testing and verification guide](split-payment-poc-testing-and-verification.md)


**Purpose:** Step-by-step testing guide covering every component in the correct verification order.

**Why it&#39;s needed:** Building the components is half the work. Developers need a clear verification path that starts from the lowest level (database columns, REST endpoints) and builds to the full end-to-end flow. The original prompts had a setup checklist but no explicit testing flow.

**Coverage:**

* Commerce module installation verification
* Admin configuration verification
* REST endpoint smoke tests (curl commands)
* Checkout UI walkthrough (including validation cases)
* Threshold guard test
* Full order placement test
* Simulation script usage for accept and decline
* Demo dashboard usage
* App Builder action log inspection
* Ten common failure modes with specific fixes

**Utilizzo esercitazione:** sezione &quot;Test&quot; o &quot;Verifica&quot;. È utile anche come riferimento per la risoluzione dei problemi.


### [Dividi POC pagamento: passaggi successivi alla verifica del concetto](split-payment-poc-next-steps.md)


**Scopo:** roadmap per l&#39;evoluzione del PoC in modelli pronti per la produzione.

**Perché è necessario:** un&#39;esercitazione PoC rischia di lasciare agli sviluppatori un messaggio di tipo &quot;che cosa è ora?&quot; sensazione. Questo documento mappa le progressioni naturali da demo a produzione: sostituzione del dashboard demo con un’estensione Experience Cloud, connessione di un ERP reale, aggiunta di Mesh API, espansione del flusso di lavoro App Builder e checklist di distribuzione della produzione.

**Contenuto chiave:**

* Schema dell&#39;evoluzione dell&#39;architettura (PoC → fase 2 → fase 3)
* Schema di integrazione ERP (cosa cambia, cosa rimane invariato)
* Schema broker Mesh API
* Lista di controllo per la distribuzione di produzione
* Collegamenti di riferimento chiave

**Utilizzo esercitazione:** sezione di chiusura &quot;Passaggi successivi&quot;. Utile anche come inizio di conversazione per definire l’ambito di un progetto reale.


## Sezioni tutorial consigliate

In base a questi file, una struttura tipica per i lettori è:

| Sezione Tutorial | Pagina Experience League |
| --- | --- |
| Introduzione e obiettivi di apprendimento | [Creazione di un POC di pagamento frazionato: strumenti App Builder e AI](create-a-split-payment-poc-app-builder-and-ai-tools.md) |
| Demo end-to-end (video) | [Crea un POC di pagamento frazionato: demo completa di App Builder](create-a-split-payment-poc-app-builder-and-ai-tools-full-demo.md) |
| Architettura: cosa vive dove | [Dividi POC pagamento: decisioni su architettura e progettazione](split-payment-poc-architecture-and-decisions.md) |
| Prerequisiti e configurazione | [Dividi POC pagamento: prerequisiti e configurazione dell&#39;ambiente](split-payment-poc-prerequisites-and-setup.md) |
| Variabili di ambiente | [Dividi POC pagamento: riferimento variabili ambiente](split-payment-poc-env-reference.md) |
| Passaggio 1: creare il modulo Commerce | [Dividi POC pagamento: richiesta di IA modulo Commerce](split-payment-poc-commerce-module-prompt.md) |
| Passaggio 2: creare App Builder Orchestrator | [Dividi POC pagamento: richiesta di IA di App Builder Orchestrator](split-payment-poc-app-builder-orchestrator-prompt.md) |
| Passaggio 3: verificare il flusso end-to-end | [Dividi POC pagamento: guida alla verifica e alla verifica](split-payment-poc-testing-and-verification.md) |
| Passaggio 4 (facoltativo): estensione dell’interfaccia di amministrazione | [Dividi POC pagamento: richiesta IA estensione interfaccia utente Experience Cloud](split-payment-poc-experience-cloud-ui-prompt.md) |
| Passaggi successivi e percorso di produzione | [Dividi POC pagamento: passaggi successivi alla verifica del concetto](split-payment-poc-next-steps.md) |


## Note importanti per gli autori dei tutorial

**Nel presupposto del tema Luma:** ogni prompt di compilazione genera il codice per un&#39;installazione Luma pulita. L&#39;esercitazione deve indicare chiaramente questo punto nella parte superiore. Gli sviluppatori con checkout personalizzati dovranno adattare i percorsi di iniezione `LayoutProcessorPlugin` ed eventualmente la configurazione RequireJS. L’introduzione della serie e i prompt di build includono callout di personalizzazione per questo.

**Nel modulo PHP:** L&#39;esercitazione non fornisce esplicitamente il codice PHP come download di copia e incolla. Il prompt di IA *lo genera*. Questo è intenzionale: insegna il pattern dell’utilizzo dell’intelligenza artificiale per creare estensioni Commerce, non solo la boilerplate di copia e incolla. Tuttavia, il codice generato quando richiesto in un ambiente pulito deve corrispondere esattamente al codice funzionante in `app/code/Client/SplitPayment/`.

**Alla soglia di $100:** è un valore PoC hardcoded. Il tutorial dovrebbe tenere presente che gli archivi reali vorranno configurarlo tramite la configurazione di Commerce Admin anziché tramite una costante in fase di compilazione.

**Dipendenza credito in negozio:** Il flusso di pagamento frazionato generato richiede `Magento_CustomerBalance` (il modulo di credito dell&#39;archivio nativo).


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
