---
title: Configurazione di Adobe Commerce con l’architettura di riferimento globale Git Split
description: Scopri come configurare Adobe Commerce utilizzando l’architettura di riferimento globale Split Git per una gestione del codice efficiente e una distribuzione semplificata. ​
kt: 16725
doc-type: tutorial
audience: all
last-substantial-update: 2025-1-6
feature: Best Practices, Configuration, Install
badge: label="Intervento di Tony Evers, Sr. Technical Architect, Adobe" type="Informative" url="https://www.linkedin.com/in/evers-tony/" tooltip="Contributo di Tony Evers"
topic: Architecture, Commerce, Development
old-role: Architect, Developer
role: Developer, User, Leader
level: Beginner, Intermediate
exl-id: ac544f77-8f5f-4ad1-92b2-bdf323100c13
source-git-commit: 79d57d2c04c42a8dc23b5735e72e841b7e51cc63
workflow-type: tm+mt
source-wordcount: '1468'
ht-degree: 0%

---

# Schema dell’architettura di riferimento globale Split Git

{{only-for-on-prem-commerce-cloud}}

Questa guida spiega come configurare Adobe Commerce con il modello GRA (Global Reference Architecture) Split Git.

Il pattern Git GRA diviso coinvolge due archivi Git per lo sviluppo e un archivio Git per istanza di Adobe Commerce. Negli esempi, si presume che ogni istanza rappresenti un marchio univoco.

![Diagramma che mostra dove è memorizzato il codice in un pattern GRA diviso](/help/assets/global-reference-architecture/split-git-gra-pattern-diagram.png){align="center"}

## Vantaggi e svantaggi di questo modello

Vantaggi:

- Riutilizzo del codice tramite un archivio di codice condiviso
- Semplice pattern GRA, adatto anche per team con limitata conoscenza del Compositore
- Oltre ai moduli, ai temi e ai Language Pack di Adobe Commerce, è possibile installare qualsiasi tipo di pacchetto Composer tramite questo modello, inclusi compositore-plugin, compositore-metapackage, componente magento2 e patch
- Possibilità di rilasciare in fasi, pianificando le versioni per le aree nelle proprie finestre di manutenzione
- Supporto per i tag Git a scopo amministrativo, non per il controllo della distribuzione
- Garantire che la combinazione di pacchetti in una distribuzione di produzione sia sviluppata e testata in questa esatta configurazione

Svantaggi:

- Nessuna flessibilità aggiuntiva rispetto ad altri modelli GRA
- Impossibile aggiornare o declassare singoli moduli per istanza, aggiornare o declassare sempre l&#39;intero GRA
- Nella maggior parte dei casi, il modello dei pacchetti bulk è un adattamento migliore in quanto è ugualmente semplice, ma più convenzionale

## Configurare Adobe Commerce con il pattern Git GRA diviso

### La struttura della directory

Il pattern Git GRA diviso dispone di due tipi di archivi: archivi di sviluppo e archivi di installazione. Gli archivi di sviluppo contengono solo parte di un’installazione completa di Adobe Commerce. Gli archivi di installazione contengono l’installazione completa di Adobe Commerce e vengono utilizzati per la distribuzione, ma non per lo sviluppo.

La struttura finale della directory di un’installazione completa di Adobe Commerce con il pattern Split Git GRA è simile alla seguente:

```text
.
├── .gitignore
├── app/
│   └── etc/
│       └── config.php
├── composer.json
├── composer.lock
└── packages/
    ├── 3rdparty/
    ├── gra/
    └── local/
```

Le directory `app/code`, `app/i18n` e `app/design` sono state omesse di proposito, perché Composer non valuta il codice in queste directory. Di conseguenza, le dipendenze dichiarate nei pacchetti non vengono installate automaticamente. Il pattern Git GRA diviso risolve questo problema installando tutto il codice personalizzato in `packages/` e trattando tale directory come archivio del compositore. Il Compositore collega i pacchetti all&#39;interno di `packages/` a `vendor/`.

### Preparare gli archivi Git

Crea 3 archivi Git per:

1. Un’istanza di Adobe Commerce
2. Codice di terze parti non installato tramite Composer
3. Le tue personalizzazioni sotto forma di moduli, temi, pacchetti di lingue e così via; il tuo GRA

In questa guida vengono utilizzati i seguenti nomi per questi archivi:

1. gra-split-brand-x
2. gra-split-3rdparty
3. gra-split-gra

Tutti gli archivi nel pattern Git GRA diviso vengono uniti in un unico archivio. Affinché Git possa consentire l’unione di più archivi, tutti e tre gli archivi devono avere una cronologia condivisa. Crea un progetto Git con un singolo commit e invialo a tutti i remoti.

