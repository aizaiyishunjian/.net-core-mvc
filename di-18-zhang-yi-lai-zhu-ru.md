## 1. Controller创建流程

* MVC收到具体Controller和Action的入站请求
* MVC向ASP.NET Service Provider 组件请求Controller示例
* Service Provider 检查Controller 构造依赖
* Service Provider 查找依赖组件在容器中配置的实现
* Service Provider 创建依赖实现类
* Service Provider 创建Controller，传递依赖实现类实例到Controller

## 2. Service声明周期

* Transient
* Scoped
* Singleton

## 3. Action注入

基于Controller构造函数的实现注入，无论某个Action是否使用该实现，都会注入。使用`[FromServices]`特性修饰的Action参数完成的实现注入，只会在特定Action调用时注入。

```
public class HomeController : Controller {
    private IRepository repository;
    public HomeController(IRepository repo) {
        repository = repo;
    }
    public ViewResult Index([FromServices]ProductTotalizer totalizer) {
        ViewBag.HomeController = repository.ToString();    
        ViewBag.Totalizer = totalizer.Repository.ToString();
        return View(repository.Products);
    }
}
```

## 4. 属性注入

**.net core DI框架并不支持基于特性的任意服务的属性注入**，这里提到的属性注入，是特定的几个属性的注入。



