\# 第1/2章 .net core mvc 与 .net mvc的区别



.net core mvc  .net mvc



1. 粗略浏览了Pro ASP.NET Core MVC, 6th Edition-Apress\(2016\)前两章内容；

.net core mvc 与.net mvc区别:

- .net mvc程序的配置一般从web.conf找数据库连接字符串、日志配置、web其他配置等；

- 在.net core mvc中Startup.cs和appsettings.json是我们查看程序的两个入口。appsettings.json负责配置像字符串连接、日志、权限校验等信息；

- Startup.cs的构造函数负责加载appsettings.json配置文件的配置，

- ConfigureServices方法负责将需要进行依赖注入的服务加入DI配置，

- Configure负责向HttpRequest管道添加中间件

2. VS2017的15大新特性：http://www.cnblogs.com/Leo\_wl/p/6581647.html‘’

3. 问题：印象VS2017支持调试状态下编辑，但是没找到怎么设置的。















