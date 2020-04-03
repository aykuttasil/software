---
author: "Aykut Asil"
date: 2020-04-03T11:12:12+03:00
lastmod: 2020-04-03T11:12:12+03:00
title: "Docker Series - 1"
url: "docker-series-1"
weight: 10
keywords: ["aykuttasil","software"]
description: "Kısa kısa docker"
tags: ["software","docker","container"]
categories: ["docker"]
---

> Docker ile ilgili komut listesine erişmek için komut satırına `docker --help` ya da sadece `docker` yazabilirsiniz.

## `docker run`

**Docker** ile ilgili bir çok temel fonksiyonu `docker run` komutu ile sağlayabilirsiniz.

- Hızlıca [Docker Hub](https://hub.docker.com/)'da bulunan bir image'i makinemize kurmak istiyorsak aşağıdaki komut işimizi görecektir.

```bash
docker run ubuntu
```

Yukarıda ki komutu yazdığınızda ilk önce ilgili image dosyasının makinenizde olup olmadığına bakılır ve sonrasında eğer varsa zaten inmiş olan image çalıştırılılr. Eğer yoksa ilk önce **docker hub**'dan indirilirek makinenize kurulur ve sonrasında çalıştırılır.

Eğer sadece `docker run ubuntu` şeklinde komutumuzu yazacak olursa aslında bu komut default olarak **ubuntu:latest** versiyonunu indirecektir. Yani `docker run ubuntu:latest` ile aynı işlevi görecektir. Ama biz belli bir versiyonu indirip kurmak istiyorsak o zaman versiyon adını(**tag**) belirtmemiz gerek. Yani;

```bash
docker run ubuntu:trusty
```

Kurmak istediğimiz **image** hangi versiyonlara sahip olduğunu görmek istersek adres -> [Docker Hub](https://hub.docker.com/)

> Makinemizde kurulu **image**'lerin listesini `docker images` komutu ile görebiliriz.
> <img src="/img/docker_images.png" />

`docker run` komutunun alabileceği bir çok parametre mevcut. Yine aynı şekilde bu komutun alabileceği parametreleri görmek için `docker run --help` yazabiliriz.

<img src="/img/docker_run_help.png" />

---

```bash
docker run --rm -i -t ubuntu
```

<img src="/img/docker_run_rm_i_t.png" />

Yukarıdaki komutu açıklayalım:

- `--rm` : container ile işimiz bittiğinde otomatik olarak sil demek
- `-i -t` : interaktif mod, yani başlattığımız **container** ile etkileşim kurabilmemizi sağlıyor. Bu parametreler olmadan çalıştırılan **container** otomatik olarak başlar, **entrypoint** olarak verilen komutu işler ve sonrasında çıkar. Ama biz **container** ile etkileşim kurmak istiyorsak `-i -t` şeklinde parametre eklememiz gerekmektedir.

---

## `docker ps`

- Makinemizde şu an çalışan **container**'ları listelemek istersek `docker ps` komutunu yazabiliriz.

- Makinemizde daha önce çalıştırdığımız ve şu an pasif durumda olan **container**'ları listelemek istersek `docker ps -a` komutunu yazabiliriz.

`docker run --rm` şeklinde çalıştırdığımız **container**'lar işimiz bittiğinde otomatik olarak silineceği için `docker ps -a` komutunu çalıştırdığımızda listelenmediğini görücez. Fakat `--rm` olmadan çalıştırıp kapatmışsak `docker ps -a` ile listelendiği görebiliriz.

<img src="/img/docker_ps_a.png" />

Şimdilik bu kadar. İkinci kısımda görüşmek üzere.

### Kaynaklar

- <https://www.youtube.com/watch?v=-7tl9-bYnqE&list=PLe1QWkyzVMv6psIEboToi7sbcNpQlhc9c>
