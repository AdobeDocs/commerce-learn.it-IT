---
title: Connettere ed eseguire query sul database
description: Scopri diversi metodi per la connessione a un progetto cloud Adobe Commerce. Scopri come estrarre un database per utilizzarlo fuori sede. Scopri alcuni metodi per mascherare i dati PII e rimuoverli.
feature: Backend Development,Console,Cloud
topic: Commerce,Development
role: Developer
level: Intermediate, Experienced
doc-type: Technical Video
duration: 0
last-substantial-update: 2024-02-14T00:00:00Z
jira: KT-14910
thumbnail: KT-14910.jpeg
exl-id: e740bbd0-5ec7-4272-89cb-0bed776eb149
source-git-commit: a951f61ff71ad3777f8aebfa3c237b2ec1a4b1a5
workflow-type: tm+mt
source-wordcount: '1047'
ht-degree: 0%

---

# Connettere ed eseguire query sul database di Adobe Commerce

Questa esercitazione spiega come connetterti a un progetto Adobe Commerce su cloud, scaricare un database per l’utilizzo fuori sede, mascherare i dati PII e rimuoverlo.

Puoi accedere ai dati di Adobe Commerce dal progetto cloud utilizzando uno dei seguenti metodi:

* Utilizzo di un dump del database locale
* Una connessione DB all’ambiente cloud remoto utilizzando un’applicazione come Mysql Workbench o Tables Plus
* Connessione diretta all&#39;ambiente cloud mediante lo strumento CLI di Magento-Cloud ed esecuzione di comandi sul server remoto

Il metodo preferito consiste nell&#39;eseguire un dump del database e scorrerlo per rimuovere eventuali informazioni sui clienti. Rimuovi completamente i dati del cliente se non sono necessari.

## Utilizzo dello strumento CLI di Adobe Commerce Cloud

La creazione di un dump del database richiede che sia [CLI ADOBE COMMERCE CLOUD](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/dev-tools/cloud-cli/cloud-cli-overview.html) installato. Sul notebook locale, accedi a una directory ed esegui il seguente comando. Assicurati di sostituire `your-project-id` con l’ID progetto, che è simile a `asasdasd45q`. È inoltre necessario sostituire `your-environment-name` con il nome dell&#39;ambiente, ad esempio `master` o `staging`.

`magento-cloud db:dump -p your-project-id -e your-environment-name`

Se non sei sicuro dell’ID del progetto o dell’ambiente, puoi omettere questi elementi nel comando:

`magento-cloud db:dump`

La CLI richiede di specificare il progetto e l&#39;ambiente corretti. Nell&#39;esempio seguente viene visualizzata tale finestra di dialogo. Questo esempio mostra diversi progetti assegnati al tuo account, ma probabilmente avrai a disposizione un solo progetto.

Cambia in una directory

```bash
cd ~/Downloads/db-tutorial 
```

Ora esegui il comando per creare l’immagine del database

```bash
magento-cloud db:dump
```

Poiché non è stato specificato un progetto o un ambiente, Adobe Commerce CLI porrà alcune domande, di seguito è riportata una finestra di dialogo di esempio

```bash
Enter a number to choose a project:
  [0] demo-ralbin (ral32nryq4123)
  [1] adobe-commerce-demo (abc123zzkipexnqo)
  [2] DX Tutorials - Commerce (abasrpikfw4123)
 > 2

Enter a number to choose an environment:
Default: master
  [0] master (type: production)
  [1] remote-db (type: development)
 > 1

Creating SQL dump file: /Users/<username>/Downloads/db-tutorial/abasrpikfw4123--remote-db-ecpefky--mysql--main--dump.sql
```

## Utilizzo degli strumenti ECE di Adobe Commerce

Se non disponi dello strumento Adobe Commerce CLI, puoi `ssh` nel progetto ed esegui il comando `ece` comando `vendor/bin/ece-tools db-dump`: risposta di esempio:

```bash
ssh abasrpikfw4123-remote-db-ecpefky--mymagento@ssh.us-4.magento.cloud

 __  __                   _          ___ _             _ 
|  \/  |__ _ __ _ ___ _ _| |_ ___   / __| |___ _  _ __| |
| |\/| / _` / _` / -_) ' \  _/ _ \ | (__| / _ \ || / _` |
|_|  |_\__,_\__, \___|_||_\__\___/  \___|_\___/\_,_\__,_|
            |___/                                        

 Welcome to Magento Cloud.

 This is environment remote-db-ecpefky
 of project abasrpikfw4123.

