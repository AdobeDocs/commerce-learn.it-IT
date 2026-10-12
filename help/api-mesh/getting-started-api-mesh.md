---
title: Introduzione a Mesh API
description: Scopri come utilizzare Mesh API su Adobe Commerce e Adobe App Builder, inclusa l’installazione di App Builder, l’utilizzo dei progetti e la creazione di un proxy inverso.
jira: KT-11802
doc-type: Tutorial
duration: 422
last-substantial-update: 2023-08-27T00:00:00.000Z
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Developer
level: Intermediate
exl-id: baae6dab-48a4-49a0-b6f6-61cbebe63d0f
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
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
    internal-label: Intermediate
source-git-commit: 71835f0240311d4b26ec8e0d9bbdf005f9887579
workflow-type: tm+mt
source-wordcount: '269'
ht-degree: 0%
---
# Introduzione a Mesh API

Se hai poca esperienza con API Mesh per Adobe Developer App Builder, Adobe consiglia di iniziare con questo tutorial introduttivo, prima di passare agli altri video e tutorial.

## Cos’è API Mesh

API Mesh combina più origini di dati per ottenere una singola risposta da utilizzare per l’applicazione.

[Consulta la documentazione completa di API Mesh.](https://developer.adobe.com/graphql-mesh-gateway/mesh/){target="_blank"}

## A chi serve questo video?

* Qualsiasi sviluppatore che non ha mai utilizzato API Mesh o [!DNL Adobe Commerce] con esperienza limitata nell&#39;utilizzo di [Adobe I/O Runtime](https://developer.adobe.com/app-builder/docs/intro_and_overview/what-is-app-builder){target="_blank"} e API Mesh.

## Contenuto video

* Panoramica su Mesh API
* Collegamenti alla documentazione supplementare
* Caso d&#39;uso per eseguire il controllo dell&#39;inventario in tempo reale al momento del pagamento
* Spostare le attività di sviluppo e l’utilizzo delle risorse dall’applicazione commerce

>[!VIDEO](https://video.tv.adobe.com/v/3417534?learn=on)

## Casi d’uso di esempio

L’applicazione Commerce dispone di un’API REST e di un endpoint GraphQL. Ad esempio, utilizza l’API REST per applicare prezzi speciali o l’endpoint GraphQL per gestire lo stato dell’inventario. Utilizzando API Mesh, puoi definire entrambi gli endpoint, recuperare le informazioni e restituirle all’applicazione richiedente come una risposta.

## Che cos’è un proxy inverso

In qualità di sviluppatore che utilizza Adobe App Builder e API Mesh, non è necessario comprendere la definizione di proxy inverso. Tuttavia, se ti interessa la funzionalità complessiva relativa ad Adobe App Builder, utilizza le risorse seguenti:

* [Che cos’è un proxy inverso](https://www.imperva.com/learn/performance/reverse-proxy/){target="_blank"}


{{$include /help/_includes/api-mesh-related-links.md}}
