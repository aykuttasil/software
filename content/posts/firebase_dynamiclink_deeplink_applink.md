---
title: "Firebase Dynamic Link & Deep Link & App Link"
date: 2023-02-09T23:09:05+03:00
weight: 1
# aliases: ["/first"]
tags: ["software","firebase","applink","deeplink","verify-app-link","custom-domain","firebase-hosting","dns","assetlink","sha256"]
author: "Me"
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "Firebase Dynamic Link & App Link & Custom Domain"
canonicalURL: "https://youtube.com"
disableHLJS: true # to disable highlightjs
disableShare: false
disableHLJS: false
hideSummary: false
searchHidden: true
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
cover:
    image: "img/flutter-firebase-dynamiclinks.png" # image path/url
    alt: "Firebase Dynamic Links" # alt text
    caption: "From Midjourney" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: false # only hide on current single page
editPost:
    URL: "https://github.com/aykuttasil/software/blob/master/content/"
    Text: "Suggest Changes" # edit text
    appendFilePath: true # to append file path to Edit link
---


## Öncelikle ne yapmak istiyoruz?
- Her hangi bir yerden uygulamamıza yönlendirme yapmak ve ilgili sayfayı/içeriği açmak
- Eğer uygulamamız yüklü değil ise kullanıcıyı direkt olarak store a yönlendirmek
- Web browser üzerinden açılmış ise ilgili web sayfasına yönlendirmek
- Dynamic link üretirken kendi domain adresimizi kullanmak
- Hangi linke ne kadar tıklanmış vs. gibi temel analytics verilere erişmek

## Deep Link & App Link

Her ikisi de aynı amaca hizmet etmesine rağmen aralarında ki fark linke tıklanma ve güvenilirlik oranını ciddi derecede etkileyecektir.

Deep Link daha global olanıdır. Yani aynı konfigürasyonu farklı farklı appler yapabilir ve bu durumda deep link ile uygulamamıza yönlendirme işlemi sekteye uğrayabilir. Yani sadece bizim uygulamamız tarafında bu linklerin işlenmesini garanti etmemiş oluruz.

App Link ise bazı doğrulamalar neticesinde(domain) mevcut linkin sadece bizim uygulamamıza yönlenmesini garanti etmiş olur.


### assetlinks.json 

App Link kullanımı için **domain**'in size ait olduğunu doğrulamanız gerekmektedir. Örneğin **aykutasil.com** adresini kullanmak istiyorsunuz. 
Bunun için https://aykutasil.com/.weelknown/assetlinks.json yoluna ilgili dosyayı yüklemeniz gerekmektedir. Bu dosyayı oluşturmak için [Android Studio](https://developer.android.com/studio/write/app-link-indexing)'yu kullanabilirsiniz. Ya da aşağıdaki dosyada ilgili alanları değiştirebilirsiniz.

**assetlinks.json**
```json
[{
  "relation": ["delegate_permission/common.handle_all_urls"],
  "target": {
    "namespace": "android_app",
    "package_name": "co.kolektifhouse.app", <--- Burası
    "sha256_cert_fingerprints":
    [ 
      "14:6D:E9:83:C5:73:06:50:D8:EE:B9:95:2F:34:FC:64:16:A0:83:42:E6:1D:BE:A8:8A:04:96:B2:3F:CF:44:E5", <--- Burası - For Debug
      "15:7D:E9:83:C5:73:06:50:D8:EE:B9:95:2F:34:FC:64:16:A0:83:42:E6:1D:BE:A8:8A:04:96:B2:3F:CF:44:E5"  <--- Burası - For Prod
    ] 
  }
}]
```
Yukarıdaki json dosyasının ilgili yerlerini değiştirmeniz ve yukarıda bahsedilen adrese yüklemeniz gerekmektedir.

#### SHA256 fingerprint

İlgili Android app'ini imzalarken kullandığınız **keystore** kullanılarak SHA256 üretmeniz gerekmektedir. Her bir uygulama versiyonu(debug/prod vs.) için ayrı ayrı üretilmesi gerekmektedir. 

```bash
keytool -list -v -keystore my-release-key.keystore
```

**Prod** sürümü için **Google Play App Signing** kullanılıyor ise **Play Store** sayfasından ilgili app için **SHA256** değerini eklemelisiniz.
Diğer türlü uygulama imzalama dosyaları farklı olacağından SHA256 değeri yanlış olacaktır.

**Play Store > Release > Setup > App Integrity** yolunu izleyerek uygulama imzalama dosyasının SHA256 değerine ulaşabilirsiniz.


## Firebase Dynamic Links

Firebase Dynamic Links için Firebase bize default olarak domain sağlamaktadır. Bu domain adresini kullanarak dynamic link oluşturabiliriz.
**Firebase Console > Dynamic Links** altından kolayca ilgili sayfaya erişebiliriz.

- <https://firebase.google.com/docs/dynamic-links/custom-domains>
  
Ayrıca bu domain haricinde custom domain'de belirleyebiliriz. Fakat bunun için ilgili domain'in Firebase'de host edilmesi gerekmektedir. 
Eğer web sitemiz için Firebase Hosting kullanıyor isek **firebase.json** dosyasını güncellememiz gerekmektedir.

