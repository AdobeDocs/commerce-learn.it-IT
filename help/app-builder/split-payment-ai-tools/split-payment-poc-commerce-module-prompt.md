---
title: 'Split payment POC: Commerce module AI prompt'
description: 'Learn how to use this prompt to generate Client_SplitPayment: REST, plugins, checkout JavaScript, I/O events, and enable, compile, and deploy commands.'
feature: App Builder, Backend Development, Eventing, Extensibility, Paas, REST, Orders
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 503
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: beb22335cec97141b46ddbbca97d21b216c55a80
workflow-type: tm+mt
source-wordcount: '1207'
ht-degree: 1%

---

# Split payment POC: Commerce module AI prompt

Use this page to copy the full prompt that generates the `Client_SplitPayment` in-process module: REST, session handling, **[!UICONTROL Checkout]**, and **[!UICONTROL Admin]** display for the split payment proof of concept. Operator workflow stays in App Builder.

## How to use this prompt

Copy everything from **PROMPT START** to **End of prompt** into Cursor (with Claude) or directly into Claude. Run it from the root of your Commerce project or a directory where the AI can create files.

## Customize before you run

* Replace `Client` with your real vendor name.
* Change `SplitPayment` if you want a different module name.
* If the site uses a custom theme, layout XML and RequireJS paths may need changes.
* If your **[!UICONTROL Cash on delivery]** method uses a different code than `cashondelivery`, update `payment-method-helper.js`.


## The prompt

**PROMPT START**

You are generating a complete, production-ready Adobe Commerce 2.4.5+ in-process module for a split payment feature. This module is the thin PHP adapter that exposes the right REST surface and attaches the right data at the right moments in the Commerce lifecycle. All operator workflow logic lives in Adobe App Builder (not in this module).

**Module identity:**
* Vendor: `Client`
* Module: `SplitPayment`
* Full name: `Client_SplitPayment`
* Namespace: `Client\SplitPayment`
* Location: `app/code/Client/SplitPayment/`
* Dependencies: `Magento_Checkout`, `Magento_CustomerBalance`, `Magento_Sales`, `Magento_Quote`, `Magento_WebApi`, `Magento_AdobeCommerceEventsClient`

Generate every file listed in the file structure below. Do not omit any file. Use `declare(strict_types=1)` in all PHP files.


### Struttura del file da generare

```
app/code/Client/SplitPayment/
├── registration.php
├── composer.json
├── Api/
│   ├── Data/SplitPaymentInterface.php
│   └── SplitPaymentManagementInterface.php
├── Block/
│   └── Order/SplitPaymentInfo.php
├── Controller/
│   └── Checkout/StoreCreditBalance.php
├── etc/
│   ├── acl.xml
│   ├── config.xml
│   ├── db_schema.xml
│   ├── db_schema_whitelist.json
│   ├── di.xml
│   ├── events.xml
│   ├── extension_attributes.xml
│   ├── io_events.xml
│   ├── module.xml
│   ├── webapi.xml
│   └── frontend/
│       └── routes.xml
├── Model/
│   ├── SplitPaymentData.php
│   ├── SplitPaymentManagement.php
│   ├── Service/
│   │   └── SplitInvoiceService.php
│   └── Session/
│       └── SplitPaymentSession.php
├── Observer/
│   ├── AutoInvoiceStoreCreditOnOrderPlace.php
│   └── CopySplitPaymentToOrder.php
├── Plugin/
│   ├── CheckoutPlugin.php
│   ├── OrderRepositoryPlugin.php
│   ├── PlaceOrderPlugin.php
│   ├── Adminhtml/
│   │   └── OrderPaymentPlugin.php
│   ├── Checkout/
│   │   └── LayoutProcessorPlugin.php
│   ├── CustomerBalance/
│   │   └── CapCustomerBalanceCollectPlugin.php
│   ├── Payment/
│   │   ├── Block/
│   │   │   └── AdminSplitPaymentTitlePlugin.php
│   │   └── Checks/
│   │       └── SplitPaymentZeroTotalPlugin.php
│   ├── Quote/
│   │   └── FixSplitPaymentGrandTotalPlugin.php
│   └── Sales/
│       └── FixInvoiceCustomerBalanceAfterTotalsPlugin.php
├── Setup/
│   └── Patch/
│       └── Data/
│           └── AddSplitPaymentOrderAttribute.php
└── view/
    └── frontend/
        ├── requirejs-config.js
        ├── layout/
        │   └── sales_order_view.xml
        ├── templates/
        │   └── order/
        │       └── split-payment-info.phtml
        └── web/
            ├── js/
            │   ├── model/
            │   │   └── payment-method-helper.js
            │   └── view/
            │       └── payment/
            │           ├── cashondelivery-method.js
            │           └── split-payment.js
            └── template/
                └── payment/
                    ├── cashondelivery.html
                    └── split-payment.html
```


