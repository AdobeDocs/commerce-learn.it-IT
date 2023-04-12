---
title: Configurare Adobe Commerce
description: Scopri come configurare Adobe Commerce per consentire l’utilizzo degli eventi in Adobe Developer App Builder.
landing-page-description: Scopri come configurare Adobe Commerce per l’utilizzo del meccanismo eventi da parte di Adobe Developer App Builder.
short-description: Scopri come configurare Adobe Commerce per l’utilizzo del meccanismo eventi da parte di Adobe Developer App Builder.
kt: 11889
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-21T00:00:00Z
exl-id: b8062042-2e90-4750-92ef-d55a76f2d842
source-git-commit: edb98cf6544954d741c43beb39f4056326c7d26b
workflow-type: tm+mt
source-wordcount: '150'
ht-degree: 0%

---

# Configurare Adobe Commerce

Scopri come configurare Adobe Commerce per esporre gli eventi. Documentazione aggiuntiva disponibile all’indirizzo [Installare gli eventi di Adobe I/O per Adobe Commerce](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}.

## A chi serve questo video?

* Sviluppatori non esperti di Adobe Commerce e Adobe Developer App Builder utilizzando eventi I/O e devono creare un progetto Adobe App Builder.

## Contenuto video {#video-content}

* Configurazione degli eventi di Adobe I/O nell’amministrazione Commerce
* Salvataggio di una chiave privata nell’amministrazione Commerce
* Salvataggio dell’identificatore univoco nell’amministratore Commerce
* Creare un provider di eventi

>[!VIDEO](https://video.tv.adobe.com/v/3415799?quality=12&learn=on)

## Comandi utili {#useful-commands}

```bash
bin/magento events:create-event-provider --label "my_provider" --description "Provides out-of-process extensibility for Adobe Commerce"

bin/magento events:subscribe observer.catalog_product_save_after --fields=name --fields=price
```

{{$include /help/_includes/io-events-related-links.md}}
