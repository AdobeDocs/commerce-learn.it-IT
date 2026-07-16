---
title: Dividi POC pagamento — riferimento rapido tutorial
description: Scopri la mappa del file POC per il pagamento frazionato. Quale pagina di Experience League corrisponde a ogni prompt di IA, ordine di sezione suggerito e note dell’autore per il checkout Luma.
feature: App Builder, Extensibility, Paas, REST, Eventing
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, User
level: Intermediate
doc-type: Tutorial
duration: 398
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: a9472912c20d157e310abfece16519156b10945f
workflow-type: tm+mt
source-wordcount: '1515'
ht-degree: 0%

---

# Dividi POC pagamento: riferimento rapido tutorial

In questa pagina viene riepilogato come è organizzata la serie di tutorial proof-of-concept per il pagamento frazionato. Mappa ogni file del prompt sorgente numerato alla relativa pagina Experience League pubblicata, elenca un ordine di sezione suggerito per i lettori e raccoglie note per autori e responsabili.


## Riferimento file per file

### Creazione di un POC di pagamento frazionato: strumenti App Builder e AI

[Creazione di un POC di pagamento frazionato: strumenti App Builder e AI](./overview.md)

**Scopo:** Introduzione e orientamento per l&#39;esercitazione.

**Perché è necessario:** offre agli sviluppatori il &quot;perché&quot; prima del &quot;come&quot;. Spiega cosa costruiranno, chi è il pubblico e, in modo critico, cosa devono personalizzare se il loro sito differisce da un’installazione Luma pulita. Funge anche da sommario per il resto della serie.

**Utilizzo esercitazione:** Apertura della sezione. Imposta il contesto prima di qualsiasi passaggio tecnico.


### Dividi pagamento POC: decisioni su architettura e progettazione

[Dividi pagamento POC: decisioni su architettura e progettazione](./architecture-and-decisions.md)

**Scopo:** spiegazione dettagliata di ogni decisione di architettura nel PoC.

**Perché è necessario:** Il punto di confusione più comune quando si lavora con questo PoC è capire *perché* qualcosa è in PHP rispetto ad App Builder. Senza questo contesto, gli sviluppatori tentano di spostare elementi che non possono essere spostati (come l’applicazione di credito all’archivio) e non riescono. Il presente documento previene tali carenze con una chiara motivazione.

**Argomenti chiave trattati:**

* La regola &quot;cosa deve rimanere in PHP&quot; e perché
* Schema di doppia applicazione della soglia
* Perché la richiesta di credito dell&#39;archivio non può essere asincrona
* I cinque casi edge di Commerce gestiti dai plug-in
* Flusso di dati dell&#39;attributo dell&#39;estensione dal checkout → preventivo → ordine → I/O Event → App Builder

**Utilizzo esercitazione:** sezione &quot;Architettura&quot; o &quot;Funzionamento&quot;. Può essere saltato da sviluppatori esperti di Commerce, ma è essenziale per i nuovi arrivati di App Builder.


### Dividi POC pagamento: prerequisiti e impostazione dell&#39;ambiente

