---
title: Visualizza e imposta le configurazioni amministratore tramite la riga di comando
description: Scopri come visualizzare e impostare le configurazioni di amministrazione utilizzando la riga di comando.
feature: Configuration,Console,System
topic: Administration,Commerce
role: Developer
level: Beginner
doc-type: Technical Video
duration: 510
last-substantial-update: 2024-01-31T00:00:00.000Z
jira: KT-14877
exl-id: 6cecba51-8d39-46f5-9864-80126d8ca3da
TQID: https://experienceleague.adobe.com/W0kaFd-DJAPA1kFO0hWaBZXsSarojbt5Hnv3QkW85hA
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2:
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 168
ht-degree: 0%

---

# Visualizza e imposta le configurazioni amministratore tramite la riga di comando

Dimostrazione di come visualizzare, impostare e trovare i valori di configurazione con Commerce CLI. Scopri dove vengono salvati i valori e da dove provengono i valori predefiniti.

## A chi serve questo video?

* Sviluppatori Adobe Commerce

## Contenuto video

>[!VIDEO](https://video.tv.adobe.com/v/3427123?learn=on)

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

* [Strumento Riga di comando](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/config-cli.html?lang=it){target="_blank"}
* [Configurare la sicurezza dell’amministratore](https://experienceleague.adobe.com/docs/commerce-admin/systems/security/security-admin.html?lang=it){target="_blank"}
