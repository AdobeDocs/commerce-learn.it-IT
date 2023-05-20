---
title: Installazione dell’interfaccia della riga di comando di Adobe I/O Runtime e del plug-in Mesh API
description: Scopri come installare l’interfaccia della riga di comando di Adobe I/O Runtime e il plug-in API Mesh
landing-page-description: Scopri come utilizzare Adobe App Builder e installare il plug-in Adobe I/O Runtime con API Mesh.
short-description: Scopri come utilizzare Adobe App Builder e installare il plug-in Adobe I/O Runtime con API Mesh.
kt: 11801
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
exl-id: 898a0918-0362-4fa4-9204-d770ff1a7e6f
source-git-commit: edb98cf6544954d741c43beb39f4056326c7d26b
workflow-type: tm+mt
source-wordcount: '195'
ht-degree: 0%

---

# Installazione di Adobe I/O Runtime CLI e del plug-in Mesh

Prima di iniziare a utilizzare Mesh API per Adobe Developer App Builder, è necessario installare `aio` CLI e il plug-in Mesh API.
Per istruzioni e prerequisiti di installazione, visita API Mesh. [Introduzione](https://developer.adobe.com/graphql-mesh-gateway/gateway/getting-started/){target="_blank"} pagina.

## A chi serve questo video?

* Sviluppatori senza esperienza di API Mesh o [!DNL Adobe Commerce] con un’esperienza limitata nell’utilizzo di [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/overview/){target="_blank"} e API Mesh.

## Contenuto video

* Introduzione a Mesh API
* Installazione di Adobe I/O Runtime CLI (interfaccia della riga di comando)
* Installazione del plug-in Mesh API

>[!VIDEO](https://video.tv.adobe.com/v/3414122?quality=12&learn=on)

## Installazione di `aio` Plug-in CLI e API Mesh

Dopo l’installazione `node` e `npm`, eseguire il comando seguente per installare `aio` CLI:

```bash
npm install -g @adobe/aio-cli
```

Una volta installata la CLI di Adobe I/O Runtime, utilizza il seguente comando per installare il plug-in Mesh API:

```bash
aio plugins:install @adobe/aio-cli-plugin-api-mesh
```

{{$include /help/_includes/api-mesh-related-links.md}}
