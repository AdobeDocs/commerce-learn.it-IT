---
title: Creare un GraphQL da utilizzare in Mesh API
description: Scopri come utilizzare più origini per Mesh API su Adobe Commerce e [!DNL Adobe App Builder]. Scopri alcuni errori comuni e come risolverli.
landing-page-description: Scopri come utilizzare API Mesh su Adobe Commerce e [!DNL Adobe App Builder]. Scopri come creare una rete con più origini e come risolvere alcuni errori comuni.
short-description: Scopri come utilizzare API Mesh su Adobe Commerce e [!DNL Adobe App Builder]. Scopri come creare una rete con più origini e come risolvere alcuni errori comuni.
kt: 11804
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Architect, Developer
level: Beginner, Intermediate
exl-id: d788a068-9d20-4db0-a0eb-fd897873253d
source-git-commit: 404d2708a6d540d6fb19a33afb20726356cd8000
workflow-type: tm+mt
source-wordcount: '243'
ht-degree: 0%

---

# Creare una mesh con più sorgenti

Questo video aiuta gli sviluppatori a comprendere come creare una rete con più sorgenti in API Mesh per Adobe Developer App Builder. Questo video mostra come creare una mesh con più sorgenti e identificare gli errori. Per ulteriori dettagli ed esempi di codice, visita [Creare una trama](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/#create-a-mesh-1){target="_blank"}.

## A chi serve questo video?

* Tutti gli utenti non esperti di API mesh
* Sviluppatori interessati a combinare più origini API e GraphQL

## Contenuto video

* Come usare [trasformazioni](https://developer.adobe.com/graphql-mesh-gateway/gateway/transforms/){target="_blank"} per modificare lo schema di origine predefinito
* Come risolvere gli errori, ad esempio conflitti di nomi, disponibilità dello schema e altri problemi di sintassi dello schema
* Aggiornamento della rete con una configurazione modificata

>[!VIDEO](https://video.tv.adobe.com/v/3414125?quality=12&learn=on)

## Creare il file di configurazione json

API Mesh utilizza un file di configurazione JSON per definire i gestori di origine. Il file JSON contiene un `sources` matrice contenente le origini della mesh. Di seguito è riportato un esempio di mesh con più sorgenti.

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
        "name": "Example",
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
