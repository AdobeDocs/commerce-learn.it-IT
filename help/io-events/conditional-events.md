---
title: Scopri come utilizzare gli eventi condizionali in Adobe Commerce
description: Scopri come utilizzare gli eventi condizionali in Adobe Developer App Builder.
landing-page-description: Scopri come utilizzare gli eventi condizionali di Adobe Commerce.
short-description: Scopri come utilizzare gli eventi condizionali di Adobe Commerce.
kt: 11890
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-21T00:00:00Z
feature: App Builder, Eventing, Backend Development
topic: Commerce, Architecture
role: Architect, Developer
level: Beginner, Intermediate
exl-id: 03787aa3-051b-4a35-b2e8-ecf6762b5eb4
source-git-commit: 404d2708a6d540d6fb19a33afb20726356cd8000
workflow-type: tm+mt
source-wordcount: '144'
ht-degree: 0%

---

# Eventi condizionali di Adobe Commerce

Scopri gli eventi condizionali in Adobe Commerce che possono essere utilizzati in Adobe Developer App Builder. È stata trovata ulteriore documentazione all’indirizzo [Installare eventi Adobe I/O per Adobe Commerce](https://developer.adobe.com/commerce/events/get-started/conditional-events/){target="_blank"}.

## A chi serve questo video?

* Sviluppatori senza esperienza di Adobe Commerce e Adobe Developer App Builder che utilizzano eventi di I/O e che devono creare un progetto Adobe di App Builder.

## Contenuto video {#video-content}

* Scopri gli eventi condizionali
* Informazioni sull&#39;utilizzo corretto del nuovo file XML io_events.xml
* Scopri come configurare gli eventi condizionali
* Definizione delle regole da utilizzare negli eventi condizionali
* Scopri come registrare gli eventi nelle istanze Commerce `app/etc/config.php`

>[!VIDEO](https://video.tv.adobe.com/v/3415806?quality=12&learn=on)

## Comandi utili {#useful-commands}

```bash
bin/magento events:subscribe plugin.magento.catalog.model.resource_model.product.save --fields=sku --fields=qty --fields=category_id

bin/magento events:subscribe plugin.magento.catalog.model.resource_model.product.save_low_stock --parent=plugin.magento.catalog.model.resource_model.product.save --fields=sku --fields=qty --fields=category_id --rules="qty|lessThan|20" --rules="category_id|in|3,4,5"

cat app/etc/config.php

bin/magento events:list

bin/magento events:list -v
```

{{$include /help/_includes/io-events-related-links.md}}
