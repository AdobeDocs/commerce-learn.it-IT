---
title: Creare un prodotto scaricabile
description: Scopri come creare un prodotto scaricabile utilizzando l’API REST e l’amministratore Adobe Commerce.
kt: 14464
doc-type: video
audience: all
activity: use
last-substantial-update: 2023-11-16T00:00:00Z
feature: Catalog Management, Admin Workspace, Backend Development, Integration, REST
topic: Commerce, Integrations, Content Management
role: Developer, User
level: Beginner
exl-id: 90753b8d-eca0-4868-b40e-9563d2b0e1e8
source-git-commit: eba043cd4169cd762653557bf9283b8d6a208ef0
workflow-type: tm+mt
source-wordcount: '584'
ht-degree: 0%

---

# Creare un prodotto scaricabile

Scopri come creare un prodotto scaricabile utilizzando l’API REST e l’amministratore Adobe Commerce.

## A chi serve questo video?

- Gestori di siti Web
- eCommerce merchandisers
- Nuovi sviluppatori di Adobe Commerce che desiderano imparare a creare prodotti in Adobe Commerce utilizzando l’API REST

## Contenuto video

>[!VIDEO](https://video.tv.adobe.com/v/3453953?learn=on&captions=ita)

## Domini scaricabili consentiti

È necessario specificare quali domini sono consentiti per consentire i download. I domini vengono aggiunti al file `env.php` del progetto tramite la riga di comando. Il file `env.php` descrive i domini che possono contenere contenuto scaricabile. Si verifica un errore se viene creato un prodotto scaricabile utilizzando l&#39;API REST _prima_ dell&#39;aggiornamento del file `php.env`:

```bash
{
    "message": "Link URL's domain is not in list of downloadable_domains in env.php."
}
```

Per impostare il dominio, connettersi al server: `bin/magento downloadable:domains:add www.example.com`

Una volta completato, `env.php` viene modificato all&#39;interno dell&#39;array _downloadable_domains_.

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

Questo esempio mostra come creare un prodotto scaricabile utilizzando cURL quando il file da scaricare non si trova sullo stesso server. Questo caso d’uso è tipico se il file viene memorizzato in un bucket S3 o in un altro gestore di risorse digitali.

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

## Creare un prodotto scaricabile utilizzando cURL (download dal server applicazioni Commerce)

Questo esempio illustra come utilizzare cURL per creare un prodotto scaricabile dall’amministratore di Adobe Commerce quando il file viene memorizzato sullo stesso server dell’applicazione Adobe Commerce.

In questo caso d&#39;uso, quando l&#39;amministratore che gestisce il catalogo sceglie `upload file`, il file viene trasferito alla directory `pub/media/downloadable/files/links/`.  L’automazione crea e sposta i file nelle rispettive posizioni in base al seguente pattern:

- Ogni file caricato viene memorizzato in una cartella in base ai primi due caratteri del nome del file.
- Quando il caricamento viene avviato, l&#39;applicazione Commerce crea o utilizza cartelle esistenti per trasferire il file.
- Durante il download del file, la sezione `link_file` del percorso utilizza la parte del percorso aggiunta alla directory `pub/media/downloadable/files/links/`.

Ad esempio, se il file caricato è denominato `download-example.zip`:

- Il file è stato caricato nel percorso `pub/media/downloadable/files/links/d/o/`.
Le sottodirectory `/d` e `/d/o` vengono create se non esistono.

- Il percorso finale del file è `/pub/media/downloadable/files/links/d/o/download-example.zip`.

- Il valore `link_url` per questo esempio è `d/o/download-example.zip`

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

## Aggiornare il prodotto utilizzando Postman {#update-downloadable-links}

Usa l&#39;endpoint `rest/all/V1/products/{sku}/downloadable-links`
`SKU` è l&#39;ID prodotto generato al momento della creazione del prodotto. Ad esempio, nell’esempio di codice seguente, è il numero 39, ma assicurati che sia aggiornato per utilizzare l’ID dal tuo sito web. Questo aggiorna i collegamenti per i prodotti scaricabili.

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

## Aggiornare un collegamento per il download di prodotti utilizzando CURL

Quando aggiorni un collegamento per il download di un prodotto utilizzando cURL, l’URL include lo SKU del prodotto che viene aggiornato.  Nell&#39;esempio di codice seguente, lo SKU è `abcd12345`. Quando invii il comando, modifica il valore in modo che corrisponda allo SKU del prodotto che desideri aggiornare.

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

- [Tipo di prodotto scaricabile](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/types/product-create-downloadable.html?lang=it){target="_blank"}
- [Guida alla configurazione dei domini scaricabili](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/config-reference-envphp.html?lang=it#downloadable_domains){target="_blank"}
- [Tutorial REST di Adobe Developer](https://developer.adobe.com/commerce/webapi/rest/tutorials/prerequisite-tasks/){target="_blank"}
- [RiDoc REST Adobe Commerce](https://adobe-commerce.redoc.ly/2.4.6-admin/tag/products#operation/PostV1Products){target="_blank"}
