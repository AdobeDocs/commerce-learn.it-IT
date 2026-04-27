---
title: Diagnosticare e correggere alcuni errori comuni di Commerce Cloud
description: Risolvi due errori comuni dei progetti Adobe Cloud che impediscono il caricamento del sito.
feature: Cloud, Site Management
topic: Commerce, Development
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
doc-type: Technical Video
duration: 297
last-substantial-update: 2024-10-30T00:00:00.000Z
jira: KT-16419
exl-id: 4c21b6a6-783a-422f-9071-3534ed68e8be
TQID: https://experienceleague.adobe.com/-mN2UoNU6uKjUoHmZT59LgT4o7p4d4O2Zl1BR3x8y-8
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2:
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 135
ht-degree: 0%

---

# Servizio di diagnostica e correzione non disponibile e si è verificato un errore

Scopri come eseguire il triage e risolvere due errori comuni rilevati nei progetti Adobe Commerce Cloud.  Scopri come e perché si verificano questi errori e quali sono i passaggi consigliati per risolverli.

## Per chi è questo video

* Sviluppatori e professionisti IT
* Amministratori di sistema

## Contenuto video

* Diagnosticare e risolvere i problemi di archiviazione:
* Gestisci modalità manutenzione
* Suggerimenti per la risoluzione efficiente dei problemi

>[!VIDEO](https://video.tv.adobe.com/v/3447701?captions=ita&learn=on)


## Comandi utilizzati nel video

Trovare le ultime 5 righe del registro eccezioni indicato nel messaggio di risposta.

```SHELL
 tail -n 5 ~/var/log/exception.log
```

Per controllare lo spazio sul disco rigido. Presta attenzione allo sviluppo/mapper/xxxx della linea

```SHELL
df -h
```

Consente di trovare i primi 15 file più grandi

```SHELL
find -type f -exec du -Sh {} + | sort -rh | head -n 15
```

Visualizza lo stato della modalità di manutenzione

```SHELL
php bin/magento maintenance:status
```

Disattiva la modalità di manutenzione

```SHELL
php bin/magento maintenance:disable 
```
