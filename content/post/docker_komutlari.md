+++
date = "2017-01-11T19:56:08+03:00"
keywords = [
  "software","docker","command","docker ps","docker images","bash"
]
autoThumbnailImage = false
title = "Docker Komutları"
#url = "docker-komutlari"
tags = [
  "sofware","docker","bash script"
]
thumbnailImagePosition = "top"
thumbnailImage = ""
coverImage = "https://c8.staticflickr.com/8/7421/9339731831_9ba94f287c_k.jpg"
metaAlignment = "center"
categories = [
  "yazilim","docker"
]

+++

Docker Komutları ve Açıklamaları

---

```bash
> docker images
```

Docker hostuna yüklü olan image leri gösterir.

---

```bash
> docker ps
```
Docker hostunda yüklü olan imagelerle inşa edilmiş olan container ları listeler.

---

```bash
> docker run hello-world
```
İlk olarak Local de (bilgisayarımızda) hello-world isminde bir image var mı kontrol eder. Eğer varsa direkt olarak çalıştırır.

Local de verilen isimde bir image bulamaz ise cloud da arama yapar, bulur, local e indirir ve ardından çalıştırır.

```bash
> docker pull hello-world
> docker run hello-world
```

komutlarının birleşimidir.

---

```bash
> docker run -p 8080:80 nginx
```
nginx image local de arar, eğer bulamazsa indirir ve local imize kurar.

**-p** parametresi kendisinden sonra verilen parametredeki portlar arasında port forwarding yani port yönlendirme yapar.

**-p** = host umuzdaki 8080 portunu container ımızda ki 80 portuna yönlendirme işlemini sağlıyoruz

--- 

```bash
> docker exec -it <container_id> /bin/bash
```
komutu ile Container’a bir Bash Shell açabiliriz. -i interaktif terminali -t ise terminalin attach olmasını istediğimizi belirtir. **docker exec -it /bin/bash** komutunu çalıştırın (Container ID’nin sadece baştan birkaç harfini vermemizin yettiğine -çakışma olmadığı müddetçe- dikkat edin).

Oluşturmuş olduğumuz container ın içine girerek komutları çalıştırmamıza olanak sağlar.

Container ın içindeyken;
```bash
> ps -ef
```
komutu ile container ın çalıştırmış olduğu tüm process leri listeleyebiliriz.

 

Komutlar
---

`docker images`	Lokal registery’de mevcut bulunan Image’ları listeler

`docker ps`	Halihazırda çalışmakta olan Container’ları listeler

`docker ps -a`	Docker Daemon üzerindeki bütün Container’ları listeler

`docker ps -aq`	Docker Daemon üzerindeki bütün Container’ların ID’lerini listeler

`docker pull <repository_name>/<image_name>:<image_tag>`	Belirtilen Image’ı lokal registry’ye indirir. Örnek: docker pull gsengun/jmeter3.0:1.7

`docker top <container_id>`	İlgili Container’da top komutunu çalıştırarak çıktısını gösterir

`docker run -it <image_id|image_name>` CMD	Verilen Image’dan terminal’i attach ederek bir Container oluşturur

`docker pause <container_id>`	İlgili Container’ı duraklatır

`docker unpause <container_id>`	İlgili Container pause ile duraklatılmış ise çalışmasına devam ettirilir

`docker stop <container_id>`	İlgili Container’ı durdurur

`docker start <container_id>`	İlgili Container’ı durdurulmuşsa tekrar başlatır

`docker rm <container_id>`	İlgili Container’ı kaldırır fakat ilişkili Volume’lara dokunmaz

`docker rm -v <container_id>`	İlgili Container’ı ilişkili Volume’lar ile birlikte kaldırır

`docker rm -f <container_id>`	İlgili Container’ı zorlayarak kaldırır. Çalışan bir Container ancak -f ile kaldırılabilir

`docker rmi <image_id|image_name>`	İlgili Image’ı siler

`docker rmi -f <image_id|image_name>`	İlgili Image’ı zorlayarak kaldırır, başka isimlerle Tag’lenmiş Image’lar -f ile kaldırılabilir

`docker info`	Docker Daemon’la ilgili özet bilgiler verir

`docker inspect <container_id>`	İlgili Container’la ilgili detaylı bilgiler verir

`docker inspect <image_id|image_name>`	İlgili Image’la ilgili detaylı bilgiler verir

`docker rm $(docker ps -aq)`	Bütün Container’ları kaldırır

`docker stop $(docker ps -aq)`	Çalışan bütün Container’ları kaldırır

`docker rmi $(docker images -aq)`	Bütün Image’ları kaldırır

`docker images -q -f dangling=true`	Dangling (taglenmemiş ve bir Container ile ilişkilendirilmemiş) Image’ları listeler

`docker rmi $(docker images -q -f dangling=true)`	Dangling Image’ları kaldırır

`docker volume ls -f dangling=true`	Dangling Volume’ları listeler

`docker volume rm $(docker volume ls -f dangling=true -q)`	Danling Volume’ları kaldırır

`docker logs <container_id>`	İlgili Container’ın terminalinde o ana kadar oluşan çıktıyı gösterir

`docker logs -f <container_id>`	İlgili Container’ın terminalinde o ana kadar oluşan çıktıyı gösterir ve -f follow parametresi ile o andan sonra oluşan logları da göstermeye devam eder

`docker exec <container_id> <command>`	Çalışan bir Container içinde bir komut koşturmak için kullanılır

`docker exec -it <container_id> /bin/bash`	Çalışan bir Container içinde terminal açmak için kullanılır. İlgili Image’da /bin/bash bulunduğu varsayımı ile

`docker attach <container_id>`	Önceden detached modda -d başlatılan bir Container’a attach olmak için kullanılır
 

Ek Kaynak : http://www.gokhansengun.com/docker-nedir-nasil-calisir-nerede-kullanilir/