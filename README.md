# 一、AMP优点：使网页可被轻松发现

# 二、前置知识点：
## 1. 制作AMP网页和内容时,建议使用HTTPS协议
## 2. AMP HTML文档必须：
### 2.1. 以 <!doctype html> doctype 开头。
### 2.2. 包含顶级` <html ⚡> `标记（也可以使用 `<html amp>`） `//将网页标识为AMP内容`
### 2.3. 必须要包含 `<head>` 和 `<body>` 标记 
### 2.4. 包含 `<meta charset="utf-8">` 标记，以此作为其 <head> 标记的<b>第一个子级</b>。
### 2.5 包含` <script async src="https://cdn.ampproject.org/v0.js"></script>` 标记，以此作为其 <head> 标记的<b>第二个子级</b>。
### 2.6 在 `<head> `标记内包含 `<meta name="viewport" content="width=device-width,minimum-scale=1,initial-scale=1"> `标记

### 2.7 在` <head>` 标记内包含 AMP 样板代码。`//CSS样板最初会隐藏内容，直到AMPJS加载完毕为止`
```
<style amp-boilerplate>body{-webkit-animation:-amp-start 8s steps(1,end) 0s 1 normal both;-moz-animation:-amp-start 8s steps(1,end) 0s 1 normal both;-ms-animation:-amp-start 8s steps(1,end) 0s 1 normal both;animation:-amp-start 8s steps(1,end) 0s 1 normal both}@-webkit-keyframes -amp-start{from{visibility:hidden}to{visibility:visible}}@-moz-keyframes -amp-start{from{visibility:hidden}to{visibility:visible}}@-ms-keyframes -amp-start{from{visibility:hidden}to{visibility:visible}}@-o-keyframes -amp-start{from{visibility:hidden}to{visibility:visible}}@keyframes -amp-start{from{visibility:hidden}to{visibility:visible}}</style><noscript><style amp-boilerplate>body{-webkit-animation:none;-moz-animation:none;-ms-animation:none;animation:none}</style></noscript>

```

### 2.8 可选元数据,在 Google 搜索“焦点新闻”轮换展示区）分发内容
```
 <script type="application/ld+json">
        {
            "@context": "http://schema.org",
            "@type": "NewsArticle",
            "headline": "Open-source framework for publishing content",
            "datePublished": "2015-10-07T12:02:41Z",
            "image": [
                "logo.jpg"
            ]
        }
    </script>
```
## 3. `<img>`不能直接使用，需要修改为`<amp-img>`
```
<amp-img src="welcome.jpg" alt="Welcome" height="400" width="800"></amp-img>
```
## 4. 每个AMP页面，只有一个嵌入的样式表
```
<style amp-custom>
  /* any custom style goes here */
  body {
    background-color: white;
  }
  amp-img {
    background-color: gray;
    border: 1px solid black;
  }
</style>
```

## 5. 同一页面有非AMP页版本和AMP版本，需要操作两步
### 5.1 非AMP页面添加一下内容
```
<link rel="amphtml" href="//www.crov.com/amp/document.html">
```
### 5.2 AMP页面添加内容

```
<link rel="canonical" href="//www.crov.com">
```

## 6. 只有一个页面，并且该页面是AMP页面，也必须添加规范链接,链接指向自身
```
<link rel="canonical" href="https://www.crov.com">
```

## 7.访问`http://www.crov.com/#development=1`,因为加上`#development=1`,可以看到控制台报错，用来检查是否有验证错误


# 三、基础实战篇
## 1. 到foundations文件夹下
`cd accelerated-mobile-pages-foundations`,TERMINAL输入`anywhere`启动服务

## 2. 访问 `http://192.168.23.220:8000/article.html`

