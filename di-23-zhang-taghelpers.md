## 1. TagHelpers

TagHelpers是.net core新引入的特性，在视图中使用C\#类转换HTML元素。

最好的理解东西的方式就是创造一个。

The best way to understand tag helpers is to create one.

## 2. TagHelpers与HTMLHelpers

* HTMLHelpers使用

```
@Html.TextBoxFor(m => m.Population)
@Html.TextBoxFor(m => m.Population, new { @class = "form-control" })
```

HTMLHelpers表达式不适合HTML元素的特性，是HTML原有的特性、样式修饰都以动态对象的方式实现，更见复杂难用。

* TagHelpers使用

```
<input class="form-control" asp-for="Population" />
```

TagHelpers则迎合了HTML元素的自然特性，更加容易理解。

.net core mvc仍然支持HtmlHelpers，但是我们应该更多的使用TagHelpers的优势。

