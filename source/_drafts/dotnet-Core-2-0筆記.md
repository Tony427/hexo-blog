---
title: dotnet Core 2.0筆記
date: 2017-09-19 12:00:00
tags: 
- DotnetCore
- C#
categories: Software
thumbnail: https://images.unsplash.com/photo-1504985954001-5737b2af529e?ixlib=rb-1.2.1&q=80&fm=jpg&crop=entropy&cs=tinysrgb&w=1080&fit=max&ixid=eyJhcHBfaWQiOjF9
---

<!-- more -->

# Docnet Coret 2.0初探筆記
以下內容主要來自 [Introduction to ASP.NET Core](https://docs.microsoft.com/zh-tw/aspnet/core/) 
整理自己覺得特別要注意的地方


## Entity Framework Core 2.0
* 沒有EF6的設計工具
* 沒有.edmx 沒有 .tt
* 使用Scaffold-DbContext建立相關model
* 支援Like function
```cs
var authors =
   from a in context.Authors
   where EF.Functions.Like(c.FirstName, "J%");
   select a;
```
* 支援全域查詢
```cs
modelBuilder.Entity<Author>()
   .HasQueryFilter(a => !p.IsDeleted);
```

```cs
var result = context.Authors
   .Include(a => a.Authors)
   .FirstOrDefault(a => a.Id == id);
```

* 支援DbContext Pooling
```cs
services.AddDbContextPool<AuthorsContext>(
   options => options.UseSqlServer(dbConnectionString));
```
* 支援Table Splitting


### 如何使用Database First
`PM > Console` 輸入下列指令產生DbContext與其他Model Class
```shell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

`BloggingContext.cs` 加入建構子
```cs
public BloggingContext(DbContextOptions<BloggingContext> options) : base(options) { }
```

`Startup.cs`使用DI註冊於專案全域
```cs
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();
    services.AddDbContext<BloggingContext>(options => options.UseSqlServer(Configuration.GetConnectionString("BloggingDatabase")));
}
```

注入Controller
```cs
private readonly BloggingContext _context;

public BlogsController(BloggingContext context)
{
    _context = context;
}
```

應用程式使用dbContext
```cs
// GET: api/Blogs
[HttpGet]
public IEnumerable<Blog> GetBlog()
{
    return _context.Blog;
}
```

#### DbContext的使用方式
`方法一` 應用程式使用建構子
```cs
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```
`方法二` 複寫`OnConfiguring`事件
```cs
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlite("Data Source=blog.db");
    }
}
```
應用程式直接使用
```cs
using (var context = new BloggingContext())
{
  // do stuff
}
```

參考來源
* [Configuring a DbContext](https://docs.microsoft.com/en-us/ef/core/miscellaneous/configuring-dbcontext)
* [Getting Started with EF Core on ASP.NET Core with an Existing Database](https://docs.microsoft.com/en-us/ef/core/get-started/aspnetcore/existing-db)

## Configuration

* [Connection Strings](https://docs.microsoft.com/en-us/ef/core/miscellaneous/connection-strings)
* [Configuration in ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration)

### 使用SecretMaganer存取機敏資料

* [Safe storage of app secrets during development in ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/security/app-secrets)
