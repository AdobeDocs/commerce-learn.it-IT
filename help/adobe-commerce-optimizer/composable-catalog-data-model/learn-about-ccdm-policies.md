---
title: Informazioni sui criteri CCDM in Composable Catalog Data Model
description: Scopri in che modo i criteri STATIC e TRIGGER di Adobe Composable Catalog Data Model controllano la visibilità dei prodotti nelle visualizzazioni del catalogo senza ricostruire il catalogo.
feature: Saas, Storefront
topic: Commerce
role: Developer
level: Beginner
doc-type: Tutorial
duration: 349
last-substantial-update: 2026-05-21T00:00:00Z
jira: KT-21258
source-git-commit: 456f3cae8c45d137a195456692c2d11204126bb7
workflow-type: tm+mt
source-wordcount: '564'
ht-degree: 0%

---

# Criteri di Adobe Composable Catalog Data Model

Se una **vista catalogo** è l&#39;obiettivo che determina ciò che gli acquirenti vedono da un catalogo di base unificato, **criteri** sono ciò di cui è fatto l&#39;obiettivo. Questo tutorial spiega cosa è un criterio, come funzionano i criteri **STATIC** e **TRIGGER** nello scenario dimostrativo **Carvelo Automobiles** e perché l&#39;aggiornamento di un criterio ha effetto immediato, senza ricompilare il catalogo.

## A chi serve questo video?

* Architetti e sviluppatori di soluzioni Commerce che configurano le visualizzazioni del catalogo e le regole di merchandising in Adobe Commerce Optimizer

## Contenuto video

* Criteri come filtri di accesso ai dati per gli attributi del prodotto
* Combinazione di più criteri nella vista del catalogo Celport (marchi e categorie di parti)
* Regole STATICHE per regole aziendali permanenti
* Criteri TRIGGER attivati dalle intestazioni di richiesta API (ad esempio `AC-Policy-Brand`)
* Aggiornamento dei criteri nelle operazioni quotidiane senza ricompilazione del catalogo

>[!VIDEO](https://video.tv.adobe.com/v/3491413?learn=on)

Un **criterio** è un **filtro di accesso ai dati**. Esamina gli attributi del prodotto e applica le regole che determinano quali prodotti può esporre una vista catalogo. I criteri si trovano sopra il catalogo composabile condiviso e non duplicano i dati del catalogo.

## Criteri STATICI

Un criterio **STATIC** è **sempre attivo**. Impone regole aziendali permanenti indipendentemente dal comportamento dell’acquirente o dallo stato della sessione.

Nello scenario Celport, le regole STATICHE includono:

* Celport venderà solo i marchi **Bolt** e **Cruz**.
* Celport mostrerà solo **freni** e **sospensioni** parti.

Queste regole non si spengono mai. I criteri STATICI sono il luogo in cui sono attivi **contratti di licenza**, **restrizioni territoriali** e **autorizzazioni marchio**. Impostali una volta e Adobe Commerce Optimizer li applica automaticamente a ogni richiesta.

## Criteri di ATTIVAZIONE

I criteri con origine di valore **TRIGGER** sono talvolta denominati **criteri esclusivi**. La vista catalogo esegue il criterio **solo quando il trigger è specificato** nell&#39;intestazione della chiamata API.

Ad esempio, una vetrina EDS (Experience Delivery Services) potrebbe offrire un menu a discesa con **Tutti i marchi**, **Aurora**, **Bolt** e **Cruz**:

* Nella visualizzazione della pagina iniziale, non è selezionato nulla, pertanto non viene inviata alcuna intestazione marchio aggiuntiva.
* Quando l&#39;acquirente seleziona **Bolt**, l&#39;API sottostante imposta l&#39;intestazione `AC-Policy-Brand` su `Bolt`.
* Quando l&#39;acquirente seleziona **Cruz**, la stessa intestazione è impostata su `Cruz`.

La vista catalogo applica il criterio TRIGGER solo quando è presente l’intestazione, che supporta il filtro interattivo senza modificare le regole STATICHE permanenti.

## STATIC e TRIGGER insieme

I criteri **STATIC** gestiscono regole permanenti. **TRIGGER** criteri gestisce quelli interattivi. Utilizzati insieme in un&#39;unica vista catalogo, forniscono sia **conformità** che **flessibilità**: limiti di assortimento fissi e perfezionamento basato sull&#39;acquirente.

## Aggiornare i criteri senza ricompilare il catalogo

Per le operazioni quotidiane, i cambiamenti delle regole sono immediati. Supponiamo che il contratto di licenza di Celport ora consenta **pneumatici**, freni e sospensioni:

1. Apri il criterio pertinente.
1. Aggiungi **pneumatici** all&#39;elenco dei valori.
1. Salva.

La modifica è **attiva immediatamente**. Non sono previsti tempi di ricostruzione, ridistribuzione o attesa del catalogo. La successiva richiesta alla vetrina Celport riflette l’aggiornamento.

I criteri sono filtri leggeri in un **catalogo condiviso**, non regole raggruppate in copie di catalogo separate. **Modificare la regola, non i dati.**

## Contenuto correlato

* [Perché esiste il modello dati del catalogo componibile](./why-ccdm-exists.md)
* [Scopri le visualizzazioni del catalogo](./learn-about-the-ccdm-feature-catalog-views.md)
* [Visualizzazioni catalogo per servizi di merchandising](https://experienceleague.adobe.com/en/docs/commerce/optimizer/setup/catalog-view){target="_blank"}
* [Guida [!DNL Adobe Commerce Optimizer]](https://experienceleague.adobe.com/en/docs/commerce/optimizer/overview){target="_blank"}
* [Guida introduttiva all’API di merchandising](https://developer.adobe.com/commerce/services/optimizer/merchandising-services/using-the-api#make-your-first-request){target="_blank"}

