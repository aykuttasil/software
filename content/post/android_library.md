+++
title = "Android Library & Bintray"
coverImage = "https://c8.staticflickr.com/8/7421/9339731831_9ba94f287c_k.jpg"
keywords = [
  "yazilim",
  "sofware","android","java","library","bintray","gradle"
]
date = "2017-01-11T17:27:29+03:00"
metaAlignment = "center"
tags = [
  "software","android","java","library","bintray","gradle"
]
categories = [
  "yazilim",
  "android",
]
autoThumbnailImage = false
thumbnailImagePosition = "top"
thumbnailImage = ""
desciption = ""

+++

Android uygulaması yazarken bazı oluşturmuş olduğunuz yapıları tekrar tekrar yazmak durumunda kalıyorsanız, sizin de artık kendi kütüphanenizi yazmanızın zamanı gelmiş geçiyor demektir. Böyle bir durum söz konusu olmak zorunda değil tabi library oluşturmak için 🙂

Bu yazının konusu Android Library oluşturmak, oluşturmuş olduğumuz bu kütüphaneyi **maven** ve **jcenter** repository e deploy etmek olucak.

Ve başlayabiliriz.

İlk olarak **Android Studio** yu açarak **File > New > New Project** e tıklayıp yeni bir proje oluşturalım.

Daha sonra oluşturmuş olduğunuz app e sağ tıklayıp **New > Module** sekmesinden gerekli yerleri doldurup projemize ekleyelim.

<a href="http://imgur.com/PQRghY8"><img src="http://i.imgur.com/PQRghY8.png" title="source: imgur.com" /></a>

<a href="http://imgur.com/IV1YJmw"><img src="http://i.imgur.com/IV1YJmw.png" title="source: imgur.com" /></a>

<a href="http://imgur.com/nQHzaY7"><img src="http://i.imgur.com/nQHzaY7.png" title="source: imgur.com" /></a>

Oluşturmuş olduğumuz module içerisine kodlarımızı yazıyoruz. Module ümüzün sanki bir projeymiş gibi kendine ait dosyaları vardır. Yani kendine ait bir AndroidManifest.xml dosyası , kendine ait drawable klasörü vs. vardı. İstediğimiz şekilde özelleştirme yapabilir ve istediğimiz herhangi bir projede bu kodları kullanabiliriz.(Zaten yapılmış olanları bu şekilde kullanıyoruz. Neyin nasıl yapıldığını anlamak her zaman için ufkumuzu genişletecektir…)

Kodlarımızı yazdık. Sıra geldi module ümüzün gradle dosyasını düzenlemeye.

Ama bundan önce Project düzeyindeki gradle dosyasını güncellememiz gerekli.

```
buildscript {
    repositories {
        jcenter()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:2.1.0'
        classpath 'com.jfrog.bintray.gradle:gradle-bintray-plugin:1.4'
        classpath 'com.github.dcendents:android-maven-gradle-plugin:1.3'
    }
}

allprojects {
    repositories {
        jcenter()
    }
}

task clean(type: Delete) {
    delete rootProject.buildDir
}
```

<a href="http://imgur.com/3uET4kz"><img src="http://i.imgur.com/3uET4kz.png" title="source: imgur.com" /></a>

 
Ve module ümüzün **gradle** dosyasını düzenliyoruz.

 
```
apply plugin: 'com.android.library'
apply plugin: 'com.github.dcendents.android-maven' // maven repository e eklememiz için gerekli gradle plugin
apply plugin: "com.jfrog.bintray" // bintray yapılandırması yapmamız için gerekli gradle plugin
// Kodların devamında göreceğiniz hazır yazılmış gradle tasklerini getiren yapıda, güncellenmesi gereken yerleri
// bu şekilde düzenliyoruz.
ext {
    PUBLISH_GROUP_ID = 'com.aykuttasil'
    PUBLISH_ARTIFACT_ID = 'androidbasichelper'
    PUBLISH_VERSION = '1.0.0'
}

android {
    compileSdkVersion 23
    buildToolsVersion "23.0.3"

    defaultConfig {
        minSdkVersion 17
        targetSdkVersion 23
        versionCode 1
        versionName "1.0.0"
    }
    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
}

// Oluşturmuş olduğumuz kütüphane aşağıdaki bağımlılıklara sahip. Sanki bir uygulama geliştiriyormuş gibi 
// bağımlılık dosyalarını ekliyoruz.
dependencies {
    testCompile 'junit:junit:4.12'
    compile 'com.android.support:appcompat-v7:23.4.0'
    compile 'com.afollestad.material-dialogs:core:0.8.5.9'
    compile 'com.joanzapata.iconify:android-iconify-fontawesome:2.1.1'
    compile 'com.android.support:appcompat-v7:23.4.0'
    compile 'com.android.support:design:23.4.0'
}

// Aşağıda ki linke girip bakarsanız aslında gradle task yazılmış olduğunu görürsünüz. Bu yazılan task ler ile
// oluşturmuş olduğumuz module dosyalarından gerekli dosyalara dönüşümlerini sağlıyoruz.
apply from: 'https://raw.githubusercontent.com/blundell/release-android-library/master/android-release-aar.gradle'

def siteUrl = 'https://github.com/aykuttasil/AndroidBasicHelper'      // Homepage URL of the library
def gitUrl = 'https://github.com/aykuttasil/AndroidBasicHelper.git'   // Git repository URL
group = "com.aykuttasil"

install {
    repositories.mavenInstaller {
        pom {
            project {
                packaging 'aar'

                name 'com.aykuttasil:androidbasichelper' // TODO
                description = 'Android Basic Helper' // TODO
                url siteUrl

                // Set your license
                licenses {
                    license {
                        name 'The Apache Software License, Version 2.0'
                        url 'http://www.apache.org/licenses/LICENSE-2.0.txt'
                    }
                }
                developers {
                    developer {
                        id 'aykuttasil' // TODO
                        name 'Aykut Asil' // TODO
                        email 'aykuttasil@gmail.com' // TODO
                    }
                }
                scm {
                    connection gitUrl
                    developerConnection gitUrl
                    url siteUrl
                }
            }
        }
    }
}
```

