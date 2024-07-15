---
title: Eseguire una query tramite GraphQL
description: Scopri come eseguire una query utilizzando GraphQL su Adobe Commerce e  [!DNL Magento Open Source]. Questa è un’introduzione a GraphQL utilizzando le chiamate GET e POST.
landing-page-description: Scopri come eseguire una query utilizzando GraphQL su Adobe Commerce e  [!DNL Magento Open Source]. Questa è un’introduzione a GraphQL utilizzando le chiamate GET e POST.
short-description: Scopri come eseguire una query utilizzando GraphQL su Adobe Commerce e  [!DNL Magento Open Source]. Questa è un’introduzione a GraphQL utilizzando le chiamate GET e POST.
kt: 13937
doc-type: video
audience: all
last-substantial-update: 2023-10-12T00:00:00Z
feature: GraphQL
topic: Commerce, Architecture, Headless
role: Architect, Developer
level: Beginner, Intermediate
exl-id: 443d711d-ec74-4e07-9357-fbbe0f774853
source-git-commit: 2041bbf1a2783975091b9806c12fc3c34c34582f
workflow-type: tm+mt
source-wordcount: '984'
ht-degree: 0%

---

# Query GraphQL

Questa è la parte 2 della serie per GraphQL e Adobe Commerce. In questo tutorial e video, scopri le query GraphQL e come eseguirle su Adobe Commerce.

