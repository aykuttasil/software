+++
thumbnailImage = ""
desciption = ""
autoThumbnailImage = false
coverImage = "https://c8.staticflickr.com/8/7421/9339731831_9ba94f287c_k.jpg"
date = "2017-01-16T11:13:16+03:00"
keywords = [
  "yazilim",
  "sofware","c#","csharp","operator","soru isareti","null condition"
]
thumbnailImagePosition = "top"
tags = [
  "software","c#","csharp","operator","soru isareti","null condition"
]
title = "CSharp Null Kontrolü"
#url = "csharp-soru-isareti-syntax"
categories = [
  "yazilim",
  "c#",
]
metaAlignment = "center"

+++

# C# ? ve ?? Operator Kullanımı

Kısaca bahsedilecek olursa **?** operatörü **null** kontrolü yapılmasını sağlar.

Yazılım geliştirme sırasında en çok rastlanan hatanın sebebi **null** dönen ifadelerdir. Bu bazen bir değişken bazen parametre vs. olabilir. Ama sebep ifadenin null olmasıdır.

Önceden şu şekilde kontrol ediyorduk.

```c#
var a;
if(a != null)
{
  Console.Write(a);
}
```

şimdi

```c#
var a;
Console.Write(a ?? "boş değer");
```
Yukarıdaki söz dizimi ile şunu söylüyoruz. 

- Eğer **a** değeri null değil ise ekrana a nın değerini yaz.
- Eğer **a** değeri null ise **??** operatörünün sağındaki değeri yani burada **boş değer** ifedesini yaz. 

---

```c#
int? length = customers?.Length; // null if customers is null   
Customer first = customers?[0];  // null if customers is null  
int? count = customers?[0]?.Orders?.Count();  // null if customers, the first customer, or Orders is null  
```

Yukarıdaki örneği incelersek aslında kod kalitemizin ve okunabilirliğin ne kadar arttığını görebiliriz.

```c#
Customers customers = DbHelper.GetCustomers();
```

Yukarıdaki kodu çalıştırdığımızda `DbHelper.GetCustomers()` fonksiyonunda **null** döndüğünü düşünelim.
Biz null kontrolü yapmadan `customers.size()`gibi bir fonksiyonu çağıracak olsak uygulamamız patlıcaktır.
Çünkü null bir ifadenin **size** ı olamaz.

```c#
int? size = customers?.size;
```

yukarıda ki gibi kodumuzu geliştirirsek uygulamamızın patlamasını önlemiş oluruz.


