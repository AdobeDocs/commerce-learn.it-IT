---
title: 'Controlli dello stato dell''inventario: sviluppo e prestazioni'
description: Scopri come valutare se sono necessari controlli di inventario in tempo reale in Adobe Commerce e rivedere le considerazioni di sviluppo e prestazioni per il tuo negozio.
feature: Best Practices, Inventory
topic: Development, Performance
role: Developer
level: Intermediate, Experienced
doc-type: Tutorial
duration: 496
last-substantial-update: 2024-05-09
jira: KT-15462
exl-id: bd2be562-5738-4398-8afb-2faeb0ba6b83
TQID: https://experienceleague.adobe.com/IfBm4JSpLXViUNTHo7amAL6GIYJsC4O-rdITtbqJV24
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: bd989d82-1e15-4534-88db-f1f51dd77ffa
    internal-label: Accounts
  - id: c1256247-af4b-46d8-9dca-0c654ecfa157
    internal-label: Order Management System
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
    internal-label: Configuration
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
    internal-label: Architecture
subfeature_v2:
  - id: b01a71b7-d17a-42b2-a9ac-af4b8d9d2ef5
    internal-label: 2FA
  - id: f56d26ed-050b-4fb7-b29b-8e6e994e80a2
    internal-label: B2B
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
    internal-label: Intermediate
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
    internal-label: Implementation
  - id: bce87dde-a4ab-44c9-8a18-ad66e4ddb377
    internal-label: Customer experience
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
    internal-label: Troubleshooting
source-git-commit: fcf0f5116ba75bd618f4ed80591ecc96132cdb45
workflow-type: tm+mt
source-wordcount: '1834'
ht-degree: 0%
---
# Lo stato dell&#39;inventario verifica le considerazioni relative allo sviluppo e alle prestazioni

L&#39;accuratezza dell&#39;inventario è un aspetto importante. Esistono alcune funzioni native che possono contribuire a garantire che questo rischio sia il più basso possibile, come gli ordini inevasi e la fissazione della soglia di scorte esaurite. Entrambi gli argomenti possono essere letti in [Adobe Experience League](https://experienceleague.adobe.com/en/docs/commerce-admin/inventory/configuration/backorders) per ulteriori spiegazioni.

Esistono progetti e casi d’uso in cui vengono richiesti controlli dello stato dell’inventario in tempo reale per un negozio Adobe Commerce. Questo tutorial fornisce ad insight la gestione di questa conversazione con considerazioni su sviluppo e prestazioni.

## Convalida se questa richiesta è necessaria

Preparati a discutere la richiesta con quante più informazioni possibili. La cosa più importante da fare è verificare che la funzionalità nativa non sia accettabile per questo progetto. Trova il motivo di questa richiesta per verificare che le funzionalità native di Adobe Commerce non soddisfino questa richiesta.

Un&#39;altra considerazione è il costo di sviluppo, test e manutenzione di questa funzione. Il parere di una parte interessata non è necessariamente un requisito. L’esecuzione della convalida dell’inventario al di fuori delle funzionalità principali di Adobe Commerce comporta dei costi associati. Questi costi sono sotto forma di debito tecnico, più test e convalida, nonché documentazione di utilizzo e documenti di supporto per la sua architettura.

## Determinare la cadenza di aggiornamento scorte accettabile

Prova a considerare i controlli di inventario e come viene eseguito in 3 approcci. Ognuna presenta vantaggi e limitazioni. Inoltre, aumentano di complessità e richiedono più test e riflessioni per la gestione degli errori. Ricorda che quando decidi di implementare una soluzione personalizzata, ci sono responsabilità e considerazioni aggiuntive. Alcuni esempi includono un processo di fallback, monitoraggio, test e risoluzione dei problemi, di competenza del team di sviluppo. Alcuni elementi importanti da includere sono la nuova documentazione di supporto, la formazione e il monitoraggio per garantire che il team di sviluppo possa supportare l’intera funzione. Un effetto collaterale è che il team di sviluppo è il proprietario del processo e non sfrutta più la funzionalità nativa fornita dall’applicazione Adobe Commerce di base. Il supporto Adobe non è in grado di fornire assistenza con questo livello di personalizzazione.

