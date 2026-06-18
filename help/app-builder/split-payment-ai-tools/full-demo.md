---
title: Demo completa di Split Payment POC — App Builder
description: Scopri come funzionano in questa demo Luma il pagamento frazionato, REST, App Builder I/O e l’accettazione/rifiuto da parte dell’operatore, oltre a un totale preordine configurabile che può bloccare il carrello.
feature: App Builder, Paas, Payments
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, User
level: Intermediate
doc-type: Technical Video
duration: 861
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 63ac13d8c5a97ee81dcdd1f3785a9875aaf2a4db
workflow-type: tm+mt
source-wordcount: '1029'
ht-degree: 0%

---

# Creazione di una demo completa per il pagamento frazionato POC: App Builder

Questa è la procedura dettagliata end-to-end del concetto proof of (Verifica del pagamento frazionato) basato su Adobe Commerce e Adobe App Builder. La demo presuppone che tu abbia già utilizzato strumenti di intelligenza artificiale e un prompt per produrre l’estensione in-process Commerce e l’app App Builder; questo video mostra cosa accade dopo che il codice viene unito, distribuito a un sito web Adobe Commerce Cloud utilizzando il tema Luma nativo e il progetto App Builder è live.

Un acquirente paga con parte contanti e parte **[!UICONTROL Store Credit]**. Commerce è il proprietario del checkout sincrono e delle API necessarie per la vetrina; App Builder gestisce l’orchestrazione, i flussi di lavoro degli operatori e i consumatori di eventi di I/O. L’implementazione di riferimento utilizza un progetto Commerce (PaaS) e il checkout nativo di Luma invece di una vetrina Edge Delivery Services, che è ancora un percorso comune per molti commercianti. Se utilizzi **Adobe Commerce as a Cloud Service** in una topologia diversa, il codice di App Builder rimane simile ma il lavoro in vetrina e quello in corso risulterebbero diversi. Per on-premise, self-hosted e Commerce nel cloud su Luma, questo video mostra una suddivisione pratica tra il codice in-process e App Builder per le nuove funzionalità.

## Video

