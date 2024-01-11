---
title: Creare un prodotto bundle
description: Scopri come creare un prodotto bundle utilizzando l’API REST e l’amministratore di Commerce.
kt: 14589
doc-type: video
audience: all
activity: use
last-substantial-update: 2024-1-8
feature: Catalog Management, Admin Workspace, Backend Development, Integration, REST
topic: Commerce, Integrations, Content Management
role: Developer, User
level: Beginner
source-git-commit: e02540438df1cc85e6be7440351a72e77cfc1bf2
workflow-type: tm+mt
source-wordcount: '641'
ht-degree: 0%

---


# Creare un prodotto bundle

Un prodotto bundle è un modo per raggruppare diversi prodotti sotto un prodotto principale. Questi prodotti secondari possono essere un set definito di prodotti o offrire alcune varianti che forniscono opzioni di configurazione flessibili per i clienti. L&#39;impostazione dei tipi di prodotto del bundle richiede un po&#39; più di tempo ed è necessario fare una certa pianificazione prima di configurarli. Tuttavia, l&#39;offerta di prodotti in bundle migliora l&#39;esperienza di acquisto rendendo più facile per i clienti di personalizzare le loro selezioni di prodotti.

Ad esempio, puoi offrire un pacchetto di prodotti denominato `Learning to surf` nel tuo negozio web. Il bundle è il prodotto principale che funge da contenitore per i prodotti secondari assegnati che specificano le opzioni disponibili:

- Una tavola da surf standard
- Un tipico guinzaglio da tavola
- Pinne rosse da tavola da surf

Quando si desidera una flessibilità aggiuntiva, si consiglia di consentire diverse opzioni di prodotti secondari. Ciò richiede un uso più complesso delle opzioni e dei prodotti secondari. Per espandere l&#39;esempio precedente, le opzioni finali sono:

- Una tavola da surf standard
- Un tipico guinzaglio da tavola
- Scelta del colore della pinna:
   - Rosso
   - Blu
   - Giallo

Che il bundle sia un gruppo statico di prodotti semplici o diversi prodotti con varianti, le opzioni di configurazione flessibili rendono i tipi di prodotto bundle uno strumento di merchandising univoco e potente per lo store di Adobe Commerce.

Prima di creare un prodotto bundle, verifica che tutti i prodotti semplici da includere nel prodotto bundle siano disponibili in Adobe Commerce. Crea quelli che non esistono.

In questa esercitazione, scopri come creare un prodotto bundle utilizzando l’API REST e l’amministratore Adobe Commerce.

Utilizza l’API REST per definire un prodotto bundle:

1. Creare prodotti semplici da utilizzare nel prodotto bundle.
1. Crea un prodotto bundle e associa i prodotti semplici.
1. Ottieni il prodotto del bundle e tutte le opzioni associate.
1. Elimina un’opzione da una sezione del bundle prodotti.
1. Ripristina le opzioni per un prodotto bundle.

Quando crei i prodotti bundle da Adobe Commerce Admin, puoi creare prima i prodotti semplici o utilizzare lo strumento automatizzato per creare prodotti semplici utilizzando una procedura guidata.

## A chi serve questo video?

- Gestori di siti Web
- eCommerce merchandisers
- Nuovi sviluppatori di Adobe Commerce che desiderano imparare a creare prodotti bundle in Adobe Commerce utilizzando l’API REST

## Contenuto video

