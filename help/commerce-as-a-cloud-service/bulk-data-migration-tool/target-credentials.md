---
title: Strumento di migrazione dati in blocco - Credenziali di Target
description: Scopri come configurare gli URL dell’istanza di destinazione, le credenziali Adobe IMS e le impostazioni CDMS nel file .env prima di eseguire lo strumento Bulk Data Migration Tool.
role: Developer
level: Intermediate
doc-type: Technical Video
topic: Migration
feature: Data Import/Export
duration: 226
last-substantial-update: 2026-07-21T00:00:00.000Z
jira: KT-22107
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
source-wordcount: '173'
ht-degree: 0%
---
# Configurare le credenziali di destinazione per lo strumento Bulk Data Migration

Impostare gli URL dell&#39;istanza di destinazione, le credenziali Adobe IMS e le impostazioni CDMS nel file `.env` prima di eseguire lo strumento di migrazione dati in blocco. Assicurati che l’URL di Adobe IMS, l’URL di destinazione e l’host CDMS corrispondano allo stesso livello di ambiente: stadio o produzione.

## A chi serve questo video?

* Architetto soluzioni
* DevOps Engineer
* Sviluppatore back-end

## Contenuto video

* Impostare gli URL REST e GraphQL dell&#39;istanza di destinazione e l&#39;ID tenant di destinazione nel file `.env`, utilizzando i valori del pannello informazioni istanza su experience.adobe.com.
* Imposta l’URL di Adobe IMS in modo che corrisponda al livello di ambiente (stage o produzione) e all’area geografica.
* Recupera l&#39;ID client Adobe IMS e il segreto client da **Project** > **OAuth Server-to-Server** in Adobe Developer Console.
* Copia l’ID dell’organizzazione di destinazione e configura le impostazioni dell’host CDMS, della porta e del server locale in modo che corrispondano al tuo ambiente.

>[!VIDEO](https://video.tv.adobe.com/v/3496167?learn=on)
