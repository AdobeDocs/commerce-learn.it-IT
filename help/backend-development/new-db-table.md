---
title: Aggiungere una tabella a un database
description: "[!DNL Commerce] dispone di un meccanismo speciale che consente di creare tabelle di database, modificare quelle esistenti e persino aggiungere alcuni dati in esse."
kt: 5613
doc-type: video
activity: use
last-substantial-update: 2023-2-10
feature: Configuration, System, Backend Development
topic: Commerce, Development
role: Admin, Developer
level: Beginner, Intermediate
exl-id: fb222752-5689-4f87-94cf-a61ed7005e6b
source-git-commit: 79529c8d77df74e6f77ab3a01b45541a38dbf680
workflow-type: tm+mt
source-wordcount: '188'
ht-degree: 0%

---

# Aggiungere una tabella a un database

>[!IMPORTANT]
>
>Questa operazione non è più consigliata. Fare riferimento a https://developer.adobe.com/commerce/php/development/components/declarative-schema/


[!DNL Commerce] dispone di un meccanismo speciale che consente di creare tabelle di database, modificare quelle esistenti e persino aggiungervi alcuni dati, ad esempio i dati di configurazione, che devono essere aggiunti quando viene installato un modulo. Questo meccanismo consente di trasferire tali modifiche tra impianti diversi.

Anziché eseguire ripetutamente operazioni SQL manuali durante la reinstallazione del sistema, gli sviluppatori creano uno script di installazione (o aggiornamento) contenente i dati. Lo script viene eseguito ogni volta che viene installato un modulo.

## A chi serve questo video?

- Sviluppatori

## Contenuto video

- Creare un modulo
- Crea script InstallSchema
- Crea script InstallData
- Aggiungi un nuovo modulo e verifica che sia stata creata una tabella con dati
- Creare uno script UpgradeSchema
- Creare uno script UpgradeData
- Esegui gli script di aggiornamento e verifica che la tabella sia stata modificata

>[!VIDEO](https://video.tv.adobe.com/v/35791?quality=12&learn=on)

## Risorse utili

- [Migrazione degli script di installazione/aggiornamento a uno schema dichiarativo](https://developer.adobe.com/commerce/php/development/components/declarative-schema/migration-scripts/)
