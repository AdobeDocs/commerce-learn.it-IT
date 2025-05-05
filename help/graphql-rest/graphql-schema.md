---
title: Linguaggio dello schema con GraphQL
description: Scopri lo schema associato a GraphQL. Leggi una descrizione dello schema, insieme ad alcuni pattern e modi interessanti per leggere lo schema.
landing-page-description: Questa è un’introduzione a GraphQL. Informazioni sullo schema e come interpretare alcuni degli elementi
short-description: Questa è un’introduzione a GraphQL. Informazioni sullo schema e come interpretare alcuni degli elementi
kt: 13939
doc-type: video
audience: all
last-substantial-update: 2023-10-12T00:00:00Z
feature: GraphQL
topic: Commerce, Architecture, Headless
role: Architect, Developer
level: Beginner, Intermediate
exl-id: 6b59db07-b99e-47ae-9ccb-d4904afc8251
source-git-commit: 2041bbf1a2783975091b9806c12fc3c34c34582f
workflow-type: tm+mt
source-wordcount: '430'
ht-degree: 0%

---

# Linguaggio dello schema

Questa è la parte 4 della serie per GraphQL e Adobe Commerce. Le query e le mutazioni utilizzate si basano su un grafico dati specifico implementato sul server, che il runtime di GraphQL utilizza e utilizza per risolvere la query. La specifica di GraphQL definisce un linguaggio agnostico per esprimere i tipi e le relazioni del grafico dati.

>[!VIDEO](https://video.tv.adobe.com/v/3446618?learn=on&captions=ita)

## Video e tutorial correlati su GraphQL in questa serie

* [Parte 1 GraphQL - Introduzione](../graphql-rest/intro-graphql.md)
* [Parte 2 GraphQL - Query](../graphql-rest/graphql-queries.md)
* [Parte 3 GraphQL - Mutazioni](../graphql-rest/graphql-mutations.md)

## Schema di esempio

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

Puoi esplorare la [documentazione di GraphQL](https://graphql.org/learn/schema/){target="_blank"} per scoprire i dettagli del sistema dei tipi, inclusa la sintassi per alcuni concetti non rappresentati qui. L’esempio precedente, tuttavia, non richiede spiegazioni. Inoltre, la sintassi è simile a quella delle query. La definizione di uno schema di GraphQL consiste semplicemente nell’esprimere gli argomenti e i campi disponibili di un determinato tipo, insieme ai tipi di tali campi. Ogni tipo di campo complesso deve avere una definizione e così via nella struttura, fino a ottenere tipi scalari semplici come `String`.

La dichiarazione `input` è simile a un `type`, ma definisce un tipo che può essere utilizzato come input per un argomento. Prendere nota anche della dichiarazione `interface`. Questa funzione è più o meno la stessa delle interfacce in PHP. Altri tipi ereditano da questa interfaccia.

La sintassi `[CartItemInput!]!` è complessa, ma alla fine è piuttosto intuitiva. Il `!` _entro_ la parentesi quadra dichiara che ogni valore nella matrice deve essere non-null, mentre il valore _esterno_ dichiara che il valore della matrice stessa deve essere non-null (ad esempio, una matrice vuota).

>[!NOTE]
>
>La logica che determina il modo in cui i dati vengono recuperati e formattati in base a uno schema e il modo in cui tale logica viene mappata su particolari tipi dipende dall’implementazione runtime di GraphQL. Le implementazioni, tuttavia, devono seguire un flusso concettuale appropriato alla luce di una comprensione relativa ai campi nidificati: viene eseguita un&#39;operazione di risoluzione associata al tipo radice `Query` o `Mutation`, che esamina ogni campo specificato nella richiesta. Per ogni campo che viene risolto in un tipo complesso, viene eseguita una risoluzione simile per quel tipo e così via, fino a quando tutto non viene risolto in valori scalari.

{{$include /help/_includes/graphql-rest-related-links.md}}
