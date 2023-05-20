---
title: Scopri come creare un modulo in Adobe Commerce per utilizzare gli eventi.
description: Scopri come creare il modulo Commerce per utilizzare gli eventi.
landing-page-description: Scopri come creare un modulo Adobe Commerce per utilizzare gli eventi.
short-description: Scopri come creare un modulo Adobe Commerce per utilizzare gli eventi.
kt: 11891
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-21T00:00:00Z
exl-id: e8103fe0-116a-499c-ae0a-3ad0511f44d0
source-git-commit: edb98cf6544954d741c43beb39f4056326c7d26b
workflow-type: tm+mt
source-wordcount: '173'
ht-degree: 0%

---

# Sviluppo di moduli Adobe Commerce

Scopri come registrare eventi, trovare eventi supportati e utilizzare un nuovo file XML `io_events.xml` nello sviluppo di moduli personalizzati. Il video mostrerà inoltre agli sviluppatori come trovare eventi registrati che possono essere utilizzati e come annullare l’abbonamento a eventuali eventi già definiti. È stata trovata ulteriore documentazione all’indirizzo [Installare eventi Adobe I/O per Adobe Commerce](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}.

## A chi serve questo video?

* Sviluppatori senza esperienza di Adobe Commerce e Adobe Developer App Builder che utilizzano eventi di I/O.

## Contenuto video {#video-content}

* Registrazione di eventi in Commerce per l’utilizzo in Adobe Developer App Builder
* Identificare gli eventi che possono essere registrati
* Scopri come registrare gli eventi in io_events.xml
* Scopri come registrare gli eventi nelle istanze Commerce `app/etc/config.php`
* Scopri come annullare l’abbonamento a un evento

>[!VIDEO](https://video.tv.adobe.com/v/3415802?quality=12&learn=on)

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
