---
title: Scopri le opzioni di importazione del catalogo native con Adobe Commerce
description: Scopri alcune delle opzioni native per l’importazione del catalogo nel tuo store Adobe Commerce.
kt: 13634
doc-type: tutorial
duration: 211
audience: all
activity: use
last-substantial-update: 2023-8-15
feature: Backend Development, Data Import/Export, REST
topic: Commerce, Administration, Content Management
role: Admin, User
level: Beginner, Intermediate
exl-id: 18713a44-df39-4b94-91ce-c7efeb4ce2b3
TQID: 'https://experienceleague.adobe.com/-JG7blrxImSXjA2DP9soZfsicISW0hkP2zJeWdMpVBU'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: bd989d82-1e15-4534-88db-f1f51dd77ffa
    internal-label: Accounts
  - id: c18ed297-2187-4aec-affb-9d9654eca6fc
    internal-label: Catalog management
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
    internal-label: Configuration
  - id: b48dbafb-4193-5648-b9d9-bf96e9c9a411
    internal-label: Backend Development
  - id: 601e4abe-d9bf-58de-a779-32ed6794dcbe
    internal-label: Data Import/Export
  - id: c4f010fa-1478-4300-a88d-706fbc036a7a
    internal-label: APIs and SDKs
subfeature_v2:
  - id: e0ca0e7a-9738-48d1-b98b-615468ab4aaf
    internal-label: REST API
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
    internal-label: User
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
    internal-label: Intermediate
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
    internal-label: Beginner
topic_v2:
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
    internal-label: Administration
source-git-commit: 7516911fc14bc80f327310b99a00f9cbff7c65c7
workflow-type: tm+mt
source-wordcount: '907'
ht-degree: 0%
---
# Opzioni per importare un catalogo

Esistono alcuni metodi nativi per importare un catalogo in Adobe Commerce. Ogni metodo ha un proprio ragionamento per l&#39;utilizzo insieme a pro e contro che devono essere presi in considerazione.

Scegli una delle opzioni seguenti per ulteriori informazioni.

>[!BEGINTABS]

>[!TAB Manuale]

## Creazione manuale dei prodotti {#manual-import}

Se disponi di un catalogo limitato e gli aggiornamenti non sono frequenti, la soluzione migliore potrebbe essere la creazione manuale. Richiede tempo per inserire ogni prodotto e formazione limitata su come utilizzare Commerce Admin. La gestione manuale dei cataloghi non è l&#39;opzione giusta per la maggior parte dei negozi, ma in alcune situazioni può avere senso. Per visualizzare la documentazione aggiuntiva per questo processo, visita [Crea un prodotto](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/product-create.html){target="_blank"}. Non dimenticare che puoi utilizzare più di un metodo per gestire il catalogo; tuttavia, una volta utilizzata l’automazione, le modifiche manuali devono essere limitate. Gli aggiornamenti automatizzati hanno la possibilità di sovrascrivere eventuali modifiche eseguite manualmente e causare quindi confusione. Quando l&#39;integrazione con Adobe Commerce per gestire il catalogo utilizza l&#39;automazione e le API, si consiglia di limitare la gestione del catalogo da parte dell&#39;amministratore tramite [ruoli utente e autorizzazioni](https://experienceleague.adobe.com/docs/commerce-admin/systems/user-accounts/permissions-user-roles.html){target="_blank"}.

### Quando considerare questo approccio

* Catalogo molto piccolo, ad esempio meno di 50 prodotti
* Gli aggiornamenti non sono frequenti
* Disponi di tutti i dettagli del prodotto, immagini e video e non vuoi perdere tempo per scoprire come convertire i dati in CSV
* Desideri includere l’aggiunta di immagini e video durante la creazione dei prodotti
* Il tuo team `not` ha familiarità con le API e come funziona OAUTH

>[!TAB CSV amministratore]

## Admin CSV import tool {#admin-csv}

