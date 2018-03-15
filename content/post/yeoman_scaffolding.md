+++
desciption = ""
tags = [
  "software","yeoman"
]
keywords = [
  "yazilim",
  "sofware","yeoman","yeoman scaffolding"
]
thumbnailImagePosition = "top"
title = "Yeoman Kullanımı"
#url = "yeoman-kullanimi"
thumbnailImage = ""
metaAlignment = "center"
categories = [
  "yazilim"
]
autoThumbnailImage = false
coverImage = "https://c8.staticflickr.com/8/7421/9339731831_9ba94f287c_k.jpg"
date = "2017-01-18T12:09:26+03:00"

+++

Yeoman Kullanımı
----

[Yeoman](http://yeoman.io/) ile kendi proje yapımızı kurgulayabilir ve opsiyonlar belirleyerek ona göre dosya içeriğimizi vs. düzenleyebiliriz.
Ya da oluşturduğumuz projeye önceden belirlediğimiz bir dosyayı ekleyebilir ve gerekli ayarlamaları otomatik olarak yapmasını sağlayabiliriz. (yeni bir Controller eklemek gibi vs.)

Ve hazırladığımız bu proje yapısını paket yönetim sistemlerine yollayarak diğer kişilerin kullanımına açabilir veyahut kendimizin de istediğimiz yerden erişebilmesini sağlayabiliriz.

Öncelikle bilgisayarınız da nodejs yüklü olduğunu varsayıyoruz. Eğer yüklü değilse [buradan](https://nodejs.org/en/) gerekli işlemleri yaparak kurabiliriz.

Daha sonra `npm install -g yo` komutunu çalıştırarak `yo`komutunun heryerden çalışmasını sağlıyoruz.

- [Generators](http://yeoman.io/generators/), diğer kişilerin oluşturmuş olduğu yapıları görmek için kullanabiliriz.

**Evet kendi proje yapımızı hazırlamaya başlayabiliriz**

Öncelikle yine **Yeoman** ın kendisini kullanarak yeni yapımızı kurmak için gerekli dosyaların yapılanmasını sağlıyoruz.
Bunun için `npm install -g generator-generator` komutunu çalıştırıyoruz.
Ve `yo generator`komutu ile gerekli dosya yapısının oluşturulmasını sağlıyoruz.

`yo generator` komutunu çalıştırdığımız da bize **proje ismi (name)** gibi sorular gelecek ve bizde uygun değerleri girerek dosyaların ona göre düzenlemesini sağlıcaz. Ama **name** değerini girerken **generators** ile başladığına emin olmalıyız.

Ayrıntılı bilgi [burada](https://github.com/yeoman/generator-generator).

Ve oluşturulan dosya yapısı şu şekilde olmalı;

<a href="http://imgur.com/VEuO0t8"><img src="http://i.imgur.com/VEuO0t8.png" title="source: imgur.com" /></a>

**app/templates** klasörü altındaki tüm dosyalar bizim asıl oluşturmak istediğimiz dosya yapısı. Yani tüm bu işlemler sonrasında `yo xyz`çalıştırdığımız da **templates** klasöründe ki dosyalar oluşturulacaktır.
Tabi ki bizim belirlediğimiz değerler ile bu dosyaların içeriği vs. oluşturulacak. Yoksa bi anlamı kalmaz dimi ;)

Yukarıda ki resimde **a,b,dummytext .txt** ve **testFolder** klasörünü görüyorsunuz.

a.txt
---
```txt
Merhaba A 

Proje ismi : <%= name %>
```

b.txt
---
```text
Dosya B

Değişken değer: <%= degiskenB %>
```

dummy.txt
---
```txt
Dummy
```

dosya içeriklerinin bu şekilde olduğunu varsayalım.

**app/index.js** dosyasını açıyoruz ve gerekli düzenlemeleri yapıyoruz.

```javascript
'use strict';
var Generator = require('yeoman-generator');
var chalk = require('chalk'); // yazı renklendirme için kullanılır
var yosay = require('yosay'); // yeoman çalıştırılırken ilk başta gözüken proje bilgilendirme kısmını hazırlamaka için kullanılır

module.exports = Generator.extend({
  prompting: function () {
    var done = this.async();

    this.log(yosay(
      'Aykut Asil proje oluşturma yapısına Hoş Geldiniz... ' +
      chalk.red('generator-sample-yeoman') +
      ' generator!'
    ));

    var prompts = [{
      type: 'confirm',
      name: 'someAnswer',
      message: 'www.aykutasil.com u ziyaret ettin mi ?',
      default: true
    }, {
      type: 'input',
      name: 'name',
      message: 'Proje adı',
      default: this.appname
    }, {
      type: 'input',
      name: 'degiskenB',
      message: 'B name',
      default: this.appname
    }];

    return this.prompt(prompts).then(function (props) {
      this.props = props;
      this.log(props.someAnswer);
      this.log(props.name);

      done();
    }.bind(this));
  },

  writing: {
    config: function () {
      this.fs.copyTpl( // Eğer hazırlanan dosya içerisinde değişken kullanılmış ise copyTpl ile kopyalama yapılır
        this.templatePath('a.txt'),
        this.destinationPath('a.txt'), {
          name: this.props.name
        }
      );

      this.fs.copyTpl(
        this.templatePath('b.txt'),
        this.destinationPath('b.txt'), {
          degiskenB: this.props.degiskenB
        }
      );

      this.fs.copy( // Hazırlanacak dosya aynen kopyalama yapılacak ise yani dosya içerisinde değişken ile doldurulacak bir bölüm yok ise
        // copy ile kopyalama yapılır
        this.templatePath('dummyfile.txt'),
        this.destinationPath('dummyfile.txt')
      );

      this.fs.copy(
        this.templatePath('testFolder/_test.txt'),
        this.destinationPath('testFolderDeneme/test.txt')
      );
    }
  },

  install: function () {
    // this.installDependencies(); // tüm dosyala kopyalandıktan sonra npm init çalıştırılması için kullanılır. Eğer çalıştırılmazsa node_modules klasörü oluşturulmamış olur.
  }
});

``` 

Yukarıda kodların yanında ayrıntılı açıklama var.

İşlemlerimizi tamamladıktan sonra local imizde test etmek ya da kullanabilmek için `npm link` komutunu çalıştırıyoruz.(ana klasör içerisinde iken çalıştırıyoruz)

Ve herhangi bir yerde yeni bir klasör oluşturalım.
Klasör içerisindeyken `yo sample_generator` komutunu çalıştıralım.


Ve sonuç:

<a href="http://imgur.com/ome2le3"><img src="http://i.imgur.com/ome2le3.png" title="source: imgur.com" /></a>

<a href="http://imgur.com/h5BxC6u"><img src="http://i.imgur.com/h5BxC6u.png" title="source: imgur.com" /></a>

<a href="http://imgur.com/f2QglwV"><img src="http://i.imgur.com/f2QglwV.png" title="source: imgur.com" /></a>

<a href="http://imgur.com/36yb5ij"><img src="http://i.imgur.com/36yb5ij.png" title="source: imgur.com" /></a>



**Ek Kaynaklar**

- https://scotch.io/tutorials/create-a-custom-yeoman-generator-in-4-easy-steps
- https://code.tutsplus.com/tutorials/build-your-own-yeoman-generator--cms-20040





