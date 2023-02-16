---
title: Creare una mesh sorgente singola GraphQL in mesh API
description: Scopri come utilizzare la mesh API su Adobe Commerce e [!DNL Adobe App Builder]. Scopri come creare una mesh con una sola origine.
landing-page-description: Scopri come utilizzare la mesh API su Adobe Commerce e [!DNL Adobe App Builder]. Scopri come creare una mesh con una sola origine.
kt: 11804
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
source-git-commit: 2cf7eb4d04f874ec42ca5f6d58ec841680376a41
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 0%

---

# Creare una mesh con una singola sorgente

Questo video aiuta gli sviluppatori a capire come creare una mesh con una singola sorgente in mesh API per Adobe Developer App Builder. Affinché questo esempio di base funzioni come previsto, è necessario un endpoint API o GraphQL accessibile al pubblico. Il video spiega anche come creare un `mesh.json` da utilizzare con la tua istanza Commerce. Per maggiori dettagli ed esempi di codice, visita [Creare una mesh](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/#create-a-mesh-1).

## A chi serve questo video?

* Chiunque sia nuovo alla mesh API
* Sviluppatori interessati alla combinazione di più origini GraphQL e API
* Chiunque abbia bisogno di sapere come filtrare la scheda di rete e filtrare da GraphQL

## Contenuto video

* Utilizzo di mesh API come proxy inverso
* Creazione di una mesh da un file di configurazione JSON
* Accesso all’endpoint GraphQL appena creato

>[!VIDEO](https://video.tv.adobe.com/v/3414124)

## Creare il file di configurazione json

API Mesh utilizza un file di configurazione JSON per definire i gestori sorgente. Il file JSON contiene un `sources` array che contiene le origini della mesh. Ecco un esempio di una mesh con una singola sorgente.

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
