---
title: Configurare Adobe Commerce
description: Scopri come configurare Adobe Commerce per consentire l’utilizzo degli eventi in Adobe Developer App Builder.
jira: KT-11889
doc-type: Tutorial
duration: 268
last-substantial-update: 2023-02-21
feature: App Builder, Configuration, Backend Development
topic: Commerce, Architecture
role: Developer, User
level: Beginner, Intermediate
exl-id: b8062042-2e90-4750-92ef-d55a76f2d842
TQID: https://experienceleague.adobe.com/6FBcDMDst5LBS7EyqA8zINr2Vs3bC--M7CXzIKf6EeQ
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
source-git-commit: 9f50b87d13f48b239d814783eb2c56319946cb29
workflow-type: tm+mt
source-wordcount: 114
ht-degree: 0%

---

# Configurare Adobe Commerce

Scopri come configurare Adobe Commerce per esporre gli eventi. È stata trovata ulteriore documentazione in [Installa Adobe I/O Events per Adobe Commerce](https://developer.adobe.com/commerce/extensibility/events/installation){target="_blank"}.

## A chi serve questo video?

* Sviluppatori senza esperienza di Adobe Commerce e Adobe Developer App Builder che utilizzano eventi di I/O e che devono creare un progetto Adobe App Builder.

## Contenuto video {#video-content}

* Configurazione del Adobe I/O Events nell’amministratore Commerce
* Salvataggio di una chiave privata nell’amministratore di Commerce
* Salvataggio dell’identificatore univoco nell’amministratore di Commerce
* Creare un provider di eventi

>[!VIDEO](https://video.tv.adobe.com/v/3419714?captions=ita&learn=on)

## Comandi utili {#useful-commands}

```bash
bin/magento events:create-event-provider --label "my_provider" --description "Provides out-of-process extensibility for Adobe Commerce"

bin/magento events:subscribe observer.catalog_product_save_after --fields=name --fields=price
```

{{$include /help/_includes/io-events-related-links.md}}

