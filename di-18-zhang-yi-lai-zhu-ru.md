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



