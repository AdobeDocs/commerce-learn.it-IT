---
title: Scopri come installare eventi IO per Adobe Commerce 2.4.6
description: Scopri come installare i moduli necessari per gli eventi IO in Adobe Commerce 2.4.6 per l’utilizzo in Adobe Developer App Builder
landing-page-description: Scopri come installare diversi moduli necessari per Adobe Commerce 2.4.6.
short-description: Learn how to install several modules needed for Adobe Commerce 2.4.6.
kt: 11887
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-22T00:00:00Z
badge: "Adobe Commerce 2.4.6"
source-git-commit: d85426bcf3ae0412a433414d70c874964905dda0
workflow-type: tm+mt
source-wordcount: '147'
ht-degree: 0%

---


# Installazione di Adobe Commerce 2.4.6

Scopri come installare diversi nuovi moduli in Adobe Commerce utilizzando Composer per la versione 2.4.6. È disponibile una documentazione aggiuntiva all’indirizzo [Installare gli eventi di Adobe I/O per Adobe Commerce](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}.

## A chi serve questo video?

* Sviluppatori non esperti di Adobe Commerce e Adobe Developer App Builder utilizzando gli eventi I/O.

## Contenuto video {#video-content}

* Comandi da eseguire per l&#39;hosting locale
* Comandi da eseguire per Adobe Commerce Cloud
* Modifica richiesta stile Adobe Commerce Cloud

>[!VIDEO](https://video.tv.adobe.com/v/3415795?quality=12&learn=on)

## Comandi utili {#useful-commands}

Esistono diversi comandi che differiscono leggermente, a seconda che ti trovi in un ambiente self-hosting o che utilizzi Adobe Commerce Cloud.

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
