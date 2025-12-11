---
title: Ottimizzazione di Adobe Commerce con l'architettura di riferimento globale per i pacchetti in blocco
description: Scopri come configurare Adobe Commerce utilizzando l’architettura di riferimento globale per pacchetti in blocco per una gestione efficiente del codice e il controllo delle versioni.
jira: KT-16726
doc-type: tutorial
audience: all
last-substantial-update: 2025-1-6
feature: Best Practices, Configuration, Install
topic: Architecture, Commerce, Development
badge: label="Intervento di Tony Evers, Sr. Technical Architect, Adobe" type="Informative" url="https://www.linkedin.com/in/evers-tony/" tooltip="Contributo di Tony Evers"
old-role: Architect, Developer
role: Developer, User, Leader
level: Beginner, Intermediate
exl-id: ac63e31e-3047-410a-a6f9-a578b495bd8c
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '1172'
ht-degree: 0%

---

# Schema dell’architettura di riferimento globale dei pacchetti bulk

Questa guida spiega come configurare Adobe Commerce con il modello Bulk Packages Global Reference Architecture (GRA).

Il modello GRA per pacchetti in blocco coinvolge un singolo archivio Git per ospitare tutte le personalizzazioni comuni. Questo singolo archivio Git viene esposto tramite Compositore come un singolo pacchetto di compositore, che contiene più moduli Adobe Commerce.

![Diagramma che mostra dove è memorizzato il codice in un modello GRA per pacchetti bulk](/help/assets/global-reference-architecture/bulk-gra-pattern-diagram.png){align="center"}

## Vantaggi e svantaggi di questo modello

Vantaggi:

- Riutilizzo del codice tramite un archivio di codice condiviso
- Flessibilità per installare diverse versioni storiche del GRA su istanze diverse, consentendo rilasci graduali
- Flessibilità per il supporto e la manutenzione di più versioni principali della GRA
- Supporto per il controllo delle versioni semantiche della GRA
- Semplicità, gli sviluppatori non hanno bisogno di più competenze rispetto ai normali modelli di sviluppo per singolo negozio
- Non sono necessari strumenti speciali, infrastrutture complesse o strategie speciali di ramificazione
- La combinazione di pacchetti in una versione viene sempre sviluppata e testata insieme

Svantaggi:

- Possibilità di aggiornare l&#39;intero GRA, inclusi tutti i pacchetti in esso contenuti.
- Il pacchetto GRA bulk non supporta pacchetti compositore diversi da moduli Adobe Commerce, Language Pack e temi, quindi non supporta metapacchetti, pacchetti di componenti Magento2, plug-in e patch per Compositore

## Configurare Adobe Commerce con il pattern Git GRA diviso

### La struttura della directory

La GRA per pacchetti bulk installa tutto il codice riutilizzabile tramite gli archivi Composer. Il codice specifico di una singola istanza risiede nell’archivio Git dell’istanza in questione. Il codice specifico dell’istanza non viene riutilizzato in altre istanze di Adobe Commerce.

La struttura finale della directory di un’installazione completa di Adobe Commerce con il modello GRA per pacchetti bulk è simile alla seguente:

```text
.
├── app/
│   └── etc/
│       └── config.php
├── composer.json
├── composer.lock
└── packages/
    └── local/
```

Le directory `app/code`, `app/i18n` e `app/design` sono state omesse di proposito, perché Composer non valuta il codice in queste directory. Di conseguenza, le dipendenze dichiarate nei pacchetti non vengono installate automaticamente. Il modello GRA per pacchetti bulk risolve questo problema installando un codice personalizzato in `packages/` e trattando la directory come repository del compositore. Il Compositore collega i pacchetti all&#39;interno di `packages/` a `vendor/`.

### Preparare gli archivi Git

Crea due archivi Git per il codice GRA condiviso e per il primo archivio. Inizia con l’archivio GRA, che ha la seguente struttura di file:

Il risultato è la seguente struttura di directory:

```text
.
├── composer.json
└── src/
    ├── GraOne/
    │   ├── composer.json
    │   └── registration.php
    ├── GraTwo/
    │   ├── composer.json
    │   └── registration.php
    └── registration.php
```

