---
title: Architettura del connettore Commerce Cloud di Salesforce
description: Scopri in che modo Salesforce Commerce Cloud Connector Starter Kit utilizza le azioni di runtime App Builder e le esportazioni delta per sincronizzare i cataloghi con Adobe Commerce Optimizer.
feature: App Builder,Saas
topic: Administration,Commerce,Integrations
role: Developer
level: Beginner
doc-type: Technical Video
duration: 288
last-substantial-update: 2025-10-20T00:00:00Z
jira: KT-19014
exl-id: 1e0edcbb-5619-45c2-b06d-9133f23a634f
source-git-commit: 456f3cae8c45d137a195456692c2d11204126bb7
workflow-type: tm+mt
source-wordcount: '202'
ht-degree: 0%

---

# Architettura del kit di avvio Salesforce Commerce Cloud

Scopri l’architettura e le funzionalità del Commerce Optimizer Connector Starter Kit, che integra Salesforce Commerce Cloud (SFCC) e Adobe App Builder. Il kit di avvio viene utilizzato da Adobe Commerce Optimizer per semplificare la sincronizzazione del catalogo per le vetrine di Edge Delivery. Spiega come una cartuccia personalizzata in SFCC rileva le modifiche al catalogo tramite i file di esportazione delta e le espone tramite API personalizzate. Queste modifiche vengono utilizzate dalle azioni runtime di App Builder, sia sincrone che asincrone, per eseguire sincronizzazioni complete e delta, aggiornamenti dei metadati e sincronizzazioni specifiche per il prodotto. Il sistema include inoltre strumenti di convalida per garantire la precisione della vetrina e utilizza la gestione dello stato di App Builder per tenere traccia dello stato di sincronizzazione e prevenire conflitti.

## A chi serve questo video?

* Architetto di soluzioni Commerce
* Tecnici Marketing Engineer
* Amministratori di eCommerce Platform

## Contenuto video

* La cartuccia SFCC e le API personalizzate rilevano le modifiche al catalogo tramite esportazioni delta, consentendo un’efficiente sincronizzazione dei dati con Adobe App Builder.
* Le azioni di runtime di App Builder gestiscono le sincronizzazioni complete e delta, la convalida e il tracciamento dello stato per garantire aggiornamenti precisi e senza conflitti di Commerce Optimizer.

>[!VIDEO](https://video.tv.adobe.com/v/3476059?captions=ita&learn=on)

