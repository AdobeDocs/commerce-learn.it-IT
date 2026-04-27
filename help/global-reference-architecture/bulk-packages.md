---
title: Ottimizzazione di Adobe Commerce con l'architettura di riferimento globale per i pacchetti in blocco
description: Scopri come configurare Adobe Commerce utilizzando l’architettura di riferimento globale per pacchetti in blocco per una gestione efficiente del codice e il controllo delle versioni.
jira: KT-16726
doc-type: tutorial
duration: 391
audience: all
last-substantial-update: 2025-1-6
feature: Best Practices, Configuration, Install
topic: Architecture, Commerce, Development
badge: label="Intervento di Tony Evers, Sr. Technical Architect, Adobe" type="Informative" url="https://www.linkedin.com/in/evers-tony/" tooltip="Contributo di Tony Evers"
old-role: Architect, Developer
role: Developer, User, Leader
level: Beginner, Intermediate
exl-id: ac63e31e-3047-410a-a6f9-a578b495bd8c
TQID: https://experienceleague.adobe.com/q4NzQxc7XJDB-TNv2pU7ghDr6bahliY6soUGPu7fhfg
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
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
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 1188
ht-degree: 0%

---

# Schema dell’architettura di riferimento globale dei pacchetti bulk

{{only-for-on-prem-commerce-cloud}}

Questa guida spiega come configurare Adobe Commerce con il modello Bulk Packages Global Reference Architecture (GRA).

Il modello GRA per pacchetti in blocco coinvolge un singolo archivio Git per ospitare tutte le personalizzazioni comuni. Questo singolo archivio Git viene esposto tramite Compositore come un singolo pacchetto di compositore, che contiene più moduli Adobe Commerce.

![Diagramma che mostra dove è memorizzato il codice in un modello GRA per pacchetti bulk](/help/assets/global-reference-architecture/bulk-gra-pattern-diagram.png){align="center"}

## Vantaggi e svantaggi di questo modello

Vantaggi:

* Riutilizzo del codice tramite un archivio di codice condiviso
* Flessibilità per installare diverse versioni storiche del GRA su istanze diverse, consentendo rilasci graduali
* Flessibilità per il supporto e la manutenzione di più versioni principali della GRA
* Supporto per il controllo delle versioni semantiche della GRA
* Semplicità, gli sviluppatori non hanno bisogno di più competenze rispetto ai normali modelli di sviluppo per singolo negozio
* Non sono necessari strumenti speciali, infrastrutture complesse o strategie speciali di ramificazione
* La combinazione di pacchetti in una versione viene sempre sviluppata e testata insieme

Svantaggi:

* Possibilità di aggiornare l&#39;intero GRA, inclusi tutti i pacchetti in esso contenuti.
* Il pacchetto GRA bulk non supporta pacchetti compositore diversi da moduli Adobe Commerce, Language Pack e temi, quindi non supporta metapacchetti, pacchetti di componenti Magento2, plug-in e patch per Compositore

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

Installa Adobe Commerce con `bin/magento setup:install`. Install the GRA sample modules in the deployment repository with Composer:

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

That last command should result in the following output to prove that the module is installed and working:

```bash
GRA One module is installed successfully and working!
GRA Two module is installed successfully and working!
```

You can create multiple bulk packages to organize code. For instance, a third-party bulk package for third-party code that is not available through Composer. Everything that you would traditionally install in `app/code` should now be in the `src/` directory of the bulk package. An exception to that rule is code that is only used in a single instance. These packages are called local packages.

### Install local packages

The deployment repository hosts local packages. They do not live in the GRA bulk package. The location of the local packages is not `app/code` but `packages/local`. Instruct Composer to treat this directory as a repository:

```bash
composer config repositories.local path 'packages/local/*/*'
git add composer.json
git commit -m 'initialize local composer package storage'
git push origin main
```

Add the example module that is hosted at <https://github.com/AntonEvers/module-example-local>:

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

That last command should result in the following output to prove that the module is installed and working:

```bash
Local module is installed successfully and working!
```

Commit the local module to the brand repository:

```bash
git add packages/local/antonevers/module-local app/etc/config.php composer.json composer.lock 
git commit -m 'add local module'
git push origin main
```

## Overview of code locations

Only if the third-party does not offer installation through a Composer repository, you can store third-party modules in the `src/` directory of your foundation repository or a dedicated third-party bulk package.

* **Adobe Commerce core**: available through repo.magento.com.
* **Third-party modules**: available through the Marketplace or a vendor&#39;s own Composer repository.
* **Third-party modules fallback option**: stored in `src/` of a bulk package.
* **GRA foundation code**: stored in `src/` of the foundation bulk package.
* **Local code**: stored in the `packages/local` directory of the deployment repository.

## Develop a GRA module

Install the bulk package from source to enable Git in the bulk package directory:

```bash
rm -r vendor/antonevers/gra-bulk-foundation
composer install --prefer-source
```

The bulk package has been checked out using Git. When you enter the `vendor/antonevers/gra-bulk-foundation` directory, you are also entering the gra-bulk-foundation Git repository. You can create, checkout and merge branches in this directory.

Add Composer dependencies to the composer.json file at the root of the GRA bulk package, which is the only file in the bulk package that Composer evaluates.

## Include third-party modules to the GRA bulk package

Add third-party packages in the require section of the composer.json at the root of the GRA foundation to add them to your GRA. That way, the packages are always installed in all your instances through composer.

## Deliver your code

To deliver code to the main branch, there are 2 paths. First the local modules, which are merged to the main branch. Run Composer update for those modules. Do not allow developers to update composer.lock in their ticket branches to reduce conflicts. Only update the composer.lock file in staging and production branches, which reduces the risk of conflicts.

Secondly, the GRA bulk packages, which are merged into the main branch of the GRA bulk repository. Then you can add a Git tag to the main branch, versioning the Composer package. Require your new version of the GRA bulk package in the composer.json of the deployment repository to install it.

## Strategia di diramazione

This GRA pattern works with all branching strategies so long as you mirror the branching strategy of the deployment repositories in your GRA bulk repository. For releases, create a release branch with the same name in both repositories. For development, create a ticket branch in both repositories.

In ticket branches, you should almost never have to update the composer.lock file. Just check out the right branches in your development environment for both the store and the GRA foundation repository with Git. The exception is when you update requirements in the GRA foundation composer.json file. Upgrading the GRA foundation in the deployment repository is only done when building the release, or when building a QA branch.

## Esempi di codice

The code examples of this article are available as a set of Git repositories, which you can use to test the proof of concept.

* Un esempio di archivio di produzione: <https://github.com/AntonEvers/gra-bulk-brand-x>
* The GRA code repository: <https://github.com/AntonEvers/gra-bulk-foundation>
* An example local module: <https://github.com/AntonEvers/module-example-local>