<a href="http://imgur.com/NPUZYLU"><img src="http://i.imgur.com/NPUZYLU.png" title="source: imgur.com" /></a>

<a href="http://imgur.com/waSW8fe"><img src="http://i.imgur.com/waSW8fe.png" title="source: imgur.com" /></a>

Evet gradle dosyalarını düzenledik. Sıra geldi çalıştırmaya.

Bunun için Android Studio içerisinde ki Terminal kısmına giriyoruz ya da mac in kendi Terminal ini açarak projemizin dizinine giriyoruz.

Ve aşağıdaki komutu giriyoruz.

`./gradlew clean build generateRelease`
 

<a href="http://imgur.com/qd5tC3N"><img src="http://i.imgur.com/qd5tC3N.png" title="source: imgur.com" /></a>

<a href="http://imgur.com/hIScB2P"><img src="http://i.imgur.com/hIScB2P.png" title="source: imgur.com" /></a>

<a href="http://imgur.com/D48QUqE"><img src="http://i.imgur.com/D48QUqE.png" title="source: imgur.com" /></a>

<a href="http://imgur.com/cgp1Abp"><img src="http://i.imgur.com/cgp1Abp.png" title="source: imgur.com" /></a>

Evet işlemimize başarılı bir şekilde tamamladık ve gerekli dosyaların oluşturulmasını sağladık.

Projemizi oluşturduğumuz klasöre gidelim ve **ModuleName > build** içerisine girerek oluşturulan zip dosyasını  görebiliriz. Bu zip dosyasının içini açarak oluşturulan dosyaları görebilirsiniz. Ve maven repository e bu zip dosyasını ekliceğimizi unutmayın.

<a href="http://imgur.com/id0McpR"><img src="http://i.imgur.com/id0McpR.png" title="source: imgur.com" /></a>

--- 

Sıra geldi kütüphanemizi maven ve jcenter repository e deploy etmeye.

İlk olarak https://bintray.com/ adresine girerek üyeliğimizi oluşturuyoruz. Ve sırasıyla aşağıdaki işlemleri yapıyoruz.

 

Hesabımızı oluşturduktan sonra **maven** içerisine girerek **Add New Package** diyoruz.

<a href="http://imgur.com/AGY1vSx"><img src="http://i.imgur.com/AGY1vSx.png" title="source: imgur.com" /></a>

Aşağıdaki alanlara gerekli bilgileri giriyoruz.

<a href="http://imgur.com/82CCD5F"><img src="http://i.imgur.com/82CCD5F.png" title="source: imgur.com" /></a>

Daha sonra oluşturmuş olduğumuz package ın içerisine girerek **New Version** a tıklıyoruz ve **1.0.0** şeklinde ya da istediğiniz şekilde Name  i düzenliyoruz.

<a href="http://imgur.com/7qNM2aj"><img src="http://i.imgur.com/7qNM2aj.png" title="source: imgur.com" /></a>

<a href="http://imgur.com/1bC8KSi"><img src="http://i.imgur.com/1bC8KSi.png" title="source: imgur.com" /></a>

Oluşturmuş olduğumuz versiyon içerisine girerek **Upload Files** diyoruz ve daha önce oluşturduğumuz **zip** dosyasını seçerek upload işlemini tamamlıyoruz.