Il primo approccio consiste nell’utilizzare la funzionalità nativa. L’utilizzo della funzionalità nativa rappresenta il rischio minore e offre numerosi vantaggi. Con questo approccio, puoi fare affidamento su tutta la documentazione e i tutorial esistenti forniti da Adobe Commerce per l’utilizzo di questa funzione. La gestione dell&#39;inventario presenta molti aspetti, quindi utilizza ciò che viene fornito con l&#39;applicazione come prima considerazione. Tuttavia, in alcuni casi d’uso i dati trovati in Commerce al momento dell’ordine non sono accurati. Un esempio della mancata sincronizzazione dei dati è che le vendite sono consentite al di fuori dell’applicazione Adobe Commerce direttamente nel sistema Order Management. Un motivo è che per garantire che i livelli di inventario accurati siano rappresentati in Adobe Commerce, è necessario un qualche tipo di integrazione per mantenere le informazioni di Adobe Commerce il più vicino possibile all’accuratezza. Se la vendita in eccesso non è accettabile, aggiungere una soglia di scorte esaurite è un buon metodo per interrompere la vendita di articoli prima di arrivare a zero. La funzionalità di sincronizzazione nativa per Adobe Commerce prevede al massimo 1 volta al giorno. Questa frequenza è sufficiente per alcuni casi d’uso, ma non è sufficiente per altri. Leggi [Importazione ed esportazione pianificate](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/data-transfer/data-scheduled-import-export) per informazioni dettagliate.

Il secondo approccio è `near real-time`. Quasi in tempo reale utilizza ancora la funzionalità nativa. Tuttavia, questo include un po’ di lavoro aggiuntivo per fornire un’integrazione che alimenta frequentemente Commerce per aggiornare il suo inventario in base a una pianificazione. Ad esempio, ogni ora. Questa opzione richiede una riflessione sul funzionamento di un’integrazione, ma l’utilizzo dell’&quot;api in blocco&quot; e la disponibilità di middleware per la trasformazione dei dati e il loro invio al reparto commerce rappresentano un ottimo approccio. Prova a utilizzare Adobe App Builder o piattaforme simili per eseguire la maggior parte del lavoro e inviare le informazioni ad Adobe Commerce con maggiore frequenza.

Il terzo approccio e il più complesso con la maggiore quantità di rischi e responsabilità è costituito dai controlli di inventario in tempo reale in tempo reale effettuati su un’API o sorgente dati esterna. Effettuare un controllo dell’inventario in tempo reale su un sistema esterno è rischioso e presenta diversi altri elementi da considerare. Di seguito sono riportati alcuni altri elementi da valutare:

* Il sistema esterno può accettare richieste REST o GraphQL?
* Esistono limiti all’endpoint, ad esempio il numero X di richieste al minuto che non coincidono con il traffico del sito web?
* Cosa succede al tempo di risposta sotto carico
* Che cosa accade quando i tempi di risposta sono lunghi? Terminare automaticamente questa operazione e utilizzare un’opzione di fallback come l’inventario nativo?
* Che tipo di monitoraggio è disponibile per garantire che le richieste API rientrino nei limiti di tolleranza

## Considerazioni per la gestione dell&#39;inventario non nativa

Mantieni le personalizzazioni il più possibile non complesse.
Quanto piatta può essere l&#39;organizzazione dell&#39;inventario, è 1 SKU e la quantità totale di scorte disponibili O ci sono altri attributi che devono essere considerati.

Se le informazioni di magazzino sono abbastanza piatte, ad esempio uno SKU e la quantità totale disponibile, le opzioni per quasi in tempo reale vengono espanse. Il concetto di tempo quasi reale significa che esiste un&#39;operazione in background che raccoglie l&#39;inventario dall&#39;origine e quindi popola un motore di archiviazione da utilizzare per rispondere alla richiesta. Per questo puoi utilizzare elementi come Redis, Mongo o altri database non relazionali. Queste opzioni sono veloci e funzionano alla grande per coppie chiave/valore. Se i dati sono un po&#39; più complessi, è necessario utilizzare un database delle relazioni, all&#39;interno o all&#39;esterno dell&#39;applicazione commerce. Scaricando questo dal database di e-commerce, mantieni l’applicazione Commerce principale isolata da queste transazioni. Un&#39;altra serie di vantaggi consiste nel salvare l&#39;I/O dall&#39;applicazione commerce, CPU, RAM e altri dall&#39;uso. Per risparmiare risorse dai server applicazioni Adobe Commerce, sfrutta le nuove API per estrarre i dati dallo storage off-site. Questo processo richiede un middleware per trasformare tutti i dati. Assicurati quindi che l’applicazione chiamante possa ottenere il risultato come previsto. Utilizzando Adobe App Builder con mesh API, i dati possono essere trasformati e restituiti correttamente formattati.

L’utilizzo di Adobe App Builder con API mesh è anche un’ottima opzione quando sono presenti più sorgenti di inventario.


## Sposta logica di esecuzione fuori dal processo

Adobe Developer App Builder fornisce un framework di estensibilità unificato di terze parti per integrare e creare esperienze personalizzate per estendere le soluzioni Adobe. Adobe Commerce può utilizzare Adobe Developer App Builder. Questo approccio è un ottimo caso d’uso per estendere alcune funzionalità che normalmente si verificano nell’applicazione principale e la sposta fuori dal sito. Rimuovendo la funzionalità dall&#39;applicazione Commerce, si riduce il numero di moduli e la complessità dell&#39;applicazione Commerce. A sua volta, un numero inferiore di personalizzazioni in-process riduce la complessità di aggiornamento e manutenzione.

