---
title: Architettura di riferimento globale per pacchetti separati
description: Ottimizza Adobe Commerce con pacchetti GRA separati. Scopri configurazione, vantaggi e best practice per una gestione flessibile dei pacchetti con versioni.
jira: KT-16727
doc-type: tutorial
duration: 594
audience: all
last-substantial-update: 2025-1-6
feature: Best Practices, Configuration, Install
topic: Architecture, Commerce, Development
badge: label="Intervento di Tony Evers, Sr. Technical Architect, Adobe" type="Informative" url="https://www.linkedin.com/in/evers-tony/" tooltip="Contributo di Tony Evers"
old-role: Architect, Developer
role: Developer, User, Leader
level: Beginner, Intermediate
exl-id: cbddc4a3-602f-4208-85cd-b906d2b81f8b
TQID: https://experienceleague.adobe.com/ihTCXVhaBPi5-6Xs1tiB-wDbVX-1CwHSgz80X0B02ts
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: ba9e5be9-7de1-4f71-a5d2-baead0e425eeid: dac87252-6066-4d6e-a9d2-f6d84c323de7id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2: id: b69b2659-1057-424e-8fc5-ed9e016dc554id: f8a45b24-4be7-4f1b-909b-60d06b483a20id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2: id: b5a62a22-46f7-4f0d-b151-3fc640bef588id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 2132
ht-degree: 0%

---

# Schema dell&#39;architettura di riferimento globale dei pacchetti separati

{{only-for-on-prem-commerce-cloud}}

Questa guida spiega come configurare Adobe Commerce con il modello GRA (Separate Packages Global Reference Architecture).

Il modello GRA per pacchetti separati coinvolge un archivio Git per ogni pacchetto comune e un archivio Git per ogni istanza di Adobe Commerce. I pacchetti comuni vengono esposti tramite Compositore con un archivio del compositore privato.

Questo modello di architettura di riferimento globale è completamente basato su Compositore ed è progettato per ottenere il massimo beneficio da tutte le funzioni di Compositore.

![Diagramma che mostra dove è memorizzato il codice in un pattern GRA per pacchetti separati](/help/assets/global-reference-architecture/separate-packages-gra-pattern-diagram.png){align="center"}

## Vantaggi e svantaggi di questo modello

Vantaggi:

* Riutilizzo del codice tramite archivi di codice condivisi
* Massima flessibilità nell&#39;installazione dei pacchetti: ogni pacchetto GRA può essere aggiornato, aggiornato o supportato singolarmente
* Supporto completo per il controllo delle versioni semantiche
* Non sono necessari strumenti speciali, infrastrutture complesse o strategie speciali di ramificazione
* Supporto per tutti i tipi di pacchetti supportati da Composer

Svantaggi:

* Lo sviluppo all&#39;interno di questo modello GRA è leggermente più difficile all&#39;inizio, c&#39;è una piccola curva di apprendimento
* Possibilità di distribuire combinazioni di pacchetti non sviluppati nella stessa configurazione, necessità di rigorose procedure di test

## Configurare Adobe Commerce con il pattern GRA per pacchetti separati

### La struttura della directory

La struttura di directory finale di un’installazione completa di Adobe Commerce con il modello GRA per pacchetti separati è simile alla seguente:

```text
.
├── app/
│   └── etc/
│       └── config.php
├── composer.json
└── composer.lock
```

Le directory `app/code`, `app/i18n` e `app/design` sono state omesse di proposito. Il GRA dei pacchetti separati installa ogni singolo pacchetto da Composer. Anche se il pacchetto viene installato solo in una singola istanza di Adobe Commerce.

### Preparare l’archivio dello store

Crea un archivio per la prima istanza di Adobe Commerce, che rappresenta un negozio web per il Brand X.

```bash
mkdir gra-separate-brand-x
cd gra-separate-brand-x
composer create-project --repository-url=https://repo.magento.com/ magento/project-enterprise-edition .
git init
git remote add origin git@github.com:AntonEvers/gra-separate-brand-x.git 
git add composer.json composer.lock
git commit -m 'initialize Brand X repository'
git push -u origin main
```

