---
title: "CircleCI Configuration"
date: 2018-11-27T14:20:45+03:00
lastmod: 2018-11-27T14:20:45+03:00
draft: false
keywords: ["ci","cd","continuous-integration","continuous-deployment","circleci","build","test","deployment"]
description: "Circle CI/CD"
tags: ["ci","cd","continuous-integration","continuous-deployment","circleci"]
categories: ["CI/CD"]
author: "Aykut Asil"
comment: true
toc: true
autoCollapseToc: false
postMetaInFooter: true
hiddenFromHomePage: false
---

# CI/CD

Uygulamaların **build/test/depleyment** süreçlerini **otomatize** etmemizi sağlayan **CI/CD** kavramları günümüz yazılım dünyasında olmazsa olmaz, bilinmezse ayıp olurlar arasına girmiş bulunmaktadır. Manuel yapılan bu işlemlerin otomatize edilmesi bize ve takım üyelerine çeşitli konularda fayda sağlayacak ve eğer manuel yöntemler ile ilerlendiği takdirde oluşabilecek hataları minimuma indirecektir. Projenizin **build/test/deployment** adımlarında çeşitli sorunlar yaşamaya başlamışsanız ya da başlamadan bitirmek istiyorsanız bu kavramları öğrenmekte fayda var.

Çeşitli **CI/CD** araçları bulunmaktadır. **Travis, Bitrise, AppCenter, CircleCI** bunlardan bazılarıdır.

> **Continuous Integration (CI)**

> Sürekli entegrasyon, geliştiricilerin kodlarını erken ve sık olarak, paylaşılan bir reponun master dalına entegre etmelerini teşvik eden bir düşünce sistemidir. Geliştirme yapılan kod sürecin en sonunda entegre edilmektense, gün içerisinde sık sık entegre edilerek testlerin çalıştırılması ve eğer varsa hataların önceden fark edilerek düzeltilmesi sağlanır. Sürekli Entegrasyon, dijital dönüşüm için önemli bir adımdır.

**Nedir?**

- Her geliştirici, günlük olarak commit yapar. Her commit otomatik olarak build ve testi tetikler. Build ve test başarısız olursa, hızlı bir şekilde onarılır.

**Neden ihtiyaç var?**

- Takım verimliliğini ve mutluluğunu geliştir.
- Sorunları bul ve çabuk çöz. Daha yüksek kalite, daha kararlı ürünler bırakın.

---

# CircleCI

Circle CI, çeşitli pricing modelleri ile birlikte belli kısıtlamalar dahilinde ücretsiz **CI/CD** araçları sunan bir ortam sunar. Hazırlanan konfigurasyon dosyası(**.yml**) ile tüm süreçler(**build/test/deployment**) otomatik olarak işletilebilir.

## config.yml

### **jobs**

Jobs bloğunun içerisinde ki tüm komutlar circleci konteynerini kullanan tek bir ünitede gerçekleştirilir.

Jobs ve Steps blokları, konfigurasyon aşamasında daha fazla kontrol ve daha sık geri bildirim sağlayarak sürece hakim olmamız için bir ortam sunar.

> **workspace**
>
> - Tek bir **workflow** içerisindeki **job**'lar arasında veri taşımamıza olanak sağlar.
>
> **cache**
>
> - **Workflow** arasında veri taşınması için cache'leme mekanizması kullanılır.
>
> **artifacts**
>
> - **Workflow** çalışmasını bitirdikten sonra verilerin kalıcı olarak saklanmasını sağlar.

#### **steps**

```bash
...
    steps:
      - checkout # Special step to checkout your source code
      - run: # Run step to execute commands, see
          # circleci.com/docs/2.0/configuration-reference/#run
          name: Running tests
          command: make test # executable command run in
          # non-login shell with /bin/bash -eo pipefail option
          # by default.
#...
```

**steps** bloğu genelde projede bir job oluşturmak için gereken adımları/komutları belirtmemizi sağlar. **checkout** özel bir komuttur ve SSH ile uygulamamızın kaynak kodunun kopyasını alır(**Github,Bitbucket**). **run** bölümünde ise çalıştırılacak komutlar belirtilir.

##### **checkout**

**checkout** komutu proje kaynak kodlarını ilgili repodan(github,bitbucket) çekerek circleci container ı içerisinde klonlar.

##### **run**

**run** bloğu altında tek bir komut çalıştırılabileceği gibi birden fazla komutun da işlenmesi sağlanabilir. Bunun için aşağıdaki gibi bir düzenlemeye ihtiyaç vardır:

```yml
      - run:
          name: Initialize Keystore File
          command: |
             echo "signingKeyAlias=$KEYSTORE_KEY_ALIAS" >> keystore.properties
             echo "signingKeyAliasPassword=$KEYSTORE_KEY_ALIAS_PASSWORD" >>  keystore.properties
             echo "signingStoreFile=$KEYSTORE_STORE_FILE" >>  keystore.properties
             echo "signingStorePassword=$KEYSTORE_STORE_PASSWORD" >>  keystore.properties
             cat keystore.properties
```

##### **save_cache**

Uygulama bağımlılık/dependency dosyalarını cache'lemek için kullanılır.

