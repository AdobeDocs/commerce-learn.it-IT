---
title: Ottimizzazione del riutilizzo del codice con Adobe Commerce
description: Scopri come ottimizzare il riutilizzo del codice in Adobe Commerce con i modelli dell’architettura di riferimento globale, migliorando le prestazioni e la conformità in più istanze.
kt: 15773
doc-type: tutorial
audience: all
last-substantial-update: 2025-1-6
feature: Best Practices, Configuration, Install
badge: label="Intervento di Tony Evers, Sr. Technical Architect, Adobe" type="Informative" url="https://www.linkedin.com/in/evers-tony/" tooltip="Contributo di Tony Evers"
topic: Architecture, Commerce, Development
old-role: Architect, Developer
role: Developer, User, Leader
level: Beginner, Intermediate
exl-id: 5475ade8-028c-4b24-a563-60dcda5ba93a
source-git-commit: 79d57d2c04c42a8dc23b5735e72e841b7e51cc63
workflow-type: tm+mt
source-wordcount: '1119'
ht-degree: 0%

---

# Tecniche di implementazione dell&#39;architettura di riferimento globale

{{only-for-on-prem-commerce-cloud}}

Esistono diversi modi per ottimizzare il riutilizzo del codice con Adobe Commerce. Ognuna di queste quattro tecniche di implementazione presenta i propri vantaggi. Gli esempi in questo articolo sono ordinati da semplici a più complessi. Scegli la strategia più adatta al tuo progetto e alla roadmap futura. La migrazione da una strategia all&#39;altra può richiedere molto tempo.

## Quando utilizzare l’architettura di riferimento globale

L’architettura di riferimento globale può essere utile, a seconda del numero di istanze che possiedi. Un’istanza è un’installazione autonoma di Adobe Commerce che utilizza il proprio database. Contare il numero di database di produzione per conoscere il numero di istanze di cui si dispone. Se gestisci più di un’istanza o se prevedi questo scenario in futuro, puoi beneficiare dell’architettura di riferimento globale. Maggiore è il numero di funzionalità condivise dalle istanze, maggiore è il valore aggiunto da un&#39;architettura di riferimento globale.

In uno di questi scenari, è consigliabile esplorare utilizzando più istanze di Adobe Commerce.

1. **Proprietari store diversi**: se si gestisce il codice per più proprietari di store, ciascuno con il proprio archivio distinto, potrebbero essere necessarie istanze separate per mantenere efficacemente i singoli requisiti.
2. **Conformità alle normative nazionali**: alcune normative impongono la memorizzazione dei dati dei clienti in aree geografiche specifiche. In tali casi, sono essenziali istanze separate per garantire il rispetto di tali regolamenti.
3. **Variazioni operative tra aree geografiche**: l&#39;utilizzo in più aree geografiche può comportare diversi programmi e requisiti di manutenzione. L’utilizzo di istanze separate offre flessibilità nella gestione efficiente di tali varianti.
4. **Vendite flash ad alta intensità**: i negozi che effettuano vendite flash su larga scala richiedono spesso prestazioni server ottimizzate. L’infrastruttura dedicata fornita da istanze separate garantisce prestazioni ottimali in periodi di domanda così elevata.
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

I moduli core e di terze parti di Adobe Commerce vengono installati direttamente tramite gli archivi del Compositore. Gli archivi Git possono essere utilizzati come archivi Compositore. In questo modello, l’intera base di codice condivisa GRA è ospitata in un singolo o in alcuni archivi Git e installata tramite Composer. La caratteristica chiave è che più moduli, Language Pack o temi sono ospitati in un unico pacchetto di compositore, semplificando lo sviluppo.

![Diagramma che mostra dove è memorizzato il codice in un modello GRA per pacchetti bulk](/help/assets/global-reference-architecture/bulk-gra-pattern-diagram.png){align="center"}

### Il pattern GRA dei pacchetti separati

