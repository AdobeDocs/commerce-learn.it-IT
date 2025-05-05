---
title: Il file .env
description: Scopri i tipi di file nel file .env per questa applicazione di esempio
landing-page-description: Scopri Adobe Developer App Builder utilizzato con Adobe Commerce e quali tipi di contenuto vengono utilizzati nel file .env
kt: 12423
doc-type: tutorial
audience: all
last-substantial-update: 2023-3-13
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Architect, Developer
level: Beginner, Intermediate
exl-id: 934fcdd1-ee61-4914-89ce-f6f04b1bc763
source-git-commit: 404d2708a6d540d6fb19a33afb20726356cd8000
workflow-type: tm+mt
source-wordcount: '157'
ht-degree: 0%

---

# Generare e configurare il file .env {#env-file}

`.env` è un file speciale che non fa parte del modulo di esempio, ma è importante per l&#39;utilizzo nell&#39;applicazione Adobe Developer App Builder. Questo file contiene segreti e altre informazioni. Evita di eseguire il commit di questo file in qualsiasi archivio di codice.

## A chi serve questo video?

* Sviluppatori senza esperienza di Adobe Commerce con esperienza limitata che utilizzano Adobe App Builder e desiderano conoscere il file `.env`.

## Contenuto video

* Introduzione al file .env e suo scopo
* Come generare il file .env
* Come aggiungere il file per aggiungere nuovi segreti
* Evita di eseguire il commit del file perché contiene informazioni riservate

>[!VIDEO](https://video.tv.adobe.com/v/3421068?quality=12&learn=on&captions=ita)

## Esempio di codice

```bash
# Specify your secrets here
# The .env file must not be committed to source control
## Adobe I/O Runtime credentials
AIO_runtime_auth=abcd1234-aaa-bbb-ccc-12345:Abcdd12345asdfadsfadsfee2323232323232
AIO_runtime_namespace=12345-someworkspace-stage
AIO_runtime_apihost=https://adobeioruntime.net
## Adobe I/O Console service account credentials (JWT) Api Key
SERVICE_API_KEY=

# You can include some commerce OAUTH credentials too, our sample module will use this
#COMMERCE_BASE_URL=https://somecommercewebsite.com/
#COMMERCE_CONSUMER_KEY=abcebdme5bvafnemk0mdeeiyfq123
#COMMERCE_CONSUMER_SECRET=ffff86sqws3pss5hhuofiqgq4t04rrr11
#COMMERCE_ACCESS_TOKEN=gdddfccronj098r4m04zyq773s5o64
#COMMERCE_ACCESS_TOKEN_SECRET=ggg7nb19jhr5gi9jzfan9ggzipe8yrus
```

Questi valori statici vengono utilizzati nel modulo di esempio nel file `actions/commerce.index.js`.

```javascript
        const oauth = getCommerceOauthClient(
            {
                url: params.COMMERCE_BASE_URL,
                consumerKey: params.COMMERCE_CONSUMER_KEY,
                consumerSecret: params.COMMERCE_CONSUMER_SECRET,
                accessToken: params.COMMERCE_ACCESS_TOKEN,
                accessTokenSecret: params.COMMERCE_ACCESS_TOKEN_SECRET
            },
            logger
        )
```

{{$include /help/_includes/app-builder-first-app-related-links.md}}