Installa Adobe Commerce con `bin/magento setup:install`. Esegue il commit del `app/etc/config.php` risultante.

### Creare archivi di pacchetti

Ogni pacchetto in questo modello di architettura di riferimento globale dispone di un proprio archivio Git. Di seguito sono riportati alcuni pacchetti di esempio contenenti moduli Adobe Commerce che rappresentano un modulo GRA, un modulo di terze parti e un modulo locale.

* <https://github.com/AntonEvers/module-example-gra>
* <https://github.com/AntonEvers/module-example-3rdparty>
* <https://github.com/AntonEvers/module-example-local>

Utilizza gli esempi per creare pacchetti personalizzati.

### Creare archivi di metapackage

I metapacchetti controllano l&#39;ambito della base di codice comune GRA in questo modello GRA. Definiscono ciò che è alla base: quale combinazione di versioni dei pacchetti viene sempre installata insieme. Ecco un esempio:

```json
{
    "name": "antonevers/gra-meta-foundation",
    "type": "metapackage",
    "require": {
        "antonevers/gra-component-foundation": "~1.0",
        "antonevers/module-gra": "~1.0",
        "antonevers/module-3rdparty": "~1.0",
        "magento/composer-dependency-version-audit-plugin": "~0.1",
        "magento/composer-root-update-plugin": "~2.0",
        "magento/product-enterprise-edition": "2.4.6-p3"
    }
}
```

Lo snippet qui sopra è il compositore.json di un metapackage. Poiché i metapacchetti contengono solo un file compositore.json e nessun altro codice. Il codice riportato sopra è anche il metapacchetto completo. Inseriscilo in un archivio Git e disponi di un archivio del compositore di metapackage installabile. Richiede un modulo GRA di esempio, un modulo di terze parti e il core Adobe Commerce. Richiede anche la gra-component-foundation, che verrà spiegata nel prossimo capitolo.

I metapacchetti sono un modo per raggruppare i pacchetti senza creare dipendenze tra i pacchetti. Quindi, anche quando non c&#39;è alcuna dipendenza tecnica tra i pacchetti, con un metapackage puoi farli installare insieme. Se nel progetto è necessario questo metapackage, viene installato qualsiasi pacchetto o metapackage necessario per il metapackage. Quindi, se crei un progetto di compositore vuoto e hai solo bisogno di questo pacchetto, Composer installa Adobe Commerce e il GRA e il modulo di terze parti.

In questo modo puoi assicurarti che ogni negozio contenga lo stesso insieme di pacchetti fondamentali.

Allo stesso modo, puoi definire un metapacchetto che definisce lo store x. Richiede il metapacchetto di base, che richiede la base GRA completa, oltre a un modulo locale:

```json
{
    "name": "antonevers/gra-meta-brand-x",
    "type": "metapackage",
    "require": {
        "antonevers/gra-meta-foundation": "~1.0",
        "antonevers/module-local": "~1.0"
    }
}
```

Il metapacchetto Brand-X è opzionale. Puoi anche saltare il metapacchetto del marchio e richiedere queste dipendenze direttamente nel progetto Compositore negozio. Il vantaggio di creare un metapacchetto per i moduli locali è che non disponi di rami di funzioni e richieste pull di funzioni sull’archivio Git dello store, ma solo negli archivi dei pacchetti. È una misura di sicurezza. Inoltre, puoi scegliere di applicare il controllo delle versioni semantiche negli archivi dei pacchetti e utilizzare tag Git diversi nel progetto principale, ad esempio per tenere traccia delle versioni denominate. Sta a te.

### File di base GRA all&#39;esterno della directory del fornitore

A volte è necessario archiviare i file all’esterno della directory del fornitore. Ad esempio `.gitignore`, file che si trovano nella directory `dev/` o file di verifica del dominio. Il tipo di pacchetto magento2-component è progettato per questo scopo. Osserva <https://github.com/AntonEvers/gra-component-foundation>.

