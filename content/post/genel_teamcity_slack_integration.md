---
author: "Aykut Asil"
date: 2021-01-05T23:50:00+03:00
lastmod: 2021-01-05T23:50:00+03:00
title: "Teamcity Slack Entegrasyonu"
url: "teamcity-slack-integration"
weight: 10
keywords: ["aykuttasil","software","ci","cd","automation","teamcity","slack"]
description: "Teamcity Slack Entegrasyonu"
tags: ["software","ci","cd","automation","teamcity","slack"]
categories: ["genel"]
---

## Senaryo

Kendi sunucunuz üzerine kurmuş olduğunuz **Teamcity**'e **Slack** entegrasyonu yaparak, **build > deploy** adımlarını **Slack** üzerinden izlemek.

## Çözüm

Öncelikle **Slack** yapılandırmaları ile başlayalım.

https://api.slack.com/apps adresine giriyoruz ve yeni bir **App** oluşturuyoruz.


![Slack App](/video/slack_app_step1.mov "Slack App")



## Kaynaklar

- <https://www.jetbrains.com/help/youtrack/standalone/Slack-Integration-HTML.html>
- <https://www.jetbrains.com/help/teamcity/notifications.html#Slack+Notifier>
- <https://api.slack.com/apps>