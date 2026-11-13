---
title: Avviare, monitorare e risolvere i problemi in Adobe Commerce Cloud
description: Scopri come preparare, avviare, monitorare e risolvere i problemi di Adobe Commerce Cloud utilizzando ambienti di staging, ticket di supporto dettagliati e strumenti di monitoraggio condivisi.
feature: Best Practices, Cloud
topic: Architecture, Commerce, Performance
old-role: Architect, Data Architect, Leader
role: Developer, Leader
level: Beginner, Intermediate
doc-type: Technical Video
duration: 3576
last-substantial-update: 2024-05-07
jira: KT-15467
exl-id: e3748ec2-b449-4cc2-927d-e7ba4f4dbaf8
TQID: 'https://experienceleague.adobe.com/sSK6X9LrKA5LUgdELTzfyNZu8RP9juyYGDdGQfPni4w'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
    internal-label: Security
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
    internal-label: Architecture
  - id: cdfd3bc1-dc23-5cf0-b965-d3c0c55cde67
    internal-label: Best Practices
  - id: 5a951749-fac9-5bc7-9a98-ebe4ff066437
    internal-label: Cloud
role_v2:
  - id: f8a45b24-4be7-4f1b-909b-60d06b483a20
    internal-label: Leader
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
    internal-label: Intermediate
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
    internal-label: Beginner
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
    internal-label: Implementation
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
    internal-label: Troubleshooting
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
    internal-label: Security
source-git-commit: 7ac0cbe05d658701f20137fdea7b9c0b4423d57b
workflow-type: tm+mt
source-wordcount: '388'
ht-degree: 0%
---
# Avviare, monitorare e risolvere i problemi in Adobe Commerce Cloud

Questo webinar fornisce diverse best practice e suggerimenti per la preparazione e il lancio di un sito web. Sottolinea l’importanza di selezionare l’ambiente di staging appropriato e di fornire dettagli precisi nei ticket di supporto per facilitare una comunicazione efficace con il team di supporto. Per migliorare la collaborazione e la risoluzione dei problemi, si consiglia di utilizzare lo stesso strumento di monitoraggio del team di supporto, ad esempio New Relic. Verranno eseguiti test approfonditi sui sistemi periferici, come i processori di pagamento, per garantirne il corretto funzionamento durante il lancio. Il documento sottolinea anche la necessità di prevedere e pianificare potenziali problemi di produzione, come i colli di bottiglia delle prestazioni e il riscaldamento della cache, per garantire un lancio regolare e di successo. La comprensione del modello di responsabilità condivisa è un altro aspetto chiave discusso, chiarendo che alcune attività, come il ripristino del database e la sicurezza del codice dell’applicazione, sono responsabilità del proprietario del sito web. Seguendo queste best practice, è possibile semplificare l’avvio di siti web, affrontare in modo proattivo potenziali problemi e avviare con successo il progetto

## Pubblico

* Team di sviluppo, manager, sviluppatori principali, architetti tecnici
* Team che implementano Adobe Commerce come offerta di aggiornamento, migrazione o nuova offerta commerce

## Contenuto video

* Scegliere l’ambiente di staging appropriato e fornire i dettagli pertinenti nel ticket.
* Utilizzo dello stesso strumento di monitoraggio (New Relic) del team di supporto per migliorare la comunicazione e la risoluzione dei problemi.
* Fornire schermate o collegamenti New Relic durante la descrizione dei problemi per aiutare il supporto a comprendere e risolvere i problemi in modo più efficace.
* Comprendere il modello di responsabilità condivisa, in particolare in termini di ripristino e sicurezza dei database.
* Verifica dei sistemi periferici, ad esempio gli elaboratori dei pagamenti, e garanzia della corretta gestione degli ID degli ordini durante il passaggio al nuovo sistema.
* Previsione e pianificazione di potenziali problemi durante la produzione, come colli di bottiglia delle prestazioni e la necessità di scaldare la cache.


>[!VIDEO](https://video.tv.adobe.com/v/3428990?learn=on)

## Tutorial correlati

* [Infrastruttura cloud e self-hosted: elenco di controllo pre-avvio](https://experienceleague.adobe.com/it/docs/commerce-learn/tutorials/cloud-infrastructure-and-self-hosted/cloud-operations/commerce-cloud-best-practices/pre-launch-checklist)
* [Infrastruttura cloud e self-hosted: avvisi di New Relic](https://experienceleague.adobe.com/it/docs/commerce-learn/tutorials/cloud-infrastructure-and-self-hosted/cloud-operations/tools/new-relic/new-relic-alert-notification-setup)
* [Creare un ticket di supporto](https://experienceleague.adobe.com/it/docs/commerce-learn/tutorials/monitoring-analytics-support/help-and-support/create-a-support-ticket)

{{$include /help/_includes/enablement-series-related-links.md}}