##### **restore_cache**

**save_cache** bloğunda cache'lenen bağımlılıkların tekrar indirilmeden kullanılmasını sağlar. Bu sayede yalnızca bir kez indirilen bağımlılıklar sayesinde uygun ortamın oluşturulma süresi minumuma inmiş olur.

##### **store_artifacts**

```yml
      - store_artifacts:
          path: app/build/reports
          destination: reports
```

Proje dosyalarının derlenmesi sonrasında **app/build/reports** klasörü altında oluşan dosyaların kalıcı olarak saklanmasını sağlar. **CircleCI** panelinde ilgili proje ve derleme bölümünde **artifacts** tabı altında **reports** klasörü içerisinden erişim sağlanabilir.

##### **store_test_results**

```yml
      - store_test_results:
          path: app/build/test-results
```

Projenin derlenmesi sonrasında **app/build/test-results** klasörü altında oluşan test dosyalarını saklamak için özel bi komuttur.

---

## orbs

```bash
version: 2.1
orbs:
    hello: circleci/hello-build@0.0.5
workflows:
    "Hello Workflow":
        jobs:
          - hello/hello-build
```

Önceden hazırlanmış konfigurasyon dosyasını mevcut dosyamıza import ederek hızlıca kullanmamızı sağlar.

Orblar, yapılandırmanızın basitleştirilmesi, paylaşılması ve projelerin içinde ve genelinde yeniden yapılandırılması için name ile içe aktardığınız veya inline yapılandırdığınız yapılandırma paketleridir.

**Not:** **orgs** bloğunu kullanabilmemiz için version numarası min **2.1** olmalıdır.

---

## workflows

**workflows** bloğu, job ların hazırlanmasını ve bunların çalışma sırasını tanımlamak için kullanılır. **Workflows**'lar, hataları daha hızlı çözmenize yardımcı olmak için basit bir yapılandırma anahtarları seti kullanarak karmaşık iş düzenlemesini destekler.

- Gerçek zamanlı durum geri bildirimi ile işleri bağımsız olarak çalıştırma ve sorunları giderme,
- Yalnızca periyodik olarak çalışması gereken işler için iş akışları planlama,
- Verimli sürüm testi için birden çok işi paralel olarak çalıştırma,
- Birden çok platforma hızla dağıtım yapma.

---

## Executor Type

CircleCI, Jobs'ları üç ortamdan birinde çalıştırmanıza olanak tanır:

- docker
- machine
- macos

> **Docker** ve **Machine** ortamları arasındaki farklara göz atmak isterseniz > https://circleci.com/docs/2.0/executor-types/

### docker

```bash
jobs:
  build:
    docker:
      - image: buildpack-deps:trusty
```

### machine

```bash
version: 2
jobs:
  build:
    machine: true
```

- **true** = **circleci/classic:latest** : haftada bir güncelleme alır. 
- **image: circleci/classic:2017-01** : güncelleme almaz. Sabit versiyon. 
- **circleci/classic:edge** : sık sık güncelle alır.

Yukarıda ki üç değerden biri olmak zorundadır.

**machine** ile **jobs**'lar aşağıdaki özelliklere sahip özel ve geçici bir VM'de çalıştırır:

| CPUs | Processor                 | RAM | HD    |
|------|---------------------------|-----|-------|
| 2    | Intel(R) Xeon(R) @ 2.3GHz | 8GB | 100GB |

**machine** yürütücüsünü kullanmak, uygulamanıza OS kaynaklarına tam erişim sağlar ve **job** ortamı üzerinde tam kontrol sağlar. Bu denetim, **ping** kullanmanız veya sistemi **sysctl** komutlarıyla değiştirmeniz gereken durumlarda yararlı olabilir.

**machine** yürütücüsünü kullanmak, Ruby ve PHP gibi diller için ek paketler indirmeden bir Docker görüntüsü oluşturmanıza da olanak tanır.

**Not:** **machine** kullanmak, gelecekteki fiyatlandırma güncellemesinde ek ücretler gerektirebilir.

### macos

```bash
jobs:
  build:
    macos:
      xcode: "9.0"
    steps:
      # Commands will execute in macOS container
      # with Xcode 9.0 installed
      - run: xcodebuild -version
```

macOS işletim sistemi ve XCode gerektiren süreçler için **macos** ortamı kullanılabilir.

### Multiple Docker Images

```bash
jobs:
  build:
    docker:
    # Primary container image where all steps run.
     - image: buildpack-deps:trusty
    # Secondary container image on common network. 
     - image: mongo:2.6.8-jessie
       command: [mongod, --smallfiles]

    working_directory: ~/

    steps:
      # command will execute in trusty container
      # and can access mongo on localhost
      - run: sleep 5 && nc -vz localhost 27017
```

Birden çok image belirtilmiş ise ilk belirtilen image ana ortamı sağlar. **steps** bloğu içerisinde belirtilen tüm komutlar ilk **image** ortamı içerisinde çalıştırılır. Tüm **container** lar ortak bir ağ içerisinde çalıştırılır ve ilk container içerisinden **localhost** ile erişim sağlanabilir.

---
























# Kaynaklar

- https://circleci.com/docs/2.0/
