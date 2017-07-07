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

## 4. 创建数据库

> Add-Migration Initial
>
> Update-Database

## 5. Identity 服务使用

* UserManage&lt;TUser&gt;：用户管理，用户信息查询；
* IPasswordValidator&lt;TUser&gt;：密码验证策略

> ```
> //内置验证
> services.AddIdentity<AppUser, IdentityRole>(opts => {
>     opts.Password.RequiredLength = 6;
>     opts.Password.RequireNonAlphanumeric = false;
>     opts.Password.RequireLowercase = false;
>     opts.Password.RequireUppercase = false;
>     opts.Password.RequireDigit = false;
> }).AddEntityFrameworkStores<AppIdentityDbContext>();
> ```

> ```
> //验证接口
> public interface IPasswordValidator<TUser> where TUser : class {
>     Task<IdentityResult> ValidateAsync(UserManager<TUser> manager,
>     TUser user, string password);
> }
> ```

> ```
> //配置自定义验证
> services.AddTransient<IPasswordValidator<AppUser>,CustomPasswordValidator>();
> ```

* IUserValidator&lt;IUser&gt;：用户验证策略

> ```
> //内置验证
> opts.User.RequireUniqueEmail = true;
> opts.User.AllowedUserNameCharacters = "abcdefghijklmnopqrstuvwxyz";
> ```

> ```
> //验证接口
> public interface IUserValidator<TUser> where TUser : class {
>     Task<IdentityResult> ValidateAsync(UserManager<TUser> manager, TUser user);
> }
> ```

> ```
> 配置自定义验证
> services.AddTransient<IUserValidator<AppUser>, CustomUserValidator>();
> ```



































