---
title: Commerce componibile con Adobe
description: Scopri l’e-commerce componibile con le opzioni della piattaforma Adobe, le vetrine Edge Delivery Services, l’API Mesh, App Builder e gli strumenti MCP di sviluppo Commerce.
feature: API Mesh, App Builder, Saas
topic: Architecture, Commerce, Development, Headless, Integrations, Performance, Personalization
role: Admin, Developer, User
level: Intermediate
doc-type: Tutorial
duration: 307
last-substantial-update: 2026-11-13
jira: KT-15730
exl-id: 4d811a2f-8488-4de7-babd-449aced42e3a
TQID: 'https://experienceleague.adobe.com/NG-US7zLBgzV425mheo3oQ9Z6gnzAr6aRCLNjHVU0aM'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
    internal-label: Security
  - id: c32adafa-ed01-4b31-997e-2413013911b0
    internal-label: Integrations
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
    internal-label: Architecture
  - id: d3b92bef-63fa-5031-a925-d04d9362d616
    internal-label: Saas
  - id: cc250cf1-34eb-4863-80d0-d170d45ea067
    internal-label: Developer tools
subfeature_v2:
  - id: a743e5dc-8f37-4b5d-a848-03c32ca30598
    internal-label: App Builder
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
    internal-label: User
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
    internal-label: Intermediate
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
    internal-label: Implementation
  - id: bce87dde-a4ab-44c9-8a18-ad66e4ddb377
    internal-label: Customer experience
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
    internal-label: Security
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
    internal-label: Personalization
  - id: f4e6943a-c91a-4134-a2c7-f4f20cfff2f0
    internal-label: Privacy
autotag-review: '2026-11-13T20:04:10.194Z'
source-git-commit: 3a52f4de8c63f08418f4fa44833a7bf7eb90175b
workflow-type: tm+mt
source-wordcount: '1311'
ht-degree: 0%
---
# Commercio componibile

L’e-commerce componibile è un approccio architettonico che combina le funzionalità di e-commerce tramite API. È possibile selezionare e integrare i componenti per le esperienze di vetrina, la gestione dei cataloghi, l&#39;individuazione dei prodotti, il pagamento e altre esigenze aziendali, invece di richiedere tutte le funzionalità da un&#39;unica applicazione strettamente collegata.

L’e-commerce headless separa il livello di presentazione vetrina dal backend e-commerce. Il Commercio componibile si basa su questa separazione, consentendo ai team di scegliere quali funzionalità mantenere insieme e quali integrare in modo indipendente. Una vetrina headless non rende intrinsecamente sostituibile ogni componente backend in modo indipendente.

Adobe supporta questo approccio attraverso opzioni di piattaforma commerce, tecnologia di vetrina e strumenti di integrazione e sviluppo. Gli MCP di sviluppo Adobe Commerce as a Cloud Service (ACCS), Adobe Commerce Optimizer (ACO), Edge Delivery Services, API Mesh, App Builder e Commerce hanno scopi diversi. Non si tratta di prodotti intercambiabili o di uno stack combinato richiesto.

## Vantaggi e compromessi

Un approccio componibile può aiutarti a:

* **Adattare l&#39;esperienza del cliente:** Scegliere i componenti storefront e integrare i servizi che soddisfano i requisiti aziendali.
* **Modernizza in modo incrementale:** Sostituisci le funzionalità selezionate mantenendo gli altri sistemi e i relativi dati.
* **Ridimensiona ed evolvi in modo indipendente:** Progetta integrazioni ed estensioni in modo che le loro modifiche non richiedano il rilascio dell&#39;intera applicazione commerce.
* **Riutilizzare le funzionalità di e-commerce:** Utilizzare le funzionalità e le API della piattaforma supportate anziché ricreare ogni funzione aziendale.

Questi vantaggi dipendono dall’implementazione. Valuta la maturità tecnica, i costi di consegna, le licenze, la sicurezza e la privacy dei dati. Definisci la proprietà dei contratti API, la sincronizzazione dei dati, l’autenticazione, le prestazioni e gli errori tra i servizi. Un&#39;architettura più modulare può anche aumentare l&#39;integrazione e la complessità operativa.

## Opzioni dell’architettura di Adobe Commerce

Per iniziare, decidi se hai bisogno di una piattaforma di e-commerce gestita o se desideri modernizzare le esperienze dei clienti attorno a un back-end transazionale esistente.

Per un confronto video, consulta [Opzioni di e-commerce disponibili di Adobe](./adobes-available-commerce-options.md).

### Adobe Commerce as a Cloud Service

