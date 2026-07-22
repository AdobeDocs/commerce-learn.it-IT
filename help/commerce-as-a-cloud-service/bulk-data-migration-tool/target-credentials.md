---
title: Strumento di migrazione dati in blocco - Credenziali di Target
description: Scopri come configurare gli URL dell’istanza di destinazione, le credenziali Adobe IMS e le impostazioni CDMS nel file .env prima di eseguire lo strumento Bulk Data Migration Tool.
role: Developer
level: Intermediate
doc-type: Technical Video
topic: Migration
feature: Data Import/Export
duration: 226
last-substantial-update: 2026-07-21T00:00:00Z
jira: KT-22107
source-git-commit: b3c029f7c1080550900cbc5838478cd7a4137a20
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

>[!VIDEO](https://video.tv.adobe.com/v/3496173?captions=ita&learn=on)