```json
{
    "name": "antonevers/gra-component-foundation",
    "type": "magento2-component",
    "require": {
        "magento/magento-composer-installer": "*"
    },
    "extra": {
        "map": [
            [
                "src/gitignore",
                ".gitignore"
            ]
        ]
    }
}
```

Questo pacchetto ha il tipo magento2-component e contiene una directory src che ospita i file copiati nella directory principale di Adobe Commerce. Il mapping in questo file copia `/src/gitignore` in `/.gitignore` nel progetto Composer principale.

In questo modo è possibile rendere parte della GRA Foundation anche file esterni alla directory del fornitore.

### Sviluppo di un modulo GRA Foundation

Lo sviluppo avviene all’interno della directory del fornitore. Chiedi a Composer di installare i pacchetti di base dall’origine. In questo modo, estrae i pacchetti da Git invece di installarli da un archivio scaricato.

```bash
rm -r vendor/antonevers/*
composer install --prefer-source
```

Con questo comando, i pacchetti nello spazio dei nomi antonevers sono stati estratti utilizzando Git. Quando si accede alla directory vendor/antonevers/module-gra, si accede anche all’archivio Git module-gra. Ora puoi creare, estrarre e unire i rami sul posto e sviluppare in questo modo, direttamente dalla directory del fornitore.

### Includi moduli di terze parti nella GRA foundation

Aggiungere pacchetti di terze parti al metapacchetto GRA. Se il codice di terze parti non è disponibile per l’installazione da un archivio Compositore, crea un pacchetto per esso. Crea un archivio Git, aggiungi i contenuti dei pacchetti (tutto ciò che si trova in app/code/Vendor/Package) e assicurati che nella directory principale dell’archivio sia presente un file compositore.json valido. Ora puoi installare questo pacchetto tramite Composer.

## Configurare un archivio Compositore privato

Un archivio privato non è obbligatorio nell’architettura di riferimento globale. Semplifica le distribuzioni e l’installazione, riduce la configurazione dell’archivio in compositore.json e aumenta la sicurezza. Le credenziali di altri archivi del Compositore e del marketplace Adobe Commerce vengono memorizzate nell’archivio privato. Non ci sono credenziali più sensibili raggruppate con il codice o sui computer degli sviluppatori.

Inoltre, alcuni archivi privati offrono funzionalità aggiuntive, ad esempio notifiche e-mail quando uno dei tuoi archivi contiene una vulnerabilità di sicurezza in una delle sue dipendenze.

Il problema della lentezza si verifica quando si dispone di più archivi VCS in compositore.json. Ogni archivio Compositore deve essere letto quando si eseguono aggiornamenti e ha 50 archivi per 50 pacchetti e ha un sovraccarico almeno 50 volte superiore rispetto a un solo archivio Compositore.

![Diagramma che mostra dove si verifica la lentezza quando manca un repository del compositore](/help/assets/global-reference-architecture/separate-packages-without-mirror-diagram.png){align="center"}

Includi un mirror del Compositore sotto forma di archivio del Compositore privato. Il mirror contiene una copia di tutti i pacchetti da altri archivi del Compositore, nonché di tutti i pacchetti ospitati Git. Con un archivio Compositore privato, puoi inoltre ottenere un controllo dell’accesso granulare.

Con la sincronizzazione Git, un archivio Compositore privato rileva automaticamente i nuovi pacchetti negli archivi Git e le nuove versioni dei pacchetti esistenti.

È possibile ospitare il proprio archivio privato con Satis: <https://composer.github.io/satis/>. Vedi un esempio di archivio pubblico in <https://antonevers.github.io/gra-composer-repository/>. Questo repository viene utilizzato come repository del compositore negli esempi di codice. Sono necessarie misure aggiuntive per rendere privato un archivio Satis.

È possibile configurare e dimenticare alcune soluzioni: Packagist privato <https://packagist.com/>, creato dalle stesse persone che hanno scritto Composer o JFrog Artifactory <https://jfrog.com/artifactory/>.

## Codice di consegna

Con i metapacchetti, ci sono 3 passaggi per consegnare il codice.

