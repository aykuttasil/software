+++
title = "Docker Container Install nano"
metaAlignment = "center"
date = "2016-12-25T03:51:49+03:00"
categories = [
  "yazilim","docker"
]
tags = [
  "docker","nano","container","linux"
]
keywords = [
  "yazilim","sofware","docker","nano","komut penceresi","apt-get","kitematic","container","linux"
]
autoThumbnailImage = true
thumbnailImagePosition = "top"
thumbnailImage = "DockerLogo.png"
coverImage = "DockerLogo.png"

+++

Docker ile ayağa kaldırmış olduğunuz bir container olduğunu varsayalım ve bu docker container a erişerek komut satırı çalıştırmanız gerekiyor.

https://docs.docker.com/engine/reference/commandline/exec/

Yukarıdaki linki takip ederek nasıl komut çalıştıracağınızı öğrenebilirsiniz.
Ya da *Kitematic* gibi bir uygulama yüklü ise bu uygulamayı açarak ve çalışan image e tıklayarak exe butonuna basmamız yeterli olucaktır.

Bu sayfada anlatmak istediğim şey tüm bunları yaptınız ve container ın içindeyken bir dosya oluşturdunuz.
Ve bu dosyanın içeriğini düzenlemeniz gerekmekte.

**Nasıl yaparız ?**


Container ın içerisinde olduğumuzdan bir görsel arayüz yok. `open . ` fln diyerek dosya yoluna erişim yapamayız.

`nano` gibi birşeye ihtiyacımız var ki bash penceresi üzerinden değişiklik yapabilelim.

Öncelikle

```bash
# apt-get update
```
komutunu çalıştırarak linux paketlerini güncelliyoruz.

Daha sonra

```bash
# export TERM=xterm
```

komutunu çalıştırıyoruz ve ardından


```bash
# apt-get install nano
```

komutu ile nano yu container ımıza yüklüyoruz.

Artık istediğimiz dosyanın path ine ulaşarak ve 

```bash
# nano dosya_adi.txt
```
diyerek command penceresi üzerindeyken düzenleme yapabiliriz.


Kolay gelsin gençler..






