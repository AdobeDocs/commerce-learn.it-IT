---
title: Reimpostare l’URI amministratore utilizzando CLI
description: Scopri come ripristinare l’URI amministratore nella CLI di Adobe Commerce Cloud. Questo metodo è utile quando le modifiche all’URL dell’amministratore causano problemi di accesso.
feature: Admin Workspace, Console
topic: Administration, Commerce
role: Developer, User
level: Beginner
doc-type: Technical Video
duration: 144
last-substantial-update: 2024-10-14
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
source-git-commit: 8d1fc3f2db752369487199175fb99a4e9db34b19
workflow-type: tm+mt
source-wordcount: '88'
ht-degree: 0%
---
# Reimposta l’URI di amministrazione utilizzando CLI

Per recuperare l’accesso all’amministratore dopo una modifica URL errata, reimposta l’URI amministratore utilizzando l’interfaccia della riga di comando di Adobe Commerce Cloud.

>[!VIDEO](https://video.tv.adobe.com/v/3435066?learn=on)

## Alcuni comandi utilizzati nell&#39;esercitazione

Per disabilitare l’URL amministratore personalizzato, imposta il valore di configurazione su 0:

`$ php bin/magento config:set admin/url/use_custom 0`

Per disabilitare il percorso di amministrazione personalizzato, imposta il valore di configurazione su 0:

`$ php bin/magento config:set admin/url/use_custom_path 0`
