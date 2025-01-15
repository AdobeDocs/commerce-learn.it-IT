---
title: Architettura di riferimento globale Monorepo
description: Scopri come utilizzare l’approccio monorepo per l’architettura di riferimento globale per creare un’esperienza di e-commerce scalabile e resiliente
jira: KT-16728
doc-type: tutorial
audience: all
last-substantial-update: 2025-1-6
feature: Best Practices, Configuration, Install
badge: label="Intervento di Tony Evers, Sr. Technical Architect, Adobe" type="Informative" url="https://www.linkedin.com/in/evers-tony/" tooltip="Contributo di Tony Evers"
topic: Architecture, Commerce, Development
role: Architect, Developer, User, Leader
level: Intermediate, Advanced
source-git-commit: 421c6de9421d715127d0c892dc05ef22f77149cf
workflow-type: tm+mt
source-wordcount: '1371'
ht-degree: 0%

---


# Schema dell’architettura di riferimento globale Monorepo

Questa guida spiega come configurare Adobe Commerce con il modello GRA (Monorepo Global Reference Architecture).

Il modello GRA Monorepo coinvolge un singolo archivio Git per ospitare tutte le personalizzazioni comuni. Questo singolo archivio Git viene esposto tramite Compositore come pacchetti di compositore separati.

![Diagramma che mostra dove è memorizzato il codice in un modello GRA monorepo](/help/assets/global-reference-architecture/monorepo-gra-pattern-diagram.png){align="center"}

## Vantaggi e svantaggi di questo modello

Vantaggi:

- Ideale per test funzionali
- Riutilizzo del codice tramite archivi di codice condivisi
- Massima flessibilità nell&#39;installazione dei pacchetti: ogni pacchetto GRA può essere aggiornato, aggiornato o supportato singolarmente
- Supporto completo per il controllo delle versioni semantiche
- Non sono necessari strumenti speciali, infrastrutture complesse o strategie speciali di ramificazione
- Supporto per tutti i tipi di pacchetti supportati da Composer
- Ideale per ambienti temporanei, che sono facoltativi, ma per team di distribuzione di volumi elevati sono molto utili

Svantaggi:

- Possibilità di distribuire combinazioni di pacchetti non sviluppati nella stessa configurazione, necessità di rigorose procedure di test
- Il modello GRA monorepo può essere complesso all&#39;inizio. Assegna un lead che aiuti il team a lavorare con il sistema

## Configurare Adobe Commerce con il pattern GRA per pacchetti separati

### La struttura della directory

La struttura di directory finale di un’installazione completa di Adobe Commerce con il pattern GRA dei pacchetti separati presenta la seguente struttura di directory:

```text
.
├── app/
│   └── etc/
│       └── config.php
├── packages/
│   └── ...
├── composer.json
└── composer.lock
```

Un archivio Git di produzione ha la seguente struttura di directory:

```text
.
├── app/
│   └── etc/
│       └── config.php
├── composer.json
└── composer.lock
```

La differenza è che le istanze di produzione si installano da Composer, dove il monorepo conserva la propria copia di ogni pacchetto all’interno della directory dei pacchetti.

### Preparare un archivio di produzione

Crea un archivio per la prima istanza di Adobe Commerce, che rappresenta un negozio web per il Brand X.

```bash
mkdir gra-monorepo-brand-x
cd gra-monorepo-brand-x
composer create-project --repository-url=https://repo.magento.com/ magento/project-enterprise-edition .
git init
git remote add origin git@github.com:AntonEvers/gra-monorepo-brand-x.git 
git add composer.json composer.lock
git commit -m 'initialize Brand X repository'
git push -u origin main
```

Installa Adobe Commerce con `bin/magento setup:install`. Eseguire il commit dei file `app/etc/config.php` e del compositore risultanti. Composer gestisce qualsiasi altra cosa in modo che non ci siano altri elementi in Git.

### Preparare l’archivio di monorepo

L’archivio monorepo inizia con gli stessi passaggi.

```bash
mkdir gra-monorepo 
cd gra-monorepo
composer create-project --repository-url=https://repo.magento.com/ magento/project-enterprise-edition .
```

