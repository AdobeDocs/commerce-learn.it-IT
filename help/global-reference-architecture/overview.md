---
title: Optimizing Code Reuse with Adobe Commerce
description: Learn how to optimize code reuse in Adobe Commerce with Global Reference Architecture patterns, enhancing performance and compliance across multiple instances.
kt: 15773
doc-type: tutorial
duration: 287
audience: all
last-substantial-update: 2025-1-6
feature: Best Practices, Configuration, Install
badge: label="Intervento di Tony Evers, Sr. Technical Architect, Adobe" type="Informative" url="https://www.linkedin.com/in/evers-tony/" tooltip="Contributo di Tony Evers"
topic: Architecture, Commerce, Development
old-role: Architect, Developer
role: Developer, User, Leader
level: Beginner, Intermediate
exl-id: 5475ade8-028c-4b24-a563-60dcda5ba93a
TQID: https://experienceleague.adobe.com/1-cE8TS4syjsMuX3VmhQu5zhFX-z3yxV-GlwxVl7eqM
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: b5f00040-57a0-4a6d-a39e-383b1936c2c9
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
  - id: f8a45b24-4be7-4f1b-909b-60d06b483a20
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 1127
ht-degree: 0%

---

# Global Reference Architecture Implementation Techniques

{{only-for-on-prem-commerce-cloud}}

There are several ways to optimize code reuse with Adobe Commerce. These four implementation techniques each have their own advantages. The examples in this article are ordered from simple to more complex. Pick the strategy that best fits your project and future roadmap. A migration from one strategy to another can be time consuming.

## When to use Global Reference Architecture

Global reference architecture can be valuable, depending on the number of instances you own. An instance is a standalone installation of Adobe Commerce using its own database. Count the number of production databases to know how many instances you own. If you maintain more than one instance, or if you foresee this scenario in the future, you can benefit from global reference architecture. The more functionality the instances share, the more value a global reference architecture adds.

In any of these scenarios, it is advisable to explore using multiple instances of Adobe Commerce.

1. **Different Store Owners**: If you maintain code for multiple store owners, each with their own distinct store, separate instances may be needed to maintain their individual requirements effectively.
2. **Compliance with National Regulations**: Certain regulations mandate that customer data must be stored within specific regions. In such cases, separate instances are essential to ensure compliance with these regulations.
3. **Operational Variances Across Geographical Regions**: Operating in multiple regions may mean differing maintenance schedules and requirements. Using separate instances allows for flexibility in managing these variations efficiently.
4. **High-Intensity Flash Sales**: Stores conducting large-scale flash sales often require optimized server performance. Dedicated infrastructure provided by separate instances ensures optimal performance during such high-demand periods.
5. **Differenze significative tra marchi o paesi**: quando la differenza tra marchi o paesi è grande, l&#39;utilizzo di una singola istanza determina il codice utilizzato solo per alcuni marchi o paesi. Istanze separate possono migliorare le prestazioni e la stabilità eliminando il codice non necessario per i marchi e i paesi che non ne hanno bisogno.

## Modelli di architettura di riferimento globali

Accanto a &quot;nessun pattern GRA&quot; ci sono quattro stili di pattern GRA.

![5 icone di pattern GRA: no GRA, split, bulk, separate e monorepo.](/help/assets/global-reference-architecture/gra-patterns-horizontal.png){align="center"}

### Nessun pattern GRA

![Icona che indica &quot;no GRA&quot;](/help/assets/global-reference-architecture/no-gra.png){align="center"}

Quando non si utilizza un pattern GRA, ogni istanza di Adobe Commerce è un’applicazione univoca. Non è possibile riutilizzare il codice, se non spostando manualmente il codice da un’istanza all’altra. Queste copie divergono sempre. La quantità di lavoro necessaria per garantire che ogni istanza abbia le stesse modifiche ma funzioni ancora come previsto può diventare travolgente. In questo scenario, tre istanze richiedono un impegno di manutenzione tre volte maggiore rispetto a una singola istanza.

![Diagramma che rappresenta 3 archivi, ognuno dei quali è una copia del precedente, con sviluppo univoco in tutte e 3 le copie.](/help/assets/global-reference-architecture/no-gra-pattern-diagram.png){align="center"}

### Il pattern Git GRA diviso

![Icona che rappresenta il pattern GRA &quot;split&quot;](/help/assets/global-reference-architecture/split-git.png){align="center"}

