---
title: Estensibilità out-of-process per Adobe Commerce
description: Scopri Adobe App Builder e perché è un aspetto importante dell’estensibilità out-of-process.
jira: KT-11433
doc-type: Tutorial
duration: 322
last-substantial-update: 2023-02-16T00:00:00.000Z
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Developer
level: Beginner, Intermediate
exl-id: 94f8d82a-4a95-46ea-8eed-edf9bed5760c
TQID: https://experienceleague.adobe.com/c3dl6gZ7Jtje5rZCB9HrwmCbFrcu8bllL0z0B9cl5Jg
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
  - id: c32adafa-ed01-4b31-997e-2413013911b0
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2:
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 777
ht-degree: 2%

---

# Introduzione ad App Builder

In passato, lo sviluppo Adobe Commerce utilizzava l’estensibilità in-process. Il modello in-process richiede che qualsiasi nuovo codice sia compatibile con gli aggiornamenti, la versione PHP del server e molte altre applicazioni e servizi server essenziali utilizzati da Commerce. Adobe Developer App Builder utilizza l’estensibilità out-of-process per evitare questi problemi di compatibilità.

## App Builder per Adobe Commerce {#app-builder}

>[!VIDEO](https://video.tv.adobe.com/v/3412839?learn=on)

Adobe Developer App Builder è una piattaforma di estensibilità senza server per l’integrazione e la creazione di esperienze personalizzate al fine di estendere le soluzioni Adobe, ed è ora disponibile per Adobe Commerce. Con App Builder, puoi creare app sicure e scalabili che estendono le funzionalità native di Commerce e si integrano con soluzioni di terze parti. In qualità di sviluppatore, ora puoi sfruttare l’estensibilità fuori processo con Adobe Commerce, il che a sua volta offre vantaggi immediati e a lungo termine.

App Builder fornisce un framework di estensibilità unificato di terze parti per l&#39;integrazione e la creazione di applicazioni personalizzate che estendono [!DNL Adobe Commerce]. Poiché questo framework di estensibilità è basato sull&#39;infrastruttura di Adobe, gli sviluppatori possono creare microservizi personalizzati ed estendere e integrare [!DNL Adobe Commerce] in altre soluzioni Adobe e integrazioni di terze parti.

App Builder consente ai clienti di estendere [!DNL Adobe Commerce] in vari casi d&#39;uso:

* estensibilità del middleware: collega i sistemi esterni con le applicazioni Adobe creando connettori personalizzati o sfruttando una suite di integrazioni predefinite.
* estensibilità dei servizi di base: estende le funzionalità principali dell’applicazione estendendo il comportamento predefinito con funzioni personalizzate e logica di business.
* estensibilità dell’esperienza utente: estendere l’esperienza di base per supportare i requisiti aziendali o creare proprietà digitali, vetrine e applicazioni di back office specifiche per il cliente.

Adobe Developer App Builder è una soluzione basata su cloud, il che significa che si adatta automaticamente. Questo servizio è inoltre distribuito a livello globale per garantire le migliori prestazioni indipendentemente dalla posizione geografica.

## Perché dovresti saperne di più su App Builder

Poiché Adobe Commerce non è un prodotto completamente SAAS, il codice sviluppato può aggiungere complessità e problemi di aggiornamento. Utilizzando l’estensibilità out-of-process, come App Builder, puoi fornire funzionalità personalizzate e univoche all’archivio Adobe Commerce senza richiedere metodi in-process.

Altri vantaggi comprendono:

* Le funzioni disaccoppiate consentono un avvio più rapido.
* Gli aggiornamenti sono ora più semplici. Le funzioni personalizzate si trovano all&#39;esterno della base di codice di Commerce, il che impedisce problemi di compatibilità durante l&#39;aggiornamento.
* Lo spostamento di funzioni e logica all’esterno di Commerce libera risorse normalmente utilizzate dai metodi di sviluppo in-process.

## Architettura {#architecture}

Invece di una soluzione preconfigurata, Adobe Developer App Builder fornisce una piattaforma di sviluppo comune, coerente e standardizzata per estendere le soluzioni Adobe Cloud come Adobe Commerce, tra cui:

* Adobe Developer Console utilizzato per lo sviluppo di estensioni e microservizi personalizzati. Crea e gestisci i progetti e accedi a tutti gli strumenti e le API necessari per creare plug-in e integrazioni.
* Strumenti open-source, SDK e librerie per creare estensioni e integrazioni personalizzate. Usa React Spectrum (toolkit dell’interfaccia utente di Adobe) per avere un’unica interfaccia utente comune per tutte le app Adobe.
* servizi come I/O Runtime per l&#39;hosting dell&#39;infrastruttura sulla piattaforma senza server di Adobe ed Eventi di I/O per le integrazioni basate su eventi. Adobe fornisce inoltre supporto predefinito per l’archiviazione di dati e file.
* In Adobe Experience Cloud puoi inviare estensioni e integrazioni da pubblicare nell’organizzazione Experience Cloud. Gli amministratori di sistema possono rivedere, gestire e approvare tali estensioni. Dopo la pubblicazione, le estensioni e gli strumenti personalizzati di App Builder sono disponibili insieme ad altre app Adobe Experience Cloud.

Il diagramma seguente illustra come un’applicazione standard basata su App Builder utilizza queste funzionalità:

![Architettura](/help/assets/app-builder/app-builder-architecture.jpeg)

Per ulteriori dettagli sull&#39;architettura di App Builder, vedere [Panoramica dell&#39;architettura](https://developer.adobe.com/app-builder/docs/guides/){target="_blank"}.

## Introduzione ad App Builder {#additional-resources}

Una panoramica della strategia di commercio componibile, che include la configurazione iniziale, può essere trovato leggendo il seguente post di blog:

[In che modo App Builder contribuisce a promuovere l’agilità aziendale per la piattaforma commerce](https://business.adobe.com/blog/how-to/how-app-builder-helps-you-implement-a-composable-commerce-strategy){target="_blank"}

Per aiutarti a iniziare a utilizzare App Builder, Adobe ha creato la seguente documentazione:

* [Guida introduttiva di App Builder](https://developer.adobe.com/app-builder/docs/getting_started/){target="_blank"}

## Continua l’apprendimento con la documentazione {#appbuilder-documentation}

App Builder fornisce video e documentazione per gli sviluppatori, incluse guide e documentazione di riferimento per aiutarti a sviluppare applicazioni personalizzate:

* [Documentazione di App Builder](https://developer.adobe.com/app-builder/docs/overview/){target="_blank"}
* [Video su App Builder](https://www.youtube.com/playlist?list=PLcVEYUqU7VRfDij-Jbjyw8S8EzW073F_o){target="_blank"}

## Prova una delle applicazioni di esempio {#appbuilder-codesamples}

Sei pronto a iniziare a sviluppare? Il seguente collegamento contiene applicazioni di esempio per iniziare:

* [App Builder Code Labs sul sito web Adobe Developer](https://developer.adobe.com/app-builder/docs/resources/){target="_blank"}

## Supporto {#support}

Per richieste di supporto per sviluppatori, utilizza il [forum Experience League](https://experienceleaguecommunities.adobe.com/t5/app-builder/ct-p/project-firefly?profile.language=it){target="_blank"} per assistenza.

{{$include /help/_includes/app-builder-related-links.md}}
