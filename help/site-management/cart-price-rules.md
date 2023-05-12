---
title: Creare una regola del prezzo del carrello
description: Scopri come creare regole sul prezzo del carrello che applicano sconti nel carrello sulla base di un insieme di condizioni.
doc-type: feature video
audience: all
role: Admin, User
activity: use
exl-id: ae8cab73-8a8b-4266-8205-b7397633e9bf
source-git-commit: 3ee6eae696d33ce792beabdf91c584e039ab3b54
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# Creare una regola del prezzo del carrello

Le regole del prezzo del carrello applicano sconti agli articoli nel carrello, in base a una serie di condizioni. Lo sconto può essere applicato automaticamente quando le condizioni sono soddisfatte o quando il cliente immette un codice coupon valido. Quando applicato, lo sconto viene visualizzato nel carrello sotto il subtotale. Una regola di prezzo del carrello può essere utilizzata in base alle esigenze per una stagione o una promozione modificando il suo stato e l&#39;intervallo di date.

## A chi serve questo video?

- Marketing di eCommerce
- Manager del sito web

## Contenuto video

>[!VIDEO](https://video.tv.adobe.com/v/343835?quality=12&learn=on)

## Problemi relativi alla visualizzazione dei prezzi

Ci sono alcuni scenari unici che richiedono che ogni voce di riga mostri il proprio sconto fornito, ma i valori potrebbero non corrispondere esattamente. Il motivo è che uno sconto della regola del prezzo del carrello viene applicato a più prodotti, ma i valori non vengono suddivisi in due posizioni decimali.

>[!BEGINSHADEBOX]

Regola del prezzo del carrello = 10% di sconto applicato a 2 prodotti nel carrello Condizione per l&#39;entrata in vigore della regola del prezzo: articoli totali nel carrello è 2 Le azioni applicano la percentuale di sconto sul prezzo del prodotto e l&#39;importo di sconto è 10

2 articoli sono aggiunti al carrello, ciascuno è $19.95

Per ottenere l&#39;importo dello sconto moltiplica i prezzi del prodotto 0,1

19,95 x 0,1 = 1,995

Questo è il problema, abbiamo 3 cifre decimali, invece di due. Convertire questo in dollari è ora un problema

>[!ENDSHADEBOX]

### La soluzione

Pensando al proprietario del sito web, che è l&#39;unica persona interessata da questo problema, è stato stabilito che mostrare ogni articolo ordinato con lo sconto fornito in dollari era il più appropriato. Per garantire che l&#39;intero importo dell&#39;ordine venga calcolato correttamente, è stato deciso di arrotondare il primo elemento e gli altri al terzo decimale. Esamina questo scenario:

>[!BEGINSHADEBOX]

Stesso 10% di sconto come sopra regola del carrello in effetti Aggiungi 2 prodotti al carrello che sono 19.95

Ogni prodotto dovrebbe ottenere $1.995 in sconti Prodotto 1 - 19.95 x 0.1 = 1.995 2 - 19.95 x 0.1 = 1.995

Un totale complessivo di 3,99 viene fornito come sconto al cliente

Quando visualizziamo gli elementi di riga al proprietario dello store nell&#39;amministratore, dobbiamo regolare il primo elemento e arrotondarlo fino a 2.000. Il secondo elementi rilasciamo il terzo decimale Prodotto 1 = 2.00 Prodotto 2 = 1.99

Lo sconto totale dei due prodotti ora, quando sommati, corrisponde allo sconto effettivo fornito a un cliente.
>[!ENDSHADEBOX]

Ecco una schermata come mostrerebbe nell&#39;amministratore per un ordine con questo scenario:

![Visualizzazione amministratore che mostra gli elementi ordinati con valori diversi](../assets/commerce-admin-cart-price-rule-values-different.png)

### Altre soluzioni potenziali e perché non sono state utilizzate

>[!BEGINSHADEBOX]

Stesso 10% di sconto come sopra regola del carrello in effetti Aggiungi 2 prodotti al carrello che sono 19.95

Ogni prodotto dovrebbe ottenere $1.995 in sconti, tuttavia se li arrotondiamo, mostra troppo sconto.

Prodotto 1 - 19,95 x 0,1 = 1,995 Prodotto 2 - 19,95 x 0,1 = 1,995

Converti in arrotondare tutti gli articoli Prodotto 1 Il nuovo valore è 2.00 Prodotto 2 Il nuovo valore è 2.00

Un totale complessivo di 3,99 è stato effettivamente fornito come uno sconto al cliente, tuttavia se arrotondiamo, mostrerebbe che $4,00 è stato dato, e questo è errato.

2.00 + 2.00 = $4.00

>[!ENDSHADEBOX]

Problema simile se il terzo decimale veniva eliminato per tutti gli elementi, mostrava uno sconto insufficiente.

>[!BEGINSHADEBOX]

Stesso 10% di sconto come sopra regola del carrello in effetti Aggiungi 2 prodotti al carrello che sono 19.95

Ogni prodotto dovrebbe ottenere $ 1,995 in sconti, tuttavia se si elimina il terzo decimale, questo accade: Prodotto 1 - 19,95 x 0,1 = 1,995 Prodotto 2 - 19,95 x 0,1 = 1,995

Converti per eliminare il terzo decimale per tutti gli articoli Prodotto 1 Il nuovo valore è 1.99 Prodotto 2 Il nuovo valore è 1.99

Un totale complessivo di 3,99 è stato effettivamente fornito come uno sconto al cliente, tuttavia se lasciamo cadere il terzo decimale, mostrerebbe che $3,98 è stato dato, e questo è errato.

1.99 + 1.99 = $3.98

>[!ENDSHADEBOX]


## Risorse aggiuntive

- [Crea una regola del prezzo del carrello - [!DNL Commerce] Guida al merchandising e alle promozioni](https://experienceleague.adobe.com/docs/commerce-admin/marketing/promotions/cart-rules/price-rules-cart-create.html)
- [Codici coupon - [!DNL Commerce] Guida al merchandising e alle promozioni](https://experienceleague.adobe.com/docs/commerce-admin/marketing/promotions/cart-rules/price-rules-cart-coupon.html)
