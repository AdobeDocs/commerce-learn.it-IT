---
title: Scopri come installare eventi IO per Adobe Commerce 2.4.6
description: Scopri come installare i moduli necessari per gli eventi IO in Adobe Commerce 2.4.6 da utilizzare in Adobe Developer App Builder
landing-page-description: Scopri come installare i diversi moduli necessari per Adobe Commerce 2.4.6.
short-description: Scopri come installare i diversi moduli necessari per Adobe Commerce 2.4.6.
kt: 11887
doc-type: tutorial
duration: 167
audience: all
last-substantial-update: 2023-02-22T00:00:00.000Z
badge: Adobe Commerce 2.4.6
feature: App Builder, Eventing
topic: Commerce, Architecture
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
exl-id: 41b31ed8-04c5-4d50-aaff-abc3718b5957
TQID: https://experienceleague.adobe.com/19IVX54xo-RAJAOuXNIABdy11ExmBRAbWugi4eZ0cF0
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2: id: b5a62a22-46f7-4f0d-b151-3fc640bef588id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
source-git-commit: 456f3cae8c45d137a195456692c2d11204126bb7
workflow-type: tm+mt
source-wordcount: 166
ht-degree: 0%

---

# Installazione di Adobe Commerce 2.4.6

Scopri come installare diversi nuovi moduli in Adobe Commerce utilizzando Composer per la versione 2.4.6. È stata trovata ulteriore documentazione in [Installa Adobe I/O Events per Adobe Commerce](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}.

## A chi serve questo video?

* Sviluppatori senza esperienza di Adobe Commerce e Adobe Developer App Builder che utilizzano eventi di I/O.

## Contenuto video {#video-content}

* Comandi da eseguire per l’hosting locale
* Comandi da eseguire per Adobe Commerce Cloud
* Modifica richiesta dello yaml Adobe Commerce Cloud

>[!VIDEO](https://video.tv.adobe.com/v/3415795?learn=on)

## Comandi utili {#useful-commands}

Esistono vari comandi che differiscono leggermente, a seconda che ti trovi in un ambiente con hosting autonomo o utilizzi Adobe Commerce Cloud.

### Hosting locale {#on-premise}

```bash
bin/magento events:generate:module

bin/magento module:enable --all

bin/magento setup:upgrade && bin/magento setup:di:compile
```

### Adobe Commerce su Cloud {#adobe-commerce-cloud}

```bash
composer info magento/ece-tools
```

Commerce Cloud `.magento.env.yaml`:

```yaml
stage:
  global:
    ENABLE_EVENTING: true
```

{{$include /help/_includes/io-events-related-links.md}}

