---
author: "Aykut Asil"
date: 2021-06-24T17:51:01+03:00
lastmod: 2021-06-24T17:51:01+03:00
title: ".Net Sqlite Konfigürasyonu"
url: "dotnet-sqlite-configuration"
weight: 10
keywords: ["aykuttasil","software","sqlite","aspnetcore",".net5","dotnet"]
description: ".Net 5 SQLite Konfigürasyonu"
tags: ["software","sqlite","aspnetcore",".net5","dotnet"]
categories: ["aspnetcore"]
---

> CLI ile Entity Framework işlemleri gerçekleştirmek için **dotnet-ef** toolunu yüklemelisiniz. 

```bash
dotnet tool install --global dotnet-ef
dotnet add package Microsoft.EntityFrameworkCore.Design
```

> SQLite provider'ı için ilgili paketi yüklemelisiniz

```bash
dotnet add package Microsoft.EntityFrameworkCore
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```


> SQLite konfigürasyonu için **startup.cs** dosyasını aşağıdaki gibi güncellemelisiniz.

```csharp
 public void ConfigureServices(IServiceCollection services)
        {
            
            services.AddDbContext<MainDbContext>(x => x.UseSqlite("DataSource=app.db"));

            services.AddControllers();
            services.AddSwaggerGen(c =>
            {
                c.SwaggerDoc("v1", new OpenApiInfo { Title = "ws_agt_ext_api", Version = "v1" });
            });
        }
```

> **Users** tablosunu oluşturmak için aşağıdaki gibi ilgili sınıfları oluşturmalısınız.

```csharp
    public class User 
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public string Surname { get; set; }
        public string Email { get; set; }
        public int Age { get; set; }
        public string Password { get; set; }
        public string RefreshToken { get; set; }
        public DateTime? RefreshTokenEndDate { get; set; }
    }

    public class MainDbContext : DbContext
    {
        public MainDbContext(DbContextOptions options) : base(options)
        {
        }

        public DbSet<User> Users { get; set; }
    }
```

> Komut satırınıa aşağıdaki komutları yazarak **app.db** dosyası ve **Users** tablosunun oluşturulmasını sağlayabilirsiniz.

```bash
dotnet ef migrations add InitialCreate
dotnet ef database update
```

> Eğer **User** tablosuna başka bir alan eklemek isterseniz aşağıdaki adımları takip ederek işlemi gerçekleştirebilirsiniz.

**User.cs**

```csharp
    ...
    public string Company { get; set; }
```

```bash
dotnet ef migrations add CompanyFieldMigration
dotnet ef database update
```

Ve **User** tablosuna **Company** sütunu eklenmiş oldu.


## Kaynaklar

- <https://docs.microsoft.com/en-us/ef/core/get-started/overview/first-app?tabs=netcore-cli>