### Behavioral Specifications

#### 1. Database Schema (`etc/db_schema.xml`)

Add these columns to `sales_order` (resource: `sales`):

| Colonna | Tipo | Nullable | Comment |
|---|---|---|---|
| `split_store_credit_amount` | decimal(12,4) | yes | Store credit portion |
| `split_cash_amount` | decimal(12,4) | yes | Cash portion due |
| `split_cash_status` | varchar(32) | yes | `pending` / `received` / `declined` |
| `split_sc_invoice_id` | int unsigned | yes | Store credit invoice entity ID |
| `split_cash_invoice_id` | int unsigned | yes | Cash invoice entity ID |

Genera anche `db_schema_whitelist.json` per queste colonne.

#### &#x200B;2. Attributi di estensione (`etc/extension_attributes.xml`)

Aggiungi `split_store_credit_amount` (virgola mobile), `split_cash_amount` (virgola mobile), `split_cash_status` (stringa) a:
* `Magento\Quote\Api\Data\CartInterface`
* `Magento\Sales\Api\Data\OrderInterface`
* `Magento\Sales\Api\Data\OrderPaymentInterface`

#### &#x200B;3. Endpoint REST (`etc/webapi.xml`)

```xml
POST /V1/split-payment/set              → anonymous (session-scoped)
POST /V1/split-payment/orders/:orderId/cash-received  → Magento_Sales::actions
POST /V1/split-payment/orders/:orderId/cash-decline   → Magento_Sales::cancel
```

Tutti e tre mappano a `Client\SplitPayment\Api\SplitPaymentManagementInterface`.

#### &#x200B;4. Eventi I/O (`etc/io_events.xml`)

Iscriviti a `observer.sales_order_place_before` e includi i campi:
`entity_id`, `quote_id`, `increment_id`, `subtotal`, `split_store_credit_amount`, `split_cash_amount`, `split_cash_status`

#### &#x200B;5. `SplitPaymentSession` — Wrapper sessione

Memorizza gli importi suddivisi dichiarati nella sessione di pagamento. Deve esporre:
* `setAmounts(float $storeCredit, float $cash): void`
* `getAmounts(): array` — restituisce `['store_credit' => float, 'cash' => float]`
* `clear(): void`
* `beginBalanceApply(): void` — imposta un flag che elimina il plug-in di correzione totale complessivo durante l&#39;applicazione del credito dell&#39;archivio
* `endBalanceApply(): void`
* `isBalanceApplyInProgress(): bool`

#### &#x200B;6. `SplitPaymentManagement` - Controller REST

**`setSplitPayment(float $storeCreditAmount, float $cashAmount, ?string $cartId = null): bool`**
* Convalida l&#39;appartenenza del carrello alla sessione corrente (confrontando gli ID di virgolette numeriche e mascherate)
* Memorizza gli importi in `SplitPaymentSession`
* Restituisce `true` in caso di esito positivo; genera `LocalizedException` con un messaggio generico in caso di errore

