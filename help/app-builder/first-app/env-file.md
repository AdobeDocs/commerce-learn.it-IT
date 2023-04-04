---
title: Il file .env
description: Scopri i tipi di file nel file .env per questa applicazione di esempio
landing-page-description: Scopri Adobe Developer App Builder utilizzato con Adobe Commerce e quali tipi di contenuto vengono utilizzati nel file .env
kt: 12423
doc-type: tutorial
audience: all
last-substantial-update: 2023-03-13T00:00:00Z
source-git-commit: d85426bcf3ae0412a433414d70c874964905dda0
workflow-type: tm+mt
source-wordcount: '157'
ht-degree: 0%

---


# Genera e configura il file .env {#env-file}

La `.env` è un file speciale che non fa parte del modulo di esempio, ma è importante per l’utilizzo nell’applicazione Adobe Developer App Builder. Questo file contiene segreti e altre informazioni. Evita di eseguire il commit di questo file in qualsiasi archivio di codice.

## A chi serve questo video?

* Sviluppatori nuovi in Adobe Commerce con esperienza limitata con Adobe App Builder che desiderano imparare a usare `.env` file.

## Contenuto video

* Introduzione al file .env e relativo scopo
* Come generare il file .env
* Come aggiungere il file per aggiungere nuovi segreti
* Evita di eseguire il commit di questo file perché contiene informazioni sensibili

>[!VIDEO](https://video.tv.adobe.com/v/3416593?quality=12&learn=on)

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

Puoi vedere questi valori statici in uso nel modulo di esempio nel file `actions/commerce.index.js`.

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