web@mymagento.0:~$ vendor/bin/ece-tools db-dump
The db-dump operation switches the site to maintenance mode, stops all active cron jobs and consumer queue processes, and disables cron jobs before starting the dump process.
Your site will not receive any traffic until the operation completes.
Do you wish to proceed with this process? (y/N)?y
[2024-02-13T19:01:45.130999+00:00] INFO: Starting backup.
[2024-02-13T19:01:45.155039+00:00] NOTICE: Enabling Maintenance mode
[2024-02-13T19:01:46.404427+00:00] INFO: Trying to kill running cron jobs and consumers processes
[2024-02-13T19:01:46.420149+00:00] INFO: Running Magento cron and consumers processes were not found.
[2024-02-13T19:01:46.420434+00:00] INFO: Waiting for lock on db dump.
[2024-02-13T19:01:46.420499+00:00] INFO: Start creation DB dump for main database...
[2024-02-13T19:01:50.697886+00:00] INFO: Finished DB dump for main database, it can be found here: /app/var/dump-main-1707850906.sql.gz
[2024-02-13T19:01:51.628328+00:00] NOTICE: Maintenance mode is disabled.
[2024-02-13T19:01:51.628419+00:00] INFO: Backup completed.
web@mymagento.0:~$ exit
logout
Connection to ssh.us-4.magento.cloud closed.
```

Utilizzare `SFTP` o `rsync` per richiamare l’immagine del database nell’ambiente locale.

L’esempio che segue utilizza `rsync` per estrarre il file in `~/Downloads/db-tutorial` cartella.

```bash
rsync -avrp -e ssh abasrpikfw4123-remote-db-ecpefky--mymagento@ssh.us-4.magento.cloud:/app/var/dump-main-1707850906.sql.gz ~/Downloads/db-tutorial
```

La finestra del terminale fornisce alcune informazioni. Ecco un esempio di output

```bash
rsync -avrp -e ssh abasrpikfw4123-remote-db-ecpefky--mymagento@ssh.us-4.magento.cloud:/app/var/dump-main-1707850906.sql.gz ~/Downloads/db-tutorial
receiving file list ... done
dump-main-1707850906.sql.gz

