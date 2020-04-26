+++
author = "Aykut Asil"
categories = ["docker"]
date = "2020-04-03T11:12:12+03:00"
description = "Kısa kısa Docker"
keywords = ["aykuttasil", "software"]
lastmod = "2020-04-03T11:12:12+03:00"
tags = ["software", "docker", "container"]
title = "Docker Series - 2"
url = "docker-series-2"
weight = 10

+++

### Docker Container'ı Detach modda çalıştırmak

Uygulamanızı yazarken eğer **thread**'ler ile biraz derin bir ilişki kurmuşsanız **daemon** kavramıyla karşılaşmış olabilirsiniz. Kısaca, "arka tarafa git ve sessiz sedasız çalışmaya devam et" demek diyebiliriz. Ayrıntılı bilgi için -> Google :)

Aynı mantık ile **container**'ımızın arka planda sesiz sedasız çalışmasını isteyebiliriz. **Örneğin** web sitemizi bir container'a kurduk, yerleştirdik ve belli bir port üzerinden dışarıya açtık. Ve artık bu **container** kapanmadan sürekli çalışması gerekiyor ki web sitemize erişebilelim. Bunun için **container**'ımızın arka planda sürekli çalışır modda yani **detach** modda çalışması gerekmektedir. Aşağıdaki komut satırı ile bunu gerçekleştirebiliriz.

```bash
docker run -d --name my-nginx-container nginx
```

> **-d** parametresi ile **container**'ımızın **detach** modda çalışmasını sağlıyoruz.
> `docker ps -a` komutu ile **container**'ımızın çalıştığını görebiliriz.

### Detach modda çalıştırılan bir Container'ı tekrar önyüze getirmek

```bash
docker attach my-nginx-container
```

Yukarıda ki komut satırı ile arka planda çalışan **container**'ımızı tekrar önyüze getirebiliriz.

---

## Docker Image

Makinemizde yüklü **docker image**'larını listelemek istersek

```bash
docker image list
```

komutunu kullanabiliriz.

<img src="/img/docker_image_list.png" />

Eğer makinemizden bir **image**'ı kaldırmak istersek

```bash
docker rmi imageId veya name
```

komutunu kullanabiliriz.

Eğer silmek istediğimiz **image**'ı daha önce çalışıp duran veya hala çalışıyor durumunda olan bir container var ise şu şekilde bir hata mesajı fırlatılır.

<img src="/img/docker_image_rmi_error.png" />

Eğer ne olursa olsun sil demek istersek `-f` parametresini eklememiz yeterli olacaktır. Yani;

```bash
docker rmi -f imageId veya name
```

---

## Container --restart=always

Eğer ayağa kaldırdığımız bir **container**'ın ölmesi durumunda otomatik olarak tekrar koşmasını istersek **run** komutuna `--restart=always` parametresini verebiliriz.

```bash
docker run -d --restart=always --name=web nginx
```

---

## Docker exec komutu

Eğer **çalışan** bir **container**'ın içerisinde bir komut çalıştırmak istersek `exec` komutunu kullanabiliriz.

Örneğin **nginx** image'ini kullanarak bir **container** ayağa kaldırdık.

> `docker run -d --name=web nginx`

Ve bu **container**'ın içine girerek `date` komutunu çalıştırmak istiyoruz. Ya da **container**'ın içindeki **bash**'e erişmek istiyoruz. Bunun için;

```bash
# docker exec -i -t continerId & name command

docker exec -i -t web date

docker exec -i -t web bash
```

komutlarını kullanabiliriz.

<img src="/img/docker_exec.png" />

---

### Kaynaklar

- <https://docs.docker.com/engine/reference/commandline/attach/>
- <https://www.youtube.com/watch?v=ZoRfHakBLX4>
