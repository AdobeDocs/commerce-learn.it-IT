---
title: Creare un prodotto raggruppato
description: Scopri come creare un prodotto raggruppato utilizzando l’API REST e l’amministratore Commerce.
kt: 14585
doc-type: video
audience: all
activity: use
last-substantial-update: 2023-11-30T00:00:00Z
feature: Catalog Management, Admin Workspace, Backend Development, Integration, REST
topic: Commerce, Integrations, Content Management
role: Developer, User
level: Beginner
exl-id: 3ad7125b-ef6d-4ea0-9fa7-8fc9eb399ec1
source-git-commit: 76a67af957b0d8c1eb64ad42f92412f338650d4b
workflow-type: tm+mt
source-wordcount: '513'
ht-degree: 0%

---

# Creare un prodotto raggruppato

Un prodotto raggruppato è costituito da semplici prodotti autonomi presentati come un gruppo. Puoi offrire varianti di un singolo prodotto o raggrupparle per stagione o tema. Prima di creare un prodotto raggruppato, verifica che tutti i prodotti semplici da includere nel gruppo siano disponibili in Adobe Commerce e creane altri inesistenti.

Questa esercitazione spiega come creare un prodotto raggruppato utilizzando l’API REST e l’amministratore Adobe Commerce.

Utilizza l’API REST per creare un prodotto di gruppo in Admin:

1. Crea un prodotto raggruppato vuoto.
1. Creare prodotti semplici da utilizzare nel prodotto raggruppato.
1. Popola il prodotto raggruppato vuoto con prodotti semplici.
1. Crea un prodotto raggruppato vuoto e associa i prodotti semplici.

   Quando si associano prodotti semplici al prodotto raggruppato, l&#39;attributo di ordinamento (`position`) nel payload viene utilizzato dal front-end per visualizzare i prodotti associati nell&#39;ordine desiderato. Se l&#39;attributo `position` non è specificato, i prodotti vengono visualizzati nell&#39;ordine in cui sono stati aggiunti al prodotto raggruppato.

Crea prima i prodotti semplici quando crei prodotti raggruppati dall’amministratore di Adobe Commerce. Quando si è pronti a creare il prodotto raggruppato, associare i prodotti semplici assegnandoli al prodotto raggruppato in un unico batch.

## A chi serve questo video?

- Gestori di siti Web
- eCommerce merchandisers
- Nuovi sviluppatori Adobe Commerce che desiderano imparare a creare prodotti raggruppati in Adobe Commerce utilizzando l’API REST.

## Contenuto video

