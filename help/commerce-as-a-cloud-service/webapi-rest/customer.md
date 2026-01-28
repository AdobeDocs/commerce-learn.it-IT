---
title: Esplora le nuove API REST del cliente
description: Scopri come utilizzare le nuove API REST dei clienti in Adobe Commerce Cloud Service. Ideale per architetti e sviluppatori.
feature: REST, Customers, Saas
topic: Development, Integrations
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 225
last-substantial-update: 2026-01-27T00:00:00Z
jira: KT-20160
source-git-commit: cb3fecf5f8b23425311dc31ed526b3b9bfe07b45
workflow-type: tm+mt
source-wordcount: '470'
ht-degree: 0%

---


# API REST del cliente

Scopri come utilizzare le nuove API REST dei clienti in Adobe Commerce as a Cloud Service. Questo tutorial è perfetto per architetti e sviluppatori che desiderano integrare e ottimizzare in modo efficace le soluzioni API.

## A chi serve questo video?

* Sviluppatori back-end responsabili della creazione di integrazioni con Adobe Commerce
* Architetti tecnici che progettano flussi di lavoro di gestione dei clienti per implementazioni commerce headless

## Contenuto video

* Esegui l’autenticazione con Adobe IMS utilizzando le credenziali server-to-server per ottenere un token di accesso per le richieste API
* Utilizza il formato dell’endpoint REST API corretto per Commerce as a Cloud Service
* Creare e aggiornare gli account cliente a livello di programmazione utilizzando le richieste POST e PUT con payload JSON appropriati

