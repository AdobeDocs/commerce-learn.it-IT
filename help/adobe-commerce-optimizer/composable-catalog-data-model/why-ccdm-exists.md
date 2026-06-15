---
title: Perché esiste il Composable Catalog Data Model (CCDM)
description: Scopri in che modo CCDM mantiene un catalogo unificato mentre gli store front ricevono prodotti, prezzi e regole filtrati utilizzando le viste catalogo e i servizi di merchandising.
feature: Saas, Storefront
topic: Commerce
role: Developer
level: Beginner
doc-type: Tutorial
duration: 259
last-substantial-update: 2026-05-15T00:00:00Z
jira: KT-18624
source-git-commit: bfe282e4f1ef04985cffb109bce90bc05a70fda0
workflow-type: tm+mt
source-wordcount: '597'
ht-degree: 0%

---

# Perché esiste il modello dati del catalogo componibile

I moderni team di commerce vendono spesso tra **marchi**, **aree geografiche**, **concessionari** e **canali digitali**. Quando ogni canale mantiene la propria copia del catalogo, i team dedicano più tempo a riconciliare SKU, prezzi e disponibilità che a migliorare l’esperienza dell’acquirente. Il **Adobe Composable Catalog Data Model (CCDM)** dietro **Adobe Commerce Optimizer** è progettato per invertire tale pattern: **un catalogo unificato** in un livello SaaS, con **visualizzazioni catalogo** e **criteri** che definiscono ciò che ogni vetrina o integrazione può visualizzare.

## A chi serve questo video?

* Architetti e sviluppatori di soluzioni Commerce che hanno poca esperienza con Adobe Commerce Optimizer e necessitano del contesto aziendale prima di configurare origini di catalogo, visualizzazioni e criteri

## Contenuto video

* Perché la duplicazione di cataloghi specifici per canale crea rischi operativi e rallenta l&#39;innovazione
* In che modo CCDM mantiene i dati dei prodotti unificati e supporta al tempo stesso scenari multi-brand e multi-area
* Funzionamento delle visualizzazioni del catalogo come &quot;obiettivo&quot; tra un catalogo di base condiviso e una vetrina o un pubblico specifico
* Modalità di utilizzo delle API di Merchandising Services in modo che le esperienze headless rimangano allineate al catalogo configurato

>[!VIDEO](https://video.tv.adobe.com/v/3491285?learn=on)

## La sfida con i cataloghi in silos

Quando ogni sito di rivenditori, vetrina regionale o proprietà del marchio mantiene **il proprio database di cataloghi**, si verificano diversi problemi:

* **Duplicazione**: lo stesso SKU, la stessa descrizione e lo stesso supporto vengono immessi più volte.
* **Deriva**: gli aggiornamenti dei prezzi, i nuovi attributi o gli elementi interrotti vengono inviati in un canale, ma non in altri.
* **Avvii più lenti**: ogni nuovo punto di contatto ripete un lavoro intenso sui dati invece di riutilizzare un singolo record di prodotto.

Il modello CCDM esiste in modo che le informazioni sul prodotto possano risiedere in **un catalogo componibile** arricchito da altri sistemi, mentre gli storefront ricevono ancora assortimenti e prezzi **appropriati** per il canale.

## Modifiche al modello dati del catalogo componibile

In Adobe Commerce Optimizer, i dati di prodotto sono **acquisiti in un catalogo di base unificato** da una o più **origini catalogo** (ad esempio, una lingua come `en-US` o sistemi upstream come un PIM o un ERP). Tale origine fornisce gli attributi e i valori non elaborati.

**Visualizzazioni catalogo**, quindi, definisci come i dati unificati sono **organizzati ed esposti** per un contesto aziendale: quali prodotti soddisfano i **criteri**, quali **listini prezzi** e quali **origini catalogo** supportano la visualizzazione. Gli stessi record sottostanti possono quindi supportare **molte proiezioni**, ad esempio visualizzazioni separate per rivenditore, regione o marchio, senza duplicare l&#39;intero catalogo per ciascun sito.

Questa separazione:**da cui provengono i dati** (origine catalogo) rispetto a **come vengono presentati** (visualizzazione catalogo) è il motivo principale per cui i team adottano CCDM anziché gestire cataloghi paralleli per canale.

## Visualizzazioni catalogo come obiettivo della vetrina

Come descritto in [Visualizzazioni catalogo per Merchandising Services](https://experienceleague.adobe.com/en/docs/commerce/optimizer/setup/catalog-view){target="_blank"}, una visualizzazione catalogo si comporta come una **lente**: gli acquirenti visualizzano solo i prodotti, i prezzi e le regole consentiti, mentre il **catalogo base** rimane il sistema di record condiviso. Questo modello è direttamente abbinato a **Servizi di merchandising**, pertanto i client API passano la vista corretta (e le relative intestazioni) e ricevono una risposta coerente e basata su criteri per ogni esperienza.

Per informazioni più dettagliate su come questi pezzi si adattano a un flusso end-to-end, vedere la procedura dettagliata per sviluppatori [Creare un catalogo componibile per la vetrina](https://developer.adobe.com/commerce/services/optimizer/ccdm-use-case){target="_blank"}.

## Contenuto correlato

* [Scopri le visualizzazioni del catalogo](./learn-about-the-ccdm-feature-catalog-views.md)
* [Visualizzazioni catalogo per servizi di merchandising](https://experienceleague.adobe.com/en/docs/commerce/optimizer/setup/catalog-view){target="_blank"}
* [Creare un catalogo componibile per la vetrina](https://developer.adobe.com/commerce/services/optimizer/ccdm-use-case){target="_blank"}
* [Guida [!DNL Adobe Commerce Optimizer]](https://experienceleague.adobe.com/en/docs/commerce/optimizer/overview){target="_blank"}
* [Guida introduttiva all’API di merchandising](https://developer.adobe.com/commerce/services/optimizer/merchandising-services/using-the-api#make-your-first-request){target="_blank"}