<a href="http://imgur.com/T2oG4DP"><img src="http://i.imgur.com/T2oG4DP.png" title="source: imgur.com" /></a>

<a href="http://imgur.com/w99RPib"><img src="http://i.imgur.com/w99RPib.png" title="source: imgur.com" /></a>


Kütüphanemizi **maven** respository yüklemiş bulunmaktayız.

Fakat Android Studio default olarak **jcenter** resository kullanmakta. Biz de bu yüzden birkaç işlem daha yapmalıyız ki geliştiriciler kolayca, tek satır ekleyerek kütüphanemizi kullanmaya başlasınlar.

Aşağıda ki sayfaya gelerek **Add to jCenter** butonuna tıklıyoruz.

<a href="http://imgur.com/lMQR4kl"><img src="http://i.imgur.com/lMQR4kl.png" title="source: imgur.com" /></a>

Ve aşağıdaki gibi gerekli yerleri doldurarak işlemimizi tamamlıyoruz.

<a href="http://imgur.com/GK7Eq9d"><img src="http://i.imgur.com/GK7Eq9d.png" title="source: imgur.com" /></a>


**Not :** Kütüphanemizin kullanımı için birkaç saat beklememiz gerekmektedir. Olmadı diye telaş yapıp kafayı yemeyin 😉


Bu arada oluşturmuş olduğumuz kütüphanemize aşağıda ki linkten erişip projelerinizde gönül rahatlığıyla kullanabilirsiniz 🙂

- **Gradle :** `compile ‘com.aykuttasil:androidbasichelper:1.0.0’`

- **GitHub :** https://github.com/aykuttasil/AndroidBasicHelper

 

---

Yukarıda ki yöntem ile local inizde **aar** dosyası oluşturarak manuel bir şekilde bu aar yi repository lere yükleyebilirsiniz.

Peki bunu sadece birkaç komuta indirgesek ve otomatik yüklemeyi sağlasak nasıl olur ? Bencede güzel olur.

 

Yukarıda ki yöntemin yine bir benzeri (daha okunabilir ve kolay) ve artı ilaveleri şeklinde devam edelim.

 

Yeni dosya yapımız ve gradle dosyalarımızı yeri aşağıdaki resimdeki gibi olmalı. Burada 3 adet kendimizin oluşturduğu gradle dosyasını görüyorsunuz gradle klasörünün altında. Diğer gradle lar zaten Android Studio tarafından oluşturuluyor.

<a href="http://imgur.com/cQyvhZb"><img src="http://i.imgur.com/cQyvhZb.png" title="source: imgur.com" /></a>


**build.gradle** dosyalarımızın içeriği aşağıdaki gibidir.

<a href="http://imgur.com/0M6faiE"><img src="http://i.imgur.com/0M6faiE.png" title="source: imgur.com" /></a>

<a href="http://imgur.com/fO5aXX8"><img src="http://i.imgur.com/fO5aXX8.png" title="source: imgur.com" /></a>

```
ext {
    bintrayRepo = 'maven' // maven reposu olduğunu belirtiyoruz
    bintrayName = 'basic-helper' 
    orgName = 'aykuttasil'

    publishedGroupId = 'com.aykuttasil'
    libraryName = 'Android Basic Helper'
    artifact = 'androidbasichelperlib' // module ile aynı isimde olması gerekiyor !!

    libraryDescription = 'Android Basic Helper'

    siteUrl = 'https://github.com/aykuttasil/AndroidBasicHelper' 
    gitUrl = 'https://github.com/aykuttasil/AndroidBasicHelper.git'

    libraryVersion = rootProject.ext.libraryVersion

    developerId = 'aykuttasil'
    developerName = 'Aykut Asil'
    developerEmail = 'aykuttasil@gmail.com'

    licenseName = 'The Apache Software License, Version 2.0'
    licenseUrl = 'http://www.apache.org/licenses/LICENSE-2.0.txt'
    allLicenses = ["Apache-2.0"]
}

if (project.rootProject.file('local.properties').exists()) {
    apply from: rootProject.file('gradle/install-v1.gradle')
    apply from: rootProject.file('gradle/bintray-android-v1.gradle')
}


// ./gradlew clean install bintrayUpload
```

```
ext {
    sdk = 24
    buildTools = "24.0.1"
    minSdk = 17
    libraryVersion = "1.0.20"
    libraryVersionCode = 11
    supportVersion = "24.2.0"
}
```

Aşağıda ki resimde gördüğünüz gibi (Android Studio Project görünümüne geçmelisiniz) 3 adet gradle dosyamızı hazırladık. Kod kalabalığı olmasın ve neyin nerede olduğu belli olsun diye bu şekilde yaptık. Yoksa direk build.gradle dosyamızın içine de yazabilirdik.

