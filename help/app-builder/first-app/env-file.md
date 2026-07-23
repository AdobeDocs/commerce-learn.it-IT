---
title: Il file .env
description: Scopri come generare e configurare il file .env per l’applicazione Adobe Developer App Builder, inclusa la gestione dei segreti e la prevenzione di commit accidentali nel controllo del codice sorgente.
jira: KT-21681
doc-type: Tutorial
duration: 177
last-substantial-update: 2023-03-13
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Developer
level: Beginner, Intermediate
exl-id: 934fcdd1-ee61-4914-89ce-f6f04b1bc763
TQID: https://experienceleague.adobe.com/Sup5quVNU60RYmkCJaYGgLq2gHCpCvaIKEFLH2MAifQ
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2:
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: 3f243b43695e65d63cb25142ec5a886ce390d502
workflow-type: tm+mt
source-wordcount: 147
ht-degree: 0%

---

# Generare e configurare il file .env {#env-file}

`.env` è un file speciale che non fa parte del modulo di esempio, ma è importante per l&#39;utilizzo nell&#39;applicazione Adobe Developer App Builder. Questo file contiene segreti e altre informazioni. Evita di eseguire il commit di questo file in qualsiasi archivio di codice.

## A chi serve questo video?

* Sviluppatori senza esperienza di Adobe Commerce con esperienza limitata che utilizzano Adobe App Builder e che desiderano conoscere il file `.env`.

## Contenuto video

* Introduzione al file .env e suo scopo
* Come generare il file .env
* Per aggiungere nuovi segreti, aggiungi il file
* Evita di eseguire il commit del file perché contiene informazioni riservate

>[!VIDEO](https://video.tv.adobe.com/v/3421068?captions=ita&learn=on)

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

