---
title: Scopri come installare eventi IO per Adobe Commerce 2.4.6
description: Scopri come installare i moduli necessari per gli eventi IO in Adobe Commerce 2.4.6 da utilizzare in Adobe Developer App Builder
landing-page-description: Scopri come installare i diversi moduli necessari per Adobe Commerce 2.4.6.
short-description: Scopri come installare i diversi moduli necessari per Adobe Commerce 2.4.6.
kt: 11887
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-22T00:00:00Z
badge: Adobe Commerce 2.4.6
exl-id: 41b31ed8-04c5-4d50-aaff-abc3718b5957
source-git-commit: edb98cf6544954d741c43beb39f4056326c7d26b
workflow-type: tm+mt
source-wordcount: '157'
ht-degree: 0%

---

# Installazione di Adobe Commerce 2.4.6

Scopri come installare diversi nuovi moduli in Adobe Commerce utilizzando Composer per la versione 2.4.6. È stata trovata ulteriore documentazione all’indirizzo [Installare eventi Adobe I/O per Adobe Commerce](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}.

## A chi serve questo video?

* Sviluppatori senza esperienza di Adobe Commerce e Adobe Developer App Builder che utilizzano eventi di I/O.

## Contenuto video {#video-content}

* Comandi da eseguire per l’hosting locale
* Comandi da eseguire per Adobe Commerce Cloud
* Adobe Commerce Cloud yaml modifica richiesta

>[!VIDEO](https://video.tv.adobe.com/v/3415795?quality=12&learn=on)

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
