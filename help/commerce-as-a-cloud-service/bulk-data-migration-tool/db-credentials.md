---
title: Strumento di migrazione dati in blocco - Credenziali DB
description: Scopri come configurare la connessione al database di origine nel file .my.cnf utilizzando Magento Cloud CLI o un ID progetto prima di eseguire lo strumento di migrazione.
role: Developer
level: Intermediate
doc-type: Technical Video
topic: Migration
feature: Data Import/Export
duration: 161
last-substantial-update: 2026-07-21T00:00:00.000Z
jira: KT-22105
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: 601e4abe-d9bf-58de-a779-32ed6794dcbe
    internal-label: Data Import/Export
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
    internal-label: Intermediate
source-git-commit: 71835f0240311d4b26ec8e0d9bbdf005f9887579
workflow-type: tm+mt
source-wordcount: '151'
ht-degree: 0%
---
# Configurare le credenziali del database per lo strumento Bulk Data Migration

Configurare la connessione al database di origine nel file `.my.cnf` prima di eseguire lo strumento di migrazione dati in blocco. I passaggi variano a seconda che l’ambiente di origine sia locale o Adobe Commerce as a Cloud Service (PaaS).

## A chi serve questo video?

* Architetto soluzioni
* DevOps Engineer
* Sviluppatore back-end

## Contenuto video

* Copiare `.my.cnf.example` in `.my.cnf` e creare una nuova sezione denominata per la connessione di origine.
* Imposta l&#39;ID progetto in `.my.cnf` se l&#39;origine è Adobe Commerce as a Cloud Service (PaaS).
* Utilizza i comandi del tunnel CLI di Magento Cloud per ottenere i valori dell’host, dell’utente, della password, della porta e del database.
* Conferma la connettività host e porta prima di eseguire lo strumento se l’origine è locale.

>[!VIDEO](https://video.tv.adobe.com/v/3496163?captions=ita&learn=on)
