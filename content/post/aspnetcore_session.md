---
title: "Asp.Net Core Session"
date: 2019-11-12T10:17:17+03:00
lastmod: 2019-11-12T10:17:17+03:00
draft: false
keywords: ["aspnetcore","webapi","session"]
description: ""
tags: ["aspnetcore","webapi","session"]
categories: ["aspnetcore","web","api"]
author: "Aykut Asil"
comment: true
toc: true
autoCollapseToc: false
postMetaInFooter: true
hiddenFromHomePage: false
---

## Asp.Net Core Session Kullanımı

**Asp.Net Core 2.1** ve üstü sürüme sahip projelerinizde **Session** kullanmak için **Startup.cs** dosyanızı aşağıdaki yöntemlerden birini seçerek güncellemeniz gerekmektedir.

---

### Yöntem 1

```csharp
        public void ConfigureServices(IServiceCollection services)
        {
            services.Configure<CookiePolicyOptions>(options =>
            {
                // This lambda determines whether user consent for non-essential cookies is needed for a given request.
                options.CheckConsentNeeded = context => true;
                options.MinimumSameSitePolicy = SameSiteMode.None;
            });
            services.AddAntiforgery(o => o.SuppressXFrameOptionsHeader = true);
            services.AddDistributedMemoryCache();
            


            services.AddSession(options =>  // <--------
            {
                // Set a short timeout for easy testing.
                // options.IdleTimeout = TimeSpan.FromSeconds(10);
                options.Cookie.HttpOnly = true;
                options.Cookie.IsEssential = true; // make the session cookie Essential
            });




            services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
        }
```

```csharp
        public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
        {
            loggerFactory.AddSerilog();
            if (env.IsDevelopment())
            {
                app.UseDeveloperExceptionPage();
            }
            else
            {
                app.UseExceptionHandler("/Error");
                app.UseHsts();
                app.UseHttpsRedirection();
            }

            app.UseStaticFiles();
            app.UseCookiePolicy();

            app.UseSession(); // <-----
            
            app.UseMvc();
        }
```

---

### Yöntem 2

```csharp
        public void ConfigureServices(IServiceCollection services)
        {
            services.Configure<CookiePolicyOptions>(options =>
            {
                // This lambda determines whether user consent for non-essential cookies is needed for a given request.
                options.CheckConsentNeeded = context => false;  // <---------------
                options.MinimumSameSitePolicy = SameSiteMode.None;
            });
            services.AddAntiforgery(o => o.SuppressXFrameOptionsHeader = true);
            services.AddDistributedMemoryCache();
            services.AddSession();
            services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
        }
```

```csharp
        public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
        {
            loggerFactory.AddSerilog();
            if (env.IsDevelopment())
            {
                app.UseDeveloperExceptionPage();
            }
            else
            {
                app.UseExceptionHandler("/Error");
                app.UseHsts();
                app.UseHttpsRedirection();
            }

            app.UseStaticFiles();
            app.UseCookiePolicy();

            app.UseSession(); // <-----
            
            app.UseMvc();
        }
```

## Neden Asp.Net Core 2.1 ve sonrası?

2018'de uygulanması zorunlu hale gelen [Avrupa Veri Koruma Kanunu](https://eugdpr.org) ile birlikte, şirketlerin kişisel verileri kullanması konusunda uygulaması gereken bazı zorunluluklar oluştu. Bu zorunluluklardan şu an için bizi ilgilendiren kısmı **cookie**lerin kullanımı. Web siteleri **cookie** kullanımı için son kullanıcının onayını almak zorunda.

Tüm bu gelişmeler neticesinde **Microsoft .Net Core** ekibi, version **2.1 ve sonrası** için **cookie** kullanımına dair bazı özelleştirmeler yapmış.

**Session**'lar da bir anlamda **cookie** sayıldığından bu kısıtlamaya maruz kalmış durumda. Default olarak **Non-Essential** kategorisine giren **Session** kullanımı için kullanıcının rızası alınmak zorunda. Ya da yukarıda gösterilen yöntemlerden biri kullanılması gerekmektedir.



## Kaynaklar

- <https://andrewlock.net/session-state-gdpr-and-non-essential-cookies>
- <https://www.microsoft.com/tr-tr/microsoft-365/growth-center/resources/5-things-to-know-about-gdpr-before-its-too-late>
- <https://eugdpr.org/>
- <https://docs.microsoft.com/en-us/aspnet/core/security/gdpr?view=aspnetcore-2.2>
