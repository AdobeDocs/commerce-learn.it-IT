---
title: Diagnosticare la replica del database di Galera nei registri di query lente MySQL
description: Scopri in che modo la progettazione di replica di Galera DB rallenta le sincronizzazioni secondarie del database, come identificare questi eventi nei registri di query lente di MySQL e come ridurre al minimo l’impatto.
doc-type: Technical Video
duration: 452
last-substantial-update: 2023-07-18
feature: Backend Development, Logs, Services
topic: Commerce, Development
role: Developer
level: Intermediate
jira: KT-13635
exl-id: 4a8a2df1-8cac-4bd9-851f-0eaae011b76c
TQID: https://experienceleague.adobe.com/NYapiIjnRv5RAS1glm8do16M4jUPmbgfVCs6ICQwbUc
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
source-git-commit: add3e29f8841ca4ca99f4c40afc656f00e93ec36
workflow-type: tm+mt
source-wordcount: 262
ht-degree: 0%

---

# Scopri la replica di Galera DB e le relative query lente MySQL

I cluster Galera offrono prestazioni e scalabilità. Quando si prendono in considerazione i database di replica, è importante comprendere che il modo in cui viene eseguita la replica dei dati è diverso rispetto a quello della replica primaria. Il database primario può eseguire operazioni in blocco. Quando la replica viene eseguita per tutti i database di replica, le operazioni vengono eseguite una alla volta. Ad esempio, se in un&#39;eliminazione sono presenti 67.000.000 elementi, nei database di replica ognuno di essi si verifica uno alla volta. Se si esaminano i log delle query lente di MySQL, l&#39;operazione potrebbe richiedere molto tempo. Il fatto che i database di replica eseguano operazioni in sequenza è un motivo per cui non è possibile sincronizzare le operazioni e rilevare l&#39;impatto sulle prestazioni.

Per consentire la sincronizzazione dei database di replica con il database primario, eseguire il batch delle operazioni di grandi dimensioni quando possibile. Eseguendo le operazioni in batch, consente di eseguire le azioni in modo tempestivo e ridurre al minimo l’impatto sulle prestazioni.

## Pubblico previsto

* Architetti
* Sviluppatori
* DevOps

## Contenuto video

* Replica di Galera nel database di replica
* Informazioni sul controllo del flusso
* Ricerca di numeri di thread nei registri di query lente di Mysql
* Le esecuzioni in blocco si verificano solo sul primario. Le repliche avvengono una alla volta
* Per aiutare la replica a tenere il passo con il principale, crea un batch delle commit di grandi dimensioni.

>[!VIDEO](https://video.tv.adobe.com/v/3423541?captions=ita&learn=on)

## Risorse utili

* [Cluster Galera](https://mariadb.com/products/enterprise/galera-cluster/)
