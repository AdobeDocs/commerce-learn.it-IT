---
title: Creare un modulo
description: Crea una pagina che restituisca json con un parametro.
topic: Development
kt: 5602
doc-type: video
activity: use
exl-id: 941c04ee-54b8-4b81-b77d-fff5875927f0
source-git-commit: 32d1366758fa6453a48570cfd08d10a93559a974
workflow-type: tm+mt
source-wordcount: '94'
ht-degree: 0%

---

# Creare un modulo

Il modulo è un elemento strutturale di [!DNL Commerce] - l&#39;intero sistema è basato su moduli. In genere, il primo passaggio nella creazione di una personalizzazione consiste nella creazione di un modulo.

## A chi serve questo video?

- Sviluppatori

## Passaggi per aggiungere un modulo

- Crea la cartella dei moduli.
- Crea il file etc/module.xml.
- Crea il file registration.php.
- Esegui la configurazione del raccoglitore/magento.
- Aggiorna lo script per installare il nuovo modulo.
- Verifica che il modulo funzioni.

>[!VIDEO](https://video.tv.adobe.com/v/35792?learn=on)

### module.xml

```xml
<?xml version="1.0"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:noNamespaceSchemaLocation="urn:magento:framework:Module/etc/module.xsd">
    <module name="Training_Sales">
        <sequence>
            <module name="Magento_Sales"/>
        </sequence>
    </module>
</config>
```

### registration.php

```PHP
<?php

use Magento\Framework\Component\ComponentRegistrar;

ComponentRegistrar::register(
    ComponentRegistrar::MODULE,
    'Training_Sales',
    __DIR__);
```

## Risorse utili

- [Guida di riferimento del modulo](https://developer.adobe.com/commerce/php/module-reference/)