Installa Adobe Commerce con `bin/magento setup:install`, esegui commit e push.

```bash
git init
git remote add origin git@github.com:AntonEvers/gra-monorepo.git 
git add composer.json composer.lock app/etc/config.php
git commit -m 'initialize monorepo GRA development repository'
git push -u origin main
```

### Prepararsi per lo sviluppo di monorepo

Per lo sviluppo di monorepo, apporta le seguenti modifiche al file compositore.json:

1. Modifica il nome e la descrizione del pacchetto in modo che sia chiaro che si tratta del pacchetto monorepo.
1. Elimina l’attributo version da compositore.json, perché la versione viene gestita utilizzando i tag Git per questo archivio.
1. Sostituisci la sezione require con un metapacchetto creato in un secondo momento.
1. Modifica la stabilità minima in dev.
1. Aggiungere un archivio di tipo percorso che punti a `packages/*/*` per ospitare pacchetti monorepo, inclusi gli alias di ramo per ogni pacchetto contenuto
1. Aggiungi un alias di ramo per il progetto stesso

La seguente differenza Git mostra la differenza tra un’installazione pulita di Adobe Commerce e le modifiche sopra menzionate:

```diff
@@ -1,6 +1,6 @@
 {
-    "name": "magento/project-enterprise-edition",
-    "description": "eCommerce Platform for Growth (Enterprise Edition)",
+    "name": "antonevers/gra-monorepo",
+    "description": "Monorepo repository for Global Reference Architecture development",
     "type": "project",
     "license": [
         "proprietary"
@@ -15,11 +15,8 @@
         "preferred-install": "dist",
         "sort-packages": true
     },
-    "version": "2.4.7-p3",
     "require": {
-        "magento/product-enterprise-edition": "2.4.7-p3",
-        "magento/composer-dependency-version-audit-plugin": "~0.1",
-        "magento/composer-root-update-plugin": "^2.0.4"
+        "antonevers/gra-meta-brand-x": "self.version"
     },
     "autoload": {
         "exclude-from-classmap": [
@@ -69,16 +66,33 @@
             "Magento\\Tools\\Sanity\\": "dev/build/publication/sanity/Magento/Tools/Sanity/"
         }
     },
-    "minimum-stability": "stable",
+    "minimum-stability": "dev",
     "prefer-stable": true,
     "repositories": [
         {
+            "type": "path",
+            "url": "packages/*/*",
+            "options": {
+                "versions": {
+                    "antonevers/gra-meta-brand-x": "1.4.x-dev",
+                    "antonevers/gra-meta-foundation": "1.4.x-dev",
+                    "antonevers/gra-component-foundation": "1.4.x-dev",
+                    "antonevers/module-gra": "1.4.x-dev",
+                    "antonevers/module-3rdparty": "1.4.x-dev",
+                    "antonevers/module-local": "1.4.x-dev"
+                }
+            }
+        },
+        {
             "type": "composer",
             "url": "https://repo.magento.com/"
         }
     ],
     "extra": {
-        "magento-force": "override"
+        "magento-force": "override",
+        "branch-alias": {
+            "dev-main": "1.4.x-dev"
+        }
     }
 }
```

### Utilizzare i metapacchetti

