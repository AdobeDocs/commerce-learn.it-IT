---
title: Creare un prodotto configurabile
description: Scopri come creare un prodotto configurabile utilizzando l’API REST e l’amministratore Commerce.
kt: 14586
doc-type: video
duration: 1760
audience: all
activity: use
last-substantial-update: 2023-12-15T00:00:00.000Z
feature: Catalog Management, Admin Workspace, Backend Development, Integration, REST
topic: Commerce, Integrations, Content Management
role: Developer, User
level: Beginner
exl-id: 112bec9a-0f8e-4252-8c52-f486a5e663b5
TQID: https://experienceleague.adobe.com/XAvtOnOIycqQ4z-uztWuVzzv0--eVit-I-QDnl67ba8
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: c18ed297-2187-4aec-affb-9d9654eca6fc
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 994
ht-degree: 0%

---

# Creare un prodotto configurabile

Un prodotto configurabile è un prodotto principale di più prodotti semplici. Definisci un prodotto configurabile per richiedere all’acquirente di effettuare una o più scelte per selezionare una variante di prodotto specifica. Ad esempio, se il prodotto è una camicia, l&#39;acquirente deve scegliere le opzioni di dimensione e colore per selezionare la camicia.

Sebbene un prodotto configurabile utilizzi più SKU e la configurazione iniziale possa richiedere un po’ di tempo, alla fine potrai risparmiare tempo. Se prevedi di espandere la tua attività, il tipo di prodotto configurabile è una buona scelta per i prodotti con più opzioni.

Prima di creare un prodotto configurabile, verifica che tutti i prodotti semplici da includere nel prodotto configurabile siano disponibili in Adobe Commerce. Crea quelli che non esistono.

In questo tutorial, scopri come creare un prodotto configurabile utilizzando l’API REST e l’amministratore Adobe Commerce.

Utilizza l’API REST per creare un prodotto configurabile:

