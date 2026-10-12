---
title: Strumento Quality Patch
description: Scopri come utilizzare lo strumento Quality patch per diagnosticare un problema, trovare una soluzione e applicare una patch trovata nell’elenco esistente di patch disponibili.
feature: Cloud, Configuration, Logs, System, Tools and External Services
topic: Architecture, Commerce, Development
role: Admin, Developer, User
level: Beginner, Intermediate
doc-type: Technical Video
duration: 903
last-substantial-update: 2024-07-17T00:00:00.000Z
jira: KT-15836
exl-id: 16710f27-1232-4c6a-aac3-9838308d1267
TQID: 'https://experienceleague.adobe.com/GpcJqSCn3XqLZtm-QdQ-ka9c-RdkG-C6Hd3FpXrh8-I'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
    internal-label: Security
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
    internal-label: Configuration
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
    internal-label: Architecture
  - id: f42e0a1a-0d79-488d-a83f-f2c30672b137
    internal-label: Reporting
  - id: 5a951749-fac9-5bc7-9a98-ebe4ff066437
    internal-label: Cloud
  - id: 8d0b446f-5b16-5a10-b272-01143504a11c
    internal-label: System
  - id: 125c1f49-aefd-5f34-a252-288937f95f6b
    internal-label: Marketing Tools
subfeature_v2:
  - id: 3c398179-d35a-51ba-b317-6c5b95feef5e
    internal-label: Logs
  - id: 618ab558-d6ad-5352-99d6-d5702c6fdf80
    internal-label: Tools and External Services
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
    internal-label: User
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
    internal-label: Intermediate
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
    internal-label: Beginner
topic_v2:
  - id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
    internal-label: Reporting
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
    internal-label: Security
source-git-commit: 71835f0240311d4b26ec8e0d9bbdf005f9887579
workflow-type: tm+mt
source-wordcount: '592'
ht-degree: 0%
---
# Strumento Qualità patch

Scopri come utilizzare lo strumento Quality patch per diagnosticare un problema, trovare una soluzione e applicare una patch trovata nell’elenco esistente di patch disponibili.

## Cosa imparerai

Scopri come risolvere un problema, quindi utilizza alcune tecniche di base per trovare una patch di qualità da applicare.

## Pubblico

* Sviluppatori che imparano a individuare i problemi e a sfruttare questo strumento per applicare patch GIT per i problemi noti

## Contenuto video

Lo strumento Quality Patches (Patch di qualità) è un’utility per riga di comando per Adobe Commerce e Magento Open Source. Ecco cosa ti consente di fare:

* Visualizzare informazioni generali sulle ultime patch di qualità.
* Applicare patch di qualità all&#39;installazione.
* Ripristinare le patch applicate, se necessario

Queste patch sono sviluppate da Adobe Developers e dalla community Magento Open Source per migliorare la stabilità e le prestazioni. Non è consigliabile applicare un numero elevato di patch, in quanto ciò potrebbe complicare gli aggiornamenti futuri.

>[!VIDEO](https://video.tv.adobe.com/v/3431436?learn=on)

## Perché utilizzare lo strumento di correzione della qualità

È possibile utilizzare lo strumento Quality Patches per Adobe Commerce o Magento Open Source se si desidera:

Stabilità e prestazioni migliorate: le patch di qualità consentono di risolvere i problemi, migliorare la sicurezza e ottimizzare l&#39;installazione.
Resta aggiornato: l’applicazione delle patch garantisce che il sistema sia aggiornato e protetto.
Ripristina modifiche: se una patch causa problemi imprevisti, puoi ripristinarla utilizzando lo strumento. È consigliabile applicare un numero limitato di patch per evitare di complicare gli aggiornamenti futuri.  

## Limitazioni o problemi relativi all&#39;utilizzo dello strumento di correzione di qualità

Sebbene lo strumento di controllo della qualità offra dei vantaggi, è necessario tenere presenti alcune considerazioni:

* Compatibilità: verifica che le patch siano compatibili con la versione specifica di Adobe Commerce o Magento Open Source.
* Test: esegui sempre il test delle patch in un ambiente di staging prima di applicarle alla produzione. Possono verificarsi problemi imprevisti.
* Dipendenze patch: alcune patch possono dipendere da altre. Presta attenzione a eventuali prerequisiti.
* Personalizzazioni: se hai apportato modifiche al codice personalizzato, le patch potrebbero essere in conflitto. Esamina attentamente le modifiche.
* Backup: eseguire il backup dell&#39;installazione prima di applicare le patch per evitare la perdita di dati.

Sebbene lo strumento Qualità patch sia utile per applicare un numero limitato di patch, non è consigliato per la gestione di un grande volume di patch. L&#39;applicazione di un numero eccessivo di patch può complicare gli aggiornamenti e la manutenzione futuri. Se hai numerose patch da applicare, considera approcci alternativi o consulta uno specialista di Magento. 

## Riepilogo

Lo strumento Quality Patches (Patch di qualità) consente alle piattaforme di e-commerce di migliorare la stabilità e la sicurezza applicando patch. Queste patch consentono di risolvere i problemi, migliorare le prestazioni e ottimizzare il sistema. Mantenere aggiornata l&#39;installazione garantisce la protezione contro le vulnerabilità.

Prima di applicare le patch, è fondamentale verificarle in un ambiente di staging. Assicurati la compatibilità con la versione specifica di Adobe Commerce o Magento Open Source. Alcune patch possono avere dipendenze, quindi controlla attentamente i prerequisiti.

 Eseguire il backup dell&#39;installazione prima di applicare le patch per evitare la perdita di dati. Se hai apportato modifiche al codice personalizzato, tieni presente che le patch potrebbero essere in conflitto. Segui le best practice e monitora l’impatto di ciascuna patch.

## Articoli e video correlati

* [Ricerca di strumenti Quality Patch](https://experienceleague.adobe.com/tools/commerce-quality-patches/index.html?lang=it)
* [Note sulla versione](https://experienceleague.adobe.com/it/docs/commerce-operations/tools/quality-patches-tool/release-notes)
* [Github per patch](https://github.com/magento/quality-patches/blob/master/patches/os/)
* [Utilizzo dello strumento di correzione di qualità](https://experienceleague.adobe.com/it/docs/commerce-operations/tools/quality-patches-tool/usage)
* [Video tecnico su QPT](https://experienceleague.adobe.com/it/docs/commerce-learn/tutorials/tools/quality-patch-tool)
