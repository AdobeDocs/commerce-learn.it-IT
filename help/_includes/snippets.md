---
title: Banner edizione
description: Riutilizzo di elementi visivi per notare una caratteristica o pagine relative a una specifica edizione
source-git-commit: 066e031bd98458c8692f1cb3234ff1ecd1b99e6e
workflow-type: tm+mt
source-wordcount: '156'
ht-degree: 0%

---

# Banner edizione

## Funzionalità solo EE {#ee-feature}

<table style="border:1px solid red">
<tr><td><img alt="Funzione di Adobe Commerce" src="../assets/adobe-logo.svg" width="20" height="20" /> Funzionalità esclusiva solo in Adobe Commerce (<a href="https://experienceleague.adobe.com/docs/commerce-admin/user-guides/home.html#product-editions">Ulteriori informazioni</a>)</td></tr>
</table>

## Funzionalità solo B2B {#b2b-feature}

<table style="border:1px solid green">
<tr><td><img alt="Funzione di Adobe Commerce" src="../assets/b2b.svg" width="20" height="20" /> Funzionalità esclusiva disponibile solo con <a href="https://experienceleague.adobe.com/docs/commerce-admin/b2b/guide-overview.html">B2B per Adobe Commerce</a></td></tr>
</table>

## 400 problemi {#avoid-400-error}

>[!CAUTION]
>
>Quando esegui chiamate API, accertati di utilizzare una sorta di searchCriteria. Puoi anche considerare l’utilizzo dell’impaginazione. Se il risultato di Adobe Commerce è troppo grande, la capacità di Adobe Developer App Builder potrebbe essere soddisfatta e causare una fine imprevista del file. Il risultato è un risultato di risposta non corretto come errore 400.\
> Supponiamo ad esempio che sia necessario richiedere ad Adobe Commerce tutti i prodotti correnti. L&#39;URL risultante sarà `{{base_url}}rest/V1/products?searchCriteria=all`. A seconda delle dimensioni del catalogo restituito, il codice JSON potrebbe essere troppo grande per essere utilizzato da App Builder. Utilizza invece la paginazione e effettua alcune richieste per evitare `Response is not valid 'message/http'.`
