---
title: Eseguire una mutazione con GraphQL
description: Introduzione all’esecuzione di una mutazione con GraphQL su Adobe Commerce e [!DNL Magento Open Source]. Esegui la tua prima mutazione utilizzando le chiamate POST.
landing-page-description: Introduzione all’esecuzione di una mutazione con GraphQL su Adobe Commerce e [!DNL Magento Open Source]. Esegui la tua prima mutazione utilizzando le chiamate POST.
short-description: Introduzione all’esecuzione di una mutazione con GraphQL su Adobe Commerce e [!DNL Magento Open Source]. Esegui la tua prima mutazione utilizzando le chiamate POST.
kt: 11524
doc-type: tutorial
audience: all
last-substantial-update: 2022-12-13T00:00:00Z
exl-id: 6b82ffda-925f-4a81-8ca5-49a2b8ab4929
source-git-commit: 67d21ca23cdccc87cdeed4a08a3ebb48e5bd1030
workflow-type: tm+mt
source-wordcount: '357'
ht-degree: 0%

---

# Mutazioni

Qualsiasi specifica API completa deve offrire la possibilità non solo di eseguire query sui dati, ma anche di crearli e aggiornarli.

REST distingue tra le richieste che modificano i dati e quelle che non lo fanno con il tipo di richiesta o con il &quot;verb&quot; (GET vs. POST o PUT).
Quando si utilizza GraphQL, le query che modificano i dati vengono distinte dalla `mutation` parola chiave che corrisponde a un tipo radice diverso nello schema definito nel server.

Osserva questo esempio di mutazione per aggiungere un prodotto al carrello di un utente. (Questo richiede un ID carrello generato per la sessione del cliente connesso o che utilizza il `createEmptyCart` mutazione.)

```graphql
mutation doAddToCart(
    $cartId: String!,
    $cartItems: [CartItemInput!]!
) {
    addProductsToCart(
        cartId: $cartId
        cartItems: $cartItems
    ) {
        cart {
            total_quantity
            prices {
                grand_total {
                    value
                }
            }
        }
    }
}
```

Puoi immaginare che la mutazione di cui sopra venga inviata in una richiesta insieme al seguente dizionario di variabili:

```json
{
  "cartId": "{cart-id}",
  "cartItems": [
    {
      "quantity": 1,
      "sku": "VT01-RN-XS"
    }
  ]
}
```

E infine, potreste ricevere una risposta come questa:

```json
{
  "data": {
    "addProductsToCart": {
      "cart": {
        "total_quantity": 1,
        "prices": {
          "grand_total": {
            "value": 35.2
          }
        }
      }
    }
  }
}
```

La cosa principale da notare che sull&#39;esempio precedente è che, oltre all&#39;uso del `mutation` parola chiave anziché `query`, la sintassi è identica a una query. Come le query, la mutazione include:

* Un nome di operazione arbitraria (`doAddToCart`)
* Un elenco di variabili (ad esempio, `$cartId`)
* Un campo iniziale (`addProductsToCart`) con argomenti (ad esempio, `cartId`, impostato sul valore di `$cartId`) tra parentesi
* Sottoselezione di campi tra parentesi

La sottoselezione dei campi consente di definire in modo flessibile i campi che si desidera restituire (dal tipo assegnato come valore restituito di `addProductsToCart` - `AddProductsToCartOutput`) al termine della mutazione.

Come spiegato in precedenza, i campi definiti in uno schema GraphQL partono da un tipo principale per le query (in genere denominati come `Query`). Analogamente, esiste un altro tipo radice per le mutazioni (in genere denominato `Mutation`). `addProductsToCart` è un campo di quel tipo principale.

Altre note sull&#39;esempio precedente:

* La `!` suffisso dei caratteri `String` e `CartItemInput` indica che la variabile è obbligatoria.
* Le parentesi quadre (`[]`) intorno al `CartItemInput` tipo specificato per `$cartItems` indicare un elenco di quel tipo anziché un singolo valore.

{{$include /help/_includes/graphql-rest-related-links.md}}
