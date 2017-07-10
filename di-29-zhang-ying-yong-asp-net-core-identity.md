## 1. Authorize

Authorize用于授权验证，但是并不指定用户应该如何认证。这样有利于Identity与其他Asp.Net应用框架无缝对接。

## 2. HttpContext

Asp.Net 平台提供上下文信息HttpContext,用于授权验证：

* HttpContext.User实现IPrincial接口

> Identity属性：实现Identity接口，描述请求用户信息
>
> IsInRole:用户角色判断

* IIdentity接口提供当前用户的基础信息

> AuthenticationType :用户认证机制
>
> IsAuthenticated:用户是否认证
>
> Name:当前用户名

.Net Core使用浏览器发送的Cookie信息判断用户是否已经被认证。如果用户被认证，IsAuthentiacted总是为true，否则为false，并默认重定向到/Account/Login。浏览器请求Account/Login，如果应用程序没有提供，则返回404。可以改变默认的重定向URL：

```
//Identity系统不依赖路由系统，因此这里必须写硬编码地址
services.AddIdentity<AppUser, IdentityRole>(opts => {
    opts.Cookies.ApplicationCookie.LoginPath = "/Users/Login";
})
```

## 3. AccountController实现

* ReturnURL:用户请求被限制访问的地址时，会被重定向到登录地址，登录后需要跳转到之前的地址。
* ValidateAntiForgeryToken:跨站脚本攻击



## 4. 用户认证

```
[HttpPost]
[AllowAnonymous]
[ValidateAntiForgeryToken]
public async Task<IActionResult> Login(LoginModel details,
string returnUrl) {
    if (ModelState.IsValid) {
        AppUser user = await userManager.FindByEmailAsync(details.Email);
        if (user != null) {
            await signInManager.SignOutAsync();
            Microsoft.AspNetCore.Identity.SignInResult result =
            await signInManager.PasswordSignInAsync(user, details.Password, false, false);
            if (result.Succeeded) {
                return Redirect(returnUrl ?? "/");
            }
        }
        ModelState.AddModelError(nameof(LoginModel.Email),"Invalid user or password");
        }
       return View(details);        
    }
}
```



