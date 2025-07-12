---
title: Utilizzare Fastly per negare l’accesso a un intero sito web
description: Limitare l’accesso al sito Adobe Commerce con gli ACL Fastly Edge e un VCL personalizzato
feature: Cloud, Configuration, Site Management, System
topic: Administration,Commerce,Development, Security
role: Admin, Developer, User
level: Intermediate, Experienced
doc-type: Technical Video
duration: 200
last-substantial-update: 2025-07-11T00:00:00Z
jira: KT-18494
source-git-commit: 810d1a17e9fe564e8450b091bbeb5574d7d76075
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 0%

---


# Utilizzare Fastly per negare l&#39;accesso a un intero sito Web

Scopri come limitare l’accesso al sito Adobe Commerce Cloud utilizzando gli ACL Fastly Edge e snippet VCL personalizzati. Questa guida dettagliata ti aiuta a proteggere l’ambiente precedente al lancio consentendo solo indirizzi IP specifici, garantendo che il tuo sito di sviluppo rimanga privato.

## Cosa imparerai

Limita l’accesso al sito Adobe Commerce con Fastly Edge ACL e VCL personalizzato | Ambienti di pre-lancio sicuri

## A chi serve questo video?

* DevOps Engineer
* Sviluppatore Adobe Commerce
* Site Reliability Engineer

>[!VIDEO](https://video.tv.adobe.com/v/3464787/?learn=on&enablevpops&captions=ita)

## Esempio di codice

Questo è un esempio del VCL utilizzato

```BASH
if ( !(client.ip ~ allowlist) && !req.http.Fastly-FF) { error 403 "Forbidden";}
```

## Documentazione correlata

* [Rilevamento indirizzo IP dannoso](https://experienceleague.adobe.com/it/docs/commerce-learn/tutorials/tools/new-relic/malicious-ip)
* [VCL personalizzato per consentire le richieste](https://experienceleague.adobe.com/it/docs/commerce-on-cloud/user-guide/cdn/custom-vcl-snippets/fastly-vcl-allowlist)
* [VCL personalizzato per il blocco delle richieste](https://experienceleague.adobe.com/it/docs/commerce-on-cloud/user-guide/cdn/custom-vcl-snippets/fastly-vcl-blocking)