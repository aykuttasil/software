---
date : "2017-01-11T01:00:19+03:00"
title : "Android ActiveAndroid Kullanımı"
#url : "activeandroid-kullanimi"
metaAlignment : "center"
tags : [
  "software","android","java","sqlite","activeandroid","orm"
]
autoThumbnailImage : false
thumbnailImagePosition : "top"
thumbnailImage : ""
categories : [
  "yazilim",
  "android",
  "orm"
]
desciption : ""
keywords : [
  "yazilim",
  "sofware",
  "activeandroid",
  "sqlite",
  "orm"
]
coverImage : "https://c8.staticflickr.com/8/7421/9339731831_9ba94f287c_k.jpg"
---

Anroid ile ORM (Object Relational Mapping) kullanarak veritabanı işlemlerinizi oldukça kolay yapabilirsiniz.

ActiveAndroid kütüphanesi ORM kütüphanelerinden biridir.

Kütüphaneyi [buradan](https://github.com/pardom/ActiveAndroid) indirebilirsiniz.

Kullanımı oldukça basittir. Sadece birkaç noktaya özellikle dikkat edilmesi gerekiyor. Bunlardan biri oluşturmuş olduğunuz tabloya yeni alanlar eklediğiniz de tablonuzu güncellemek. Veya herhangi bir sebeple tabloda çeşitli işlemler yapmak.

Aşağıda kısaca Tablo oluşturmaktan ve gerektiğinde Tablo yu nasıl güncelleyeceğimizden bahsedicem.

```java
@Table(name = "Items")
public class ModelSampleItem extends Model {

    @Column(name = "Name")
    public String name;

    @Column(name = "Surname")
    public String surname;

    @Column(name = "Phone")
    public String phoneNumber;

    @Column(name = "Xyz")
    public String xyz;

    @Column(name = "EMail")
    public String email;

    @Column(name = "TryColumn")
    public String tryColumn;


    @Column(name = "Column3")
    public String column3;

    

    public ModelSampleItem() {
        super();

    }
}
```

Yukarıda ki yapıyı kullanarak istediğiniz Tablo yu oluşturmanız mümkün.

Tabi bundan önce yapmamız gereken birkaç ayar var.

Android uygulamamızın Manifest dosyasına custom Application Name tanımlıyoruz ve bu isimde bir Class oluşturuyoruz.

```xml
<application
    android:name=".app.AppController"
```

ve Application dan türeyen sınıfımız

```java
public class AppController extends com.activeandroid.app.Application {


    @Override
    public void onCreate() {
        super.onCreate();

    }
}
```

`com.activeandroid.app.Application`

Yukarıda ki sınıftan türetmek  aslında **onCreate()** içerisinde **ActiveAndroid.initializ(this)** ile aynı anlama gelmektedir.Ama yukarıda ki gibi yaparsak daha güzel bir görünüm olacaktır.

Ve aşağıdaki meta-taglarını `<application>` tagları arasıne ekliyoruz.

```xml
<meta-data
    android:name="AA_DB_NAME"
    android:value="newiztop1.db" />
<meta-data
    android:name="AA_DB_VERSION"
    android:value="1" />
```

Yapılandırma ayarlarımız bu kadar. Artık **Items** benzeri tablolarınızı yukarıda ki gibi yazmanız ve uygulamanızı çalıştırmanız yeterli olacaktır.

 

Yukarıda ki tabloya sonradan bir alan eklememiz gerekti. Uygulamamız büyüdükçe nelere ihtiyacı olacağını kestirmek imkansıza yakındır.

```java
@Column(name = "Column4")
    public String column4;
```

Alanımızı Items sınıfımıza ekliyoruz. Ama çalıştırdığımız da uygulamamız hata verecektir.

Uygulamamıza yeni alan eklediğimizi bildirmek için biraz kıvranmamız gerekmekte.

Öncelikle **assets** klasörünün içine migrations isilmli bir klasör oluşturuyoruz. Bu ismi vermek zorunludur. assets/migrations

Ve oluşturduğumuz migrations klasörüne **2.sql** isimli bir dosya oluşturuyoruz.Niye 2 peki?

```xml 
<meta-data
    android:name="AA_DB_NAME"
    android:value="newiztop1.db" />

<meta-data
    android:name="AA_DB_VERSION"
    android:value="1" />
```

Android Manifest dosyasına yapılandırırken **DB_VERSION** olarak 1 verdik. Bura da belirttiğimiz sayının bir fazlası olması gerekiyor *.sql dosyamızın ismi.

Ve **2.sql** dosyamızın içine

```sql
BEGIN TRANSACTION;
ALTER TABLE Items ADD COLUMN Column4 TEXT;
COMMIT;
```

ekliyoruz.

Ve çalıştırdığımız da **Items** tablomuza **Column4** isimli bir alan eklenmiş olacaktır.Artık bu alanımızı da gönül rahatlığıyla kullanabiliriz.

 

**Items Tablomuza herhangi bir satır eklemek istersek :**

```java
ModelSampleItem msi = new ModelSampleItem();
msi.name = "Aykut";
msi.surname = "Asil";
msi.phoneNumber = "535";
msi.email = "huuuu@gmail.com";
msi.xyz = "xy<";
msi.Column4 = "merhaba";
msi.save();
```
dememiz yeterli olacaktır.

 
**Kaydedilmiş bir veriyi okumak istersek :**

```java
ModelSampleItem myModel = Model.load(ModelSampleItem.class, 6); // id si 6 olan veriyi getirir
```
veya

```java
List<ModelSampleItem> listMOdel = new Select().from(ModelSampleItem.class).execute(); // Tüm verileri List şeklinde getirir.
``` 

İhityacınıza yönelik sorgulamalar yapabilirsiniz. Tek yapmanız gereken biraz kurcalamak.

