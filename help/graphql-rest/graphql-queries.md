---
title: Eseguire una query tramite GraphQL
description: Scopri come eseguire una query utilizzando GraphQL su Adobe Commerce e [!DNL Magento Open Source]. Questa è un’introduzione a GraphQL utilizzando le chiamate GET e POST.
landing-page-description: Scopri come eseguire una query utilizzando GraphQL su Adobe Commerce e [!DNL Magento Open Source]. Questa è un’introduzione a GraphQL utilizzando le chiamate GET e POST.
short-description: Scopri come eseguire una query utilizzando GraphQL su Adobe Commerce e [!DNL Magento Open Source]. Questa è un’introduzione a GraphQL utilizzando le chiamate GET e POST.
kt: 11524
doc-type: tutorial
audience: all
last-substantial-update: 2022-12-13T00:00:00Z
feature: GraphQL
topic: Commerce, Architecture, Headless
role: Architect, Developer
level: Beginner, Intermediate
exl-id: 443d711d-ec74-4e07-9357-fbbe0f774853
source-git-commit: 404d2708a6d540d6fb19a33afb20726356cd8000
workflow-type: tm+mt
source-wordcount: '937'
ht-degree: 0%

---

# Query GraphQL

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

L’esempio precedente si basa sullo schema predefinito di GraphQL per Adobe Commerce, definito sul server. In questa richiesta, si eseguono query su più tipi di dati contemporaneamente. La query esprime esattamente i campi desiderati e i dati restituiti sono formattati in modo simile alla query stessa.

>[!NOTE]
>
>I client GraphQL offuscano la forma dell’effettivo invio della richiesta HTTP, ma questo è facile da scoprire. Se utilizzi un client basato su browser, osserva [!UICONTROL Network] quando viene inviata una query. La richiesta contiene un corpo non elaborato costituito da &quot;query: `{string}`&quot;, dove `{string}` è semplicemente la stringa non elaborata dell’intera query. Se la richiesta viene inviata come GET, la query potrebbe invece essere codificata nel parametro della stringa di query &quot;query&quot;. A differenza di REST, il tipo di richiesta HTTP non ha importanza, solo il contenuto della query.


## Ricerca di ciò che si desidera

`country` e `categories` nell’esempio, rappresentano due &quot;query&quot; diverse per due tipi diversi di dati. A differenza di un paradigma API tradizionale come REST, che definirebbe endpoint separati ed espliciti per ogni tipo di dati. GraphQL offre la flessibilità di eseguire query su un singolo endpoint con un’espressione che può recuperare più tipi di dati contemporaneamente.

Analogamente, la query specifica esattamente i campi desiderati per entrambi `country` (`id` e `full_name_english`) e `categories` (`items`, che dispone di una sottoselezione di campi) e i dati ricevuti rispecchiano tale specifica di campo. Presumibilmente sono disponibili molti più campi per questi tipi di dati, ma puoi recuperare solo ciò che hai richiesto.


>[!NOTE]
>
>Il valore restituito per `items` è in realtà un _array_ di valori, ma si selezionano comunque direttamente i relativi sottocampi. Quando il tipo di un campo è un elenco, GraphQL comprende implicitamente le sottoselezioni da applicare a ogni elemento dell’elenco.

## Argomenti

Mentre i campi da restituire sono specificati tra parentesi graffe di ciascun tipo, gli argomenti e i relativi valori denominati vengono specificati tra parentesi dopo il nome del tipo. Gli argomenti sono spesso facoltativi e influiscono sul modo in cui i risultati delle query vengono filtrati, formattati o altrimenti trasformati.

Stai passando un `id` argomento a `country`, specificando il paese in cui eseguire la query e `filters` argomento per `categories`.

## Campi fino in fondo

Mentre si tende a pensare a `country` e `categories` come query o entità separate, l’intera struttura espressa nella query in realtà è costituita solo da campi. L’espressione di `products` non è sintatticamente diverso da quello di `categories`. Entrambi sono campi, e non c&#39;è differenza tra la loro costruzione.

Qualsiasi grafico dati di GraphQL ha un singolo tipo &quot;radice&quot; (in genere a cui si fa riferimento `Query`) per avviare la struttura ad albero e i tipi spesso considerati entità vengono assegnati ai campi di questa radice. La query di esempio sta eseguendo una query generica per il tipo di radice e selezionando i campi `country` e `categories`. Quindi seleziona i sottocampi di quei campi e così via, potenzialmente con diversi livelli di profondità. Se il tipo restituito di un campo è un tipo complesso, ad esempio un tipo con campi specifici anziché scalare, continuare a selezionare i campi desiderati.

Questo concetto di campi nidificati è anche il motivo per cui puoi trasmettere argomenti per `products` (`pageSize` e `currentPage`) come per il livello superiore `categories` campo.

![Albero campo GraphQL](../assets/graphql-field-tree.png)

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

La prima cosa da notare è la parola chiave aggiunta `query` prima della parentesi graffa di apertura della query, insieme al nome di un&#39;operazione (`getProducts`). Il nome dell&#39;operazione è arbitrario e non corrisponde a nulla nello schema del server. Questa sintassi è stata aggiunta per supportare l’introduzione delle variabili.

Nella query precedente, i valori hardcoded per gli argomenti dei campi vengono inseriti direttamente come stringhe o numeri interi. La specifica di GraphQL, tuttavia, dispone di un supporto di prima classe per separare l’input dell’utente dalla query principale utilizzando le variabili.

Nella nuova query si utilizzano le parentesi prima della parentesi graffa di apertura dell&#39;intera query per definire un `$search` variabile (le variabili utilizzano sempre la sintassi del prefisso del simbolo del dollaro). È questa variabile che viene fornita al `search` argomento per `products`.

Quando una query contiene variabili, la richiesta GraphQL deve includere un dizionario di valori con codifica JSON separato accanto alla query stessa. Per la query precedente, oltre al corpo della query puoi inviare anche il seguente JSON di valori di variabile:

```json
{
    "search": "VT01"
}
```

>[!NOTE]
>
>Se stai provando queste query sul sito di esempio Venia anziché sulla tua istanza Adobe Commerce, i risultati restituiti saranno probabilmente vuoti per `related_products`.

In qualsiasi client che supporta GraphQL e che utilizzi per i test (ad esempio Altair e GraphiQL), l’interfaccia utente supporta l’immissione delle variabili JSON separatamente dalla query.

Proprio come hai visto che la richiesta HTTP effettiva per una query GraphQL contiene &quot;query: `{string}`&quot; nel corpo, qualsiasi richiesta contenente un dizionario di variabili include semplicemente un’ulteriore &quot;variabile: `{json}`&quot; nello stesso corpo, dove `{json}` è la stringa JSON con i valori della variabile.

La nuova query utilizza anche un _frammento_ (`productDetails`) per riutilizzare la stessa selezione di campi in più posizioni. [Ulteriori informazioni sui frammenti](https://graphql.org/learn/queries/#fragments){target="_blank"} nella documentazione di GraphQL.

{{$include /help/_includes/graphql-rest-related-links.md}}