**`markCashReceived(int $orderId): bool`**
* Carica l&#39;ordine per `entity_id`
* Convalida `split_cash_status === 'pending'`
* Imposta lo stato su `received`, lo stato su `processing`
* Aggiunge un commento di cronologia: `"Cash payment of $X.XX received."`
* Chiamate `SplitInvoiceService::createCashPortionInvoice($orderId)`
* Aggiunge un commento con l&#39;ID incremento fattura di cassa
* Chiama `createShipmentIfPossible($orderId)` — crea una spedizione utilizzando `ShipOrder::execute()` se sono presenti articoli con spedizione consentita
* Restituisce `true`; genera `LocalizedException` generico in caso di errore

**`markCashDeclined(int $orderId): bool`**
* Ordine di caricamento
* Convalida `split_cash_status === 'pending'`
* Convalida `$order->canCancel()`
* Imposta lo stato su `declined`, aggiunge il commento: `"Cash payment declined (simulated fraud check)."`
* Salva ordine
* Chiamate `OrderManagement::cancel($orderId)`
* Restituisce `true`; genera `LocalizedException` generico in caso di errore

#### &#x200B;7. `PlaceOrderPlugin` — aroundPlaceOrder su `QuoteManagement`

Questo è il plug-in più critico. Esegue `aroundPlaceOrder`:

1. Carica il preventivo. Se non è attivo, chiamare immediatamente `$proceed()`
2. Leggi `SplitPaymentSession::getAmounts()`
3. Se `customer_balance_amount_used > 0` nel preventivo, chiamare `BalanceManagementInterface::remove($cartId)` (handle `LocalizedException` — registra e rigenera come messaggio generico)
4. Chiama `recollectQuoteForBalanceOperation()` — carica le virgolette, chiama `collectTotals()`, salva (entro `beginBalanceApply()` / `endBalanceApply()` per eliminare il plug-in di correzione totale complessivo)
5. Se `$storeCredit > 0`, chiamare `BalanceManagementInterface::apply($cartId, $storeCredit)` (errore handle)
6. Ricarica offerta; imposta attributi estensione: `split_store_credit_amount`, `split_cash_amount`, `split_cash_status = 'pending'`
7. Salva `payment->additional_information['client_split_payment']` come `['store_credit' => x, 'cash' => y]`
8. Salva offerta
9. Chiama `$proceed()`, ID ordine di acquisizione
10. Chiama `SplitPaymentSession::clear()`
11. ID ordine di reso

#### &#x200B;8. `CopySplitPaymentToOrder` — Osservatore su `sales_model_service_quote_submit_before`

