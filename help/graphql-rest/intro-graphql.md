---
title: Introduzione a GraphQL
description: Scopri come utilizzare GraphQL su Adobe Commerce e  [!DNL Magento Open Source]. Utilizza le chiamate GET e POST di GraphQL per Adobe Commerce e  [!DNL Magento Open Source].
short-description: Scopri come utilizzare le chiamate GET e POST di GraphQL per Adobe Commerce e  [!DNL Magento Open Source].
kt: 11524
doc-type: video
audience: all
last-substantial-update: 2023-10-12T00:00:00Z
feature: GraphQL
topic: Commerce, Architecture, Headless
role: Architect, Developer
level: Beginner, Intermediate
exl-id: 8ea823da-24a3-4627-885c-4b3279b9142c
source-git-commit: b8b1e40a2f4d38954f0d21bc6f1a91b7ec0bd8c9
workflow-type: tm+mt
source-wordcount: '481'
ht-degree: 0%

---

# Introduzione a GraphQL

Questa è la parte 1 della serie per GraphQL e Adobe Commerce. GraphQL è rapidamente diventato lo standard di settore per la potenza delle applicazioni lato client che comunicano con un back-end. È un argomento sempre più rilevante per gli sviluppatori di Adobe Commerce, in quanto la piattaforma continua a espandere le proprie funzionalità nel regno delle implementazioni headless.

Se hai poca esperienza con GraphQL, questa sezione ti indirizza ai concetti e all’utilizzo di base.

>[!VIDEO](https://video.tv.adobe.com/v/3443949?captions=ita&learn=on)

## Video e tutorial correlati su GraphQL in questa serie

* [Parte 2 GraphQL - Query](../graphql-rest/graphql-queries.md)
* [Parte 3 GraphQL - Mutazioni](../graphql-rest/graphql-mutations.md)
* [Parte 4 GraphQL - Schema](../graphql-rest/graphql-schema.md)

## Cos’è GraphQL?

GraphQL è una specifica per un linguaggio di query API univoco e per il runtime che fornisce dati in risposta a tale linguaggio di query.

Le API web tradizionali come REST sono state utili per diversi sistemi che trasmettono dati avanti e indietro, ma hanno fornito prestazioni inferiori al picco per le moderne esperienze di collegamento alle app come le applicazioni web progressive. In applicazioni come questa, i livelli front-end e back-end dell&#39;applicazione _same_ comunicano tramite API Web. L’approccio regolamentato di schemi come REST spesso non fornisce la flessibilità appropriata in questo contesto, dove molti tipi di dati devono essere recuperati rapidamente.

GraphQL consente a un client di descrivere in modo esplicito _esattamente_ i dati necessari. Invece di richiedere più richieste di rete per recuperare più tipi di dati, una singola richiesta può eseguire query per molti tipi. Inoltre, le risposte sono mantenute snelle includendo (in un formato che rispecchia intuitivamente la query) solo i tipi e i campi richiesti.

Il runtime che implementa le specifiche GraphQL può essere creato in qualsiasi linguaggio. Adobe Commerce e [!DNL Magento Open Source] utilizzano
[graphql-php](https://webonyx.github.io/graphql-php/){target="_blank"} implementazione PHP e crea i propri livelli sopra di esso.

[Visualizza la documentazione completa di GraphQL](https://graphql.org/learn){target="_blank"}

## Utilizzo di un client GraphQL

Per testare esempi di codice e tutorial è necessario un client GraphQL con interfaccia grafica. Sono disponibili diverse opzioni:

* [Altair](https://altairgraphql.dev/){target="_blank"} è un client eccellente e completo di tutte le funzionalità creato appositamente per GraphQL. Adobe utilizza Altair nei video con procedura guidata.
* Se non desideri installare l’applicazione desktop, esistono anche estensioni Altair che vengono eseguite direttamente nel
  Browser [Chrome](https://chromewebstore.google.com/detail/altair-graphql-client/flnheeellpciglgpaodhkhmapeljopja){target="_blank"}, Firefox o [Edge](https://microsoftedge.microsoft.com/addons/detail/altair-graphql-client/kpggioiimijgcalmnfnalgglgooonopa){target="_blank"}.
* [GraphiQL](https://github.com/graphql/graphiql/tree/main/packages/graphiql){target="_blank"} è un&#39;implementazione dell&#39;IDE GraphQL di GraphQL Foundation. Non si tratta di uno strumento installabile, ma di un pacchetto che puoi utilizzare per creare l’interfaccia da solo.
* Se hai già familiarità con [Postman](https://www.postman.com/){target="_blank"}, questo offre un supporto decente per le query GraphQL, anche se non è completo quanto un client GraphQL dedicato.

Nel client GraphQL, devi inviare le richieste al percorso URL `/graphql` nell&#39;istanza Adobe Commerce o [!DNL Magento Open Source]. Se preferisci utilizzare un&#39;istanza esistente per i test, puoi utilizzare la demo del tema Venia (l&#39;implementazione di esempio di PWA Studio): `https://venia.magento.com/graphql`

{{$include /help/_includes/graphql-rest-related-links.md}}
