---
title: Eseguire query sui dati
description: Scopri come eseguire query sui dati per un’istanza di Adobe Commerce Optimizer.
feature: Saas, Storefront
topic: Commerce
role: Developer
level: Beginner
doc-type: Tutorial
duration: 182
last-substantial-update: 2025-08-13T00:00:00Z
jira: KT-18548
source-git-commit: c598e46f7119ebdb1575e41c65d6285109fd9af9
workflow-type: tm+mt
source-wordcount: '94'
ht-degree: 0%

---

# Query di dati Adobe Commerce Optimizer

Scopri come eseguire query sui dati utilizzando GraphQL su un’istanza Adobe Commerce Optimizer.

## A chi serve questo video?

* Architetto e sviluppatori di soluzioni Commerce

## Contenuto video

* Eseguire query sui dati tramite GraphQL
* Utilizzo di jq per semplificare la lettura di json

>[!VIDEO](https://video.tv.adobe.com/v/3470800?learn=on&enablevpops)

## Esempi di codice

Assicurarsi di scambiare valori come `{{insert-your-graphql-endpoint-url}}`, `{{insert-your-ac-source-locale}}` e `{{your-search-query-string}}` con i valori necessari nella query.

Query di esempio di base

```bash
curl '{{insert-your-graphql-endpoint-url}}' \
-H 'Content-Type: application/json' \
-H 'AC-Source-Locale: {{insert-your-ac-source-locale}}' \
-d '{"query": "query ProductSearch($search: String!) { productSearch( phrase: $search, page_size: 10, current_page: 2) { items { productView { sku name description shortDescription images { url } ... on SimpleProductView { attributes { label name value } price { regular { amount { value currency } } roles } } } } } }", "variables": { "search": "{{your-search-query-string}}"}}'
```

Query di esempio di base che utilizza `jq` per stampare correttamente l&#39;output

```bash
curl '{{insert-your-graphql-endpoint-url}}' \
-H 'Content-Type: application/json' \
-H 'AC-Source-Locale: {{insert-your-ac-source-locale}}' \
-d '{"query": "query ProductSearch($search: String!) { productSearch( phrase: $search, page_size: 10, current_page: 2) { items { productView { sku name description shortDescription images { url } ... on SimpleProductView { attributes { label name value } price { regular { amount { value currency } } roles } } } } } }", "variables": { "search": "{{your-search-query-string}}"}}' | jq .
```

## Contenuto correlato

* [Guida introduttiva all&#39;API di merchandising](https://developer.adobe.com/commerce/services/optimizer/merchandising-services/using-the-api/#make-your-first-request){target="_blank"}
* [[!DNL Adobe Commerce Optimizer] Guida](https://experienceleague.adobe.com/en/docs/commerce/optimizer/overview){target="_blank"}
