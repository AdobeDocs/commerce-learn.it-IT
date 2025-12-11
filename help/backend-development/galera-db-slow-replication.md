---
title: Scopri come trovare query lente nei registri di query lente mysql e perché il metodo di progettazione della replica di Galera DB potrebbe essere il motivo
description: Galera DB dispone di un metodo di progettazione che rende la replica dei dati nei database secondari più lunga di quella primaria. Scopri come trovare questi eventi nel registro di query lente di mysql e il motivo per cui visualizzi le voci nei registri di query lente e forse come evitarli in futuro.
kt: 13635
doc-type: video
activity: use
last-substantial-update: 2023-7-18
feature: Backend Development, Logs, Services
topic: Commerce, Development
old-role: Architect, Developer
role: Developer
level: Intermediate
exl-id: 4a8a2df1-8cac-4bd9-851f-0eaae011b76c
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '304'
ht-degree: 0%

---

# Scopri la replica di Galera DB e le relative query lente MySQL

I cluster Galera offrono prestazioni e scalabilità. Quando si prendono in considerazione i database secondari, è importante comprendere in che modo la replica dei dati viene eseguita è diversa rispetto a quella primaria. Il database primario può eseguire operazioni in blocco. Quando la replica viene eseguita per tutti i database secondari, le operazioni vengono eseguite una alla volta. Ad esempio, se in un&#39;eliminazione sono presenti 67.000.000 elementi, nei database secondari ognuno si verifica uno alla volta. Esaminando i registri delle query lente di Mysql, è possibile che questa azione richieda molto tempo. Poiché i database secondari eseguono le operazioni una alla volta, è possibile rilevare un impatto sulle prestazioni e la mancata sincronizzazione degli elementi.

Come soluzione, se possibile, creare un batch delle operazioni di grandi dimensioni per consentire ai database secondari di rimanere sincronizzati con quelli primari. Eseguendo le operazioni in batch, consente di eseguire le azioni in modo tempestivo e ridurre al minimo l’impatto sulle prestazioni.

## A chi serve questo video?

- Architetti
- Sviluppatori
- DevOps

## Contenuto video

- Replica Galera su database secondario
- Informazioni sul controllo del flusso
- Ricerca di numeri di thread nei registri di query lente di Mysql
- Le esecuzioni in blocco si verificano solo sul primario. Le repliche avvengono una alla volta
- Assegna in batch i commit di grandi dimensioni per aiutare la replica a tenere il passo con il principale

>[!VIDEO](https://video.tv.adobe.com/v/3423541?captions=ita&learn=on)

## Risorse utili

- [Cluster Galera](https://galeracluster.com/)