1. Ottieni gli attributi per un [set di attributi](https://experienceleague.adobe.com/docs/commerce-admin/catalog/product-attributes/create/attribute-sets.html?lang=it) per utilizzare i numeri ID per le chiamate API successive.
1. Crea prodotti semplici da utilizzare nel prodotto configurabile.
1. Crea un prodotto configurabile vuoto e associa i prodotti semplici.
1. Imposta gli attributi del prodotto configurabile.
1. Popola il prodotto configurabile vuoto con prodotti semplici.
1. Ottieni il prodotto configurabile e tutti gli attributi.
1. Ottieni i prodotti secondari assegnati per il prodotto configurabile.
1. Elimina l’associazione di prodotti semplici a prodotti configurabili.

Quando crei prodotti configurabili dall’amministratore di Adobe Commerce, puoi creare prima i prodotti semplici o utilizzare lo strumento automatizzato che crea nuovi prodotti semplici da utilizzare mediante la procedura guidata.

## A chi serve questo video?

* Gestori di siti Web
* eCommerce merchandisers
* New Adobe Commerce developers who want to learn how to create configurable products in Adobe Commerce using the REST API

## Contenuto video

>[!VIDEO](https://video.tv.adobe.com/v/3426381?learn=on)

## Get the color attributes using cURL

In this example, the entire attribute set with all the individual attributes is returned for attribute set 10. It can be long, hundreds of lines are not uncommon. When reviewing the response, the attribute ID for color will likely be in the middle. Expedite the search for these values by using grep or other methods to search the results. My response was near line 665 and is included in the following snippet from the JSON response.

```json
...
{
        "attribute_id": 93,
        "attribute_code": "color",
        "frontend_input": "select",
        "entity_type_id": "4",
        "is_required": false,
        "options": [
            {
                "label": " ",
                "value": ""
            },
            {
                "label": "Red",
                "value": "13"
            },
            {
                "label": "Blue",
                "value": "14"
            },
            {
                "label": "Green",
                "value": "15"
            }
        ],
...
```


To retrieve the attribute IDs to set up your configurable product, update the `attribute-sets/10/attributes` portion of the following cURL request to replace `10` with the attribute set ID in your environment. This request uses the GET method.

```bash
curl --location '{{your.url.here}}rest/V1/products/attribute-sets/10/attributes' \
--header 'Authorization: Bearer {{Your Bearer Token}}'
```

## Create the first simple product using cURL

### Adjust environment IDs and product details

Create the first simple product by using the API to send the following POST request using cURL.

Before submitting the request, update the example with values for your environment.

* Change `"attribute-set": 10` to replace `10` with the attribute set ID from your environment.
* Change `"value": "13"` to replace `13` with the value from your environment.

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=aff63a1634f6f1a773e7a4894bf1a55c' \
--data '{
  "product": {
    "sku": "Kids-Hawaiian-Ukulele-red",
    "name": "Kids Hawaiian Ukulele Red",
    "attribute_set_id": 10,
    "price": 12.50,
    "status": 1,
    "visibility": 1,
    "type_id": "simple",
    "weight": "0.5",
    "extension_attributes": {
        "stock_item": {
            "qty": "10",
            "is_in_stock": true
        }
    },
    "custom_attributes": [
        {
            "attribute_code": "color",
            "value": "13"
        }
    ]
  }
}
'
```

## Create the second simple product using cURL

Create the second simple product by using the API to send the following POST request using cURL.

Before submitting the request, update the example with values for your environment.

* Change `"attribute_set_id": 10,` and replace `10` with the attribute set id from in your environment.
* Change `"value": "14"` and replace `14` with the value from your environment.

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=aff63a1634f6f1a773e7a4894bf1a55c' \
--data '{
  "product": {
    "sku": "Kids-Hawaiian-Ukulele-Blue",
    "name": "Kids Hawaiian Ukulele Blue",
    "attribute_set_id": 10,
    "price": 15,
    "status": 1,
    "visibility": 1,
    "type_id": "simple",
    "weight": "0.5",
    "extension_attributes": {
        "stock_item": {
            "qty": "20",
            "is_in_stock": true
        }
    },
    "custom_attributes": [
        {
            "attribute_code": "color",
            "value": "14"
        }
    ]
  }
}
'
```

## Create the third simple product using cURL

Create the third simple product by sending the following POST request using cURL.

Before submitting the request, update the example with values for your environment.

* Change `"attribute_set_id": 10,` to replace `10` with the attribute set ID from your environment.
* Change `"value": "15"` and replace `15` with the value from your environment.

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=aff63a1634f6f1a773e7a4894bf1a55c' \
--data '{
  "product": {
    "sku": "Kids-Hawaiian-Ukulele-Green",
    "name": "Kids Hawaiian Ukulele Green",
    "attribute_set_id": 10,
    "price": 25,
    "status": 1,
    "visibility": 1,
    "type_id": "simple",
    "weight": "0.5",
    "extension_attributes": {
        "stock_item": {
            "qty": "30",
            "is_in_stock": true
        }
    },
    "custom_attributes": [
        {
            "attribute_code": "color",
            "value": "15"
        }
    ]
  }
}
'
```

## Create an empty configurable product using cURL

Create an empty configurable product by sending the following POST request using cURL.

Before submitting the request, update the example with values for your environment.

* Change `"attribute_set_id": 10,` and replace `10` with the attribute set id from your environment.
* Change `"value": "93"` and replace `93` with the value from your environment.

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=aff63a1634f6f1a773e7a4894bf1a55c' \
--data '{
  "product": {
    "sku": "Kids-Hawaiian-Ukulele",
    "name": "Kids Hawaiian Ukulele",
    "attribute_set_id": 10,
    "status": 1,
    "visibility": 4,
    "type_id": "configurable",
    "weight": "0.5",
    "custom_attributes": [
        {
            "attribute_code": "color",
            "value": "93"
        }
    ]
  }
}'
```

## Set the options available for the configurable product

Set the options available for the configurable product by sending the following POST request using cURL.

Before submitting the request, change `"attribute_id": 93,` to replace `93` with the attribute id from your environment.

```bash
curl --location '{{your.url.here}}/rest/default/V1/configurable-products/Kids-Hawaiian-Ukulele/options' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=aff63a1634f6f1a773e7a4894bf1a55c' \
--data '{
  "option": {
    "attribute_id": "93",
    "label": "Color",
    "position": 0,
    "is_use_default": true,
    "values": [
      {
        "value_index": 9
      }
    ]
  }
}'
```

If you forget to set the options for the configurable product (parent), you get an error when you try to associate a child product to the configurable product. The error message is similar to the following example:

`{"message":"The parent product doesn't have configurable product options.","trace":"#0 [internal function]: Magento\\ConfigurableProduct\\Model\\LinkManagement->addChild('Kids-Hawaiian-U...'}`

## Link the child product to the configurable

Now, you have created three simple products:

* `"Kids Hawaiian Ukulele Red"`,
* `"Kids-Hawaiian-Ukulele-Blue"`
* `"Kids-Hawaiian-Ukulele-Green"`

Add these simple products as children of the configurable product by sending the following POST request. Submit a separate request for each product.

For each request, update the `childSKU` value with the value for the child product you are adding. The following example assigns the simple product `kids-Hawaiian-Ukulele-red` to the configurable product with the SKU `Kids-Hawaiian-Ukulele-red`.


```bash
curl --location '{{your.url.here}}rest/default/V1/configurable-products/Kids-Hawaiian-Ukulele/child' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=aff63a1634f6f1a773e7a4894bf1a55c' \
--data '{
  "childSku": "Kids-Hawaiian-Ukulele-red"
}
'
```

## Get a configurable product using cURL

Now that you have created a configurable product with three assigned child SKUs. You can see the linked IDs for the assigned products by sending the following GET request using cURL. This request returns detailed information about the configurable product.

```json
...
        "configurable_product_links": [
            155,
            157,
            156
        ]
...
```

The following uses the GET method

```bash
curl --location '{{your.url.here}}/rest/default/V1/products/Kids-Hawaiian-Ukulele' \
--header 'Authorization: Bearer {{Your Bearer Token}}'
```

## Get the children product associated to a configurable product

Return only the children associated with the configurable product by sending the following GET request. The response will include all the attributes for the child product including SKU and price.

Di seguito viene utilizzato il metodo GET

```bash
curl --location '{{your.url.here}}/rest/default/V1/configurable-products/kids-hawaiian-ukulele/children' \
--header 'Authorization: Bearer {{Your Bearer Token}}'
```

## Elimina o rimuovi un prodotto secondario dal prodotto principale configurabile

Puoi rimuovere un prodotto secondario da un prodotto configurabile senza eliminare il prodotto dal catalogo inviando la seguente richiesta DELETE utilizzando cURL.

```bash
curl --location --request DELETE '{{your.url.here}}/rest/default/V1/configurable-products/Kids-Hawaiian-Ukulele/children/Kids-Hawaiian-Ukulele-Blue' \
--header 'Authorization: Bearer {{Your Bearer Token}}'
```

## Risorse aggiuntive

* [Crea un tutorial di prodotto configurabile](https://developer.adobe.com/commerce/webapi/rest/tutorials/configurable-product/){target="_blank"}
* [Prodotto configurabile](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/types/product-create-configurable.html?lang=it){target="_blank"}
* [Tutorial REST per Adobe Developer](https://developer.adobe.com/commerce/webapi/rest/tutorials/prerequisite-tasks/){target="_blank"}
* [Adobe Commerce REST ReDoc](https://adobe-commerce.redoc.ly/2.4.6-admin/tag/products#operation/PostV1Products){target="_blank"}
