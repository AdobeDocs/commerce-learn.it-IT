---
title: Rilevare indirizzi IP
description: Scopri come rilevare gli indirizzi IP per gli ambienti Adobe Commerce Cloud per migliorare la sicurezza e semplificare la comunicazione con il server
feature: Cloud, Configuration
topic: Commerce, Development, Integrations
role: Developer
level: Beginner
doc-type: Technical Video
duration: 0
last-substantial-update: 2025-04-07T00:00:00Z
jira: KT-17553
exl-id: beb0a6e1-e6b1-4ec0-976c-77a22a27e8a2
source-git-commit: 3acec65129773a8ba94eb52c53d15d7633440717
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# Rilevare indirizzi IP per ambienti diversi

Scopri come rilevare gli indirizzi IP per diversi ambienti in un progetto Adobe Commerce Cloud. Utilizzando una serie di comandi, tra cui Adobe Commerce CLI, sed, xargs, dig, grep e sort -u, gli utenti possono identificare gli indirizzi IP per gli ambienti di sviluppo, staging e produzione.

## A chi serve questo video?

* Sviluppatori: cercando di capire come raccogliere gli indirizzi IP per il progetto Adobe Commerce Cloud.
* DevOps e team di sicurezza che devono limitare l’accesso a sistemi di terze parti o back-end

## Contenuto video {#video-content}

* Scopri come individuare l’indirizzo IP per qualsiasi ambiente in Adobe Commerce Cloud.

