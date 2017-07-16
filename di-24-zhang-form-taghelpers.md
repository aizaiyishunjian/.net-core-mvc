## 1. FormTagHelper

* asp-controller
* asp-aciton
* asp-route-\*
* asp-route
* asp-area
* asp-antiforgery:跨站脚本防伪，与ValidateAntiForgeryToken结合防止跨站请求伪造。

## 2. InputTagHelper

* aps-for
* asp-format

## 3. SelectTagHelper

* asp-for
* asp-items

## 4. TextAreaTagHelper

* asp-for

## 5. EnvironmentTagHelper

* names

## 6. ScriptTagHelper

* asp-src-include
* asp-src-exclude
* asp-append-version
* asp-fallback-src
* asp-fallback-src-include
* asp-fallback-src-exclude
* asp-fallback-test

### 缓存清除

图片、javascript、css经常被缓存，造成新的版本更改无及时更新到客户端。

```
<script asp-src-include="/lib/jquery/dist/**/*.min.js"
asp-append-version="true">
</script>
```

```
<script src="/lib/jquery/dist/jquery.min.js?v=3zRSQ1HF-ocUiVcdv9yKTXqM">
</script>
```

### CDN（内容分发网络）

```
<!DOCTYPE html>
<html>
    <head>
        <meta name="viewport" content="width=device-width" />
        <title>Cities</title>
        <script src="http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.js"
        asp-fallback-src-include="/lib/jquery/dist/**/*.min.js"
        asp-fallback-test="window.jQuery">
        </script>
        <link href="/lib/bootstrap/dist/css/bootstrap.css" rel="stylesheet" />
    </head>
    <body class="panel-body">
        <div>@RenderBody()</div>
    </body>
</html>
```

```
<head>
    <meta name="viewport" content="width=device-width" />
    <title>Cities</title>
    <script src="http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.js">
    </script>
    <script>
        (window.jQuery||document.write("\u003Cscript
        src=\u0022\/lib\/jquery\/dist\/jquery.min.js
        \u0022\u003E\u003C\/script\u003E"));
    </script>
    <link href="/lib/bootstrap/dist/css/bootstrap.css" rel="stylesheet" />
</head>
```

## 7. LinkTagHelper

* asp-href-include
* asp-href-exclude
* asp-append-version
* asp-fallback-href
* asp-fallback-include
* asp-fallback-exclude
* asp-fallback-href-test-class
* asp-fallback-href-test-property
* asp-fallback-href-test-value

## 8. AnchorTagHelper

* asp-aciton
* asp-controller
* asp-area
* asp-fragment
* asp-host
* asp-protocol
* asp-route
* asp-route-\*

## 9. ImageTagHelper

* asp-append-version





