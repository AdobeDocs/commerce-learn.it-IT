---
title: Creare un GraphQL sorgente multiplo da utilizzare in mesh API
description: Scopri come utilizzare più sorgenti per API Mesh su Adobe Commerce e [!DNL Adobe App Builder]. Scopri alcuni errori comuni e come risolverli.
landing-page-description: Scopri come utilizzare la mesh API su Adobe Commerce e [!DNL Adobe App Builder]. Scopri come creare una richiesta con più origini e come risolvere alcuni errori comuni.
kt: 11804
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
source-git-commit: b6d501c5c852e1cc2cf1f05f91b5a9d96ac7d036
workflow-type: tm+mt
source-wordcount: '278'
ht-degree: 0%

---

# Creare una mesh API GraphQL sorgente multipla

Il video aiuta uno sviluppatore a capire come creare un proxy inverso GraphQL con più sorgenti. Questo video mostra come unire diverse sorgenti, identificare gli errori e salvare le modifiche in git. Per esempi di codice di base utilizzati nel video, visita [Creare una mesh](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/#create-a-mesh-1).

## A chi serve questo video?

* Chiunque abbia poca esperienza con la mesh API
* Sviluppatori interessati all’utilizzo di più origini graphql
* Chiunque abbia bisogno di sapere come filtrare la scheda di rete e filtrare per graphql

## Contenuto video

* Come lo schema API degli attributi personalizzati complessi da una seconda origine può sovrascrivere lo schema di origine predefinito
* Modifica della configurazione della mesh api per tenere conto del secondo schema di override
* Come risolvere gli errori che possono verificarsi nel processo, come conflitto di denominazione, disponibilità dello schema e altra sintassi SDL
* Esempio di errori comuni dopo tentativi di unione degli schemi
* Ricostruzione della mesh api dopo le modifiche
* Salvataggio delle modifiche a Git dopo la modifica della configurazione di API Mesh

>[!VIDEO](https://video.tv.adobe.com/v/3414125)

## Creare il file di configurazione json

Ad Adobe, App Builder per sapere di tutte le tue sorgenti, le definisci in una configurazione JSON. Ogni sorgente è un elemento di un array e puoi avere uno o più elementi. Ecco un esempio di una richiesta sorgente multipla collegata per formare una singola risposta.

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
      },
      {
        "name": "ERP",
        "handler": {
          "graphql": {
            "endpoint": "https://www.example.com/graphql/"
          }
        }
      }
    ]
  }
}
```

{{$include /help/_includes/api-mesh-related-links.md}}