Legge gli importi frazionati dalla sessione → pagamento informazioni_aggiuntive → attributi di estensione del preventivo (nell&#39;ordine di priorità). Scrive `split_store_credit_amount`, `split_cash_amount`, `split_cash_status = 'pending'` nell&#39;oggetto ordine.

#### &#x200B;9. `AutoInvoiceStoreCreditOnOrderPlace` — Osservatore su `sales_order_place_after`

Dopo l&#39;inserimento dell&#39;ordine, se l&#39;ordine ha un importo di credito dell&#39;archivio (`split_store_credit_amount > 0`), chiamare `SplitInvoiceService::createStoreCreditPortionInvoice($orderId)` per fatturare immediatamente la parte di credito dell&#39;archivio.

#### 10. `SplitInvoiceService`

**`createStoreCreditPortionInvoice(int $orderId): ?int`**
Crea una fattura solo per la parte di credito del punto vendita. Imposta `customer_balance_amount` sulla fattura sull&#39;importo del credito dell&#39;archivio. Registra e salva la fattura. Restituisce l&#39;ID entità fattura o null in caso di errore (log; non rigenerare).

**`createCashPortionInvoice(int $orderId): ?int`**
Crea una fattura per la parte di cassa (le voci/gli importi degli ordini rimanenti non coperti dalla fattura SC). Registra e salva. Restituisce l’ID entità o null in caso di errore.

#### &#x200B;11. `CheckoutPlugin` — il `PaymentInformationManagement`

Plug-in su `Magento\Checkout\Model\PaymentInformationManagement::savePaymentInformationAndPlaceOrder`. Prima di procedere:
* Carica il preventivo dalla sessione di pagamento
* Ottieni la soglia configurata: `Magento\Framework\App\Config\ScopeConfigInterface::getValue('split_payment/general/threshold')`, impostazione predefinita `100`
* Se `$quote->getGrandTotal() > $threshold`, genera `LocalizedException('Payment could not be processed. Please try again or contact support.')`

#### &#x200B;12. `CapCustomerBalanceCollectPlugin` — su `Customerbalance` totale

Dopo l&#39;esecuzione della raccolta totale del saldo cliente nativo, limitare `customer_balance_amount_used` a `SplitPaymentSession::getAmounts()['store_credit']`. Questo impedisce a Commerce di applicare in eccesso il saldo cliente completo quando il cliente ha dichiarato una porzione di credito del negozio più piccola.

#### &#x200B;13. `FixSplitPaymentGrandTotalPlugin` — il `Quote\Address\Total\Grand`

Dopo la raccolta totale generale: se esiste una sessione di pagamento frazionato e `isBalanceApplyInProgress()` è false, impostare il totale complessivo dell&#39;offerta sull&#39;importo di cassa della sessione. In questo modo l’interfaccia utente per il pagamento mostra solo le scadenze in contanti.

#### &#x200B;14. `FixInvoiceCustomerBalanceAfterTotalsPlugin` — il `Sales\Model\Order\Invoice`

Dopo la raccolta dei totali della fattura, se l&#39;ordine associato della fattura ha `split_sc_invoice_id`, correggere `customer_balance_amount` nella fattura per evitare la doppia applicazione del credito di magazzino.

#### &#x200B;15. `SplitPaymentZeroTotalPlugin` — il `Payment\Model\Checks\ZeroTotal`

Consenti pagamento COD quando `SplitPaymentSession::getAmounts()['cash'] > 0`, anche se il totale complessivo del preventivo è pari a $0 (perché il credito dello store copre l&#39;intero ordine).

#### &#x200B;16. `LayoutProcessorPlugin` — il `Checkout\Block\Checkout\LayoutProcessor`

Dopo l’elaborazione del layout:
* Inject the `Client_SplitPayment/js/view/payment/split-payment` component into the `additional` children of the `cashondelivery` payment method component at `components.checkout.children.steps.children.billing-step.children.payment.children.renders.children.offline-payments.children.cashondelivery.children.additional`
* Remove the native store credit UI component (`customerBalance`, `useStoreCredit`) from the payment step — the split payment component owns store credit display/application

#### 17. `OrderRepositoryPlugin` — on `OrderRepositoryInterface`

After `get()` and `getList()`, hydrate the order&#39;s extension attributes from the flat `sales_order` columns (`split_store_credit_amount`, `split_cash_amount`, `split_cash_status`).

#### 18. `AdminSplitPaymentTitlePlugin` — on `Payment\Block\Info`

After `getTitle()` returns, if the payment method is `cashondelivery` and the order has a split payment, append `" (Split: Cash $X.XX + Store Credit $Y.YY)"` to the title.

#### 19. `OrderPaymentPlugin` — on `Sales\Block\Adminhtml\Order\Payment`

In `_beforeToHtml` or via afterToHtml, append split payment detail (cash amount, store credit amount, status) to the payment block HTML in the Commerce Admin order view.

#### 20. Storefront Store Credit Balance Controller

`Controller/Checkout/StoreCreditBalance.php` — route: `GET /splitpayment/checkout/storecreditbalance`

Returns JSON: `{"balance": float, "logged_in": bool}`. If customer is not logged in, returns `{"balance": 0, "logged_in": false}`. Reads balance from `Magento\CustomerBalance\Model\Balance`.

Register the frontend route in `etc/frontend/routes.xml` with `frontName="splitpayment"`.

#### 21. Checkout KnockoutJS Component — `split-payment.js`

A `uiComponent` that:
* Detects when the `cashondelivery` payment method is selected (`quote.paymentMethod.subscribe`)
* Loads the customer&#39;s store credit balance via `GET /splitpayment/checkout/storecreditbalance`
* Pre-fills the cash amount field with the full order total (calculated from `total_segments` excluding `grand_total` and `customerbalance` — never uses `grand_total` directly since it may be set to the cash remainder)
* As the customer changes the cash amount input: computes store credit needed = order total − cash; validates; posts to `POST /V1/split-payment/set`
* Shows validation messages for: cash > order total, insufficient store credit, not logged in
* Shows a success message when a valid split is entered: `"The remaining $X.XX will automatically be applied from your store credit."`
* Resets when another payment method is selected (posts `{storeCreditAmount: 0, cashAmount: 0}` to clear session)

#### 22. `cashondelivery-method.js`

Extends `Magento_OfflinePayments/js/view/payment/offline-payments`. Uses `payment-method-helper.js` to detect the cash method code. Registra il componente `split-payment` nell&#39;area `additional`.

#### 23. `payment-method-helper.js`

Utility che restituisce `getCashMethodCode()` — controlla `window.checkoutConfig.paymentMethods` per `cashondelivery`; se necessario, utilizza `checkmo`.

#### &#x200B;24. `cashondelivery.html` Modello

Modello COD standard ma include l&#39;area `<!-- ko foreach: getRegion('additional') -->` in modo che il componente secondario del pagamento frazionato possa eseguire il rendering.

#### &#x200B;25. `split-payment.html` Modello

Modello KnockoutJS per i campi di pagamento frazionato:
* Visualizzazione saldo credito negozio disponibile
* Input importo contante (numero, passaggio 0,01)
* Visualizzazione porzione di credito del Negozio (sola lettura)
* Applica automaticamente il messaggio di credito del negozio (visualizzato quando il frazionamento è valido e il credito del negozio è > 0)
* Messaggio di errore di convalida

#### 26. `requirejs-config.js`

Mappe:
* `Client_SplitPayment/js/view/payment/split-payment` → componente
* `Client_SplitPayment/js/view/payment/cashondelivery-method` → la sostituzione del COD
* `Client_SplitPayment/js/model/payment-method-helper` → helper

#### 27. `etc/config.xml`

Valori di configurazione di sistema predefiniti:

```xml
<split_payment>
  <general>
    <threshold>100</threshold>
    <enabled>1</enabled>
  </general>
</split_payment>
```


### Note critiche sull’implementazione

**La richiesta di credito dell&#39;archivio deve utilizzare `BalanceManagementInterface`, non la manipolazione diretta del modello.** `BalanceManagementInterface::apply()` gestisce la sessione, la convalida e il ricalcolo atomico del carrello.

**`PlaceOrderPlugin`deve utilizzare `aroundPlaceOrder` (non `beforePlaceOrder`).** Il credito dell&#39;archivio deve essere applicato mentre il carrello è ancora attivo e questo deve essere garantito prima che venga chiamato `$proceed()`.

**Il pattern del flag di sessione per `beginBalanceApply` / `endBalanceApply` è critico.** Senza di esso, `FixSplitPaymentGrandTotalPlugin` viene eseguito durante `collectTotals()` all&#39;interno dell&#39;operazione di saldo e imposta il totale complessivo sul resto del contante, causando il mancato funzionamento di `BalanceManagementInterface::apply()` o la limitazione del credito.

**Non esporre mai al cliente i dettagli relativi agli errori interni.** Tutti i `catch` blocchi che emergono dalle risposte REST devono generare `LocalizedException('Payment could not be processed. Please try again or contact support.')`.

**`entity_id`è l&#39;ID del database numerico.** Le chiamate REST da App Builder utilizzano sempre `entity_id`, non `increment_id`.

**`SplitInvoiceService`deve rilevare e registrare gli errori anziché propagarli.** L&#39;errore di creazione della fattura non deve annullare un ordine già effettuato. Registrare l&#39;errore e consentire all&#39;amministratore di gestirlo manualmente.


### Dopo la generazione dei file

Esegui questi comandi nella directory principale del progetto Commerce:

```bash
bin/magento module:enable Client_SplitPayment
bin/magento setup:upgrade
bin/magento setup:di:compile
bin/magento setup:static-content:deploy -f
bin/magento cache:flush
```

### Fine del prompt


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