<a href="http://imgur.com/6coc2bg"><img src="http://i.imgur.com/6coc2bg.png" title="source: imgur.com" /></a>

```
apply plugin: 'com.github.dcendents.android-maven'

group = publishedGroupId                               // Maven Group ID for the artifact

install {
    repositories.mavenInstaller {
        // This generates POM.xml with proper parameters
        pom {
            project {
                packaging 'aar'
                groupId publishedGroupId
                artifactId artifact

                // Add your description here
                name libraryName
                description libraryDescription
                url siteUrl

                // Set your license
                licenses {
                    license {
                        name licenseName
                        url licenseUrl
                    }
                }
                developers {
                    developer {
                        id developerId
                        name developerName
                        email developerEmail
                    }
                }
                scm {
                    connection gitUrl
                    developerConnection gitUrl
                    url siteUrl

                }
            }
        }
    }
}
//from https://github.com/workarounds/bundler/blob/master/gradle/install-v1.gradle
```


```
apply plugin: 'com.jfrog.bintray'

version = libraryVersion

task sourcesJar(type: Jar) {
    from sourceSets.main.allSource
    classifier = 'sources'
}

task javadocJar(type: Jar, dependsOn: javadoc) {
    classifier = 'javadoc'
    from javadoc.destinationDir
}
artifacts {
    archives javadocJar
    archives sourcesJar
}

// Bintray
Properties properties = new Properties()
properties.load(project.rootProject.file('local.properties').newDataInputStream())

bintray {
    user = properties.getProperty("bintray.user")
    key = properties.getProperty("bintray.apikey")

    configurations = ['archives']
    pkg {
        repo = bintrayRepo
        name = bintrayName
        desc = libraryDescription
        userOrg = orgName
        websiteUrl = siteUrl
        vcsUrl = gitUrl
        licenses = ['Apache-2.0']
        publish = true
        publicDownloadNumbers = true
        version {
            desc = libraryDescription
            gpg {
                sign = true //Determines whether to GPG sign the files. The default is false
                passphrase = properties.getProperty("bintray.gpg.password")
                //Optional. The passphrase for GPG signing'
            }
        }
    }
}

//from https://github.com/workarounds/bundler/blob/master/gradle/bintray-java-v1.gradle
```

```
apply plugin: 'com.jfrog.bintray'

version = libraryVersion

task sourcesJar(type: Jar) {
    from android.sourceSets.main.java.srcDirs
    classifier = 'sources'
}

task javadoc(type: Javadoc) {
    failOnError = false
    source = android.sourceSets.main.java.srcDirs
    classpath += project.files(android.getBootClasspath().join(File.pathSeparator))
}

task javadocJar(type: Jar, dependsOn: javadoc) {
    classifier = 'javadoc'
    from javadoc.destinationDir
}

artifacts {
    archives javadocJar
    archives sourcesJar
}

// Bintray
Properties properties = new Properties()
properties.load(project.rootProject.file('local.properties').newDataInputStream())

bintray {
    user = properties.getProperty("bintray.user")
    key = properties.getProperty("bintray.apikey")

    configurations = ['archives']
    pkg {
        repo = bintrayRepo
        name = bintrayName
        desc = libraryDescription
        userOrg = orgName
        websiteUrl = siteUrl
        vcsUrl = gitUrl
        licenses = allLicenses
        publish = true
        publicDownloadNumbers = true
        version {
            desc = libraryDescription
            gpg {
                sign = true //Determines whether to GPG sign the files. The default is false
                passphrase = properties.getProperty("bintray.gpg.password")
                //Optional. The passphrase for GPG signing'
            }
        }
    }
}
```

Tüm bu dosya yapısını hazırladıktan sonra bintray konfigürasyonu için birkaç işlem daha kaldı.

**local.properties** dosyamızın içine bintray bilgilerimizi giriyoruz. Bu bilgileri direk build.gradle içerisine girebilirdik fakat bu bilgilere sadece biz sahip olmalıyız 🙂 ve gitignore un içinde local.properties dosyasının ekli olduğuna dikkat edelim !!

<a href="http://imgur.com/hSrkkEc"><img src="http://i.imgur.com/hSrkkEc.png" title="source: imgur.com" /></a>

 

Bintray api key almak için bintray sitesine giderek gerekli sayfaya ulaşıyoruz.

<a href="http://imgur.com/bmNY7Is"><img src="http://i.imgur.com/bmNY7Is.png" title="source: imgur.com" /></a>

<a href="http://imgur.com/Vot0Fkk"><img src="http://i.imgur.com/Vot0Fkk.png" title="source: imgur.com" /></a>

 
Evet tüm işlemler bu kadar.

Artık terminalden aşağıdaki komutu yazarak otomatik yüklenmeyi sağlayabiliriz.

`./gradlew clean install bintrayUpload`
 

 

