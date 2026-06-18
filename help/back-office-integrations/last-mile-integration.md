---
title: Integrazione dell’ultimo miglio nel kit di avvio Commerce
description: Scopri l’integrazione dell’ultimo miglio in Commerce utilizzando gli hook di estensibilità per la convalida, la trasformazione, la pre-elaborazione, l’invio e la post-elaborazione.
doc-type: Technical Video
duration: 557
last-substantial-update: 2024-07-30
feature: Best Practices, Backend Development, Integration
topic: Architecture, Commerce, Development
role: Developer
level: Intermediate
jira: KT-15869
exl-id: e86e8c7b-d5d2-484d-90a2-9c5309c7ea1d
TQID: https://experienceleague.adobe.com/TCR23A98L8XrVDEQeqLQoOXKQPBQu-Wb7YnGUkBXgak
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: c1256247-af4b-46d8-9dca-0c654ecfa157
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
source-git-commit: 9568f37b026d0e659e8092282cb923c7ecde58ac
workflow-type: tm+mt
source-wordcount: 342
ht-degree: 0%

---

# Integrazione dell’ultimo miglio con Adobe Starter Kit

Scopri gli elementi da considerare all’avvio dell’integrazione dell’ultimo miglio con Adobe Commerce, concentrandoti sugli hook di estensibilità per migliorare la connettività con sistemi di terze parti. Questo video illustra un approccio strutturato in cui vari hook, come la convalida, la trasformazione, la pre-elaborazione, l’invio e la post-elaborazione, garantiscono un flusso di dati e una sincronizzazione del sistema fluidi. Ciascun amo ha uno scopo distinto, tra cui:

* Convalida dei dati in arrivo in base agli schemi
* Trasformazione di oggetti dati tra sistemi
* Esecuzione di calcoli prima dell&#39;invio di informazioni rilevanti
* Invio dei dati al sistema di destinazione

È importante mantenere file JavaScript separati per ciascun blocco per garantire l’integrità della logica di business e facilitare i futuri aggiornamenti del framework, garantendo una configurazione dell’integrazione solida e adattabile.

Scopri il significato delle attività di post-elaborazione tramite l’hook di post-elaborazione, che consente agli utenti di eseguire azioni aggiuntive dopo la sincronizzazione dei dati, ad esempio l’aggiunta di commenti agli ordini o la memorizzazione di ID esterni. Il video include best practice come l’incapsulamento delle richieste API in librerie specifiche per semplificare le connessioni con sistemi di terze parti. Scoprirai anche i casi d’uso tipici di ogni hook e le indicazioni sulla gestione di scenari diversi.

## Pubblico

* Sviluppatori che desiderano imparare la struttura e la funzionalità degli hook di estensibilità e come questi hook possono migliorare la connettività con sistemi di terze parti.
* Sviluppatori che desiderano apprendere i casi d’uso tipici e le best practice associate a ciascun hook di estensibilità, come convalida, trasformazione, pre-elaborazione, invio e post-elaborazione, per facilitare il flusso di dati, la sincronizzazione del sistema e la manutenzione efficiente dell’impostazione dell’integrazione. &#x200B;

## Contenuto video

* Scopri la struttura delle azioni richiamate nell’integrazione dell’ultimo miglio.
* Comprendi i casi d’uso tipici all’interno dell’hook di convalida, inclusa la convalida dei dati in arrivo rispetto agli schemi e l’esclusione di eventi specifici in base a determinati criteri. &#x200B;
* Scopri il ruolo dell’hook di trasformazione nella trasformazione degli oggetti dati tra i sistemi di origine e di destinazione.
* Scopri l’importanza dell’hook di invio nel facilitare l’effettivo invio dei dati al sistema di destinazione.

>[!VIDEO](https://video.tv.adobe.com/v/3451933?captions=ita&learn=on)

{{$include /help/_includes/starter-kit-related-links.md}}
