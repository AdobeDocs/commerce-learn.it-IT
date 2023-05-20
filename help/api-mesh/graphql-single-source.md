---
title: Creare una rete a sorgente singola GraphQL in Mesh API
description: Scopri come utilizzare API Mesh su Adobe Commerce e [!DNL Adobe App Builder]. Scopri come creare una rete con una sola origine.
landing-page-description: Scopri come utilizzare API Mesh su Adobe Commerce e [!DNL Adobe App Builder]. Scopri come creare una rete con una sola origine.
short-description: Scopri come utilizzare API Mesh su Adobe Commerce e [!DNL Adobe App Builder]. Scopri come creare una rete con una sola origine.
kt: 11804
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
exl-id: 9a78457a-1539-49c0-ac69-4bbfc6786137
source-git-commit: edb98cf6544954d741c43beb39f4056326c7d26b
workflow-type: tm+mt
source-wordcount: '242'
ht-degree: 0%

---

# Creare una trama con un&#39;unica origine

Questo video aiuta gli sviluppatori a comprendere come creare una trama con un’unica origine in API Mesh per Adobe Developer App Builder. Affinché questo esempio di base funzioni come previsto, è necessario un endpoint API o GraphQL accessibile al pubblico. Il video spiega anche come creare un `mesh.json` da utilizzare con l’istanza Commerce. Per ulteriori dettagli ed esempi di codice, visita [Creare una trama](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/#create-a-mesh-1){target="_blank"}.

## A chi serve questo video?

* Qualsiasi nuovo utente di API mesh
* Sviluppatori interessati a combinare più origini GraphQL e API
* Chiunque debba sapere come filtrare la scheda di rete e filtrare per GraphQL

## Contenuto video

* Utilizzo di API Mesh come proxy inverso
* Creazione di una trama da un file di configurazione JSON
* Accesso all’endpoint GraphQL appena creato

>[!VIDEO](https://video.tv.adobe.com/v/3414124?quality=12&learn=on)

## Creare il file di configurazione json

API Mesh utilizza un file di configurazione JSON per definire i gestori di origine. Il file JSON contiene un `sources` matrice contenente le origini della mesh. Di seguito è riportato un esempio di mesh con un&#39;unica origine.

```json
{
"meshConfig": {
    "sources": [
      {
        "name": "Commerce",
        "handler": {
          "graphql": {
            "endpoint": "https://venia.magento.com/graphql/"
          }
        }
      }
    ]
  }
}
```

{{$include /help/_includes/api-mesh-related-links.md}}
