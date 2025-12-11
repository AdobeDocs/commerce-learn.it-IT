---
title: Scopri Commerce Integration Starter Kit con le cartelle chiave e gli script di automazione illustrati
description: Scopri l’organizzazione del codice sorgente nel kit di avvio dell’integrazione di Commerce. ​
landing-page-description: Esplorazione dell’organizzazione del codice Source in un kit di avvio per l’integrazione di Commerce
kt: 15868
doc-type: video
duration: 420
audience: all
last-substantial-update: 2024-7-30
feature: Best Practices, Backend Development, Integration
topic: Architecture, Commerce, Development
old-role: Architect, Developer
role: Developer
level: Intermediate
exl-id: 678f4d2b-c57e-4afb-a535-1048a88bc3b1
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '377'
ht-degree: 0%

---

# Organizzazione del codice Source per Adobe Starter Kit

Scopri l’organizzazione del codice sorgente nel kit di avvio dell’integrazione di Adobe Commerce. &#x200B; Esplora la struttura del progetto, evidenziando le cartelle chiave come `actions` e `scripts` e il relativo contenuto&#x200B; La cartella &#39;actions&#39; contiene sottocartelle come `ingestion` e `webhook` che contengono codice essenziale per la gestione e il tracciamento degli eventi. Verranno inoltre fornite informazioni sulle cartelle `starter-kit-info` e `scripts`. La cartella `scripts` si concentra su script di automazione come `commerce-event-subscribe` e `onboarding` che semplificano la configurazione degli eventi e la configurazione del provider all&#39;interno del progetto.
&#x200B;
Esplora la logica alla base della struttura del codice sorgente, descrivendo nel dettaglio in che modo le cartelle `commerce` e `external` in ciascuna cartella di entità gestiscono gli eventi provenienti da sistemi diversi. Nel video viene illustrato il ruolo della cartella `consumer` nell&#39;invio degli eventi all&#39;azione di runtime appropriata del gestore eventi, in modo da garantire un&#39;elaborazione senza problemi. Il video illustra inoltre il meccanismo di esecuzione dei nuovi tentativi implementato nelle azioni di runtime per gestire in modo efficace gli eventi che generano errori. &#x200B;Scopri l’organizzazione e la funzionalità del codice sorgente nel kit di avvio dell’integrazione di Adobe Commerce, fornendo informazioni utili sulla gestione degli eventi, gli script di automazione e le configurazioni di configurazione.

## Pubblico

* Sviluppatori che desiderano comprendere come il codice sorgente è organizzato in cartelle chiave come `actions` e `scripts`.
* Scopri la cartella `actions` che contiene sottocartelle come `ingestion` e ` webhook` che contengono codice essenziale per la gestione degli eventi e il tracciamento delle distribuzioni.
* Sviluppatori che desiderano conoscere la cartella `actions` che include cartelle per entità come `customer`, `order`, `product` e `stock`.

## Contenuto video

* Le quattro cartelle principali: `actions`, `scripts`, `test` e `utils`, con particolare attenzione alle cartelle `actions` e `scripts` durante la sessione. &#x200B;
* Scopri la cartella `actions` e come contiene sottocartelle cruciali come `ingestion` e `webhook`.
* Esplora la cartella `actions` e spiega perché esistono cartelle specifiche per entità come `customer`, `order`, `product` e `stock`, ognuna contenente azioni di runtime strutturate in `commerce` e `external` cartelle per gestire in modo efficace gli eventi provenienti da Commerce e da sistemi di terze parti. &#x200B;
* Scopri l’importanza di non modificare il codice nella cartella `starter-kit-info`, che contiene un’azione di runtime utilizzata da Adobe per tenere traccia delle distribuzioni dei progetti in base al kit di avvio. &#x200B;
* Informazioni sulla cartella `scripts` contenente script di automazione come `commerce-event-subscribe` e `onboarding`, che automatizzano la configurazione dell&#39;evento, la configurazione del provider e la configurazione del modulo Adobe I/O Events in Commerce. &#x200B;

>[!VIDEO](https://video.tv.adobe.com/v/3431691?learn=on)

{{$include /help/_includes/starter-kit-related-links.md}}
