---
title: Installare il plug-in Adobe I/O Runtime CLI e API Mesh
description: Scopri come installare l’interfaccia della riga di comando di Adobe I/O Runtime e il plug-in Mesh API per iniziare a utilizzare Mesh API per Adobe Developer App Builder.
jira: KT-11801
doc-type: Tutorial
duration: 410
last-substantial-update: 2023-02-08T00:00:00.000Z
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Developer
level: Beginner
exl-id: 898a0918-0362-4fa4-9204-d770ff1a7e6f
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
source-wordcount: '182'
ht-degree: 0%
---
# Installazione di Adobe I/O Runtime CLI e del plug-in Mesh

Prima di iniziare a utilizzare API Mesh per Adobe Developer App Builder, è necessario installare la CLI `aio` e il plugin Mesh API.
Per le istruzioni di installazione e i prerequisiti, visita la pagina Mesh API [Guida introduttiva](https://developer.adobe.com/graphql-mesh-gateway/mesh/basic/){target="_blank"}.

## A chi serve questo video?

* Sviluppatori senza esperienza di API Mesh o [!DNL Adobe Commerce] con esperienza limitata utilizzando [Adobe I/O Runtime](https://developer.adobe.com/app-builder/docs/intro_and_overview/what-is-app-builder){target="_blank"} e API Mesh.

## Contenuto video

* Introduzione a Mesh API
* Installazione di Adobe I/O Runtime CLI (interfaccia della riga di comando)
* Installazione del plug-in Mesh API

>[!VIDEO](https://video.tv.adobe.com/v/3414122?learn=on)

## Installazione del plug-in `aio` CLI e API Mesh

Per installare la CLI `aio`, eseguire il comando seguente dopo aver installato `node` e `npm`:

```bash
npm install -g @adobe/aio-cli
```

Una volta installata la CLI di Adobe I/O Runtime, utilizza il seguente comando per installare il plug-in Mesh API:

```bash
aio plugins:install @adobe/aio-cli-plugin-api-mesh
```

{{$include /help/_includes/api-mesh-related-links.md}}