sent 38 bytes  received 2691041 bytes  358810.53 bytes/sec
total size is 2690241  speedup is 1.00
```

Visualizza il contenuto del file per verificarne il corretto download.

```bash
ls -lah
total 29840
drwxr-xr-x    4 <ussername>  staff   128B Feb 13 13:02 .
drwx------@ 103 <ussername>   staff   3.2K Feb 13 12:52 ..
-rw-r--r--    1 <ussername>   staff    11M Feb 13 12:53 abasrpikfw4123--remote-db-ecpefky--mysql--main--dump.sql
-rw-r--r--    1 <ussername>   staff   2.6M Feb 13 13:01 dump-main-1707850906.sql.gz
```

Una volta che disponi dei dati, assicurati di rimuoverli o mascherarli. Il seguente script di esempio ti aiuterà a iniziare.

In questo esempio i dati del cliente vengono convertiti in stringhe casuali, ma vengono mantenuti tutti gli elementi. Questo esempio contiene alcune tabelle aggiuntive per dimostrare che i dati PII dei clienti si trovano nelle tabelle di terze parti e nelle tabelle principali. Esamina attentamente i dati in ogni tabella e maschera o rimuovi eventuali dati dei clienti.

In genere l’architetto o lo sviluppatore principale è l’unico responsabile del mascheramento e della bonifica delle immagini di database. Disporre di uno strumento sanitario dedicato riduce l&#39;esposizione dei dati non elaborati, riducendo così la possibilità di violare le norme e i regolamenti di conformità.

```sql
SET FOREIGN_KEY_CHECKS=0;
UPDATE customer_entity SET email = REPLACE(email, SUBSTRING(email, LOCATE('@', email) +1), CONCAT(UUID(), '.com'));
UPDATE email_contact SET email = REPLACE(email, SUBSTRING(email, LOCATE('@', email) +1), CONCAT(UUID(), '.com'));
UPDATE sales_invoice_grid SET customer_email = 'customer@example.com', customer_name  = 'Jack Smith';
UPDATE sales_order SET customer_email = 'customer@example.com', customer_firstname = 'Sally', customer_lastname = 'Smith', remote_ip = '127.0.0.1';
UPDATE sales_order_address SET region = 'Ohio', postcode = '12345-1234', lastname = 'Smith', street = '123 Main street', region_id = 44, city = 'Phoenix', telephone = NULL, firstname = 'Jane', company = NULL;
UPDATE sales_order_grid SET customer_email = 'customer@example.com', shipping_name = 'Jack', billing_name = 'Jack Smith', billing_address = '123 Main Street', shipping_address = '321 Pine Street', customer_name = 'Jane Smith';
UPDATE sales_shipment_grid SET customer_email = 'customer@example.com', customer_name = 'Jane Smith', billing_address = '123 Main street', billing_name = 'Jack Doe', shipping_name = 'Susie Smith';
UPDATE quote SET customer_email = 'customer@example.com', customer_firstname = 'Sally', customer_lastname = 'Jones', customer_dob = NULL, remote_ip = '127.0.0.1';
UPDATE quote_address SET email = 'customer@example.com', firstname = 'Jack', lastname = 'Smith', company = NULL, street = '123 Main st', city = 'AnyCity', region = 'Some State', region_id = 44, postcode = '12345-1234', telephone = NULL;
UPDATE magento_rma SET customer_custom_email = 'customer@example.com' WHERE customer_custom_email IS NOT NULL;
UPDATE customer_address_entity SET firstname = 'Jack', lastname = 'Smith', telephone = '909-555-1212', postcode = NULL,  region = NULL, street = '123 Main street', city = 'Anycity', company = NULL;
UPDATE customer_grid_flat SET name = 'Jane Doe', email = 'customer@example.com', dob = NULL, gender = NULL, taxvat = NULL, shipping_full = '', billing_full = '', billing_firstname = 'Jack', billing_lastname = 'Smith', billing_telephone = NULL, billing_postcode = NULL, billing_country_id = NULL, billing_region = NULL, billing_street = '123 Main street', billing_city = 'Anycity', billing_fax = NULL, billing_vat_id = NULL, billing_company = NULL;
UPDATE sales_creditmemo_grid SET billing_name = 'Sally', billing_address = '123 Main Street', customer_name = 'Jack Smith', customer_email = 'customer@example.com';
UPDATE magento_rma_grid SET customer_name = 'Jack Smith';
UPDATE newsletter_subscriber SET subscriber_email = 'customer@example.com';
UPDATE core_config_data SET value = '' WHERE path = 'orderexport/general/serial';
UPDATE core_config_data SET value = '' WHERE path = 'productexport/general/serial';
UPDATE core_config_data SET value = '' WHERE path = 'trackingimport/general/serial';
UPDATE core_config_data SET value = '' WHERE path = 'stockimport/general/serial';
UPDATE core_config_data SET value = '' WHERE path = 'remarketing/onescript/merchant_id';
UPDATE core_config_data SET value = '' WHERE path = 'remarketing/onescript/merchant_id';
UPDATE core_config_data SET value = '' WHERE path = 'algoliasearch_credentials/credentials/application_id';
UPDATE core_config_data SET value = '' WHERE path = 'algoliasearch_credentials/credentials/search_only_api_key';
UPDATE core_config_data SET value = '' WHERE path = 'tax/avatax/production_account_number';
UPDATE core_config_data SET value = '' WHERE path = 'tax/avatax/production_license_key';
UPDATE core_config_data SET value = '' WHERE path = 'design/head/includes';
UPDATE core_config_data SET value = '' WHERE path = 'payment/braintree/merchant_id';
UPDATE core_config_data SET value = '' WHERE path = 'payment/braintree/public_key';     
UPDATE core_config_data SET value = '' WHERE path = 'payment/braintree/private_key';
UPDATE core_config_data SET value = '' WHERE path = 'system/full_page_cache/fastly/fastly_service_id';
UPDATE core_config_data SET value = '' WHERE path = 'system/full_page_cache/fastly/fastly_api_key';
UPDATE core_config_data SET value = '' WHERE path = 'google/analytics/container_id';  
UPDATE core_config_data SET value = '' WHERE path = 'analytics/general/token';
UPDATE vault_payment_token SET public_hash = UUID(), details = '{"type":"VI","maskedCC":"1111","expirationDate":"01\/2019"}';
TRUNCATE customer_log; 
TRUNCATE customer_visitor; 
TRUNCATE magento_logging_event;
TRUNCATE oauth_consumer;
TRUNCATE oauth_nonce;
TRUNCATE oauth_token;
TRUNCATE password_reset_request_event;
TRUNCATE acknowledgement;
TRUNCATE acknowledgement_report;
TRUNCATE avatax_log;
TRUNCATE avatax_queue;
TRUNCATE cron_schedule;
SET FOREIGN_KEY_CHECKS=1;
```

In alternativa, è possibile eliminare i record invece di mascherare le informazioni, riducendo anche le dimensioni del nuovo database. Una volta mascherati o rimossi i dati PII, questi possono essere forniti in modo sicuro a un compagno di squadra per l’utilizzo nel proprio ambiente locale.

## Connessione DB remoto a un progetto Adobe Commerce Cloud

Questo metodo consente di modificare ed eliminare accidentalmente i dati reali. Questo approccio deve essere utilizzato con cautela. L&#39;utilizzo di un backup del database e la revisione dei dati offline è l&#39;approccio preferito. In alcuni casi è necessario accedere ai dati direttamente sul Adobe Commerce Cloud, ma questo comporta dei rischi. Non ci sono &quot;sei sicuro?&quot; domande poste, in modo da poter modificare o rimuovere inavvertitamente i dati.

Super importante! Effettuare una connessione DB remota è comodo e utilizza dati reali in tempo reale, ma comporta dei rischi. Personalmente e in qualità di architetto tecnico principale per Adobe Commerce, non lo consiglio. È troppo facile dimenticare di essere sul database remoto ed eliminare o modificare accidentalmente i dati. È disponibile un&#39;opzione per la connessione alla replica di sola lettura, ma questo offre un certo impatto sul sito a seconda del peso delle attività SQL. Tuttavia, poiché è possibile, questi sono i passaggi per realizzarlo.

Stabilire un tunnel SSH:

```bash
magento-cloud tunnel:open
```

Dopo aver selezionato il progetto e l’ambiente, viene restituito il comando utilizzato nelle impostazioni dell’interfaccia grafica mysql.

```bash
magento-cloud tunnel:open