Questa struttura di directory menziona solo il file compositore.json e il file registration.php dei moduli GraOne e GraTwo. In realtà, all&#39;interno di questi moduli sono presenti più file.

Esegui questi comandi per avviare l’archivio Git:

```bash
mkdir gra-bulk-foundation
cd gra-bulk-foundation
git init
git remote add origin git@github.com:AntonEvers/gra-bulk-foundation.git
vim composer.json # see code snippet below for contents
git add composer.json
git commit -m 'initialize GRA foundation repository'
git push -u origin main
```

Il contenuto del file compositore.json:

```json
{
    "name": "antonevers/gra-bulk-foundation",
    "description": "Shared code repository",
    "type": "library",
    "license": [
        "OSL-3.0",
        "AFL-3.0"
    ],
    "require": {},
    "autoload": {
        "files": [
            "src/registration.php"
        ],
        "psr-0": {
            "": "src/"
        }
    }
}
```

Modifica lo spazio dei nomi del pacchetto compositore.json con uno spazio dei nomi personalizzato.

Contenuto del file src/registration.php:

```php
<?php

declare(strict_types=1);

$pathList[] = dirname(__DIR__) . '/src/*/*/registration.php';
foreach ($pathList as $path) {
    $files = glob($path, GLOB_NOSORT | GLOB_ERR);
    if ($files === false) {
        throw new RuntimeException('glob() returned error while searching in \'' . $path . '\'');
    }
    foreach ($files as $file) {
        require_once $file;
    }
}
```

Il file registration.php cerca altri file registration.php all’interno dei moduli di Adobe Commerce e li esegue.

Creare i due moduli di esempio utilizzando il codice in <https://github.com/AntonEvers/gra-bulk-foundation>. Composer non valuta i file compositore.json nei moduli di esempio. Loro sono lì come abitudine. Se decidi di spostare i moduli in un’altra posizione, i file compositore.json diventano di nuovo necessari.

### Configurare l’archivio del Negozio

L’archivio di distribuzione contiene l’intera installazione di Adobe Commerce, compreso il codice GRA. Creare l’archivio di distribuzione:

```bash
mkdir gra-bulk-brand-x
cd gra-bulk-brand-x
composer create-project --repository-url=https://repo.magento.com/ magento/project-enterprise-edition .
git init
git remote add origin git@github.com:AntonEvers/gra-bulk-brand-x.git 
git add composer.json composer.lock
git commit -m 'initialize Brand X repository'
git push -u origin main
```

Installa Adobe Commerce con `bin/magento setup:install`. Installa i moduli di esempio GRA nell’archivio di distribuzione con Composer:

```bash
composer config repositories.gra-foundation vcs git@github.com:AntonEvers/gra-bulk-foundation.git
composer require antonevers/gra-bulk-foundation:@dev
bin/magento module:enable AntonEvers_GraOne AntonEvers_GraTwo
bin/magento test:gra-one
bin/magento test:gra-two
git add app/etc/config.php composer.json composer.lock
git commit -m 'install GRA foundation'
git push origin main
```

L’ultimo comando dovrebbe produrre il seguente output per dimostrare che il modulo è installato e funzionante:

```bash
GRA One module is installed successfully and working!
GRA Two module is installed successfully and working!
```

Puoi creare più pacchetti in blocco per organizzare il codice. Ad esempio, un pacchetto in blocco di terze parti per il codice di terze parti che non è disponibile tramite Composer. Tutto ciò che si installa tradizionalmente in `app/code` ora dovrebbe trovarsi nella directory `src/` del pacchetto bulk. Un&#39;eccezione a tale regola è il codice utilizzato solo in una singola istanza. Questi pacchetti sono denominati pacchetti locali.

### Installare pacchetti locali

Il repository di distribuzione ospita i pacchetti locali. Non vivono nel pacchetto di gra bulk. Il percorso dei pacchetti locali non è `app/code` ma `packages/local`. Compositore istruzioni per trattare questa directory come un archivio:

```bash
composer config repositories.local path 'packages/local/*/*'
git add composer.json
git commit -m 'initialize local composer package storage'
git push origin main
```