![Icona che rappresenta il pattern GRA dei &quot;pacchetti separati&quot;](/help/assets/global-reference-architecture/separate-packages.png){align="center"}

Ogni modulo, Language Pack o tema di Adobe Commerce viene installato come pacchetto del compositore separato. Ogni personalizzazione dispone di un proprio archivio Git. Ciò significa flessibilità finale nella composizione delle istanze e dispone di una gestione affidabile delle dipendenze del Compositore. Per ottimizzare le prestazioni, tutti i pacchetti vengono rispecchiati in un unico repository del compositore privato.

![Diagramma che mostra dove è memorizzato il codice in un pattern GRA dei pacchetti separati](/help/assets/global-reference-architecture/separate-packages-gra-pattern-diagram.png){align="center"}

### Il modello GRA monorepo

![Icona che rappresenta il pattern GRA &quot;monorepo&quot;](/help/assets/global-reference-architecture/monorepo.png){align="center"}

Tutto lo sviluppo avviene in un unico archivio di codice. L’automazione genera pacchetti per le nuove versioni e li pubblica in un archivio del compositore. Il modello combina il basso sovraccarico di sviluppo dell&#39;approccio dei pacchetti di massa con la flessibilità dell&#39;approccio dei pacchetti separati. Il modello monorepo è ideale anche per l’esecuzione di test funzionali automatizzati.

![Diagramma che mostra dove è memorizzato il codice in un modello GRA monorepo](/help/assets/global-reference-architecture/monorepo-gra-pattern-diagram.png){align="center"}

## Scelta di un pattern GRA

La scelta di un modello GRA è fatta valutando la complessità del progetto, la necessità di flessibilità e la capacità di adattamento del team di sviluppo.

Per i team con poca esperienza Adobe Commerce, la cosa migliore è iniziare semplice. Tuttavia, se il progetto richiede un modello di GRA più complesso a causa delle sue caratteristiche, non compromettere.

Caratteristiche comuni del progetto relative a ciascun modello:

1. **Nessun pattern GRA**: singola istanza di Adobe Commerce senza piani di estensione. Più istanze di Adobe Commerce con compatibilità minima tra di esse.

2. **Schema Git GRA diviso**: team che desiderano evitare Composer per le proprie personalizzazioni; nella maggior parte dei casi, il pattern Bulk packages è quello preferito a Split Git.

3. **Pattern GRA per pacchetto bulk**: base di codice di personalizzazione con interdipendenza elevata. Tutte le istanze hanno combinazioni molto simili di pacchetti personalizzati. Nessuna promozione frequente o retrocessione di singoli pacchetti. Team con poca esperienza nella gestione del codice e che necessitano di semplicità.

4. **Pattern GRA per pacchetti separati**: è necessaria una gestione flessibile dell&#39;ambito di rilascio. Sono previsti fino a 50 pacchetti personalizzati nei prossimi 5 anni. Livelli potenzialmente globali e regionali di codice comune. Nessun piano di migrazione a un modello Monorepo. Il team è tecnicamente esperto e ha una rigorosa aderenza al processo.

5. **Modello GRA Monorepo**: tutte le caratteristiche del modello GRA dei pacchetti separati. Nei prossimi 5 anni sono previsti più di 50 pacchetti. Necessità di test automatizzati estesi. Supporto per ambienti temporanei. Codebase complessa di scala aziendale che deve essere scalata senza scalare i costi di manutenzione a un tasso uguale.

### Il costo di una scelta sbagliata

È possibile migrare da un modello all’altro. Il diagramma seguente mostra il livello di impatto del passaggio da un modello all&#39;altro. Le linee verdi mostrano un impatto ridotto, mentre le linee gialle e ambrate mostrano migrazioni da moderatamente complesse a complesse.

![Diagramma che mostra frecce colorate tra tutti e 4 i pattern GRA, che indica il livello di difficoltà nel passare da uno all&#39;altro.](/help/assets/global-reference-architecture/wrong-choice.png){align="center"}
