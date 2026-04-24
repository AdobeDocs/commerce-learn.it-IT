---
title: 'Create a split payment POC: App Builder and AI tools'
description: Learn about a split payment proof of concept with App Builder and Commerce PaaS, including the goals, architecture, and what this first session covers.
feature: App Builder, Paas, Payments
topic: App Builder, Commerce, Development, Integrations
role: Developer, Leader, User
level: Intermediate
doc-type: Technical Video
duration: 259
jira: KT-20791
source-git-commit: 47b35088f2d3139d58791a2f7d327159db8f2175
workflow-type: tm+mt
source-wordcount: '576'
ht-degree: 0%

---

# Create a split payment POC: App Builder and AI tools

This is the first of a set of tutorialsthat introduce you to using AI-assisted development to help you build a split payment proof of concept. You work with Adobe App Builder and Adobe Commerce in the cloud (PaaS) or on-premises, moving from an overview in this session to the hands-on tutorials that follow. Expect goals, high-level architecture, and a roadmap to what the rest of the series covers.

## Video

>[!VIDEO](https://video.tv.adobe.com/v/3483933?learn=on)

## Important Details


This tutorial bridges the gap between where most Adobe Commerce teams are today — deep in PHP — and where Adobe wants them to go: event-driven, serverless business logic running outside Commerce in Adobe App Builder. It does this through a real, working feature rather than a toy example.

### What You&#39;ll Actually Build

A split payment system where customers pay using a combination of Cash on Delivery and Store Credit. After the order is placed, an operator (or ERP system) confirms or declines the cash payment through a standalone dashboard — without ever opening Commerce Admin. The entire accept/decline workflow runs in App Builder.
The Architectural Lesson (This Is the Core Teaching)
The tutorial demonstrates a deliberate and repeatable decision framework:

What must stay in PHP: anything that runs synchronously in the Commerce request cycle, or that calls Commerce-internal APIs with no clean external surface
What moves to App Builder: everything else — event processing, operator workflow, external integrations, and operator-facing tools

This isn&#39;t &quot;move everything to App Builder.&quot; It&#39;s a practical, honest starting point for teams who need to begin the transition without a rewrite.

### Why the PHP Code Isn&#39;t Included

The AI prompt approach is actually better than sample code for this use case, because the prompt captures the behavioral contracts and architectural reasoning that sample code alone cannot convey. A developer who runs the prompt and reads the output understands why the code is shaped the way it is, not just what it looks like.

### What Is Included

Complete App Builder application code (consistent across projects — use it directly or as a reference)
A full set of numbered AI prompts designed for Cursor and Claude, covering the Commerce module, the App Builder orchestrator, the operator dashboard, testing, and the path to production
Uno script di simulazione per testare il flusso di accettazione/rifiuto rispetto a un sito Commerce live senza la necessità di un ERP reale
Documentazione dell’architettura che illustra ogni decisione

### Per Chi È Questo

Sviluppatori on-premise su Adobe Commerce o Commerce Cloud che stanno iniziando la loro prima vera integrazione App Builder. Non per la distribuzione headless API-first, in particolare per i team in transizione che eseguono una vetrina tradizionale e che desiderano scoprire come scaricare la logica di business reale su App Builder senza abbandonare l’investimento PHP esistente.

### Callout prerequisiti

Adobe Commerce 2.4.5 o versione successiva, accesso a un’organizzazione Adobe Developer Console con un progetto App Builder ed Eventi di I/O abilitati. Per l’interfaccia utente di pagamento viene utilizzato un tema Luma pulito: i temi personalizzati richiederanno di regolare il prompt prima di eseguirlo.

### Considerazioni finali

Questa dovrebbe essere considerata una discussione di livello base sullo sviluppo basato sull’intelligenza artificiale. Questo tutorial è una dimostrazione per l’utilizzo di strumenti di intelligenza artificiale in un flusso di lavoro di sviluppo di Commerce, non solo un tutorial sulla suddivisione dei pagamenti.
