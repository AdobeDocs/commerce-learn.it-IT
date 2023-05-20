---
title: Crea una regola prezzo carrello
description: Scopri come creare regole di prezzo del carrello che applicano sconti nel carrello in base a una serie di condizioni.
doc-type: feature video
audience: all
role: Admin, User
activity: use
exl-id: ae8cab73-8a8b-4266-8205-b7397633e9bf
source-git-commit: 3ee6eae696d33ce792beabdf91c584e039ab3b54
workflow-type: tm+mt
source-wordcount: '632'
ht-degree: 0%

---

# Crea una regola prezzo carrello

Le regole di prezzo del carrello applicano sconti agli articoli nel carrello, in base a una serie di condizioni. Lo sconto può essere applicato automaticamente quando le condizioni vengono soddisfatte o quando il cliente immette un codice coupon valido. Quando applicato, lo sconto viene visualizzato nel carrello sotto il subtotale. Una regola di prezzo del carrello può essere utilizzata in base alle esigenze per una stagione o una promozione modificandone lo stato e l’intervallo di date.

## A chi serve questo video?

- Ecommerce marketer
- Gestori di siti Web

## Contenuto video

>[!VIDEO](https://video.tv.adobe.com/v/343835?quality=12&learn=on)

## Problemi relativi alla visualizzazione dei prezzi

Esistono alcuni scenari univoci che richiedono che ogni voce visualizzi lo sconto fornito, ma i valori potrebbero non corrispondere esattamente. Il motivo è che uno sconto della regola del prezzo del carrello viene applicato a più prodotti, ma i valori non si dividono uniformemente in due posizioni decimali.

>[!BEGINSHADEBOX]

Regola prezzo carrello = sconto del 10% applicato a 2 prodotti nel carrello Condizione affinché la regola prezzo abbia effetto: gli articoli totali nel carrello sono 2 Le azioni applicano la percentuale di sconto sul prezzo del prodotto e l&#39;importo dello sconto è 10

2 articoli aggiunti al carrello, ciascuno è $ 19,95

Per ottenere l&#39;importo dello sconto, moltiplicare il prezzo del prodotto per 0,1

19,95 x 0,1 = 1,995

Questo è il problema, abbiamo 3 cifre decimali, invece di 2. Convertire questo in dollari è ora un problema

>[!ENDSHADEBOX]

### La soluzione

Pensando al proprietario del sito web, che è l&#39;unica persona interessata da questo problema, è stato determinato che mostrare ogni articolo ordinato con lo sconto fornito in dollari era il più appropriato. Per garantire un calcolo corretto dell’intero importo dell’ordine, è stato deciso di arrotondare il primo elemento e gli altri rilasciano il terzo decimale. Esamina questo scenario:

>[!BEGINSHADEBOX]

Stesso 10% di sconto come sopra regola del carrello in vigore Aggiungi 2 prodotti al carrello che sono 19.95

Ogni prodotto dovrebbe ottenere $1.995 in sconti Prodotto 1 - 19.95 x 0.1 = 1.995 2 - 19.95 x 0.1 = 1.995

Viene fornito al cliente un totale complessivo di 3,99 come sconto

Quando visualizzi gli elementi riga al proprietario del negozio nell’amministratore, è necessario regolare il primo elemento e arrotondarlo a 2.000. Il secondo elemento rilascia il terzo decimale Prodotto 1 = 2,00 Prodotto 2 = 1,99

Lo sconto totale dei due prodotti ora, quando sommato insieme corrisponde allo sconto effettivo fornito a un cliente.
>[!ENDSHADEBOX]

Ecco una schermata come apparirebbe nell’amministratore per un ordine con questo scenario:

![Visualizzazione amministratore che mostra gli elementi ordinati con valori diversi](../assets/commerce-admin-cart-price-rule-values-different.png)

### Altre soluzioni potenziali e perché non sono state utilizzate

>[!BEGINSHADEBOX]

Stesso 10% di sconto come sopra regola del carrello in vigore Aggiungi 2 prodotti al carrello che sono 19.95

Ogni prodotto dovrebbe ottenere $1.995 in sconti, tuttavia se semplicemente li arrotondiamo, mostra troppo sconto.

Prodotto 1 - 19,95 x 0,1 = 1,995 Prodotto 2 - 19,95 x 0,1 = 1,995

Converti per arrotondare per eccesso tutti gli articoli Prodotto 1 Il nuovo valore è 2,00 Prodotto 2 Il nuovo valore è 2,00

Un totale complessivo di 3,99 è stato effettivamente fornito come uno sconto al cliente, tuttavia se arrotondiamo per eccesso, sarebbe mostrare che $ 4,00 è stato dato, e questo non è corretto.

2.00 + 2.00 = $4.00

>[!ENDSHADEBOX]

Problema simile se il terzo decimale veniva rimosso per tutti gli elementi, lo sconto fornito risultava insufficiente.

>[!BEGINSHADEBOX]

Stesso 10% di sconto come sopra regola del carrello in vigore Aggiungi 2 prodotti al carrello che sono 19.95

Ogni prodotto dovrebbe ottenere $1.995 in sconti, tuttavia se si abbassa solo il terzo decimale, questo accade: Prodotto 1 - 19.95 x 0.1 = 1.995 Prodotto 2 - 19.95 x 0.1 = 1.995

Converti per rilasciare il terzo decimale per tutti gli elementi Prodotto 1 Il nuovo valore è 1,99 Prodotto 2 Il nuovo valore è 1,99

Un totale complessivo di 3,99 è stato effettivamente fornito come uno sconto al cliente, tuttavia se lasciamo cadere il terzo decimale, dimostrerebbe che $ 3,98 è stato dato, e questo non è corretto.

1.99 + 1.99 = $3.98

>[!ENDSHADEBOX]


## Risorse aggiuntive

- [Crea una regola prezzo carrello - [!DNL Commerce] Guida al merchandising e alle promozioni](https://experienceleague.adobe.com/docs/commerce-admin/marketing/promotions/cart-rules/price-rules-cart-create.html)
- [Codici coupon - [!DNL Commerce] Guida al merchandising e alle promozioni](https://experienceleague.adobe.com/docs/commerce-admin/marketing/promotions/cart-rules/price-rules-cart-coupon.html)
