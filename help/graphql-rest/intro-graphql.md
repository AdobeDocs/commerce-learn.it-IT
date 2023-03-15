---
title: Introduzione a GraphQL
description: Scopri come utilizzare GraphQL su Adobe Commerce e [!DNL Magento Open Source]. Utilizzare chiamate GraphQL GET e POST per Adobe Commerce e [!DNL Magento Open Source].
landing-page-description: Scopri come utilizzare GraphQL su Adobe Commerce e [!DNL Magento Open Source]. Utilizzare chiamate GraphQL GET e POST per Adobe Commerce e [!DNL Magento Open Source].
short-description: Learn how to use GraphQL on Adobe Commerce and [!DNL Magento Open Source]. Use GraphQL GET and POST calls for Adobe Commerce and [!DNL Magento Open Source].
kt: 11524
doc-type: tutorial
audience: all
last-substantial-update: 2022-12-13T00:00:00Z
exl-id: 8ea823da-24a3-4627-885c-4b3279b9142c
source-git-commit: 67d21ca23cdccc87cdeed4a08a3ebb48e5bd1030
workflow-type: tm+mt
source-wordcount: '474'
ht-degree: 0%

---

# Introduzione a GraphQL

GraphQL è diventato rapidamente lo standard di settore per la capacità delle applicazioni lato client di comunicare con un back-end. Si tratta di un argomento sempre più rilevante per gli sviluppatori di Adobe Commerce, poiché la piattaforma continua ad espandere le sue capacità nel regno delle implementazioni headless.

Se hai poca esperienza con GraphQL, questa sezione ti orienta ai concetti e all’utilizzo di base.

## Cos&#39;è GraphQL?

GraphQL è una specifica per un linguaggio di query API univoco e per il runtime che fornisce i dati in risposta a tale linguaggio di query.

Le API web tradizionali come REST sono state utili per diversi sistemi che trasmettono dati avanti e indietro, ma hanno fornito prestazioni meno elevate per esperienze di collegamento app moderne come Progressive Web Application. In applicazioni come questa, gli strati front-end e back-end della _stesso_ l&#39;applicazione comunica tramite API web. L&#39;approccio standardizzato di schemi come il REST spesso non fornisce la flessibilità appropriata in questo contesto, dove molti tipi di dati devono essere recuperati rapidamente.

GraphQL consente a un cliente di descrivere in modo espressivo _esattamente_ i dati di cui ha bisogno. Invece di richiedere più richieste di rete per recuperare più tipi di dati, una singola richiesta può eseguire query per molti tipi. Inoltre, le risposte vengono mantenute magre includendo (in un formato che rispecchia intuitivamente la query) solo i tipi e i campi richiesti.

Il runtime che implementa la specifica GraphQL può essere generato in qualsiasi lingua. Adobe Commerce e [!DNL Magento Open Source] utilizza
[graphql-php](https://webonyx.github.io/graphql-php/){target="_blank"} Implementazione PHP e crea i propri livelli sopra di esso.

[Visualizza la documentazione completa di GraphQL](https://graphql.org/learn){target="_blank"}

## Utilizzo di un client GraphQL

È necessario un client GUI GraphQL per testare campioni di codice ed esercitazioni. Sono disponibili diverse opzioni:

* [Altair](https://altairgraphql.dev/){target="_blank"} è un cliente eccellente e completo costruito appositamente per GraphQL. L&#39;Adobe utilizza Altair nei video di approfondimento.
* Se non desideri installare l’applicazione desktop, ci sono anche le estensioni Altair che vengono eseguite direttamente nel tuo
   [Chrome](https://chrome.google.com/webstore/detail/altair-graphql-client/flnheeellpciglgpaodhkhmapeljopja){target="_blank"}, Firefox, or [Edge](https://microsoftedge.microsoft.com/addons/detail/altair-graphql-client/kpggioiimijgcalmnfnalgglgooonopa){target="_blank"} browser.
* [GraphiQL](https://github.com/graphql/graphiql/tree/main/packages/graphiql){target="_blank"} è un’implementazione dell’IDE GraphQL da GraphQL Foundation. Questo non è uno strumento installabile, ma un pacchetto che puoi utilizzare per creare l&#39;interfaccia da solo.
* Se hai già familiarità con [Postman](https://www.postman.com/){target="_blank"}, dispone di un supporto decente per le query GraphQL, anche se non è così completo come un client GraphQL dedicato.

Nel client GraphQL, devi inviare le richieste al percorso URL `/graphql` sul tuo Adobe Commerce o [!DNL Magento Open Source] istanza. Se preferisci utilizzare un’istanza esistente per i test, puoi utilizzare la demo del tema Venia (l’implementazione di esempio di PWA Studi): `https://venia.magento.com/graphql`

{{$include /help/_includes/graphql-rest-related-links.md}}
