---
title: Reimpostare l’URI amministratore utilizzando CLI
description: Scopri come ripristinare l’URI amministratore nella CLI di Adobe Commerce Cloud. Questo metodo è utile quando le modifiche all’URL dell’amministratore causano problemi di accesso.
feature: Admin Workspace, Console
topic: Administration, Commerce
role: Developer, User
level: Beginner
doc-type: Technical Video
duration: 123
last-substantial-update: 2024-10-14T00:00:00Z
jira: KT-16338
exl-id: dbc155d7-8ce9-4622-abfb-1d8077c3a975
source-git-commit: 25ee35b730cc6265665a87c9c37d24e88c41b60e
workflow-type: tm+mt
source-wordcount: '107'
ht-degree: 0%

---

# Reimposta l’URI di amministrazione utilizzando CLI

Scopri come ripristinare l’URI amministratore utilizzando il comando cli di Adobe Commerce Cloud. Questa opzione è utile se l’URL dell’amministratore è stato modificato dall’amministratore, ma è stato commesso un errore e non è più possibile accedere all’amministratore.

>[!VIDEO](https://video.tv.adobe.com/v/3439700/?learn=on&captions=ita)

## Alcuni comandi utilizzati nell&#39;esercitazione

Modifica la configurazione in modo che l’URL del percorso amministratore personalizzato sia impostato su 0:

`$ php bin/magento config:set admin/url/use_custom 0`

Modifica la configurazione per l’URL del percorso di amministrazione personalizzato su 0

`$ php bin/magento config:set admin/url/use_custom_path 0`
