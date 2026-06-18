---
title: 'Dividi POC pagamento: prerequisiti e impostazione dell''ambiente'
description: Scopri come impostare Commerce, Admin for COD e memorizzare il credito, l’integrazione OAuth, gli eventi I/O, App Builder e CLI aio prima che venga richiesto di creare la suddivisione dei pagamenti.
feature: App Builder, Configuration, Eventing, Extensibility, Paas, Payments, REST
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, User
level: Intermediate
doc-type: Tutorial
duration: 262
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 63ac13d8c5a97ee81dcdd1f3785a9875aaf2a4db
workflow-type: tm+mt
source-wordcount: '741'
ht-degree: 1%

---

# Dividi POC pagamento: prerequisiti e impostazione dell&#39;ambiente

Completa ogni passaggio di questa esercitazione prima di eseguire qualsiasi prompt di generazione. L’assenza di un singolo passaggio è il motivo più comune per cui il flusso interrompe l’esercitazione intermedia.

## &#x200B;1. Requisiti di Adobe Commerce

* Adobe Commerce **2.4.5 o versione successiva** (on-premise o Commerce Cloud)
* Accesso Git al progetto Commerce (è stato aggiunto un modulo in `app/code/`)
* Accesso a **[!UICONTROL Commerce Admin]**

### Solo Commerce Cloud: abilita evento di I/O

Aggiungi quanto segue a `.magento.env.yaml` e distribuiscilo prima di aggiungere il modulo:

```yaml
stage:
  global:
    ENABLE_EVENTING: true
```

> **Avviso:** la distribuzione deve essere completata prima che la dipendenza del modulo I/O Event possa essere risolta.


## &#x200B;2. Configurazione amministratore Commerce

Segui questi passaggi prima di tutto. Il JavaScript di pagamento dipende da corrispondenze di stringhe esatte.

### 2 bis. Abilita contrassegno con titolo esatto

**[!UICONTROL Stores]** > **[!UICONTROL Configuration]** > **[!UICONTROL Sales]** > **[!UICONTROL Payment Methods]** > **[!UICONTROL Cash On Delivery Payment]**

* **[!UICONTROL Enabled]**: **Sì**
* **[!UICONTROL Title]**: **`Cash`** (questa stringa esatta corrisponde a quella del JavaScript di estrazione)

> **Nota:** se il tuo Negozio utilizza un&#39;implementazione o un titolo contrassegno diverso, regola `payment-method-helper.js` nel modulo Commerce.

### 2 ter. Abilita credito negozio

**[!UICONTROL Stores]** > **[!UICONTROL Configuration]** > **[!UICONTROL Customers]** > **[!UICONTROL Customer Configuration]** > **[!UICONTROL Store Credit Options]**

* **[!UICONTROL Enable Store Credit Functionality]**: **Sì**

### 2 quater. Aggiungi il credito del punto vendita al cliente di prova

**[!UICONTROL Customers]** > **[!UICONTROL All Customers]** > *[il tuo cliente di prova]* > **[!UICONTROL Store Credit]** > **[!UICONTROL Update Balance]**

Aggiungi almeno **$50** nel credito dell&#39;archivio. Verrai testato con un ordine inferiore a 100 $ in totale.

### 2d. Creare l’integrazione con Commerce

**[!UICONTROL System]** > **[!UICONTROL Integrations]** > **[!UICONTROL Add New Integration]**

