---
title: Estensibilità out-of-process per Adobe Commerce
description: Scopri Adobe App Builder e perché è un aspetto importante dell’estensibilità out-of-process.
landing-page-description: Scopri cos’è App Builder e come può essere utile con le strategie di sviluppo di Adobe Commerce.
kt: 11433
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-16T00:00:00Z
source-git-commit: 82ccecf2789e1eedf447af2705a3840d0302fdba
workflow-type: tm+mt
source-wordcount: '819'
ht-degree: 0%

---


# Introduzione ad App Builder

In passato, lo sviluppo Adobe Commerce utilizzava l’estensibilità in-process. Il modello in-process richiede che qualsiasi nuovo codice sia compatibile con gli aggiornamenti, la versione PHP del server e molte altre applicazioni e servizi server essenziali utilizzati da Commerce. Adobe Developer App Builder utilizza l’estensibilità fuori processo per evitare questi problemi di compatibilità.

## App Builder per Adobe Commerce {#project-firefly}

>[!VIDEO](https://video.tv.adobe.com/v/3412839)

Adobe Developer App Builder è una piattaforma di estensibilità senza server per l’integrazione e la creazione di esperienze personalizzate al fine di estendere le soluzioni Adobe ed è ora disponibile per Adobe Commerce. Con App Builder puoi creare app sicure e scalabili che estendono le funzionalità native di Commerce e si integrano con soluzioni di terze parti. In qualità di sviluppatore, ora puoi sfruttare l’estensibilità fuori processo con Adobe Commerce, il che a sua volta offre vantaggi immediati e a lungo termine.

App Builder fornisce un framework di estensibilità unificato di terze parti per l’integrazione e la creazione di applicazioni personalizzate che estendono [!DNL Adobe Commerce]. Poiché questo framework di estensibilità è basato sull’infrastruttura di Adobe, gli sviluppatori possono creare microservizi personalizzati ed estendere e integrare [!DNL Adobe Commerce] in altre soluzioni Adobe e integrazioni di terze parti.

App Builder consente ai clienti di estendere [!DNL Adobe Commerce] in vari casi d’uso:

* estensibilità del middleware: possibilità di collegare i sistemi esterni alle applicazioni Adobe creando connettori personalizzati o sfruttando una suite di integrazioni predefinite.
* estensibilità dei servizi di base: estende le funzionalità principali dell’applicazione estendendo il comportamento predefinito con funzioni personalizzate e logica di business.
* estensibilità dell’esperienza utente: estendere l’esperienza di base per supportare i requisiti aziendali o creare proprietà digitali, vetrine e applicazioni di back office specifiche per il cliente.

App Builder (precedentemente noto come Project Firefly) è una soluzione basata su cloud, il che significa che si adatta automaticamente. Questo servizio è inoltre distribuito a livello globale per garantire le migliori prestazioni indipendentemente dalla posizione geografica.

## Perché dovresti saperne di più su App Builder

Poiché Adobe Commerce non è un prodotto completamente SAAS, il codice sviluppato può aggiungere complessità e problemi di aggiornamento. Utilizzando l’estensibilità out-of-process, come App Builder, puoi fornire funzionalità personalizzate e univoche all’archivio Adobe Commerce senza richiedere metodi in-process.

Altri vantaggi comprendono:

* Le funzioni disaccoppiate consentono un avvio più rapido.
* Gli aggiornamenti sono ora più semplici. Le funzioni personalizzate si trovano all’esterno della base di codice di Commerce, il che impedisce problemi di compatibilità durante l’aggiornamento.
* Lo spostamento di funzioni e logica al di fuori di Commerce libera risorse normalmente utilizzate dai metodi di sviluppo in-process.

## Architettura {#architecture}

Invece di una soluzione preconfigurata, Adobe Developer App Builder fornisce una piattaforma di sviluppo comune, coerente e standardizzata per l’estensione di soluzioni Adobe Cloud come Adobe Commerce, tra cui:

* Console Adobe Developer utilizzata per lo sviluppo di estensioni e microservizi personalizzati. Crea e gestisci i progetti e accedi a tutti gli strumenti e le API necessari per creare plug-in e integrazioni.
* Strumenti open-source, SDK e librerie per creare estensioni e integrazioni personalizzate. Utilizza React Spectrum (toolkit dell’interfaccia utente di Adobe) per avere un’unica interfaccia utente comune per tutte le app di Adobe.
* servizi come I/O Runtime per l&#39;hosting dell&#39;infrastruttura sulla piattaforma senza server di Adobe ed Eventi di I/O per le integrazioni basate su eventi. Adobe fornisce inoltre supporto predefinito per l’archiviazione di dati e file.
* In Adobe Experience Cloud puoi inviare estensioni e integrazioni da pubblicare nell’organizzazione di Experience Cloud. Gli amministratori di sistema possono rivedere, gestire e approvare tali estensioni. Dopo la pubblicazione, le estensioni e gli strumenti personalizzati di App Builder sono disponibili insieme ad altre app Adobe Experience Cloud.

Il diagramma seguente illustra come un’applicazione standard basata su App Builder utilizza queste funzionalità:

![Architettura](/help/assets/app-builder/firefly-architecture.jpeg)

Per ulteriori dettagli sull&#39;architettura di App Builder, vedi [Panoramica dell’architettura](https://developer.adobe.com/app-builder/docs/guides/){target="_blank"}.

## Estensione Sales Channel Amazon {#amazon-sales-channel-extension}

>[!IMPORTANT]
>
>L’estensione del Sales Channel Amazon è ancora in fase di sviluppo e non è stata rilasciata ufficialmente.  Questi video e tutorial hanno lo scopo di mostrare come utilizzare Adobe Developer App Builder per un caso d’uso pratico.

I seguenti tutorial mostrano come collegare Adobe Commerce al Sales Channel Amazon utilizzando un’estensione App Builder.

* [panoramica tecnica App Builder](../app-builder/app-builder-technical-overview.md)
* [framework di estensibilità](../app-builder/extensibility-framework-commerce-eventing.md)
* [dimostrazione funzionale App Builder](../app-builder/app-builder-functional-demonstration.md)

## Introduzione ad App Builder {#additional-resources}

Una panoramica della strategia di commercio componibile, che include la configurazione iniziale, può essere trovato leggendo il seguente post di blog:

[In che modo App Builder aiuta a stimolare l’agilità aziendale per la piattaforma commerce](https://business.adobe.com/blog/how-to/how-app-builder-helps-you-implement-a-composable-commerce-strategy){target="_blank"}

Per aiutarti a iniziare a utilizzare App Builder, Adobe ha creato la seguente documentazione:

* [Guida introduttiva di App Builder](https://developer.adobe.com/app-builder/docs/getting_started/){target="_blank"}

## Continua l’apprendimento con la documentazione {#appbuilder-documentation}

App Builder fornisce video e documentazione per gli sviluppatori, incluse guide e documentazione di riferimento per aiutarti a sviluppare applicazioni personalizzate:

* [Documentazione di App Builder](https://developer.adobe.com/app-builder/docs/overview/){target="_blank"}
* [Video di App Builder](https://www.youtube.com/playlist?list=PLcVEYUqU7VRfDij-Jbjyw8S8EzW073F_o){target="_blank"}

## Prova una delle applicazioni di esempio {#appbuilder-codesamples}

Sei pronto a iniziare a sviluppare? Il seguente collegamento contiene applicazioni di esempio per iniziare:

* [Laboratori di codice di App Builder sul sito web di Adobe Developer](https://developer.adobe.com/app-builder/docs/resources/){target="_blank"}

## Supporto {#support}

Per le richieste di supporto per gli sviluppatori, utilizza [Forum Experience League](https://experienceleaguecommunities.adobe.com/t5/app-builder/ct-p/project-firefly){target="_blank"} per assistenza.

{{$include /help/_includes/app-builder-related-links.md}}
