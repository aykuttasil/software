---
title: "Android Debugging With Local Web Server"
date: 2019-08-03T03:14:28+03:00
lastmod: 2019-08-03T03:14:28+03:00
draft: false
keywords: ["android","debugging","local-web-server","windows"]
description: "Android Debugging With Local Web Server"
tags: ["android","debugging","local-web-server","windows"]
categories: ["android","debugging"]
author: "Aykut Asil"
comment: true
toc: true
autoCollapseToc: false
postMetaInFooter: true
hiddenFromHomePage: false
---

Fiziksel Android cihazımızla localimizde çalışan web server'a erişmek, development aşamasında eminim ihtiyaç duyduğunuz veya duyacağınız gereksinimlerden biridir. Bununla beraber local makinenizde bulunan VM üzerinde koşan web server'a erişmek.. Ah harika..

Canlı bir örnek verirsem sanıyorum daha iyi olacak.

> Geliştirme yaptığınız makinenizinde **MacOS** işletim sistemi var. Bununla beraber çeşitli ihtiyaçlarınızdan ötürü windows kurmanız gerekti ve **Virtual Machine** kurarak içine windows yüklediniz. **Windows** üzerinde Visual Studio ile bir **WebApi** ayağa kaldırdınız ve bu **api**'ye fiziksel **Android** cihazınızdan erişmek istiyorsunuz. Üstüne üstlük normal **web adresi** girer gibi yani **myrestapi.local** gibi bir adres üzerinden erişim sağlamak istiyorsunuz.

## Kurulum

Artık kuruluma geçebiliriz.

### **Windows** için gerekli düzenlemeler

Eğer kurulu değilse ilk olarak **IIS**'i etkinleştirmemiz gerekmektedir. Bunun için arama çubuğuna **Turn Windows features on or off** yazarak ilgili pencereyi açıyoruz. Ve aşağıda seçili olan kutucukları seçerek kurulumu tamamlıyoruz.

![iis](/image/iis_open.png)

Daha sonra arama kutusuna **IIS** yazarak ilgili sayfaya gidiyoruz. **Sites** sağ tıklayarak **Add Website** diyoruz.

![iis](/image/iis_addwebsite.png)

Açılan pencerede **Physical path:**'i projenin kaynak kodlarının **web.config** dosyasının bulunduğu klasör olarak ayarlıyoruz.

![iis1](/image/iis_addwebsite1.png)

![iis2](/image/iis_addwebsite2.png)

Sıra geldi **hosts** dosyasını düzenlemeye. Bunun için [şuraya](../local_iis_site_kurulumu/) gidip gerekli ayarlamaları yapabilirsiniz.

---

### Mac için gerekli düzenlemeler

Ana işletim sistemi(Mac) üzerinden VM'deki local web server'a **myrestapi.local** adresi üzerinden erişim sağlamak için **mac** tarafında da **host** dosyasını düzenlememiz gerekmektedir. Bunun için **terminal**'ı açarak ve `~ sudo nano /private/etc/hosts` komutunu girerek ilgili dosyayı açıyoruz. Ve **VM** üzerinde çalışan windows'umuzun **IP**'sini öğrenerek aşağıdaki gibi en alt satıra ekliyoruz.

![mac host](/image/mac_host.png)

### Chrome için düzenlemeler

Mac üzerinde bulunan **Chrome**'u açıyoruz ve arama kutusuna `chrome://inspect` yazarak ilgili sayfayı açıyoruz. Bu sırada **android** cihazımızın **usb** kablosu ile mac'e bağlı olması gerekmektedir. Açılan sayfada cihazımızın bağlı olduğunu görüyor olmamız lazım.

**Port forwarding** butonuna basıyoruz ve aşağıdaki gibi giriş yapıyoruz.

![chrome_port_forwarding](/image/chrome_port_forwarding.png)

### Android Cihazımız için düzenlemeler

**Android** cihazlarının farklılığından dolayı aşağıdaki ayarları yapma şekli farklılık gösterebilir ama muhtemelen benzer şekillerde olacaktır.

Android cihazımız üzerinden **Wi-Fi Settings** sayfasını açıyoruz. Ve **Proxy** ayarlama kısmından **Manuel** seçerek aşağıdaki gibi giriş yapıyoruz. **Chrome** yapılandırma kısmında girmiş olduğumuz **8080** port numarasının buradaki ile aynı olmasına dikkat ediyoruz.

![android proxy](/image/android_proxy.jpeg)

![android proxy](/image/android_proxy1.jpeg)

---

Evet tüm düzenlemelerimizi yaptık. Artık **VM (Windows)** üzerinde koşan **api**mize <htttp://myrestapi.local> adresinden ve **Windows, Mac veya Android** üzerindeki bir browserdan erişim sağlayabiliriz. Ve tabi geliştirmekte olduğunuz **android** uygulaması üzerinden de bu adrese erişim sağlayabileceksiniz.

Geliştirme yaparken **Android** cihazımız üzerinde **Chrome**'un açık olmasına dikkat ediyoruz. Ve cihazın **Proxy** ayarlarının yukarıda bahsettiğimiz şekilde ayarlı olmasına dikkat ediyoruz.

> **Not:** Geliştirme sonrasında ilgili **android** cihazımızdan normal **wi-fi** bağlantısı ile internete erişmek için **proxy** ayarlarını **"Yok"** olarak işaretlemeyi yani **default** ayarına geri göndürmeyi unutmayalım.

## Kaynaklar

- <https://www.maketecheasier.com/setup-local-web-server-all-platforms/>
