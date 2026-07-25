---
title: 'Strumento Bulk Data Migration (Migrazione dati in blocco): migrazione in una fase'
description: Scopri come eseguire una migrazione in un’unica fase con lo strumento Bulk Data Migration per esecuzioni sicure e ambienti in cui l’origine può rimanere attiva durante l’estrazione.
role: Developer
level: Intermediate
doc-type: Technical Video
topic: Migration
feature: Data Import/Export
duration: 737
last-substantial-update: 2026-07-24T00:00:00Z
jira: KT-22139
source-git-commit: 838387ffddbd8bee3ef3ec22694818eb2de5fe2d
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