>[!VIDEO](https://video.tv.adobe.com/v/3457493/?learn=on)

## Comando per ottenere l’indirizzo IP

Al posto delle informazioni sul segnaposto, devi utilizzare l’ID del progetto e il nome dell’ambiente.  Potrebbe anche essere necessario modificare `{1..3}` in modo che corrisponda al numero di nodi nel cluster Adobe Commerce Cloud, ma il numero più comune è 3.

```bash
magento-cloud environment:url -p InsertYourProjectID -e UseYourEnvironmentName --pipe -1 | sed 's/.\.c\.(.)/\1/;s/.$//' | xargs -I% dig +short {1..3}."%" | grep '^\d' | sort -u
```

## CLI di Adobe Commerce Cloud

```bash
magento-cloud environment:url -p InsertYourProjectID -e UseYourEnvironmentName --pipe -1
```

Lo strumento CLI di Magento-Cloud è progettato per aiutare sviluppatori e amministratori di sistema a gestire progetti e ambienti Adobe Commerce Cloud in modo efficiente. Estende le funzionalità della Cloud Console, consentendo agli utenti di eseguire attività di routine ed eseguire l’automazione localmente. Le funzionalità principali includono la gestione degli ambienti di integrazione, l&#39;estrazione e l&#39;unione degli ambienti, l&#39;elenco delle variabili e l&#39;utilizzo di SSH per la connessione agli ambienti remoti. Lo strumento semplifica i flussi di lavoro consentendo l&#39;esecuzione di comandi direttamente dalla workstation locale, migliorando il processo complessivo di sviluppo e distribuzione.

In questa sezione iniziale del codice di esempio, `magento-cloud environment:url -p InsertYourProjectID -e UseYourEnvironmentName --pipe -1` richiede l&#39;URL per l&#39;ambiente. Il valore restituito è simile al seguente `http://integration-1ajmyuq-mk7xr7zmslfg.us-4.magentosite.cloud/`. Ogni tanto assomiglia di più a `http://mcprod.russell.dummycachetest.com.c.abcikdxbg789.ent.magento.cloud/`.  Questo primo comando è piuttosto semplice e ora è il momento di passare al comando successivo.

Per ulteriori informazioni, leggere [Panoramica di Cloud CLI](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/dev-tools/cloud-cli/cloud-cli-overview){target="_blank"}

## Utilizzo di `sed` per ricerca e sostituzione

```bash
sed 's/.\.c\.(.)/\1/;s/.$//'
```

Il comando `sed` in UNIX®Linux® sta per Stream Editor. Viene utilizzato per eseguire trasformazioni di testo di base su un flusso di input (un file o un input da una pipeline). Gli usi comuni includono la ricerca, la ricerca e la sostituzione, l’inserimento e l’eliminazione di testo. Il comando `sed` elabora il testo riga per riga e applica le operazioni specificate, rendendolo uno strumento potente per la manipolazione e la creazione di script di testo.

Come indicato in precedenza, in genere sono disponibili 2 tipi di URL restituiti dal file CLI `magento-cloud`. Una variante contiene `.com.c.c` al centro. Questa variante è quella che deve essere modificata. Se viene rilevata questa struttura, è necessario rimuovere tutto a partire dall&#39;inizio dell&#39;URL fino a `.com.c.c`.  Quindi, rimane solo l’ultima parte dell’URL. Un URL di esempio è simile a `http://mcprod.russell.dummycachetest.com.c.abcikdxbg789.ent.magento.cloud/`.  Quando viene rilevato questo modello, l&#39;obiettivo è mantenere tutto dopo `.c.`.  In questo codice di esempio fornito, `sed 's/.\.c\.(.)/\1/'` viene utilizzato per acquisire questa parte e ignorare il resto del valore restituito originale. La parte rimanente dell&#39;URL è simile a `abcikdxbg789.ent.magento.cloud/`.\
Due comandi in esecuzione in `sed`. Sono separati da un punto e virgola. La seconda parte del comando `;s/.$//'` di `sed` consiste nel rimuovere eventuali barre finali, se esistenti, per ripulire l&#39;URL in modo che sia simile a `abcikdxbg789.ent.magento.cloud`.  A questo punto, l’URL è stato pulito e pronto per il comando successivo.

## Xargs con scavo

```bash
xargs -I% dig +short {1..3}."%"
```

Il comando `xargs` in UNIX®Linux® viene utilizzato per generare ed eseguire righe di comando dall&#39;input standard. Prende input da una pipe o da un file e lo converte in argomenti per un altro comando. È particolarmente utile per gestire un numero elevato di argomenti che superano il limite della shell. Il comando `xargs` può essere utilizzato per eseguire operazioni quali lo spostamento, la copia o l&#39;eliminazione di file. Consente un’elaborazione batch efficiente passando più argomenti ai comandi in una singola esecuzione.

Il comando `dig`, abbreviazione di Domain Information Groper, è uno strumento di amministrazione della rete utilizzato per eseguire query sui server DNS (Domain Name System). Consente di recuperare informazioni sui record DNS, ad esempio record A, AAAA, MX e CNAME. Il comando `dig` viene comunemente utilizzato per risolvere i problemi DNS, verificare le configurazioni DNS e raccogliere informazioni dettagliate sui nomi di dominio e i relativi indirizzi IP associati. Utilizzando varie opzioni e flag, gli utenti possono personalizzare l’output per visualizzare dettagli specifici o un riepilogo conciso.

L&#39;utilizzo di `xargs` con `dig` rende la procedura complicata, ma è necessaria. L’obiettivo è prendere l’URL pulito e salvarlo.  Una volta salvato come variabile, l&#39;URL viene inserito nel comando `dig`.

Il comando `dig` è stato creato per raccogliere informazioni DNS. Per ridurre la quantità di dati restituiti, viene utilizzato l&#39;argomento `+short`. Utilizzando `dig` in combinazione con `+short`, vengono restituiti gli indirizzi IP e talvolta le stringhe.

Nella parte del comando `xargs` salverà l&#39;URL `abcikdxbg789.ent.magento.cloud` e lo inserirà nel comando successivo `dig`. La tecnica di salvataggio dell&#39;URL combinata con l&#39;iterazione semplifica l&#39;utilizzo con il comando `dig`. Tieni presente che il mio codice di esempio è uno dei modi per raggiungere l’obiettivo e puoi modificare le cose per soddisfare le tue esigenze e aspettative.

A questo punto, l’URL è pronto. Ora vediamo come è possibile controllare ogni server del cluster. Per Adobe Commerce Cloud, ogni server del cluster ha un numero. L’identificatore del server è un prefisso dell’URL che è stato appena pulito e reso pronto per l’uso. Un modo rapido e semplice per effettuare il check-off dei server è l&#39;utilizzo di `{1..3}`. Utilizzando `{1..3}` che notifica l&#39;esecuzione del comando `dig` per 3 volte. Di seguito è riportata una rappresentazione di ciò che accade se si dovesse guardare l’esecuzione in tempo reale.

```bash
dig +short 1.<projectid>.ent.magento.cloud
dig +short 2.<projectid>.ent.magento.cloud
dig +short 3.<projectid>.ent.magento.cloud
```

A scopo illustrativo, l&#39;URL di esempio utilizzato da `dig` sarà simile al seguente:

```bash
dig +short 1.aabcikdxbg789.ent.magento.cloud
dig +short 2.abcikdxbg789.ent.magento.cloud
dig +short 3.abcikdxbg789.ent.magento.cloud
```

Se `{1..3}` è stato modificato in `{1..6}`, si presenterà come segue:

```bash
dig +short 1.aabcikdxbg789.ent.magento.cloud
dig +short 2.abcikdxbg789.ent.magento.cloud
dig +short 3.abcikdxbg789.ent.magento.cloud
dig +short 4.aabcikdxbg789.ent.magento.cloud
dig +short 5.abcikdxbg789.ent.magento.cloud
dig +short 6.abcikdxbg789.ent.magento.cloud
```

## Il comando `grep`

Talvolta viene restituita una stringa come parte del risultato di `dig`. A questo scopo, l’obiettivo è solo gli indirizzi IP e sono composti da numeri con punti. Per assicurarsi che l&#39;output finale contenga solo numeri, è possibile regolare il comando. Al termine, la sintassi finale è ` grep '^\d'`.  Aggiungendo `'^\d'`, il comando `grep` mantiene solo i numeri e ignora tutto il resto.

## Il comando `sort`

Utilizzando `sort -u`, verranno rimossi tutti i duplicati dall&#39;elenco degli IP. I duplicati si verificano solo quando si esaminano i livelli di sviluppo.

Questi ambienti di livello inferiore sono multi-tenant e condividono i server sottostanti con molti altri progetti. Gli ambienti di sviluppo sono server singoli e non cluster. Pertanto, quando il comando dig esegue il ciclo su ogni iterazione, restituisce più volte lo stesso IP. Pertanto, utilizzando il comando `sort -u`, tutti gli IP duplicati vengono rimossi e rimangono solo indirizzi IP univoci.



## Documentazione correlata

* [Indirizzi IP regionali](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/project/regional-ip-addresses|https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/project/regional-ip-addresses){target="_blank"}
