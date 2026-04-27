---
title: Strumento Quality Patch
description: Learn how to use the quality patch tool when diagnosing a problem, finding a solution and applying a patch found in the existing list of patches available.
feature: Cloud, Configuration, Logs, System, Tools and External Services
topic: Architecture, Commerce, Development
role: Admin, Developer, User
level: Beginner, Intermediate
doc-type: Technical Video
duration: 903
last-substantial-update: 2024-07-17T00:00:00.000Z
jira: KT-15836
exl-id: 16710f27-1232-4c6a-aac3-9838308d1267
TQID: https://experienceleague.adobe.com/GpcJqSCn3XqLZtm-QdQ-ka9c-RdkG-C6Hd3FpXrh8-I
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
  - id: f42e0a1a-0d79-488d-a83f-f2c30672b137
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2:
  - id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 593
ht-degree: 0%

---

# Quality patch tool

Learn how to use the quality patch tool when diagnosing a problem, finding a solution and applying a patch found in the existing list of patches available.

## Cosa imparerai

Learn how to triage an issue, then use some basic techniques to find a quality patch to apply a fix.

## Pubblico

* Developers learning how to find issues and leverage this tool for applying GIT patches for known issues

## Contenuto video

The Quality Patches Tool is a command-line utility for Adobe Commerce and Magento Open Source. Here&#39;s what it allows you to do:

* View general information about the latest quality patches.
* Apply quality patches to your installation.
* Revert applied patches if needed

These patches are developed from Adobe Developers the Magento Open Source community to enhance stability and performance. Keep in mind that it&#39;s not recommended for applying large numbers of patches, as it can complicate future upgrades.

>[!VIDEO](https://video.tv.adobe.com/v/3431436?learn=on)

## Why use the quality patch tool

You might want to use the Quality Patches Tool for Adobe Commerce or Magento Open Source if you&#39;re looking to:

Enhance stability and performance: Quality patches address issues, improve security, and optimize your installation.
Stay up-to-date: Applying patches ensures that your system is current and protected.
Revert changes: If a patch causes unexpected issues, you can revert it using the tool. Remember, it&#39;s best suited for applying a limited number of patches to avoid complicating future upgrades.  

## Limitations or concerns with using the quality patch tool

While the Quality Patches Tool offers benefits, there are a few considerations to keep in mind:

* Compatibility: Ensure that the patches are compatible with your specific version of Adobe Commerce or Magento Open Source.
* Testing: Always test patches in a staging environment before applying them to production. Possono verificarsi problemi imprevisti.
* Dipendenze patch: alcune patch possono dipendere da altre. Presta attenzione a eventuali prerequisiti.
* Personalizzazioni: se hai apportato modifiche al codice personalizzato, le patch potrebbero essere in conflitto. Esamina attentamente le modifiche.
* Backup: eseguire il backup dell&#39;installazione prima di applicare le patch per evitare la perdita di dati.

Sebbene lo strumento Qualità patch sia utile per applicare un numero limitato di patch, non è consigliato per la gestione di un grande volume di patch. L&#39;applicazione di un numero eccessivo di patch può complicare gli aggiornamenti e la manutenzione futuri. In caso di numerose patch da applicare, prendere in considerazione approcci alternativi o consultare uno specialista Magento. 

## Riepilogo

Lo strumento Quality Patches (Patch di qualità) consente alle piattaforme di e-commerce di migliorare la stabilità e la sicurezza applicando patch. Queste patch consentono di risolvere i problemi, migliorare le prestazioni e ottimizzare il sistema. Mantenere aggiornata l&#39;installazione garantisce la protezione contro le vulnerabilità.

Prima di applicare le patch, è fondamentale verificarle in un ambiente di staging. Assicurati la compatibilità con la versione specifica di Adobe Commerce o Magento Open Source. Alcune patch possono avere dipendenze, quindi controlla attentamente i prerequisiti.

 Eseguire il backup dell&#39;installazione prima di applicare le patch per evitare la perdita di dati. Se hai apportato modifiche al codice personalizzato, tieni presente che le patch potrebbero essere in conflitto. Segui le best practice e monitora l’impatto di ciascuna patch.

## Articoli e video correlati

* [Ricerca di strumenti Quality Patch](https://experienceleague.adobe.com/tools/commerce-quality-patches/index.html?lang=it)
* [Note sulla versione](https://experienceleague.adobe.com/it/docs/commerce-operations/tools/quality-patches-tool/release-notes)
* [Github per patch](https://github.com/magento/quality-patches/blob/master/patches/os/)
* [Utilizzo dello strumento di correzione di qualità](https://experienceleague.adobe.com/it/docs/commerce-operations/tools/quality-patches-tool/usage)
* [Technical video on QPT](https://experienceleague.adobe.com/it/docs/commerce-learn/tutorials/tools/quality-patch-tool)
