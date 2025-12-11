---
title: Utilizzare la funzionalità nativa di un meccanismo di ripetizione
description: Sfrutta il meccanismo di esecuzione di nuovi tentativi di Adobe I/O Events per applicazioni resilienti, incluse condizioni di esecuzione di nuovi tentativi e indicatori visivi.
landing-page-description: Comprendi e utilizza il meccanismo integrato di esecuzione di nuovi tentativi di Adobe I/O Events per migliorare la resilienza delle applicazioni e gestire in modo efficace le attivazioni degli eventi.
kt: 15872
doc-type: video
duration: 314
audience: all
last-substantial-update: 2024-7-31
feature: Best Practices, Backend Development, Integration
topic: Architecture, Commerce, Development
old-role: Architect, Developer
role: Developer
level: Intermediate
exl-id: 412060b3-76ae-4c27-bf96-8eb2a0f0d0e8
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '342'
ht-degree: 0%

---

# Meccanismo di esecuzione di nuovi tentativi Adobe I/O Events per la resilienza delle applicazioni

Il video illustra una guida completa sull’utilizzo del meccanismo di esecuzione dei tentativi integrato di Adobe I/O Events per migliorare la resilienza delle applicazioni. Scopri in che modo i codici di stato delle risposte HTTP specifici attivano i nuovi tentativi degli eventi. Adobe I/O Events utilizza strategie di back-off esponenziali e fisse per i nuovi tentativi, con intervalli che aumentano da un minuto a 15 minuti. La documentazione descrive anche come vengono visualizzati gli indicatori dei tentativi nella console per sviluppatori, con suggerimenti visivi come icone di avviso e frecce circolari che indicano rispettivamente gli eventi non riusciti e quelli ritentati.

Scopri come funziona il meccanismo di esecuzione dei nuovi tentativi nel contesto delle azioni di runtime &quot;consumer&quot; e determina se un evento viene ritentato. Le risposte riuscite sono indicate con un codice di stato 200, mentre le risposte di errore includono un oggetto errore con un attributo &quot;statusCode&quot;. L’azione di runtime &quot;consumer&quot; determina il codice di risposta HTTP da restituire in base ai risultati dell’elaborazione a valle, garantendo un’efficiente gestione degli eventi e le eventuali attivazioni riuscite.

## Pubblico

* Sviluppatori che desiderano comprendere i codici di stato delle risposte HTTP specifici che attivano i nuovi tentativi degli eventi.
* Team che desiderano scoprire le strategie di back-off esponenziali e fisse utilizzate da Adobe I/O Events per i nuovi tentativi.
* Sviluppatori che desiderano comprendere in che modo gli indicatori visivi nella console per sviluppatori rappresentano eventi non riusciti e tentativi.

## Video dontent

* Adobe I/O Events dispone di un meccanismo predefinito di esecuzione di nuovi tentativi che ritenta automaticamente le attivazioni degli eventi in base a specifici codici di stato di risposta HTTP.
* Il meccanismo di esecuzione di nuovi tentativi implementato da Adobe I/O Events prevede strategie di back-off esponenziali e fisse.
* Indicatori visivi nella console per sviluppatori, ad esempio icone di avviso per eventi non riusciti e icone a freccia circolare per eventi ritentati.
* Le azioni di runtime &quot;consumer&quot; svolgono un ruolo fondamentale nella determinazione dei codici di stato delle risposte HTTP appropriati per la gestione degli eventi.

>[!VIDEO](https://video.tv.adobe.com/v/3431695?learn=on)

{{$include /help/_includes/starter-kit-related-links.md}}

## Documentazione correlata

* [Webhook non è in grado di gestire un evento](https://developer.adobe.com/events/docs/support/faq/#what-happens-if-my-webhook-is-unable-to-handle-a-specific-event-but-handles-all-other-events-gracefully)
* [Webhook disattivato e contrassegnato come instabile](https://developer.adobe.com/events/docs/support/faq/#what-happens-if-my-webhook-is-down-why-is-my-event-registration-marked-as-unstable)
