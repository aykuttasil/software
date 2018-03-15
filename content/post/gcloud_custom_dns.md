+++
title = "Google Cloud DNS"
#url = "google_cloud_dns"
desciption = "Google Cloud DNS"
metaAlignment = "center"
categories = [
  "genel"
]
tags = [
  "cloud","gcloud","dns","custom-dns","domain","cloud-dns"
]
keywords = [
  "gcloud","custom-dns","custom-domain","google-cloud","cloud-dns"
]
autoThumbnailImage = false
thumbnailImagePosition = "top"
thumbnailImage = ""
date = 2017-07-12
+++

# Google Cloud Platform Cloud DNS

Örnek senaryomuz şu şekilde olsun.

Google Cloud ortamında hazır olarak bulunan **(Cloud Launcher)** sanal makinelerden wordpress yüklü olanı seçtik ve hızlıca makinemizi hazır hale getirdik. Google Cloud bize makineye ait bir **ip** adresi verdi ve bu ip ile [worpress](https://wordpress.org/) sitemize erişim sağlayabiliyoruz.
Elimizde başka bir yerden aldığımız bir domain adresi var ve bu adresi wordpress sitemize yönlendirmek istiyoruz.

## Cloud DNS nedir?

Google Cloud platformunun dns yapılandırması için vermiş olduğu hizmettir.

<img src="/image/cloud_dns_1.png" height="400px" />

---

# Domain adresimizi nasıl yönlendirebiliriz?

Domain adresimiz örnek olarak http://www.aykutasil.com olsun

- Resimdeki gibi inputları dolduruyoruz.
<img src="/image/cloud_dns_2.png" height="400px" />

- Oluştur butonuna bastığımızda aşağıda ki gibi bir ekran gelecektir.
<img src="/image/cloud_dns_3.png"/>

- Daha sonra bu ekranda bulunan **Kayıt Kümesi Ekle** butonuna basarak **A** ve **CNAME** düzenlemeleri yapıyoruz. **A** kaydı oluştururken **IPv4 Adresi** inputuna **wordpress** kurulu sanal makinemizin IP sini veriyoruz. Bu IP ye **Compute Engine** sekmesinden ilgili makineye tıklayarak ulaşabiliriz.
<img src="/image/cloud_dns_4.png" height="400px"/>
<img src="/image/cloud_dns_5.png" height="400px"/>

- Kayıtları eklediğimizde son durum şöyle olması gerekiyor.
<img src="/image/cloud_dns_6.png"/>

- Domain aldığımız şirketin yönetim paneline girdiğimizde, DNS ayarları benzeri bir sekme olacaktır. Bu sekmeden **nameserver(NS)** bilgilerini yukarıdaki resimde bulunan;
	- **ns-cloud-a1.googledomains.com.**
	- **ns-cloud-a2.googledomains.com.**
	- **ns-cloud-a3.googledomains.com.**
	- **ns-cloud-a4.googledomains.com.**

satırları ile güncellememiz gerekmektedir.


Biraz bekledikten sonra (5-10 dakika) artık http://www.aykutasil.com olarak giriş yaptığınızda ilgili wordpress makinenize erişirim sağlayabilirsiniz.