Enter a number to choose a project:
  [0] demo-ralbin (ral32nryq4123)
  [1] adobe-commerce-demo (abc123zzkipexnqo)
  [2] DX Tutorials - Commerce (abasrpikfw4123)
 > 2

Enter a number to choose an environment:
Default: master
  [0] master (type: production)
  [1] remote-db (type: development)
 > 1

SSH tunnel opened to database at: mysql://user:@127.0.0.1:30000/main
SSH tunnel opened to redis at: redis://127.0.0.1:30001
SSH tunnel opened to opensearch at: http://127.0.0.1:30002
SSH tunnel opened to rabbitmq at: amqp://guest:guest@127.0.0.1:30003

Logs are written to: /Users/<user>/.magento-cloud/tunnels.log

List tunnels with: magento-cloud tunnels
View tunnel details with: magento-cloud tunnel:info
Close tunnels with: magento-cloud tunnel:close

Save encoded tunnel details to the MAGENTO_CLOUD_RELATIONSHIPS variable using:
  export MAGENTO_CLOUD_RELATIONSHIPS="$(magento-cloud tunnel:info --encode)"
```

Stabilire una connessione utilizzando un&#39;interfaccia grafica MySQL utilizzando `SSH tunnel opened to database at` comando.

```bash
SSH tunnel opened to database at: mysql://user:@127.0.0.1:30000/main
```

Ora che disponi delle informazioni corrette, continua a inserire questi valori nella console Cloud.

Puoi trovare il nome host e il nome utente SSH dalle credenziali cloud nella console Cloud.

![logo - Console Adobe Commerce Cloud](./assets/cloud-ui-screenshot.png "Console Adobe Commerce Cloud")

Ecco un esempio: `ssh abasrpikfw4123-remote-db-ecpefky--mymagento@ssh.us-4.magento.cloud`
Il nome host SSH è tutto dopo il segno @: `ssh.us-4.magento.cloud` in questo esempio.
Il nome utente SSH è tutto ciò che precede il segno @:  `abasrpikfw4123-remote-db-ecpefky—mymagento`

## Ricerca dei valori per la connessione al database

Per accedere direttamente al database MariaDB è necessario utilizzare SSH per accedere all&#39;ambiente Cloud remoto e connettersi al database.

1. Utilizza SSH per accedere all’ambiente remoto.

   ```bash
   magento-cloud ssh
   ```

1. Recuperare le credenziali di accesso MySQL da `database` e `type` proprietà in [$MAGENTO_CLOUD_RELATIONSHIPS](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/app/properties/properties.html?lang=en#relationships) variabile.

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   o

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"])));'
   ```

   Nella risposta, trovare le informazioni MySQL. Ad esempio:

   ```json
   "database" : [
      {
         "password" : "",
         "rel" : "mysql",
         "hostname" : "nnnnnnnn.mysql.service._.magentosite.cloud",
         "service" : "mysql",
         "host" : "database.internal",
         "ip" : "###.###.###.###",
         "port" : 3306,
         "path" : "main",
         "cluster" : "projectid-integration-id",
         "query" : {
            "is_master" : true
         },
         "type" : "mysql:10.3",
         "username" : "user",
         "scheme" : "mysql"
      }
   ],
   ```

