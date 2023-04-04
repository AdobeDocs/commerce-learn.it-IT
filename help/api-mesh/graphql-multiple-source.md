---
title: Creare un GraphQL sorgente multiplo da utilizzare in mesh API
description: Scopri come utilizzare più sorgenti per API Mesh su Adobe Commerce e [!DNL Adobe App Builder]. Scopri alcuni errori comuni e come risolverli.
landing-page-description: Scopri come utilizzare la mesh API su Adobe Commerce e [!DNL Adobe App Builder]. Scopri come creare una mesh con più sorgenti e come risolvere alcuni errori comuni.
short-description: Discover how to use API Mesh on Adobe Commerce and [!DNL Adobe App Builder]. Learn about creating a mesh that has multiple sources and how to resolve some common errors.
kt: 11804
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
source-git-commit: d85426bcf3ae0412a433414d70c874964905dda0
workflow-type: tm+mt
source-wordcount: '217'
ht-degree: 0%

---

# Creare una mesh con più sorgenti

Questo video aiuta gli sviluppatori a capire come creare una mesh con più sorgenti in mesh API per Adobe Developer App Builder. Questo video mostra come creare una mesh con più sorgenti e identificare gli errori. Per maggiori dettagli ed esempi di codice, visita [Creare una mesh](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/#create-a-mesh-1){target="_blank"}.

## A chi serve questo video?

* Chiunque abbia poca esperienza con la mesh API
* Sviluppatori interessati alla combinazione di più origini API e GraphQL

## Contenuto video

* Come utilizzare [trasformazioni](https://developer.adobe.com/graphql-mesh-gateway/gateway/transforms/){target="_blank"} per modificare lo schema di origine predefinito
* Come risolvere i problemi relativi agli errori, ad esempio i conflitti dei nomi, la disponibilità dello schema e altri problemi di sintassi dello schema
* Aggiornamento della mesh con una configurazione modificata

>[!VIDEO](https://video.tv.adobe.com/v/3414125?quality=12&learn=on)

## Creare il file di configurazione json

API Mesh utilizza un file di configurazione JSON per definire i gestori sorgente. Il file JSON contiene un `sources` array che contiene le origini della mesh. Ecco un esempio di una mesh con più sorgenti.

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
