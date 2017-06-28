# 1、浏览第六章内容

第六章其实感觉不用看的，因为Visual Studio一直在用，应该没什么不懂得。但是呢还是发现几个有意思的地方：

* Start Without Debugging：这个就有意思了，我们平常不愿意使用，因为不可以打断点调试，但是他有一个优点就是我们可以编辑class，并通过刷新浏览器把改变反映到浏览器。我觉得这个可编辑和刷新反馈真的挺好。

* Start Debugging:这个是我们经常使用的开发模式，优点是我们可以打断点在任何一个地方查看执行环境，当然缺点也很明显，编辑class必须重启。

* Browser Link:这个也是厉害了，我们平时不常用。在浏览器和Visual Studio之间维持一个长链接，可以通过Browser Link面板刷新浏览器，查看修改的内容。

* Multiple Browsers:这个在兼容性样式开发时，觉得是个不错的选择，同时在多个浏览器打开和刷新反馈修改情况。

* 下面是Startup中常见的三个配置，不解释了：

```
app.UseDeveloperExceptionPage();
```

```
app.UseBrowserLink();
```

```
app.UseStaticFiles()；
```

# 2、第七章：单元测试

感觉是不用看的。其实在MVC项目中，我们更多选择直接在浏览器查看修改效果和修改错误。大多时候我们是数据管理平台，不依赖其他设备就可以正常运行，所以单元测试不先得那么重要。

之前做后台控制项目倒是做过单元测试，这是因为后台项目一般运行依赖特殊环境，开发中完整测试比较困难，所以需要保证单元的健康性、健壮性，对单元测试的需求就更迫切。

总而言之，这次略过不看啦。

# 3、问题

* 添加controller报错：

no executable found matching command “dotnet-aspnet-codegenerator”

解决办法：

安装：Microsoft.VisualStudio.Web.CodeGeneration.Tools

（[https://stackoverflow.com/questions/41450148/no-executables-found-matching-command-dotnet-aspnet-codegenerator/41827013](https://stackoverflow.com/questions/41450148/no-executables-found-matching-command-dotnet-aspnet-codegenerator/41827013)）却提示安装失败：

包Microsoft.VisualStudio.Web.CodeGeneration.Tools具有一个包类型DotnetCliTool，

解决办法：

添加

&lt;ItemGroup&gt;  
&lt;DotNetCliToolReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Tools" Version="1.0.1" /&gt;  
&lt;/ItemGroup&gt;

到csproj文件。

（[http://blog.csdn.net/shouhou\_bingo/article/details/73506010](http://blog.csdn.net/shouhou_bingo/article/details/73506010)）

