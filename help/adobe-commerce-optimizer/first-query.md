---
title: Eseguire query sui dati
description: Scopri come eseguire query sui dati dei prodotti Adobe Commerce Optimizer utilizzando GraphQL, incluso come formattare le risposte JSON con jq e strutturare le variabili di query di ricerca.
feature: Saas, Storefront
topic: Commerce
role: Developer
level: Beginner
doc-type: Tutorial
duration: 204
last-substantial-update: 2025-08-13T00:00:00.000Z
jira: KT-18548
exl-id: bad3d926-2952-4bac-b685-adb16f009f8d
autotag-review: '2026-08-11T18:59:22.151Z'
TQID: 'https://experienceleague.adobe.com/IxrS6rwleWgU0-jtwu4hUavQuZesbQ6h5z7zVZR2xCo'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: d3b92bef-63fa-5031-a925-d04d9362d616
    internal-label: Saas
  - id: d1e21356-0064-4f48-9089-16e3f0dbd2a6
    internal-label: Storefront
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
    internal-label: Beginner
source-git-commit: 71835f0240311d4b26ec8e0d9bbdf005f9887579
workflow-type: tm+mt
source-wordcount: '127'
ht-degree: 0%
---
# Query di dati Adobe Commerce Optimizer

Scopri come eseguire query sui dati utilizzando GraphQL su un’istanza Adobe Commerce Optimizer.

## A chi serve questo video?

* Architetto e sviluppatori di soluzioni Commerce

## Contenuto video

* Eseguire query sui dati tramite GraphQL
* Utilizzo di jq per semplificare la lettura di json

>[!VIDEO](https://video.tv.adobe.com/v/3470800?learn=on)

## Esempi di codice

Assicurarsi di scambiare valori come `{{insert-your-graphql-endpoint-url}}`, `{{insert-your-ac-view-id}}` e `{{your-search-query-string}}` con i valori necessari nella query.

Query di esempio di base

```bash
curl '{{insert-your-graphql-endpoint-url}}' \
-H 'Content-Type: application/json' \
-H 'AC-View-ID: {{insert-your-ac-view-id}}' \
-d '{"query": "query ProductSearch($search: String!) { productSearch( phrase: $search, page_size: 10, current_page: 2) { items { productView { sku name description shortDescription images { url } ... on SimpleProductView { attributes { label name value } price { regular { amount { value currency } } roles } } } } } }", "variables": { "search": "{{your-search-query-string}}"}}'
```

Query di esempio di base che utilizza `jq` per stampare correttamente l&#39;output

```bash
curl '{{insert-your-graphql-endpoint-url}}' \
-H 'Content-Type: application/json' \
-H 'AC-View-ID: {{insert-your-ac-view-id}}' \
-d '{"query": "query ProductSearch($search: String!) { productSearch( phrase: $search, page_size: 10, current_page: 2) { items { productView { sku name description shortDescription images { url } ... on SimpleProductView { attributes { label name value } price { regular { amount { value currency } } roles } } } } } }", "variables": { "search": "{{your-search-query-string}}"}}' | jq .
```

## Contenuto correlato

* [Guida introduttiva all’API di merchandising](https://developer.adobe.com/commerce/services/optimizer/merchandising-services/using-the-api#make-your-first-request){target="_blank"}
* [Guida [!DNL Adobe Commerce Optimizer]](https://experienceleague.adobe.com/en/docs/commerce/optimizer/overview){target="_blank"}

