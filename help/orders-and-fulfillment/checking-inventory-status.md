---
title: Lo stato dell'inventario verifica le considerazioni relative allo sviluppo e alle prestazioni
description: Scopri alcune idee e considerazioni sull’esecuzione dei controlli dello stato dell’inventario per Adobe Commerce.
feature: Best Practices, Inventory
topic: Development, Performance
old-role: Architect, Developer
role: Developer
level: Intermediate, Experienced
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-05-09T00:00:00Z
jira: KT-15462
exl-id: bd2be562-5738-4398-8afb-2faeb0ba6b83
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '1932'
ht-degree: 0%

---

# Lo stato dell&#39;inventario verifica le considerazioni relative allo sviluppo e alle prestazioni

La precisione con l&#39;inventario è una considerazione molto importante. Esistono alcune funzioni native che possono contribuire a garantire che questo rischio sia il più basso possibile, come gli ordini inevasi e la fissazione della soglia di scorte esaurite. Entrambi gli argomenti possono essere letti in [Experience League](https://experienceleague.adobe.com/en/docs/commerce-admin/inventory/configuration/backorders) per ulteriori spiegazioni.

Esistono progetti e casi d’uso in cui vengono richiesti controlli dello stato dell’inventario in tempo reale per un negozio Adobe Commerce. Questo tutorial fornisce ad insight la gestione di questa conversazione con considerazioni su sviluppo e prestazioni.

## Convalida se questa richiesta è assolutamente necessaria

Preparati a interagire con la conversazione con quante più informazioni possibili. La cosa più importante da fare è verificare che la funzionalità nativa non sia accettabile per questo progetto. Trova il motivo di questa richiesta per verificare che le funzionalità native di Adobe Commerce non soddisfino questa richiesta.

Un&#39;altra considerazione è il costo di sviluppo, test e manutenzione di questa funzione. Solo perché alcune parti interessate ritengono che sia necessario, non significa che sia un requisito. L’esecuzione della convalida dell’inventario oltre le funzionalità principali di Adobe Commerce comporta dei costi. Questi costi sono legati al debito tecnico, a ulteriori test e convalide, nonché alla documentazione di utilizzo e ai documenti di supporto per la sua architettura.

## Determinare la cadenza di aggiornamento scorte accettabile

Prova a considerare i controlli di inventario e come viene eseguito in 3 approcci. Ognuna presenta vantaggi e limitazioni. Inoltre, aumentano di complessità e richiedono più test e riflessioni per la gestione degli errori. Ricorda che quando decidi di seguire un percorso personalizzato, ci sono responsabilità e considerazioni aggiuntive. Alcuni esempi includono un processo di fallback; il team di sviluppo è responsabile del monitoraggio, del test e della risoluzione dei problemi. Alcuni elementi importanti da includere sono la nuova documentazione di supporto, la formazione e il monitoraggio per garantire che il team di sviluppo possa supportare l’intera funzione. Un ulteriore effetto collaterale è che il team di sviluppo è completamente responsabile del processo e non sfrutta più la funzionalità nativa fornita dall’applicazione Adobe Commerce di base. Il supporto Adobe non è in grado di fornire assistenza con questo livello di personalizzazione.

Il primo approccio consiste nell’utilizzare la funzionalità nativa. L’utilizzo della funzionalità nativa rappresenta il rischio minore e offre numerosi vantaggi. Con questo approccio, puoi fare affidamento su tutta la documentazione e i tutorial esistenti forniti da Adobe Commerce per l’utilizzo di questa funzione. Ci sono molti aspetti nella gestione dell&#39;inventario, quindi utilizzare ciò che viene con l&#39;applicazione dovrebbe essere la prima considerazione. Tuttavia, vi sono casi di utilizzo in cui i dati trovati in Commerce al momento dell’ordine potrebbero non essere completamente accurati. Un esempio di come le cose possono uscire dalla sincronizzazione è che le vendite sono consentite al di fuori dell&#39;applicazione Adobe Commerce direttamente nel sistema di gestione degli ordini. Un motivo è che per garantire che i livelli di inventario accurati siano rappresentati in Adobe Commerce, sarebbe necessario un qualche tipo di integrazione per mantenere le informazioni di Adobe Commerce il più vicino possibile all’accuratezza. Se la vendita in eccesso non è accettabile, aggiungere una soglia di scorte esaurite è un buon metodo per interrompere la vendita di articoli prima di arrivare a zero. La funzionalità di sincronizzazione nativa per Adobe Commerce prevede al massimo 1 volta al giorno. Questo è sufficiente per alcuni casi d’uso, ma potrebbe non essere abbastanza frequente per altri. Per ulteriori informazioni, leggere [Importazione ed esportazione pianificate](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/data-transfer/data-scheduled-import-export).

