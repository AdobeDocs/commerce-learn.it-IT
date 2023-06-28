---
title: Introduzione a GraphQL
description: Scopri come utilizzare GraphQL su Adobe Commerce e [!DNL Magento Open Source]. Utilizzare le chiamate di GraphQL GET e POST per Adobe Commerce e [!DNL Magento Open Source].
landing-page-description: Scopri come utilizzare GraphQL su Adobe Commerce e [!DNL Magento Open Source]. Utilizzare le chiamate di GraphQL GET e POST per Adobe Commerce e [!DNL Magento Open Source].
short-description: Scopri come utilizzare GraphQL su Adobe Commerce e [!DNL Magento Open Source]. Utilizzare le chiamate di GraphQL GET e POST per Adobe Commerce e [!DNL Magento Open Source].
kt: 11524
doc-type: tutorial
audience: all
last-substantial-update: 2022-12-13T00:00:00Z
feature: GraphQL
topic: Commerce, Architecture, Headless
role: Architect, Developer
level: Beginner, Intermediate
exl-id: 8ea823da-24a3-4627-885c-4b3279b9142c
source-git-commit: 404d2708a6d540d6fb19a33afb20726356cd8000
workflow-type: tm+mt
source-wordcount: '493'
ht-degree: 0%

---

# Introduzione a GraphQL

GraphQL è rapidamente diventato lo standard di settore per la potenza delle applicazioni lato client che comunicano con un back-end. È un argomento sempre più rilevante per gli sviluppatori di Adobe Commerce, in quanto la piattaforma continua a espandere le proprie funzionalità nel regno delle implementazioni headless.

Se hai poca esperienza con GraphQL, questa sezione ti indirizza ai concetti e all’utilizzo di base.

## Cos’è GraphQL?

GraphQL è una specifica per un linguaggio di query API univoco e per il runtime che fornisce dati in risposta a tale linguaggio di query.

Le API web tradizionali come REST sono state utili per diversi sistemi che trasmettono dati avanti e indietro, ma hanno fornito prestazioni inferiori al picco per esperienze di collegamento alle app moderne come i Progressive Web Application. In applicazioni come questa, i livelli front-end e back-end della _uguale_ comunicazione tra applicazioni tramite API web. L’approccio regolamentato di schemi come REST spesso non fornisce la flessibilità appropriata in questo contesto, dove molti tipi di dati devono essere recuperati rapidamente.

GraphQL consente a un cliente di descrivere in modo esplicito _esattamente_ i dati di cui ha bisogno. Invece di richiedere più richieste di rete per recuperare più tipi di dati, una singola richiesta può eseguire query per molti tipi. Inoltre, le risposte sono mantenute snelle includendo (in un formato che rispecchia intuitivamente la query) solo i tipi e i campi richiesti.

Il runtime che implementa le specifiche GraphQL può essere creato in qualsiasi linguaggio. ADOBE COMMERCE e [!DNL Magento Open Source] utilizzare il
[graphql-php](https://webonyx.github.io/graphql-php/){target="_blank"} Implementazione PHP e si costruisce i propri livelli sopra di esso.

[Consulta la documentazione completa di GraphQL](https://graphql.org/learn){target="_blank"}

## Utilizzo di un client GraphQL

Per testare esempi di codice e tutorial è necessario un client GraphQL con interfaccia grafica. Sono disponibili diverse opzioni:

* [Altair](https://altairgraphql.dev/){target="_blank"} è un client eccellente e completo di tutte le funzionalità progettato appositamente per GraphQL. L’Adobe utilizza Altair nei video con procedura guidata.
* Se non desideri installare l’applicazione desktop, esistono anche estensioni Altair che vengono eseguite direttamente nel
  [Chrome](https://chrome.google.com/webstore/detail/altair-graphql-client/flnheeellpciglgpaodhkhmapeljopja){target="_blank"}, Firefox, or [Edge](https://microsoftedge.microsoft.com/addons/detail/altair-graphql-client/kpggioiimijgcalmnfnalgglgooonopa){target="_blank"} browser.
* [GraphiQL](https://github.com/graphql/graphiql/tree/main/packages/graphiql){target="_blank"} è un’implementazione dell’IDE di GraphQL di GraphQL Foundation. Non si tratta di uno strumento installabile, ma di un pacchetto che puoi utilizzare per creare l’interfaccia da solo.
* Se conosci già [Postman](https://www.postman.com/){target="_blank"}, dispone di un supporto decente per le query GraphQL, anche se non è disponibile completamente come un client GraphQL dedicato.

Nel client GraphQL, devi inviare le richieste al percorso URL `/graphql` sul tuo Adobe Commerce o [!DNL Magento Open Source] dell&#39;istanza. Se preferisci utilizzare un’istanza esistente per i test, puoi utilizzare la demo del tema Venia (l’implementazione di esempio di PWA Studi): `https://venia.magento.com/graphql`

{{$include /help/_includes/graphql-rest-related-links.md}}