```bash
mkdir gra-split-brand-x
cd gra-split-brand-x
git init
git remote add origin git@github.com:AntonEvers/gra-split-brand-x.git
git remote add 3rdparty git@github.com:AntonEvers/gra-split-3rdparty.git
git remote add gra git@github.com:AntonEvers/gra-split-gra.git
touch .gitkeep
git add .gitkeep
git commit -m 'initialize repository'
git push -u origin main
git push 3rdparty main
git push gra main
```

Se si invia il file temporaneo `.gitkeep` a tutti i computer remoti, viene creato lo stesso commit iniziale con lo stesso hash di commit, creando una cronologia condivisa. Ogni modifica creata in un telecomando può essere unita alle altre.

Da qui, gli archivi divergono. L’archivio gra-split-brand-x contiene codice specifico per il brand. L’archivio gra-split-3rdparty contiene solo codice di terze parti. L’archivio gra-split-gra contiene solo la base dell’architettura di riferimento globale, costituita da tutto il codice personalizzato.

Installa Adobe Commerce nell’archivio gra-split-brand-x.

```bash
composer create-project --no-install --repository-url=https://repo.magento.com/ magento/project-enterprise-edition temp
mv temp/composer.json ./
rmdir temp
git add composer.json
git commit -m 'install Adobe Commerce'
git push origin main
```

Crea commit iniziali negli archivi gra-split-3rdparty e gra-split-gra. Il modo più semplice consiste nell’estrarre questi archivi in directory separate.

```bash
cd ..
git clone git@github.com:AntonEvers/gra-split-3rdparty.git
git clone git@github.com:AntonEvers/gra-split-gra.git
cd gra-split-3rdparty
mkdir -p packages/3rdparty
touch packages/3rdparty/.gitkeep
git add packages/3rdparty/.gitkeep
git commit -m 'initialize 3rd party package storage'
git push origin main
cd ../gra-split-gra
mkdir -p packages/gra
touch packages/gra/.gitkeep
git add packages/gra/.gitkeep
git commit -m 'initialize GRA package storage'
git push origin main
```

Questi due archivi memorizzano pacchetti di terze parti e pacchetti GRA. Può esistere un codice esclusivo per ogni istanza di Adobe Commerce. Crea un percorso per archiviare questi pacchetti locali nell’archivio gra-split-brand-x.

```bash
cd ../gra-split-brand-x
mkdir -p packages/local
touch packages/local/.gitkeep
git add packages/local/.gitkeep
git commit -m 'initialize local package storage'
git push origin main
```

### Dove memorizzare diversi tipi di codice

Adobe Commerce è un’applicazione Compositore. Il modo migliore per eseguire l’installazione è sempre tramite gli archivi del Compositore. Solo se un fornitore di moduli non offre l’installazione tramite un archivio Compositore, puoi archiviare moduli di terze parti nell’archivio di terze parti. La posizione preferita per il codice personalizzato è nell’archivio GRA. Quando un modulo viene utilizzato solo da una specifica istanza, diventa codice locale.

Riepilogo:

- **Adobe Commerce**: archiviato in un repository Composer.
- **Moduli di terze parti**: archiviati in un repository Composer.
- **Opzione di fallback moduli di terze parti**: archiviata nell&#39;archivio Git gra-split-3rdparty.
- **Codice GRA Foundation**: archiviato nell&#39;archivio Git gra-split-gra.
- **Codice locale**: archiviato nell&#39;archivio Git gra-split-brand-x.

### Collegare l&#39;archiviazione del pacchetto a Composer

Il Compositore può trattare la directory dei pacchetti come un archivio del compositore. Informa Compositore sulla posizione dei pacchetti all’interno della directory dei pacchetti.

```json
"repositories": [
  {"type": "path", "url": "packages/local/*/*"},
  {"type": "path", "url": "packages/gra/*/*"},
  {"type": "path", "url": "packages/3rdparty/*/*"},
  {"type": "composer", "url": "https://repo.magento.com"}
]
```

Composer cerca i file compositore.json con due livelli di profondità nelle tre directory di archiviazione. Creare sottodirectory nelle tre directory di archiviazione del codice esattamente come apparirebbero nella directory `vendor/`.

Ad esempio: se un pacchetto è installato normalmente in `vendor/example-corp/module-example/`, è necessario archiviarlo in `packages/3rdparty/example-corp/module-example/`. Il Compositore esegue il collegamento simbolico del pacchetto a `vendor/example-corp/module-example/` quando necessario.

