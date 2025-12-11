---
title: Installazione dell’interfaccia della riga di comando di Adobe I/O Runtime e del plug-in Mesh API
description: Scopri come installare l’interfaccia della riga di comando di Adobe I/O Runtime e il plug-in API Mesh
landing-page-description: Scopri come utilizzare Adobe App Builder e installare il plug-in Adobe I/O Runtime con API Mesh.
short-description: Scopri come utilizzare Adobe App Builder e installare il plug-in Adobe I/O Runtime con API Mesh.
kt: 11801
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
exl-id: 898a0918-0362-4fa4-9204-d770ff1a7e6f
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '186'
ht-degree: 0%

---

# Installazione di Adobe I/O Runtime CLI e del plug-in Mesh

Prima di iniziare a utilizzare API Mesh per Adobe Developer App Builder, è necessario installare la CLI `aio` e il plugin Mesh API.
Per le istruzioni di installazione e i prerequisiti, visita la pagina Mesh API [Guida introduttiva](https://developer.adobe.com/graphql-mesh-gateway/gateway/getting-started/){target="_blank"}.

## A chi serve questo video?

* Sviluppatori senza esperienza di API Mesh o [!DNL Adobe Commerce] con esperienza limitata utilizzando [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/overview/){target="_blank"} e API Mesh.

## Contenuto video

* Introduzione a Mesh API
* Installazione di Adobe I/O Runtime CLI (interfaccia della riga di comando)
* Installazione del plug-in Mesh API

>[!VIDEO](https://video.tv.adobe.com/v/3414122?quality=12&learn=on)

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
