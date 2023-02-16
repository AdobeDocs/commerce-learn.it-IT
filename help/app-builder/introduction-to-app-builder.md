---
title: Estensibilità fuori processo per Adobe Commerce
description: Scopri come Adobe App Builder e perché è un aspetto importante dell’estensibilità fuori processo.
landing-page-description: Scopri cos’è App Builder e come può essere utile con le strategie di sviluppo di Adobe Commerce.
kt: 11433
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-16T00:00:00Z
source-git-commit: f4c092b4534587f5656bbf298dbf94f783d93be7
workflow-type: tm+mt
source-wordcount: '746'
ht-degree: 0%

---


# Introduzione ad App Builder

Lo sviluppo Adobe Commerce ha utilizzato l’estensibilità all’interno del processo. Il modello in-process richiede che qualsiasi nuovo codice sia compatibile con gli aggiornamenti, la versione PHP del server e molti altri servizi e applicazioni server essenziali utilizzati da Commerce. Adobe Developer App Builder utilizza l’estensibilità fuori processo per evitare questi problemi di compatibilità.

## Generatore di app per Adobe Commerce {#project-firefly}

>[!VIDEO](https://video.tv.adobe.com/v/3412839)

Adobe Developer App Builder è una piattaforma di estensibilità senza server per l’integrazione e la creazione di esperienze personalizzate per estendere le soluzioni Adobe ed è ora disponibile per Adobe Commerce. Con App Builder, puoi creare app sicure e scalabili che estendono le funzionalità native di Commerce e si integrano con soluzioni di terze parti. In qualità di sviluppatore, ora puoi sfruttare l’estensibilità fuori processo con Adobe Commerce e questo a sua volta offre vantaggi immediati e a lungo termine.

App Builder fornisce un framework di estensibilità unificato di terze parti per l’integrazione e la creazione di applicazioni personalizzate che si estendono [!DNL Adobe Commerce]. Poiché questo framework di estensibilità è basato sull’infrastruttura di Adobe, gli sviluppatori possono creare microservizi personalizzati, estendere e integrare [!DNL Adobe Commerce] tra altre soluzioni Adobe e integrazioni di terze parti.

App Builder offre ai clienti un modo per estendere [!DNL Adobe Commerce] in vari casi d&#39;uso:

* estensibilità middleware: connette i sistemi esterni con le applicazioni Adobe creando connettori personalizzati o sfrutta una suite di integrazioni predefinite.
* estensibilità dei servizi di base : estende le funzionalità delle applicazioni di base estendendo il comportamento predefinito con funzioni personalizzate e logica di business.
* estensibilità dell’esperienza utente : estende l’esperienza di base per supportare i requisiti aziendali o creare proprietà digitali, vetrine e applicazioni back-office specifiche per i clienti.

App Builder (noto in precedenza come Project Firefly) è una soluzione basata su cloud, il che significa che si ridimensiona automaticamente. Questo servizio viene anche distribuito a livello globale per consentire le migliori prestazioni indipendentemente dalla posizione geografica.

## Perché dovresti saperne di più su App Builder

Poiché Adobe Commerce non è un prodotto SAAS completo, il codice sviluppato può aggiungere complessità e problemi di aggiornamento. Utilizzando l’estensibilità out-of-process, ad esempio App Builder, puoi fornire funzionalità personalizzate e univoche al tuo archivio Adobe Commerce senza richiedere metodi in-process.

Altri vantaggi includono:

* Le funzioni disaccoppiate consentono un rapido avvio.
* Gli aggiornamenti ora sono più facili. Le funzioni personalizzate non sono incluse nella base di codice Commerce, il che impedisce problemi di compatibilità durante l’aggiornamento.
* Lo spostamento di funzioni e logiche al di fuori di Commerce consente di liberare risorse normalmente utilizzate dai metodi di sviluppo in-process.

## Architettura {#architecture}

Invece di una soluzione standard, Adobe Developer App Builder fornisce una piattaforma di sviluppo comune, coerente e standardizzata per l’estensione delle soluzioni Adobe Cloud, come Adobe Commerce, che include:

* Console Adobe Developer utilizzata per lo sviluppo di microservizi personalizzati e estensioni. Crea e gestisci progetti accedendo a tutti gli strumenti e le API necessari per creare plug-in e integrazioni.
* Strumenti open-source, SDK e librerie per creare estensioni e integrazioni personalizzate. Utilizza React Spectrum (toolkit per l’interfaccia utente di Adobe) per avere un’interfaccia utente comune per tutte le app di Adobe.
* servizi come I/O Runtime per l&#39;hosting di infrastrutture su piattaforma senza server Adobe ed eventi I/O per integrazioni basate su eventi. Adobe fornisce inoltre supporto integrato per l’archiviazione di dati e file.
* Adobe Experience Cloud in cui invii estensioni e integrazioni da pubblicare nell’organizzazione Experience Cloud. Gli amministratori di sistema possono rivedere, gestire e approvare queste estensioni. Dopo la pubblicazione, le estensioni e gli strumenti personalizzati di App Builder sono disponibili insieme ad altre app Adobe Experience Cloud.

Il diagramma seguente illustra come un’applicazione standard integrata in App Builder utilizza queste funzionalità:

![Architettura](/help/assets/app-builder/firefly-architecture.jpeg)

Per ulteriori dettagli sull’architettura di App Builder, consulta la sezione [Panoramica dell’architettura](https://developer.adobe.com/app-builder/docs/guides/).

## Estensione Amazon Sales Channel {#amazon-sales-channel-extension}

Le seguenti esercitazioni mostrano come collegare Adobe Commerce ad Amazon Sales Channel utilizzando un’estensione App Builder.

* [panoramica tecnica di App Builder](../app-builder/app-builder-technical-overview.md)
* [quadro di estensibilità](../app-builder/extensibility-framework-commerce-eventing.md)
* [dimostrazione funzionale di App Builder](../app-builder/app-builder-functional-demonstration.md)

## Guida introduttiva ad App Builder {#additional-resources}

Per aiutarti a iniziare con App Builder, Adobe ha creato la seguente documentazione:

* [Guida introduttiva di App Builder](https://developer.adobe.com/app-builder/docs/getting_started/)

## Continua a imparare con la documentazione {#appbuilder-documentation}

App Builder fornisce video e documentazione per gli sviluppatori, incluse guide e documentazione di riferimento per sviluppare applicazioni personalizzate:

* [Documentazione di App Builder](https://developer.adobe.com/app-builder/docs/overview/)
* [Video di App Builder](https://www.youtube.com/playlist?list=PLcVEYUqU7VRfDij-Jbjyw8S8EzW073F_o)

## Provare una delle applicazioni di esempio {#appbuilder-codesamples}

Pronti per iniziare lo sviluppo? Il seguente collegamento contiene applicazioni di esempio per aiutarti a iniziare:

* [Labs di codice di App Builder sul sito web Adobe Developer](https://developer.adobe.com/app-builder/docs/resources/)

## Supporto {#support}

Per le richieste di supporto per sviluppatori, utilizza il [Forum Experience League](https://experienceleaguecommunities.adobe.com/t5/app-builder/ct-p/project-firefly) per assistenza.

{{$include /help/_includes/app-builder-related-links.md}}