>[!VIDEO](https://video.tv.adobe.com/v/3484107?captions=ita&learn=on)

## A chi serve questo video?

* Architetti tecnici che pianificano l&#39;estensibilità di Commerce
* Sviluppatori che implementano checkout e integrazioni
* Team di implementazione che necessitano di una suddivisione realistica tra PHP e App Builder

## Imposta lo scenario sulla vetrina

L&#39;account demo contiene **[!UICONTROL Store Credit]** e questo saldo viene visualizzato in fase di pagamento. Aggiungi i prodotti fino a quando il totale dell&#39;ordine è di circa $ 80. Questa dimensione ti dà spazio per mostrare sia il percorso di accettazione riuscito che un percorso di declino, simile a un declino delle carte, senza la lotta matematica.

**[!UICONTROL Split payment]** è offerto solo ai clienti con accesso, perché la sessione deve esporre il credito dello store. Il pagamento come ospite non mostra l’opzione di suddivisione.

1. Nel passaggio **[!UICONTROL Payment]**, scegliere il metodo di pagamento (la demo rinomina **[!UICONTROL Cash on delivery]** in **Solo contante**).
1. Nel metodo di pagamento viene visualizzata un&#39;area di pagamento frazionato. Il campo di cassa viene precompilato sul totale dell&#39;ordine e il componente legge **[!UICONTROL Store Credit]** dalla sessione.
1. Per un carrello ~$80, imposta **$50** in contanti e **$30** di credito store in modo che il componente mostri $50 + $30 = $80.
1. Quando gli importi sono validi, inserisci l’ordine.

L&#39;interfaccia utente attiva una chiamata a una nuova API REST **[!UICONTROL Commerce]** per la suddivisione. Il pagamento rimane sincrono: Commerce applica il credito del negozio, memorizza le linee divise nell’ordine ed emette un evento di I/O che App Builder utilizzerà in seguito. La pagina di successo è immediata per il cliente.

## Controlla l’ordine nell’amministratore Commerce

In **[!UICONTROL Sales]** > **[!UICONTROL Orders]**, apri il nuovo ordine. Nell&#39;area **[!UICONTROL Payment information]** è visualizzata la suddivisione, ad esempio **$50** contanti e **$30** credito archivio. Lo stato è **In sospeso** quando il contante non è ancora stato raccolto; il credito dell&#39;archivio è già utilizzato al momento della creazione dell&#39;ordine.

**[!UICONTROL Comments]** può includere testo aggiunto da App Builder. **[!UICONTROL Payment orchestrator action]** in App Builder ha ricevuto l&#39;evento I/O, ha controllato gli importi suddivisi e ha utilizzato REST autenticato per aggiungere commenti. **[!UICONTROL Commerce]** lo tratta come qualsiasi altra chiamata REST e non ha bisogno di conoscere il writer.

## Utilizzare il dashboard degli operatori di App Builder (ERP o back office)

Un&#39;esperienza HTML semplice viene eseguita come azione Web **[!UICONTROL App Builder]** (nessun **[!UICONTROL Admin]** per questa visualizzazione). Il dashboard chiama l&#39;API REST **[!UICONTROL Commerce Order]** e filtra su **In sospeso** in modo da trovare la gamba contanti da 50 $.

In genere, questo modello viene integrato con uno strumento ERP o di frode. Sono dimostrate due azioni:

* **Accetta** - Conferma che è stato ricevuto contante (in produzione, spesso un post automatico da un sistema di cassetti o di magazzini).
* **Rifiuto** - Simula una frode o un passaggio di raccolta non riuscito (in produzione, in genere automatizzato dal servizio rischi).

**Accettare e completare l&#39;ordine**

1. Nell&#39;app **[!UICONTROL App Builder]**, utilizza **Accept** per confermare i contanti.
1. L&#39;app chiama un nuovo endpoint **[!UICONTROL Commerce]** che finalizza la linea di cassa. Il flusso dell&#39;ordine di base (fatturazione e in questa build spedizione automatica) viene eseguito con il comportamento nativo **[!UICONTROL Commerce]**. Nessuno di questi percorsi richiede un elemento umano in **[!UICONTROL Admin]**; l&#39;orchestrazione e l&#39;endpoint sono attivi in App Builder.

Aggiorna lo stesso ordine in **[!UICONTROL Admin]**: lo stato passa a **Completo** quando il contante viene accettato, con la fattura e, se il prodotto è spedibile, una spedizione per ridurre l&#39;intero ciclo di vita nella demo.

**Rifiuta e annulla**

1. Aprire un ordine predefinito ancora in fase di declino.
1. Nell&#39;app **[!UICONTROL App Builder]**, utilizza **Rifiuta** per simulare una frode o un errore di raccolta.
1. In **[!UICONTROL Admin]**, l&#39;ordine è **Annullato**. La cronologia mostra uno stato di cassa rifiutato, i commenti spiegano il risultato, l&#39;acquirente non è stato addebitato alla linea di credito al negozio come se si trattasse di una vendita finale e la fattura di credito al negozio viene annullata con l&#39;annullamento. Un sistema di produzione invierebbe inoltre una notifica al cliente e rilascerebbe eventuali blocchi di magazzino o di servizio.

## Soglia totale ordine (convalida prima della creazione dell’ordine)

La configurazione di App Builder o **[!UICONTROL Commerce]** può supportare regole di convalida; questa build blocca gli ordini superiori a **$100** nel valore del carrello (un limite dimostrativo modificabile). Un controllo del valore non è la regola business più comune, in quanto i controlli di inventario o di disponibilità sono più tipici, ma indica che la convalida può essere eseguita alle **[!UICONTROL Payment]** in **[!UICONTROL Commerce]** prima della creazione di un ordine, mentre App Builder gestisce ancora l&#39;automazione di completamento.

1. Aggiungere prodotti fino a quando il totale è superiore a **$100**.
1. La divisione **[!UICONTROL UI]** viene ancora visualizzata; il risultato del limite superiore viene visualizzato solo su **Inserisci ordine**.
1. L&#39;acquirente visualizza un errore generico come **Impossibile elaborare il pagamento. Riprovare o contattare il supporto tecnico.**
1. **[!UICONTROL cart]** è invariato e il cliente può abbassare il totale, scegliere un altro metodo o contattare il supporto tecnico.

Un punteggio di rischio, una regola di regione o simile potrebbe collegarsi qui; **[!UICONTROL Commerce]** blocca l&#39;ordine e **[!UICONTROL App Builder]** può possedere le notifiche e il funzionamento del caso dopo il fatto.

## Commerce on-premise, Commerce nel cloud e **Adobe Commerce as a Cloud Service**

La procedura dettagliata corrisponde a **[!UICONTROL Adobe Commerce]** in un&#39;infrastruttura gestita dall&#39;utente o a **[!UICONTROL Commerce in the cloud]** (PaaS) con una vetrina tradizionale. Per **[!UICONTROL Adobe Commerce as a Cloud service]** (SaaS) con un front-end diverso e nessun modulo in-process in questa forma, i pezzi di App Builder sono in gran parte gli stessi, mentre il lavoro di storefront ed estensione sarebbe diverso. In tutti i casi, vale lo stesso principio: lascia che **[!UICONTROL Commerce]** faccia ciò che deve accadere all&#39;interno della richiesta **[!UICONTROL Commerce]** e utilizza **[!UICONTROL App Builder]** per il resto dell&#39;esperienza.

{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
