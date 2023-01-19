---
title: Estensibilità fuori processo per Adobe Commerce
description: Scopri come Adobe App Builder e perché è un aspetto importante dell’estensibilità fuori processo.
landing-page-description: Scopri cos’è app builder e come può essere utile con le strategie di sviluppo di Adobe Commerce.
kt: 11433
doc-type: tutorial
audience: all
last-substantial-update: 2023-01-11T00:00:00Z
source-git-commit: ef0fa95e776b97ddbaf30e0acd1340e30f12738f
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---


# Estensibilità fuori processo

Lo sviluppo di Adobe Commerce è stato eseguito storicamente utilizzando lo stesso archivio dell’applicazione principale.  Questo viene chiamato in-process.  Questa tecnica è molto valida e offre allo sviluppatore un meccanismo previsto per l’estensione dell’applicazione.  Tuttavia, questo ha un prezzo.  Ogni volta che aggiungi un nuovo codice alla base di codice, questo deve essere compatibile con qualsiasi aggiornamento.  Devi anche essere compatibile con la versione PHP dei server e con molte altre applicazioni e servizi server che il commercio utilizzerà.  Adobe Developer App Builder richiede la stessa estensione della funzionalità ma la sposta fuori dal sito.  Il codice e la logica sono completamente esterni e questo metodo è indicato come fuori processo.

## Generatore di app per Adobe Commerce {#project-firefly}

>[!VIDEO](https://video.tv.adobe.com/v/3412839)

Adobe Developer App Builder fornisce un framework di estensibilità per gli sviluppatori al fine di estendere [!DNL Adobe Commerce] per fornire estensibilità fuori processo.

App Builder fornisce un framework di estensibilità unificato di terze parti per l’integrazione e la creazione di applicazioni personalizzate che si estendono [!DNL Adobe Commerce]. Poiché questo framework di estensibilità è basato sull’infrastruttura di Adobe, gli sviluppatori possono creare microservizi personalizzati, nonché estendere e integrare [!DNL Adobe Commerce] tra soluzioni Adobe e altre integrazioni di terze parti.

App Builder offre ai clienti un modo per estendere [!DNL Adobe Commerce] in vari casi d&#39;uso:

* estensibilità middleware: connette i sistemi esterni con le applicazioni Adobe creando connettori personalizzati o sfrutta una suite di integrazioni predefinite.
* estensibilità dei servizi di base : estende le funzionalità delle applicazioni di base estendendo il comportamento predefinito con funzioni personalizzate e logica di business.
* estensibilità dell’esperienza utente : estende l’esperienza di base per supportare i requisiti aziendali o creare proprietà digitali, vetrine e applicazioni back-office specifiche per i clienti.

App Builder (noto in precedenza come Project Firefly) è una soluzione basata su cloud, il che significa che si ridimensiona automaticamente. Questo servizio viene anche distribuito a livello globale per consentire le migliori prestazioni indipendentemente dalla posizione geografica.

## Perché dovresti saperne di più su App Builder

Poiché Adobe Commerce non è un SAAS completo, il codice sviluppato o installato può aggiungere complessità e problemi di aggiornamento. Utilizzando l’estensibilità out-of-process, ad esempio App Builder, puoi fornire funzionalità personalizzate e univoche al tuo archivio Adobe Commerce senza richiedere metodi in-process.

Altri vantaggi includono:

* Le funzioni disaccoppiate consentono un rapido avvio.
* Gli aggiornamenti ora sono più facili. Le funzioni personalizzate non si trovano nella base di codice di e-commerce, che evita problemi di compatibilità durante l’aggiornamento.
* Lo spostamento di funzioni e logiche al di fuori dell’e-commerce libera risorse normalmente utilizzate dai metodi di sviluppo in-process.

## Architettura {#architecture}

Invece di una soluzione standard, Adobe Developer App Builder fornisce una piattaforma di sviluppo comune, coerente e standardizzata per l’estensione delle soluzioni Adobe Cloud, come Adobe Commerce, che include:

* Console Adobe Developer utilizzata per lo sviluppo di microservizi personalizzati e estensioni. Crea e gestisci progetti accedendo a tutti gli strumenti e le API necessari per creare plug-in e integrazioni.
* Strumenti open-source, SDK e librerie per creare estensioni e integrazioni personalizzate. Utilizza React Spectrum (toolkit per l’interfaccia utente di Adobe) per avere un’interfaccia utente comune per tutte le app di Adobe.
* servizi come I/O Runtime per l&#39;hosting di infrastrutture su piattaforma senza server Adobe ed eventi I/O per integrazioni basate su eventi. Adobe fornisce inoltre supporto integrato per l’archiviazione di dati e file.
* Adobe Experience Cloud in cui invii estensioni e integrazioni da pubblicare nell’organizzazione Experience Cloud. Gli amministratori di sistema possono rivedere, gestire e approvare queste estensioni. Dopo la pubblicazione, le estensioni e gli strumenti personalizzati di App Builder sono disponibili insieme ad altre app Adobe Experience Cloud.

Il diagramma seguente illustra come un’applicazione standard integrata in App Builder utilizza queste funzionalità:

![Architettura](/help/assets/app-builder/firefly-architecture.jpeg)

Per ulteriori dettagli sull’architettura di App Builder, consulta la sezione [Panoramica dell’architettura](https://developer.adobe.com/app-builder/docs/guides/).

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

