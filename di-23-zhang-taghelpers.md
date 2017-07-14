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

### **实现目标**

```
<button type="submit" bs-button-color="danger">Add</button>
```

转换为

```
<button type="submit" class="btn btn-danger">Add</button>
```

### **定义TagHelpers类**

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

* **TagHelperContext**

* AllAttributes:HTML元素的只读属性集合

* Items:HTML元素的TagHelpers集合
* UniqueId:HTML元素的唯一标识

* **TagHelperOutput**

* TagName

* Attributes
* Content
* PreElement
* PostElement
* PreContent
* PostContent
* TagMode
* SupressOutput\(\)

### **注册TagHelper**

TagHelper只有注册后才能使用，作用域：

在当前视图注册的，只能在当前视图使用；

在当前文件夹注册的，只能在当前文件夹及其子文件夹使用；

在Views/\_ViewImport.cshtml注册的，可以在所有视图中使用；

```
//_viewImport.cshmtl
@using Cities.Models
@addTagHelper Cities.Infrastructure.TagHelpers.*, Cities
```

* **减小作用范围**

在Views根文件夹注册的taghelper对所用对应的HTML元素都会做转换，这样会做成下面的错误

```
//原始button
<button type="reset" class="btn btn-primary" >Reset</button>
//错误转换
<button type="reset" class="btn btn-">Reset</button>
```

1. 方案一：taghelper实现逻辑判断，是否需要处理和转换传入元素
2. 方案二：使用`HtmlTargetElement限制TagHelper应用的范围。`

```
 //button指定应用的元素类型
 //Attributes目标元素包含的特性集合，逗号分隔
 //ParentTag目标元素的父元素
 //TagStructure：元素结构：
 [HtmlTargetElement("button", Attributes = "bs-button-color", ParentTag = "form")]
 public class ButtonTagHelper : TagHelper {
    public string BsButtonColor { get; set; }
    public override void Process(TagHelperContext context,
    TagHelperOutput output) {
        output.Attributes.SetAttribute("class", $"btn btn-{BsButtonColor}");
    }
}
```

* **增加作用范围**

当我们需要把一个TagHelper应用到多个元素上时，我们可以扩展taghelper作用范围。

```
//不指定应用的元素类型
[HtmlTargetElement(Attributes = "bs-button-color", ParentTag = "form")]
public class ButtonTagHelper : TagHelper {
    public string BsButtonColor { get; set; }
    public override void Process(TagHelperContext context,
    TagHelperOutput output) {
        output.Attributes.SetAttribute("class", $"btn btn-{BsButtonColor}");
    }
}
```

通过忽略元素类型参数，我们可以将taghelper应用到所有HTML元素上，但是这样会带来潜在的问题，因此我们有一个折中的方案，多次应用`HtmlTargetElement元素`

```
[HtmlTargetElement("button", Attributes = "bs-button-color", ParentTag = "form")]
[HtmlTargetElement("a", Attributes = "bs-button-color", ParentTag = "form")]
public class ButtonTagHelper : TagHelper {
    public string BsButtonColor { get; set; }
    public override void Process(TagHelperContext context,
    TagHelperOutput output) {
        output.Attributes.SetAttribute("class", $"btn btn-{BsButtonColor}");
    }
}
```

## 4. 高级TagHelpers特性

### 自定义元素

```
<formbutton type="submit" bg-color="danger" />
<formbutton type="reset" />
```

#### taghelpers类

当处理自定义元素时，我们必须使用`HtmlTargetElement` 指定元素名称。默认的约束元素后缀taghelper只适合标准HTML元素。

**注意这里设置type = Type ,这是因为任何Attribute，只要有Property和它对应，TagHelper在元素输出时将忽略。**

```
[HtmlTargetElement("formbutton")]
public class FormButtonTagHelper : TagHelper {
    public string Type { get; set; } = "Submit";
    public string BgColor { get; set; } = "primary";
    public override void Process(TagHelperContext context,
    TagHelperOutput output) {
        output.TagName = "button";
        output.TagMode = TagMode.StartTagAndEndTag;
        output.Attributes.SetAttribute("class", $"btn btn-{BgColor}");
        output.Attributes.SetAttribute("type", Type);
        output.Content.SetContent(Type == "submit" ? "Add" : "Reset");
    }
}
```

#### Prepending和Appending内容和元素

* 在输出元素外围插入元素

PreElement和PostElement

* 在输出元素内部插入内容

PreContent和PostContent

#### 抑制内容输出

```
[HtmlTargetElement(Attributes = "show-for-action")]
public class SelectiveTagHelper : TagHelper {
    public string ShowForAction { get; set; }
    [ViewContext]
    [HtmlAttributeNotBound]
    public ViewContext ViewContext { get; set; }
    public override void Process(TagHelperContext context,
    TagHelperOutput output) {
        if (!ViewContext.RouteData.Values["action"].ToString()
        .Equals(ShowForAction, StringComparison.OrdinalIgnoreCase)) {
            //通过调用SuppressOutput可以组织元素内容输出。
            output.SuppressOutput();
        }
    }
}
```











