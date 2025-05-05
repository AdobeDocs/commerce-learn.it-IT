---
title: Crea una regola prezzo carrello
description: Scopri come creare regole di prezzo del carrello che applicano sconti nel carrello in base a una serie di condizioni.
doc-type: feature video
audience: all
activity: use
last-substantial-update: 2022-12-28T00:00:00Z
feature: Configuration, System, Customers, Shopping Cart
topic: Commerce, Administration
role: Admin, Leader, User
level: Beginner
duration: 171
jira: KT-17148
exl-id: ae8cab73-8a8b-4266-8205-b7397633e9bf
source-git-commit: d290ba1d9c8892b4322aeb19d3c65d9d8087a309
workflow-type: tm+mt
source-wordcount: '687'
ht-degree: 0%

---

# Crea una regola prezzo carrello

Le regole di prezzo del carrello applicano sconti agli articoli nel carrello, in base a una serie di condizioni. Lo sconto può essere applicato automaticamente quando le condizioni vengono soddisfatte o quando il cliente immette un codice coupon valido. Quando applicato, lo sconto viene visualizzato nel carrello sotto il subtotale. Una regola di prezzo del carrello può essere utilizzata in base alle esigenze per una stagione o una promozione modificandone lo stato e l’intervallo di date.

## A chi serve questo video?

- Ecommerce marketer
- Gestori di siti Web

## Contenuto video

>[!VIDEO](https://video.tv.adobe.com/v/3410805?quality=12&learn=on&captions=ita)

## Problemi relativi alla visualizzazione dei prezzi

Esistono alcuni scenari univoci che richiedono che ogni voce visualizzi lo sconto fornito, ma i valori potrebbero non corrispondere esattamente. Il motivo è che uno sconto della regola del prezzo del carrello viene applicato a più prodotti, ma i valori non si dividono uniformemente in due posizioni decimali.

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

Pensando al proprietario del sito web, che è l&#39;unica persona interessata da questo problema, è stato determinato che mostrare ogni articolo ordinato con lo sconto fornito in dollari era il più appropriato. Per garantire un calcolo corretto dell’intero importo dell’ordine, è stato deciso di arrotondare il primo elemento e gli altri rilasciano il terzo decimale. Esamina questo scenario:

>[!BEGINSHADEBOX]

Stesso sconto del 10% come sopra regola del carrello in vigore
Aggiungi 2 prodotti al carrello che sono 19,95

Ogni prodotto dovrebbe ottenere $1.995 in sconti
Prodotto 1 - 19,95 x 0,1 = 1,995
2 - 19,95 x 0,1 = 1,995

Viene fornito al cliente un totale complessivo di 3,99 come sconto

Quando visualizzi gli elementi riga al proprietario del negozio nell’amministratore,
è necessario modificare il primo elemento e arrotondarlo a 2.000. Il secondo elemento viene eliminato il terzo decimale
Prodotto 1 = 2,00
Prodotto 2 = 1,99

Lo sconto totale dei due prodotti ora, quando sommato insieme corrisponde allo sconto effettivo fornito a un cliente.
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

- [Crea regola prezzo carrello - [!DNL Commerce] Guida al merchandising e alle promozioni](https://experienceleague.adobe.com/docs/commerce-admin/marketing/promotions/cart-rules/price-rules-cart-create.html?lang=it)
- [Codici coupon - [!DNL Commerce] Guida al merchandising e alle promozioni](https://experienceleague.adobe.com/docs/commerce-admin/marketing/promotions/cart-rules/price-rules-cart-coupon.html?lang=it)
