---
title: 'Dividi POC pagamento: prerequisiti e impostazione dell''ambiente'
description: Scopri come impostare Commerce, Admin for COD e memorizzare il credito, l’integrazione OAuth, gli eventi I/O, App Builder e CLI aio prima che venga richiesto di creare la suddivisione dei pagamenti.
feature: App Builder, Configuration, Eventing, Extensibility, Paas, Payments, REST
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 262
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 1e2c7e0e6d0f2d174b88406ce3fb7c787676ecee
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

### 2 quater. Add store credit to your test customer

**[!UICONTROL Customers]** > **[!UICONTROL All Customers]** > *[your test customer]* > **[!UICONTROL Store Credit]** > **[!UICONTROL Update Balance]**

Add at least **$50** in store credit. You will test with an order under $100 total.

### 2d. Create the Commerce integration

**[!UICONTROL System]** > **[!UICONTROL Integrations]** > **[!UICONTROL Add New Integration]**

* **[!UICONTROL Name]**: `Split Payment App Builder` (or any name you prefer)
* On the **[!UICONTROL API]** tab, grant at minimum:
   * `Magento_Sales::actions` (required for the `cash-received` endpoint)
   * `Magento_Sales::cancel` (required for the `cash-decline` endpoint)
   * Quote or cart management (full or a relevant subset)
   * **[!UICONTROL Customer balance]** (full or a relevant subset)

Select **[!UICONTROL Save]**, then **[!UICONTROL Activate]**.

**Copy all four values; they are only shown once:**

| Valore | Variabile di ambiente |
| --- | --- |
| Consumer Key | `COMMERCE_CONSUMER_KEY` |
| Consumer Secret | `COMMERCE_CONSUMER_SECRET` |
| Token di accesso | `COMMERCE_ACCESS_TOKEN` |
| Access Token Secret | `COMMERCE_ACCESS_TOKEN_SECRET` |

Store these values securely. You need them in every App Builder `.env` file.


## 3. Adobe Developer Console and App Builder

* Access to an Adobe Developer Console organization
* An **App Builder project** (new or one you reuse)
* A workspace configured; the prompts assume **[!UICONTROL Stage]**
* **[!UICONTROL Adobe I/O Events]** added as a service to the workspace
* **Commerce** connected as an event provider for the workspace

The event code used in the proof of concept is: `com.adobe.commerce.observer.sales_order_place_before`

If you have not connected Commerce as an event provider, see [Configure Commerce to emit events to Adobe I/O](https://developer.adobe.com/commerce/extensibility/events/configure-commerce/){target="_blank"} in the Commerce Extensibility guide.


## 4. Local development environment

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


## 5. Two project directories to know

This tutorial uses two separate directory roots. Keep them separate.

**Commerce project root** (your Magento git repository):

```text
<commerce-root>/
└── app/code/Client/SplitPayment/   ← Module goes here
```

**App Builder project root** (the `split-payment-orchestrator` folder from the source package, or a new project you create):

```text
split-payment-orchestrator/
├── app.config.yaml
├── package.json
├── .env                            ← Copy from .env.example, then fill in
└── actions/
```


## 6. entity_id compared to increment_id

> **Always use `entity_id` (the numeric database ID), not `increment_id` (for example `000000042`), in REST calls.**

Find `entity_id` from the **[!UICONTROL Commerce Admin]** order URL:

```text
/admin/sales/order/view/order_id/42/   →   entity_id = 42
```

Or from the simulation script:

```bash
node scripts/simulate-split-payment.mjs show 42
```


## 7. The $100 threshold

The split payment UI and threshold guard target orders **$100.00 or less**. The flow behaves as follows:

* **At or below $100:** the split payment UI appears; the customer can set a cash plus store credit split
* **Above $100:** `CheckoutPlugin` throws an error at the payment step

To test, build a cart whose subtotal, shipping, and tax are **less than or equal to $100** (for example, a product under $90 so shipping and tax still fit under the cap).

The threshold is stored in:

* Commerce config: `split_payment/general/threshold` (default `100` in `etc/config.xml`)
* App Builder environment: `PAYMENT_THRESHOLD=100` (must match Commerce)


## 8. Fastly (Commerce Cloud only)

Le azioni di App Builder richiamano Commerce su REST (`/rest/V1/split-payment/orders/...`). Se il progetto Commerce Cloud utilizza Fastly con la inserire nell&#39;elenco Consentiti di IP in uscita, è necessario inserire nell&#39;elenco Consentiti gli indirizzi IP in uscita del runtime App Builder.

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
* [ ] `.env` file preparati per [Dividi POC pagamento: riferimento variabili di ambiente](split-payment-poc-env-reference.md) (e pacchetto di origine, se utilizzato)


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