>[!VIDEO](https://video.tv.adobe.com/v/3424120?learn=on)

## Video e tutorial correlati su GraphQL in questa serie

* [Parte 1 GraphQL - Introduzione](../graphql-rest/intro-graphql.md)
* [Parte 3 GraphQL - Mutazioni](../graphql-rest/graphql-mutations.md)
* [Parte 4 GraphQL - Schema](../graphql-rest/graphql-schema.md)

## Esempio di sintassi GraphQL

Immergiamo direttamente nella sintassi delle query GraphQL con un esempio completo. (Ricorda, puoi provare questa cosa da solo contro https://venia.magento.com/graphql.)

Osserva la seguente query GraphQL, suddivisa pezzo per pezzo:

```graphql
{
    country (
        id: "US"
    ) {
        id
        full_name_english
    }

    categories(
        filters: {
            name: {
                match: "Tops"
            }
        }
    ) {
        items {
            name
            products(
                pageSize: 10,
                currentPage: 2
            ) {
                items {
                    sku
                }
            }
        }
    }
}
```

Una risposta plausibile da parte di un server GraphQL per la query di cui sopra potrebbe essere:

```json
{
  "data": {
    "country": {
      "id": "US",
      "full_name_english": "United States"
    },
    "categories": {
      "items": [
        {
          "name": "Tops",
          "products": {
            "items": [
              {
                "sku": "VSW06"
              },
              {
                "sku": "VT06"
              },
              {
                "sku": "VSW07"
              },
              {
                "sku": "VT07"
              },
              {
                "sku": "VSW08"
              },
              {
                "sku": "VT08"
              },
              {
                "sku": "VSW09"
              },
              {
                "sku": "VT09"
              },
              {
                "sku": "VSW10"
              },
              {
                "sku": "VT10"
              }
            ]
          }
        }
      ]
    }
  }
}
```

L’esempio precedente si basa sullo schema predefinito di GraphQL per Adobe Commerce, definito sul server. In questa richiesta,
eseguire query su più tipi di dati contemporaneamente. La query esprime esattamente i campi desiderati e i dati restituiti sono formattati
in modo simile alla query stessa.

>[!NOTE]
>
>I client GraphQL offuscano la forma dell’effettivo invio della richiesta HTTP, ma questo è facile da scoprire. Se si utilizza un client basato su browser, osservare la scheda [!UICONTROL Network] quando viene inviata una query. La richiesta contiene un corpo non elaborato costituito da &quot;query: `{string}`&quot;, dove `{string}` è semplicemente la stringa non elaborata dell&#39;intera query. Se la richiesta viene inviata come GET, la query potrebbe invece essere codificata nel parametro della stringa di query &quot;query&quot;. A differenza di REST, il tipo di richiesta HTTP non ha importanza, solo il contenuto della query.


## Ricerca di ciò che si desidera

`country` e `categories` nell&#39;esempio rappresentano due &quot;query&quot; diverse per due diversi tipi di dati. A differenza di un paradigma API tradizionale come REST, che definirebbe endpoint separati ed espliciti per ogni tipo di dati. GraphQL offre la flessibilità di eseguire query su un singolo endpoint con un’espressione che può recuperare più tipi di dati contemporaneamente.

Analogamente, la query specifica esattamente i campi desiderati sia per `country` (`id` e `full_name_english`) che per `categories` (`items`, che dispone di una sottoselezione di campi) e i dati ricevuti rispecchiano la specifica del campo. Presumibilmente sono disponibili molti più campi per questi tipi di dati, ma puoi recuperare solo ciò che hai richiesto.


>[!NOTE]
>
>È possibile notare che il valore restituito per `items` è in realtà un _array_ di valori, ma si stanno comunque selezionando direttamente i sottocampi. Quando il tipo di un campo è un elenco, GraphQL comprende implicitamente le sottoselezioni da applicare a ogni elemento dell’elenco.

## Argomenti

Mentre i campi da restituire sono specificati tra parentesi graffe di ciascun tipo, gli argomenti e i relativi valori denominati vengono specificati tra parentesi dopo il nome del tipo. Gli argomenti sono spesso facoltativi e influiscono sul modo in cui i risultati delle query vengono filtrati, formattati o altrimenti trasformati.

Si sta passando un argomento `id` a `country`, specificando il paese specifico da interrogare e un argomento `filters` per `categories`.

## Campi fino in fondo

Anche se si tende a considerare `country` e `categories` come query o entità separate, l&#39;intera struttura espressa nella query in realtà è costituita solo da campi. L&#39;espressione di `products` non è sintatticamente diversa da quella di `categories`. Entrambi sono campi, e non c&#39;è differenza tra la loro costruzione.

Qualsiasi grafo dati di GraphQL ha un singolo tipo &quot;radice&quot; (in genere indicato come `Query`) per avviare la struttura ad albero e i tipi spesso considerati entità vengono assegnati ai campi di questa radice. La query di esempio sta eseguendo una query generica per il tipo radice e selezionando i campi `country` e `categories`. Quindi seleziona i sottocampi di quei campi e così via, potenzialmente con diversi livelli di profondità. Se il tipo restituito di un campo è un tipo complesso, ad esempio un tipo con campi specifici anziché scalare, continuare a selezionare i campi desiderati.

Questo concetto di campi nidificati è anche il motivo per cui è possibile passare argomenti per `products` (`pageSize` e `currentPage`) nello stesso modo in cui si è fatto per il campo `categories` di livello superiore.

![Struttura campi GraphQL](../assets/graphql-field-tree.png)

## Variabili

Proviamo con un’altra query:

```graphql
query getProducts(
    $search: String
) {
    products(
        search: $search
    ) {
        items {
            ...productDetails
            related_products {
                ...productDetails
            }
        }
    }
}

fragment productDetails on ProductInterface {
    sku
    name
}
```

La prima cosa da notare è l&#39;aggiunta della parola chiave `query` prima della parentesi graffa di apertura della query, insieme al nome di un&#39;operazione (`getProducts`). Il nome dell&#39;operazione è arbitrario e non corrisponde ad alcun elemento nello schema del server. Questa sintassi è stata aggiunta per supportare l’introduzione delle variabili.

Nella query precedente, i valori hardcoded per gli argomenti dei campi vengono inseriti direttamente come stringhe o numeri interi. La specifica di GraphQL, tuttavia, dispone di un supporto di prima classe per separare l’input dell’utente dalla query principale utilizzando le variabili.

Nella nuova query si utilizzano le parentesi prima della parentesi graffa di apertura dell&#39;intera query per definire una variabile `$search` (le variabili utilizzano sempre la sintassi del prefisso del segno del dollaro). Questa variabile viene fornita all&#39;argomento `search` per `products`.

Quando una query contiene variabili, la richiesta GraphQL deve includere un dizionario di valori con codifica JSON separato accanto alla query stessa. Per la query precedente, oltre al corpo della query puoi inviare anche il seguente JSON di valori di variabile:

```json
{
    "search": "VT01"
}
```

>[!NOTE]
>
>Se si stanno provando queste query sul sito di esempio Venia anziché sulla propria istanza Adobe Commerce, è probabile che i risultati restituiti siano vuoti per `related_products`.

In qualsiasi client che supporta GraphQL e che utilizzi per i test (ad esempio Altair e GraphiQL), l’interfaccia utente supporta l’immissione delle variabili JSON separatamente dalla query.

Proprio come hai visto che la richiesta HTTP effettiva per una query GraphQL contiene &quot;query: `{string}`&quot; nel corpo, qualsiasi richiesta contenente un dizionario di variabili include semplicemente un &quot;variables: `{json}`&quot; aggiuntivo nello stesso corpo, dove `{json}` è la stringa JSON con i valori della variabile.

La nuova query utilizza anche un _frammento_ (`productDetails`) per riutilizzare la stessa selezione di campi in più posizioni. [Ulteriori informazioni sui frammenti](https://graphql.org/learn/queries/#fragments){target="_blank"} nella documentazione di GraphQL.

{{$include /help/_includes/graphql-rest-related-links.md}}
