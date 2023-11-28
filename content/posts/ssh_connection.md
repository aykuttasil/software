---
title: "SSH ile Sanal Sunucuya Bağlantı Kurma ve Yapılandırma"
date: 2023-11-28T11:25:28+03:00
weight: 1
# aliases: ["/first"]
tags: ["software","ssh","ssh-keygen"]
categories: ["software","ssh"]
author: "Me"
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "SSH (Secure Shell), uzak sunuculara güvenli bir şekilde bağlanmamızı sağlayan bir protokoldür. Bu protokolü kullanarak, sanal sunucularımıza güvenli bir şekilde erişebiliriz. Bu blog yazısında, SSH ile sanal sunucuya bağlantı kurma işlemi ve tekrar tekrar şifre girmeden bağlantı kurma yöntemleri üzerine odaklanacağız."
canonicalURL: "https://canonical.url/to/page"
disableHLJS: true # to disable highlightjs
disableShare: false
disableHLJS: false
hideSummary: false
searchHidden: true
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
cover:
    image: "<img/flutter-firebase-dynamiclinks.png>" # image path/url
    alt: "aykutasil.com" # alt text
    caption: "<text>" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: true # only hide on current single page
editPost:
    URL: "https://github.com/aykuttasil/software/blob/master/content/"
    Text: "Suggest Changes" # edit text
    appendFilePath: true # to append file path to Edit link
    
url: "ssh-connection"
---

# SSH ile Sanal Sunucuya Bağlantı Kurma ve Yapılandırma

SSH (Secure Shell), uzak sunuculara güvenli bir şekilde bağlanmamızı sağlayan bir protokoldür. Bu protokolü kullanarak, sanal sunucularımıza güvenli bir şekilde erişebiliriz. Bu blog yazısında, SSH ile sanal sunucuya bağlantı kurma işlemi ve tekrar tekrar şifre girmeden bağlantı kurma yöntemleri üzerine odaklanacağız.

## SSH Nedir?

SSH, ağ üzerinde güvenli bir şekilde iletişim kurmak için kullanılan bir protokoldür. Bu protokol, şifreleme ve kimlik doğrulama özellikleri sayesinde verilerin güvenli bir şekilde iletilmesini sağlar.

## SSH ile Bağlantı Kurma

1. **SSH İstemcisini Kullanma**

   İlk adım, bağlanmak istediğiniz bilgisayar veya terminal üzerinde bir SSH istemcisine sahip olmaktır. Linux ve macOS kullanıcıları genellikle terminali, Windows kullanıcıları ise PuTTY gibi araçları kullanabilir.

2. **Bağlantı Komutunu Kullanma**

   SSH ile bağlantı kurmak için aşağıdaki komutu kullanabilirsiniz:

```bash
    ssh kullaniciadi@sunucuip
```

Örneğin:

```bash
    ssh username@example.com
```

Bu komut, kullaniciadi olarak belirttiğiniz kullanıcı adıyla, sunucuip olarak belirttiğiniz sanal sunucuya bağlantı kurar.


> Şifre İle Giriş Yapma

İlk bağlantıda genellikle şifrenizi girmeniz istenecektir. Şifrenizi girdikten sonra, başarılı bir şekilde sanal sunucuya bağlanmış olacaksınız.


## SSH Anahtarları ile Şifre Girmeden Bağlantı Kurma

SSH anahtarları, şifre kullanmadan güvenli bir şekilde bağlantı kurmamıza olanak tanır. İşte adım adım SSH anahtarlarıyla şifre girmeden bağlantı kurma:

1. **SSH Anahtarı Oluşturma**

Terminal üzerinde şu komutu kullanarak bir SSH anahtarı oluşturun:

```bash
   ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

veya

```bash
   ssh-keygen -t ed25519 
```

Bu komut, sizden bir dosya adı ve şifre isteyecektir. Dosya adı olarak genellikle varsayılan değer olan `id_rsa` kullanılır. Şifre girmek istemiyorsanız, boş bırakabilirsiniz.

2. **SSH Anahtarını Sunucuya Ekleme**

SSH anahtarını sunucuya eklemek için, aşağıdaki komutu kullanabilirsiniz:

```bash
   ssh-copy-id kullaniciadi@sunucuip
```

Bu komut, kullaniciadi olarak belirttiğiniz kullanıcı adıyla, sunucuip olarak belirttiğiniz sanal sunucuya SSH anahtarını ekler.

> **SSH Anahtarını Sunucuya Manuel Olarak Ekleme**

SSH anahtarını sunucuya manuel olarak eklemek için, aşağıdaki adımları izleyebilirsiniz:

1. Öncelikle, SSH anahtarınızın içeriğini kopyalayın:

```bash
   cat ~/.ssh/id_rsa.pub
```

2. Daha sonra, sunucuya bağlanın ve `~/.ssh/authorized_keys` dosyasını açın:

```bash
   nano ~/.ssh/authorized_keys
```

3. Son olarak, SSH anahtarınızın içeriğini bu dosyaya yapıştırın ve kaydedin.
4. Artık, SSH anahtarınızı kullanarak sunucuya bağlanabilirsiniz.
5. SSH anahtarınızı sunucudan kaldırmak isterseniz, `~/.ssh/authorized_keys` dosyasından ilgili anahtarı silmeniz yeterlidir.
6. SSH anahtarınızı sunucudan kaldırdıktan sonra, tekrar şifre ile bağlantı kurmanız gerekecektir.
