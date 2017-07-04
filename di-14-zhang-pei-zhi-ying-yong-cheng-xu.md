## 1. Program

* UseKestrel 
* UseContentRoot 应用程序根文件夹
* UseIISintegration 启用IIS集成
* UseStartup 指定配置Asp.Net应用程序类
* Build 整个前面配置，准备应用

## 2. Startup

* Application启动
* Starup类实例化 
* ConfigureServices 方法调用，创建配置服务
* Configure方法调用，管道装配
* Request Handling 启动

## 3. 中间件

中间件即组成请求处理的管道的组件。中间件不需要实现接口或者继承基类。

* Content-Generation MiddleWare

```
public class ContentMiddleware {
    private RequestDelegate nextDelegate;
    public ContentMiddleware(RequestDelegate next) {
        nextDelegate = next;
    }
    public async Task Invoke(HttpContext httpContext) {
        if (httpContext.Request.Path.ToString().ToLower() == "/middleware") {
            await httpContext.Response.WriteAsync("This is from the content middleware", Encoding.UTF8);
        } else {
            await nextDelegate.Invoke(httpContext);
        }
    }
}
```

![](file:///C:\Users\w4800\AppData\Roaming\Tencent\Users\1486753326\QQ\WinTemp\RichOle\KHS}[OGILY`G9{Y47WYT4GN.png)

* Short-Curcuiting Middleware

```
public class ShortCircuitMiddleware {
    private RequestDelegate nextDelegate;
    public ShortCircuitMiddleware(RequestDelegate next) {
        nextDelegate = next;
    }
    public async Task Invoke(HttpContext httpContext) {
        if (httpContext.Request.Headers["User-Agent"]
            .Any(h => h.ToLower().Contains("edge"))) {
            httpContext.Response.StatusCode = 403;
        } else {
            await nextDelegate.Invoke(httpContext);
        }
    }
}
```

![](file:///C:\Users\w4800\AppData\Roaming\Tencent\Users\1486753326\QQ\WinTemp\RichOle\C11NS6%29}@}YBOCLY]E5CR[V.png)

* Request-Edting MiddleWare

```
public class BrowserTypeMiddleware {
    private RequestDelegate nextDelegate;
    public BrowserTypeMiddleware(RequestDelegate next) {
        nextDelegate = next;
    }
    public async Task Invoke(HttpContext httpContext) {
        httpContext.Items["EdgeBrowser"]= httpContext.Request.Headers["User-Agent"]
            .Any(v => v.ToLower().Contains("edge"));
            await nextDelegate.Invoke(httpContext);
    }
}
```

![](file:///C:\Users\w4800\AppData\Roaming\Tencent\Users\1486753326\QQ\WinTemp\RichOle\HG_9MV0}J200I5%280`CDKD1Z.png)

* Response-Edting MiddleWare

```
public class ErrorMiddleware {
    private RequestDelegate nextDelegate;
    public ErrorMiddleware(RequestDelegate next) {
        nextDelegate = next;
    }
    public async Task Invoke(HttpContext httpContext) {
        await nextDelegate.Invoke(httpContext);
        if (httpContext.Response.StatusCode == 403) {
            await httpContext.Response
            .WriteAsync("Edge not supported", Encoding.UTF8);
        } else if (httpContext.Response.StatusCode == 404) {
            await httpContext.Response
                .WriteAsync("No content middleware response", Encoding.UTF8);
        }
    }
}
```



![](file:///C:\Users\w4800\AppData\Roaming\Tencent\Users\1486753326\QQ\WinTemp\RichOle\G~EG%29EL9%28AQG4TSQ4DNMPS4.png)

