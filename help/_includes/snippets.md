---
title: Banner edizione
description: Riutilizzati elementi visivi per annotare la funzionalità o le pagine che si applicano a una specifica edizione
source-git-commit: 8c7c64ddff456815b0a1649f497e917da8b8fca0
workflow-type: tm+mt
source-wordcount: '168'
ht-degree: 0%

---

# Banner edizione

## Funzione EE only {#ee-feature}

<table style="border:1px solid red">
<tr><td><img alt="Funzione Adobe Systems Commerce" src="../assets/adobe-logo.svg" width="20" height="20" /> Funzionalità esclusiva solo in Adobe Systems Commerce ( <a href="https://experienceleague.adobe.com/docs/commerce-admin/user-guides/home.html#product-editions"> Scopri altro </a> )</td></tr>
</table>

## Funzione solo B2B {#b2b-feature}

<table style="border:1px solid green">
<tr><td><img alt="Funzione Adobe Systems Commerce" src="../assets/b2b.svg" width="20" height="20" /> Funzionalità esclusiva disponibile solo con <a href="https://experienceleague.adobe.com/docs/commerce-admin/user-guides/home.html#product-editions"> B2B per Adobe Systems Commerce</a></td></tr>
</table>

## 400 problemi {#avoid-400-error}

>[!CAUTION]
>
>Quando si eseguono chiamate API, assicurarsi che venga utilizzata una sorta di searchCriteria. Potresti anche considerare l&#39;utilizzo dell&#39;impaginazione. Se il risultato di Adobe Systems Commerce è troppo grande, la capacità di Adobe Systems Developer app Builder può essere soddisfatta e causare una fine imprevista del file. Il risultato è un risultato di risposta non valido come errore 400.\
> Ad esempio, supponiamo che sia necessario richiesta tutti i prodotti correnti da Adobe Systems Commerce. La URL risultante assomiglierebbe `{{base_url}}rest/V1/products?searchCriteria=all` . A seconda delle dimensioni del catalogo restituito, il JSON potrebbe essere troppo grande perché app Builder possa consumare. Utilizza invece l&#39;impaginazione e fai alcune richieste da evitare `Response is not valid 'message/http'.`