## 3. 新建article.amp.html，拷贝amp.html代码
```
<!doctype html>
<html lang="en">
  <head>

    <title>News Article</title>

    <link href="base.css" rel="stylesheet" />

    <script type="text/javascript" src="base.js"></script>
  </head>
  <body>
    <header>
      News Site
    </header>
    <article>
      <h1>Article Name</h1>

      <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Etiam egestas tortor sapien, non tristique ligula accumsan eu.</p>
    </article>
    <img src="mountains.jpg">
  </body>
</html>

```
## 4. `<head>标记底部`增加AMP库
`<script async src="https://cdn.ampproject.org/v0.js"></script>`
## 5. 启用AMP验证工具，访问`http://192.168.23.220:8000/article.html#development=1`，可以看到报错，见img目录下的new1.error1.jpg

## 6. 修正错误
### 6.1 `The mandatory attribute '⚡' is missing in tag 'html'. `
解决方案：`<html amp lang="en">`

### 6.2 ` The mandatory tag 'meta charset=utf-8' is missing or incorrect.`
解决方案：`<meta charset="utf-8" />`

## 6.3 `The mandatory tag 'link rel=canonical' is missing or incorrect.`
解决方案：`<link rel="canonical" href="/article.html">`

## 6.4 `The mandatory tag 'meta name=viewport' is missing or incorrect. `

解决方案：`<meta name="viewport" content="width=device-width,minimum-scale=1,initial-scale=1">

## 6.5 `The attribute 'href' in tag 'link rel=stylesheet for fonts' is set to the invalid value 'base.css'.`

解决方案：
### a. 移除`<link>`标记，内嵌`<style amp-custom></style>`
### b. 将base.css文件中的样式复制到`<style amp-custom></style>`标记中
```
<style amp-custom>

/* The content from base.css */

</style>
```
## 6.7 `Custom JavaScript is not allowed`
解决方案：base.js不含任何JS代码，移除即可

## 6.8 
```
The mandatory tag 'head > style[amp-boilerplate]' is missing or incorrect. 
The mandatory tag 'noscript > style[amp-boilerplate]' is missing or incorrect.
The mandatory tag 'noscript enclosure for boilerplate' is missing or incorrect.
```
解决方案：加入AMP样板代码
```
<style amp-boilerplate>body{-webkit-animation:-amp-start 8s steps(1,end) 0s 1 normal both;-moz-animation:-amp-start 8s steps(1,end) 0s 1 normal both;-ms-animation:-amp-start 8s steps(1,end) 0s 1 normal both;animation:-amp-start 8s steps(1,end) 0s 1 normal both}@-webkit-keyframes -amp-start{from{visibility:hidden}to{visibility:visible}}@-moz-keyframes -amp-start{from{visibility:hidden}to{visibility:visible}}@-ms-keyframes -amp-start{from{visibility:hidden}to{visibility:visible}}@-o-keyframes -amp-start{from{visibility:hidden}to{visibility:visible}}@keyframes -amp-start{from{visibility:hidden}to{visibility:visible}}</style><noscript><style amp-boilerplate>body{-webkit-animation:none;-moz-animation:none;-ms-animation:none;animation:none}</style></noscript>

```

## 6.9 `The tag 'img' may only appear as a descendant of tag 'noscript'. Did you mean 'amp-img'?`
解决方案：将`<img>替换为<amp-img>`
`<amp-img src="mountains.jpg"></amp-img>`

## 6.10 上述6.9引发新的报错
```
The element did not specify a layout attribute
Incomplete layout attributes specified for tag 'amp-img'. For example, provide attributes 'width' and 'height
```
### 原因：
### 1. 为了减少DOM重排量，AMP包含一个布局系统，确保在下载和呈现网页的生命周期中尽早地了解网页布局

### 2. AMP的布局方式可以避免移动文字，即使图片和广告需要很长时间才能加载完成也是如此

解决方案：`<amp-img src="mountains.jpg" layout="responsive" width="266" height="150"></amp-img>` 

### 说明：
`layout="responsive //图片自适应`