Scarica il codice di esempio da [AntonEvers/gra-meta-foundation](https://github.com/AntonEvers/gra-meta-foundation) su GitHub per ottenere i metapackages e i moduli di esempio utilizzati in questo esempio.

I metapacchetti del compositore raggruppano più pacchetti del compositore in un unico pacchetto. Quando un metapackage è richiesto, tutti i pacchetti che esso bundle sono installati automaticamente attraverso la sezione Composer require del metapackage.

Per questo esempio, sono disponibili due metapackages:

1. **antonevers/gra-meta-brand-x**: metapacchetto che contiene tutto ciò che costituisce &quot;Brand X&quot;
1. **antonevers/gra-meta-foundation**: metapackage contenente tutto ciò che è sempre installato in qualsiasi marchio

Il metapacchetto del marchio richiede il metapacchetto di base. Quando è richiesto il metapacchetto del brand, è automaticamente richiesto anche il metapacchetto di base. Consulta i due file compositore.json dei metapackages per vedere come si relazionano:

antonevers/gra-meta-brand-x:

```json
{
    "name": "antonevers/gra-meta-brand-x",
    "type": "metapackage",
    "license": [
        "OSL-3.0",
        "AFL-3.0"
    ],
    "require": {
        "antonevers/gra-meta-foundation": "^1.4",
        "antonevers/module-local": "^1.4"
    }
}
```

antonevers/gra-meta-foundation:

```json
{
    "name": "antonevers/gra-meta-foundation",
    "type": "metapackage",
    "license": [
        "OSL-3.0",
        "AFL-3.0"
    ],
    "require": {
        "antonevers/gra-component-foundation": "^1.4",
        "antonevers/module-gra": "^1.4",
        "antonevers/module-3rdparty": "^1.4",
        "magento/composer-dependency-version-audit-plugin": "~0.1",
        "magento/composer-root-update-plugin": "^2.0.4",
        "magento/product-enterprise-edition": "2.4.7-p3"
    }
}
```

I metapacchetti sono un buon modo per organizzare il codice che appartiene tra loro. Utilizza i metapacchetti per definire gruppi di pacchetti regionali, globali, specifici per il brand o qualsiasi gruppo utile per te. Se gestisci più installazioni di Adobe Commerce, i matapacchetti rappresentano un modo sicuro e versatile di definire il contesto in cui un pacchetto è previsto.

Metapackages presenti nel monorepo all&#39;interno della directory `packages`. In questo caso, viene eseguito il mirroring della struttura di directory di `vendor`:

```text
.
├── packages/
│   └── antonevers
│       ├── gra-meta-brand-x
│       │   └── composer.json
│       └── gra-meta-foundation
│           └── composer.json
├── composer.json
└── composer.lock
```

### Aggiungere e sviluppare moduli

I moduli nel monorepo esistono nella directory `packages`. In questo modo Composer può trovarli attraverso l’archivio del tipo di percorso.

Scarica il codice di esempio da [AntonEvers/gra-meta-foundation](https://github.com/AntonEvers/gra-meta-foundation) su GitHub per ottenere i metapackages e i moduli di esempio utilizzati in questo esempio.

```text
.
├── packages/
│   └── antonevers
│       ├── gra-meta-brand-x
│       ├── gra-meta-foundation
│       ├── module-3rdparty
│       ├── module-gra
│       └── module-local
├── composer.json
└── composer.lock
```

Se necessario, nella directory `packages` possono essere presenti più spazi dei nomi.

Lo sviluppo avviene nella directory dei pacchetti. I collegamenti simbolici ai pacchetti all&#39;interno della directory `packages` vengono creati nella directory `vendor` una volta eseguito `composer update`. In questo modo, il codice diventa parte dell’installazione di Adobe Commerce.

Eseguire `bin/magento module:enable --all` o solo per moduli specifici per abilitare i moduli aggiunti.

A questo punto è necessario disporre di un’installazione di Adobe Commerce funzionante con i tre moduli di esempio installati e funzionanti. Puoi verificare se i moduli sono installati e funzionano eseguendo i comandi:

```bash
bin/magento test:gra
bin/magento test:3rdparty
bin/magento test:local
```

### Creazione automatizzata dei pacchetti

Sono disponibili diverse opzioni per la creazione automatica dei pacchetti. Alcune opzioni sono:

1. [Packagist privato](https://packagist.com/)
1. [Semplifica Generatore Monorepo](https://github.com/symplify/monorepo-builder)
1. Creare una soluzione personalizzata

[Private Packagist](https://packagist.com/) automatizza il riconoscimento dei pacchetti in Git monorepo e li espone tramite Composer. È compatibile con Adobe Commerce, veloce, a bassa manutenzione e soggetto a errori, ed è per questo che questa guida si concentra sull’opzione Private Packagist.

Non rientra nell&#39;ambito di questa guida spiegare come impostare l&#39;elenco dei package privati. Vedere i [documenti](https://packagist.com/docs).

Esiste la possibilità di trasformare un pacchetto in un monorepo dopo aver configurato la sincronizzazione dell’organizzazione e gli archivi Git vengono sincronizzati automaticamente in Private Packagist.

Per prima cosa, vai alla scheda dei pacchetti e trova il monorepo:

![Schermata del Packagist privato con il pacchetto monorepo visibile nella schermata dei pacchetti](/help/assets/global-reference-architecture/packagist-packages-before-multi-package.png){align="center"}

Fai clic sul pacchetto monorepo e fai clic su &quot;Modifica&quot; nella schermata dei dettagli, che ti porta alla seguente pagina:

![Schermata di Private Packagist con la pagina di modifica del pacchetto monorepo](/help/assets/global-reference-architecture/packagist-packages-edit.png)

Sotto il primo campo di input è presente un collegamento che indica: Creare un archivio con più pacchetti. Fai clic su questo collegamento.

![Schermata del Packagist privato con la configurazione di più pacchetti](/help/assets/global-reference-architecture/packagist-packages-multi-package.png)

Definisci la posizione in cui trovare i pacchetti del compositore all’interno del monorepo. Nell&#39;esempio, la posizione è `packages/**/composer.json`. Modifica la posizione in modo da limitare o ampliare la ricerca dei pacchetti da estrarre da parte di Private Packagist.

La scheda dei pacchetti mostra tutti i pacchetti trovati dopo il salvataggio e il monorepo stesso non sarà più visibile come pacchetto Compositore:

![Schermata Packagist privato con tutti i pacchetti monorepo visibili nella schermata dei pacchetti](/help/assets/global-reference-architecture/packagist-packages-after-multi-package.png)

Viene creata una versione in Composer per ogni pacchetto all’interno del monorepo, per ogni tag o ramo creato sul monorepo in Git.

## Installare i pacchetti nell’ambiente di produzione

Seguire le istruzioni di Private Packagist per aggiungere Private Packagist come repository del compositore. Private Packagist può e deve essere utilizzato come mirror per tutti gli archivi Composer e Git, incluso packagist.org. In questo modo, le credenziali non devono essere condivise con gli sviluppatori e hai il controllo completo su ogni pacchetto. Questo esempio non segue questa best practice in quanto esporrebbe pubblicamente la base di codice di Adobe Commerce.

Scarica [GRA Monorepo Brand X](https://github.com/AntonEvers/gra-monorepo-brand-x) da GitHub per visualizzare un esempio di un negozio di produzione.

Nell&#39;archivio di produzione non è presente alcuna directory `packages` e tutti i pacchetti vengono installati tramite Composer. L’unico pacchetto richiesto è:

```json
    "require": {
        "antonevers/gra-meta-brand-x": "^1.0"
    },
```

Eppure, tutto Adobe Commerce e l&#39;intero GRA è installato attraverso i requisiti di questo metapackage.

## Controllo delle versioni

Tutti i pacchetti del monorepo ricevono la stessa versione del monorepo stesso. Consideralo come pubblicazione di nuove versioni dell’intera applicazione. In produzione, tuttavia, è possibile installare un mix di pacchetti da diverse versioni monorepo.

## Ambienti effimeri

Se si utilizzano ambienti effimeri o si prevede di utilizzarli, il monorepo è una scelta eccellente. Ogni versione e ramo del monorepo contiene tutti i file di moduli personalizzati, di terze parti e di Adobe Commerce. Con un’installazione completa in ogni ramo, è possibile eseguire ogni tipo di test, compresi i test funzionali. Con altre configurazioni GRA, come i pacchetti separati o i pacchetti in blocco GRA, è innanzitutto necessario creare un ambiente Adobe Commerce funzionante prima di poter eseguire i test funzionali. Dal punto di vista di DevOps, monorepo lo rende molto più semplice.

## Esempi di codice

Gli esempi di codice di questo articolo sono stati combinati in un set di archivi Git, che puoi utilizzare per riprodurre con la prova del concetto.

- Esempio di archivio monorepo: <https://github.com/AntonEvers/gra-monorepo>
- Un esempio di archivio di produzione: <https://github.com/AntonEvers/gra-monorepo-brand-x>
