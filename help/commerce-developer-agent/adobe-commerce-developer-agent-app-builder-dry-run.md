---
title: Esecuzione a secco di Adobe Commerce Developer Agent App Builder
description: Scopri come generare, distribuire e testare tre casi di utilizzo dell’estensibilità di Commerce con Adobe Commerce Developer Agent in questa pratica esecuzione a secco di App Builder.
feature: Extensibility, App Builder, Eventing, Configuration
topic: App Builder, Development, Integrations
role: Developer
level: Intermediate
doc-type: Tutorial
duration: 438
last-substantial-update: 2026-08-28T00:00:00Z
source-git-commit: 92af5355fa31c1ce9e627679b0a1bb92cce0e1d8
workflow-type: tm+mt
source-wordcount: '1700'
ht-degree: 0%

---

# Esecuzione a secco di Adobe Commerce Developer Agent App Builder

Procedura dettagliata per la creazione, la distribuzione e il test di casi d’uso di estensibilità per Commerce con Adobe Commerce Developer Agent (CDA). Questa esecuzione a secco copre tre casi d’uso: un webhook di limite della quantità del carrello, un blocco dell’ordine di valore elevato e l’archiviazione basata su eventi per gli ordini conservati, dalla blueprint al test funzionale.

## Introduzione

### Come segnalare i problemi e fornire feedback

Durante la fase di asciugatura, si incontrano spigoli sgrossati, come previsto durante l&#39;utilizzo di una nuova feature. Acquisisci e condividi eventuali problemi con il contatto del programma Adobe utilizzando il modello di feedback fornito durante l’onboarding.