# 四、升级实战篇
## 1. 轮播图
### 1.1 引入amp-carousel组件库
```
<script async custom-element="amp-carousel" src="https://cdn.ampproject.org/v0/amp-carousel-0.1.js"></script>
```
### 1.2 
```
<amp-carousel layout="responsive" width="300" height="168" type="slides" loop autoplay delay="2000">
      <amp-img src="mountains-1.jpg" width="300" height="168"  layout="responsive"></amp-img>
      <amp-img src="mountains-2.jpg" width="300" height="168"  layout="responsive"></amp-img>
      <amp-img src="mountains-3.jpg" width="300" height="168"  layout="responsive" ></amp-img>
</amp-carousel>
```

## 2. 导航元素
### 2.1 引入`<amp-slider>组件`
`<script async custom-element="amp-sidebar" src="https://cdn.ampproject.org/v0/amp-sidebar-0.1.js"></script>
`
### 2.2 编写`<header>`代码，含有`<amp-sidebar>元素`
```
<header class="headerbar">
  <div role="button" on="tap:sidebar1.toggle" tabindex="0" class="hamburger">☰</div>
  <div class="site-name">News Site</div>

  <amp-sidebar id="sidebar1" layout="nodisplay" side="left">
    <div role="button" aria-label="close sidebar" on="tap:sidebar1.toggle" tabindex="0" class="close-sidebar">✕</div>
    <ul class="sidebar">
      <li><a href="#">Example 1</a></li>
      <li><a href="#">Example 2</a></li>
      <li><a href="#">Example 3</a></li>
    </ul>
  </amp-sidebar>
</header>
```

### 注意：ID标识符：`siderbar1`,`on`属性来操作toggle边栏

## 3. 添加自定义字体,引入Roboto
### 3.1 方法1：
`<link rel="stylesheet" type="text/css" href="https://fonts.googleapis.com/css?family=Roboto|Roboto:b">
`
### 3.2 方法2：
```
@font-face {
font-family: 'Mic-icon';
src: url("//crov.micstatic.com/gb/font/micon/micon-2/focusUED.eot?v=20180525");
src: url("//crov.micstatic.com/gb/font/micon/micon-2/focusUED.eot?#iefix&v=20180525") format("embedded-opentype"), url("//crov.micstatic.com/gb/font/micon/micon-2/focusUED.woff?v=20180525") format("woff"), url("//crov.micstatic.com /gb/font/micon/micon-2/focusUED.ttf?v=20180525") format("truetype");
font-weight: normal;
font-style: normal;
}

.ob-icon {
font-family: 'Mic-icon';
font-weight: normal;
font-style: normal;
text-decoration: inherit;
-webkit-font-smoothing: antialiased;
display: inline-block;
*display: inline;
*zoom: 1;
font-size: 16px;
line-height: 1;
vertical-align: middle;
text-decoration: none !important;
}
```

# 五、表单相关
## 1. form以及自定义校验
```
<form class="m-form J-home-search-form" action="//www.crov.com/search" target="_top" method="GET" target="_top" name="searchForm" custom-validation-reporting="show-all-on-submit">
    <input type="text" required class="input-text J-home-search-input" id="keyword2" maxlength="200" name="keyword" value="" placeholder="What are you looking for？" autocomplete="off" pattern="^[\x00-\xFF]+$">
        <button type="submit" class="btn">Shop Now</button>
        <div class="error-tip">
            <span visible-when-invalid="valueMissing" validation-for="keyword2">Please input keyword(s).</span>
            <span visible-when-invalid="patternMismatch" validation-for="keyword2">Please input the information in English only.</span>
        </div>
</form>
```

### 注意点：
#### 1.1 form两种方式：GET POST

GET:action可以还是action


POST:action必须修改为action-xhr

### 1.2 自定义校验`<form>`元素增加`custom-validation-reporting="show-all-on-submit"`

`<span>元素增加visible-when-invalid="valueMissing" //必填`
`<span>元素增加visible-when-invalid="patternMismatch" //只能输入中文`

```
<span visible-when-invalid="valueMissing" validation-for="keyword2">Please input keyword(s).</span>
<span visible-when-invalid="patternMismatch" validation-for="keyword2">Please input the information in English only.</span>
```

### 1.3 select










