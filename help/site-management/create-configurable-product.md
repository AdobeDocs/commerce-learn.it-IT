---
title: Creare un prodotto configurabile
description: Scopri come creare un prodotto configurabile utilizzando l’API REST e l’amministratore di Commerce.
kt: 14586
doc-type: video
audience: all
activity: use
last-substantial-update: 2023-12-15T00:00:00Z
feature: Catalog Management, Admin Workspace, Backend Development, Integration, REST
topic: Commerce, Integrations, Content Management
role: Developer, User
level: Beginner
source-git-commit: f3ec375c2332bfae98970d7e10a6a7ad258386e3
workflow-type: tm+mt
source-wordcount: '952'
ht-degree: 0%

---


# Creare un prodotto configurabile

Un prodotto configurabile è un prodotto principale di più prodotti semplici. Definisci un prodotto configurabile per richiedere all’acquirente di effettuare una o più scelte per selezionare una variante di prodotto specifica. Ad esempio, se il prodotto è una camicia, l&#39;acquirente deve scegliere le opzioni di dimensione e colore per selezionare la camicia.

Sebbene un prodotto configurabile utilizzi più SKU e la configurazione iniziale possa richiedere un po’ di tempo, alla fine potrai risparmiare tempo. Se prevedi di espandere la tua attività, il tipo di prodotto configurabile è una buona scelta per i prodotti con più opzioni.

Prima di creare un prodotto configurabile, verifica che tutti i prodotti semplici da includere nel prodotto configurabile siano disponibili in Adobe Commerce. Crea quelli che non esistono.

In questo tutorial, scopri come creare un prodotto configurabile utilizzando l’API REST e l’amministratore Adobe Commerce.

Utilizza l’API REST per creare un prodotto configurabile:

