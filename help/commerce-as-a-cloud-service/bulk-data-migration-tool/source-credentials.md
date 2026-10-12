---
title: Strumento di migrazione dati in blocco - Credenziali Source
description: Scopri come configurare l’URL dell’istanza sorgente e le credenziali di autenticazione nel file .env prima di eseguire lo strumento di migrazione dei dati in blocco.
role: Developer
level: Intermediate
doc-type: Technical Video
topic: Migration
feature: Data Import/Export
duration: 238
last-substantial-update: 2026-07-21T00:00:00.000Z
jira: KT-22095
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
source-wordcount: '146'
ht-degree: 0%
---
# Configurare le credenziali di origine per lo strumento Bulk Data Migration

Impostare l&#39;URL dell&#39;istanza di origine e le credenziali di autenticazione nel file `.env` prima di eseguire lo strumento di migrazione dati in blocco. I passaggi di autenticazione variano leggermente a seconda che l’ambiente di origine sia locale o Adobe Commerce as a Cloud Service (PaaS).

## A chi serve questo video?

* Architetto soluzioni
* DevOps Engineer
* Sviluppatore back-end

## Contenuto video

* Impostare l&#39;URL dell&#39;istanza di origine e gli URL REST e GraphQL nel file `.env`.
* Recupera o crea le chiavi di integrazione da **System** > **Extensions** > **Integrazioni** nell&#39;amministrazione di Adobe Commerce.
* Per generare i quattro token richiesti, attiva l’integrazione.
* Recupera il token CLI di Magento da account.magento.cloud se l’origine è Adobe Commerce as a Cloud Service (PaaS).

>[!VIDEO](https://video.tv.adobe.com/v/3496148?captions=ita&learn=on)
