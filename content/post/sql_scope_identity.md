+++
date = "2017-01-11T17:20:37+03:00"
metaAlignment = "center"
categories = [
  "yazilim",
  "sql",
]
tags = [
  "software","sql","sql_scope_identity","mssql"
]
thumbnailImage = ""
keywords = [
  "yazilim",
  "sofware","sql","sql_scope_identity","mssql"
]
thumbnailImagePosition = "top"
coverImage = "https://c8.staticflickr.com/8/7421/9339731831_9ba94f287c_k.jpg"
title = "SQL SCOPE_IDENTITY()"
#url = "sql_scope_identity"
desciption = ""
autoThumbnailImage = false

+++

**Kısaca özetlemek gerekirse :** insert ile kayıt edilen tablo satırının id sini getirir.

Yapılan son kaydın id sini almak için uzun  yol olarak insert sorgusunu çalıştırdıktan sonra bir select sorgusu atarak ve order by id desc diyerek ilk sıradaki kaydı okuyabilir ve bu kaydın id sini alabiliriz.

Tüm bunları yapmaktansa:

```sql
DECLARE @son_satir_id INT;

INSERT  INTO tbQwerty
        ( 
          Ad ,
          Soyad
        )
VALUES  ( 
          'Aykut'
          'Asil'
        )
SET @son_satir_id = SCOPE_IDENTITY();
```

Ayrıntılı Bilgi  : https://technet.microsoft.com/tr-tr/library/ms190315(v=sql.110).aspx

Ayrıntılı Bilgi : https://ahmetrende.com/2010/11/23/sql-serverda-son-kaydin-id-degerini-almak-scope_identity/


