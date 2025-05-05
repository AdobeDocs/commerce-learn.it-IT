---
title: Visualizza e imposta le configurazioni amministratore tramite la riga di comando
description: Scopri come visualizzare e impostare le configurazioni di amministrazione utilizzando la riga di comando.
feature: Configuration,Console,System
topic: Administration,Commerce
role: Developer
level: Beginner
doc-type: Technical Video
duration: 462
last-substantial-update: 2024-01-31T00:00:00Z
jira: KT-14877
exl-id: 6cecba51-8d39-46f5-9864-80126d8ca3da
source-git-commit: d578c066f3e51827694c8bf85aa2324035a8b07b
workflow-type: tm+mt
source-wordcount: '151'
ht-degree: 0%

---

# Visualizza e imposta le configurazioni amministratore tramite la riga di comando

Dimostrazione di come visualizzare, impostare e trovare i valori di configurazione con Commerce CLI. Scopri dove vengono salvati i valori e da dove provengono i valori predefiniti.

## A chi serve questo video?

- Sviluppatori Adobe Commerce

## Contenuto video

>[!VIDEO](https://video.tv.adobe.com/v/3427123?&learn=on)

## Alcuni comandi utilizzati nell&#39;esercitazione

Cambia l&#39;impostazione di sicurezza della password su consigliata:

`$ php bin/magento config:set admin/security/password_is_forced 0`

Mostra l&#39;indirizzo e-mail per la funzionalità di copia automatica dell&#39;ordine cliente

`$ php bin/magento config:show sales_email/order/copy_to`

Mostra il risultato vuoto per una configurazione che ha un valore nell’amministratore

`php bin/magento config:show trans_email/ident_sales/email`

## Query Mysql utilizzate nell’esercitazione

```
SELECT * FROM core_config_data WHERE path = 'sales_email/order/copy_to';

SELECT * FROM core_config_data WHERE path = 'sales_email/order_comment/copy_to';

SELECT * FROM core_config_data WHERE path = 'trans_email/ident_sales/email';
```

## Dove trovare l’e-mail di vendita predefinita

Come trovare il valore di configurazione definito in una posizione specifica nella base di codice?
`grep -rnw vendor/magento/ -e 'sales@example.com'`

Per visualizzare una pagina nel terminale e visualizzare i numeri di riga `cat -n vendor/magento/module-email/etc/config.xml`

## Risorse aggiuntive

- [Strumento da riga di comando](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/config-cli.html?lang=it){target="_blank"}
- [Configura sicurezza amministratore](https://experienceleague.adobe.com/docs/commerce-admin/systems/security/security-admin.html?lang=it){target="_blank"}
