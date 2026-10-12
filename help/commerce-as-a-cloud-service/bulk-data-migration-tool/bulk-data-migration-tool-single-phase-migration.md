---
title: 'Strumento Bulk Data Migration (Migrazione dati in blocco): migrazione in una fase'
description: Scopri come eseguire una migrazione in un’unica fase con lo strumento Bulk Data Migration per esecuzioni sicure e ambienti in cui l’origine può rimanere attiva durante l’estrazione.
role: Developer
level: Intermediate
doc-type: Technical Video
topic: Migration
feature: Data Import/Export
duration: 737
last-substantial-update: 2026-07-24T00:00:00.000Z
jira: KT-22139
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
source-wordcount: '238'
ht-degree: 0%
---
# Eseguire una migrazione monofase con lo strumento Bulk Data Migration

Esegui una migrazione in un’unica fase quando l’ambiente sorgente può rimanere attivo durante l’estrazione, ideale per esecuzioni in modalità &quot;dry run&quot; e ambienti di sviluppo o sandbox. Se hai bisogno di un&#39;origine bloccata, ad esempio un passaggio alla produzione in cui non è possibile inoltrare nuovi ordini a metà migrazione, guarda invece il video della migrazione per fasi disponibile in questa serie.

## A chi serve questo video?

* Architetto soluzioni
* DevOps Engineer
* Sviluppatore back-end

## Contenuto video

* Creare l&#39;immagine Docker con `bin console build`. Eseguire di nuovo questa operazione solo se il file Docker cambia.
* Per avviare il gestore di contenitori CLI CDMS, eseguire `bin console start`, quindi aprire una shell nel contenitore una volta per scaricare le relative dipendenze.
* Per eseguire la pipeline completa in dieci passaggi, eseguire `bin console migration`: controllare la configurazione, inizializzare l&#39;ambiente, aprire i tunnel PaaS, eseguire gli integration test, registrarsi con CDMS, analizzare lo schema di destinazione, generare i dati di test, estrarre i dati di origine, caricare in ACCS, verificare i checksum, pulire e riepilogare.
* Controlla il rapporto di riepilogo della migrazione: il passaggio 8 (verifica integrità dati) registra gli errori senza arrestare la pipeline, pertanto un’esecuzione completata non garantisce una verifica pulita.
* Questo comando monofase è una pipeline completa e autonoma: non utilizzarlo come passaggio all’interno del flusso di lavoro in modalità di manutenzione (migrazione graduale), che dispone di comandi dedicati.

>[!VIDEO](https://video.tv.adobe.com/v/3496322?captions=ita&learn=on)