Adobe Commerce as a Cloud Service è una piattaforma di e-commerce SaaS gestita. Adobe gestisce l’applicazione principale, l’infrastruttura e gli aggiornamenti. La sua architettura include Commerce Foundation, funzionalità di vetrina headless, servizi di merchandising e strumenti per sviluppatori.

Utilizza questa opzione durante la valutazione di una base di e-commerce gestita con punti di estensione supportati. Per la logica di business personalizzata, pianificare l&#39;estensibilità fuori processo supportata anziché presupporre che le estensioni PHP esistenti possano essere distribuite immutate. ACS non supporta le vetrine Luma; se applicabile, includi la migrazione delle vetrine nel piano di implementazione.

Consulta la [panoramica di Adobe Commerce as a Cloud Service](https://experienceleague.adobe.com/en/docs/commerce/cloud-service/overview).

### Adobe Commerce Optimizer

Adobe Commerce Optimizer supporta la gestione del catalogo, l’individuazione dei prodotti, le ricerche, i consigli e le esperienze di vetrina, consentendo al contempo di mantenere un back-end di e-commerce esistente. I dati del catalogo possono provenire da fonti esterne e un sistema di terze parti può rimanere responsabile del carrello e del ciclo di vita del pagamento.

Utilizza questa opzione quando l’obiettivo è la modernizzazione rivolta al cliente e non è necessario sostituire il back-end transazionale. Pianifica esplicitamente l’acquisizione del catalogo e le integrazioni di carrello e pagamento. ACCS e ACO sono opzioni di architettura separate, non uno stack combinato richiesto. Verifica il supporto dell’integrazione corrente prima di selezionare i servizi per l’implementazione.

Consulta la [panoramica di Adobe Commerce Optimizer](https://experienceleague.adobe.com/en/docs/commerce/optimizer/overview).

### Distribuzioni esistenti di Adobe Commerce

Adobe Commerce su infrastruttura cloud e implementazioni on-premise può anche adottare in modo incrementale vetrine headless e integrazioni basate su API supportate. A seconda della distribuzione, puoi mantenere una vetrina esistente o adottare una vetrina headless supportata. Mantenere unite le funzionalità di e-commerce esistenti può ridurre il lavoro di integrazione consentendo al contempo l’evoluzione separata di determinati servizi.

La disponibilità delle funzioni, la compatibilità delle estensioni, le licenze e i requisiti di configurazione variano a seconda dell’offerta e della versione. Confronta le [piattaforme e servizi Adobe Commerce](https://experienceleague.adobe.com/en/docs/commerce/information-center/explore-platforms-and-services/platform-and-services) prima di scegliere un&#39;architettura.

## Commerce Storefront con tecnologia Edge Delivery Services

Commerce Storefront con tecnologia Edge Delivery Services fornisce l&#39;esperienza vetrina e il livello di distribuzione dei contenuti. La sua architettura headless consente ai team di sviluppare il front-end in modo indipendente dalla commerce foundation. L’authoring basato su documenti e la modifica visiva supportano gli aggiornamenti dei contenuti, mentre i componenti della vetrina utilizzano le API Commerce per le funzionalità di acquisto.

È possibile distinguere l&#39;interfaccia vetrina dai sistemi responsabili dei dati di catalogo, dei prezzi, delle operazioni del carrello, del pagamento e degli ordini. Edge Delivery Services non sostituisce il back-end di e-commerce transazionale. Identifica i servizi supportati e il percorso di integrazione per il backend scelto prima di implementare la vetrina.

Consulta la [documentazione di Commerce Storefront](https://experienceleague.adobe.com/en/tools/commerce-storefront/).

## Integrazione ed estensibilità

API Mesh e App Builder consentono di collegare ed estendere le funzionalità di e-commerce senza richiedere che ogni personalizzazione sia incorporata nell’applicazione commerce di base.

### Mesh API

API Mesh per Adobe Developer App Builder combina più origini API Adobe, private e di terze parti in un endpoint GraphQL interrogabile. Utilizzala per comporre dati per esperienze front-end e coordinare l’accesso ai servizi esistenti.

La composizione API non elimina la necessità di definire contratti API, autenticazione, gestione degli errori o proprietà della logica di business transazionale. Mantenere esplicite queste responsabilità nella progettazione dell&#39;integrazione.

Consulta la [Panoramica di API Mesh](https://developer.adobe.com/graphql-mesh-gateway/).

### Adobe App Builder

Adobe App Builder fornisce una piattaforma per le applicazioni che estendono le soluzioni Adobe ed vengono eseguite sull’infrastruttura Adobe, incluso Adobe I/O Runtime. Puoi utilizzarlo per personalizzazioni fuori processo e integrazioni con sistemi come CRM, ERP e PIM.

Scegli i punti di estensione, gli eventi e i webhook supportati per l’offerta Commerce che utilizzi. Progetta e testa l’applicazione separatamente dal ciclo di vita della versione di Commerce di base.

Consulta la [panoramica di App Builder](https://developer.adobe.com/app-builder/docs/intro_and_overview/).

## MCP di sviluppo e competenze Commerce

Gli MCP di sviluppo e le competenze di Commerce supportano lo sviluppo basato sull’intelligenza artificiale di applicazioni App Builder e personalizzazioni della vetrina. Possono anche aiutare a migrare le estensioni PHP di Adobe Commerce esistenti alle applicazioni App Builder durante il passaggio ad ACS.

I server MCP (Model Context Protocol) collegano i client di codifica IA compatibili agli strumenti di sviluppo. Gli strumenti di Adobe includono Commerce Developer MCP Server, App Builder MCP Server e Dropins AI Tools, insieme a competenze specifiche per Commerce.

Gli MCP di sviluppo Commerce sono strumenti di sviluppo, non servizi di consegna in vetrina, carrello, pagamento o elaborazione degli ordini. Non rendono un&#39;applicazione compatibile automaticamente con ogni offerta Commerce. Rivedi le modifiche generate e testale nell’ambiente di destinazione.

Consulta [MCP di sviluppo Commerce e competenze](https://developer.adobe.com/commerce/extensibility/developer-agent/coding-tools).

## Pianificare un’implementazione componibile

1. Definisci gli obiettivi aziendali, le esperienze dei clienti, i requisiti di sicurezza e le competenze disponibili per il team.
2. Scegli la piattaforma commerce e identifica i sistemi esistenti da mantenere o sostituire.
3. Seleziona una vetrina supportata e definisci i limiti tra i servizi di presentazione, merchandising e transazionali.
4. Stabilisci la proprietà dei dati e i contratti API per informazioni sul catalogo, prezzi, inventario, clienti, carrelli, pagamento e ordini.
5. Pianifica l’autenticazione, la sincronizzazione dei dati, la memorizzazione in cache, il monitoraggio e la gestione degli errori tra le integrazioni.
6. Implementa in modo incrementale e convalida funzionalità, prestazioni e sicurezza prima di ogni versione.

Un&#39;implementazione componibile ha successo quando i suoi componenti soddisfano i requisiti aziendali e lavorano insieme in modo affidabile. È possibile selezionare le funzionalità necessarie anziché aggiungere servizi solo per rendere l&#39;architettura più modulare.

## Risorse correlate

Esplora questi tutorial, video, guide all’implementazione e la playlist per pianificare la tua architettura e creare integrazioni commerce.

### Piattaforme Commerce e vetrine

* [Architettura di Adobe Commerce as a Cloud Service](../commerce-as-a-cloud-service/architecture.md)
* [Vetrine mobili e architettura modulare](../commerce-as-a-cloud-service/product-overview/composable-storefront-and-modular-architecture.md)
* [Panoramica di Adobe Commerce Optimizer](../adobe-commerce-optimizer/overview.md)
* [Perché esiste il modello dati del catalogo componibile](../adobe-commerce-optimizer/composable-catalog-data-model/why-ccdm-exists.md)
* [Esperienze di acquisto con Edge Delivery Services](../edge-delivery-services/shopper-experience.md)

### Integrazione e sviluppo

* [Estensibilità out-of-process con App Builder](../app-builder/foundations/introduction-to-app-builder.md)
* [Introduzione a Mesh API](../api-mesh/getting-started-api-mesh.md)
* [Creare un Mesh API con più sorgenti](../api-mesh/graphql-multiple-source.md)
* [Creare tariffe di spedizione personalizzate con strumenti AI e MCP](../app-builder/custom-shipping-rates-ai-tools/ai-tools-starter-kit.md)

### Documentazione sull’architettura e lo sviluppo

* [Migrazione ad Adobe Commerce as a Cloud Service](https://experienceleague.adobe.com/en/docs/commerce/cloud-service/migration/overview)
* [Integrazioni Adobe Commerce Optimizer](https://experienceleague.adobe.com/en/docs/commerce/optimizer/integrations-overview)
* [Guida introduttiva allo sviluppo per Commerce Storefront](https://experienceleague.adobe.com/en/tools/commerce-storefront/get-started/)
* [Best practice per MCP di sviluppo e competenze Commerce](https://developer.adobe.com/commerce/extensibility/developer-agent/best-practices)

### Playlist

* [Introduzione a Mesh API con Adobe App Builder e Adobe Commerce](https://experienceleague.adobe.com/en/playlists/commerce-get-started-app-builder-and-api-mesh)
