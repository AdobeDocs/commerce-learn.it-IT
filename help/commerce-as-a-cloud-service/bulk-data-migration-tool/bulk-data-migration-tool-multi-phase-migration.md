---
title: Strumento Bulk Data Migration (Migrazione dati in blocco) - Migrazione in più fasi
description: Scopri come eseguire una migrazione multi-fase con lo strumento Bulk Data Migration (Migrazione dati in blocco) utilizzando la modalità di manutenzione quando l’origine deve rimanere bloccata durante il passaggio alla produzione.
feature: Data Import/Export
topic: Migration
role: Developer
doc-type: Technical Video
duration: 211
last-substantial-update: 2026-07-27T00:00:00Z
jira: KT-22157
source-git-commit: c3b81a5ffc652bc7ce7640b67fe5529067607251
workflow-type: tm+mt
source-wordcount: '220'
ht-degree: 0%

---


# Eseguire una migrazione multifase con lo strumento Bulk Data Migration (Migrazione dati in blocco)

Esegui una migrazione multifase quando l’ambiente sorgente deve essere bloccato durante l’estrazione: ideale per i clienti di produzione in cui non è possibile effettuare nuovi ordini durante la migrazione. Utilizza la modalità di manutenzione e dispone di cinque fasi che devono essere eseguite in ordine. Se l&#39;origine può rimanere attiva, guarda invece il video sulla migrazione monofase in questa serie.

## A chi serve questo video?

* Architetto soluzioni
* DevOps Engineer
* Sviluppatore back-end

## Contenuto video

* Prima di iniziare, è necessario distinguere tra i seguenti elementi: `bin/console` comandi eseguiti sullo strumento di migrazione stesso; `bin/magento maintenance` comandi eseguiti sul server Commerce di origine. Lo strumento non attiva o disattiva la modalità di manutenzione. Si tratta di un passaggio manuale.
* La fase uno viene eseguita mentre l&#39;origine è ancora attiva: `bin console migration:before-maintenance` controlla la configurazione, inizializza l&#39;ambiente, si connette al CDMS, registra la migrazione, esegue test funzionali e crea dati di test sintetici. Non attivare la modalità di manutenzione fino al termine di questa fase.
* La terza fase consiste nell&#39;estrazione da un ambiente bloccato: `bin/console migration:during-maintenance` riapre i tunnel PaaS se necessario, estrae dall&#39;origine, pulisce le visualizzazioni di gestione temporanea, carica nel target ACCS, esegue la verifica e pulisce i dati di test sul target.

>[!VIDEO](https://video.tv.adobe.com/v/3496413?learn=on)
