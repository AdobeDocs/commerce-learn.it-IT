---
title: Configurare, distribuire e personalizzare un webhook di acquisizione
description: Scopri come configurare, distribuire e personalizzare un webhook di acquisizione per collegare Adobe Commerce a un sistema di back office di terze parti e gestire la traduzione degli eventi.
doc-type: Technical Video
duration: 697
last-substantial-update: 2024-07-30
feature: Best Practices, Backend Development, Integration
topic: Architecture, Commerce, Development
role: Developer
level: Intermediate
jira: KT-15870
exl-id: f2654873-256e-4c1b-abed-8bfbc4db3fbb
TQID: https://experienceleague.adobe.com/nUXdrsjzeD939jOjZS8ywPV3OeOaxpZCmeuveACtYrY
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: 9568f37b026d0e659e8092282cb923c7ecde58ac
workflow-type: tm+mt
source-wordcount: 412
ht-degree: 0%

---

# Configurare, distribuire e personalizzare un webhook di acquisizione

Scopri come configurare e personalizzare un webhook di acquisizione per integrare Commerce con un sistema di back office di terze parti. &#x200B; questo video spiega come il webhook può risolvere i limiti nella comunicazione degli eventi tra sistemi fornendo un endpoint disponibile al pubblico per adattare i messaggi dal sistema di terze parti all’API Adobe IO Eventing. Il processo prevede la configurazione del webhook nel file `actions.config.yaml`, l&#39;abilitazione nel file `app.config.yaml` e la distribuzione per garantire la funzionalità corretta.

Il video illustra i passaggi per modificare il codice del webhook in modo da tradurre gli eventi di terze parti in formati compatibili con i tipi di evento sottoscritti dell’integrazione. &#x200B; Descrive l&#39;aggiunta di un file `event-mapping.json` per facilitare questa traduzione e sottolinea l&#39;importanza di ridistribuire l&#39;azione di runtime dopo aver apportato modifiche. Il video evidenzia inoltre l&#39;importanza di convalidare e trasformare i payload degli eventi in arrivo in modo che siano allineati allo schema previsto, garantendo la corretta elaborazione e integrazione con l&#39;API di Commerce per la creazione di clienti.

## Pubblico

* Sviluppatori che desiderano configurare un webhook di acquisizione
* Chiunque desideri personalizzare il codice per la traduzione degli eventi
* Sviluppatori e architetti che desiderano comprendere l’importanza dell’autenticazione e della gestione del payload

## Contenuto video

* Configurazione e distribuzione: il video sottolinea l&#39;importanza di configurare il webhook di acquisizione nel file `actions.config.yaml` e di abilitarlo nel file `app.config.yaml`. Evidenzia inoltre la necessità di ridistribuire il progetto dopo aver apportato modifiche per garantire il corretto funzionamento del webhook.
* Personalizzazione per la compatibilità: è fondamentale personalizzare il codice del webhook per tradurre gli eventi di terze parti in formati in linea con i tipi di evento sottoscritti dell’integrazione. &#x200B; Questa personalizzazione garantisce una comunicazione perfetta tra i sistemi e un&#39;elaborazione degli eventi di successo.
* Implementazione dell’autenticazione: le aziende sono responsabili dell’implementazione di meccanismi di autenticazione adatti alle loro esigenze per evitare richieste non autorizzate durante l’utilizzo del webhook di acquisizione. Questo passaggio è essenziale per mantenere la sicurezza e l&#39;integrità dell&#39;integrazione.
* Convalida e trasformazione del payload: la convalida e la trasformazione dei payload degli eventi in arrivo in modo che corrispondano allo schema previsto è fondamentale per la corretta elaborazione e integrazione con l’API di Commerce. &#x200B; Tramite la suddivisione e la mappatura dei campi in modo appropriato, l&#39;integrazione può funzionare in modo efficiente con i dati necessari.

>[!VIDEO](https://video.tv.adobe.com/v/3431694?learn=on)

{{$include /help/_includes/starter-kit-related-links.md}}

## Esempi di codice

* [Webhook di acquisizione personalizzato](https://github.com/adobe/adobe-commerce-samples/tree/main/starter-kit/customize-ingestion-webhook)
* [Aggiungi modulo di pianificazione acquisizione](https://github.com/adobe/adobe-commerce-samples/tree/main/starter-kit/add-ingestion-scheduler)
