---
title: Architettura del connettore Commerce Cloud di Salesforce
description: Scopri in che modo Salesforce Commerce Cloud Connector Starter Kit utilizza le azioni di runtime App Builder e le esportazioni delta per sincronizzare i cataloghi con Adobe Commerce Optimizer.
feature: App Builder,Saas
topic: Administration,Commerce,Integrations
role: Developer
level: Beginner
doc-type: Technical Video
duration: 288
last-substantial-update: 2025-10-20T00:00:00.000Z
jira: KT-19014
exl-id: 1e0edcbb-5619-45c2-b06d-9133f23a634f
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: d3b92bef-63fa-5031-a925-d04d9362d616
    internal-label: Saas
  - id: cc250cf1-34eb-4863-80d0-d170d45ea067
    internal-label: Developer tools
subfeature_v2:
  - id: a743e5dc-8f37-4b5d-a848-03c32ca30598
    internal-label: App Builder
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
    internal-label: Beginner
source-git-commit: 7516911fc14bc80f327310b99a00f9cbff7c65c7
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

>[!VIDEO](https://video.tv.adobe.com/v/3476046?learn=on)

