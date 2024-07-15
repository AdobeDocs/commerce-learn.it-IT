---
title: Scopri come installare eventi IO per Adobe Commerce 2.4.5
description: Scopri come installare i moduli necessari per gli eventi IO in Adobe Commerce 2.4.5 da utilizzare in Adobe Developer App Builder
landing-page-description: Scopri come installare i diversi moduli necessari per Adobe Commerce 2.4.5 utilizzando Compositore.
short-description: Scopri come installare i diversi moduli necessari per Adobe Commerce 2.4.5 utilizzando Compositore.
kt: 11886
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-22T00:00:00Z
badge: Adobe Commerce 2.4.5
feature: App Builder, Eventing
topic: Commerce, Architecture
role: Architect, Developer
level: Beginner, Intermediate
exl-id: e0adfd85-5a3d-44ba-aab5-ecd7c61715cf
source-git-commit: 404d2708a6d540d6fb19a33afb20726356cd8000
workflow-type: tm+mt
source-wordcount: '190'
ht-degree: 0%

---

# Installazione di Adobe Commerce 2.4.5

Scopri come installare diversi nuovi moduli in Adobe Commerce utilizzando Composer per la versione 2.4.5. In questo modo vengono impostati i moduli richiesti da utilizzare nell’applicazione Adobe Commerce. È stata trovata ulteriore documentazione in [Installa eventi di Adobe I/O per Adobe Commerce](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}.

## A chi serve questo video?

* Sviluppatori senza esperienza di Adobe Commerce e Adobe Developer App Builder con eventi di I/O

## Contenuto video {#video-content}

* Installazione dei moduli richiesti tramite Compositore
* Comandi da eseguire per l’hosting locale
* Comandi da eseguire per Adobe Commerce Cloud
* Adobe Commerce Cloud yaml modifica richiesta

>[!VIDEO](https://video.tv.adobe.com/v/3415794?quality=12&learn=on)

## Comandi utili {#useful-commands}

Esistono vari comandi che differiscono leggermente, a seconda che ti trovi in un ambiente con hosting autonomo o utilizzi Adobe Commerce Cloud.

### Hosting locale {#on-premise}

```bash
composer require magento/commerce-eventing=^1.0 --no-update

composer update

bin/magento events:generate:module

bin/magento module:enable --all

bin/magento setup:upgrade && bin/magento setup:di:compile
```

### Adobe Commerce su Cloud {#adobe-commerce-cloud}

```bash
composer require magento/commerce-eventing=^1.0 --no-update

composer update

composer info magento/ece-tools
```

Commerce Cloud `.magento.env.yaml`:

```yaml
stage:
  global:
    ENABLE_EVENTING: true
```

{{$include /help/_includes/io-events-related-links.md}}
