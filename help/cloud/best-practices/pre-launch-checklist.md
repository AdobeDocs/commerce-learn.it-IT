---
title: Elenco di controllo pre-avvio di Adobe Commerce Cloud
description: Scopri l’elenco di controllo pre-lancio di Adobe Commerce Cloud.
feature: Cloud
topic: Commerce, Architecture, Development
role: Architect, Developer
level: Intermediate
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-04-17T00:00:00Z
jira: KT-15180
kt: 15180
exl-id: c6adb2c2-f194-4a3d-9290-e0837ef062ae
source-git-commit: 00a8d6883473de796abc79ef2e9be34f56429a17
workflow-type: tm+mt
source-wordcount: '1605'
ht-degree: 0%

---

# Commerce Cloud elenco di controllo pre-lancio

Di seguito è riportata una sintesi della [documentazione di lancio del sito](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/launch/overview){target="_blank"} di Adobe Commerce.

Questo elenco di controllo ha lo scopo di facilitare la pianificazione e l’esecuzione del lancio del sito Adobe Commerce Cloud. Collabora con l&#39;integratore di sistemi per Adobe Commerce Cloud per garantire che tutte le attività di configurazione e gli elementi dell&#39;elenco di controllo siano completati e verificati. In caso di difficoltà con qualsiasi voce dell’elenco di controllo o in caso di domande, contatta il Customer Technical Advisor o il Customer Success Engineer dedicato. Se al tuo account non è assegnato un CTA/CSE, puoi creare un ticket di supporto per assistenza.

Se hai assegnato un CTA/CSE all&#39;account, contatta l&#39;account e l&#39;Account Manager almeno 4 settimane prima di avviare il nuovo sito Adobe Commerce Cloud per notificare loro la **intenzione** di avviare.

- Alcuni controlli sono evidenziati con [!BADGE Blocker]{type=caution tooltip="Potenziale bloccante"}
- Assicurati di collaborare con il tuo sviluppatore o partner di integrazione dei sistemi per allinearlo all’approccio di implementazione.

>[!IMPORTANT]
> Se non utilizzi e non completi questo elenco di controllo, accetti la [responsabilità](https://experienceleague.adobe.com/en/docs/commerce-operations/security-and-compliance/shared-responsibility){target="_blank"} per eventuali effetti negativi e rischi associati alla pianificazione dell&#39;avvio della produzione e alla stabilità continua del sito.

## 1. Pre-Go Live

1. Consulta la documentazione sui test e la pubblicazione della [documentazione di lancio del sito](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/launch/overview){target="_blank"}

   >[!NOTE]
   >Assicurati che un _&quot;piano di preparazione alla pubblicazione&quot;_ completo sia stato preparato con il tuo partner o integratore di sistemi, includendo tutte le azioni necessarie. Ricorda che l&#39;elenco di controllo pre-lancio enfatizza le best practice di Adobe, ma _**non**_ sostituisce la necessità del tuo piano di preparazione alla pubblicazione.

2. [!BADGE Blocco]{type=caution tooltip="Potenziale bloccante"}[Guida utente](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/site-wide-analysis-tool/intro){target="_blank"})
3. L’utente finale/esercente ha condotto test di accettazione utente (UAT, User Acceptance Testing), comprese le operazioni back-end.
4. Il team di integratori di sistemi ha eseguito UAT end-to-end su staging e produzione. Consulta la [documentazione di Experience League](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/develop/test/staging-and-production){target="_blank"}.
5. Conferma distribuzione del codice e test negli ambienti di staging e produzione ([Ulteriori informazioni](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/develop/test/staging-and-production){target="_blank"}).
6. Il cluster di produzione è stato ridimensionato in modo permanente alla linea di base giornaliera contrattuale. Parla con il CTA/CSE assegnato per ulteriori dettagli o genera un ticket di supporto.

## 2. Configurazioni correnti