Utilizzare lo spazio dei nomi e il nome del pacchetto del compositore come struttura di directory. Ad esempio: un modulo che esiste tradizionalmente in `app/code/MyCorp/MyCustomization/` ha il nome `my-corp/module-my-customization` in compositore.json. Archivia il pacchetto in `packages/gra/my-corp/module-my-customization`.

### Includi nuovi pacchetti negli archivi delle istanze

Unisci i pacchetti dei sistemi remoti di terze parti e GRA nell’archivio gra-split-brand-x.

```bash
cd gra-split-brand-x
git fetch - all
git merge gra/main 3rdparty/main
git push origin main
```

Il risultato è la seguente struttura di directory:

```text
.
├── composer.json
└── packages/
    ├── 3rdparty/
    ├── gra/
    └── local/
```

Le modifiche nell’archivio di terze parti e nell’archivio di GRA Foundation vengono unite negli archivi del brand. In questo modo, il codice GRA e di terze parti vengono mantenuti in un’unica posizione. Sposta le modifiche ai brand con un’unione Git.

Adobe Commerce non riconosce automaticamente i nuovi moduli. Per eseguire il compositore è necessario aggiungere un nuovo pacchetto dopo un&#39;unione. Esegui l’aggiornamento del compositore ogni volta che aggiorni uno dei pacchetti dopo un’unione.

### Installare i moduli di esempio

Come prova di concetto, installa moduli di esempio per vedere come funziona il modello GRA.

Eseguire `composer install` e `bin/magento install` prima di continuare.

Esistono 3 moduli di test per su GitHub:

