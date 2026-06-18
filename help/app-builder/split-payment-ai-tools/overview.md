---
title: Dividi POC di pagamento — Strumenti App Builder e AI
description: Scopri una proof of concept per pagamenti divisi con App Builder e Commerce PaaS, inclusi gli obiettivi, l’architettura e ciò che copre questa prima sessione.
feature: App Builder, Paas, Payments
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, User
level: Intermediate
doc-type: Technical Video
duration: 237
jira: KT-20791
source-git-commit: 63ac13d8c5a97ee81dcdd1f3785a9875aaf2a4db
workflow-type: tm+mt
source-wordcount: '572'
ht-degree: 0%

---

# Creazione di un POC di pagamento frazionato: strumenti App Builder e AI

Questa è la prima di una serie di tutorial che ti introducono all’utilizzo dello sviluppo assistito da intelligenza artificiale per aiutarti a creare una proof of concept di pagamento frazionato. Lavori con Adobe App Builder e Adobe Commerce nel cloud (PaaS) o on-premise, passando da una panoramica in questa sessione ai tutorial pratici che seguono. Aspettatevi obiettivi, architettura di alto livello e una roadmap per ciò che il resto della serie copre.

## Video

>[!VIDEO](https://video.tv.adobe.com/v/3483933?learn=on)

## Dettagli importanti


Questo tutorial colma il divario tra la maggior parte dei team di Adobe Commerce (con un livello di PHP elevato) e dove Adobe vuole che vadano: logica di business basata su eventi e senza server eseguita al di fuori di Commerce in Adobe App Builder. Lo fa attraverso una vera funzione funzionante piuttosto che un esempio di giocattolo.

### Cosa verrà effettivamente creato

Un sistema di pagamento frazionato in cui i clienti pagano utilizzando una combinazione di Contante alla consegna e Credito store. Una volta effettuato l&#39;ordine, l&#39;operatore (o sistema ERP) conferma o rifiuta il pagamento in contanti tramite un dashboard autonomo, senza mai aprire Commerce Admin. L’intero flusso di lavoro di accettazione/rifiuto viene eseguito in App Builder.

#### La lezione di architettura (insegnamento di base)

Il tutorial illustra un framework decisionale deliberato e ripetibile:

* **Cosa deve rimanere in PHP:** tutto ciò che viene eseguito in modo sincrono nel ciclo di richiesta di Commerce o che chiama le API interne di Commerce senza alcuna superficie esterna pulita
* **Cosa si sposta in App Builder:** tutto il resto: elaborazione di eventi, flusso di lavoro dell&#39;operatore, integrazioni esterne e strumenti rivolti all&#39;operatore

Non si tratta di &quot;spostare tutto in App Builder&quot;. È un punto di partenza pratico e onesto per i team che devono iniziare la transizione senza riscrivere.

### Perché il codice PHP non è incluso

L’approccio di prompt basato sull’intelligenza artificiale è in realtà migliore del codice di esempio per questo caso d’uso, perché il prompt acquisisce i contratti comportamentali e il ragionamento architettonico che il codice di esempio da solo non è in grado di trasmettere. Uno sviluppatore che esegue il prompt e legge l’output capisce perché il codice ha la forma che ha, non solo l’aspetto.

### Cosa è incluso

* Codice completo dell’applicazione App Builder (coerente tra i progetti, utilizzalo direttamente o come riferimento)
* Un set completo di prompt AI numerati progettati per Cursor e Claude, che coprono il modulo Commerce, l’orchestratore App Builder, il dashboard dell’operatore, i test e il percorso di produzione
* Uno script di simulazione per testare il flusso di accettazione/rifiuto rispetto a un sito Commerce live senza la necessità di un ERP reale
* Documentazione dell’architettura che illustra ogni decisione

### Per Chi È Questo

Sviluppatori on-premise su Adobe Commerce o Commerce Cloud che stanno iniziando la loro prima vera integrazione App Builder. Non per la distribuzione headless API-first, in particolare per i team in transizione che eseguono una vetrina tradizionale e che desiderano scoprire come scaricare la logica di business reale su App Builder senza abbandonare l’investimento PHP esistente.

### Callout prerequisiti

Adobe Commerce 2.4.5 o versione successiva, accesso a un’organizzazione Adobe Developer Console con un progetto App Builder ed Eventi di I/O abilitati. Per l’interfaccia utente di pagamento viene utilizzato un tema Luma pulito: i temi personalizzati richiederanno di regolare il prompt prima di eseguirlo.

### Considerazioni finali

Questa dovrebbe essere considerata una discussione di livello base sullo sviluppo basato sull’intelligenza artificiale. Questo tutorial è una dimostrazione per l’utilizzo di strumenti di intelligenza artificiale in un flusso di lavoro di sviluppo di Commerce, non solo un tutorial sulla suddivisione dei pagamenti.

{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