Questo strumento consente al proprietario di un negozio di importare un catalogo utilizzando un file CSV creato dall’amministratore di commerce.
[Importa dati da Commerce Admin](https://experienceleague.adobe.com/docs/commerce-admin/systems/data-transfer/import/data-import.html){target="_blank"}

Pro:
Il caricamento di un file CSV dall’amministratore è un approccio diretto alla gestione del catalogo. Consente aggiornamenti più rapidi dei prodotti del catalogo a un catalogo di dimensioni moderate.

Contro:

* Lento
* Dimensioni massime del file di caricamento definite sul server e potrebbero non essere facilmente regolabili dal proprietario di un archivio.
* Richiede l’accesso come amministratore e qualcuno per eseguire l’azione, l’automazione è limitata
* Le importazioni programmate sono limitate a 1 volta al giorno
* Le immagini e i video associati devono essere caricati separatamente

### Quando considerare questo approccio

* Dimensione catalogo moderata
* Gli aggiornamenti non sono più di una volta al giorno
* puoi accedere in parte alle configurazioni del server nel caso in cui sia necessario aumentare la dimensione massima di caricamento file
* Il tuo team `not` ha familiarità con le API e come funziona OAUTH

>[!TAB API REST in blocco]

## API REST in blocco {#bulk-rest-api}

L’API REST in blocco consente l’automazione e aggiornamenti più frequenti. Questa API è più veloce che utilizzare il caricamento CSV da parte dell’amministratore.
[Documentazione degli endpoint bulk](https://developer.adobe.com/commerce/webapi/rest/use-rest/bulk-endpoints/){target="_blank"}

Pro:
Possibilità di importare set di dati di grandi dimensioni non in formato CSV.

Contro:

* Le immagini e i video associati devono essere caricati separatamente
* Può essere limitata dai vincoli di larghezza di banda del provider di hosting

### Quando considerare questo approccio

* Il catalogo è di qualsiasi dimensione
* Gli aggiornamenti sono frequenti; è accettabile più di 1 volta al giorno
* Il tempo di importazione è importante ma non critico ed è accettabile un breve ritardo nell’elaborazione dei dati di importazione
* I dati non sono strutturati in formato CSV e non è possibile trasformarli utilizzando l’automazione

>[!TAB API REST ASINCRONA]

## API REST ASINCRONA {#async-rest-api}

Un endpoint web asincrono intercetta i messaggi in un’API web e li scrive nella coda dei messaggi. Ogni volta che il sistema accetta una tale richiesta API, genera un identificatore UUID. Adobe Commerce include questo UUID quando aggiunge il messaggio alla coda. Quindi, un consumatore legge i messaggi dalla coda ed esegue i messaggi singolarmente.
[Documentazione degli endpoint web asincroni](https://developer.adobe.com/commerce/webapi/rest/use-rest/asynchronous-web-endpoints/){target="_blank"}

Pro:

* Importare rapidamente i dati
* L&#39;ambito dell&#39;archivio è supportato oppure è possibile specificare `all` per eseguire l&#39;operazione su tutti gli archivi esistenti

Contro:

* Richiesta GET non supportata

### Quando considerare questo approccio

* Le importazioni sono frequenti
* Nessun problema con un ritardo ridotto dal momento in cui vengono inviati tramite API e quindi elaborati dalla coda dei messaggi.


>[!TAB API REST CSV]

## API REST CSV {#csv-rest-api}

Questa opzione API consente importazioni estremamente veloci rispetto a tutte le altre opzioni native.

[Importa dati REST CSV api](https://developer.adobe.com/commerce/webapi/rest/modules/import/){target="_blank"}
Pro:

* Metodo più veloce per elaborare i dati in arrivo
* Può essere eseguito più volte al giorno
* I dati possono essere compressi utilizzando gzip per le richieste di grandi dimensioni, in modo da evitare i limiti di dimensione delle richieste HTTP.

Contro:

* Le immagini e i video associati devono essere caricati separatamente
* I dati devono essere in formato CSV

### Quando considerare questo approccio

* Il catalogo è di qualsiasi dimensione
* Gli aggiornamenti sono frequenti; è accettabile più di 1 volta al giorno
* Il tempo totale di importazione è importante
* I dati sono già in formato CSV o possono essere facilmente trasformati tramite automazione

>[!ENDTABS]

## Risorse aggiuntive

* [Importare dati utilizzando un nuovo CSV REST](https://developer.adobe.com/commerce/webapi/rest/modules/import/){target="_blank"}
* [Documentazione principale sui dati di importazione](https://experienceleague.adobe.com/docs/commerce-admin/systems/data-transfer/import/data-import.html){target="_blank"}
* [Note sulla versione di Adobe Commerce 2.4.6](https://experienceleague.adobe.com/docs/commerce-operations/release/notes/adobe-commerce/2-4-6.html){target="_blank"}
* [Utenti, ruoli e autorizzazioni](../site-management/users-roles-permissions.md){target="_blank"}