1. Unisci le modifiche in pacchetti e crea una versione dei pacchetti modificati.
2. (Facoltativo, solo se vengono aggiunti nuovi pacchetti) Richiedi i nuovi pacchetti in metapackages e crea una versione dei metapackages.
3. (Facoltativo, solo se vengono aggiunti nuovi pacchetti) Richiedi i nuovi metapacchetti in Adobe Commerce e implementali.

L’ambito di distribuzione è controllato con le versioni del pacchetto. La creazione di una versione stabile di un pacchetto indica che il pacchetto è pronto per la distribuzione in produzione.

Per creare una nuova versione, esegui l’aggiornamento del compositore nel progetto principale Composer, che contiene l’installazione completa dell’archivio. Vengono installate tutte le versioni più recenti dei pacchetti.

## Controllo delle versioni

Il controllo delle versioni in pacchetti separati GRA è sinonimo di assegnazione di tag ai moduli in Git. I tag Git creano versioni numerate dei pacchetti installati da Composer.
Il corretto approccio al controllo delle versioni consente il flusso automatico dei pacchetti, mantenendo al contempo la sicurezza.

Due esempi:

```json
{
    "name": "antonevers/gra-meta-foundation",
    "type": "metapackage",
    "require": {
        "antonevers/gra-component-foundation": "1.1.4",
        "antonevers/module-gra": "1.0.0",
        "antonevers/module-3rdparty": "1.3.89"
    }
}
```

Questo esempio mostra una definizione rigorosa delle dipendenze. Sono necessari 3 pacchetti nelle versioni esatte. L’aggiornamento del Compositore con questo metapacchetto nell’installazione non esegue alcuna operazione. Installa sempre questi 3 pacchetti in queste versioni esatte, anche se è disponibile una versione più recente.

```json
{
    "name": "antonevers/gra-meta-foundation",
    "type": "metapackage",
    "require": {
        "antonevers/gra-component-foundation": "~1.0",
        "antonevers/module-gra": "~1.0",
        "antonevers/module-3rdparty": "~1.0"
    }
}
```

In questo esempio viene mostrata una definizione di dipendenze allentata. Con `~1.0` è possibile installare qualsiasi versione di questi pacchetti se sono maggiori o uguali a `1.0.0` e minori di `2.0.0`, con una preferenza per la versione più recente disponibile. Ulteriori informazioni su come definire le dipendenze delle versioni in <https://getcomposer.org/doc/articles/versions.md>:

> L&#39;operatore ~ è meglio spiegato dall&#39;esempio: `~1.2` equivale a `>=1.2 <2.0.0`, mentre `~1.2.3` equivale a `>=1.2.3 <1.3.0`.

Non appena rilasci una nuova versione di uno dei pacchetti menzionati, questa viene installata automaticamente con l’aggiornamento del Compositore.

Applicare il controllo delle versioni semantiche. È possibile apprendere tutte le informazioni sul controllo delle versioni semantiche in <https://semver.org/>. In particolare, le domande frequenti sono da leggere. Con il controllo delle versioni semantiche, i numeri in &quot;1.0.0&quot; sono denominati MAJOR.MINOR.PATCH. Le versioni secondarie e patch di un pacchetto devono essere sicure da introdurre senza interrompere l’applicazione.
È possibile includere automaticamente le patch e scegliere manualmente aggiornamenti minori. Tieni presente che in questo modo puoi sostenere costi aggiuntivi scegliendo manualmente ogni modifica minore:

```json
{
    "name": "antonevers/gra-meta-foundation",
    "type": "metapackage",
    "require": {
        "antonevers/gra-component-foundation": "~1.1.0",
        "antonevers/module-gra": "~1.0.0",
        "antonevers/module-3rdparty": "~1.3.0"
    }
}
```

Naturalmente, tutto questo funziona solo se si applica il controllo delle versioni semantiche in modo coerente, sempre. E non solo nei metapacchetti, ma anche nei requisiti dei tuoi pacchetti regolari dovrebbero definire le dipendenze in modo più vago. Se nel sistema è presente una dipendenza rigida, tale pacchetto è limitato alla definizione rigida.