Quindi utilizzare i valori di configurazione nell&#39;interfaccia utente grafica MySQL. Nell&#39;esempio seguente viene utilizzato MySQL Workbench, ma tutte le app che supportano le connessioni MySQL avranno campi simili.

![logo - Esempio di interfaccia grafica Mysql con Mysql Workbench](./assets/mysql-workbench-after-connecting.png " Esempio di interfaccia grafica di Mysql con Mysql Workbench")

![logo - Esempio di interfaccia grafica Mysql con TablesPlus](./assets/tablesPlus-db-connection.png " Esempio di interfaccia grafica Mysql con TablesPlus")

Dopo aver configurato tutto, è possibile utilizzare una GUI MySQL per eseguire query su un progetto Adobe Commerce Cloud remoto.

## Connessione diretta al database del progetto cloud per eseguire SQL

Il metodo seguente utilizza `magento-cloud` cli per la connessione diretta al database mysql e l&#39;esecuzione di SQL, che consente di eseguire più rapidamente le query sul database. Se è necessario copiare il database, fare riferimento a uno dei metodi alternativi per [creare un dump del database](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/create-database-dump-on-cloud.html).

```bash
magento-cloud db:sql    

Enter a number to choose a project:
  [0] demo-ralbin (ral32nryq4123)
  [1] adobe-commerce-demo (abc123zzkipexnqo)
  [2] DX Tutorials - Commerce (abasrpikfw4123)
 > 2

Enter a number to choose an environment:
Default: master
  [0] master (type: production)
  [1] remote-db (type: development)
 > 1

Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 273454
Server version: 10.6.15-MariaDB-1:10.6.15+maria~deb10-log mariadb.org binary distribution

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
```

Ad esempio, è possibile trovare tutti i record del `core_config_data` tabella che contiene la parola `secure` come parte della colonna `path`:

```sql
MariaDB [main]> SELECT * FROM core_config_data WHERE path LIKE '%secure%' \G;
*************************** 1. row ***************************
 config_id: 5
     scope: default
  scope_id: 0
      path: web/unsecure/base_url
     value: http://remote-db-ecpefky-abasrpikfw4123.us-4.magentosite.cloud/
updated_at: 2024-02-02 18:03:17
*************************** 2. row ***************************
 config_id: 6
     scope: default
  scope_id: 0
      path: web/secure/base_url
     value: https://remote-db-ecpefky-abasrpikfw4123.us-4.magentosite.cloud/
updated_at: 2024-02-02 18:03:17
*************************** 3. row ***************************
 config_id: 8
     scope: default
  scope_id: 0
      path: web/secure/use_in_adminhtml
     value: 1
updated_at: 2023-04-26 19:43:58
3 rows in set (0.001 sec)

ERROR: No query specified

MariaDB [main]> 
```

## Risorse aggiuntive

[CLI ADOBE COMMERCE CLOUD](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/dev-tools/cloud-cli/cloud-cli-overview.html)
[Configura servizio MySQL](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/service/mysql.html)
[Configurare una connessione remota al database MySQL](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/database-server/mysql-remote.html)
[Creazione di un dump del database sull’infrastruttura cloud di Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/create-database-dump-on-cloud.html)
