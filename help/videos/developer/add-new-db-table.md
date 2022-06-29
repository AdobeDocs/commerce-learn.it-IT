---
title: Aggiungere una tabella a un database
description: '"[!DNL Commerce] dispone di un meccanismo speciale che ti consente di creare tabelle di database, modificarne quelle esistenti e persino di aggiungere alcuni dati."'
topic: Development
kt: 5613
doc-type: video
activity: use
exl-id: fb222752-5689-4f87-94cf-a61ed7005e6b
source-git-commit: acee5ba84ea32e14a743cd269f77ced821548ad6
workflow-type: tm+mt
source-wordcount: '167'
ht-degree: 0%

---

# Aggiungere una tabella a un database

[!DNL Commerce] dispone di un meccanismo speciale che consente di creare tabelle di database, modificare quelle esistenti e persino aggiungere alcuni dati ad esse, come i dati di configurazione, che devono essere aggiunti quando viene installato un modulo. Tale meccanismo consente di trasferire tali modifiche tra impianti diversi.

Invece di eseguire ripetutamente operazioni SQL manuali durante la reinstallazione del sistema, gli sviluppatori creano uno script di installazione (o aggiornamento) che contiene i dati. Lo script viene eseguito ogni volta che viene installato un modulo.

## A chi serve questo video?

- Sviluppatori

## Contenuto video

- Creare un modulo
- Crea script InstallSchema
- Crea script InstallData
- Aggiungi un nuovo modulo e verifica che sia stata creata una tabella con dati
- Creare uno script UpgradeSchema
- Creare uno script UpgradeData
- Esegui gli script di aggiornamento e verifica che la tabella sia cambiata

>[!VIDEO](https://video.tv.adobe.com/v/35791?quality=12&learn=on)

## Risorse utili

- [Comandi di migrazione](https://devdocs.magento.com/guides/v2.4/extension-dev-guide/declarative-schema/migration-commands.html)
