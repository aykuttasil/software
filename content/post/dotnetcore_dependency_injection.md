---
title: ".Net Core Dependency Injection"
date: 2018-08-11T14:58:39+03:00
lastmod: 2018-08-11T14:58:39+03:00
draft: true
keywords: ["dotnet-core","dotnet","netcore","dependency-injection","di"]
description: ".Net Core Dependency Injection"
tags: ["dotnetcore","dependency-injcection"]
categories: ["dotnet-core",".netcore"]
author: "Aykut Asil"

# You can also close(false) or open(true) something for this content.
# P.S. comment can only be closed
comment: true
toc: true
autoCollapseToc: false
postMetaInFooter: true
hiddenFromHomePage: false

---

# Dependency Injection

Dependency Injection nedir sorusununun cevabını arayanları google a davet ediyorum. .Net Core ile birlikte gelen Dependency Injection yapısı nasıl kullanılır cevabı için okumaya devam edebilirsiniz.

Bir çok dilde dependency injection için harici kütüphaneler harici yapılar kurmak vs. gerekli lakin .net core da bu böyle değil. Hazır bir şekilde bize sunulmuş. Bizden istenen **inject** edilecek sınıfı yazmak ve bu sınıfı **Startup.cs** dosyası içindeki **ConfigureServices** fonksiyonun parametresi olan **IServiceCollection** sınıfına söylemek.

> Bir sınıfı oluştururken bu sınıfın ihtiyaçlarına yönelik yapılandırmalar yapmak gerekir. Eğer uygulama boyunca sadece tek bir instance istiyorsak Singleton patternini kullanarak üretilmesini sağlamamız gerekir. Sınıfa her ihtiyaç duyduğumuzda zaten daha önceden oluşturulmuş olan instance ı çağırıyor olmak ile her çağırdığımızda sınıfın farklı bir instance ını oluşturuyor olmak farklı şeylerdir ve hangi şekilde oluşturmamız gerektiğini ihtiyaçlarımız belirleyecektir.

.Net Core da inject edilecek sınıfın nasıl inject edileceğini IServiceCollection sınıfına eklerken belirtmemiz gerekmektedir.

## Nasıl Yapılır?

```csharp
public void ConfigureServices(IServiceCollection services)
        {
            services.Configure<CookiePolicyOptions>(options =>
            {
                // This lambda determines whether user consent for non-essential cookies is needed for a given request.
                options.CheckConsentNeeded = context => true;
                options.MinimumSameSitePolicy = SameSiteMode.None;
            });


            services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);


            // Transient lifetime services are created each time they're requested. This lifetime works best for lightweight, stateless services.
            services.AddTransient<ILog, MyLogServices>();

            // Scoped lifetime services are created once per request.
            services.AddScoped<ILog, MyLogServices>();

            // Transient lifetime services are created each time they're requested. This lifetime works best for lightweight, stateless services.
            services.AddSingleton<ILog, MyLogServices>();
            services.AddScoped<ILog, MyLogServices>();

        }
```