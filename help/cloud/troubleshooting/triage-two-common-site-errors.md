---
title: Diagnosticare e correggere alcuni errori comuni relativi alle Commerci Cloud
description: Risolvi due errori comuni relativi al progetto Adobe Cloud che impediscono il caricamento del sito.
feature: Cloud, Site Management
topic: Commerce, Development
role: Architect, Developer
level: Beginner, Intermediate
doc-type: Technical Video
duration: 260
last-substantial-update: 2024-10-30T00:00:00Z
jira: KT-16419
source-git-commit: 27c1715dd42853013181d9c729549a5a32bc2af0
workflow-type: tm+mt
source-wordcount: '135'
ht-degree: 0%

---


# Servizio di diagnostica e correzione non disponibile e si è verificato un errore

Scopri come eseguire il triage e risolvere due errori comuni rilevati nei progetti Adobe Commerce Cloud.  Scopri come e perché si verificano questi errori e quali sono i passaggi consigliati per risolverli.

## Per chi è questo video

- Sviluppatori e professionisti IT
- Amministratori di sistema

## Contenuto video

- Diagnosticare e risolvere i problemi di archiviazione:
- Gestisci modalità manutenzione
- Suggerimenti per la risoluzione efficiente dei problemi

>[!VIDEO](https://video.tv.adobe.com/v/3435766?learn=on)


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
