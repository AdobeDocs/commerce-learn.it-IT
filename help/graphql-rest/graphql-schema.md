---
title: Linguaggio dello schema con GraphQL
description: Scopri lo schema coinvolto in GraphQL. Leggere una descrizione dello schema, insieme ad alcuni pattern e modi interessanti per leggere lo schema.
landing-page-description: Questa è un'introduzione a GraphQL. Informazioni sullo schema e su come interpretare alcuni degli elementi
short-description: This is an introduction to GraphQL. Understanding the schema and how to interpret some of the elements
kt: 11524
doc-type: tutorial
audience: all
last-substantial-update: 2022-12-13T00:00:00Z
exl-id: 6b59db07-b99e-47ae-9ccb-d4904afc8251
source-git-commit: 67d21ca23cdccc87cdeed4a08a3ebb48e5bd1030
workflow-type: tm+mt
source-wordcount: '381'
ht-degree: 0%

---

# Linguaggio dello schema

Le query e le mutazioni utilizzate si basano su un grafico di dati specifico implementato sul server, che il runtime di GraphQL consuma e utilizza per risolvere la query. La specifica GraphQL definisce un linguaggio agnostico per esprimere i tipi e le relazioni del grafico dei dati.

Di seguito è riportato uno schema di tipo abbreviato che supporta le query e le mutazioni esaminate finora:

```graphql
input FilterMatchTypeInput {
  match: String
}

type Money {
  value: Float
}

type Country {
  id: String
  full_name_english: String
}

interface ProductInterface {
  sku: String
  name: String
  related_products: [ProductInterface]
}

type CategoryFilterInput {
  name: FilterMatchTypeInput
}

type CategoryProducts {
  items: [ProductInterface]
}

type CategoryTree {
  name: String
  products(pageSize: Int, currentPage: Int): CategoryProducts
}

type CategoryResult {
  items: [CategoryTree]
}

type Products {
  items: [ProductInterface]
}

type Query {
  country (id: String): Country
  categories (filters: CategoryFilterInput): CategoryResult
  products (search: String): Products
}

input CartItemInput {
  sku: String!
  quantity: Float!
}

type CartPrices {
  grand_total: Money
}

type Cart {
  prices: CartPrices
  total_quantity: Float!
}

type AddProductsToCartOutput {
  cart: Cart!
}

type Mutation {
  addProductsToCart(cartId: String!, cartItems: [CartItemInput!]!): AddProductsToCartOutput
}
```

È possibile approfondire [la documentazione GraphQL](https://graphql.org/learn/schema/){target="_blank"} per informazioni sui dettagli del sistema di tipi, compresa la sintassi per alcuni concetti non rappresentati qui. L&#39;esempio di cui sopra, tuttavia, è auto-esplicativo. Inoltre, tieni presente che la sintassi è simile alla sintassi della query. La definizione di uno schema GraphQL consiste semplicemente nell’esprimere gli argomenti e i campi disponibili di un determinato tipo, insieme ai tipi di tali campi. Ogni tipo di campo complesso deve disporre di una definizione e così via, attraverso l&#39;albero, fino ad arrivare a tipi scalari semplici come `String`.

La `input` La dichiarazione è sotto tutti gli aspetti come una `type` definisce un tipo che può essere utilizzato come input per un argomento. Inoltre, tieni presente `interface` dichiarazione. Questa funzione è più o meno la stessa delle interfacce in PHP. Altri tipi ereditano da questa interfaccia.

La sintassi `[CartItemInput!]!` sembra difficile, ma alla fine è abbastanza intuitivo. La `!` _interno_ la parentesi quadra dichiara che ogni valore della matrice deve essere non-null, mentre quello _fuori_ dichiara che il valore dell&#39;array stesso deve essere non-null (ad esempio, una matrice vuota).

>[!NOTE]
>
>La logica che determina il modo in cui i dati vengono recuperati e formattati in base a uno schema e il modo in cui tale logica viene mappata a tipi particolari dipende dall’implementazione runtime di GraphQL. Le implementazioni, tuttavia, devono seguire un flusso concettuale che abbia senso alla luce di una comprensione intorno ai campi nidificati: Operazione di risoluzione associata alla radice `Query` o `Mutation` viene eseguito il tipo , che esamina ogni campo specificato nella richiesta. Per ogni campo che si risolve in un tipo complesso, viene eseguita una risoluzione simile per quel tipo, e così via, fino a quando tutto viene risolto in valori scalari.

{{$include /help/_includes/graphql-rest-related-links.md}}
