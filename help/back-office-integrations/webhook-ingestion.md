---
title: Configurazione, distribuzione e personalizzazione di un webhook di acquisizione per l’integrazione di Commerce con un sistema di terze parti
description: Scopri come impostare e personalizzare un webhook di acquisizione per facilitare la comunicazione tra Commerce e un sistema di back office di terze parti.
landing-page-description: Scopri come utilizzare Commerce Integration Starter Kit per integrare Commerce con un sistema di back office di terze parti utilizzando un webhook di acquisizione.
kt: 15870
doc-type: video
duration: 593
audience: all
last-substantial-update: 2024-7-30
feature: Best Practices, Backend Development, Integration
topic: Architecture, Commerce, Development
role: Architect, Developer
level: Intermediate
source-git-commit: aed143b96f13a413f85fc461e11f358b4c657015
workflow-type: tm+mt
source-wordcount: '413'
ht-degree: 0%

---

# Configurazione, distribuzione e personalizzazione di un webhook di acquisizione

Scopri come configurare e personalizzare un webhook di acquisizione per l’integrazione di Commerce con un sistema di back office di terze parti&#x200B;. Questo video spiega come il webhook può risolvere i limiti nella comunicazione degli eventi tra sistemi fornendo un endpoint disponibile al pubblico per adattare i messaggi dal sistema di terze parti all&#39;API dell&#39;evento IO di Adobe. Il processo prevede la configurazione del webhook nel file `actions.config.yaml`, l&#39;abilitazione nel file `app.config.yaml` e la distribuzione per garantire la funzionalità corretta.

Il video illustra i passaggi per modificare il codice del webhook in modo da tradurre gli eventi di terze parti in formati compatibili con i tipi di evento sottoscritti dell’integrazione. Descrive l&#39;aggiunta di un file `event-mapping.json` per facilitare questa traduzione e sottolinea l&#39;importanza di ridistribuire l&#39;azione di runtime dopo aver apportato modifiche&#x200B; Il video evidenzia inoltre l’importanza di convalidare e trasformare i payload degli eventi in arrivo in modo che siano allineati allo schema previsto, garantendo la corretta elaborazione e integrazione con l’API di Commerce per la creazione dei clienti.

## Pubblico

* Sviluppatori che desiderano configurare un webhook di acquisizione
* Chiunque desideri personalizzare il codice per la traduzione degli eventi
* Sviluppatori e architetti che desiderano comprendere l’importanza dell’autenticazione e della gestione del payload

## Contenuto video

* Configurazione e distribuzione: il video sottolinea l&#39;importanza di configurare il webhook di acquisizione nel file `actions.config.yaml` e di abilitarlo nel file `app.config.yaml`. Evidenzia inoltre la necessità di ridistribuire il progetto dopo aver apportato modifiche per garantire il corretto funzionamento del webhook.
* Personalizzazione per la compatibilità: è fondamentale personalizzare il codice del webhook per tradurre gli eventi di terze parti in formati in linea con i tipi di evento sottoscritti dell’integrazione. &#x200B; Questa personalizzazione garantisce una comunicazione fluida tra i sistemi e un&#39;elaborazione degli eventi di successo.
* Implementazione dell’autenticazione: le aziende sono responsabili dell’implementazione di meccanismi di autenticazione adatti alle loro esigenze per evitare richieste non autorizzate durante l’utilizzo del webhook di acquisizione. Questo passaggio è essenziale per mantenere la sicurezza e l&#39;integrità dell&#39;integrazione.
* Convalida e trasformazione del payload: la convalida e la trasformazione dei payload degli eventi in arrivo in modo che corrispondano allo schema previsto è fondamentale per la corretta elaborazione e integrazione con l’API di Commerce. &#x200B; Tagliando e mappando i campi in modo appropriato, l’integrazione può funzionare in modo efficiente con i dati necessari.

>[!VIDEO](https://video.tv.adobe.com/v/3431694?learn=on)

{{$include /help/_includes/starter-kit-related-links.md}}

## Esempi di codice

* [Webhook di acquisizione personalizzato](https://github.com/adobe/adobe-commerce-samples/tree/main/starter-kit/customize-ingestion-webhook)
* [Aggiungi pianificazione acquisizione](https://github.com/adobe/adobe-commerce-samples/tree/main/starter-kit/add-ingestion-scheduler)