>[!TIP]
>
> Quando segnali un problema:
>
> * Includi `projectId` (visibile nell&#39;URL del browser).
> * Includi le schermate quando pertinente.

### Prerequisiti

**Account e accesso**

* Almeno il ruolo **Sviluppatore** nell&#39;organizzazione IMS ad accesso anticipato.
* Accesso amministratore a un&#39;istanza di Adobe Commerce as a Cloud Service (ACCS) in tale organizzazione, disponibile all&#39;indirizzo experience.adobe.com in **Istanze di Cloud Service**.
* Un account GitHub.

**Strumenti**

Per la convalida funzionale è necessaria una vetrina Edge Delivery Services (EDS). Sono necessari:

* Node.js 22+
* Adobe I/O CLI: `npm install -g @adobe/aio-cli`
* Plug-in Commerce AIO CLI: `aio plugins:install https://github.com/adobe-commerce/aio-cli-plugin-commerce`

Installa la boilerplate di storefront in una cartella vuota, selezionando la tua istanza ACS quando richiesto:

```bash
aio commerce extensibility app-setup -s aem-boilerplate-commerce -n storefront
```

Avvia la vetrina:

```bash
cd storefront
npm run start
```

## Apertura di Commerce Developer Agent

1. Passare a Commerce Developer Agent all&#39;indirizzo experience.adobe.com, in **Developer Agent**.
1. Effettua l’accesso utilizzando le credenziali della tua organizzazione IMS Early Access.

## Caso d’uso 1: webhook delle unità massime del carrello

Questo caso d’uso convalida i limiti di quantità del carrello prima che venga aggiunto un prodotto, utilizzando un webhook Commerce sincrono.

### Fase blueprint

Immetti il seguente prompt e fai clic su **Genera blueprint**:

```text
Add a validation webhook that runs before a product is added to the cart.

Use the Commerce webhook method observer.sales_quote_item_save_before (type before) — do not use
observer.checkout_cart_product_add_before, observer.sales_quote_add_item, or any other event.

Calculate the total by summing all quote line quantities and the quantity of the current item.
If the same SKU already exists in the quote, exclude its existing quantity to avoid double-counting.

If the total is greater than the maximum allowed, block the add and show:
"You have reached the maximum amount of items."

The maximum allowed must be configurable in Commerce Admin as max_cart_units, with default 10.

Map payload fields using name and source properties:
- name: item.qty, source: data.item.qty
- name: item.sku, source: data.item.sku
- name: quote, source: context_checkout_session.get_quote[items.qty,items.sku]

Set required: true and fallback_error_message: "You have reached the maximum amount of items."
on the webhook config.

When blocking the add, do not use exceptionOperation, because it serializes exceptionClass as class.
Instead, manually return an exception operation response whose body includes type:
{
  "op": "exception",
  "message": "You have reached the maximum amount of items.",
  "type": "\\Magento\\Framework\\GraphQl\\Exception\\GraphQlInputException"
}
```

>[!NOTE]
>
> Cerca queste cose:
>
> * Viene creata una blueprint (v1) che acquisisce i requisiti.
> * Vengono create le attività per guidare l’implementazione.

Affina il blueprint immettendo i dettagli nella casella di chat o facendo clic su una delle pillole sopra la casella di chat (*Sfida presupposti*, *Trova spazi vuoti di progettazione*, ecc.). Una volta ottenuti i risultati desiderati, fare clic su **Approva piano** per procedere.

### Fase di sviluppo

L’agente passa alla fase di sviluppo e inizia a eseguire il provisioning dell’area di lavoro.

>[!NOTE]
>
> Cercate questi file nel pannello Esplora risorse:
>
> * `app.commerce.config.ts`
> * `app.config.yaml`
> * `install.yaml`
> * `package-lock.json`
> * `package.json`

Una volta eseguito il provisioning, l’agente mostra un elenco di attività di implementazione e inizia a generare.

>[!NOTE]
>
> Cerca queste cose:
>
> * Il codice generato corrisponde ai requisiti.
> * La schermata di streaming `Validate` mostra l&#39;avanzamento della convalida dell&#39;area di lavoro (`aio app build`).
> * Se la convalida non riesce, l&#39;agente corregge automaticamente il codice generato.

Una volta completato il codice, fai clic sulla scheda **Integrazioni** per andare avanti.

### Configurare le integrazioni

**Connetti o crea un&#39;area di lavoro App Builder**

Per creare o collegare un progetto App Builder, segui le istruzioni visualizzate.

Se ti connetti a un’area di lavoro esistente, assicurati che:

* Servizio `Runtime` aggiunto.
* Sono state aggiunte le seguenti API: Adobe Commerce as a Cloud Service, I/O Management API, App Builder Data Services, I/O Events, Adobe I/O Events for Adobe Commerce.

Se crei una nuova area di lavoro, aggiungi manualmente l&#39;API **Adobe Commerce as a Cloud Service**.

>[!IMPORTANT]
>
> Una volta connesso a un progetto App Builder esistente, espandi **Configurazione avanzata** e incolla il JSON dell&#39;area di lavoro, quindi fai clic su **Ricontrolla stato** per verificare che tutte le API richieste siano installate.

Fai clic su **Avanti** per continuare.

**Connetti a Commerce**

Seleziona l&#39;istanza di ACCS dall&#39;elenco oppure immetti l&#39;URL nel campo **URL base REST Commerce**, quindi fai clic su **Connetti istanza di Commerce**. Fai clic su **Avanti** per continuare.

**Connetti a GitHub**

Connetti l’area di lavoro a un archivio GitHub immettendo l’URL dell’archivio e utilizzando l’app GitHub o un token di accesso personale. Fai clic su **Avanti** per continuare.

**Configurare le variabili di ambiente**

Inserisci le variabili di ambiente richieste dal progetto.

### Distribuisci

Fai clic su **Sviluppa** per tornare alla fase di sviluppo, quindi chiedi all&#39;agente di distribuire nel campo del prompt.

>[!NOTE]
>
> Cercare un messaggio di conferma della distribuzione che mostri lo spazio dei nomi Organizzazione, Progetto, Workspace e Runtime.

Conferma la distribuzione.

>[!NOTE]
>
> Cerca:
>
> * La schermata di streaming `Validate` mostra l&#39;avanzamento della convalida pre-distribuzione.
> * L’agente corregge autonomamente il codice in caso di errore di convalida.
> * La schermata di streaming `Deploy` mostra l&#39;avanzamento della distribuzione (`aio app deploy`).
> * L’agente corregge autonomamente il codice se la distribuzione non riesce.

### Associa l’app a Gestione app

1. Passa all’URL di amministrazione dell’istanza ACS e accedi.
1. Seleziona **App** nel menu a sinistra, quindi **Gestione app**.
1. Fare clic su **+ Associa app** (in alto a destra).
1. Seleziona il progetto e il Workspace a cui CDA ha distribuito, quindi fai clic su **Associa**.

>[!NOTE]
>
> Cerca una scheda che mostra il nome e la versione dell’applicazione e le funzionalità implementate (configurazione aziendale, webhook, eventi, ecc.).

### Installare e configurare in Gestione app

1. Nella riga dell&#39;applicazione fare clic su **Installa**, quindi su **Chiudi**.
1. Sulla stessa riga, fai clic su **Configura** per inserire i valori della configurazione aziendale, quindi su **Chiudi**.

>[!NOTE]
>
> Cerca un modulo che mostri ogni campo di configurazione specificato dalla blueprint, precompilato con i valori predefiniti specificati.

### Test funzionali

1. Nella configurazione dell&#39;app di gestione app, imposta **Unità carrello massime** su 3 (valore basso per un test rapido).
1. Nella vetrina, inizia con un carrello vuoto.
1. Aggiungere prodotti dalla pagina Dettagli prodotto (PDP) fino a quando la quantità totale supera i 3. L&#39;ultima aggiunta non riesce.
1. In PDP, viene visualizzato: *&quot;È stato raggiunto il numero massimo di elementi.&quot;*
1. Al di sotto del limite, aggiunge ancora riuscita.

>[!NOTE]
>
> Dalla pagina dell’elenco dei prodotti (PLP), un’aggiunta bloccata ha esito negativo senza alcun messaggio: si tratta di un comportamento di vetrina, non di un errore del webhook. Preferisci il PDP per la verifica.

## Caso d&#39;uso 2: blocco di ordini di valore elevato e codice di verifica

Torna alla fase **Blueprint** per iniziare questo caso d&#39;uso.

### Fase blueprint

Immetti il seguente prompt e fai clic su **Genera blueprint**:

```text
Add a Commerce event priority subscription to `plugin.sales.api.order_management.place`.

Extract `entity_id` and `grand_total` from the Commerce event payload using event `fields` in `app.commerce.config.ts`.

Important: the runtime action receives a CloudEvents-shaped payload. For Commerce eventing extracted fields,
parse them from `params.data.value`, not directly from `params.data`. The handler must use:
- `params.data.value.entity_id`
- `params.data.value.grand_total`

When `grand_total` is greater than `order_hold_threshold`:
1. Generate a verification code locally.
2. Put the order on hold with state and status `holded`.
When putting the order on hold, save the verification code using `custom_attributes`, not `extension_attributes`.
The Commerce `POST V1/orders` payload should include:
{
  "entity": {
    "entity_id": <entity_id>,
    "state": "holded",
    "status": "holded",
    "custom_attributes": [
      {
        "attribute_code": "<hold_verification_attribute>",
        "value": "<verification_code>"
      }
    ]
  }
}
3. Save the verification code via a `POST V1/orders` Commerce REST API call.

Make these configurable in Commerce Admin:
- `order_hold_threshold`, default `500`
- `hold_verification_attribute`, default `lab_verification_code`

Validate inputs before use:
- `entity_id` must be a positive integer.
- `grand_total` must be a non-negative number.
```

>[!NOTE]
>
> Cerca queste cose:
>
> * Viene creata una blueprint (v2) che acquisisce i requisiti.
> * Le attività del piano originale vengono mantenute.
> * Sono state aggiunte nuove attività corrispondenti ai nuovi requisiti.

Ridefinisci il blueprint in base alle esigenze, quindi fai clic su **Approva piano** per andare avanti.

### Sviluppo, distribuzione, associazione e installazione

Segui lo stesso processo utilizzato nel caso d’uso 1 per passare dai requisiti a un’applicazione installata, senza dover riconfigurare le integrazioni.

>[!IMPORTANT]
>
> Per apportare modifiche a un&#39;app già associata, devi **Annullare l&#39;associazione** e **Associarla** di nuovo in Gestione app.

### Test funzionali

1. Nella configurazione dell&#39;app per la gestione delle app, imposta **Soglia di blocco ordine (USD)** su 50 (facile da superare in un carrello di prova).
1. Verificare che l&#39;attributo personalizzato dell&#39;ordine esista (impostazione predefinita: `lab_verification_code`).
1. Effettua un ordine con un totale complessivo superiore a 50 dollari.
1. Attendere circa 30 secondi (gli eventi sono asincroni; la distribuzione non prioritaria può richiedere fino a ~59 secondi).
1. In Commerce Admin → Sales → Orders, aprire l&#39;ordine. Lo stato è **In attesa** (`holded`); gli attributi personalizzati includono `lab_verification_code` con un valore casuale.
1. Facoltativo: inserire prima un ordine inferiore a 50 dollari. Questo gestore non lo blocca.

## Caso d&#39;uso 3: archiviazione basata su eventi per gli ordini conservati

Torna alla fase **Blueprint** per iniziare questo caso d&#39;uso.

### Fase blueprint

Immetti il seguente prompt e fai clic su **Genera blueprint**:

```text
When an order is saved with state holded, archive it to external storage and
record a reference that can be looked up later by order ID.

Add an event priority subscription on observer.sales_order_save_after, filtered to fire only when
state equals holded. From the event payload, extract:
- `entity_id`
- `payment.amount_ordered`
- `custom_attributes` (to read the `lab_verification_code` attribute set in Step 3)

The event handler must:
1. Persist the order details to the `held_orders` App Builder DB collection:
{
  "order_id": <entity_id>,
  "grand_total": <payment.amount_ordered>,
  "verification_code": <lab_verification_code>,
  "archived_at": <ISO timestamp>
}
2. Ensure the record can be looked up later by order ID.

The `held_orders` collection must exist before the handler runs:
- Provision persistent App Builder Database Storage in region `amer`.
- Create the collection during app installation.
- Create a unique index on `order_id` during installation.
- Drop the whole `held_orders` collection when the app is uninstalled.

Register the event handler separately from the existing cart validation webhook and high-value order hold action:
- runtime action: `order-archive/archive-held-order`
- non-web action
- `include-ims-credentials: true` on the archive action and the installation action

Follow the `commerce-app-storage` skill for DB auth, installation steps, and ext.config wiring.
Do not use custom IMS credential normalization or `Core.AuthClient.generateAccessToken`.
```

>[!NOTE]
>
> Cerca queste cose:
>
> * Viene creata una blueprint (v3) che acquisisce i requisiti.
> * Le attività del piano originale vengono mantenute.
> * Sono state aggiunte nuove attività corrispondenti ai nuovi requisiti.

Ridefinisci il blueprint in base alle esigenze, quindi fai clic su **Approva piano** per andare avanti.

### Sviluppo, distribuzione, associazione e installazione

Segui lo stesso processo utilizzato nei casi d’uso precedenti per passare dai requisiti a un’applicazione installata, senza dover riconfigurare le integrazioni.

>[!IMPORTANT]
>
> Per apportare modifiche a un&#39;app già associata, devi **Annullare l&#39;associazione** e **Associarla** di nuovo in Gestione app.

### Test funzionali

1. Assicurati che la soglia del caso d’uso 2 sia sufficientemente bassa per eseguire il test (ad esempio, 50 $ in configurazione di app).
1. Posiziona un ordine oltre tale soglia in modo che il caso d’uso 2 lo metta in attesa (~30 secondi).
1. In Adobe Developer Console → il progetto → Stage → Events, apri la registrazione per l’evento di archiviazione degli ordini conservati (aggiunto o aggiornato al momento dell’installazione).
1. Conferma che un evento è stato recapitato alla registrazione dopo lo spostamento dell’ordine in attesa. Utilizzare la traccia o il monitoraggio dell&#39;evento Commerce collegato a `order-archive/archive-held-order`.

>[!NOTE]
>
> Gli eventi sono asincroni: attendi fino a ~30-59 secondi dopo che l’ordine è stato messo in attesa.

## Risoluzione dei problemi

Se l’applicazione generata da CDA non si comporta come previsto o genera errori, chiedi all’agente di risolvere i problemi dalla fase Develop.

>[!NOTE]
>
> CDA non ha alcuna visibilità sui passaggi che avvengono al di fuori di esso. L’associazione, l’installazione, la configurazione e i test funzionali vengono eseguiti tutti in Commerce Admin, App Management o nella vetrina, non in CDA. Se si verifica un problema in una di queste aree, l&#39;agente non può vederlo accadere, quindi considera quanto manca:
>
> * Cosa hai fatto e dove (ad esempio, &quot;clic su Installa in Gestione app&quot;).
> * Cosa ti aspettavi che accadesse.
> * Cos&#39;è successo?
> * Testo o messaggio di errore visualizzato sullo schermo.
> * Eventuali errori rilevanti provenienti dalla console del browser o dai registri App Builder di Adobe Developer Console e dalle tracce di debug della registrazione degli eventi.

Più concreto è il rapporto, migliore sarà la capacità dell&#39;agente di diagnosticare il problema.

## Passaggi facoltativi

**Scarica il codice**

Per continuare a perfezionare o modificare l’IDE preferito, scarica il codice generato da CDA facendo clic sull’icona Scarica sulla barra degli strumenti di Esplora risorse di Develop stage. Seleziona una cartella di destinazione e fai clic su **Salva**, quindi decomprimi il pacchetto dell&#39;area di lavoro.

>[!NOTE]
>
> Cerca:
>
> * Tutti i file visualizzati in Esplora risorse della fase di sviluppo sono presenti nella cartella decompressa.
> * Nessun errore di &quot;compilazione&quot; durante la creazione del progetto con `aio app build`.

Per utilizzare le stesse abilità agente utilizzate da CDA, installale nella cartella dei progetti:

```bash
npx skills add adobe/aio-commerce-sdk --skill commerce-app-init -y && \
npx skills add adobe/aio-commerce-sdk --skill commerce-app-eventing -y && \
npx skills add adobe/aio-commerce-sdk --skill commerce-app-webhooks -y && \
npx skills add adobe/aio-commerce-sdk --skill commerce-app-business-config -y && \
npx skills add adobe/aio-commerce-sdk --skill commerce-app-storage -y && \
npx skills add adobe/skills --skill appbuilder-project-init -y
```

Quindi avvia IDE o CLI e inizia a visualizzare la richiesta.

**Allega contesto tramite file o collegamento**

Invece di visualizzare le istruzioni direttamente nelle fasi Blueprint o Sviluppo, puoi allegare il contesto utilizzando un file di testo o un collegamento:

1. Fare clic sull&#39;icona dell&#39;allegato nella finestra di chat.
1. Fai clic su **Aggiungi file** per caricare un file di testo locale oppure immetti un URL e fai clic su **Aggiungi collegamento** per aggiungere contesto tramite un file remoto.
1. Fai clic su **Fine** e immetti un prompt per spostare l&#39;agente.

>[!NOTE]
>
> Cerca l’agente che incorpora il contesto dagli allegati nel suo turno successivo.

## Problemi noti e soluzioni alternative

**La fase blueprint non genera attività**

Per sbloccarsi e continuare, spostare l&#39;agente per generare attività.

**I pulsanti per il push e il pull da GitHub non sono funzionanti**

Scarica il file ZIP del progetto dalla fase di sviluppo.

{{$include /help/_includes/commerce-developer-agent-related-links.md}}

## Risorse aggiuntive

* [Panoramica di Commerce Developer Agent](https://developer.adobe.com/commerce/extensibility/developer-agent/)
* [Guida introduttiva di Commerce Developer Agent](https://developer.adobe.com/commerce/extensibility/developer-agent/getting-started)
* [Suggerimenti per Commerce Developer Agent](https://developer.adobe.com/commerce/extensibility/developer-agent/prompting)
* [Supporto e feedback di Commerce Developer Agent](https://developer.adobe.com/commerce/extensibility/developer-agent/support)
