---
title: Utilizzare Fastly per negare l’accesso a un intero sito web
description: Limitare l’accesso al sito Adobe Commerce con gli ACL Fastly Edge e un VCL personalizzato
feature: Cloud, Configuration, Site Management, System
topic: Administration,Commerce,Development, Security
role: Admin, Developer, User
level: Intermediate, Experienced
doc-type: Technical Video
duration: 231
last-substantial-update: 2025-07-11T00:00:00Z
jira: KT-18494
exl-id: 121e7a2f-f9fd-4cd1-b2be-48a12b538008
source-git-commit: 9aa4d70ee6a3825f027aa2a9c6a1ac0f876ed59f
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 0%

---

# Utilizzare Fastly per negare l&#39;accesso a un intero sito Web

Scopri come limitare l’accesso al sito Adobe Commerce Cloud utilizzando gli ACL Fastly Edge e snippet VCL personalizzati. Questa guida dettagliata ti aiuta a proteggere l’ambiente precedente al lancio consentendo solo indirizzi IP specifici, garantendo che il tuo sito di sviluppo rimanga privato.

## Cosa imparerai

Limitare l’accesso al sito Adobe Commerce con Fastly Edge ACL e VCL personalizzato | Ambienti pre-lancio sicuri

## A chi serve questo video?

* DevOps Engineer
* Sviluppatore Adobe Commerce
* Site Reliability Engineer

>[!VIDEO](https://video.tv.adobe.com/v/3464779?learn=on)

## Esempio di codice

Questo è un esempio del VCL utilizzato

```BASH
if ( !(client.ip ~ allowlist) && !req.http.Fastly-FF) { error 403 "Forbidden";}
```

## Documentazione correlata

* [Rilevamento indirizzo IP dannoso](https://experienceleague.adobe.com/en/docs/commerce-learn/tutorials/tools/new-relic/malicious-ip)
* [VCL personalizzato per consentire le richieste](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/cdn/custom-vcl-snippets/fastly-vcl-allowlist)
* [VCL personalizzato per il blocco delle richieste](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/cdn/custom-vcl-snippets/fastly-vcl-blocking)
