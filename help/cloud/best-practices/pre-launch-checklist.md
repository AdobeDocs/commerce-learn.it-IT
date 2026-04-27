---
title: Elenco di controllo pre-avvio di Adobe Commerce Cloud
description: Scopri l’elenco di controllo pre-avvio di Adobe Commerce Cloud e come utilizzarlo con il tuo integratore prima del lancio.
feature: Cloud
topic: Commerce, Architecture, Development
role: Admin, Developer, User
level: Intermediate
doc-type: Tutorial
duration: 451
last-substantial-update: 2024-04-17T00:00:00.000Z
jira: KT-15180
exl-id: c6adb2c2-f194-4a3d-9290-e0837ef062ae
TQID: https://experienceleague.adobe.com/czbb8zkX55fzgKiZthAj4whBCF-IL2bEox0M7rDr9oE
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
subfeature_v2:
  - id: f8ddfd3b-6194-46e8-a176-0e918039be56
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: bce87dde-a4ab-44c9-8a18-ad66e4ddb377
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
  - id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
  - id: e1e0219c-f879-479f-8427-888ed2a6e9c2
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 2365
ht-degree: 0%

---

# Elenco di controllo pre-avvio

In questa pagina è disponibile un riepilogo della [documentazione di lancio del sito](https://experienceleague.adobe.com/it/docs/commerce-on-cloud/user-guide/launch/overview){target="_blank"} di Adobe Commerce.

Questo elenco di controllo ha lo scopo di facilitare la pianificazione e l’esecuzione del lancio del sito Adobe Commerce Cloud. Collabora con l’integratore di sistemi per Adobe Commerce Cloud per garantire che tutte le attività di configurazione e gli elementi dell’elenco di controllo siano completati e verificati. In caso di difficoltà con qualsiasi voce dell’elenco di controllo o in caso di domande, contatta il Customer Technical Advisor o il Customer Success Engineer dedicato. Se al tuo account non è assegnato un CTA/CSE, puoi creare un ticket di supporto per assistenza.

Se all&#39;account è stato assegnato un CTA/CSE, contatta l&#39;account e l&#39;Account Manager almeno 4 settimane prima di avviare il nuovo sito Adobe Commerce Cloud per notificare l&#39;**intenzione** di avviare.

* Alcuni controlli sono evidenziati con [!BADGE Blocker]{type=caution tooltip="Potential Blocker"} in quanto potrebbero bloccare il lancio se non vengono esaminati attentamente.
* Collabora con il tuo sviluppatore o con il partner di integrazione dei sistemi in modo che il tuo approccio di implementazione rimanga allineato.

>[!IMPORTANT]
> Se non utilizzi e non completi questo elenco di controllo, accetti [responsabilità](https://experienceleague.adobe.com/it/docs/commerce-operations/security-and-compliance/shared-responsibility){target="_blank"} per eventuali effetti negativi e rischi associati alla pianificazione dell&#39;avvio della produzione e alla stabilità continua del sito.

## 1. Pre-Go Live

1. Review the documentation about testing and going live [Site launch documentation](https://experienceleague.adobe.com/it/docs/commerce-on-cloud/user-guide/launch/overview){target="_blank"}

   >[!NOTE]
   >Ensure a comprehensive _&quot;go live readiness plan&quot;_ is fully prepared with your partner or system integrator, incorporating all necessary action items. Remember, while the pre-launch checklist emphasizes Adobe&#39;s best practices, it _&#x200B;**does not**&#x200B;_ replace the need for your own go-live readiness plan.

2. [!BADGE Blocker]{type=caution tooltip="Potential Blocker"} Review the Support Insights (SWAT) Recommendations and Information ([User Guide](https://experienceleague.adobe.com/it/docs/commerce-operations/tools/site-wide-analysis-tool/intro){target="_blank"})
3. Confirm end users and merchants have completed UAT (User Acceptance Testing), including backend operations.
4. Confirm the system integrator team has performed end-to-end UAT on staging and production. Refer to the [Experience League Documentation](https://experienceleague.adobe.com/it/docs/commerce-on-cloud/user-guide/develop/test/staging-and-production){target="_blank"}.
5. Confirm code deployment and testing in staging and production environments ([Read more](https://experienceleague.adobe.com/it/docs/commerce-on-cloud/user-guide/develop/test/staging-and-production){target="_blank"}).
6. Confirm the production cluster has been up-sized permanently to the contracted daily baseline. Speak to the assigned CTA/CSE for more details, or raise a support ticket.

## 2. Current Configurations

1. Upgrade Adobe Commerce and related packages/services to the [latest version](https://experienceleague.adobe.com/it/docs/commerce-operations/release/notes/overview){target="_blank"}
2. Review the current configurations and services with your SI/Partner, and [follow the best practices](https://experienceleague.adobe.com/it/docs/commerce-operations/implementation-playbook/best-practices/planning/catalog-management){target="_blank"}.
3. Review the MySQL/Shared-Files [disk usage](https://experienceleague.adobe.com/it/docs/commerce-on-cloud/user-guide/develop/storage/manage-disk-space){target="_blank"}

## 3. Fastly Configurations

1. [!BADGE Blocker]{type=caution tooltip="Potential Blocker"} Make sure that caching is working ([Full-Page Cache](https://experienceleague.adobe.com/it/docs/commerce-admin/systems/tools/cache-management){target="_blank"} or [GraphQL caching](https://developer.adobe.com/commerce/webapi/graphql/usage/caching/){target="_blank"}). Read the [Fastly set up guide](https://experienceleague.adobe.com/it/docs/commerce-on-cloud/user-guide/cdn/fastly){target="_blank"}.
2. Use the GET method for GraphQL queries on PWA/Headless websites when applicable.

   >[!NOTE]
   > Only the queries submitted with an HTTP GET operation can be cached (if applicable). [POST queries cannot be cached](https://developer.adobe.com/commerce/webapi/graphql/usage/caching/){target="_blank"}.

3. Ensure that Fastly Image Optimization is enabled ([See Fastly Image Optimization](https://experienceleague.adobe.com/it/docs/commerce-on-cloud/user-guide/cdn/fastly-image-optimization){target="_blank"})
4. Verify that the correct shield location is configured ([Configure cache, backends and origin shielding](https://experienceleague.adobe.com/it/docs/commerce-on-cloud/user-guide/cdn/setup-fastly/fastly-custom-cache-configuration){target="_blank"}).
5. Confirm the Web Application Firewall (**WAF**) is working. (See [Troubleshooting blocked requests](https://experienceleague.adobe.com/it/docs/commerce-on-cloud/user-guide/cdn/fastly-waf-service){target="_blank"}, if any, and limitations.)
6. Aggiorna l&#39;elenco Fastly [&quot;Ignored URL Parameters&quot;](https://github.com/iancassidyweb/magento2/commit/68fdecfcd26c957382b8d68b64887e0a83298524){target="_blank"} nel pannello di amministrazione per migliorare le prestazioni della cache.

   >[!NOTE]
   > Nella configurazione Fastly in _Amministratore > Archivi > Configurazioni > Sistema > Cache a pagina intera > Configurazione rapida > Configurazione avanzata > Parametri URL ignorati (globali)_, è possibile trovare un elenco separato da virgole di parametri che Fastly deve ignorare durante la ricerca di pagine memorizzate in cache. Ricaricate il file VCL dopo aver modificato l&#39;elenco.

## &#x200B;4. DNS e SSL

1. [!BADGE Blocco]{type=caution tooltip="Potential Blocker"} Verificare che tutti i nomi di dominio richiesti siano stati richiesti. _(Inviare in anticipo un ticket di supporto per tutti i domini aggiunti o modificati)_
2. [!BADGE Il certificato SSL (TLS) di blocco]{type=caution tooltip="Potential Blocker"} è stato applicato ai domini. Leggi [questo articolo](https://experienceleague.adobe.com/it/docs/commerce-knowledge-base/kb/how-to/ssl-tls-certificates-for-magento-commerce-cloud-faq){target="_blank"} per ulteriori informazioni.
3. Aggiorna il valore [TTL (Time to Live)](https://experienceleague.adobe.com/it/docs/commerce-on-cloud/user-guide/launch/checklist){target="_blank"} DNS al minimo possibile per il lancio.
4. Abilitare SendGrid SPF e DKIM.

   >[!NOTE]
   > Aggiungere i record CNAME SendGrid per ogni dominio alla configurazione DNS. Leggi [SendGrid email service](https://experienceleague.adobe.com/it/docs/commerce-on-cloud/user-guide/project/sendgrid){target="_blank"} per scoprire come modificare i domini del mittente e altro ancora.

## &#x200B;5. Configurazioni del database

Adobe Commerce Cloud utilizza un cluster MariaDB Galera come database per gli ambienti di staging e produzione. I cluster Galera sono fondamentali per migliorare le prestazioni e la scalabilità. Per informazioni sulle pratiche ottimali e sui vincoli delle repliche dei cluster Galera, consulta i seguenti articoli.

* [Best practice per le configurazioni MySQL](https://experienceleague.adobe.com/it/docs/commerce-operations/implementation-playbook/best-practices/planning/mysql-configuration){target="_blank"}
* Avvisi gestiti su Adobe Commerce: [Avvisi MariaDB](https://experienceleague.adobe.com/it/docs/commerce-operations/tools/managed-alerts-for-adobe-commerce/managed-alerts-on-magento-commerce-mariadb-alerts){target="_blank"}
* Best practice per la [configurazione del database](https://experienceleague.adobe.com/it/docs/commerce-operations/implementation-playbook/best-practices/planning/database-on-cloud){target="_blank"}
* [Replica cluster Galera e controllo del flusso](https://experienceleague.adobe.com/it/docs/commerce-learn/tutorials/extensibility/backend-development/galera-db-slow-replication){target="_blank"} (analisi approfondita)

1. [La connessione slave MySQL](https://experienceleague.adobe.com/it/docs/commerce-operations/implementation-playbook/best-practices/planning/mysql-configuration#slave-connections){target="_blank"} è consigliata per migliorare le prestazioni durante carichi di database elevati.
2. Verificare che il formato della riga per tutte le tabelle del database sia impostato su [DYNAMIC anziché COMPACT](https://experienceleague.adobe.com/it/docs/commerce-operations/implementation-playbook/best-practices/maintenance/mariadb-upgrade#convert-database-table-storage-format){target="_blank"} (in particolare per le migrazioni on-premise al cloud).
3. Modificare il motore di archiviazione del database da [MyISAM a InnoDB](https://experienceleague.adobe.com/it/docs/commerce-operations/implementation-playbook/best-practices/planning/database-on-cloud#convert-all-myisam-tables-to-innodb){target="_blank"} per tutte le tabelle.
4. Rivedi e ottimizza le tabelle del database di dimensioni superiori a 1 GB con largo anticipo.
5. The database schema information is current and up to date. (Refer to [this guide](https://mariadb.com/docs/server/ha-and-performance/optimization-and-tuning/query-optimizations/statistics-for-optimizing-queries/engine-independent-table-statistics#collecting-statistics-with-the-analyze-table-statement){target="_blank"}).

## 6. Deployments

1. Review the Static Content Deployment (SCD) ideal state to reduce maintenance time during deployments on the production environment. Review [Static Content Deployment (SCD) Strategies](https://experienceleague.adobe.com/it/docs/commerce-on-cloud/user-guide/develop/deploy/static-content){target="_blank"} and [Store configuration management](https://experienceleague.adobe.com/it/docs/commerce-on-cloud/user-guide/configure-store/store-settings){target="_blank"} guide.
2. Review minification settings for HTML, JavaScript, and CSS. (This does not apply to PWA/Headless websites).
3. Confirm that the utilization of the following cloud variables aligns with their intended purposes. ([SCD_MATRIX](https://experienceleague.adobe.com/it/docs/commerce-on-cloud/user-guide/configure/env/stage/variables-build#scd_matrix){target="_blank"}, [SCD_ON_DEMAND](https://experienceleague.adobe.com/it/docs/commerce-on-cloud/user-guide/configure/env/stage/variables-global#scd_on_demand){target="_blank"} and [SKIP_SCD](https://experienceleague.adobe.com/it/docs/commerce-on-cloud/user-guide/configure/env/stage/variables-deploy#skip_scd){target="_blank"})

## 7. Testing and Troubleshooting

1. Test the outgoing transactional emails. Read more about [Adobe Commerce Cloud - SendGrid Mail functionality](https://experienceleague.adobe.com/it/docs/commerce-on-cloud/user-guide/project/sendgrid){target="_blank"}.
2. [!BADGE Blocker]{type=caution tooltip="Potenziale bloccante"} Confirm there are no Adobe-related blockers to launch.
3. [!BADGE Blocker]{type=caution tooltip="Potenziale bloccante"} Perform load and stress testing on the Production instance before going live and share results with the assigned CTA/CSE.

   >[!NOTE]
   > A [load and stress test serves the purpose](https://experienceleague.adobe.com/it/docs/commerce-on-cloud/user-guide/develop/test/guidance#:~:text=A%20load%20test%20can%20help,Scan%20Tool%20for%20your%20sites.){target="_blank"} of identifying bottlenecks and uncovering performance issues within the application. It plays a crucial role in managing expectations regarding cluster size and determining the necessary scaling adjustments to meet the business requirements effectively.

   >[!IMPORTANT]
   > **_WARNING:_** When preparing a load test, **do not** send live transaction emails (even to dummy addresses). Sending emails during testing can cause the project to reach the default send limit (12k) configured for SendGrid prior to launch.
   > 
   > * How to disable email communication:
   >   Go to _Store > Configuration > Advanced > System > Email Sending Settings_.

4. Conduct security penetration testing on the production instance as part of the [shared responsibility security model](https://business.adobe.com/it/products/commerce.html){target="_blank"}. For PCI (Payment Card Industry) compliance, the customized site requires penetration testing.

## 8. Other Configurations

1. Switch indexing to _&quot;update on schedule_&quot;, except the **_customer_grid_** which remains on &quot;SAVE&quot; (see [Indexing modes](https://developer.adobe.com/commerce/php/development/components/indexing/#indexing-modes){target="_blank"}).
2. Document any third-party search engines or extensions in use.
3. Verificare che le configurazioni [SEO (Search Engine Optimization) siano configurate correttamente](https://experienceleague.adobe.com/it/docs/commerce-admin/marketing/seo/seo-overview){target="_blank"} per consentire a indicizzatori/crawler di analizzare il sito Web, se necessario.
4. Aggiungere reindirizzamenti e route (vedere [Configurare le route](https://experienceleague.adobe.com/it/docs/commerce-on-cloud/user-guide/configure/routes/routes-yaml){target="_blank"})

   >[!NOTE]
   > Aggiungi reindirizzamenti e route al file route.yaml nell&#39;ambiente di integrazione e verifica la configurazione in questo ambiente prima di distribuirlo nell&#39;ambiente di staging e produzione.

   Esempio di frammento `routes.yaml`:

   ```yaml
           "http://{all}/":
               type: upstream
               upstream: "mymagento:http"
   
           "http://{all}/":
               type: upstream
               upstream: "mymagento:http"
   ```

5. Verificare che Xdebug sia disabilitato se è stato abilitato durante lo sviluppo (vedere [Configurare Xdebug per Commerce su Cloud](https://experienceleague.adobe.com/it/docs/commerce-on-cloud/user-guide/develop/test/debug){target="_blank"}).
6. Verificare che OPcache e altre configurazioni siano state aggiornate con precisione nel file php.ini ([fare riferimento a questo esempio](https://github.com/magento/magento-cloud/blob/master/php.ini#L41){target="_blank"}).
7. Iscriviti alla [**pagina di stato di Adobe Commerce**](https://status.adobe.com/it-it/cloud/experience_cloud#/){target="_blank"}.
8. Sottoscrivi i canali di notifica New Relic &quot;[Managed Alerts for Adobe Commerce](https://experienceleague.adobe.com/it/docs/commerce-operations/tools/managed-alerts-for-adobe-commerce/managed-alerts-for-magento-commerce){target="_blank"}&quot; per monitorare le metriche delle prestazioni specificate ([ulteriori informazioni](https://experienceleague.adobe.com/it/docs/commerce-on-cloud/user-guide/monitor/new-relic/new-relic-service){target="_blank"}).

## &#x200B;9. Sicurezza

1. Imposta Adobe Commerce Security Scan.

   >[!NOTE]
   > [Adobe Commerce Security Scan è uno strumento utile](https://experienceleague.adobe.com/it/docs/commerce-admin/systems/security/security-scan){target="_blank"} che consente di individuare le versioni del software obsolete, la configurazione non corretta e il malware potenziale sul sito. Registrati, pianifica l’esecuzione frequente e assicurati che le e-mail vengano inviate al contatto di sicurezza tecnico corretto.
   > 
   > Completa questa attività durante UAT. Se si utilizza l&#39;opzione Scansioni periodiche, assicurarsi di pianificare le scansioni a bassa richiesta. Dopo aver effettuato l&#39;accesso al tuo account Adobe Commerce, apri lo strumento Security Scan dal tuo account (consulta [Security Scan](https://experienceleague.adobe.com/it/docs/commerce-admin/systems/security/security-scan){target="_blank"} per informazioni sull&#39;accesso e l&#39;utilizzo).

2. Modifica le impostazioni predefinite per l’amministratore di Adobe Commerce.
3. Modificare la password amministratore (vedere [Configurazione di Admin Security](https://experienceleague.adobe.com/it/docs/commerce-admin/systems/security/security-admin){target="_blank"}).
4. Modificare l&#39;URL amministratore (vedere [Utilizzo di un URL amministratore personalizzato](https://experienceleague.adobe.com/it/docs/commerce-admin/stores-sales/site-store/store-urls#use-a-custom-admin-url){target="_blank"}).
5. Rimuovere gli utenti non più inclusi nel progetto (vedere [Creare e gestire gli utenti](https://experienceleague.adobe.com/it/docs/commerce-on-cloud/user-guide/project/user-access){target="_blank"}).
6. Verificare che le password dell&#39;amministratore soddisfino i requisiti (vedere [Requisiti password amministratore](https://experienceleague.adobe.com/it/docs/commerce-admin/systems/security/security-admin){target="_blank"}).
7. Configurare l&#39;autenticazione a due fattori (vedere [Autenticazione a due fattori (amministratore)](https://experienceleague.adobe.com/it/docs/commerce-admin/systems/security/security-admin#two-factor-authentication){target="_blank"}).

## &#x200B;10. Vai in diretta

Quando è il momento di passare al cutover, esegui i seguenti passaggi (per ulteriori informazioni, consulta [Configurazioni DNS](https://experienceleague.adobe.com/it/docs/commerce-on-cloud/user-guide/launch/checklist){target="_blank"}):

1. Accedi al servizio DNS e aggiorna i record A e CNAME per ciascuno dei tuoi domini e nomi host:
   1. Aggiungi un record CNAME per _&lt;&lt;www.yourdomain.com>>_, puntando a **prod.magentocloud.map.fastly.net**
   2. Imposta quattro record A per _&lt;&lt;yourdomain.com>>_, puntando a:\
      151.101.1.124\
      151.101.65.124\
      151.101.129.124\
      151.101.193.124
2. Modifica l’URL di base di Adobe Commerce in _&lt;&lt;www.yourdomain.com>>_
3. Attendi che il tempo TTL passi, quindi riavvia il browser web.
4. Verifica il sito web.

### Se hai un problema che blocca il lancio:

Se si verificano problemi che impediscono l&#39;avvio del sistema durante il passaggio al nuovo sistema, il modo più rapido per ottenere assistenza tempestiva consiste nell&#39;utilizzare l&#39;help desk e aprire un ticket con il motivo &quot;Impossibile avviare il mio negozio&quot; e chiamare un numero di assistenza della hotline (per i numeri e le procedure correnti, vedere [Hotline per le notifiche P1 di Adobe Commerce](https://experienceleague.adobe.com/it/docs/support-resources/adobe-support-tools-guide/adobe-commerce-support/adobe-commerce-p1-notification-hotline){target="_blank"}):

* Numero verde USA: (+1) 877 282 7436 (direttamente alla hotline Adobe Commerce P1)
* Numero verde USA: (+1) 800 685 3620 (nel primo menu, premere 7 per la hotline Adobe Commerce P1)
* Locale USA: (+1) 408 537 8777

## &#x200B;11. Post-pubblicazione

Una volta che il sito è attivo, invia un’e-mail a CTA (Customer Technical Advisor), CSE (Customer Success Engineer) e AM (Account Manager) assegnati. Tuttavia, se non hai un account manager assegnato al progetto, puoi creare un ticket di supporto per richiedere l’abilitazione del monitoraggio High SLA dopo la pubblicazione del sito. Non appena il sito viene verificato per essere avviato con Fastly abilitato e la memorizzazione in cache, CTA/CSE esegue le seguenti attività:

* Assegnare i tag al cluster come live e creare un ticket di supporto per attivare il monitoraggio High SLA (Service Level Agreement).
* Attiva New Relic Synthetics per il monitoraggio dei tempi di attività.

>[!MORELIKETHIS]
> 
> * [Panoramica preparazione avvio - Playbook di implementazione](https://experienceleague.adobe.com/it/docs/commerce-operations/implementation-playbook/best-practices/launch/overview){target="_blank"}
> * [Elenco di controllo avvio - Guida utente](https://experienceleague.adobe.com/it/docs/commerce-on-cloud/user-guide/launch/checklist){target="_blank"}
> * [Elenco di controllo preliminare - Guida dell&#39;amministratore di Commerce/Gestione siti](https://experienceleague.adobe.com/it/docs/commerce-admin/start/setup/prelaunch-checklist){target="_blank"}
> * [Modello di sicurezza con responsabilità condivisa](https://experienceleague.adobe.com/it/docs/commerce-operations/security-and-compliance/shared-responsibility){target="_blank"}
