---
author: "Aykut Asil"
date: 2020-11-04T13:14:18+03:00
lastmod: 2020-11-04T13:14:18+03:00
title: "Android Fastlane Kurulumu"
url: "fastlane"
weight: 10
keywords: ["software","android","mobil","fastlane","automation","ci/cd"]
description: "Fastlane "
tags: ["software","android","mobil","fastlane","automation","ci/cd"]
categories: ["android","mobil"]
---

**[Fastlane](https://fastlane.tools/)**, mobil ile ilgili neredeyse tüm süreçleri **(build,test,deploy vs.)** otomatize etmeye yarayan bir araçtır. **Fastlane** kullanarak hangi **CI/CD** platformunu kullanıyor olursanız olun uygulama süreçleriniz tekil hale getirebilirsiniz.

### Kurulum

```bash
brew install fastlane
```

**Fastlane** aracının kurulumunu tamamladıktan sonra, ilk olarak projenizin root klasörüne gelip `fastlane init` komutunu çalıştırmalısınız.

- Komut satırında **Package Name** istenildiğinde uygulamamızın **package name**'ini (com.example.myapplication) giriyoruz. Diğer adımları şimdilik es geçebilirsiniz.

Proje yapılandırması tamamlandığında **app** klasörü ile aynı seviyede **fastline** isimli bir klasör oluşacak. Ve bu klasör içerisinde bulunan **Fastfile** isimli dosyayı projemizi yapılandırmak için kullanıcaz.

Dosyayı aşağıdaki gibi güncelleyebilirsiniz.

```
# This file contains the fastlane.tools configuration
# You can find the documentation at https://docs.fastlane.tools
#
# For a list of all available actions, check out
#
#     https://docs.fastlane.tools/actions
#
# For a list of all available plugins, check out
#
#     https://docs.fastlane.tools/plugins/available-plugins
#

# Uncomment the line if you want fastlane to automatically update itself
update_fastlane

default_platform(:android)

platform :android do

  desc "Build"
  lane :build do
    gradle(task: "clean assembleRelease")
  end

  desc "Runs all the tests"
  lane :test do
    gradle(task: "test")
  end

  desc "Submit a new Beta Build to Crashlytics Beta"
  lane :beta do
    build

    crashlytics
  
    # sh "your_script.sh"
    # You can also use other beta testing services here
  end

  desc "Deploy a new version to the Google Play"
  lane :deploy do
    build

    upload_to_play_store
  end
end
```

Komut satırına `fastlane build` ya da `fastlane test` yazarak ilgili **lane**'i çalıştırabilirsiniz.

Ya da daha hızlı çalıştırabilmek için `bundle exec fastlane test` şeklinde yazabilirsiniz.

Eğer **bundle exec** komutunda bir problem yaşarsanız komut satırında belirttiği şekilde **bundle** aracının güncellenmesini sağlayabilirsiniz. Sonrasında problem ortadan kalkacaktır.



### Kaynaklar

- <https://fastlane.tools/>
- <https://www.raywenderlich.com/10187451-fastlane-tutorial-for-android-getting-started>