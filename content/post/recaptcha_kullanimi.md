+++
metaAlignment = "center"
desciption = ""
categories = [
  "yazilim",
]
tags = [
  "software",
  "recaptcha"
]
coverImage = "https://c8.staticflickr.com/8/7421/9339731831_9ba94f287c_k.jpg"
title = "ReCaptcha Kullanımı"
#url = "recaptcha-kullanimi"
thumbnailImagePosition = "top"
thumbnailImage = ""
date = "2017-01-11T00:14:08+03:00"
autoThumbnailImage = false
keywords = [
  "yazilim",
  "sofware",
  "recaptcha"
]

+++

# ReCaptcha

Sitenize gelebilecek saldıralara karşı bir önlem olarak kullanılacak eklentilerden biridir ReCaptcha.

Kolay bir şekilde entegre edilir ve kolay bir şekilde güvenlik sağlanır.

Bunun için aşağıdaki adımları uygulamanız yeterli olacaktır.

- https://www.google.com/recaptcha/admin#list sitesine gidiniz.Ve gerekli yerleri doldurunuz.
- Label kısmına ReCaptcha i hangi sayfada kullanacaksınız,(örneğin giriş sayfası için) GirişCaptcha diyin.
- Domain kısmına sitenizin domain i ekleyin.Her biri bir satır olacak şekilde.Birden fazla domain girerek aynı kodu farklı domainlerde çalıştırabilirsiniz.
- Owner kısmına ise Adınız yazın.

- <a href="http://imgur.com/sQRkz2S"><img src="http://i.imgur.com/sQRkz2S.jpg" title="source: imgur.com" /></a>
- div kısmını ReCaptcha nın nerede gözükmesini istiyorsanız oraya koyun.(form elementinin içersinde olmalıdır.)
- script kısmını site kodunuzun en altına, body tagınızın bi üstüne koyun.
- Client tarafında yapacaklarınız bu kadar.
- Server tarafına geçtiğimizde ise Client tarafındaki formdan gönderilen bilgileri alır gibi “g-recaptcha-response” parametresiyle değeri alıyoruz.Ve ReCaptcha api sinin kullanarak eşleşmenin sağlanığ sağlanmadığına bakıyoruz.
- Bunun için server kısmına
 
 ```c#
        private static bool ControlReCaptcha(string secretKey, string reCaptcha, string remoteIp)
        {
            var secretkey = secretKey;
            var recaptcha = reCaptcha;
            var remoteip = remoteIp;
            var recaptchacommand = "https://www.google.com/recaptcha/api/siteverify?secret=" + secretkey +
                                   "&response=" + recaptcha + "&remoteip=" + remoteip;

            var resp = WebRequest.Create(recaptchacommand).GetResponse();
            var str = resp.GetResponseStream();
            var sr = new StreamReader(str);
            var a = sr.ReadToEnd();
            JObject obj = JObject.Parse(a);
            var sonuc = obj["success"].ToString().ToLower();
            return sonuc == "true";
        }
```
  
fonksiyonunu ekliyoruz.Ve gerekli parametreleri girdiğimizde fonksiyon bize true – false olacak şekilde sonucu döndürüyor.
 

