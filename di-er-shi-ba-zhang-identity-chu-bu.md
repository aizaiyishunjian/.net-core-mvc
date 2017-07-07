# 基本使用步骤

## 1. 自定义User

IdentityUser提供基本的User信息，可以通过继承IdentityUser,实现对用户信息的扩展。

```
public class AppUser : IdentityUser {
    // no additional members are required
    // for basic Identity installation
}
```

## 2. 自定义DbContext

我们的DbContext类需要继承IdentityDbContext&lt;T&gt;，用于数据库操作，需要在配置文件提供Identity访问的数据库连接字符串。

```
public class AppIdentityDbContext : IdentityDbContext<AppUser> {
    public AppIdentityDbContext(DbContextOptions<AppIdentityDbContext> options)
        : base(options) { }
}
```

## 3. 配置Identity服务和插件

```
public void ConfigureServices(IServiceCollection services) {
    services.AddDbContext<AppIdentityDbContext>(options =>
        options.UseSqlServer(
            Configuration["Data:SportStoreIdentity:ConnectionString"]));
        services.AddIdentity<AppUser, IdentityRole>()
            .AddEntityFrameworkStores<AppIdentityDbContext>();
        services.AddMvc();
}


public void Configure(IApplicationBuilder app) {
    app.UseStatusCodePages();
    app.UseDeveloperExceptionPage();
    app.UseStaticFiles();
    app.UseIdentity();
    app.UseMvcWithDefaultRoute();
}
```



