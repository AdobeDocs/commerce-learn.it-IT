---
title: Crea regole prezzo carrello
description: Scopri come creare regole di prezzo del carrello che applicano sconti nel carrello quando vengono soddisfatte le condizioni definite.
doc-type: Tutorial
last-substantial-update: 2022-12-28T00:00:00Z
feature: Configuration, System, Customers, Shopping Cart
topic: Commerce, Administration
role: User
level: Beginner
duration: 353
jira: KT-17148
exl-id: ae8cab73-8a8b-4266-8205-b7397633e9bf
source-git-commit: 9aa4d70ee6a3825f027aa2a9c6a1ac0f876ed59f
workflow-type: tm+mt
source-wordcount: '677'
ht-degree: 0%

---

# Crea regole prezzo carrello

Le regole di prezzo del carrello applicano sconti agli articoli nel carrello in base alle condizioni impostate. Lo sconto può essere applicato automaticamente quando le condizioni vengono soddisfatte o quando il cliente immette un codice coupon valido. Lo sconto viene visualizzato nel carrello sotto il subtotale. Puoi attivare o disattivare una regola per una stagione o una promozione modificandone lo stato e l’intervallo di date.

## A chi serve questo video?

* Ecommerce marketer
* Gestori di siti Web

## Contenuto video

* Crea le regole di prezzo del carrello e i codici coupon facoltativi.
* Scopri come appaiono gli sconti nel carrello e per le promozioni.

>[!VIDEO](https://video.tv.adobe.com/v/343835?learn=on)

## Problemi relativi alla visualizzazione dei prezzi

In alcuni casi, ogni riga deve mostrare lo sconto applicato, ma i valori visualizzati potrebbero non corrispondere esattamente. Ciò si verifica quando una regola del prezzo del carrello applica uno sconto su più prodotti e la suddivisione non si divide in modo uniforme su due posizioni decimali.

>[!BEGINSHADEBOX]

Regola prezzo carrello = sconto del 10% applicato a 2 prodotti nel carrello
Condizione per l&#39;entrata in vigore della regola del prezzo: il totale degli articoli nel carrello è 2
Le azioni applicano la percentuale di sconto sul prezzo del prodotto e l&#39;importo dello sconto è 10

2 articoli aggiunti al carrello, ciascuno è $ 19,95

Per ottenere l&#39;importo dello sconto, moltiplicare il prezzo del prodotto per 0,1

19,95 x 0,1 = 1,995

Questo è il problema, abbiamo 3 cifre decimali, invece di 2. Convertire questo in dollari è ora un problema

>[!ENDSHADEBOX]

### La soluzione

Per il commerciante nell’Admin, l’approccio più chiaro è quello di mostrare ogni linea ordinata con il relativo sconto in dollari. Per mantenere corretto il totale dell&#39;ordine, arrotondare la prima riga verso l&#39;alto e rilasciare il terzo decimale sulle righe rimanenti. Esamina questo scenario:

>[!BEGINSHADEBOX]

Stesso sconto del 10% come sopra regola del carrello in vigore
Aggiungi 2 prodotti al carrello che sono 19,95

Ogni prodotto dovrebbe ottenere $1.995 in sconti
Prodotto 1 - 19,95 x 0,1 = 1,995
2 - 19,95 x 0,1 = 1,995

Viene fornito al cliente un totale complessivo di 3,99 come sconto

Quando visualizzi gli elementi riga al proprietario del negozio nell’amministratore,
è necessario modificare il primo elemento e arrotondarlo a 2.000. Per il secondo elemento, rilascia il terzo decimale.
Prodotto 1 = 2,00
Prodotto 2 = 1,99

Lo sconto totale dei due prodotti ora quando sommato corrisponde allo sconto effettivo fornito a un cliente.
>[!ENDSHADEBOX]

Ecco una schermata come apparirebbe nell’amministratore per un ordine con questo scenario:

![Visualizzazione amministratore con elementi ordinati con valori diversi](../assets/commerce-admin-cart-price-rule-values-different.png)

### Altre soluzioni potenziali e perché non sono state utilizzate

>[!BEGINSHADEBOX]

Stesso sconto del 10% come sopra regola del carrello in vigore
Aggiungi 2 prodotti al carrello che sono 19,95

Ogni prodotto dovrebbe ottenere 1,995 $ di sconti,
tuttavia, se le arrotondiamo, ci sono troppi sconti.

Prodotto 1 - 19,95 x 0,1 = 1,995
Prodotto 2 - 19,95 x 0,1 = 1,995

Converti per arrotondare tutti gli elementi
Prodotto 1 Il nuovo valore è 2,00
Prodotto 2 Il nuovo valore è 2,00

Il totale complessivo di 3,99 è stato effettivamente fornito come sconto al cliente,
tuttavia, se arriviamo a questo punto, ciò dimostrerebbe che sono stati dati 4 dollari, il che non è corretto.

2,00 + 2,00 = 4,00 $

>[!ENDSHADEBOX]

Problema simile se il terzo decimale veniva rimosso per tutti gli elementi, lo sconto fornito risultava insufficiente.

>[!BEGINSHADEBOX]

Stesso sconto del 10% come sopra regola del carrello in vigore
Aggiungi 2 prodotti al carrello che sono 19,95

Ogni prodotto dovrebbe ottenere $ 1.995 in sconti, tuttavia se si abbassa solo il terzo decimale, questo accade:
Prodotto 1 - 19,95 x 0,1 = 1,995
Prodotto 2 - 19,95 x 0,1 = 1,995

Converti per rilasciare il terzo decimale per tutti gli elementi
Prodotto 1 Il nuovo valore è 1,99
Prodotto 2 Il nuovo valore è 1,99

Il totale complessivo di 3,99 è stato effettivamente fornito come sconto al cliente,
tuttavia, se si lascia cadere il terzo decimale, si vedrebbe che $3,98 è stato dato, e questo non è corretto.

1,99 + 1,99 = $ 3,98

>[!ENDSHADEBOX]

## Risorse aggiuntive

* [Crea regola prezzo carrello - [!DNL Commerce] Guida al merchandising e alle promozioni](https://experienceleague.adobe.com/docs/commerce-admin/marketing/promotions/cart-rules/price-rules-cart-create.html?lang=it){target="_blank"}
* [Codici coupon - [!DNL Commerce] Guida al merchandising e alle promozioni](https://experienceleague.adobe.com/docs/commerce-admin/marketing/promotions/cart-rules/price-rules-cart-coupon.html?lang=it){target="_blank"}