1. [modulo-esempio-locale](https://github.com/AntonEvers/module-example-local)
2. [module-example-gra](https://github.com/AntonEvers/module-example-gra)
3. [modulo-esempio-3rdparty](https://github.com/AntonEvers/module-example-3rdparty)

### Installare un modulo locale

L&#39;aggiunta di un modulo al pool di codice locale è semplice. Scarica ed estrai il modulo. Richiedilo con Compositore. Abilitarla con `bin/magento` e confermare i file nell&#39;archivio del brand.

```bash
cd gra-split-brand-x
cd packages/local
mkdir antonevers
cd antonevers
curl -OL https://github.com/AntonEvers/module-example-local/archive/refs/heads/main.zip
unzip main.zip
rm main.zip
mv module-example-local-main module-local
git add module-local
cd ../../..
composer require antonevers/module-local:@dev
bin/magento module:enable AntonEvers_Local
bin/magento test:local
```

L’ultimo comando dovrebbe produrre il seguente output per dimostrare che il modulo è installato e funzionante:

```bash
Local module is installed successfully and working!
```

Se vedi l’output qui sopra, puoi eseguirne il commit nell’archivio del brand.

```bash
git add packages/local/antonevers/module-local app/etc/config.php composer.json composer.lock 
git commit -m 'add local module'
git push origin main
```

### Installare e sviluppare un modulo GRA Foundation

L’aggiunta di un modulo all’archivio GRA è diversa dall’installazione dei moduli locali. Per impostazione predefinita, i commit vengono aggiunti a origin/main, che è l’archivio gra-split-brand-x. Le modifiche ai moduli GRA devono essere inviate all’archivio gra-split-gra e quindi unite all’archivio gra-split-brand-x.

### Creare un ambiente di sviluppo

Crea un ambiente di sviluppo con una combinazione di tutti i pool di codice in un&#39;unica posizione. Puoi inviare il codice all’archivio locale, GRA e di terze parti singolarmente tramite symlink. Per iniziare, crea una nuova directory di sviluppo accanto al tuo marchio, alle directory GRA e repo di terze parti.

```text
.
├── gra-development    # <---
├── gra-split-3rdparty
├── gra-split-brand-x
└── gra-split-gra
```

```bash
cd ..
mkdir gra-development
cd gra-development
cp ../gra-split-brand-x/composer.json ../gra-split-brand-x/composer.lock .
mkdir packages
ln -s ../../gra-split-brand-x/packages/local/ packages/
ln -s ../../gra-split-3rdparty/packages/3rdparty/ packages/
ln -s ../../gra-split-gra/packages/gra/ packages/
```

Il risultato è la seguente struttura di directory:

```text
.
├── packages/
│ ├── 3rdparty -> ../../gra-split-3rdparty/packages/3rdparty/
│ ├── gra -> ../../gra-split-gra/packages/gra/
│ └── local -> ../../gra-split-brand-x/packages/local/
├── composer.lock
└── composer.json
```

Eseguire `composer install` e `bin/magento install` nella directory gra-development.

È ora possibile confermare le modifiche direttamente dalle directory `packages/3rdparty`, `packages/gra` e `package/local`. Git invia le modifiche all’archivio Git a cui si collegano le directory. È importante che il comando git commit venga eseguito all&#39;interno della directory `packages/3rdparty`, `packages/gra` o `package/local`. Non eseguire il commit Git nella directory principale del progetto.

### Installare i moduli di esempio

Installa i moduli di esempio di terze parti e GRA nelle directory dei pacchetti.

```bash
cd packages/gra
mkdir antonevers
cd antonevers
curl -OL https://github.com/AntonEvers/module-example-gra/archive/refs/heads/main.zip
unzip main.zip
rm main.zip
mv module-example-gra-main module-gra
git add module-gra
 
cd ../../3rdparty
mkdir antonevers
cd antonevers
curl -OL https://github.com/AntonEvers/module-example-3rdparty/archive/refs/heads/main.zip
unzip main.zip
rm main.zip
mv module-example-3rdparty-main module-3rdparty
git add module-3rdparty
 
cd ../../..
composer require antonevers/module-gra:@dev antonevers/module-3rdparty:@dev
bin/magento module:enable AntonEvers_Gra AntonEvers_ThirdParty
bin/magento test:gra
bin/magento test:3rdparty
```

L’ultimo comando dovrebbe produrre il seguente output per dimostrare che il modulo è installato e funzionante:

```bash
GRA module is installed successfully and working!
3rd party module is installed successfully and working!
```

Se vedi l’output qui sopra, puoi eseguirne il commit nell’archivio del brand. Eseguire `git remote -v` per verificare che si stia eseguendo il commit al remoto corretto.

```bash
cd packages/gra
git remote -v 
origin git@github.com:AntonEvers/gra-split-gra.git (fetch)
origin git@github.com:AntonEvers/gra-split-gra.git (push)
git add antonevers/module-gra
git commit -m 'add GRA module'
git push origin main
 
cd ../3rdparty
git remote -v 
origin git@github.com:AntonEvers/gra-split-3rdparty.git (fetch)
origin git@github.com:AntonEvers/gra-split-3rdparty.git (push)
git add antonevers/module-3rdparty
git commit -m 'add third-party module'
git push origin main
```

### Consegna del codice alle istanze

Unisci gli archivi GRA e di terze parti all’archivio gra-split-brand-x per distribuire il codice a un’istanza di Adobe Commerce. Eseguire `composer require`, `bin/magento module:enable` e confermare il risultato.

```bash
cd gra-split-brand-x
git fetch - all
git merge gra/main 3rdparty/main
composer require antonevers/module-gra:@dev antonevers/module-3rdparty:@dev
bin/magento module:enable AntonEvers_Gra AntonEvers_ThirdParty
git add app/etc/config.php composer.lock composer.json
git commit -m 'install GRA and third-party modules'
git push origin main
```

## Strategia di diramazione

Questo modello GRA funziona con tutte le strategie di ramificazione, se rispecchi la strategia di ramificazione degli archivi dei negozi nei tuoi archivi di terze parti e GRA. Per le versioni, crea un ramo della versione con lo stesso nome in tutti e tre gli archivi. Unisci i rami della versione nell’archivio durante la preparazione della versione.

A volte hai una filiale di ticket che richiede la modifica sia del codice locale che di quello di terze parti o del codice GRA. In questo caso, è necessario creare le filiali dei ticket in tutti gli archivi correlati.

Non unire mai commit di terze parti e GRA nell’archivio del brand all’interno dei rami di ticket. ma dai un&#39;occhiata ai rami giusti dell&#39;ambiente di sviluppo per ogni pool di codice. L’unione nell’archivio del brand viene eseguita solo durante la composizione della versione o durante la composizione di un ramo di controllo qualità.

## Esempi di codice

Gli esempi di codice di questo articolo sono disponibili come set di archivi Git, che puoi utilizzare per testare la verifica di concetto.

- Un esempio di archivio di produzione: <https://github.com/AntonEvers/gra-split-brand-x>
- Archivio del codice di terze parti: <https://github.com/AntonEvers/gra-split-3rdparty>
- Archivio del codice GRA: <https://github.com/AntonEvers/gra-split-gra>
- Esempio di modulo locale: <https://github.com/AntonEvers/module-example-local>
- Esempio di modulo GRA: <https://github.com/AntonEvers/module-example-gra>
- Esempio di modulo di terze parti: <https://github.com/AntonEvers/module-example-3rdparty>
