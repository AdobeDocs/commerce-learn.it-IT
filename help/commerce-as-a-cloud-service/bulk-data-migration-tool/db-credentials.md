---
title: Strumento di migrazione dati in blocco - Credenziali DB
description: Scopri come configurare la connessione al database di origine nel file .my.cnf utilizzando Magento Cloud CLI o un ID progetto prima di eseguire lo strumento di migrazione.
role: Developer
level: Intermediate
doc-type: Technical Video
topic: Migration
feature: Data Import/Export
duration: 161
last-substantial-update: 2026-07-21T00:00:00Z
jira: KT-22105
source-git-commit: 0dcb41e9138a36528f10333b0b5a9a9b2a39ed40
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

>[!VIDEO](https://video.tv.adobe.com/v/3496152?learn=on)
