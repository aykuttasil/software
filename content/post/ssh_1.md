---
title: "SSH Sürekli Parola İstemesi Sorunu"
date: 2018-10-26T01:04:21+03:00
lastmod: 2018-10-26T01:04:21+03:00
draft: false
keywords: ["ssh"]
description: ""
tags: ["ssh"]
categories: ["genel"]
author: "Aykut Asil"
comment: true
toc: true
autoCollapseToc: false
postMetaInFooter: true
hiddenFromHomePage: false
---

# Problem

./ssh klasörünüzde **id_rsa** ve **id_rsa.pub** dosyanız var ve **Github, Gitlab, DigitalOcean vs.** gibi platformlara erişim için bu **rsa** keyini kullanıyorsunuz. Her biri için ayrı ayrı **rsa** key oluşturmuşda olabilirsiniz tabi. Fakat `git clone git@gitlab.com:aykuttasil/test.git` gibi bir komutu çalıştırdığınızda `Enter passphrase for key '/Users/aykutasil/.ssh/id_rsa':` gibi bir uyarı karşınıza çıkıyor ve **rsa** keyinizin şifresini girmenizi istiyor. Her **git** komutunda aynı şey ile karşılaşıyorsunuz.

Yukarıda ki durum **ssh**'ın kullanım amacına ters düştüğü için ortada bir problem var demektir. 

# Çözüm

- `ssh-add -K ~/.ssh/id_rsa` komutunu çalıştırın.

Ve **./ssh/** klasörü altındaki **config** dosyasında aşağıdaki bloğu ekleyin.

```text
Host *
  UseKeychain yes
  AddKeysToAgent yes
  IdentityFile ~/.ssh/id_rsa
```  

Artık erişmek istediğiniz sunucu sizin bilgisayarınızı ve kullanıcınızı tanıyacağı için tekrar tekrar şifre girmeniz gerekmeyecektir.