Custom domain belirlerken dynamic link olarak kullanacağımız adrese dikkat etmeliyiz. Bunun için ya **subdomain** belirlemeliyiz ya da dynamic linke özel bir path belirlememiz gerekmektedir. Örneğin domain adresimiz **aykutasil.com** olsun. Dynamic link için belirleyeceğimiz adres ya **link.aykutasil.com** şeklinde olmalı ya da **aykutasil.com/link** şeklinde bir path olmalı. Diğer türlü conflict olacağından yönlendirme işlemi başarısız olacaktır.

> Bu kısım benim oldukça zorlandığım kısımlardan biri oldu :/

Diyelim ki **https://aykutasil.com** şeklinde bir websitemiz var. Ve **Firebase** harici başka bir yerde host ediliyor.

Dynamic link için **https://aykutasil.com/link** path'ini kullanmak istiyoruz.

Eğer websitemiz **firebase'de host edilmiyor ise** bu mümkün görünmüyor. 

Çünkü **https://aykutasil.com** yazdığımızda DNS server başka bir hosta yönlendireceği için **dynamic link** olarak işlev görmeyecektir.

**Dynamic Link** için kullanılan domain adresinin bir şekilde **Firebase Hosting'e yönlenmesi** gerekiyor.

Bu nedenle https://aykutasil.com ile başlayan bir **dynamic link** üretmemiz mümkün değil. (**Firebase harici bir yerde host ediliyor ise**)

Bu durumda **https://link.aykutasil.com** şeklinde bir **subdomain** kullanabiliriz. Bu **subdomain** adresini **DNS ayarlarından Firebase Hosting'e bakacak şekilde** düzenleyebiliriz. 

### Custom Domain

- **https://link1.aykutasil.com** subdomain adresini aşağıdaki gibi ekleyebiliriz. 

**Firebase Console > Hosting > Add custom domain** 

![Custom Firebase Hosting Domain](/img/firebase-hosting-custom-domain.png)

- Sonrasında bu domain adresinin bize ait olduğunu kanıtlamak için **DNS ayarlarına** bir **TXT** kaydı eklememizi isteyecek.

- Bu **TXT** yapılandırması için bir sonraki adımda göstermiş olduğum **A** kaydı yapılandırması ile aynı yöntemi izliyoruz.

Daha sonra **https://aykutasil.com** için domain yapılandırmasına yeni bir **A** kaydı eklememiz gerekmektedir. 
Bunu yapmak için domain servis sağlayıcı ayarlarına gitmemiz gerekmektedir.

![Custom Firebase Hosting Domain](/img/firebase-hosting-custom-domain1.png)

Ben domain adresimi **Cloudflare** üzerinden yapılandırdığım için bunu **Cloudflare** ayarlarından yapacağım.


![Custom Firebase Hosting Domain](/img/cloudflare-firebase-dns.png)

> En sondaki **link** olan kaydı **link1** olarak kaydetmelisiniz.


**Firebase Hosting** sayfasına gittiğinizde eklediğiniz domain i görebilirsiniz. 
Bir süre sonra **Status** u **Connected** olarak görmeniz gerekmektedir.. 

![Custom Firebase Hosting Domain](/img/firebase-hosting-custom-domain2.png)

**Connected** durumuna geçtikten sonra **Firebase** otomatik olarak **well-known/assetlinks.json** dosyasını ekleyecektir.

> <https://link.aykutasil.com/.well-known/assetlinks.json>
![Custom Firebase Hosting Domain](/img/firebase-hosting-custom-domain3.png)

Ve artık bu **subdomaini** kullanarak **Firebase Dynamic Links** oluşturabilirsiniz.

## Verify App Link

**Android 12 ve üstü** yüklü cihazınız var ise aşağıdaki komut satırı yardımı ile **App Link** verify edebilirsiniz.

```bash
adb shell pm get-app-links package-name
```

Aşağıdakine benzer bir cevap almanız gerekiyor.

![Custom Firebase Hosting Domain](/img/app-link-verify.png)

## Test

```bash
$ adb shell am start
        -W -a android.intent.action.VIEW
        -d "https://link.aykutasil.com/coupon" co.kolektifhouse.app

$ adb shell am start -a android.intent.action.VIEW \
    -c android.intent.category.BROWSABLE \
    -d "https://link.aykutasil.com/coupon"

```

## Kaynaklar
- <https://firebase.google.com/docs/dynamic-links>
- <https://docs.flutter.dev/development/ui/navigation/deep-linking>
- <https://www.youtube.com/watch?v=KNAb2XL7k2g>
- <https://www.kodeco.com/21376846-firebase-dynamic-links-getting-started>
- <https://developer.android.com/training/app-links>
- <https://developer.android.com/training/app-links/verify-android-applinks>
- <https://simonmarquis.github.io/Android-App-Linking/>
- <https://blog.logrocket.com/understanding-deep-linking-flutter-uni-links/>
- <https://developer.android.com/studio/write/app-link-indexing>
- <https://firebase.google.com/docs/dynamic-links/custom-domains>