Aggiungere il modulo di esempio ospitato in <https://github.com/AntonEvers/module-example-local>:

```bash
mkdir -p packages/local
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

Esegui il commit del modulo locale nell’archivio del brand:

```bash
git add packages/local/antonevers/module-local app/etc/config.php composer.json composer.lock 
git commit -m 'add local module'
git push origin main
```

## Panoramica delle posizioni del codice

Solo se la terza parte non offre l&#39;installazione tramite un repository Composer, è possibile memorizzare i moduli di terze parti nella directory `src/` dell&#39;archivio di base o in un pacchetto bulk di terze parti dedicato.

- **Adobe Commerce core**: disponibile tramite repo.magento.com.
- **Moduli di terze parti**: disponibili tramite Marketplace o l&#39;archivio del Compositore di un fornitore.
- **Opzione di fallback moduli di terze parti**: archiviata in `src/` di un pacchetto bulk.
- **Codice GRA Foundation**: archiviato in `src/` del pacchetto bulk di foundation.
- **Codice locale**: archiviato nella directory `packages/local` dell&#39;archivio di distribuzione.

## Sviluppare un modulo GRA

Installa il pacchetto bulk dall’origine per abilitare Git nella directory del pacchetto bulk:

```bash
rm -r vendor/antonevers/gra-bulk-foundation
composer install --prefer-source
```

Il pacchetto bulk è stato estratto utilizzando Git. Quando si immette la directory `vendor/antonevers/gra-bulk-foundation`, si immette anche l&#39;archivio Git gra-bulk-foundation. Puoi creare, estrarre e unire rami in questa directory.

Aggiungi le dipendenze del Compositore al file compositore.json nella radice del pacchetto bulk GRA, che è l’unico file nel pacchetto bulk valutato da Composer.

## Includi moduli di terze parti nel pacchetto di massa GRA

Aggiungi pacchetti di terze parti nella sezione richiesta del file compositore.json nella directory principale della GRA Foundation per aggiungerli alla GRA. In questo modo, i pacchetti vengono sempre installati in tutte le istanze tramite Compositore.

## Distribuisci il codice

Per consegnare il codice al ramo principale, sono disponibili 2 percorsi. Innanzitutto i moduli locali, che vengono uniti al ramo principale. Esegui l’aggiornamento del Compositore per tali moduli. Non consentire agli sviluppatori di aggiornare compositore.lock nei rami dei ticket per ridurre i conflitti. Aggiorna il file compositore.lock solo nei rami di staging e produzione, riducendo il rischio di conflitti.

In secondo luogo, i pacchetti GRA bulk, che vengono uniti nel ramo principale dell’archivio GRA bulk. Quindi puoi aggiungere un tag Git al ramo principale, creando versioni del pacchetto Compositore. Richiedi la nuova versione del pacchetto bulk GRA nel file compositore.json dell’archivio di distribuzione per installarlo.

## Strategia di diramazione

Questo modello GRA funziona con tutte le strategie di diramazione purché rispecchi la strategia di diramazione degli archivi di distribuzione nell’archivio in blocco GRA. Per le versioni, crea un ramo della versione con lo stesso nome in entrambi gli archivi. Per lo sviluppo, crea una sezione ticket in entrambi gli archivi.

Nei rami ticket non dovrebbe quasi mai essere necessario aggiornare il file compositore.lock. È sufficiente controllare i rami giusti nell’ambiente di sviluppo sia per lo store che per l’archivio GRA foundation con Git. L&#39;eccezione si verifica quando si aggiornano i requisiti nel file compositore.json di GRA Foundation. L’aggiornamento di GRA Foundation nell’archivio di distribuzione viene eseguito solo durante la creazione della versione o durante la creazione di un ramo di controllo qualità.

## Esempi di codice

Gli esempi di codice di questo articolo sono disponibili come set di archivi Git, che puoi utilizzare per testare la verifica di concetto.

- Un esempio di archivio di produzione: <https://github.com/AntonEvers/gra-bulk-brand-x>
- Archivio del codice GRA: <https://github.com/AntonEvers/gra-bulk-foundation>
- Esempio di modulo locale: <https://github.com/AntonEvers/module-example-local>
