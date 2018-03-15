+++
thumbnailImage = ""
desciption = "TSQL NULLIF Fonksiyonu Nedir ve Nasil Kullanilir"
thumbnailImagePosition = "top"
keywords = [
  "yazilim",
  "sofware",
  "sql",
  "nullif"
]
autoThumbnailImage = false
coverImage = "https://c8.staticflickr.com/8/7421/9339731831_9ba94f287c_k.jpg"
title = "TSQL NULLIF Fonksiyonu Nedir ve Nasıl Kullanılır"
#url = "sql_nullif_fonksiyonu_kullanimi"
metaAlignment = "center"
categories = [
  "yazilim",
  "sql",
]
date = "2017-01-10T23:08:34+03:00"
tags = [
  "software",
  "sql",
  "mssql",
  "tsql",
  "nullif"
]

+++

## TSQL NULLIF fonksiyonu

NULLIF fonksiyonu verilen iki parametre birbirine esit ise NULL deger döndürür eger degerleri fakli iki parametre verilirse sonuç olarak birinci parametrenin degerini döndürür.

Kullanimi ve anlamasi kolay bir fonksiyon oldugu için basit bir örnekle anlatmaya çalisalim.

Ilk olarak kullanacagimiz geçici tabloyu asagidaki gibi çalistirip içine insert komutu ile veri kaydedelim.

```sql
CREATE TABLE #Urun (
UrunID TINYINT,
ListeFiyati DECIMAL NULL); 

GO

INSERT #Urun VALUES(1,100);
INSERT #Urun VALUES(2,NULL);
INSERT #Urun VALUES(3,0);
INSERT #Urun VALUES(7,250);
INSERT #Urun VALUES(9,458);
```

ListeFiyati kolonuna göre count yaparsak sonuç olarak 4 dönecegini görürsünüz, çünkü UrunID’si 2 olan ürünün liste fiyati girilmemistir. Count fonksiyonu null degerleri saymayacagi için 5 degil 4 degerini döndürür. Count(*) veya count(UrunID) yazarsaniz ID kolonu hiç null olmadigi için 5 degerini görürsünüz.

 
```sql
select count(*) from #Urun
--sonuç : 5

select count(UrunID) from #Urun
--sonuç : 5

select count(ListeFiyati) from #Urun
--sonuç 4
```

Simdi gelelim NULLIF fonksiyonuna. Liste fiyati 0 olarak girilen ürünlerin sayiya dahil etmek istemiyorsak, Liste fiyati 0 olanlari NULL ‘a çevirerek count islemini yapabiliriz.

```sql
select NULLIF(ListeFiyati,0) from #Urun
/*
Liste Fiyati 0 olanlari NULL yap.
Sonuç:
100
NULL
NULL
250
458
*/

--Liste Fiyati 0 olanlar hariç sayiyi bulmak istiyorsak. (count fonksiyonunun NULL degerleri saymadigini unutmayalim)
select count(NULLIF(ListeFiyati,0)) from #Urun
--sonuç : 3.
--Çünkü bir ürüne ait liste fiyati 0 di. Onu NULL’ a degistirdik ve ----sayisini getirdik.
```
[Kaynak](http://www.yazilimmutfagi.com/10170/veritabani/sql-server/tsql-nullif-fonksiyonu-nedir-ve-nasil-kullanilir.aspx)