Il secondo approccio sarebbe `near real-time`. Quasi in tempo reale utilizza ancora la funzionalità nativa. Tuttavia, questo include un po’ di lavoro aggiuntivo per fornire un’integrazione che alimenta frequentemente Commerce per aggiornare il suo inventario in base a una pianificazione. Ad esempio, ogni ora. Questa opzione richiede alcune riflessioni su come può funzionare un’integrazione, ma utilizzare l’api in blocco e avere un po’ di middleware per trasformare i dati e inviarli a commerce è un ottimo approccio. Prova a utilizzare Adobe App Builder o piattaforme simili per eseguire la maggior parte del lavoro e inviare le informazioni ad Adobe Commerce con maggiore frequenza.

Il terzo approccio e il più complesso con la maggiore quantità di rischi e responsabilità è costituito dai controlli di inventario in tempo reale in tempo reale effettuati su un’API o sorgente dati esterna. Effettuare un controllo dell’inventario in tempo reale su un sistema esterno è rischioso e presenta diversi altri elementi da considerare. Di seguito sono riportati solo alcuni altri aspetti da valutare:

* Il sistema esterno può accettare richieste REST o GraphQL?
* esistono limiti all’endpoint, ad esempio il numero X di richieste al minuto, che potrebbero non coincidere con il traffico del sito web
* Cosa succede al tempo di risposta sotto carico
* Che cosa accade quando i tempi di risposta sono lunghi? Terminare automaticamente questa operazione e utilizzare un’opzione di fallback come l’inventario nativo?
* Che tipo di monitoraggio è disponibile per garantire che le richieste API rientrino nei limiti di tolleranza

## Considerazioni quando si considera la gestione dell’inventario non nativa

Mantieni le personalizzazioni il più possibile non complesse.
Quanto piatta può essere l&#39;organizzazione dell&#39;inventario, è solo 1 SKU e la quantità totale di scorte disponibili O ci sono altri attributi che devono essere considerati.

Se le informazioni di magazzino sono abbastanza piatte, ad esempio uno SKU e la quantità totale disponibile, le opzioni per quasi in tempo reale vengono espanse. Il concetto di tempo quasi reale significa che esiste un&#39;operazione in background che raccoglie l&#39;inventario dall&#39;origine e quindi popola un motore di archiviazione da utilizzare per rispondere alla richiesta. Per questo puoi utilizzare elementi come Redis, Mongo o altri database non relazionali. Queste opzioni sono interessanti perché sono molto veloci e funzionano alla grande per coppie chiave/valore. Se i dati sono un po&#39; più complessi, probabilmente sarà necessario utilizzare un database delle relazioni, all&#39;interno o all&#39;esterno dell&#39;applicazione commerce. Scaricando questo dal database di e-commerce, mantieni l’applicazione Commerce principale isolata da queste transazioni. Un&#39;altra serie di vantaggi consiste nel salvare l&#39;I/O dall&#39;applicazione commerce, CPU, RAM e altri dall&#39;uso. Invece di utilizzare le risorse dei server applicazioni Adobe Commerce, sfrutta le nuove API per estrarre i dati dallo storage off-site.  Probabilmente sarà necessario un middleware per trasformare tutti i dati. Assicurati quindi che l’applicazione chiamante possa ottenere il risultato come previsto. Utilizzando Adobe App Builder con mesh API, i dati possono essere trasformati e restituiti correttamente formattati.

L’utilizzo di Adobe App Builder con API mesh è anche un’ottima opzione quando sono presenti più sorgenti di inventario.


## Spostamento della logica di esecuzione in una posizione fuori processo

Adobe Developer App Builder fornisce un framework di estensibilità unificato di terze parti per integrare e creare esperienze personalizzate per estendere le soluzioni Adobe. Adobe Commerce può utilizzare Adobe Developer App Builder. Questo sarebbe un ottimo caso d’uso per estendere alcune funzionalità che normalmente si verificano nell’applicazione principale e la sposta all’esterno del sito. Rimuovendo la funzionalità dall&#39;applicazione Commerce, si riduce il numero di moduli e la complessità dell&#39;applicazione Commerce. A sua volta, un numero inferiore di personalizzazioni in-process riduce la complessità di aggiornamento e manutenzione.

