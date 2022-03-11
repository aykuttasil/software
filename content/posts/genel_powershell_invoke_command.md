---
author: "Aykut Asil"
date: 2020-12-21T13:01:16+03:00
lastmod: 2020-12-21T13:01:16+03:00
title: "PowerShell Invoke-Command ile Uzak Bilgisayarda Komut Çalıştırma"
url: "powershell-invokecommand-ile-uzak-bilgisayarda-komut-calistirma"
weight: 10
keywords: ["aykuttasil","software","powershell","invoke-command","ci/cd","remote-command"]
description: "PowerShell Invoke-Command kullanımı. .Net Core projelerinin deploy edilebilmesi için app_offline.htm dosyasının oluşturulması"
tags: ["software","powershell","invoke-command","ci/cd","remote-command"]
categories: ["genel","devops"]
---

## Senaryo

Şu anda çalışmış olduğum şirkette, kendi local sunucularımız üzerine kurmuş olduğumuz **Teamcity** ile projelerin derlenip sunulma aşamasını otomatize etmeye çalışıyoruz. **Teamcity** kurmuş olduğumuz sunucu ile projelerin **deploy** olacağı sunucular farklı. Biz şimdilik bu deploy sürecini **teamcity** built-in FTP özelliğini kullanarak hallediyoruz. Fakat **.net core** projelerinin deploy olma sürecinde yaşanan bir problem var. Eğer **.net core** projeniz **IIS** arkasında konuşlanma şeklinde ayarlanmış ise **IIS**, projenizin **.exe** dosyasını çalıştıyor olacak ve bu çalışma süresince siz bu dosyayı değiştirme, silme vb. işlemlere sokamıyor olacaksınız. Dolayısıyla otomatize edilmiş deploy süreci bu adımda patlıyor olacak. Çözüm olarak **microsoft**'un önerdiği birkaç yöntem var. Bunlardan biri deploy adımında **IIS**'i durdurmak ve sonrasında tekrar çalıştırmak. Bir diğeri **app_offline.htm** dosyasını proje **root** klasörüne kopyalamak ve sonrasında silmek.

Biz şu an için **app_offline.htm** dosyasını oluşturup, deploy sonrasında silme seçeneğini seçtik.

1. Deploy adımı çalışmadan önce aşağıdaki **Powershell** komutu ile uzak sunucuya bağlanarak projemizin **root** klasöründe **app_offline.htm** dosyası oluşturuyoruz:


```bash
$Username = 'uzak_masaustu_kullanicisi'
$Password = 'uzak_masaustu_kullanici_sifresi'
$pass = ConvertTo-SecureString -AsPlainText $Password -Force
$credential = new-object -typename System.Management.Automation.PSCredential -argumentlist $Username,$pass
Invoke-Command -ComputerName uzakmasaustu.computername.com -Credential $credential -ScriptBlock { New-Item -Path "D:\proje\root\path" -Name app_offline.htm }
```

2. Deploy adımı

3. Ve projenin **IIS** tarafından tekrar çalıştırılması için **app_offline.htm** dosyasını siliyoruz.

```bash
$Username = 'uzak_masaustu_kullanicisi'
$Password = 'uzak_masaustu_kullanici_sifresi'
$pass = ConvertTo-SecureString -AsPlainText $Password -Force
$credential = new-object -typename System.Management.Automation.PSCredential -argumentlist $Username,$pass
Invoke-Command -ComputerName uzakmasaustu.computername.com -Credential $credential -ScriptBlock { Remove-Item -Path "D:\proje\root\path\app_offline.htm" }
```


