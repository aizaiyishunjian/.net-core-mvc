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

## 3. 理解TagHelpers

* **实现目标**

```
<button type="submit" bs-button-color="danger">Add</button>
```

转换为

```
<button type="submit" class="btn btn-danger">Add</button>
```

* **定义TagHelpers类**

```
namespace Cities.Infrastructure.TagHelpers {
    public class ButtonTagHelper : TagHelper {
        public string BsButtonColor { get; set; }
        public override void Process(TagHelperContext context,
        TagHelperOutput output) {
            output.Attributes.SetAttribute("class", $"btn btn-{BsButtonColor}");
        }
    }
}
```

TagHelper的类名是要转换的HTML元素名后缀TagHelper,比如这里的ButtonTagHelper告诉MVC是用来操作Button元素的。MVC查看TagHelper类的属性定义，并对名字匹配的属性进行赋值，MVC将转换HTML属性值为C\#属性值。属性名会自动从HTML风格转换为C\#风格，bs-button-color-&gt;BsButtonColor。这种自动转换可以被打断，通过配置C\#类属性的`HtmlAttributeName`特性。

**TagHelperContext**

1. AllAttributes:HTML元素的只读属性集合
2. Items:HTML元素的TagHelpers集合
3. UniqueId:HTML元素的唯一标识

**TagHelperOutput**

1. TagName
2. Attributes
3. Content
4. PreElement
5. PostElement
6. PreContent
7. PostContent
8. TagMode
9. SupressOutput\(\)

* **注册TagHelper**





