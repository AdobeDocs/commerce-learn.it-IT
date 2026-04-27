---
title: Creare un prodotto scaricabile
description: Scopri come creare un prodotto scaricabile utilizzando l’API REST e l’amministratore Adobe Commerce.
kt: 14464
doc-type: video
duration: 946
audience: all
activity: use
last-substantial-update: 2023-11-16T00:00:00.000Z
feature: Catalog Management, Admin Workspace, Backend Development, Integration, REST
topic: Commerce, Integrations, Content Management
role: Developer, User
level: Beginner
exl-id: 90753b8d-eca0-4868-b40e-9563d2b0e1e8
TQID: https://experienceleague.adobe.com/YHtAD-NRQmIG58myhZk9X7-jJjwlk8S4NX9jYnZwnQc
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
  - id: c18ed297-2187-4aec-affb-9d9654eca6fc
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2:
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 631
ht-degree: 0%

---

# Creare un prodotto scaricabile

Scopri come creare un prodotto scaricabile utilizzando l’API REST e l’amministratore Adobe Commerce.

## A chi serve questo video?

* Gestori di siti Web
* eCommerce merchandisers
* Nuovi sviluppatori di Adobe Commerce che desiderano imparare a creare prodotti in Adobe Commerce utilizzando l’API REST

## Contenuto video