Per trovare l’ispirazione necessaria per ottenere questo risultato, il team di Adobe ha creato una documentazione che può essere un’ottima fonte di ispirazione e fornire esempi di codice di lavoro. Quando un acquirente aggiunge un prodotto al carrello, un sistema di gestione dell’inventario di terze parti controlla se l’articolo è in magazzino. In caso affermativo, consenti l’aggiunta del prodotto. In caso contrario, visualizza un messaggio di errore.  Per esempi di codice e ulteriori informazioni, vai a [Casi d&#39;uso del webhook](https://developer.adobe.com/commerce/extensibility/webhooks/use-cases/#add-product-to-cart).

## Quando eseguire i controlli di inventario

Quando verificare se l&#39;inventario è ancora disponibile alla fine spetterà all&#39;azionista aziendale, all&#39;architetto del software con un contributo da parte di altri soggetti chiave. Alcuni momenti appropriati possono essere l’aggiunta di un articolo al carrello e l’immissione del flusso di lavoro di pagamento. Qualsiasi altro evento aggiungerà semplicemente il carico ai sistemi back-end quando potrebbe non essere necessario. Tieni presente che l’obiettivo è individuare un problema di inventario solo quando è fondamentale. Altri controlli possono essere utili, ma se possono influire sull’obiettivo generale dei controlli dello stato dell’inventario, devono essere considerati con attenzione e consentiti solo se le parti interessate o altri sono consapevoli del potenziale rischio di un carico aggiuntivo sui sistemi back-end.

## Ricercare l’origine dell’inventario

È necessaria un&#39;indagine completa della fonte di inventario esterna. Gli elementi da valutare sono le opzioni API disponibili, il supporto per GraphQL e i tempi di risposta previsti. Se l&#39;origine inventario ha una larghezza di banda di connessione limitata o non è mai stata progettata per essere utilizzata in una richiesta in tempo reale, la possibilità di utilizzo è esclusa e l&#39;architetto deve considerare invece quasi in tempo reale.  Se i tempi di richiesta API superano i parametri definiti, questa opzione non sarà disponibile.  Un esempio potrebbe essere che le risposte API sono 200 MS® per una richiesta alla volta, ma aumentano a 500-900 MS® con un carico moderato.  Questo peggiorerebbe con un carico maggiore e impedirebbe la disponibilità delle chiamate di inventario live.

Assicurati di testare i tempi di risposta dell’API con richieste semplici e con un volume elevato simile al traffico previsto sul sito web live. Ricordati di testare tutte le aree dal commercio allo stesso tempo per simulare scenari reali. Se si prevede che nelle pagine dei prodotti si verifichi una chiamata di inventario live, durante la visualizzazione e la modifica di un carrello e durante il pagamento, il test di carico deve simulare tutti questi eventi contemporaneamente per simulare il comportamento reale del cliente e le aspettative di un negozio.

## Opzioni di fallback

Se l’origine dell’inventario non è disponibile e è disponibile il monitoraggio, si consiglia di utilizzare la funzionalità nativa di Adobe Commerce. Tuttavia, con un monitoraggio appropriato, l’esperienza del cliente può cambiare in modo dinamico per riflettere la perdita dei controlli di inventario in tempo reale. Ciò può significare che una vendita o un evento viene annullato in anticipo o rimosso dalla visualizzazione per evitare le vendite in eccesso. L&#39;aspettativa su cosa fare quando la fonte di inventario è inattiva per qualsiasi motivo deve essere considerata e discussa con il proprietario del negozio in modo che tutti siano a conoscenza di qualsiasi processo automatico che subentra in quella sfortunata circostanza.

## Conclusione

La decisione di effettuare controlli di inventario in tempo reale è significativa. La garanzia che il proprietario del sito web, il team di sviluppo e altri siano pienamente istruiti e consapevoli di tutti i vantaggi e le potenziali insidie incombono sul lead sviluppatore o sull’architetto. Fornendo un piano accurato che copra i motivi e un processo di fallback è fondamentale per il successo.

I controlli live dell’inventario possono essere effettuati, ma richiedono ricerche e riflessioni su test e convalida durante il ciclo di controllo qualità. Assicurati che il test di carico e i test automatizzati end-to-end aiutino a garantire che tutti i potenziali problemi vengano rilevati e valutati.

Considerando le azioni da eseguire se il monitoraggio rileva una tendenza di chiamate non riuscite al sistema esterno o se i tempi di risposta superano le soglie accettabili, il sito può rimanere online e offrire la minore irritazione ai clienti. Le opzioni di fallback possono essere semplici come la disattivazione dei controlli di inventario esterni e l’utilizzo della funzionalità nativa, oppure avanzate come la disattivazione di una promozione, la notifica al team di sviluppo dei problemi in aumento o persino il reindirizzamento delle richieste a un secondo o terzo sistema di backend, se disponibile. Il modo in cui viene esercitato il meccanismo di fallback dovrebbe essere pensato come l’effettiva implementazione, perché ogni integrazione a un certo punto presenta dei problemi. Tutto ciò che è automatizzato o richiede un&#39;azione manuale deve essere chiaramente documentato.