1. Aggiorna Adobe Commerce e i pacchetti/servizi correlati alla [versione più recente](https://experienceleague.adobe.com/en/docs/commerce-operations/release/notes/overview){target="_blank"}
2. Rivedi le configurazioni e i servizi correnti con il tuo SI/Partner e [segui le best practice](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/best-practices/planning/catalog-management){target="_blank"}.
3. Rivedi MySQL/Shared-Files [utilizzo disco](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/develop/storage/manage-disk-space){target="_blank"}

## 3. Configurazioni Fastly

1. [!BADGE Blocco]{type=caution tooltip="Potenziale bloccante"}[Cache a pagina intera](https://developer.adobe.com/commerce/frontend-core/guide/caching/){target="_blank"} o [Memorizzazione in cache di GraphQL](https://developer.adobe.com/commerce/webapi/graphql/usage/caching/){target="_blank"}). Leggi la [Guida alla configurazione rapida](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/cdn/fastly){target="_blank"}.
2. Utilizza il metodo GET per le query GraphQL su siti web PWA/Headless, se applicabile.

   >[!NOTE]
   > È possibile memorizzare in cache solo le query inviate con un’operazione HTTP GET (se applicabile). Impossibile memorizzare nella cache [query POST](https://developer.adobe.com/commerce/webapi/graphql/usage/caching/){target="_blank"}.

3. Assicurati che Ottimizzazione immagine rapida sia abilitato ([Vedi Ottimizzazione immagine rapida](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/cdn/fastly-image-optimization){target="_blank"})
4. Verificare che la posizione dello scudo corretta sia configurata ([Configura cache, backend e schermatura origine](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/cdn/setup-fastly/fastly-custom-cache-configuration){target="_blank"}).
5. Firewall applicazione Web (**WAF**) funziona. (Vedi [Risoluzione dei problemi relativi alle richieste bloccate](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/cdn/fastly-waf-service){target="_blank"}, se presenti, e limitazioni)
6. Aggiorna l&#39;elenco Fastly [&quot;Ignored URL Parameters&quot;](https://github.com/iancassidyweb/magento2/commit/68fdecfcd26c957382b8d68b64887e0a83298524){target="_blank"} nel pannello di amministrazione per migliorare le prestazioni della cache.

   >[!NOTE]
   > Nella configurazione Fastly in _Amministratore > Archivi > Configurazioni > Sistema > Cache a pagina intera > Configurazione rapida > Configurazione avanzata > Parametri URL ignorati (globali)_, è possibile trovare un elenco separato da virgole di parametri che Fastly deve ignorare durante la ricerca di pagine memorizzate in cache. Ricarica il file VCL dopo aver modificato l&#39;elenco

## 4. DNS e SSL

1. [!BADGE Blocco]{type=caution tooltip="Potenziale bloccante"}_(Inviare in anticipo un ticket di supporto per tutti i domini aggiunti o modificati)_
2. [!BADGE Blocco]{type=caution tooltip="Potenziale bloccante"}[questo articolo](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/how-to/ssl-tls-certificates-for-magento-commerce-cloud-faq){target="_blank"} per ulteriori informazioni.
3. Aggiorna il valore [TTL (Time to Live)](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/launch/checklist#to-update-dns-configuration-for-site-launch){target="_blank"} DNS al minimo possibile per il lancio.
4. Abilita Sendgrid SPF e DKIM

   >[!NOTE]
   > Aggiungere i record CNAME SendGrid per ogni dominio alla configurazione DNS. Leggi [Servizio e-mail SendGrid](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/project/sendgrid){target="_blank"} per scoprire come modificare i domini del mittente e altro ancora.

## 5. Configurazioni del database

Adobe Commerce Cloud utilizza un cluster MariaDB Galera come database sia per gli ambienti di staging che per quelli di produzione. I cluster Galera sono fondamentali per migliorare le prestazioni e la scalabilità. Per informazioni sulle pratiche ottimali e sui vincoli delle repliche dei cluster Galera, consulta i seguenti articoli.

- [Best practice per le configurazioni MySQL](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/best-practices/planning/mysql-configuration){target="_blank"}
- Avvisi gestiti su Adobe Commerce: [Avvisi MariaDB](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-on-magento-commerce-mariadb-alerts){target="_blank"}
- Best practice per la [configurazione del database](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/best-practices/planning/database-on-cloud){target="_blank"}
- Analisi approfondita per [repliche cluster Galera e controllo del flusso.](https://experienceleague.adobe.com/en/docs/commerce-learn/tutorials/backend-development/galera-db-slow-replication){target="_blank"}

1. [La connessione slave MYSQL](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/best-practices/planning/mysql-configuration#slave-connections){target="_blank"} è consigliata per migliorare le prestazioni durante carichi di database elevati.
2. Verificare che il formato della riga per tutte le tabelle del database sia impostato su [DYNAMIC invece di COMPACT](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/best-practices/maintenance/mariadb-upgrade#convert-database-table-storage-format){target="_blank"} (in particolare per le migrazioni on-premise al cloud).
3. Modificare il motore di archiviazione del database da [MyISAM a InnoDB](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/best-practices/planning/database-on-cloud#convert-all-myisam-tables-to-innodb){target="_blank"} per tutte le tabelle.
4. Rivedi e ottimizza le tabelle del database di dimensioni superiori a 1 GB con largo anticipo.
5. Le informazioni sullo schema del database sono aggiornate. (Consulta [questa guida](https://mariadb.com/kb/en/engine-independent-table-statistics/#collecting-statistics-with-the-analyze-table-statement){target="_blank"}).

## 6. Distribuzioni

1. Rivedi lo stato ideale dell’implementazione di contenuti statici (SCD) per ridurre i tempi di manutenzione durante le distribuzioni nell’ambiente di produzione. Rivedi [Strategie di distribuzione del contenuto statico](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/develop/deploy/static-content){target="_blank"} e [Guida alla gestione della configurazione dell&#39;archivio](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/configure-store/store-settings){target="_blank"}.
2. Rivedi le impostazioni di minimizzazione per HTML, JavaScript e CSS. (Questo non si applica ai siti web PWA/Headless).
3. Conferma che l’utilizzo delle seguenti variabili cloud sia in linea con le finalità previste. ([SCD_MATRIX](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/configure/env/stage/variables-build#scd_matrix){target="_blank"}, [SCD_ON_DEMAND](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/configure/env/stage/variables-global#scd_on_demand){target="_blank"} e [SKIP_SCD](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/configure/env/stage/variables-deploy#skip_scd){target="_blank"})

## 7. Test e risoluzione dei problemi

1. Verifica le e-mail transazionali in uscita. Ulteriori informazioni sulla funzionalità [Adobe Commerce Cloud - SendGrid Mail](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/project/sendgrid){target="_blank"}.
2. [!BADGE Blocco]{type=caution tooltip="Potenziale bloccante"}
3. [!BADGE Blocco]{type=caution tooltip="Potenziale bloccante"}

   >[!NOTE]
   > Un [test di carico e stress serve allo scopo](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/develop/test/guidance#:~:text=A%20load%20test%20can%20help,Scan%20Tool%20for%20your%20sites.){target="_blank"} di identificare i colli di bottiglia e individuare i problemi di prestazioni all&#39;interno dell&#39;applicazione. Svolge un ruolo cruciale nella gestione delle aspettative relative alle dimensioni dei cluster e nella determinazione degli adeguamenti di scalabilità necessari per soddisfare i requisiti aziendali in modo efficace.

   >[!IMPORTANT]
   > **_AVVISO:_** Durante la preparazione di un test di carico,_ **_non_** inviare e-mail di transazione live (anche a indirizzi fittizi). L’invio di e-mail durante il test può far raggiungere al progetto il limite di invio predefinito (12k) configurato per SendGrid prima dell’avvio.
   > 
   > - Come disattivare la comunicazione e-mail:
   >   Vai a _Store > Configurazione > Avanzate > Sistema > Impostazioni invio e-mail_.

4. Eseguire test di penetrazione della sicurezza nell&#39;istanza di produzione come parte del [modello di sicurezza con responsabilità condivisa](https://business.adobe.com/products/magento/secure-ecommerce.html){target="_blank"}. Per la conformità PCI (Payment Card Industry), il sito personalizzato richiede test di penetrazione.

## 8. Altre configurazioni

1. Passa all&#39;indicizzazione a _&quot;aggiornamento in base alla pianificazione_&quot;, ad eccezione di **_customer_grid_** che rimane in &quot;SAVE&quot; (vedere [Modalità di indicizzazione](https://developer.adobe.com/commerce/php/development/components/indexing/#indexing-modes){target="_blank"}).
2. Stai utilizzando motori di ricerca o estensioni di terze parti?
3. Verificare che le configurazioni [SEO (Search Engine Optimization) siano configurate correttamente](https://experienceleague.adobe.com/en/docs/commerce-admin/marketing/seo/seo-overview){target="_blank"} per consentire agli indicizzatori/crawler di eseguire la scansione del sito Web, se necessario.
4. Aggiungere reindirizzamenti e route (vedere [Configurare le route](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/configure/routes/routes-yaml){target="_blank"})

   >[!NOTE]
   >Aggiungi reindirizzamenti e route al file route.yaml nell&#39;ambiente di integrazione e verifica la configurazione in questo ambiente prima di distribuirlo nell&#39;ambiente di staging e produzione.

       &quot;http://{all}/&quot;:
       tipo: upstream
       a monte: &quot;mymagento:http&quot;
       
       &quot;http://{all}/&quot;:
       tipo: a monte
       a monte: &quot;mymagento:http&quot;
   
5. Verificare che XDebug sia disabilitato se abilitato durante lo sviluppo (vedere [Configurare Xdebug](https://developer.adobe.com/commerce/cloud-tools/docker/test/configure-xdebug/){target="_blank"}).
6. Verificare che op-cache e altre configurazioni siano state aggiornate con precisione nel file php.ini ([fare riferimento a questo esempio](https://github.com/magento/magento-cloud/blob/master/php.ini#L41){target="_blank"}).
7. Iscriviti alla [**pagina di stato di Adobe Commerce**](https://status.adobe.com/cloud/experience_cloud#/){target="_blank"}.
8. Sottoscrivi i canali di notifica New Relic &quot;[Managed Alerts for Adobe Commerce](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-for-magento-commerce){target="_blank"}&quot; per monitorare le metriche delle prestazioni specificate ([ulteriori informazioni](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/monitor/new-relic/new-relic-service){target="_blank"}).

## 9. Sicurezza

1. Configurare Adobe Commerce Security Scan

   >[!NOTE]
   > [Adobe Commerce Security Scan è uno strumento utile](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/security/security-scan){target="_blank"} che consente di individuare le versioni del software obsolete, la configurazione non corretta e il malware potenziale sul sito. Registrati, pianifica l’esecuzione frequente e assicurati che le e-mail vengano inviate al contatto di sicurezza tecnico corretto.
   > 
   > Completa questa attività durante UAT. Se si utilizza l&#39;opzione Scansioni periodiche, assicurarsi di pianificare le scansioni a bassa richiesta. Visita la pagina [Analisi protezione](https://account.magento.com/scanner/index/dashboard/){target="_blank"} nell&#39;account Adobe Commerce. Per accedere a Security Scan, devi accedere a un account Adobe Commerce.

2. Modifica le impostazioni predefinite per l’amministratore di Adobe Commerce.
3. Modificare la password amministratore (vedere [Configurazione di Admin Security](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/security/security-admin){target="_blank"}).
4. Modificare l&#39;URL amministratore (vedere [Utilizzo di un URL amministratore personalizzato](https://experienceleague.adobe.com/en/docs/commerce-admin/stores-sales/site-store/store-urls#use-a-custom-admin-url){target="_blank"}).
5. Rimuovere gli utenti che non fanno più parte del progetto (vedere [Creare e gestire gli utenti](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/project/user-access){target="_blank"}).
6. Le password per gli amministratori sono configurate (vedi [Requisiti password amministratore](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/security/security-admin){target="_blank"}).
7. Configurare l&#39;autenticazione a due fattori (vedere [Autenticazione a due fattori](https://developer.adobe.com/commerce/testing/functional-testing-framework/two-factor-authentication/){target="_blank"}).

## 10. Lancio

Quando è il momento di passare al cutover, esegui i seguenti passaggi (per ulteriori informazioni, consulta [Configurazioni DNS](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/launch/checklist){target="_blank"}):

1. Accedi al servizio DNS e aggiorna i record A e CNAME per ciascuno dei tuoi domini e nomi host:
   1. Aggiungi un record CNAME per _&lt;&lt;www.yourdomain.com>_, che punta a **prod.magentocloud.map.fastly.net**
   2. Impostare quattro record A per _&lt;&lt;yourdomain.com>>_, puntando a:\
      151 101 124\
      151 101 65 124\
      151 101 129 124\
      151 101 193 124
2. Cambia l&#39;URL di base di Adobe Commerce in _&lt;&lt;www.yourdomain.com>>_
3. Attendi che il tempo TTL passi, quindi riavvia il browser web.
4. Verifica il sito web.

### Se hai un problema che blocca il lancio:

In caso di problemi che impediscono l&#39;avvio durante il passaggio, il metodo più veloce per ottenere un supporto tempestivo appropriato consiste nell&#39;utilizzare l&#39;help desk e aprire un ticket con il motivo &quot;Impossibile avviare il mio store&quot; e chiamare un numero di assistenza della hotline (vedere [l&#39;elenco dei numeri della hotline di Adobe Commerce P1 (Priorità 1)](https://support.magento.com/hc/en-us/articles/360042536151){target="_blank"}):

- Numero verde USA: (+1) 877 282 7436 (direttamente alla hotline Adobe Commerce P1)
- Numero verde USA: (+1) 800 685 3620 (nel primo menu, premere 7 per la hotline Adobe Commerce P1)
- Locale USA: (+1) 408 537 8777

## 11. Post Go-Live

Una volta che il sito è attivo, invia un’e-mail al CTA (Customer Technical Advisory), al CSE (Customer Success Engineer) e all’AM (Account Manager) assegnati. Tuttavia, se al progetto non è stato assegnato un account manager, è possibile creare un ticket di supporto per richiedere l&#39;attivazione del monitoraggio SLA elevato dopo la pubblicazione del sito. Non appena il sito viene verificato per essere avviato con Fastly abilitato e la memorizzazione in cache, il CTA/CSE esegue le seguenti attività:

- Assegnare i tag al cluster come live e creare un ticket di supporto per attivare il monitoraggio SLA (Service Level Agreement) elevato.
- Attiva New Relic Synthetics per il monitoraggio dei tempi di attività.

>[!MORELIKETHIS]
> 
> - [Panoramica preparazione avvio - Playbook di implementazione](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/launch/overview){target="_blank"}
> - [Elenco di controllo avvio - Guida utente](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/launch/checklist){target="_blank"}
> - [Elenco di controllo preliminare - Guida dell&#39;amministratore di Commerce/Gestione siti](https://experienceleague.adobe.com/en/docs/commerce-admin/start/setup/prelaunch-checklist){target="_blank"}
> - [Modello di sicurezza con responsabilità condivisa](https://experienceleague.adobe.com/en/docs/commerce-operations/security-and-compliance/shared-responsibility){target="_blank"}