Per trarre ispirazione da come viene eseguita questa attività, il team di Adobe ha creato una documentazione che rappresenta una grande fonte di ispirazione e fornisce esempi di codice di lavoro. Quando un acquirente aggiunge un prodotto al carrello, un sistema di gestione dell’inventario di terze parti controlla se l’articolo è in magazzino. In caso affermativo, consenti l’aggiunta del prodotto. In caso contrario, visualizza un messaggio di errore. Per esempi di codice e ulteriori informazioni, vai a [Casi d&#39;uso del webhook](https://developer.adobe.com/commerce/extensibility/webhooks/use-cases/#add-product-to-cart).

## Quando eseguire i controlli di inventario

Quando verificare se l’inventario è ancora disponibile dipende dalle parti interessate, dall’architetto del software e da altre parti interessate. Alcuni momenti appropriati includono l’aggiunta di un elemento al carrello e l’immissione del flusso di lavoro di pagamento. Qualsiasi altro evento aggiunge un carico ai sistemi back-end quando non è necessario. Tieni presente che l’obiettivo è individuare un problema di inventario solo quando è fondamentale. Considera attentamente altri controlli che influiscono sull’obiettivo generale per i controlli dello stato dell’inventario e consentiscili solo se le parti interessate sono consapevoli del potenziale rischio di carico aggiuntivo.

## Ricercare l’origine dell’inventario

È necessaria un&#39;indagine completa della fonte di inventario esterna. Gli elementi da valutare sono le opzioni API disponibili, il supporto per GraphQL e i tempi di risposta previsti. Se l&#39;origine inventario ha una larghezza di banda di connessione limitata o non è mai stata progettata per essere utilizzata in una richiesta in tempo reale, la possibilità di utilizzo è esclusa e l&#39;architetto deve considerare invece quasi in tempo reale. Se i tempi di richiesta API superano i parametri definiti, questa opzione non è disponibile. Un esempio di questo comportamento è che le risposte API sono di 200 ms per le richieste una tantum, ma aumentano a 500-900 ms con un carico moderato. Questa situazione peggiora con un carico maggiore e impedisce la disponibilità delle chiamate di inventario live.

Assicurati di testare i tempi di risposta dell’API con richieste semplici e con un volume elevato simile al traffico previsto sul sito web live. Ricordati di testare tutte le aree dal commercio allo stesso tempo per simulare scenari reali. Se le chiamate di inventario live si verificano sulle pagine dei prodotti, nel carrello e durante il pagamento, il test di caricamento deve simulare tutte queste simultaneamente per simulare il comportamento reale del cliente.

## Opzioni di fallback

Se l’origine dell’inventario non è disponibile e è disponibile il monitoraggio, si consiglia di utilizzare la funzionalità nativa di Adobe Commerce. Tuttavia, con un monitoraggio appropriato, l’esperienza del cliente può cambiare in modo dinamico per riflettere la perdita dei controlli di inventario in tempo reale. Ciò significa che una vendita o un evento viene annullato in anticipo o rimosso dalla visualizzazione per evitare le vendite in eccesso. Discutere il piano di fallback con il proprietario del negozio in modo che tutti comprendano il processo automatico che subentra se l&#39;origine dell&#39;inventario non funziona.

## Conclusione

La decisione di effettuare controlli di inventario in tempo reale è significativa. La garanzia che il proprietario del sito web, il team di sviluppo e altri siano pienamente istruiti e consapevoli di tutti i vantaggi e le potenziali insidie incombono sul lead sviluppatore o sull’architetto. Fornendo un piano accurato che copra i motivi e un processo di fallback è fondamentale per il successo.

I controlli live dell’inventario possono essere effettuati, ma richiedono ricerche e riflessioni su test e convalida durante il ciclo di controllo qualità. Assicurati che il test di carico e i test automatizzati end-to-end aiutino a garantire che tutti i potenziali problemi vengano rilevati e valutati.

Se il monitoraggio rileva chiamate non riuscite o tempi di risposta lenti, esegui azioni per mantenere il sito online e ridurre al minimo l’irritazione dei clienti. Le opzioni di fallback spaziano dall’utilizzo della funzionalità nativa alla disattivazione delle promozioni, alla notifica al team di sviluppo o al reinstradamento delle richieste a un sistema di back-end secondario. Il modo in cui il meccanismo di fallback viene implementato deve essere pianificato con la stessa attenzione con cui viene pianificata l’integrazione effettiva, perché ad un certo punto ogni sistema sperimenta dei problemi. Tutto ciò che è automatizzato o richiede un&#39;azione manuale deve essere chiaramente documentato.
