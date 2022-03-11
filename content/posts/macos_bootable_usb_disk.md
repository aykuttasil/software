---
author: "Aykut Asil"
date: 2022-03-09T22:37:33+03:00
lastmod: 2022-03-09T22:37:33+03:00
title: "Macos_bootable_usb_disk"
url: "macos-bootable-usb-disk"
weight: 10
keywords: ["aykuttasil","software","macos","bootable","usb","disk"]
description: ""
tags: ["software","macos","bootable","usb","disk"]
categories: ["macos","utility"]
draft: true
---

## USB Bellekten Monterey Kurulumu

Geçtiğimiz günlerde atıl durumda duran mac bilgisayarıma format atmak istedim. Fakat maalesef yanlış adımlar neticesinde mac'i komple açamaz hale geldim.

**Çözüm** için **bootable usb disk** oluşturdum ve sorunu hallettim. Tabi bunun için diğer mac bilgisayarımı kullanmam gerekti.

Çözüm için öncelikle [Mac OS Monterey](https://www.apple.com/macos/monterey/) indirmem gerekti. <macappstores://apps.apple.com/us/app/macos-monterey/id1576738294?mt=12> bu adresi kullanarak indirdim. **Applications** klasörüne inen **install** dosyasını kullanarak aşağıdaki komut ile usb belleği bootable hale getirdim.

```bash
sudo /Applications/Install\ macOS\ Monterey.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolumeName
```

Sonrasında usb belleği sorun olan mac bilgisayarıma taktım ve power tuşuna bastıktan sonra **option(ALT)** tuşuna basılı tuttum. Ve karşıma **Install Monterey** penceresi çıktı. Sonrasında normal şekilde kurulumu tamamladım.


## Kaynaklar
- https://support.apple.com/en-us/HT201372