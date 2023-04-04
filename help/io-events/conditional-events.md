---
title: Scopri come utilizzare gli eventi condizionali in Adobe Commerce
description: Scopri come utilizzare gli eventi condizionali da utilizzare in Adobe Developer App Builder.
landing-page-description: Scopri come utilizzare gli eventi condizionali di Adobe Commerce.
short-description: Learn how to use Adobe Commerce conditional events.
kt: 11890
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-21T00:00:00Z
source-git-commit: d85426bcf3ae0412a433414d70c874964905dda0
workflow-type: tm+mt
source-wordcount: '136'
ht-degree: 0%

---


# Eventi condizionali Adobe Commerce

Scopri gli eventi condizionali in Adobe Commerce che possono essere utilizzati in Adobe Developer App Builder. Documentazione aggiuntiva disponibile all’indirizzo [Installare gli eventi di Adobe I/O per Adobe Commerce](https://developer.adobe.com/commerce/events/get-started/conditional-events/){target="_blank"}.

## A chi serve questo video?

* Sviluppatori non esperti di Adobe Commerce e Adobe Developer App Builder utilizzando eventi I/O e devono creare un progetto Adobe App Builder.

## Contenuto video {#video-content}

* Informazioni sugli eventi condizionali
* Scopri l’utilizzo corretto del nuovo file XML io_events.xml
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