>[!VIDEO](https://video.tv.adobe.com/v/3426797?learn=on)

## Creare prodotti con REST

I seguenti comandi creano tutti i prodotti necessari per definire il prodotto del bundle in questo esempio: cinque prodotti semplici e un prodotto del bundle con tre opzioni.

Prima di inviare la richiesta, aggiorna l’esempio con i valori per il tuo ambiente.

- Cambia `"attribute-set": 4` da sostituire `4` con l’ID del set di attributi dal tuo ambiente.

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=41d94540f5bd8b691017a850bc3b82b3' \
--data '{
  "product": {
    "sku": "10-ft-beginner-surfboard",
    "name": "10 Foot Beginner surfboard",
    "attribute_set_id": 4,
    "price": 100,
    "type_id": "simple",
    "extension_attributes": {
      "stock_item": {
        "qty": 100,
        "is_in_stock":true
      }
    }
  }
}
'
```

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=41d94540f5bd8b691017a850bc3b82b3' \
--data '{
  "product": {
    "sku": "8-ft-surboard-leash",
    "name": "8 Foot surboard leash",
    "attribute_set_id": 4,
    "price": 50,
    "type_id": "simple",
    "extension_attributes": {
      "stock_item": {
        "qty": 100,
        "is_in_stock":true
      }
    }
  }
}
'
```

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=41d94540f5bd8b691017a850bc3b82b3' \
--data '{
  "product": {
    "sku": "red-fins-and-fin-plugs",
    "name": "Red fins and fin plugs",
    "attribute_set_id": 4,
    "price": 15,
    "type_id": "simple",
    "extension_attributes": {
      "stock_item": {
        "qty": 100,
        "is_in_stock":true
      }
    }
  }
}
'
```

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=41d94540f5bd8b691017a850bc3b82b3' \
--data '{
  "product": {
    "sku": "blue-fins-and-fin-plugs",
    "name": "Blue fins and fin plugs",
    "attribute_set_id": 4,
    "price": 15,
    "type_id": "simple",
    "extension_attributes": {
      "stock_item": {
        "qty": 100,
        "is_in_stock":true
      }
    }
  }
}
'
```

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=41d94540f5bd8b691017a850bc3b82b3' \
--data '{
  "product": {
    "sku": "yellow-fins-and-fin-plugs",
    "name": "Yellow fins and fin plugs",
    "attribute_set_id": 4,
    "price": 15,
    "type_id": "simple",
    "extension_attributes": {
      "stock_item": {
        "qty": 100,
        "is_in_stock":true
      }
    }
  }
}
'
```

## Creare un prodotto bundle e assegnare i prodotti semplici come opzioni

Crea un prodotto bundle inviando la seguente richiesta POST.

Prima di inviare la richiesta, aggiorna l’esempio con i valori per il tuo ambiente.

- Cambia `"attribute_set_id": 4,` e sostituisci `4` con l’id del set di attributi dall’ambiente.

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=41d94540f5bd8b691017a850bc3b82b3' \
--data '{
  "product": {
    "sku": "beginner-surfboard",
    "name": "Beginner Surfboard",
    "attribute_set_id": 4,
    "status": 1,
    "visibility": 4,
    "type_id": "bundle",
    "extension_attributes": {
      "stock_item": {
        "qty": 100,
        "is_in_stock":true
      },
      "bundle_product_options": [
        {
          "option_id": 0,
          "position": 1,
          "sku": "surfboard-essentials",
          "title": "Beginners 10ft Surfboard",
          "type": "checkbox",
          "required": true,
          "product_links": [
            {
              "sku": "10-ft-beginner-surfboard",
              "option_id": 1,
              "qty": 1,
              "position": 1,
              "is_default": false,
              "price": 100,
              "price_type": 0,
              "can_change_quantity": 0
            }
          ]
        },
        {
          "option_id": 1,
          "position": 2,
          "sku": "surboard-leash",
          "title": "Surfboard leash",
          "type": "checkbox",
          "required": true,
          "product_links": [
            {
              "sku": "8-ft-surboard-leash",
              "option_id": 2,
              "qty": 1,
              "position": 1,
              "is_default": false,
              "price": 50,
              "price_type": 0,
              "can_change_quantity": 0
            }
          ]
        },
        {
          "option_id": 3,
          "position": 2,
          "sku": "surfboard-color",
          "title": "Choose a color for the fins and fin plugs",
          "type": "radio",
          "required": true,
          "product_links": [
            {
              "sku": "red-fins-and-fin-plugs",
              "option_id": 2,
              "qty": 1,
              "position": 1,
              "is_default": false,
              "price": 0,
              "price_type": 0,
              "can_change_quantity": 0
            },
            {
              "sku": "blue-fins-and-fin-plugs",
              "option_id": 2,
              "qty": 1,
              "position": 2,
              "is_default": false,
              "price": 0,
              "price_type": 0,
              "can_change_quantity": 0
            },
            {
              "sku": "yellow-fins-and-fin-plugs",
              "option_id": 3,
              "qty": 1,
              "position": 3,
              "is_default": false,
              "price": 0,
              "price_type": 0,
              "can_change_quantity": 0
            }
          ]
        }
      ]
    },
    "custom_attributes": [
      {
        "attribute_code": "price_view",
        "value": "0"
      }
    ]
  },
  "saveOptions": true
}
'
```

## Eliminare un’opzione da un prodotto bundle

Rimuovi un prodotto secondario da un prodotto bundle senza eliminare il prodotto dal catalogo inviando la seguente richiesta DELETE utilizzando cURL.

```bash
curl --location --request DELETE '{{your.url.here}}/rest/default/V1/bundle-products/beginner-surfboard/options/35/children/blue-fins-and-fin-plugs' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=41d94540f5bd8b691017a850bc3b82b3'
```

## Ripristina opzioni prodotto

Quando aggiorni le opzioni del prodotto bundle, assicurati di includere tutte le opzioni che desideri associare a questo prodotto. Se il set di opzioni originale conteneva tre prodotti ed uno è stato rimosso, includi tutte e tre le opzioni nella richiesta POST per garantire che il bundle di prodotto specifichi tutte le opzioni. Se hai incluso solo l’opzione rimossa, il pacchetto di prodotti aggiornato includerà solo tale opzione.

Individua l’ID opzione esaminando la risposta dalla creazione per il prodotto bundle. In tale risposta, la Commissione `option_id` è `35`.

```json
...
,
            {
                "option_id": 35,
                "title": "added back color options for fins and fin plugs",
                "required": true,
                "type": "radio",
                "position": 2,
                "sku": "beginner-surfboard",
                "product_links": [
                    {
                        "id": "77",
                        "sku": "red-fins-and-fin-plugs",
                        "option_id": 35,
                        "qty": 1,
                        "position": 1,
                        "is_default": false,
                        "price": 15,
                        "price_type": null,
                        "can_change_quantity": 0
                    },
                    {
                        "id": "78",
                        "sku": "blue-fins-and-fin-plugs",
                        "option_id": 35,
                        "qty": 1,
                        "position": 2,
                        "is_default": false,
                        "price": 15,
                        "price_type": null,
                        "can_change_quantity": 0
                    },
                    {
                        "id": "79",
                        "sku": "yellow-fins-and-fin-plugs",
                        "option_id": 35,
                        "qty": 1,
                        "position": 3,
                        "is_default": false,
                        "price": 15,
                        "price_type": null,
                        "can_change_quantity": 0
                    }
                ]
            }
...
```

Aggiorna il pacchetto di prodotti per aggiungere l’opzione rimossa inviando la seguente richiesta POST.

```bash
curl --location '{{your.url.here}}/rest/default/V1/bundle-products/options/add' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=41d94540f5bd8b691017a850bc3b82b3' \
--data '{
  "option": {
    "option_id": 35,
    "title": "Choose a color for the fins and fin plugs",
    "required": true,
    "type": "radio",
    "position": 2,
    "sku": "beginner-surfboard",
    "product_links": [
      {
        "sku": "red-fins-and-fin-plugs",
        "option_id": 35,
        "qty": 1,
        "position": 1,
        "is_default": false,
        "price": 0,
        "price_type": null,
        "can_change_quantity": 0,
        "extension_attributes": {}
      },
      {
        "sku": "blue-fins-and-fin-plugs",
        "option_id": 35,
        "qty": 1,
        "position": 2,
        "is_default": false,
        "price": 0,
        "price_type": null,
        "can_change_quantity": 0,
        "extension_attributes": {}
      },
      {
        "sku": "yellow-fins-and-fin-plugs",
        "option_id": 35,
        "qty": 1,
        "position": 3,
        "is_default": false,
        "price": 0,
        "price_type": null,
        "can_change_quantity": 0,
        "extension_attributes": {}
      }
    ],
    "extension_attributes": {}
  }
}'
```

## Risorse aggiuntive

- [Tutorial su come creare un bundle](https://developer.adobe.com/commerce/webapi/rest/tutorials/bundle-product/){target="_blank"}
- [Prodotto bundle](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/types/product-create-bundle.html){target="_blank"}
- [Tutorial REST per Adobe Developer](https://developer.adobe.com/commerce/webapi/rest/tutorials/prerequisite-tasks/){target="_blank"}
- [Adobe Commerce REST ReDoc](https://adobe-commerce.redoc.ly/2.4.6-admin/tag/products#operation/PostV1Products){target="_blank"}
