---
title: Eseguire una mutazione utilizzando GraphQL
description: Ottieni un’introduzione all’esecuzione di una mutazione utilizzando GraphQL su Adobe Commerce e [!DNL Magento Open Source]. Esegui la prima mutazione utilizzando chiamate POST.
landing-page-description: Ottieni un’introduzione all’esecuzione di una mutazione utilizzando GraphQL su Adobe Commerce e [!DNL Magento Open Source]. Esegui la prima mutazione utilizzando chiamate POST.
short-description: Ottieni un’introduzione all’esecuzione di una mutazione utilizzando GraphQL su Adobe Commerce e [!DNL Magento Open Source]. Esegui la prima mutazione utilizzando chiamate POST.
kt: 11524
doc-type: tutorial
audience: all
last-substantial-update: 2022-12-13T00:00:00Z
feature: GraphQL
topic: Commerce, Architecture, Headless
role: Architect, Developer
level: Beginner, Intermediate
exl-id: 6b82ffda-925f-4a81-8ca5-49a2b8ab4929
source-git-commit: 404d2708a6d540d6fb19a33afb20726356cd8000
workflow-type: tm+mt
source-wordcount: '357'
ht-degree: 0%

---

# Mutazioni

Qualsiasi specifica API completa deve offrire la possibilità non solo di eseguire query sui dati, ma anche di crearli e aggiornarli.

REST distingue tra le richieste che modificano i dati e quelle che non hanno il tipo di richiesta o &quot;verbo&quot; (GET vs. POST o PUT).
Quando si utilizza GraphQL, le query che modificano i dati si distinguono in base al `mutation` parola chiave che corrisponde a un tipo di radice diverso nello schema definito nel server.

Osserva questa mutazione di esempio per aggiungere un prodotto al carrello di un utente. (Questo richiede un ID carrello generato per la sessione del cliente connesso o utilizzando `createEmptyCart` mutazione).

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

Puoi immaginare che la mutazione di cui sopra venga inviata in una richiesta insieme alle seguenti variabili del dizionario:

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

E infine, potresti ricevere una risposta come questa:

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

La cosa principale da notare su questo esempio è che, oltre all&#39;uso del `mutation` parola chiave anziché `query`, la sintassi è identica a una query. Come le query, la mutazione include:

* Nome di operazione arbitrario (`doAddToCart`)
* Un elenco di variabili (ad esempio, `$cartId`)
* Un campo iniziale (`addProductsToCart`) con argomenti (ad esempio, `cartId`, impostato sul valore di `$cartId`) tra parentesi
* Sottoselezione di campi tra parentesi graffe

La sottoselezione dei campi consente di definire in modo flessibile i campi che si desidera restituire (dal tipo assegnato come valore restituito di `addProductsToCart` - `AddProductsToCartOutput`) dopo il completamento della mutazione.

Come spiegato in precedenza, i campi definiti in uno schema GraphQL iniziano da un tipo principale per le query (in genere denominato `Query`). Analogamente, esiste un altro tipo di radice per le mutazioni (in genere indicato come `Mutation`). `addProductsToCart` è un campo su quel tipo di radice.

Altre note sull’esempio precedente:

* Il `!` suffisso dei caratteri `String` e `CartItemInput` indica che la variabile è obbligatoria.
* Parentesi quadre (`[]`) intorno al `CartItemInput` tipo specificato per `$cartItems` indica un elenco di quel tipo anziché un singolo valore.

{{$include /help/_includes/graphql-rest-related-links.md}}
