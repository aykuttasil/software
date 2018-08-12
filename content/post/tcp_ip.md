+++
title = "TCP/IP Nasıl Çalışır ?"
#url = "tcpip-nedir_nasil_calisir"
date = "2017-01-11T18:05:02+03:00"
metaAlignment = "center"
thumbnailImagePosition = "top"
thumbnailImage = ""
coverImage = "https://c8.staticflickr.com/8/7421/9339731831_9ba94f287c_k.jpg"
tags = [
  "software","tcp","ip","tcp/ip"
]
keywords = [
  "yazilim",
  "sofware","tcp","ip"
]
autoThumbnailImage = false
desciption = ""
categories = [
  "yazilim",
  "genel",
]

+++

# TCP/IP Nasıl Çalışır ?

Bilgisayar ağları kullanılarak bilgisayarların birbirileriyle haberleşmeye başladıkları ilk yıllarda iki bilgisayarın birbiriyle haberleşebilmeleri için aynı marka/model cihazları kullanmaları gerekiyordu. Bunun üzerine farklı üreticiler tarafından üretilen cihazların birbiriyle sorunsuz ve belirli bir düzen içinde haberleşebilmesi için çeşitli standartlar geliştirilmiştir.

Bunlardan en çok kullanılanı Açık Sistem Bağlantıları komitesi tarafından geliştirilen 7 katmanlı OSI referans modeli ve Amerikan Savunma Bakanlığı tarafından geliştirilen TCP/IP referans modelidir. OSI iki bilgisayar arasındaki haberleşme problemini çözmek için 7 katmanlı (aşamalı) bir ağ sistemi önermiştir.

<a href="http://imgur.com/TVlx38I"><img src="http://i.imgur.com/TVlx38I.gif" title="source: imgur.com" /></a>

OSI referans modelindeki 7 katmana karşılık TCP/IP referans modeli 4 katmanlı bir çözüm sunar ve 7 katmanlı OSI modeline göre daha hızlı bir iletişim imkânı sunar. OSI modeli iletişim standartlarını belirlemeye yöneliktir ve TCP/IP daha uygulanabilir bir model olduğu için daha çok uygulamaya yöneliktir.

## TCP/IP Nedir?

**TCP/IP** birçok protokolün toplandığı bir protokoller ailesidir. Bu referans modeline en çok kullanılan iki protokolün ismi verilmiştir; TCP (Transmission Control Protocol) ve IP (Internet Protocol). Bu referans modelinde 4 farklı katmanda 15’ten fazla protokol vardır. Veriler bu katmanlar arasında sırasıyla paketlenerek gönderilir, alıcıda ise paketlemenin tersi sırayla teker teker açılarak veri ulaştırılmış olur.

**► Protokol:** Protokoller cihazlar arası iletişimde kullanılan, iletişim kurallarını belirleyen ağ dilleridir. Referans modelinin her katmanda ayrı protokoller görev yapar. Farklı referans modellerinde aynı protokoller çalışabilir.

## TCP/IP Referans Modeli Katmanları

<a href="http://imgur.com/vXJSOn0"><img src="http://i.imgur.com/vXJSOn0.jpg" title="source: imgur.com" /></a>

**► Uygulama katmanı:** Bu katmanda gönderilecek veri tipi ve veriyi işleyen uygulamalar bulunur. Örneğin bir HTML web sayfası ve bu veri tipini kullanan HTTP protokolü bu katmandadır. OSI modelindeki sunum ve oturum katmanları TCP/IP modelinde uygulama katmanı içerisinde yer alır. E-Posta gönderimi için kullanılan SMTP ve dosya gönderimi için kullanılan FTP protokolleri bu katmanda bulunur.

**► Taşıma katmanı:** Bu katmanda verinin nasıl gönderileceği belirlenir. Veri güvenliği, hata kontrolü gibi işlemler yapılır. TCP ve UDP bu katmandadır. TCP klasik veri aktarımında UDP ise medya aktarımında kullanılır. TCP, UDP ye göre daha güvenli fakat daha yavaş çalışır. Çünkü TCP ‘de gönderilen her veri paketinin ardından verinin yerine doğru bir şekilde ulaşıp ulaşmadığı kontrol edilir.

**► Ağ katmanı:** IP katmanı olarak da adlandırılan bu katman da verilerin gideceği adres veriye eklenir yani veri bu katmandan gönderilir ve yönlendirilir. IPv4 ün gelecekte yetersiz kalma durumuna karşı IPv6 sistemine geçmek için çalışmalar başlatılmıştır.IPv4 32 bit iken IPv6 ile 128 bitlik adresler kullanılacak. Bu sayede daha fazla cihaza IP adresi atanabilecek.

**► Fiziksel katman:** Bu katman verinin hangi yolla gönderileceği belirlenir. İletişim ortamının özelliklerini, haberleşme hızını ve kodlama şemasını belirler. Ethernet, Wi-Fi, Token Ring, ATM gibi protokoller bu katmanda çalışır.

## Katmanlar ve Protokoller Nasıl İşler?

Örneğin bir web sayfası bilgisayarınıza şu şekilde gelir;

- ► Web sayfasının saklı olduğu sunucuda uygulamalar sayfanın HTML veri formatında bir çıktısını oluşturur. Ve bu veriyi HTTP protokolüyle gönder komutunu verir. Bunlar 4. katmanda yani uygulama katmanında olur. Buradan çıkan veri 3. katmana yani taşıma katmanına gönderilir.

- ► Taşıma katmanında veriye taşıma katmanının bilgileri yani port bilgisi ve veri boyutu eklenir.

- ► Üçüncü katmandan çıkan veri paketine ikinci katmanda verinin gönderileceği bilgisayarın (sunucunun) ve sizin bilgisayarınızın IP adresleri ve verinin son halinin boyutu eklenir.

- ► Son katmanda yani fiziksel katmanda fiziksel adresler ve verinin yeni boyutu pakete eklenir.

- ► Paket sunucudan çıkar ve sunucu ile sizin bilgisayarınız arasındaki binlerce kilometrelik yolu kat ederek bilgisayarınıza ulaşır.

<a href="http://imgur.com/rQQwnvv"><img src="http://i.imgur.com/rQQwnvv.png" title="source: imgur.com" /></a>

Veri bilgisayarınıza ulaştığında bu sefer tersi sırayla katmanlardaki protokoller işletilir. Bilgisayarınız önce fiziksel katmanı ardından ağ katmanını, daha sonra taşıma ve uygulama katmanlarını işletir. Ve en sonunda kalan paketi web tarayıcınıza gönderir. Her katmanda ayrı donanımlar görev yapar. Fiziksel katmanda Switch, ağ katmanında Router, taşıma katmanında ise NAT gibi donanımlar kullanılır.

[Kaynak](http://www.elektrikport.com/teknik-kutuphane/tcpip-nasil-calisir/9004)
