---
title: Informazioni sulle visualizzazioni del catalogo nel modello dati del catalogo componibile
description: Scopri come le visualizzazioni catalogo in Adobe Composable Catalog Data Model (CCDM) mappano un catalogo di base a ogni vetrina con prodotti, prezzi e regole distinti.
feature: Saas, Storefront
topic: Commerce
role: Developer
level: Beginner
doc-type: Tutorial
duration: 297
last-substantial-update: 2026-05-15T00:00:00Z
jira: KT-21132
source-git-commit: e3257f9713b26b0ab8ca2e827aeaac4532ff9dff
workflow-type: tm+mt
source-wordcount: '543'
ht-degree: 0%

---

# Visualizzazioni catalogo nel modello dati del catalogo componibile di Adobe

Le visualizzazioni del catalogo consentono di gestire ogni pubblico in modo diverso rispetto a un singolo catalogo componibile. Questo tutorial spiega cos&#39;è una visualizzazione catalogo, come Adobe Commerce Optimizer la applica a un acquirente e perché un ID visualizzazione univoco è l&#39;ancoraggio per prodotti, prezzi e regole, utilizzando lo scenario dimostrativo **Carvelo Automobiles**.

## A chi serve questo video?

* Architetti e sviluppatori di soluzioni Commerce che modellano cataloghi multi-brand o multi-dealer su Adobe Commerce Optimizer

## Contenuto video

* Lo scenario Carvelo Automobiles (marchi, concessionari e accordi)
* Che cos’è una visualizzazione catalogo e la metafora &quot;obiettivo&quot; su un catalogo di base unificato
* Come una vetrina utilizza una vista a catalogo per filtrare prodotti e prezzi (ad esempio, Celport)
* Visualizzazione del catalogo con ID univoci e valore aziendale di un’unica fonte di verità

>[!VIDEO](https://video.tv.adobe.com/v/3491261?learn=on)

## Scenario: Carvelo Automobiles

**Carvelo Automobiles** è una società fittizia di parti di ricambio per auto spesso utilizzata in dimostrazioni, documentazione e tutorial di Adobe Commerce. Carvelo vende parti in tre marchi: **Aurora**, **Bolt** e **Cruz** attraverso tre concessionari:

* **Arkbridge** appartiene a West Coast Inc.
* **Kingsbluff** e **Celport** appartengono a East Coast Inc.

Ogni concessionario ha un proprio accordo su quali prodotti può vendere. In questo modo viene creata una configurazione veramente complessa che può essere eseguita da **un singolo catalogo di base** di circa sei milioni di SKU. La funzionalità che rende possibile questa operazione è una **visualizzazione catalogo**.

## Che cos&#39;è una visualizzazione catalogo?

Una **visualizzazione catalogo** è una visualizzazione configurata del catalogo per un contesto aziendale specifico. Consideralo come un **obiettivo**. Il catalogo di base unificato si trova al centro, con ogni SKU per ogni marchio. Ogni concessionario ha la propria lente, la propria vista catalogo.

Nell’esempio:

* L&#39;obiettivo **Arkbridge** può mostrare tutte e tre le marche: Aurora, Bolt e Cruz.
* L&#39;obiettivo **Celport** mostra solo un sottoinsieme di parti Bolt e Cruz.

Ogni visualizzazione catalogo può anche controllare **i prezzi**. I dati del catalogo sottostante **non cambiano**. Solo gli aspetti visualizzabili (assortimento, prezzo e regole) cambiano per quel contesto.

## Applicazione di una visualizzazione catalogo in Commerce Optimizer

Quando un acquirente visita la vetrina **Celport**, Adobe Commerce Optimizer utilizza la **vista catalogo Celport** per determinare esattamente quali prodotti, prezzi e regole applicare. L&#39;acquirente vede solo quello che permette quell&#39;obiettivo.

Altri prodotti possono ancora esistere nel catalogo, ad esempio pneumatici Aurora, motori Bolt o batterie Cruz, ma la vetrina di **Celport non li espone mai** se la vista catalogo non lo consente.

## ID della vista catalogo e valore aziendale

Ogni vista catalogo ha un **ID univoco**. Questo ID è ciò che collega la vetrina alla sua configurazione del catalogo. È possibile impostarlo una sola volta nella configurazione di vetrina e seguire il comportamento a valle: i prodotti giusti, i prezzi corretti e le regole corrette.

Invece di mantenere cataloghi separati per ogni rivenditore o marchio e di mantenerli sincronizzati, è possibile gestire **un** catalogo componibile. Le visualizzazioni del catalogo consentono di modellare **diverse esperienze della vetrina** da quella **singola origine di verità**.

## Contenuto correlato

* [Guida [!DNL Adobe Commerce Optimizer]](https://experienceleague.adobe.com/en/docs/commerce/optimizer/overview){target="_blank"}
* [Guida introduttiva all’API di merchandising](https://developer.adobe.com/commerce/services/optimizer/merchandising-services/using-the-api/#make-your-first-request){target="_blank"}
