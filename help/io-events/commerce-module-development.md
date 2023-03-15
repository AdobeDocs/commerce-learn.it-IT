---
title: Scopri come creare un modulo in Adobe Commerce per utilizzare gli eventi.
description: Scopri come creare un modulo Commerce per utilizzare gli eventi.
landing-page-description: Scopri come creare un modulo Adobe Commerce per utilizzare gli eventi.
short-description: Learn how to create an Adobe Commerce module to use events.
kt: 11891
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-21T00:00:00Z
source-git-commit: 67d21ca23cdccc87cdeed4a08a3ebb48e5bd1030
workflow-type: tm+mt
source-wordcount: '162'
ht-degree: 0%

---


# Sviluppo del modulo Adobe Commerce

Scopri come registrare gli eventi, trovare gli eventi supportati e utilizzare un nuovo file XML `io_events.xml` nello sviluppo di moduli personalizzati. Il video mostrerà anche agli sviluppatori come trovare gli eventi registrati che possono essere utilizzati e come annullare l’iscrizione di eventuali eventi già definiti. Documentazione aggiuntiva disponibile all’indirizzo [Installare gli eventi di Adobe I/O per Adobe Commerce](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}.

## A chi serve questo video?

* Sviluppatori non esperti di Adobe Commerce e Adobe Developer App Builder utilizzando eventi I/O.

## Contenuto video {#video-content}

* Registrazione di eventi in Commerce per l’utilizzo in Adobe Developer App Builder
* Identificare gli eventi che possono essere registrati
* Scopri come registrare gli eventi in io_events.xml
* Scopri come registrare gli eventi nelle istanze Commerce `app/etc/config.php`
* Scopri come annullare l’iscrizione a un evento

>[!VIDEO](https://video.tv.adobe.com/v/3415802)

## Comandi utili {#useful-commands}

```bash
bin/magento events:list:all Magento_Catalog

bin/magento events:info plugin.magento.catalog.api.category_repository.save

bin/magento events:subscribe observer.catalog_category_save_after --fields=entity_id --fields=parent_id

cat app/etc/config.php

bin/magento events:unsubscribe observer.catalog_category_save_after

bin/magento events:list
```

{{$include /help/_includes/io-events-related-links.md}}
