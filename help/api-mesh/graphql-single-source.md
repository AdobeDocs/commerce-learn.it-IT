---
title: Creare una richiesta sorgente singola GraphQL da utilizzare in mesh API
description: Scopri come utilizzare la mesh API su Adobe Commerce e [!DNL Adobe App Builder]. Scopri come creare una richiesta con una sola origine.
landing-page-description: Scopri come utilizzare la mesh API su Adobe Commerce e [!DNL Adobe App Builder]. Scopri come creare una richiesta con una sola origine.
kt: 11804
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
source-git-commit: b6d501c5c852e1cc2cf1f05f91b5a9d96ac7d036
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# Creare una mesh API GraphQL a sorgente singola

Il video aiuta uno sviluppatore a capire come creare un proxy inverso GraphQL e ha una singola sorgente. Per il corretto funzionamento di GraphQL Mesh, è necessario un URL accessibile al pubblico con schema GraphQL valido. Il video spiega anche come impostare il json iniziale da utilizzare con il sito web commerce. Per esempi di codice di base utilizzati nel video, visita [Creare una mesh](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/#create-a-mesh-1).

## A chi serve questo video?

* Chiunque abbia poca esperienza con la mesh API
* Sviluppatori interessati all’utilizzo di più origini graphql
* Chiunque abbia bisogno di sapere come filtrare la scheda di rete e filtrare per graphql

## Contenuto video

* Utilizzo dell’API come proxy inverso
* Configurazione JSON con l’interfaccia della riga di comando di Adobe Developer
* Accesso all’endpoint GraphQL appena creato

>[!VIDEO](https://video.tv.adobe.com/v/3414124)

## Creare il file di configurazione json

Ad Adobe, App Builder per sapere di tutte le tue sorgenti, le definisci in una configurazione JSON. Ogni sorgente è un elemento di un array e puoi avere uno o più elementi. Esempio di una singola origine

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
