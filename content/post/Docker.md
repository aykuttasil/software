+++
categories = [
  "yazilim","docker"
]
keywords = [
  "software","docker"
]
autoThumbnailImage = false
thumbnailImagePosition = "top"
metaAlignment = "center"
title = "Docker"
date = "2017-01-11T19:55:08+03:00"
tags = [
  "sofware","docker"
]
thumbnailImage = ""
coverImage = "https://c8.staticflickr.com/8/7421/9339731831_9ba94f287c_k.jpg"

+++

DOCKER

---

Docker aldı başını gidiyor. Yeni bir devrim açıyor.

E haliyle yazılım dünyası buna sessiz kalmamakla beraber bir çok Docker nedir? örnekleri vs. hazırlanıyor.

Bu nedenle biz Docker nedir?  tanımlamasından daha çok hızlı ilerlemeler şeklinde ufak uygulamalar geliştiricez. Bende bu yazı serisiyle paralel şekilde öğrenimimi sağlayacağımı belirtmek isterim ! 🙂

 

Başlıyoruz..

İlk olarak makinemize Docker ı kuralım.

- Mac: https://docs.docker.com/docker-for-mac/

- Windows : https://docs.docker.com/docker-for-windows/

İndirip kurulumu sağladıktan sonra terminali açarak


`> docker run hello-world`

komutunu çalıştırıyoruz ve Docker a merhaba diyoruz.

```
Cannot connect to the Docker daemon. Is the docker daemon running on this host?
```
Yukarıda ki mesajı alanlar için;

```bash
$ docker-machine start # start virtual machine for docker
$ docker-machine env  # it's helps to get environment variables
$ eval "$(docker-machine env default)" #set environment variables
```

Komutlarını çalıştırarak Docker ın sistemimize tanıtıyoruz ve sonrasında tekrar **docker run Hello-World** komutunu çalıştırıyoruz.

<a href="http://imgur.com/HP4lgFS"><img src="http://i.imgur.com/HP4lgFS.png" title="source: imgur.com" /></a>

Hadi bakalım hayırlı olsun.. 😉

Ek Kaynak için : http://www.enterprisecoding.com/post/yeni-baslayanlar-icin-docker