[Dividi POC pagamento: prerequisiti e impostazione dell&#39;ambiente](./prerequisites-and-setup.md)

**Scopo:** completare l&#39;elenco di controllo prima della verifica prima della scrittura del codice o dell&#39;esecuzione delle richieste.

**Perché è necessario:** La fase di installazione presenta la più alta percentuale di errori nelle esercitazioni. Questo documento assicura che l’amministratore di Commerce sia configurato correttamente (titolo COD esatto, credito archiviato abilitato, test del saldo del cliente, creazione dell’integrazione), che il progetto App Builder disponga di eventi di I/O e che il provider di eventi Commerce sia connesso e che l’ambiente locale disponga delle versioni corrette.

**Elementi di enfasi:**

* Il titolo del contrassegno deve essere esattamente `Cash` (critico: JavaScript esegue la corrispondenza delle stringhe)
* L&#39;integrazione di Commerce deve essere *attivata* (non solo salvata) affinché le credenziali OAuth funzionino
* `entity_id` rispetto a `increment_id` qui spiegati per evitare confusione in tutto

**Utilizzo esercitazione:** sezione &quot;Prerequisiti&quot; o &quot;Guida introduttiva&quot;. Deve essere completato in modo interattivo, non solo letto.


### Dividi pagamento POC: riferimento variabili ambiente

[Dividi pagamento POC: riferimento variabili ambiente](./env-reference.md)

**Scopo:** tutte le variabili di ambiente per tutti e tre i componenti in un&#39;unica posizione.

**Perché è necessario:** Le stesse quattro credenziali OAuth vengono visualizzate in tre file diversi con nomi di variabili diversi (`COMMERCE_*` rispetto a `COMMERCE_INTEGRATION_*`). L&#39;utilizzo di un singolo riferimento impedisce il debug &quot;perché non funziona&quot; in cui un set di credenziali presenta un errore di battitura.

**Componenti coperti:**

* `split-payment-orchestrator/.env`
* `commerce-checkout-starter-kit/.env` (IMS + OAuth)
* `commerce-backend-ui-1/.env.simulation`

**Uso del tutorial:** Barra laterale di riferimento o sezione &quot;Configurazione&quot;. Utilizzato anche come complemento ai prompt di generazione.


### Dividi pagamento POC: richiesta di IA del modulo Commerce

[Dividi pagamento POC: richiesta di IA del modulo Commerce](./commerce-module-prompt.md)

**Scopo:** richiesta di IA completa e autonoma per generare l&#39;intero modulo PHP `Client_SplitPayment`.

**Perché è necessario:** Questo è l&#39;artefatto primario della build AI per il lato PHP. È scritto in modo che uno sviluppatore senza esperienza PHP possa passarlo a Cursor (con Claude) e ottenere un modulo di lavoro. Vengono specificati tutti i file, viene definito ogni contratto comportamentale, vengono incluse le note critiche sull’implementazione e vengono forniti i comandi per abilitare il modulo in seguito.

**Copertura:**

* Struttura completa dei file (oltre 30 file)
* Schema del database, attributi di estensione, endpoint REST, configurazione eventi I/O
* Tutte le classi PHP con specifiche comportamentali complete (non solo le firme)
* KnockoutJS checkout specifiche dei componenti
* I cinque plug-in per casi edge con spiegazione del motivo per cui esistono
* Callout di personalizzazione per temi non Luma

**Uso dell&#39;esercitazione:** sezione &quot;Generare il modulo Commerce&quot;. Il prompt stesso è l’artefatto: gli sviluppatori lo copiano nel loro strumento di intelligenza artificiale e lo eseguono.


### Dividi pagamento POC: App Builder Orchestrator AI prompt

[Dividi pagamento POC: App Builder Orchestrator AI prompt](./orchestrator-prompt.md)

**Scopo:** richiesta di IA completa e autonoma per generare l&#39;applicazione App Builder `split-payment-orchestrator`.

**Perché è necessario:** Le quattro azioni di App Builder (`payment-orchestrator`, `payment-accept`, `payment-decline`, `demo-dashboard`) sono il nucleo della storia &quot;cosa è passato dal PHP&quot;. Questo prompt genera tutti gli eventi con specifiche comportamentali complete, struttura `app.config.yaml` corretta e configurazione della registrazione dell&#39;evento.

**Copertura:**

* `app.config.yaml` con tutte e quattro le azioni e la registrazione dell&#39;evento di I/O
* `commerce-client.js` client condiviso OAuth 1.0a
* Tutte e quattro le azioni con specifiche logiche complete
* Lo stub `store-credit.js` è obsoleto (con spiegazione del *perché* non è utilizzato — questo è importante dal punto di vista pedagogico)
* Dashboard demo con rendering, filtro degli ordini e sicurezza di HTML
* `.env.example` con tutte le variabili

**Utilizzo dell&#39;esercitazione:** sezione &quot;Generare l&#39;applicazione App Builder&quot;. Accedi al prompt dei moduli di Commerce.


### Dividi pagamento POC: richiesta di IA per l’estensione dell’interfaccia utente di Experience Cloud

[Dividi pagamento POC: richiesta di IA per l’estensione dell’interfaccia utente di Experience Cloud](./experience-cloud-ui-prompt.md)

**Scopo:** richiesta di IA per generare l&#39;estensione opzionale di SDK dell&#39;interfaccia utente di amministrazione di Experience Cloud.

**Perché è necessario:** Il dashboard demo nel prompt dell&#39;orchestratore è intenzionalmente approssimativo, è una prova di concetto, non di produzione. Questa sezione mostra agli sviluppatori il passaggio successivo: incorporare il dashboard dei pagamenti divisi in Commerce Admin Shell utilizzando l’interfaccia di amministrazione di SDK. Mancava completamente dai prompt originali.

**Copertura:**

* `ext.config.yaml` per l&#39;estensione SDK dell&#39;interfaccia utente amministratore
* Componenti React per il dashboard e i dettagli dell’ordine
* Azioni di back-end che utilizzano l’autenticazione IMS per l’inserimento in elenco e OAuth per l’accettazione/il rifiuto
* Script di simulazione (utilizzato anche per il test)

**Utilizzo dell&#39;esercitazione:** Sezione facoltativa &quot;Ulteriori informazioni&quot; o &quot;Percorso di produzione&quot;. Può essere ignorato se il tutorial si concentra solo sul PoC.


### Dividi pagamento POC: guida al test e alla verifica

[Dividi pagamento POC: guida al test e alla verifica](./testing-and-verification.md)

**Scopo:** guida dettagliata ai test per ogni componente nell&#39;ordine di verifica corretto.

**Perché è necessario:** La creazione dei componenti è la metà del lavoro. Gli sviluppatori devono disporre di un percorso di verifica chiaro che inizi dal livello più basso (colonne di database, endpoint REST) e prosegua fino al flusso end-to-end completo. I prompt originali contenevano un elenco di controllo della configurazione, ma non un flusso di test esplicito.

**Copertura:**

* Verifica dell&#39;installazione del modulo Commerce
* Verifica configurazione amministratore
* Prove di fumo sul punto finale REST (comandi curl)
* Procedura dettagliata dell’interfaccia utente Checkout (inclusi i casi di convalida)
* Test soglia di sicurezza
* Test di posizionamento dell’ordine completo
* Utilizzo degli script di simulazione per accettazione e rifiuto
* Utilizzo dashboard demo
* Ispezione del registro azioni di App Builder
* Dieci modalità di errore comuni con correzioni specifiche

**Utilizzo esercitazione:** sezione &quot;Test&quot; o &quot;Verifica&quot;. È utile anche come riferimento per la risoluzione dei problemi.


### Dividi pagamento POC: passaggi successivi alla prova di concetto

[Dividi pagamento POC: passaggi successivi alla prova di concetto](./next-steps.md)

**Scopo:** roadmap per l&#39;evoluzione del PoC in modelli pronti per la produzione.

**Perché è necessario:** un&#39;esercitazione PoC rischia di lasciare agli sviluppatori un messaggio di tipo &quot;che cosa è ora?&quot; sensazione. Questo documento mappa le progressioni naturali da demo a produzione: sostituzione del dashboard demo con un’estensione Experience Cloud, connessione di un ERP reale, aggiunta di Mesh API, espansione del flusso di lavoro App Builder ed elenco di controllo della distribuzione di produzione.

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
| Introduzione e obiettivi di apprendimento | [Creazione di un POC di pagamento frazionato: strumenti App Builder e AI](./overview.md) |
| Demo end-to-end (video) | [Crea un POC di pagamento frazionato: demo completa di App Builder](./full-demo.md) |
| Architettura: cosa vive dove | [Dividi POC pagamento: decisioni su architettura e progettazione](./architecture-and-decisions.md) |
| Prerequisiti e configurazione | [Dividi POC pagamento: prerequisiti e configurazione dell&#39;ambiente](./prerequisites-and-setup.md) |
| Variabili di ambiente | [Dividi POC pagamento: riferimento variabili ambiente](./env-reference.md) |
| Passaggio 1: creare il modulo Commerce | [Dividi POC pagamento: richiesta di IA modulo Commerce](./commerce-module-prompt.md) |
| Passaggio 2: creare App Builder Orchestrator | [Dividi POC pagamento: richiesta di IA di App Builder Orchestrator](./orchestrator-prompt.md) |
| Passaggio 3: verificare il flusso end-to-end | [Dividi POC pagamento: guida alla verifica e alla verifica](./testing-and-verification.md) |
| Passaggio 4 (facoltativo): estensione dell’interfaccia di amministrazione | [Dividi POC pagamento: richiesta IA estensione interfaccia utente di Experience Cloud](./experience-cloud-ui-prompt.md) |
| Passaggi successivi e percorso di produzione | [Dividi POC pagamento: passaggi successivi alla verifica del concetto](./next-steps.md) |


## Note importanti per gli autori dei tutorial

**Nel presupposto del tema Luma:** ogni prompt di compilazione genera il codice per un&#39;installazione Luma pulita. L&#39;esercitazione deve indicare chiaramente questo punto nella parte superiore. Gli sviluppatori con checkout personalizzati dovranno adattare i percorsi di iniezione `LayoutProcessorPlugin` ed eventualmente la configurazione RequireJS. L’introduzione della serie e i prompt di build includono callout di personalizzazione per questo.

**Nel modulo PHP:** L&#39;esercitazione non fornisce esplicitamente il codice PHP come download di copia e incolla. Il prompt di IA *lo genera*. Questo è intenzionale: insegna il pattern dell’utilizzo dell’intelligenza artificiale per creare estensioni Commerce, non solo la boilerplate di copia e incolla. Tuttavia, il codice generato quando richiesto in un ambiente pulito deve corrispondere esattamente al codice funzionante in `app/code/Client/SplitPayment/`.

**Alla soglia di $100:** è un valore PoC hardcoded. Il tutorial dovrebbe tenere presente che gli archivi reali vorranno configurarlo tramite la configurazione di Commerce Admin anziché tramite una costante in fase di compilazione.

**Dipendenza credito in negozio:** Il flusso di pagamento frazionato generato richiede `Magento_CustomerBalance` (il modulo di credito dell&#39;archivio nativo).


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