>[!VIDEO](https://video.tv.adobe.com/v/3454046?learn=on&captions=ita)

## Impostazione del prodotto raggruppato

In questo esempio, esistono tre prodotti semplici (creati per primi) e un prodotto raggruppato. Al prodotto raggruppato sono associati due prodotti semplici e al prodotto raggruppato viene aggiunto il terzo prodotto semplice.

## Creare il primo prodotto semplice utilizzando cURL

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=a42da0096288718c9556f77549c6305f; private_content_version=564dde2976849891583a9a649073f01e' \
--data '{
  "product": {
    "sku": "product-sku-one",
    "name": "Simple product one",
    "attribute_set_id": 4,
    "price": 1.11,
    "type_id": "simple"
  }
}
```

## Creare il secondo prodotto semplice utilizzando cURL

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=a42da0096288718c9556f77549c6305f; private_content_version=564dde2976849891583a9a649073f01e' \
--data '{
  "product": {
    "sku": "product-sku-two",
    "name": "Simple Product two",
    "attribute_set_id": 4,
    "price": 2.22,
    "type_id": "simple"
  }
}
```

## Creare il terzo prodotto semplice utilizzando cURL

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=a42da0096288718c9556f77549c6305f; private_content_version=564dde2976849891583a9a649073f01e' \
--data '{
  "product": {
    "sku": "product-sku-three",
    "name": "Simple product three",
    "attribute_set_id": 4,
    "price": 3.33,
    "type_id": "simple"
  }
}
```

## Creare un prodotto raggruppato vuoto utilizzando cURL

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=28a020734488eef2600f8d5c7f302b53; private_content_version=564dde2976849891583a9a649073f01e' \
--data '{
    "product":{
        "sku":"my-new-grouped-product",
        "name":"This is my New Grouped Product",
        "attribute_set_id":4,
        "type_id":"grouped",
        "visibility":4
    }
}
'
```

## Aggiungere il primo e il secondo prodotto semplice al prodotto raggruppato

```bash
curl --location '{{your.url.here}}/rest/default/V1/products/my-new-grouped-product/links' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=28a020734488eef2600f8d5c7f302b53; private_content_version=564dde2976849891583a9a649073f01e' \
--data '{
    "items":[
        {
            "sku":"my-new-grouped-product",
            "link_type":"associated",
            "linked_product_sku":"product-sku-one",
            "linked_product_type":"simple",
            "position":1,
            "extension_attributes":{
            "qty":1
            }
        },
        {
            "sku":"my-new-grouped-product",
            "link_type":"associated",
            "linked_product_sku":"product-sku-two",
            "linked_product_type":"simple",
            "position":2,
            "extension_attributes":{
            "qty":1
            }
        }
    ]
}
'
```

## Aggiungere il terzo prodotto semplice al prodotto raggruppato esistente

Includere il numero di posizione appropriato (tranne `1` o `2`), utilizzato per i primi due prodotti originariamente associati al prodotto raggruppato. In questo esempio, la posizione è `4`.

```bash
curl --location --request PUT '{{your.url.here}}/rest/default/V1/products/my-new-grouped-product/links' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=9e61396705e9c17423eca2bdf2deefb2' \
--data '{
    "entity":{
        "sku":"my-new-grouped-product",
        "link_type":"associated",
        "linked_product_sku":"product-sku-three",
        "linked_product_type":"simple",
        "position":4,
        "extension_attributes":{
            "qty":1
        }
    }
}

'
```

## Eliminare un prodotto semplice da un prodotto raggruppato

Per [eliminare un prodotto semplice](https://developer.adobe.com/commerce/webapi/rest/tutorials/grouped-product/) da un prodotto raggruppato, utilizzare: `DELETE /V1/products/{sku}/links/{type}/{linkedProductSku}`.

Per individuare gli elementi da utilizzare come `{type}`, utilizzare xdebug per acquisire la richiesta e valutare i $linkTypes: `related`, `crosssell`, `uupsell` e `associated`.
![Tipi di collegamento prodotto raggruppati - testo alternativo](/help/assets/site-management/catalog/grouped-types.png "Tipi di collegamento prodotto raggruppati acquisiti durante la sessione xdebug")

Quando si collegavano i prodotti semplici al prodotto raggruppato, il payload conteneva alcune sezioni simili alle seguenti:

```bash
        {
            "sku":"my-new-grouped-product",
            "link_type":"associated",
            "linked_product_sku":"product-sku-two",
            "linked_product_type":"simple",
            "position":2,
            "extension_attributes":{
            "qty":1
            }
        }
```

Nel payload, il valore `link_type` `associated` fornisce il valore `{type}` richiesto nella richiesta DELETE. L&#39;URL della richiesta sarà simile a `/V1/products/my-new-grouped-product/links/associated/product-sku-three`.

Vedere la richiesta cURL per eliminare il prodotto semplice con SKU `product-sku-three` dal prodotto raggruppato con SKU `my-new-grouped-product`:

```bash
curl --location --request DELETE '{{your.url.here}}rest/default/V1/products/my-new-grouped-product/links/associated/product-sku-three' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=9e61396705e9c17423eca2bdf2deefb2'
```

## Ottenere un prodotto raggruppato utilizzando cURL

```bash
curl --location '{{your.url.here}}rest/default/V1/products/some-grouped-product-sku' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=3b797a6cc3c5c71f2193109fb9838b12'
```

## Risorse aggiuntive

- [Crea e gestisci prodotti raggruppati](https://developer.adobe.com/commerce/webapi/rest/tutorials/grouped-product/){target="_blank"}
- [Prodotto Raggruppato](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/types/product-create-grouped.html?lang=it){target="_blank"}
- [Tutorial REST di Adobe Developer](https://developer.adobe.com/commerce/webapi/rest/tutorials/prerequisite-tasks/){target="_blank"}
- [RiDoc REST Adobe Commerce](https://adobe-commerce.redoc.ly/2.4.6-admin/tag/products#operation/PostV1Products){target="_blank"}
