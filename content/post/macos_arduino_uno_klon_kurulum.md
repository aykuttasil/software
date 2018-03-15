+++
categories = [
  "yazilim",
  "arduino",
]
thumbnailImagePosition = "top"
thumbnailImage = ""
coverImage = "https://c8.staticflickr.com/8/7421/9339731831_9ba94f287c_k.jpg"
date = "2017-01-11T17:22:38+03:00"
desciption = ""
metaAlignment = "center"
keywords = [
  "yazilim",
  "sofware","arduino","uno","klon"
]
title = "Mac OS Arduino Uno (Klon) Kurulumu"
#url = "macos-arduino-uno-klon-kurulum"
tags = [
  "software","arduino","uno","klon"
]
autoThumbnailImage = false

+++

Sizde benim gibi klon Arduino Uno kartınızı çalıştıramayıp bozuk zannettiyseniz doğru yerdesiniz.

Arduino Uno klon kartlarında orijinal Arduino Uno kartına göre farklı bir işlemci kullanılmış ( daha ucuz olanından 😉 ) ve bu yüzden Mac OS kullanıyorsanız ayrı bir driver dosyası indirmeniz gerekmekte.


Orijinal Arduino Kartlarının yapımı iki koldan yürüyor. Bir taraf open source u destekleyen diğer taraf parayı destekleyen insanlardan oluşuyor(muş).

- http://www.arduino.cc   Parayı destekleyen tarafın sitesi

- http://www.arduino.org  Open source u destekleyen taraf

 

Ben **arduino.org** daki Arduino IDE yi kurdum çünkü diğerinde birkaç sorun yaşamıştım ama günün sonunda her iki siteden indirdiğiniz IDE ile de geliştirme yapabilirsiniz.

Dedikten sonra;

Mac OS a klon Arduino Uno driverını yüklemek için (CH341) ->  http://www.wch.cn/download/CH341SER_MAC_ZIP.html
linkinden dosyayı indirin. Çince ama idare edin, Download a basmanız yeter 😉

Ya da bu linki kullanabilirsiniz : http://javacolors.blogspot.com.tr/2014/08/dccduino-usb-drivers-ch340-ch341-chipset.html

Daha sonra terminal e açarak aşağıdaki komutu yazın.

```bash
sudo nvram boot-args="kext-dev-mode=1"
```

Makineye yeniden başlatın.
Artık Arduino Uno yu Mac OS görüyor olması lazım.

Olmadı şu linke bir göz atabilirsin : http://arduino.stackexchange.com/questions/12133/mac-osx-yosemite-no-serial-ports-showing-for-uno-r3

Ya da şu video yu izleyebilirsin : https://www.youtube.com/watch?v=oHqYK1ezRzo

 

Olduysa Arduino IDE nizden port ayarlarını yapmayı unutmayın.

 