Questo modello è costituito da archivi Git per lo sviluppo e da un archivio Git per istanza. Ogni file nell’istanza viene mantenuto in uno degli archivi di sviluppo. Si riuniscono come una treccia formando l&#39;intero GRA. Ogni riga di codice esiste solo in un singolo archivio di sviluppo e viene installata nelle istanze utilizzando la tecnica di intreccio, determinando il riutilizzo del codice.

![Diagramma che mostra dove è memorizzato il codice in un pattern GRA diviso](/help/assets/global-reference-architecture/split-git-gra-pattern-diagram.png){align="center"}

### Il modello GRA dei pacchetti bulk

![Icona che rappresenta il modello GRA &quot;bulk&quot;](/help/assets/global-reference-architecture/bulk-packages.png){align="center"}

I moduli core e di terze parti di Adobe Commerce vengono installati direttamente tramite gli archivi del Compositore. Gli archivi Git possono essere utilizzati come archivi Compositore. In questo modello, l’intera base di codice condivisa GRA è ospitata in un singolo o in alcuni archivi Git e installata tramite Composer. The key characteristic is that multiple modules, language packs or themes are hosted in a single composer package, simplifying development.

![Diagramma che mostra dove è memorizzato il codice in un modello GRA per pacchetti bulk](/help/assets/global-reference-architecture/bulk-gra-pattern-diagram.png){align="center"}

### The separate packages GRA pattern

![An icon representing the &quot;separate packages&quot; GRA pattern](/help/assets/global-reference-architecture/separate-packages.png){align="center"}

Each Adobe Commerce module, language pack or theme is installed as a separate composer package. Each customization has its own Git repository. This means ultimate flexibility in the composition of the instances and has reliable Composer dependency management. For performance optimization, all packages are mirrored in a single private composer repository.

![A diagram showing where code is stored in a separate packages GRA pattern](/help/assets/global-reference-architecture/separate-packages-gra-pattern-diagram.png){align="center"}

### The monorepo GRA pattern

![An icon representing the &quot;monorepo&quot; GRA pattern](/help/assets/global-reference-architecture/monorepo.png){align="center"}

All development takes place in a single code repository. Automation generates packages for new versions and publishes them to a composer repository. The pattern combines the low development overhead of the bulk packages approach with the flexibility of the separate packages approach. The monorepo pattern is also ideal for running automated functional tests.

![Diagramma che mostra dove è memorizzato il codice in un modello GRA monorepo](/help/assets/global-reference-architecture/monorepo-gra-pattern-diagram.png){align="center"}

## Choosing a GRA pattern

The choice for a GRA pattern is made by assessing the project complexity, the need for flexibility, and the development team&#39;s ability to adapt.

Teams with little Adobe Commerce experience best start simple. However, if the project demands a more complex GRA pattern due to its characteristics, do not compromise.

Common project characteristics related to each pattern:

1. **No GRA pattern**: Single instance of Adobe Commerce without plans to extend. Multiple instances of Adobe Commerce with minimal commonality between them.

2. **Split Git GRA pattern**: Teams that wish to avoid Composer for their customizations, in most cases Bulk packages pattern is a preferred pattern to Split Git.

3. **Bulk package GRA pattern**: Customization codebase with high interdependency. Tutte le istanze hanno combinazioni molto simili di pacchetti personalizzati. Nessuna promozione frequente o retrocessione di singoli pacchetti. Team con poca esperienza nella gestione del codice e che necessitano di semplicità.

4. **Pattern GRA per pacchetti separati**: è necessaria una gestione flessibile dell&#39;ambito di rilascio. Sono previsti fino a 50 pacchetti personalizzati nei prossimi 5 anni. Livelli potenzialmente globali e regionali di codice comune. Nessun piano di migrazione a un modello Monorepo. Il team è tecnicamente esperto e ha una rigorosa aderenza al processo.

5. **Modello GRA Monorepo**: tutte le caratteristiche del modello GRA dei pacchetti separati. Nei prossimi 5 anni sono previsti più di 50 pacchetti. Necessità di test automatizzati estesi. Supporto per ambienti temporanei. Codebase complessa di scala aziendale che deve essere scalata senza scalare i costi di manutenzione a un tasso uguale.

### Il costo di una scelta sbagliata

È possibile migrare da un modello all’altro. Il diagramma seguente mostra il livello di impatto del passaggio da un modello all&#39;altro. Le linee verdi mostrano un impatto ridotto, mentre le linee gialle e ambrate mostrano migrazioni da moderatamente complesse a complesse.

![Diagramma che mostra frecce colorate tra tutti e 4 i pattern GRA, che indica il livello di difficoltà nel passare da uno all&#39;altro.](/help/assets/global-reference-architecture/wrong-choice.png){align="center"}
