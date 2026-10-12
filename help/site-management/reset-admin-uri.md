---
title: Reimpostare l’URI amministratore utilizzando CLI
description: Scopri come ripristinare l’URI amministratore nella CLI di Adobe Commerce Cloud. Questo metodo è utile quando le modifiche all’URL dell’amministratore causano problemi di accesso.
feature: Admin Workspace, Console
topic: Administration, Commerce
role: Developer, User
level: Beginner
doc-type: Technical Video
duration: 144
last-substantial-update: 2024-10-14T00:00:00.000Z
jira: KT-16338
exl-id: dbc155d7-8ce9-4622-abfb-1d8077c3a975
TQID: 'https://experienceleague.adobe.com/OgyaTHVhQSRFApJPYwkzodSyAJcBjwk8kIrw4VBXltQ'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
    internal-label: Configuration
  - id: e7dae43f-215c-4cdf-90d3-c5a461a6e669
    internal-label: Admin tools and workspace
  - id: 125c1f49-aefd-5f34-a252-288937f95f6b
    internal-label: Marketing Tools
subfeature_v2:
  - id: bb2df8be-afdd-4818-b6b5-95ca1dd3bc3a
    internal-label: Admin workspace
  - id: c4af0798-d497-5e6b-8380-19812c26d00a
    internal-label: Console
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
    internal-label: User
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
    internal-label: Beginner
topic_v2:
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
    internal-label: Administration
source-git-commit: 71835f0240311d4b26ec8e0d9bbdf005f9887579
workflow-type: tm+mt
source-wordcount: '107'
ht-degree: 0%
---
# Reimposta l’URI di amministrazione utilizzando CLI

Scopri come ripristinare l’URI amministratore utilizzando il comando cli di Adobe Commerce Cloud. Questa opzione è utile se l’URL dell’amministratore è stato modificato dall’amministratore, ma è stato commesso un errore e non è più possibile accedere all’amministratore.

>[!VIDEO](https://video.tv.adobe.com/v/3435066?learn=on)

## Alcuni comandi utilizzati nell&#39;esercitazione

Modifica la configurazione in modo che l’URL del percorso amministratore personalizzato sia impostato su 0:

`$ php bin/magento config:set admin/url/use_custom 0`

Modifica la configurazione per l’URL del percorso di amministrazione personalizzato su 0

`$ php bin/magento config:set admin/url/use_custom_path 0`
