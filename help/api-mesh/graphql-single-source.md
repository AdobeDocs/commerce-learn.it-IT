---
title: Creare una rete a sorgente singola GraphQL in Mesh API
description: Scopri come utilizzare API Mesh su Adobe Commerce e Adobe App Builder. Scopri come creare una rete con un’unica origine GraphQL e accedere al nuovo endpoint.
jira: KT-11804
doc-type: Tutorial
duration: 485
last-substantial-update: 2023-02-08T00:00:00.000Z
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Developer
level: Beginner
exl-id: 9a78457a-1539-49c0-ac69-4bbfc6786137
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: 72863f3c-9d27-5dda-afe1-d9f934b1fba0
    internal-label: Extensibility
  - id: b48dbafb-4193-5648-b9d9-bf96e9c9a411
    internal-label: Backend Development
  - id: c4f010fa-1478-4300-a88d-706fbc036a7a
    internal-label: APIs and SDKs
  - id: cc250cf1-34eb-4863-80d0-d170d45ea067
    internal-label: Developer tools
  - id: 125c1f49-aefd-5f34-a252-288937f95f6b
    internal-label: Marketing Tools
subfeature_v2:
  - id: ce84ce08-883f-4337-ae83-6bb1855ca732
    internal-label: API Mesh
  - id: a743e5dc-8f37-4b5d-a848-03c32ca30598
    internal-label: App Builder
  - id: 618ab558-d6ad-5352-99d6-d5702c6fdf80
    internal-label: Tools and External Services
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
    internal-label: Beginner
source-git-commit: 7516911fc14bc80f327310b99a00f9cbff7c65c7
workflow-type: tm+mt
source-wordcount: '216'
ht-degree: 0%
---
# Creare una trama con un&#39;unica origine

Questo video aiuta gli sviluppatori a comprendere come creare una mesh con un’unica sorgente in API Mesh per Adobe Developer App Builder. Affinché questo esempio di base funzioni, è necessario un endpoint API o GraphQL accessibile al pubblico. Nel video viene inoltre illustrato come creare un semplice file `mesh.json` da utilizzare con l&#39;istanza di Commerce. Per ulteriori dettagli ed esempi di codice, visita [Creare una rete](https://developer.adobe.com/graphql-mesh-gateway/mesh/basic/create-mesh){target="_blank"}.

## A chi serve questo video?

* Qualsiasi nuovo utente di API mesh
* Sviluppatori interessati a combinare più origini GraphQL e API
* Chiunque debba sapere come filtrare la scheda di rete e filtrare per GraphQL

## Contenuto video

* Utilizzo di API Mesh come proxy inverso
* Creazione di una trama da un file di configurazione JSON
* Accesso all’endpoint GraphQL appena creato

>[!VIDEO](https://video.tv.adobe.com/v/3414124?learn=on)

## Creare il file di configurazione JSON

API Mesh utilizza un file di configurazione JSON per definire i gestori di origine. Il file JSON contiene un array `sources` che contiene le origini per la rete. Di seguito è riportato un esempio di mesh con un&#39;unica origine.

```json
{
"meshConfig": {
    "sources": [
      {
        "name": "Commerce",
        "handler": {
          "graphql": {
            "endpoint": "https://venia.magento.com/graphql/"
          }
        }
      }
    ]
  }
}
```

{{$include /help/_includes/api-mesh-related-links.md}}
