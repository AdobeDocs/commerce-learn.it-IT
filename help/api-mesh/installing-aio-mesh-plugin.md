---
title: Installazione dell’interfaccia della riga di comando di Adobe I/O Runtime e del plug-in Mesh API
description: Scopri come installare l’interfaccia della riga di comando di Adobe I/O Runtime e il plug-in API Mesh
jira: KT-11801
doc-type: Tutorial
duration: 433
last-substantial-update: 2023-02-08T00:00:00Z
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Developer
level: Beginner, Intermediate
exl-id: 898a0918-0362-4fa4-9204-d770ff1a7e6f
source-git-commit: 003d55eac7e13a02ee633bed5ea9ab98825db151
workflow-type: tm+mt
source-wordcount: '150'
ht-degree: 0%

---

# Installazione di Adobe I/O Runtime CLI e del plug-in Mesh

Prima di iniziare a utilizzare API Mesh per Adobe Developer App Builder, è necessario installare la CLI `aio` e il plugin Mesh API.
Per le istruzioni di installazione e i prerequisiti, visita la pagina Mesh API [Guida introduttiva](https://developer.adobe.com/graphql-mesh-gateway/gateway/getting-started/){target="_blank"}.

## A chi serve questo video?

* Sviluppatori senza esperienza di API Mesh o [!DNL Adobe Commerce] con esperienza limitata utilizzando [Adobe I/O Runtime](https://developer.adobe.com/app-builder/docs/intro_and_overview/what-is-app-builder){target="_blank"} e API Mesh.

## Contenuto video

* Introduzione a Mesh API
* Installazione di Adobe I/O Runtime CLI (interfaccia della riga di comando)
* Installazione del plug-in Mesh API

>[!VIDEO](https://video.tv.adobe.com/v/3414122?learn=on)

## Installazione del plug-in `aio` CLI e API Mesh

Dopo aver installato `node` e `npm`, eseguire il comando seguente per installare la CLI `aio`:

```bash
npm install -g @adobe/aio-cli
```

Una volta installata la CLI di Adobe I/O Runtime, utilizza il seguente comando per installare il plug-in Mesh API:

```bash
aio plugins:install @adobe/aio-cli-plugin-api-mesh
```

{{$include /help/_includes/api-mesh-related-links.md}}