* **[!UICONTROL Name]**: `Split Payment App Builder` (o un nome qualsiasi)
* Nella scheda **[!UICONTROL API]**, concedi almeno:
   * `Magento_Sales::actions` (obbligatorio per l&#39;endpoint `cash-received`)
   * `Magento_Sales::cancel` (obbligatorio per l&#39;endpoint `cash-decline`)
   * Gestione dei preventivi o del carrello (completo o relativo sottoinsieme)
   * **[!UICONTROL Customer balance]** (completo o un sottoinsieme rilevante)

Selezionare **[!UICONTROL Save]**, quindi **[!UICONTROL Activate]**.

**Copia tutti e quattro i valori; vengono visualizzati una sola volta:**

| Valore | Variabile di ambiente |
| --- | --- |
| Chiave consumer | `COMMERCE_CONSUMER_KEY` |
| Segreto consumer | `COMMERCE_CONSUMER_SECRET` |
| Token di accesso | `COMMERCE_ACCESS_TOKEN` |
| Segreto token di accesso | `COMMERCE_ACCESS_TOKEN_SECRET` |

Memorizza questi valori in modo sicuro. Sono necessari in ogni file di App Builder `.env`.


## &#x200B;3. ADOBE DEVELOPER CONSOLE e APP BUILDER

* Accesso a un’organizzazione Adobe Developer Console
* Un **progetto App Builder** (nuovo o riutilizzato)
* Un&#39;area di lavoro configurata; i prompt presuppongono **[!UICONTROL Stage]**
* **[!UICONTROL Adobe I/O Events]** aggiunto come servizio all&#39;area di lavoro
* **Commerce** connesso come provider di eventi per l&#39;area di lavoro

Il codice evento utilizzato nella prova di concetto è: `com.adobe.commerce.observer.sales_order_place_before`

Se non hai connesso Commerce come provider di eventi, consulta [Configurare Commerce per emettere eventi in Adobe I/O](https://developer.adobe.com/commerce/extensibility/events/configure-commerce/){target="_blank"} nella guida Commerce Extensibility.


## &#x200B;4. Ambiente di sviluppo locale

```bash
# Required versions
node --version    # 18.x or later
npm --version     # any recent version

# Adobe I/O CLI
npm install -g @adobe/aio-cli

# Authenticate
aio login

# Select your project and workspace
aio app use
# Confirm the org, project, and workspace shown are correct
```


## &#x200B;5. Due directory di progetto da conoscere

Questa esercitazione utilizza due directory principali separate. Tenetele separate.

**Directory principale del progetto Commerce** (archivio Git Magento):

```text
<commerce-root>/
└── app/code/Client/SplitPayment/   ← Module goes here
```

**Directory principale progetto App Builder** (la cartella `split-payment-orchestrator` dal pacchetto di origine o un nuovo progetto creato):

```text
split-payment-orchestrator/
├── app.config.yaml
├── package.json
├── .env                            ← Copy from .env.example, then fill in
└── actions/
```


## &#x200B;6. entity_id confrontato con increment_id

> **Utilizzare sempre `entity_id` (l&#39;ID di database numerico) e non `increment_id` (ad esempio `000000042`) nelle chiamate REST.**

Trova `entity_id` dall&#39;URL ordine **[!UICONTROL Commerce Admin]**:

```text
/admin/sales/order/view/order_id/42/   →   entity_id = 42
```

Oppure dallo script di simulazione:

```bash
node scripts/simulate-split-payment.mjs show 42
```


## &#x200B;7. La soglia di $100

L&#39;interfaccia utente per il pagamento frazionato e gli ordini target di soglia guardia **$100.00 o meno**. Il flusso si comporta come segue:

* **Al prezzo di $100 o inferiore:** viene visualizzata l&#39;interfaccia utente per il pagamento frazionato; il cliente può impostare un frazionamento del credito in contanti e del negozio
* **Al di sopra di $100:** `CheckoutPlugin` genera un errore al passaggio di pagamento

Per eseguire il test, crea un carrello con subtotale, spese di spedizione e imposte pari a **o inferiori a $100** (ad esempio, un prodotto inferiore a $90 in modo che la spedizione e le imposte rientrino ancora nel limite).

La soglia viene memorizzata in:

* Configurazione Commerce: `split_payment/general/threshold` (impostazione predefinita `100` in `etc/config.xml`)
* Ambiente App Builder: `PAYMENT_THRESHOLD=100` (deve corrispondere a Commerce)


## &#x200B;8. Fastly (solo Commerce Cloud)

Le azioni di App Builder richiamano Commerce su REST (`/rest/V1/split-payment/orders/...`). Se il progetto Commerce Cloud utilizza Fastly con la inserisce nell&#39;elenco Consentiti di IP in uscita, è necessario inserire nell&#39;elenco Consentiti gli indirizzi IP in uscita del runtime App Builder.

**Come verificare:** Eseguire prima lo script di simulazione (curl diretto con firma OAuth). Se funziona ma l&#39;azione App Builder restituisce `403`, è probabile che Fastly blocchi la richiesta.

Utilizza la documentazione corrente di Adobe per gli intervalli IP in uscita da App Builder e aggiungili alla configurazione Fastly in base alle esigenze.


## Elenco di controllo per la verifica

Prima di avviare le richieste di generazione, verifica quanto segue:

* [ ] versione di Commerce è 2.4.5 o successiva
* [ ] evento di I/O abilitato (Commerce Cloud) e distribuito
* [ ] Il contrassegno è abilitato con il titolo impostato esattamente su `Cash`
* [ Il credito dell&#39;archivio ] è abilitato
* [ ] Il cliente di prova ha un credito di almeno $50
* [ ] L&#39;integrazione Commerce viene creata e attivata e tutti e quattro i valori OAuth vengono salvati
* [ ] Nel progetto App Builder sono configurati il servizio Eventi di I/O e il provider di eventi Commerce
* [ ] `aio login` è completato e l&#39;area di lavoro corretta è selezionata con `aio app use`
* [ ] Node.js 18 o versione successiva è installato e `aio` CLI è installato
* [ ] `.env` file preparati per [Dividi POC pagamento: riferimento variabili di ambiente](./env-reference.md) (e pacchetto di origine, se utilizzato)


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
