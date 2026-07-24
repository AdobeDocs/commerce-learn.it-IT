---
title: Creare un modulo Adobe Commerce per utilizzare gli eventi I/O
description: Scopri come creare il modulo Commerce per utilizzare gli eventi.
jira: KT-11891
doc-type: Tutorial
duration: 314
last-substantial-update: 2023-02-21
feature: App Builder, Eventing, Backend Development
topic: Commerce, Architecture
role: Developer
level: Beginner, Intermediate
exl-id: e8103fe0-116a-499c-ae0a-3ad0511f44d0
TQID: https://experienceleague.adobe.com/bRnOh6fnsyTY-21f81vIXV4-eeitLXQWsAbjg2rx-Is
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
source-git-commit: 282072f1e29b836d19dee2e1b6498f75150fe3a5
workflow-type: tm+mt
source-wordcount: 140
ht-degree: 0%

---

# Sviluppo di moduli Adobe Commerce

Scopri come registrare eventi, trovare eventi supportati e utilizzare un nuovo file XML `io_events.xml` nello sviluppo di moduli personalizzati. Il video mostra inoltre agli sviluppatori come trovare gli eventi registrati da utilizzare e come rimuovere gli eventi già definiti. È stata trovata ulteriore documentazione in [Installa Adobe I/O Events per Adobe Commerce](https://developer.adobe.com/commerce/extensibility/events/installation){target="_blank"}.

## A chi serve questo video?

* Sviluppatori senza esperienza di Adobe Commerce e Adobe Developer App Builder che utilizzano eventi di I/O.

## Contenuto video {#video-content}

* Registrazione di eventi Commerce per Adobe Developer App Builder
* Identificare gli eventi che possono essere registrati
* Scopri come registrare gli eventi in io_events.xml
* Scopri come registrare gli eventi nelle istanze di Commerce `app/etc/config.php`
* Scopri come annullare l’abbonamento a un evento

>[!VIDEO](https://video.tv.adobe.com/v/3419836?captions=ita&learn=on)

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