>[!VIDEO](https://video.tv.adobe.com/v/3453953?captions=ita&learn=on)

## Domini scaricabili consentiti

È necessario specificare quali domini sono consentiti per consentire i download. I domini vengono aggiunti al file `env.php` del progetto tramite la riga di comando. Il file `env.php` descrive i domini che possono contenere contenuto scaricabile. Si verifica un errore se viene creato un prodotto scaricabile utilizzando l&#39;API REST _prima_ dell&#39;aggiornamento del file `php.env`:

```bash
{
    "message": "Link URL's domain is not in list of downloadable_domains in env.php."
}
```

Per impostare il dominio, connettersi al server: `bin/magento downloadable:domains:add www.example.com`

Una volta completato, `env.php` viene modificato all&#39;interno dell&#39;array _downloadable_ domains_.

```php
    'downloadable_domains' => [
        'www.example.com'
    ],
```

Ora che il dominio è stato aggiunto a `env.php`, è possibile creare un prodotto scaricabile in Adobe Commerce Admin o utilizzando l&#39;API REST.

Per ulteriori informazioni, consulta [Riferimento configurazione](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/config-reference-envphp.html?lang=it#downloadable_domains).

>[!IMPORTANT]
>In alcune versioni di Adobe Commerce, quando un prodotto viene modificato in Adobe Commerce Admin, potrebbe venire visualizzato il seguente errore. Il prodotto viene creato utilizzando l’API REST, ma il download collegato ha un prezzo `null`.

`Deprecated Functionality: number_format(): Passing null to parameter #1 ($num) of type float is deprecated in /app/vendor/magento/module-downloadable/Ui/DataProvider/Product/Form/Modifier/Data/Links.php on line 228`.

Per correggere l&#39;errore, utilizzare l&#39;API del collegamento di aggiornamento: `POST V1/products/{sku}/downloadable-links.`

Per ulteriori informazioni, consulta la sezione [Aggiornare un collegamento di download del prodotto utilizzando cURL](#update-downloadable-links).

## Creare un prodotto scaricabile utilizzando cURL (download dal server remoto)

Questo esempio mostra come creare un prodotto scaricabile utilizzando cURL quando il file da scaricare non si trova sullo stesso server. This use case is typical if the file is stored in an S3 bucket or other digital asset manager.

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=b78cae2338f12d846d233d4e9486607e; private_content_version=564dde2976849891583a9a649073f01' \
--data '{
  "product": {
    "sku": "POSTMAN-download-product-1",
    "name": "POSTMAN download product-1",
    "attribute_set_id": 4,
    "price": 9.9,
    "status": 1,
    "visibility": 4,
    "type_id": "downloadable",
    "extension_attributes": {
        "website_ids": [
            1
        ],
        
        "downloadable_product_links": [
            {
                "title": "My url link",
                "sort_order": 1,
                "is_shareable": 1,
                "price": 0,
                "number_of_downloads": 0,
                "link_type": "url",
                "link_url": "{{location.url.where.file.exists}}/some-file.zip",
                "sample_type": "url",
                "sample_url": "{{location.url.where.file.exists}}/sample-file.zip"
            }
        ],
        "downloadable_product_samples": []
    },
    "product_links": [],
    "options": [],
    "media_gallery_entries": [],
    "tier_prices": []
  }
}
'
```

## Create a downloadable product using cURL (download from Commerce application server)

This example demonstrates how to use cURL to create a downloadable product from the Adobe Commerce Admin when the file is stored on the same server as the Adobe Commerce application.

In this use case, when the administrator managing the catalog chooses `upload file`, the file is transferred to the `pub/media/downloadable/files/links/` directory.  Automation creates and moves the files to their respective locations based on the following pattern:

* Each uploaded file is stored in a folder based on the first two characters of the file name.
* When the upload is initiated, the Commerce application creates or uses existing folders to transfer the file.
* When downloading the file, the `link_file` section of the path uses the portion of the path appended to the `pub/media/downloadable/files/links/` directory.

For example, if the uploaded file is named `download-example.zip`:

* The file is uploaded to the path `pub/media/downloadable/files/links/d/o/`.
The subdirectories `/d` and `/d/o` are created if they do not exist.

* The final path to the file is `/pub/media/downloadable/files/links/d/o/download-example.zip`.

* The `link_url` value for this example is `d/o/download-example.zip`

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=571f5ebe48857569cd56bde5d65f83a2; private_content_version=9f3286b427812be6aec0b04cae33ba35' \
--data '{
  "product": {
    "sku": "sample-local-download-file",
    "name": "A downloadable product with locally hosted download file",
    "attribute_set_id": 4,
    "price": 33,
    "status": 1,
    "visibility": 4,
    "type_id": "downloadable",
    "extension_attributes": {
        "website_ids": [
            1
        ],
        "downloadable_product_links": [
            {
                "title": "an api version of already upload file",
                "sort_order": 1,
                "is_shareable": 1,
                "price": 0,
                "number_of_downloads": 0,
                "link_type": "file",
                "link_file": "/d/o/downloadable-file-demo-file-upload.zip",
                "sample_type": null
            }
        ],
        "downloadable_product_samples": []
    },
    "product_links": [],
    "options": [],
    "media_gallery_entries": [],
    "tier_prices": []
  }
}'
```

## Ottieni un prodotto utilizzando CURL

```bash
curl --location '{{your.url.here}}/rest/default/V1/products/POSTMAN-download-product-1' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=b78cae2338f12d846d233d4e9486607e; private_content_version=564dde2976849891583a9a649073f01e'
```

## Update the product using Postman {#update-downloadable-links}

Use the endpoint `rest/all/V1/products/{sku}/downloadable-links`
The `SKU` is the product ID that was generated when the product was created. For example in the code sample below, it is the number 39, but make sure it is updated to use the ID from your website. This updates the links for the downloadable products.

```json
{
  "link": {
    "id": 39,
    "title": "My swagger update",
    "sort_order": 0,
    "is_shareable": 0,
    "price": 0,
    "number_of_downloads": 0,
    "link_type": "url",
    "link_file": "{{your.url.here}}/some-file.zip",
    "link_url": "{{your.url.here}}/some-file.zip",
    "link_file_content": {
      "file_data": "{{your.url.here}}/some-file.zip",
      "extension_attributes": {}
    }
  },
  "isGlobalScopeContent": true
}
```

## Update a product download link using CURL

When you update a product download link using cURL, the URL includes the SKU for the product that is being updated.  In the following code example, the SKU is `abcd12345`. When you submit the command, change value to match the SKU for the product you want to update.

```bash
curl --location '{{your.url.here}}/rest/all/V1/products/abcd12345/downloadable-links' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=fa5d76f4568982adf369f758e8cb9544; private_content_version=564dde2976849891583a9a649073f01e' \
--data '{
  "link": {
    "id": {{The ID of the download link for example 39}},
    "title": "My update",
    "sort_order": 0,
    "is_shareable": 0,
    "price": 0,
    "number_of_downloads": 0,
    "link_type": "url",
    "link_file": "{{your.url.here}}/some-file.zip",
    "link_url": "{{your.url.here}}/some-file.zip",
    "link_file_content": {
      "file_data": "{{your.url.here}}/some-file.zip",
      "extension_attributes": {}
    }
  },
  "isGlobalScopeContent": true
}'
```

## Risorse aggiuntive

* [Downloadable Product Type](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/types/product-create-downloadable.html?lang=it){target="_blank"}
* [Downloadable Domains Configuration Guide](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/config-reference-envphp.html?lang=it#downloadable_domains){target="_blank"}
* [Tutorial REST per Adobe Developer](https://developer.adobe.com/commerce/webapi/rest/tutorials/prerequisite-tasks/){target="_blank"}
* [Adobe Commerce REST ReDoc](https://adobe-commerce.redoc.ly/2.4.6-admin/tag/products#operation/PostV1Products){target="_blank"}
