---
title: Banner edizione
description: Elementi visivi riutilizzati per annotare feature o pagine applicabili a una specifica edizione
source-git-commit: 8c7c64ddff456815b0a1649f497e917da8b8fca0
workflow-type: tm+mt
source-wordcount: '168'
ht-degree: 0%

---

# Banner edizione

## Solo funzionalità EE {#ee-feature}

<table style="border:1px solid red">
<tr><td><img alt="Funzione Adobe Commerce" src="../assets/adobe-logo.svg" width="20" height="20" /> Funzione esclusiva solo in Adobe Commerce (<a href="https://experienceleague.adobe.com/docs/commerce-admin/user-guides/home.html#product-editions">Ulteriori informazioni</a>)</td></tr>
</table>

## Funzionalità solo B2B {#b2b-feature}

<table style="border:1px solid green">
<tr><td><img alt="Funzione Adobe Commerce" src="../assets/b2b.svg" width="20" height="20" /> Funzione esclusiva disponibile solo con <a href="https://experienceleague.adobe.com/docs/commerce-admin/user-guides/home.html#product-editions">B2B per Adobe Commerce</a></td></tr>
</table>

## 400 questioni {#avoid-400-error}

>[!CAUTION]
>
>Quando esegui le chiamate API, assicurati che venga utilizzato un qualche tipo di searchCriteria. È inoltre possibile utilizzare l’impaginazione. Se il risultato di Adobe Commerce è troppo grande, la capacità di Adobe Developer App Builder potrebbe essere soddisfatta e causare un arresto imprevisto del file. Il risultato è un risultato di risposta non corretto come errore 400.\
> Ad esempio, supponiamo che sia necessario richiedere tutti i prodotti correnti da Adobe Commerce. L’URL risultante sarà simile a `{{base_url}}rest/V1/products?searchCriteria=all`. A seconda delle dimensioni del catalogo restituito, il json potrebbe essere troppo grande per essere utilizzato da App Builder. Utilizza invece l’impaginazione ed effettua alcune richieste per evitare `Response is not valid 'message/http'.`
