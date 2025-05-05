---
title: Scopri come funziona Percona Toolkit pt-query-digest e perché viene utilizzato
description: Analizzare le query MySQL da file di registro lenti, generali e binari. Può anche analizzare le query da "SHOW PROCESSLIST" e i dati del protocollo MySQL da tcpdump.
kt: 13846
doc-type: video
activity: use
last-substantial-update: 2023-8-28
feature: Backend Development, Tools and External Services, Logs
topic: Commerce, Development
role: Architect, Developer
level: Intermediate
exl-id: 77e91f1b-b3ae-4c6d-bb6d-4fd7ebbb0baf
source-git-commit: 598bff1fd2cefdc449d5ae3431401aec1e796313
workflow-type: tm+mt
source-wordcount: '102'
ht-degree: 0%

---

# Percona Toolkit pt-query-digest

Scopri perché utilizzi pt-query-digest e alcuni esempi reali per contribuire ad approfondire il ragionamento.

## A chi serve questo video?

- Architetti
- Sviluppatori
- DevOps

## Contenuto video

- Scopri come utilizzare pt-query-digest
- Scopri i vantaggi e le carenze di questa funzione Percona Toolkit
- Comprendere i risultati e scoprire quali possibili passaggi delle prestazioni dovrebbero essere considerati

>[!VIDEO](https://video.tv.adobe.com/v/3423480?learn=on)

## Riferimenti al codice

```MYSQL
Be sure to change to match your logs and time frame

$ pt-query-digest mysql-slow.log.7 > mysql-slow.log.7.DIGEST
```

## Risorse utili

- [Toolkit Percona](https://docs.percona.com/percona-toolkit/pt-query-digest.html){target="_blank"}
- [Deadlock in MySQL](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/database/deadlocks-in-mysql.html?lang=it){target="_blank"}