1. Ottieni gli attributi per un [set di attributi](https://experienceleague.adobe.com/docs/commerce-admin/catalog/product-attributes/create/attribute-sets.html) per utilizzare i numeri ID per le chiamate API successive.
1. Crea prodotti semplici da utilizzare nel prodotto configurabile.
1. Crea un prodotto configurabile vuoto e associa i prodotti semplici.
1. Imposta gli attributi del prodotto configurabile.
1. Popola il prodotto configurabile vuoto con prodotti semplici.
1. Ottieni il prodotto configurabile e tutti gli attributi.
1. Ottieni i prodotti secondari assegnati per il prodotto configurabile.
1. Elimina l’associazione di prodotti semplici a prodotti configurabili.

Quando crei prodotti configurabili dall’amministratore di Adobe Commerce, puoi creare prima i prodotti semplici o utilizzare lo strumento automatizzato che crea nuovi prodotti semplici da utilizzare mediante la procedura guidata.

## A chi serve questo video?

- Gestori di siti Web
- eCommerce merchandisers
- Nuovi sviluppatori Adobe Commerce che desiderano imparare a creare prodotti configurabili in Adobe Commerce utilizzando l’API REST

## Contenuto video

>[!VIDEO](https://video.tv.adobe.com/v/3426381?learn=on)

## Ottieni gli attributi del colore utilizzando cURL

In questo esempio, per il set di attributi 10 viene restituito l&#39;intero set di attributi con tutti i singoli attributi. Può essere lunga, centinaia di linee non sono rare. Durante l’esame della risposta, l’ID attributo del colore sarà probabilmente al centro. Accelerare la ricerca di questi valori utilizzando grep o altri metodi per cercare i risultati. La mia risposta era vicina alla riga 665 ed è inclusa nel seguente snippet della risposta JSON.

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


Per recuperare gli ID attributo per configurare il prodotto configurabile, aggiorna il `attribute-sets/10/attributes` parte della seguente richiesta cURL da sostituire `10` con l’ID del set di attributi nell’ambiente. Questa richiesta utilizza il metodo GET.

```bash
curl --location '{{your.url.here}}rest/V1/products/attribute-sets/10/attributes' \
--header 'Authorization: Bearer {{Your Bearer Token}}'
```

## Creare il primo prodotto semplice utilizzando cURL

### Regolare gli ID ambiente e i dettagli del prodotto

Crea il primo prodotto semplice utilizzando l’API per inviare la seguente richiesta POST utilizzando cURL.

Prima di inviare la richiesta, aggiorna l’esempio con i valori per il tuo ambiente.

- Cambia `"attribute-set": 10` da sostituire `10` con l’ID del set di attributi dal tuo ambiente.
- Cambia `"value": "13"` da sostituire `13` con il valore dell’ambiente.

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

## Creare il secondo prodotto semplice utilizzando cURL

Crea il secondo prodotto semplice utilizzando l’API per inviare la seguente richiesta POST utilizzando cURL.

Prima di inviare la richiesta, aggiorna l’esempio con i valori per il tuo ambiente.

- Cambia `"attribute_set_id": 10,` e sostituisci `10` con l’id del set di attributi da nell’ambiente.
- Cambia `"value": "14"` e sostituisci `14` con il valore dell’ambiente.

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

## Creare il terzo prodotto semplice utilizzando cURL

Crea il terzo prodotto semplice inviando la seguente richiesta POST utilizzando cURL.

Prima di inviare la richiesta, aggiorna l’esempio con i valori per il tuo ambiente.

- Cambia `"attribute_set_id": 10,` da sostituire `10` con l’ID del set di attributi dal tuo ambiente.
- Cambia `"value": "15"` e sostituisci `15` con il valore dell’ambiente.

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

## Crea un prodotto configurabile vuoto utilizzando cURL

Crea un prodotto configurabile vuoto inviando la seguente richiesta POST utilizzando cURL.

Prima di inviare la richiesta, aggiorna l’esempio con i valori per il tuo ambiente.

- Cambia `"attribute_set_id": 10,` e sostituisci `10` con l’id del set di attributi dall’ambiente.
- Cambia `"value": "93"` e sostituisci `93` con il valore dell’ambiente.

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

## Imposta le opzioni disponibili per il prodotto configurabile

Imposta le opzioni disponibili per il prodotto configurabile inviando la seguente richiesta POST utilizzando cURL.

Prima di inviare la richiesta, modifica `"attribute_id": 93,` da sostituire `93` con l’id attributo del tuo ambiente.

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

Se si dimentica di impostare le opzioni per il prodotto configurabile (padre), viene visualizzato un errore quando si tenta di associare un prodotto figlio al prodotto configurabile. Il messaggio di errore è simile al seguente esempio:

`{"message":"The parent product doesn't have configurable product options.","trace":"#0 [internal function]: Magento\\ConfigurableProduct\\Model\\LinkManagement->addChild('Kids-Hawaiian-U...'}`

## Collega il prodotto secondario al configurabile

Ora hai creato tre semplici prodotti:

- `"Kids Hawaiian Ukulele Red"`,
- `"Kids-Hawaiian-Ukulele-Blue"`
- `"Kids-Hawaiian-Ukulele-Green"`

Aggiungi questi prodotti semplici come elementi secondari del prodotto configurabile inviando la seguente richiesta POST. Invia una richiesta separata per ciascun prodotto.

Per ogni richiesta, aggiorna la `childSKU` valore con il valore del prodotto secondario che stai aggiungendo. L’esempio seguente assegna il prodotto semplice `kids-Hawaiian-Ukulele-red` al prodotto configurabile con SKU `Kids-Hawaiian-Ukulele-red`.


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

## Ottieni un prodotto configurabile utilizzando cURL

Ora che hai creato un prodotto configurabile con tre SKU secondarie assegnate. Puoi visualizzare gli ID collegati per i prodotti assegnati inviando la seguente richiesta GET utilizzando cURL. Questa richiesta restituisce informazioni dettagliate sul prodotto configurabile.

```json
...
        "configurable_product_links": [
            155,
            157,
            156
        ]
...
```

Di seguito viene utilizzato il metodo GET

```bash
curl --location '{{your.url.here}}/rest/default/V1/products/Kids-Hawaiian-Ukulele' \
--header 'Authorization: Bearer {{Your Bearer Token}}'
```

## Ottieni il prodotto secondario associato a un prodotto configurabile

Restituisci solo gli elementi figlio associati al prodotto configurabile inviando la seguente richiesta GET. La risposta includerà tutti gli attributi del prodotto secondario, inclusi SKU e prezzo.

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

- [Crea un tutorial di prodotto configurabile](https://developer.adobe.com/commerce/webapi/rest/tutorials/configurable-product/){target="_blank"}
- [Prodotto configurabile](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/types/product-create-configurable.html){target="_blank"}
- [Tutorial REST per Adobe Developer](https://developer.adobe.com/commerce/webapi/rest/tutorials/prerequisite-tasks/){target="_blank"}
- [Adobe Commerce REST ReDoc](https://adobe-commerce.redoc.ly/2.4.6-admin/tag/products#operation/PostV1Products){target="_blank"}