Per trovare queste dipendenze rigide, digita compositore dipende \&lt;nome pacchetto\>. Per ulteriori informazioni, vedere <https://getcomposer.org/doc/03-cli.md#depends-why>.

## Strategia di diramazione

Puoi utilizzare varie strategie di ramificazione per supportare questo modello di strategia di riferimento globale, a condizione che il ramo principale sia l’unico ramo in cui vengono modificati i pacchetti. Se esegui una versione su più rami, si corre il rischio di perdere in modo casuale la funzionalità tra le versioni. Crea solo versioni stabili sul ramo principale.

Crea solo rami di funzioni negli archivi dei pacchetti. Non negli archivi di installazione del negozio. Puoi comunque introdurre qualsiasi modifica al tuo archivio semplicemente utilizzando Composer. Evita la necessità di unioni Git nell’archivio di distribuzione.

Tipi di ramo comuni nelle strategie di ramificazione e negli archivi in cui dovrebbero essere presenti:

**Rami di funzionalità**: esistono negli archivi dei pacchetti, non altrove.

**Rami della versione**: creati in qualsiasi repository: pacchetti, metapacchetti, archivi di installazione dell&#39;archivio. Quando si pianifica una versione, raggruppare le modifiche nei rami di rilascio dei pacchetti prima del controllo delle versioni. Supponiamo che tu stia preparando una versione con il nome in codice &quot;Unicorn&quot;. Puoi creare un ramo release-unicorn nei pacchetti con modifiche. Unisci qualsiasi cosa lì dentro e poi richiedere &quot;dev-release-unicorn as 1.4.0&quot; nel metapacchetto. Ulteriori informazioni sugli alias per visualizzare gli eventi: <https://getcomposer.org/doc/articles/aliases.md>.

**Rami di controllo qualità/sviluppo**: simile ai rami di rilascio.

**Ramo principale**: deve esistere in ogni archivio e deve essere sempre il ramo che rappresenta lo stato di produzione o pronto per la produzione. The main branch is where you tag code to release versions.
Make sure you choose a branching strategy with little maintenance overhead. For example, merging the main branch back into QA, UAT, release, or dev branches after a hotfix release is an overhead maintenance task. The more packages, the more repositories and the more repetitive overhead tasks.

Use a tool like mixu/gr to perform routine operations on multiple Git repositories in a batch: <https://github.com/mixu/gr>

## Convenzioni di denominazione

With the Separate Packages GRA pattern, packages are part of the GRA foundation if the foundation metapackage requires them. Add or remove packages from the metapackage to move them in and out of the foundation.

Metapackages give flexibility to the installation scope of packages. It is extra important that the names of packages do not contain any words that relate to the intended use of the package. The name antonevers/module-gra-store-locator may become confusing when you decide to take that package out of the GRA foundation. Avoid scope (GRA, foundation, local). Avoid region (EMEA, Spain, Global). Most definitely avoid the name of the store that a package is built for. Choose names that only relate to the functionality that is added in the package. That way you can reuse them everywhere you please, also in unforeseen future scenarios. The name antonevers/module-store-locator would be excellent.

Make sure that related packages show up together in overviews. Build names from generic to specific. So, antonevers/module-b2b-tax-exempt instead of antonevers/tax-exempt-module-b2b.

## Code examples

The code examples of this blog post have been combined in a set of Git repositories, which you can use to play around with the proof of concept.

* An example production store: <https://github.com/AntonEvers/gra-separate-brand-x>
* An example foundation module: <https://github.com/AntonEvers/module-example-gra>
* An example third-party module: <https://github.com/AntonEvers/module-example-3rdparty>
* An example local module: <https://github.com/AntonEvers/module-example-local>
* An example foundation metapackage: <https://github.com/AntonEvers/gra-meta-foundation>
* An example local metapackage (optional): <https://github.com/AntonEvers/gra-meta-brand-x>
* An example Composer repository: <https://github.com/AntonEvers/gra-composer-repository>
