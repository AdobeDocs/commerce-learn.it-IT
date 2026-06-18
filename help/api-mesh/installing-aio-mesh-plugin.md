---
title: Installare il plug-in Adobe I/O Runtime CLI e API Mesh
description: Scopri come installare l’interfaccia della riga di comando di Adobe I/O Runtime e il plug-in Mesh API per iniziare a utilizzare Mesh API per Adobe Developer App Builder.
jira: KT-11801
doc-type: Tutorial
duration: 410
last-substantial-update: 2023-02-08T00:00:00Z
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Developer
level: Beginner
exl-id: 898a0918-0362-4fa4-9204-d770ff1a7e6f
source-git-commit: c73744d503de5023e5c001d0534200522db55b04
workflow-type: tm+mt
source-wordcount: '167'
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
