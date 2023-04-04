---
title: Installazione dell’interfaccia della riga di comando di Adobe I/O Runtime e del plug-in API Mesh
description: Scopri come installare l’interfaccia della riga di comando di Adobe I/O Runtime e il plug-in API Mesh
landing-page-description: Scopri come utilizzare Adobe App Builder e installare Adobe I/O Runtime con il plugin API Mesh.
short-description: Discover how to use Adobe App Builder and install the Adobe I/O Runtime with API Mesh plugin.
kt: 11801
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
source-git-commit: d85426bcf3ae0412a433414d70c874964905dda0
workflow-type: tm+mt
source-wordcount: '177'
ht-degree: 0%

---


# Installazione del plug-in Adobe I/O Runtime CLI e Mesh

Prima di iniziare a utilizzare la mesh API per Adobe Developer App Builder, devi installare la variabile `aio` CLI e il plug-in API Mesh.
Per istruzioni e prerequisiti per l’installazione, visita la mesh API [Introduzione](https://developer.adobe.com/graphql-mesh-gateway/gateway/getting-started/){target="_blank"} pagina.

## A chi serve questo video?

* Sviluppatori non ancora entrati nella mesh API o [!DNL Adobe Commerce] con esperienza limitata utilizzando [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/overview/){target="_blank"} e API Mesh.

## Contenuto video

* Introduzione alla mesh API
* Installazione di Adobe I/O Runtime CLI (interfaccia a riga di comando)
* Installazione del plug-in API Mesh

>[!VIDEO](https://video.tv.adobe.com/v/3414122?quality=12&learn=on)

## Installazione di `aio` Plug-in CLI e API Mesh

Dopo l&#39;installazione `node` e `npm`, esegui il seguente comando per installare il `aio` CLI:

```bash
npm install -g @adobe/aio-cli
```

Una volta installato Adobe I/O Runtime CLI, utilizza il seguente comando per installare il plug-in API Mesh:

```bash
aio plugins:install @adobe/aio-cli-plugin-api-mesh
```

{{$include /help/_includes/api-mesh-related-links.md}}
