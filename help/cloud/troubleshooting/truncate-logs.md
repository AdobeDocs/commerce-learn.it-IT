---
title: Tronca registri
description: Scopri come eseguire il triage di una distribuzione non riuscita a causa di un disco rigido completo troncando i file di registro di grandi dimensioni.
feature: Cloud, Site Management
topic: Commerce, Development
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
doc-type: Technical Video
duration: 302
last-substantial-update: 2025-3-25
jira: KT-17595
exl-id: 4a36de40-fb55-41ad-afef-35fc18a271ec
TQID: 'https://experienceleague.adobe.com/A-ecz3Yr1lCNQZmM9ung-lf7WIJZmB3h0TnsblyAbkQ'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
    internal-label: Configuration
  - id: 5a951749-fac9-5bc7-9a98-ebe4ff066437
    internal-label: Cloud
  - id: 938d2364-5176-55ec-80f1-9415253e5e51
    internal-label: Site Management
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
    internal-label: Intermediate
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
    internal-label: Beginner
topic_v2:
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
    internal-label: Troubleshooting
source-git-commit: 71835f0240311d4b26ec8e0d9bbdf005f9887579
workflow-type: tm+mt
source-wordcount: '200'
ht-degree: 0%
---
# Tronca registri

Scopri come effettuare il triage e una distribuzione non riuscita a causa di un disco rigido completo. Scopri come trovare e quali comandi possono essere eseguiti per liberare spazio nell’ambiente Adobe Commerce Cloud.

Se si ritiene che possano essere necessari questi file di registro, è possibile `rsync` o utilizzare altri metodi per ottenere una copia disponibile dal server prima di troncarli.

## Per chi è questo video

* Sviluppatori e professionisti IT
* Amministratori di sistema

## Contenuto video

* Diagnosticare e risolvere una distribuzione non riuscita
* Dove si trovano alcuni file di registro di grandi dimensioni comuni
* Metodo rapido per troncare un file di registro

>[!VIDEO](https://video.tv.adobe.com/v/3454572?learn=on)


## Comandi utilizzati nel video

Per controllare lo spazio del disco rigido `df -h`. Presta attenzione allo sviluppo/mapper/xxxx della linea

```SHELL
df -h

Filesystem                              Size  Used Avail Use% Mounted on
/dev/loop217                           1016M 1016M     0 100% /
none                                    492K  4.0K  488K   1% /dev
fake-sysfs                              120G     0  120G   0% /sys
tmpfs                                   120G     0  120G   0% /sys/fs/cgroup
tmpfs                                   384M     0  384M   0% /dev/shm
tmpfs                                    50M  460K   50M   1% /run
tmpfs                                   5.0M     0  5.0M   0% /run/lock
/dev/loop236                            144M  144M     0 100% /app
/dev/mapper/hyjh5nlaoabqtxxnh4opgjqzpu  4.9G  4.9G     0 100% /mnt
/dev/loop14                             8.0G  403M  7.6G   5% /tmp
/dev/mapper/platform-lxc                5.0T   69G  4.7T   2% /run/shared
```


Visualizzare i file e le relative dimensioni in un formato leggibile, ad esempio kb, mb e gb, utilizzando il comando `ls -lah`

```SHELL
ls -lah

total 4.7G
drwxr-xr-x 2 web web 4.0K Jul 16  2024 .
drwxr-xr-x 6 web web 4.0K Jan 10  2024 ..
-rw-rw-r-- 1 web web 487K Jul  5  2024 cache.log
-rw-r--r-- 1 web web 1.2K Jul 16  2024 cloud.error.log
-rw-r--r-- 1 web web 328K Mar 25 14:09 cloud.log
-rw-rw-r-- 1 web web 2.4G Jul  5  2024 cron.log
-rw-rw-r-- 1 web web  363 Dec  6  2023 debug.log
-rw-rw-r-- 1 web web  15K Jan 10  2024 indexation.log
-rw-r--r-- 1 web web 229K Jan 10  2024 install_upgrade.log
-rw-r--r-- 1 web web 2.9K Jul 16  2024 patch.log
-rw-rw-r-- 1 web web 2.4G Mar 25 15:36 support_report.log
-rw-rw-r-- 1 web web  516 Dec  6  2023 system.log
```

## Esempi per troncare il registro

Dopo aver eseguito il ssh nel progetto e nell&#39;ambiente corretti, passare alla directory `var/log`. È quindi possibile troncare un file con un elemento simile a `> some-log-file.log`

```BASH
> support_report.log 
> cron.log 
```

## Documentazione correlata

* [Notifiche stato](https://experienceleague.adobe.com/it/docs/commerce-on-cloud/user-guide/dev-tools/integrations/health-notifications){target="_blank"}
