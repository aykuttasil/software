+++
thumbnailImagePosition = "top"
categories = [
  "yazilim",
  "test"
]
date = "2017-01-11T01:09:39+03:00"
title = "Postman ile API Test Yazımı"
#url = "postman-api-test"
keywords = [
  "yazilim",
  "sofware",
  "postman",
  "test",
  "api"
]
autoThumbnailImage = false
coverImage = "https://c8.staticflickr.com/8/7421/9339731831_9ba94f287c_k.jpg"
desciption = ""
metaAlignment = "center"
thumbnailImage = ""
tags = [
  "software","postman","api test"
]

+++


# POSTMAN

Postman HTTP Request lerinizi istediğiniz şekilde düzenleyip çalıştırabileceğiniz ve test edebileceğiniz bir Chrome eklentisidir.

Bu [linkten](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop) Chrome a ekleyebilirsiniz.

 
**Postman kullanarak API lerinizi test edebilirsiniz..**

Tek bir butona tıklayarak istediğiniz kadar Request i çalıştırabilir ve bu Requst lerden gelen değerleri başka bir Requestinize parametre olarak gönderebilrsiniz.



<a href="http://imgur.com/uNpJFK4"><img src="http://i.imgur.com/uNpJFK4.jpg" title="source: imgur.com" /></a> 

Yukarıdaki ekran Postman i yüklediğinizde açılan ilk ekrandır.

**Enter request URL here** yazan kutucuğa End Point yani istek yapacağımız adres yazılır.

Şuan **GET** olarak gözüken yerden yapılacak olan istediğin cinsi seçilir.**(POST , GET ,PUT ,DELETE vs.)**

- **Authorization** tabından Request gönderilecek adreste bir kimlik doğrulama var ise burdan gerekli parametreler girilebilir.

- **Headers** tabından yapılacak olan istekte bulunması gereken Header bilgileri girilir.(Content-Type gibi)

- **Body** tabından End Point i belirtilmiş adrese gönderilecek veriler girilir.Bu veriiler form-data , urlencoded , raw , binary şeklinde olabilir.

- **Pre-request** script tabını  kullanarak istek yapmadan hemen önce otomatik olarak yapılacak işlemleri belirleyebiliriz.

- **Tests** tabından yapılan isteklere ait çeşitli testler yazabiliriz.

Ve **SEND** butouna tıklayarak istek gönderilir.

---

**No Environment** yazan kısma tıklayarak farklı projeler için Environment tanımlayabilriz.Ve her proje kendine ait Environment  Variable ları kullanır.

**Environment variable** lar **Global** olarak ta tanımlanabilir.Bu şekilde tanımlanmış variable lar tüm oluşturulmuş olan Environment lar içinden erişilebilir.

Sabit environment lar oluşturmak için No Environment tabına tıklanarak bir Environment oluşturulur ve **Manage Environment** diyerek sabit değişkenler oluşturulabilir.

Dinamik olarak oluşturmak için Tests tabı kullanılır.

Ve kullanım şekli :

```
postman.setEnvironmentVariable("key","value")
``` 

Global variable oluşturmak için:

```
postman.setGlobalEnvironmentVariable("key","value");
```

--- 

<a href="http://imgur.com/xZwuAHQ"><img src="http://i.imgur.com/xZwuAHQ.jpg" title="source: imgur.com" /></a>

Yukarıda görüldüğü gibi Requestimize ait sabit değerleri bu şekilde environment variable olarak kaydedebiliriz.

---

<a href="http://imgur.com/LuJbfDN"><img src="http://i.imgur.com/LuJbfDN.jpg" title="source: imgur.com" /></a>

Yukarıda görüldüğü gibi oluşturmuş olduğumuz environment variable ı adres çubuğunda kullandık.Ve url nin devamını kendimiz el ile yazdık.Response olarak dönen değeride yukarıda ki  gibi görebiliyoruz.

---

<a href="http://imgur.com/jqkOC9D"><img src="http://i.imgur.com/jqkOC9D.jpg" title="source: imgur.com" /></a>

Yaptığımız istek sonucu dönen Responsumuzu test etmeye geldi sıra.

Yukarıda ki resimde gördüğünüz gibi gelen response değerinde status diye bir property bulunuyor  mu ? ve responce code değeri 200 mü ? diye basit bir test yazıyoruz.

**SEND** dediğimizde yazdığımız testlerde çalışacak ve Response sonucunu gördüğümüz ekranın üstünde bulunan Tests(2/2) tabına tıklayarak test sonçlarını görebilicez.

---

Şimdi bir Blog yazısı kayıt etmeye çalışalım.

<a href="http://imgur.com/iOr6a5D"><img src="http://i.imgur.com/iOr6a5D.jpg" title="source: imgur.com" /></a>


Bu şekilde yazıp  **SEND** e bastığımız da Response olarak Invalid hatası vericektir.Çünkü Authorization bilgilerini girmemizi istemektedir.Bunu yapabilmemiz aşağıdaki gibi bir token oluşturuyoruz.

 
<a href="http://imgur.com/PIMTrVC"><img src="http://i.imgur.com/PIMTrVC.jpg" title="source: imgur.com" /></a>

<a href="http://imgur.com/optSB3D"><img src="http://i.imgur.com/optSB3D.jpg" title="source: imgur.com" /></a>


Ve oluşturduğumuz token bilgileriniz Blog post ederken kullanabilmek için **Environment Variable** olarak atıyoruz.

---

Ve aşağıdaki gibi Blog Post değerlerimizi güncelliyoruz.

<a href="http://imgur.com/XWxEZRv"><img src="http://i.imgur.com/XWxEZRv.jpg" title="source: imgur.com" /></a>

<a href="http://imgur.com/5hcJ3kJ"><img src="http://i.imgur.com/5hcJ3kJ.jpg" title="source: imgur.com" /></a>

---

Bir Blog mesajı yazarken

```json
{
 "post" : "Merhaba"
}
```
bu şekilde oluşturabileceğiniz gibi

```json
{
 "post" : "{{token}}"
}
```

bu şekilde de kaydetmiş olduğunuz Environment Variable ları değer olarak atayabilirsiniz.

Bu şekil de zincirleme reaksiyonlar oluşturabilir ve bir istek sonucu dönen değeri diğer isteğinize parametre olarak gönderebilirsiniz.


<a href="http://imgur.com/3qNG7qq"><img src="http://i.imgur.com/3qNG7qq.jpg" title="source: imgur.com" /></a>

---

<a href="http://imgur.com/LPrqyEn"><img src="http://i.imgur.com/LPrqyEn.png" title="source: imgur.com" /></a>

Yukarıda görüldüğü gibi environment variable değerinizi Url nize parametre olarak yerleştirebilirsiniz.

Postman de test yazarken kullanabileceğiniz ve çok yararlı olabilecek console.log u aktif etmek için bir kaç ayar yapmanız gerekmekte.

- Chromu u açın ve adres satırına chrome://flags  yazın.
- Çıkan pencerece “package” diye arama yapın (CTRL + F)
- Ve  #debug-packed-apps  enable yapın.

<a href="http://imgur.com/hMi6dle"><img src="http://i.imgur.com/hMi6dle.png" title="source: imgur.com" /></a>

Şimdi Postman ekranına gelin ve herhangi bir yere sağ tıklayarak öğeyi denetle diyin ve Console tabını açın.

Bu ekran da postman de test yazarken kullanmış olduğunuz `console.log(“xyz”)` komutunun sonucunu görebilirsiniz.
