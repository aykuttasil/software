+++
date = "2017-01-02T14:35:49+03:00"
title = "PendingIntent Flags"

+++

```java
PendingIntent resultPendingIntent = PendingIntent.getActivity(
        getApplicationContext(),
        100,
        intent,
        PendingIntent.FLAG_CANCEL_CURRENT
);
```

PendingIntent ler Android uygulama geliştirilmesi sırasında birçok yerde karşımıza çıkabilir.

Özetle PendingIntent kullanılarak asenkron işlem başlatılımı yapılır.

En genel kullanım alanlarından bir tanesi de Notification lar iledir.

Kullanıcı, hazırlamış olduğumuz notification a tıkladığında alacak olduğu tepki nasıl olmalı? sorusuna cevap vermek için PendingIntent imizin flag parametresini uygun şekilde düzenlemeliyiz.

___

**Not:** PendingIntent düzenlenirken requestCode aynı olmalıdır. Eğer her notif için farklı requestCode verirsek her biri farklı notif gibi davranır. Yani verdiğimiz flag in bir değeri kalmaz.
Uygulamanın akışına göre kimi zaman da requestCode u farklı vermemiz gerekebilir.

Hangi amaçla kullanacağınız tabi ki size ve uygulamanıza kalmış. ;)
___

### **PendingIntent.FLAG_CANCEL_CURRENT**

**FLAG_CANCEL_CURRENT** ile şunu söylüyoruz.
Arka arkaya birden fazla notification gelirse eğer, sen en son geleni dikkate al sadece. Diğerlerinin artık hiç bir önemi yok.
En son hariç diğer notificationlardan birine tıklandığında herhangi bir işlem yapma diyoruz.
___

### **PendingIntent.FLAG_ONE_SHOT**

**FLAG_ONE_SHOT** ile şunu söylüyoruz.
Arka arkaya birden fazla notification gelirse eğer, sen sadece ilk geleni dikkate al. Diğer gelenlerin bi önemi yok.
İlk gelen notif hariç diğerlerinden birine tıklanırsa herhangi bir işlem yapma diyoruz.
___

### **PendingIntent.FLAG_UPDATE_CURRENT**

**FLAG_UPDATE_CURRENT** ile şunu söylüyoruz.
Arka arkaya birden fazla notification gelirse eğer, sen en son geleni dikkate al sadece. Diğerleri önemli değil.
En son hariç diğer notification a tıkladığımızda en son gelen parametrelere göre işlem yap diyoruz.
___





