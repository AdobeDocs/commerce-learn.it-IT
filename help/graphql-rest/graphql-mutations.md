---
title: Eseguire una mutazione utilizzando GraphQL
description: Ottieni un'introduzione sull'esecuzione di una mutazione utilizzando GraphQL su Adobe Commerce e  [!DNL Magento Open Source]. Esegui la prima mutazione utilizzando chiamate POST.
landing-page-description: Ottieni un'introduzione sull'esecuzione di una mutazione utilizzando GraphQL su Adobe Commerce e  [!DNL Magento Open Source]. Esegui la prima mutazione utilizzando chiamate POST.
short-description: Ottieni un'introduzione sull'esecuzione di una mutazione utilizzando GraphQL su Adobe Commerce e  [!DNL Magento Open Source]. Esegui la prima mutazione utilizzando chiamate POST.
kt: 13938
doc-type: video
audience: all
last-substantial-update: 2023-10-12T00:00:00Z
feature: GraphQL
topic: Commerce, Architecture, Headless
role: Architect, Developer
level: Beginner, Intermediate
exl-id: 6b82ffda-925f-4a81-8ca5-49a2b8ab4929
source-git-commit: 2041bbf1a2783975091b9806c12fc3c34c34582f
workflow-type: tm+mt
source-wordcount: '404'
ht-degree: 0%

---

# Mutazioni

Questa è la parte 3 della serie per GraphQL e Adobe Commerce. Le mutazioni consentono di salvare, aggiornare e restituire valori tramite GraphQL.


>[!VIDEO](https://video.tv.adobe.com/v/3424121?learn=on)

## Video e tutorial correlati su GraphQL in questa serie

* [Parte 1 GraphQL - Introduzione](../graphql-rest/intro-graphql.md)
* [Parte 2 GraphQL - Query](../graphql-rest/graphql-queries.md)
* [Parte 4 GraphQL - Schema](../graphql-rest/graphql-schema.md)

## Esempio di mutazione

Qualsiasi specifica API completa deve offrire la possibilità non solo di eseguire query sui dati, ma anche di crearli e aggiornarli.

REST distingue tra le richieste che modificano i dati e quelle che non hanno il tipo di richiesta o &quot;verbo&quot; (GET vs. POST o PUT).
Quando si utilizza GraphQL, le query che modificano i dati si distinguono per la parola chiave `mutation` che corrisponde a un
tipo di radice nello schema definito nel server.

Osserva questa mutazione di esempio per aggiungere un prodotto al carrello di un utente. (Richiede un ID carrello generato)
per la sessione del cliente connesso o utilizzando la mutazione `createEmptyCart`.)

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

La cosa principale da notare riguardo all&#39;esempio precedente è che, a parte l&#39;uso della parola chiave `mutation` invece di `query`,
la sintassi è identica a una query. Come le query, la mutazione include:

* Nome di operazione arbitrario (`doAddToCart`)
* Un elenco di variabili (ad esempio, `$cartId`)
* Un campo iniziale (`addProductsToCart`) con argomenti (ad esempio, `cartId`, impostato sul valore di `$cartId`) tra parentesi
* Sottoselezione di campi tra parentesi graffe

La sottoselezione dei campi consente di definire in modo flessibile i campi che si desidera restituire (dal tipo assegnato come
valore restituito `addProductsToCart` - `AddProductsToCartOutput`) dopo il completamento della mutazione.

Come spiegato in precedenza, i campi definiti in uno schema GraphQL iniziano con un tipo principale per le query (in genere denominato `Query`). Analogamente,
esiste un altro tipo di radice per le mutazioni (in genere definito `Mutation`). `addProductsToCart` è un campo
su quel tipo di radice.

Altre note sull’esempio precedente:

* Il suffisso del carattere `!` `String` e `CartItemInput` indica che la variabile è obbligatoria.
* Le parentesi quadre (`[]`) intorno al tipo `CartItemInput` specificato per `$cartItems` indicano un elenco
di quel tipo piuttosto che un singolo valore.

{{$include /help/_includes/graphql-rest-related-links.md}}
