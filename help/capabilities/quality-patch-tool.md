---
title: Strumento Quality Patch
description: Scopri come utilizzare lo strumento Quality patch per diagnosticare un problema, trovare una soluzione e applicare una patch trovata nell’elenco esistente di patch disponibili.
feature: Cloud, Configuration, Logs, System, Tools and External Services
topic: Architecture, Commerce, Development
role: Admin, Architect, User
level: Beginner, Intermediate
doc-type: Technical Video
duration: 771
last-substantial-update: 2024-07-17T00:00:00Z
jira: KT-15836
exl-id: 16710f27-1232-4c6a-aac3-9838308d1267
source-git-commit: 43085b1a139e21ea695cc10ffab1413088ba4426
workflow-type: tm+mt
source-wordcount: '546'
ht-degree: 0%

---

# Strumento Qualità patch

Scopri come utilizzare lo strumento Quality patch per diagnosticare un problema, trovare una soluzione e applicare una patch trovata nell’elenco esistente di patch disponibili.

## Cosa aspettarsi dal video

Scopri come risolvere un problema, quindi utilizza alcune tecniche di base per trovare una patch di qualità da applicare.

## Pubblico

* Sviluppatori che imparano a individuare i problemi e a sfruttare questo strumento per applicare patch GIT per i problemi noti

## Contenuto video

Lo strumento Quality Patches è un&#39;utility da riga di comando per Adobe Commerce e Magento Open Source. Ecco cosa ti consente di fare:

* Visualizzare informazioni generali sulle ultime patch di qualità.
* Applicare patch di qualità all&#39;installazione.
* Ripristinare le patch applicate, se necessario

Queste patch vengono sviluppate dalla comunità di sviluppatori Adobe e dal Magento Open Source per migliorare la stabilità e le prestazioni. Non è consigliabile applicare un numero elevato di patch, in quanto ciò potrebbe complicare gli aggiornamenti futuri.

>[!VIDEO](https://video.tv.adobe.com/v/3431436?learn=on)

## Perché utilizzare lo strumento di correzione della qualità

È possibile utilizzare lo strumento di controllo qualità per Adobe Commerce o Magento Open Source se si desidera:

Stabilità e prestazioni migliorate: le patch di qualità consentono di risolvere i problemi, migliorare la sicurezza e ottimizzare l&#39;installazione.
Resta aggiornato: l’applicazione delle patch garantisce che il sistema sia aggiornato e protetto.
Ripristina modifiche: se una patch causa problemi imprevisti, puoi ripristinarla utilizzando lo strumento. È consigliabile applicare un numero limitato di patch per evitare di complicare gli aggiornamenti futuri.  

## Limitazioni o problemi relativi all&#39;utilizzo dello strumento di correzione di qualità

Sebbene lo strumento di controllo della qualità offra dei vantaggi, è necessario tenere presenti alcune considerazioni:

* Compatibilità: verifica che le patch siano compatibili con la versione specifica di Adobe Commerce o del Magento Open Source.
* Test: esegui sempre il test delle patch in un ambiente di staging prima di applicarle alla produzione. Possono verificarsi problemi imprevisti.
* Dipendenze patch: alcune patch possono dipendere da altre. Presta attenzione a eventuali prerequisiti.
* Personalizzazioni: se hai apportato modifiche al codice personalizzato, le patch potrebbero essere in conflitto. Esamina attentamente le modifiche.
* Backup: eseguire il backup dell&#39;installazione prima di applicare le patch per evitare la perdita di dati.

Sebbene lo strumento Qualità patch sia utile per applicare un numero limitato di patch, non è consigliato per la gestione di un grande volume di patch. L&#39;applicazione di un numero eccessivo di patch può complicare gli aggiornamenti e la manutenzione futuri. Se hai numerose patch da applicare, considera approcci alternativi o consulta uno specialista di Magento. 

## Riepilogo

Lo strumento Quality Patches (Patch di qualità) consente alle piattaforme di e-commerce di migliorare la stabilità e la sicurezza applicando patch. Queste patch consentono di risolvere i problemi, migliorare le prestazioni e ottimizzare il sistema. Mantenere aggiornata l&#39;installazione garantisce la protezione contro le vulnerabilità.

Prima di applicare le patch, è fondamentale verificarle in un ambiente di staging. Assicurati la compatibilità con la versione specifica di Adobe Commerce o del Magento Open Source. Alcune patch possono avere dipendenze, quindi controlla attentamente i prerequisiti.

 Eseguire il backup dell&#39;installazione prima di applicare le patch per evitare la perdita di dati. Se hai apportato modifiche al codice personalizzato, tieni presente che le patch potrebbero essere in conflitto. Segui le best practice e monitora l’impatto di ciascuna patch.

## Articoli e video correlati

* [Ricerca di strumenti di patch di qualità](https://experienceleague.adobe.com/tools/commerce-quality-patches/index.html)
* [Note sulla versione](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/quality-patches-tool/release-notes)
* [Github per le patch](https://github.com/magento/quality-patches/blob/master/patches/os/)
* [Utilizzo dello strumento di correzione qualità](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/quality-patches-tool/usage)
* [Video tecnico su QPT](https://experienceleague.adobe.com/en/docs/commerce-learn/tutorials/tools/quality-patch-tool)
