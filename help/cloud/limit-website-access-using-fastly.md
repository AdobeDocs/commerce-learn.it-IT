---
title: Use Fastly to deny access to an entire website
description: Restrict Adobe Commerce Site Access with Fastly Edge ACLs and  a Custom VCL
feature: Cloud, Configuration, Site Management, System
topic: Administration,Commerce,Development, Security
role: Admin, Developer, User
level: Intermediate, Experienced
doc-type: Technical Video
duration: 231
last-substantial-update: 2025-07-11T00:00:00.000Z
jira: KT-18494
exl-id: 121e7a2f-f9fd-4cd1-b2be-48a12b538008
TQID: https://experienceleague.adobe.com/OQlPdH-rAoSoPK1-zemp5OODX6pKWV-dW1C88KVRE6I
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: b5f00040-57a0-4a6d-a39e-383b1936c2c9
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
topic_v2:
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 175
ht-degree: 0%

---

# Use Fastly to deny access for an entire website

Learn how to restrict access to your Adobe Commerce Cloud site using Fastly Edge ACLs and custom VCL snippets. This step-by-step guide helps you secure your pre-launch environment by allowing only specific IP addresses, ensuring your development site stays private.

## Cosa imparerai

Restrict Adobe Commerce Site Access with Fastly Edge ACLs &amp; Custom VCL | Secure Pre-Launch Environments

## A chi serve questo video?

* DevOps Engineer
* Adobe Commerce Developer
* Site Reliability Engineer

>[!VIDEO](https://video.tv.adobe.com/v/3464787?captions=ita&learn=on)

## Code Sample

This is an example of the VCL used

```BASH
if ( !(client.ip ~ allowlist) && !req.http.Fastly-FF) { error 403 "Forbidden";}
```

## Documentazione correlata

* [Detecting malicious IP address](https://experienceleague.adobe.com/it/docs/commerce-learn/tutorials/tools/new-relic/malicious-ip)
* [Custom VCL for allowing requests](https://experienceleague.adobe.com/it/docs/commerce-on-cloud/user-guide/cdn/custom-vcl-snippets/fastly-vcl-allowlist)
* [Custom VCL for blocking requests](https://experienceleague.adobe.com/it/docs/commerce-on-cloud/user-guide/cdn/custom-vcl-snippets/fastly-vcl-blocking)
