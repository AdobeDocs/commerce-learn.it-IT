---
title: Eseguire una query utilizzando GraphQL
description: Scopri come eseguire una query utilizzando GraphQL su Adobe Commerce e [!DNL Magento Open Source]. Questa è un’introduzione a GraphQL che utilizza le chiamate GET e POST.
landing-page-description: Scopri come eseguire una query utilizzando GraphQL su Adobe Commerce e [!DNL Magento Open Source]. Questa è un’introduzione a GraphQL che utilizza le chiamate GET e POST.
kt: 11524
doc-type: tutorial
audience: all
last-substantial-update: 2022-12-13T00:00:00Z
source-git-commit: 9dc530107470617f88992d8eb2ed9feb017a6530
workflow-type: tm+mt
source-wordcount: '916'
ht-degree: 0%

---

# Query GraphQL

Approfondiamo la sintassi della query GraphQL con un esempio completo. (Ricorda, puoi provare questo da solo contro https://venia.magento.com/graphql.)

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

Una risposta plausibile di un server GraphQL per la query di cui sopra potrebbe essere:

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

L&#39;esempio precedente si basa, ad Magento, sullo schema GraphQL predefinito, definito sul server. In questa richiesta, esegui query su più tipi di dati contemporaneamente. La query esprime esattamente i campi desiderati e i dati restituiti vengono formattati in modo simile alla query stessa.

>[!NOTE]
>
>I client GraphQL offuscano la forma della richiesta HTTP effettiva inviata, ma questo è facile da scoprire. Se utilizzi un client basato su browser, osserva la [!UICONTROL Network] quando viene inviata una query. La richiesta contiene un corpo non elaborato costituito da &quot;query: `{string}`&quot;, dove `{string}` è semplicemente la stringa non elaborata dell’intera query. Se la richiesta viene inviata come GET, la query potrebbe essere codificata nel parametro della stringa di query &quot;query&quot;. A differenza di REST, il tipo di richiesta HTTP non ha importanza, solo il contenuto della query.


## Query per ciò che desideri

`country` e `categories` nell&#39;esempio rappresentano due diverse &quot;query&quot; per due diversi tipi di dati. A differenza di un paradigma API tradizionale come REST, che definirebbe endpoint separati ed espliciti per ogni tipo di dati, GraphQL offre la flessibilità di eseguire query su un singolo endpoint con un’espressione che può recuperare diversi tipi di dati contemporaneamente.

Allo stesso modo, la query specifica esattamente i campi desiderati per entrambi `country` (`id` e `full_name_english`) e `categories` (`items`, che dispone di una sottoselezione di campi) e i dati ricevuti riflettono la specifica del campo. Ci sono presumibilmente molti più campi disponibili per questi tipi di dati, ma si ottiene solo ciò che è stato richiesto.


>[!NOTE]
>
>È possibile notare che il valore restituito per `items` è in realtà un _array_ di valori, ma si selezionano direttamente i relativi sottocampi. Quando un tipo di campo è un elenco, GraphQL comprende implicitamente le sottoselezioni da applicare a ogni elemento dell’elenco.

## Argomenti

Mentre i campi che si desidera restituire sono specificati tra parentesi graffe di ciascun tipo, gli argomenti e i valori denominati per essi sono specificati tra parentesi dopo il nome del tipo. Gli argomenti sono spesso facoltativi e spesso influiscono sul modo in cui i risultati delle query vengono filtrati, formattati o altrimenti trasformati.

Stai passando un `id` argomento `country`, specificando il paese in cui si desidera eseguire la query e un `filters` argomento `categories`.

## Campi in discesa

Mentre potresti tendere a pensare a `country` e `categories` come query o entità separate, l’intera struttura ad albero espressa nella query in realtà è costituita solo da campi. L&#39;espressione di `products` non è sintatticamente diverso da quello di `categories`. Entrambi sono campi, e non c&#39;è differenza tra la loro costruzione.

Qualsiasi grafico dei dati GraphQL ha un singolo tipo &quot;root&quot; (in genere riferito a `Query`) per iniziare la struttura ad albero e i tipi spesso considerati entità vengono semplicemente assegnati ai campi di questa radice. La nostra query di esempio sta effettuando una query generica per il tipo di radice e selezionando i campi `country` e `categories`. Seleziona quindi i campi secondari di tali campi e così via, potenzialmente con più livelli di profondità. Ovunque il tipo restituito di un campo sia un tipo complesso (ad esempio, uno con campi propri anziché un tipo scalare), continuare a selezionare i campi desiderati.

Questo concetto di campi nidificati è anche il motivo per cui è possibile passare argomenti per `products` (`pageSize` e `currentPage`) nello stesso modo in cui lo hai fatto per il livello superiore `categories` campo .

![Struttura dei campi GraphQL](../assets/graphql-field-tree.png)

## Variabili

Proviamo una query diversa:

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

La prima cosa da notare è la parola chiave aggiunta `query` prima della parentesi graffa di apertura della query, insieme a un nome di operazione (`getProducts`). Questo nome operazione è arbitrario; non corrisponde a nulla nello schema del server. È stata aggiunta questa sintassi per supportare l’introduzione di variabili.

Nella query precedente, i valori hardcoded per gli argomenti dei campi direttamente, come stringhe o interi. La specifica GraphQL, tuttavia, dispone del supporto di prima classe per separare l’input dell’utente dalla query principale utilizzando le variabili.

Nella nuova query, utilizzi parentesi prima della parentesi di apertura dell’intera query per definire un `$search` variabile (le variabili usano sempre la sintassi del prefisso del simbolo del dollaro) ed è questa variabile che viene fornita al `search` argomento `products`.

Quando una query contiene variabili, la richiesta GraphQL deve includere accanto alla query stessa un dizionario di valori codificato in JSON separato. Per la query di cui sopra, oltre al corpo della query puoi inviare il seguente JSON di valori di variabili:

```json
{
    "search": "VT01"
}
```

>[!NOTE]
>
>Se stai provando queste query contro il sito di esempio Venia piuttosto che la tua istanza di Magento, probabilmente non riceverai alcun risultato per `related_products`.

In qualsiasi client GraphQL-aware che utilizzi per i test (come Altair e GraphiQL), l’interfaccia utente supporta l’immissione delle variabili JSON separatamente dalla query.

Proprio come hai visto che l&#39;effettiva richiesta HTTP per una query GraphQL contiene &quot;query: `{string}`&quot; nel suo corpo, qualsiasi richiesta contenente un dizionario di variabili semplicemente include un&#39;ulteriore &quot;variabili: `{json}`&quot; nello stesso corpo, dove `{json}` è la stringa JSON con i valori della variabile .

La nuova query utilizza anche un _frammento_ (`productDetails`) per riutilizzare la stessa selezione di campi in più posizioni. [Ulteriori informazioni sui frammenti](https://graphql.org/learn/queries/#fragments) nella documentazione di GraphQL.
