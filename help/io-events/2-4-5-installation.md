---
title: Scopri come installare eventi IO per Adobe Commerce 2.4.5
description: Scopri come installare i moduli necessari per gli eventi IO in Adobe Commerce 2.4.5 per l’utilizzo in Adobe Developer App Builder
landing-page-description: Scopri come installare diversi moduli necessari per Adobe Commerce 2.4.5 utilizzando il compositore.
short-description: Learn how to install several modules needed for Adobe Commerce 2.4.5 using composer.
kt: 11886
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-22T00:00:00Z
badge: "Adobe Commerce 2.4.5"
source-git-commit: 67d21ca23cdccc87cdeed4a08a3ebb48e5bd1030
workflow-type: tm+mt
source-wordcount: '169'
ht-degree: 0%

---


# Installazione di Adobe Commerce 2.4.5

Scopri come installare diversi nuovi moduli in Adobe Commerce utilizzando Composer per la versione 2.4.5. Questo imposta i moduli richiesti da utilizzare nell’applicazione Adobe Commerce. Documentazione aggiuntiva disponibile all’indirizzo [Installare gli eventi di Adobe I/O per Adobe Commerce](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}.

## A chi serve questo video?

* Sviluppatori nuovi per Adobe Commerce e Adobe Developer App Builder utilizzando gli eventi I/O

## Contenuto video {#video-content}

* Installazione dei moduli richiesti utilizzando il compositore
* Comandi da eseguire per l&#39;hosting locale
* Comandi da eseguire per Adobe Commerce Cloud
* Modifica richiesta stile Adobe Commerce Cloud

>[!VIDEO](https://video.tv.adobe.com/v/3415794)

## Comandi utili {#useful-commands}

Esistono diversi comandi che differiscono leggermente, a seconda che ti trovi in un ambiente self-hosting o che utilizzi Adobe Commerce Cloud.

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
