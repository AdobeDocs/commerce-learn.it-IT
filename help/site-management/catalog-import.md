---
title: Learn about catalog import options that come native with Adobe Commerce
description: Learn how a few of the native options for importing your catalog to your Adobe Commerce store.
kt: 13634
doc-type: tutorial
duration: 211
audience: all
activity: use
last-substantial-update: 2023-8-15
feature: Backend Development, Data Import/Export, REST
topic: Commerce, Administration, Content Management
role: Admin, User
level: Beginner, Intermediate
exl-id: 18713a44-df39-4b94-91ce-c7efeb4ce2b3
TQID: https://experienceleague.adobe.com/-JG7blrxImSXjA2DP9soZfsicISW0hkP2zJeWdMpVBU
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: bd989d82-1e15-4534-88db-f1f51dd77ffa
  - id: c18ed297-2187-4aec-affb-9d9654eca6fc
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2:
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 898
ht-degree: 0%

---

# Options for importing a catalog

There are a few native methods for importing a catalog into Adobe Commerce. Each method has its own reasoning for usage along with pros and cons that must be considered.

Choose from one of the options below to learn more.

>[!BEGINTABS]

>[!TAB Manual]

## Creating the products manually {#manual-import}

If you have a limited catalog and updates are infrequent, creating them manually might be the best option. It requires time to enter each product and some limited training to how to use the Commerce Admin. Manual catalog management is not the right option for most stores, but in certain situations, it may make sense. To see additional documentation for this process, visit [Create a product](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/product-create.html?lang=it){target="_blank"}. Do not forget, you can use more than one method to manage your catalog, however once automation is used, manual edits must be limited. Automated updates have the opportunity to overwrite any changes performed manually, and therefore cause confusion. Once the integration with Adobe Commerce to manage the catalog is using automation and APIs, it is advised to restrict management of the catalog from the admin through [user roles and permissions](https://experienceleague.adobe.com/docs/commerce-admin/systems/user-accounts/permissions-user-roles.html?lang=it){target="_blank"}.

### When to consider this approach

* Very small catalog, for example fewer than 50 products
* Updates are infrequent
* You have all the product details, images, videos, and you do not want to take the time to learn how to convert the data to CSV
* You want to include adding images and videos when creating the products
* Your team is `not` familiar with APIs and how OAUTH works

>[!TAB Admin CSV]

## Admin CSV import tool {#admin-csv}

This tool allows a store owner to import a catalog using a CSV right from the commerce admin.
[Import Data from Commerce Admin](https://experienceleague.adobe.com/docs/commerce-admin/systems/data-transfer/import/data-import.html?lang=it){target="_blank"}

Pros:
Uploading a CSV from the admin is a straight forward approach to catalog management. It allows for faster catalog product updates to a moderately sized catalog.

Cons:

* Slow
* Maximum upload file sized defined on server and may not be easily adjusted by a store owner.
* Requires admin access and someone to perform the action, automation is limited
* Schedule imports are limited to 1x a day max
* The images and videos associated must be uploaded separately

### When to consider this approach

* Catalog size is moderate
* Updates are not more than once a day
* you have some access to server configurations in case that you must increase max file upload size
* Your team is `not` familiar with APIs and how OAUTH works

>[!TAB Bulk REST API]

## Bulk REST API {#bulk-rest-api}

The bulk REST API allows for automation and more frequent updates. This API is faster than using the admin upload of CSV.
[Bulk endpoints documentation](https://developer.adobe.com/commerce/webapi/rest/use-rest/bulk-endpoints/){target="_blank"}

Pro:
Possibilità di importare set di dati di grandi dimensioni non in formato CSV.

Contro:

* Le immagini e i video associati devono essere caricati separatamente
* Può essere limitata dai vincoli di larghezza di banda del provider di hosting

### Quando considerare questo approccio

* Il catalogo è di qualsiasi dimensione
* Gli aggiornamenti sono frequenti; è accettabile più di 1 volta al giorno
* Il tempo di importazione è importante ma non critico ed è accettabile un breve ritardo nell’elaborazione dei dati di importazione
* I dati non sono strutturati in formato CSV e non è possibile trasformarli utilizzando l’automazione

>[!TAB API REST ASINCRONA]

## API REST ASINCRONA {#async-rest-api}

Un endpoint web asincrono intercetta i messaggi in un’API web e li scrive nella coda dei messaggi. Ogni volta che il sistema accetta una tale richiesta API, genera un identificatore UUID. Adobe Commerce include questo UUID quando aggiunge il messaggio alla coda. Quindi, un consumatore legge i messaggi dalla coda ed esegue i messaggi singolarmente.
[Documentazione degli endpoint web asincroni](https://developer.adobe.com/commerce/webapi/rest/use-rest/asynchronous-web-endpoints/){target="_blank"}

Pro:

* Importare rapidamente i dati
* L&#39;ambito dell&#39;archivio è supportato oppure è possibile specificare `all` per eseguire l&#39;operazione su tutti gli archivi esistenti

Contro:

* La richiesta GET non è supportata

### Quando considerare questo approccio

* Le importazioni sono frequenti
* Nessun problema con un ritardo ridotto dal momento in cui vengono inviati tramite API e quindi elaborati dalla coda dei messaggi.


>[!TAB API REST CSV]

## API REST CSV {#csv-rest-api}

Questa opzione API consente importazioni estremamente veloci rispetto a tutte le altre opzioni native.

[Importa dati REST CSV api](https://developer.adobe.com/commerce/webapi/rest/modules/import/){target="_blank"}
Pro:

* Metodo più veloce per elaborare i dati in arrivo
* Può essere eseguito più volte al giorno
* I dati possono essere compressi utilizzando gzip per le richieste di grandi dimensioni, in modo da evitare i limiti di dimensione delle richieste HTTP.

Contro:

* Le immagini e i video associati devono essere caricati separatamente
* I dati devono essere in formato CSV

### Quando considerare questo approccio

* Il catalogo è di qualsiasi dimensione
* Gli aggiornamenti sono frequenti; è accettabile più di 1 volta al giorno
* Il tempo totale di importazione è importante
* I dati sono già in formato CSV o possono essere facilmente trasformati tramite automazione

>[!ENDTABS]

## Risorse aggiuntive

* [Importare dati utilizzando un nuovo CSV REST](https://developer.adobe.com/commerce/webapi/rest/modules/import/){target="_blank"}
* [Documentazione principale sui dati di importazione](https://experienceleague.adobe.com/docs/commerce-admin/systems/data-transfer/import/data-import.html?lang=it){target="_blank"}
* [Note sulla versione di Adobe Commerce 2.4.6](https://experienceleague.adobe.com/docs/commerce-operations/release/notes/adobe-commerce/2-4-6.html?lang=it){target="_blank"}
* [Utenti, ruoli e autorizzazioni](../site-management/users-roles-permissions.md){target="_blank"}