>[!VIDEO](https://video.tv.adobe.com/v/3479370/?captions=ita&learn=on&enablevpops)

## Esempi di codice

Prima di iniziare, raccogliere tutti i valori richiesti da [Experience Cloud](https://experience.adobe.com) e [Adobe Developer Console](https://developer.adobe.com/console). La predisposizione di questi valori garantisce un processo di configurazione fluido.

>[!NOTE]
>
>Assicurati di lavorare nell’organizzazione corretta. La selezione dell’organizzazione influisce sulle istanze e sugli ambienti visibili in Experience Cloud e Developer Console.

### Dettagli istanza - experience.adobe.com

I dettagli dell’istanza contengono elementi come l’ID istanza, gli endpoint GraphQL e le credenziali.

### Dettagli sviluppatore - https://developer.adobe.com/console/

In Developer Console puoi gestire le credenziali API, inclusi ID client, segreti client e token di accesso. È inoltre possibile creare nuovi tipi di credenziali, ad esempio l&#39;autenticazione da server a server o da app nativa.

## Prerequisiti

| Elemento | Valore | Dove è questo valore |
|--- |--- |--- |
| ID istanza | `<instance_id>` | experience.adobe.com |
| Endpoint REST | `<rest_endpoint>` | experience.adobe.com |
| ID client | `<client_id>` | developer.adobe.com/console |
| Segreto client | `<client_secret>` | developer.adobe.com/console |


## Passaggio 1: ottenere il token di accesso (autenticazione server-to-server)

>[!IMPORTANT]
>
> Le variabili mostrate in questo esempio non sono valide. Utilizza &lt;client_id> e &lt;client_secret> dalle credenziali del progetto.

```bash
curl -X POST 'https://ims-na1.adobelogin.com/ims/token/v3' \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -d 'grant_type=client_credentials&client_id=<client_id>&client_secret=<client_secret>&scope=openid,AdobeID,email,additional_info.projectedProductContext,profile,commerce.aco.ingestion,commerce.accs,org.read,additional_info.roles'
```

**Risposta di esempio:**

```json
{
  "access_token": "eyJhbGciOiJSUzI1NiIs...",
  "token_type": "bearer",
  "expires_in": 86399
}
```

## Passaggio 2: creare un cliente

>[!IMPORTANT]
>
> L&#39;URL fornito in questo esempio non è valido. Utilizza l’URL di base REST. Scambia &quot;&lt;rest_endpoint>&quot; con il tuo URL. È simile a `https://na1-sandbox.api.commerce.adobe.com/AbCYab34cdEfGHiJ27123`.
>
> Questo endpoint non ha /rest/ come parte dell’URL. L’inclusione di porta a un errore.

**Endpoint:** `POST /V1/customers`

```bash
curl -X POST \
  "<rest_endpoint>/V1/customers" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer <ACCESS_TOKEN>" \
  -d '{
    "customer": {
      "email": "john.doe@example.com",
      "firstname": "John",
      "lastname": "Doe",
      "store_id": 1,
      "website_id": 1
    },
    "password": "TempPa55word!"
  }'
```

**Risposta:**

```json
{
  "id": 5,
  "group_id": 1,
  "created_at": "2026-01-23 20:40:15",
  "updated_at": "2026-01-23 20:40:15",
  "created_in": "Default Store View",
  "email": "john.doe@example.com",
  "firstname": "John",
  "lastname": "Doe",
  "store_id": 1,
  "website_id": 1,
  "addresses": [],
  "disable_auto_group_change": 0
}
```

## Passaggio 3: aggiornare un cliente

>[!IMPORTANT]
>
> L&#39;URL fornito in questo esempio non è valido. Utilizza l’URL di base REST. Scambia &quot;&lt;rest_endpoint>&quot; con il tuo URL. È simile a `https://na1-sandbox.api.commerce.adobe.com/AbCYab34cdEfGHiJ27123`.

Il numero `5` nell&#39;esempio seguente è l&#39;ID del cliente creato in precedenza utilizzando POST `"id": 5,`. Assicurati di cambiare `5` con qualsiasi ID restituito nella richiesta.

**Endpoint:** `PUT /V1/customers/{customerId}`

```bash
curl -X PUT \
  "<rest_endpoint>/V1/customers/5" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer <ACCESS_TOKEN>" \
  -d '{
    "customer": {
      "id": 5,
      "email": "john.doe@example.com",
      "firstname": "John",
      "lastname": "Doe-Updated"
    }
  }'
```

**Risposta:**

```json
{
  "id": 5,
  "group_id": 1,
  "created_at": "2026-01-23 20:40:15",
  "updated_at": "2026-01-23 20:40:30",
  "created_in": "Default Store View",
  "email": "john.doe@example.com",
  "firstname": "John",
  "lastname": "Doe-Updated",
  "store_id": 1,
  "website_id": 1,
  "addresses": []
}
```

## Script completo (All-in-One)

>[!IMPORTANT]
>
> Le variabili mostrate in questo esempio non sono valide. Utilizza l’ID client e il segreto client dalle credenziali del progetto. Utilizza l’URL di base REST. Scambia &quot;&lt;rest_endpoint>&quot; con l’URL dell’endpoint REST da experience.adobe.com. È simile a `https://na1-sandbox.api.commerce.adobe.com/AbCDefGHiJ1234567`.

```bash
#!/bin/bash

# Configuration be sure to update these with your projects unique values
CLIENT_ID="<client_id>"
CLIENT_SECRET="<client_secret>"
REST_ENDPOINT="<rest_endpoint>"

# Step 1: Get Access Token
echo "Getting access token..."
ACCESS_TOKEN=$(curl -s -X POST 'https://ims-na1.adobelogin.com/ims/token/v3' \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -d "grant_type=client_credentials&client_id=${CLIENT_ID}&client_secret=${CLIENT_SECRET}&scope=openid,AdobeID,email,additional_info.projectedProductContext,profile,commerce.aco.ingestion,commerce.accs,org.read,additional_info.roles" | jq -r '.access_token')

echo "Token obtained: ${ACCESS_TOKEN:0:50}..."

# Step 2: Create Customer
echo ""
echo "Creating customer..."
CREATE_RESPONSE=$(curl -s -X POST \
  "${REST_ENDPOINT}/V1/customers" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer ${ACCESS_TOKEN}" \
  -d '{
    "customer": {
      "email": "john.doe@example.com",
      "firstname": "John",
      "lastname": "Doe",
      "store_id": 1,
      "website_id": 1
    },
    "password": "TempPa55word!"
  }')

echo "Create Response:"
echo "$CREATE_RESPONSE" | jq .

# Extract customer ID
CUSTOMER_ID=$(echo "$CREATE_RESPONSE" | jq -r '.id')
echo "Customer ID: $CUSTOMER_ID"

# Step 3: Update Customer
echo ""
echo "Updating customer..."
curl -s -X PUT \
  "${REST_ENDPOINT}/V1/customers/${CUSTOMER_ID}" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer ${ACCESS_TOKEN}" \
  -d "{
    \"customer\": {
      \"id\": ${CUSTOMER_ID},
      \"email\": \"john.doe@example.com\",
      \"firstname\": \"john\",
      \"lastname\": \"Doe-Updated\"
    }
  }" | jq .
```

## Note importanti su questa esercitazione

1. **Percorso URL**: Usa `https://<server>.api.commerce.adobe.com/<tenant-id>/V1/customers` - **NOT** `https://<host>/rest/<store-view-code>/V1/customers`
1. **Autenticazione**: questo tutorial ha utilizzato il tipo di concessione Server-to-Server (`client_credentials`)
1. **Ambito richiesto**: `commerce.accs`
1. **Scadenza token**: 86400 secondi (24 ore)

## Riferimenti

* [Note sulla versione di Adobe Commerce as a Cloud Service](https://experienceleague.adobe.com/it/docs/commerce/cloud-service/release-notes)
* [Riferimento API REST SaaS](https://developer.adobe.com/commerce/webapi/reference/rest/saas/)
* [Guida all&#39;autenticazione utente](https://developer.adobe.com/commerce/webapi/rest/authentication/user/)
