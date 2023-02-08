---
title: Installazione dell'interfaccia della riga di comando di Adobe Developer IO e del plug-in API Mesh
description: Scopri come installare l’interfaccia della riga di comando di Adobe Developer IO e il plugin API Mesh
landing-page-description: Scopri come utilizzare Adobe App Builder e installare Adobe Developer IO con il plugin API Mesh.
kt: 11801
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
source-git-commit: b6d501c5c852e1cc2cf1f05f91b5a9d96ac7d036
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---


# Installazione del plug-in Adobe Developer IO e Mesh

Prima di iniziare, è necessario configurare alcuni elementi. Innanzitutto, la configurazione dell&#39;interfaccia della riga di comando di Adobe Developer IO. Quindi, assicurati che il plug-in API Mesh sia configurato in ogni ambiente.
Per istruzioni su come configurare l&#39;ambiente locale per eseguire Node, nvm e installare Adobe Developer IO, visita [Guida introduttiva di GraphQL Mesh](https://developer.adobe.com/graphql-mesh-gateway/gateway/getting-started/).

## A chi serve questo video?

* Sviluppatori non ancora entrati in Adobe App Builder o [!DNL Magento Open Source] con esperienza limitata con Adobe Developer IO e API Mesh.

## Contenuto video

* Introduzione alla mesh API
* Installazione dell&#39;interfaccia della riga di comando di Adobe Developer IO
* Aggiunta del plug-in API Mesh alla riga di comando AIO

>[!VIDEO](https://video.tv.adobe.com/v/3414122/)

## Esempi di comandi che utilizzano NPM e AIO

L’installazione dell’interfaccia della riga di comando di Adobe Developer è piuttosto semplice. Dopo che Node ha installato esegui questo comando `npm install -g @adobe/aio-cli`
Una volta installato il client Adobe Developer, è possibile installare il plugin mesh. Esegui questo comando `aio plugins:install @adobe/aio-cli-plugin-api-mesh`

{{$include /help/_includes/api-mesh-related-links.md}}
