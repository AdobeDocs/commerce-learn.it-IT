---
title: Creare un GraphQL da utilizzare in Mesh API
description: Scopri come utilizzare più origini per Mesh API su Adobe Commerce e  [!DNL Adobe App Builder]. Scopri alcuni errori comuni e come risolverli.
jira: KT-21677
doc-type: Tutorial
duration: 381
last-substantial-update: 2023-02-08T00:00:00.000Z
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Developer
level: Beginner
exl-id: d788a068-9d20-4db0-a0eb-fd897873253d
TQID: https://experienceleague.adobe.com/O6ONn4NzMP-VqN0nsCoD-OPkZGMBelLWB-KNP1fZqmA
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2: id: b5a62a22-46f7-4f0d-b151-3fc640bef588id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2: id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: c73744d503de5023e5c001d0534200522db55b04
workflow-type: tm+mt
source-wordcount: 198
ht-degree: 0%

---

# Creare una mesh con più sorgenti

Questo video aiuta gli sviluppatori a comprendere come creare una mesh con più sorgenti in API Mesh per Adobe Developer App Builder. Questo video mostra come creare una mesh con più sorgenti e identificare gli errori. Per ulteriori dettagli ed esempi di codice, visita [Creare una rete](https://developer.adobe.com/graphql-mesh-gateway/mesh/basic/create-mesh){target="_blank"}.

## A chi serve questo video?

* Tutti gli utenti non esperti di API mesh
* Sviluppatori interessati a combinare più origini API e GraphQL

## Contenuto video

* Come utilizzare [trasformazioni](https://developer.adobe.com/graphql-mesh-gateway/mesh/basic/transforms/){target="_blank"} per modificare lo schema di origine predefinito
* Come risolvere gli errori, ad esempio conflitti di nomi, disponibilità dello schema e altri problemi di sintassi dello schema
* Aggiornamento della rete con una configurazione modificata

>[!VIDEO](https://video.tv.adobe.com/v/3414125?learn=on)

## Creare il file di configurazione JSON

API Mesh utilizza un file di configurazione JSON per definire i gestori di origine. Il file JSON contiene un array `sources` che contiene le origini per la rete. Di seguito è riportato un esempio di mesh con più sorgenti.

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
