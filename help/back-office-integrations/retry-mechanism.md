---
title: Garantire robustezza utilizzando la funzionalità nativa di un meccanismo di esecuzione di nuovi tentativi.
description: Utilizzo del meccanismo di esecuzione di nuovi tentativi di Adobe I/O Events per applicazioni resilienti, incluse condizioni di esecuzione di nuovi tentativi e indicatori visivi. ​
landing-page-description: Comprendi e utilizza il meccanismo integrato di esecuzione di nuovi tentativi di Adobe I/O Events per migliorare la resilienza delle applicazioni e gestire in modo efficace le attivazioni degli eventi. ​
kt: 15872
doc-type: video
duration: 314
audience: all
last-substantial-update: 2024-7-31
feature: Best Practices, Backend Development, Integration
topic: Architecture, Commerce, Development
role: Architect, Developer
level: Intermediate
source-git-commit: 11daa4b29dafe5fcecf6b75cf2269b87f65c4612
workflow-type: tm+mt
source-wordcount: '350'
ht-degree: 0%

---

# Utilizzo del meccanismo di esecuzione di nuovi tentativi degli eventi Adobe I/O per la &#x200B; della resilienza delle applicazioni

Il video illustra una guida completa sull’utilizzo del meccanismo di esecuzione dei tentativi integrato di Adobe I/O Events per migliorare la resilienza delle applicazioni. Scopri in che modo i codici di stato delle risposte HTTP specifici attivano i nuovi tentativi degli eventi. Adobe I/O Events utilizza strategie di back-off esponenziali e fisse per i nuovi tentativi, con intervalli che aumentano da un minuto a 15 minuti&#x200B;. La documentazione descrive anche come vengono visualizzati gli indicatori dei tentativi nella console per sviluppatori, con suggerimenti visivi come icone di avviso e frecce circolari che indicano rispettivamente gli eventi non riusciti e quelli ritentati.

Scopri come funziona il meccanismo di esecuzione dei nuovi tentativi nel contesto delle azioni di runtime &quot;consumer&quot; e determina se un evento viene ritentato. &#x200B;Le risposte riuscite sono indicate con un codice di stato 200, mentre le risposte di errore includono un oggetto errore con un attributo &quot;statusCode&quot;. L’azione di runtime &quot;consumer&quot; determina il codice di risposta HTTP da restituire in base ai risultati dell’elaborazione a valle, garantendo un’efficiente gestione degli eventi e le eventuali attivazioni riuscite. &#x200B;


## Pubblico

* Sviluppatori che desiderano comprendere i codici di stato delle risposte HTTP specifici che attivano i nuovi tentativi degli eventi.
* Team che desiderano scoprire le strategie di back-off esponenziali e fisse utilizzate dagli eventi Adobi I/O per i nuovi tentativi.
* Sviluppatori che desiderano comprendere in che modo gli indicatori visivi nella console per sviluppatori rappresentano eventi non riusciti e tentativi.

## Contenuto video

* Adobe I/O Events dispone di un meccanismo predefinito di esecuzione di nuovi tentativi che ritenta automaticamente le attivazioni degli eventi in base a specifici codici di stato di risposta HTTP&#x200B;
* Il meccanismo di esecuzione di nuovi tentativi implementato da Adobe I/O Events prevede strategie di back-off esponenziali e fisse. &#x200B;
* Indicatori visivi nella console per sviluppatori, ad esempio icone di avviso per eventi non riusciti e icone a freccia circolare per eventi ritentati.
* Le azioni di runtime &quot;consumer&quot; svolgono un ruolo fondamentale nella determinazione dei codici di stato delle risposte HTTP appropriati per la gestione degli eventi.

>[!VIDEO](https://video.tv.adobe.com/v/3431695?learn=on)

{{$include /help/_includes/starter-kit-related-links.md}}

## Documentazione correlata

* [Webhook non è in grado di gestire un evento](https://developer.adobe.com/events/docs/support/faq/#what-happens-if-my-webhook-is-unable-to-handle-a-specific-event-but-handles-all-other-events-gracefully)
* [Webhook disattivato e contrassegnato come instabile](https://developer.adobe.com/events/docs/support/faq/#what-happens-if-my-webhook-is-down-why-is-my-event-registration-marked-as-unstable)
