# HTML 代码规范
## 通用
* [强制]文档类型一律采用HTML5标准 `<!DOCTYPE html>`
* [强制]字符集一律采用 `utf-8`

>通过明确声明字符编码，能够确保浏览器快速并容易的判断页面内容的渲染方式。这样做的好处是，可以避免在 HTML 中使用字符实体标记（character entity），从而全部与文档编码一致

```
<head>
  <meta charset="utf-8">
</head>
```
* [强制] 为 html 根元素指定 lang 属性，从而为文档设置正确的语言。这将有助于语音合成工具确定其所应该采用的发音，有助于翻译工具确定其翻译时所应遵守的规则等等

```
<html lang="zh-cn">

```
* [强制]所有的HTML标签必须`小写`
*  [强制]元素 `id` 必须保证页面唯一

> 同一个页面中，不同的元素包含相同的 id，不符合 id 的属性含义。并且使用 document.getElementById 时可能导致难以追查的问题。

* [强制]同一页面，应避免 `name` 与 `id`值相同

> IE 浏览器会混淆元素的 `id` 和 `name` 属性， `document.getElementById` 可能获得不期望的元素。所以在对元素的 `id` 与 `name` 属性值的命名需要非常小心。
一个比较好的实践是，为 `id` 和 `name` 使用不同的命名法。

```
  <input name="foo">
  <div id="foo"></div>
  <script>
      // IE6 将显示 INPUT
      alert(document.getElementById('foo').tagName);
  </script>
```
* [强制]用于样式的`class`一律采用 `中划线`
* [强制]class 必须代表相应模块或部件的`内容或功能`，不得以样式信息进行命名

```
<!-- 推荐 -->
<div class="sidebar"></div>

<!-- 不推荐 -->
<div class="left"></div>
```
* [建议]尽量采用`id`作为`js选择器`一律采用 `下划线`

## 标签
* [强制]对 HTML5 中规定允许省略的闭合标签，不允许省略闭合标签。

```
    <!-- good -->
    <ul>
        <li>first</li>
        <li>second</li>
    </ul>
    <!-- bad -->
    <ul>
        <li>first
        <li>second
    </ul>
```

* [强制]无需闭合标签，不允许自闭合(`input` , `br` , `img` , `hr` 等)

```
<!-- 推荐 -->
<input type="text" name="title">

<!-- 不推荐 -->
<input type="text" name="title" />
```

* [强制]标签使用必须符合标签嵌套规则。

> 比如 div 不得置于 p 中，tbody 必须置于 table 中，行内元素不要嵌套块级元素。<br>
>  `h` 、 `p` 、 `dt` 标签内不能嵌入`块级`元素。<br>
> `a` 标签内不能嵌入其他交互式元素，eg: `a`,`button`, `select`, `input`<br>
> `ul` 标签的子节点标签必须是 `li`<br>
> `dl` 标签的子节点只能是 `dt` 、 `dd` 标签

* [强制]HTML 标签的使用应该遵循标签的语义化,下面是常见标签语义

```
  p - 段落
  h1,h2,h3,h4,h5,h6 - 层级标题
  strong,em - 强调
  ins - 插入
  del - 删除
  abbr - 缩写
  cite - 引述来源作品的标题
  q - 引用
  blockquote - 一段或长篇引用
  ul - 无序列表
  ol - 有序列表
  dl,dt,dd - 定义列表
```

* [强制]使用 `button` 元素时必须指明 `type`属性值。

> button元素的默认 type 为 submit，如果被置于 form 元素中，点击后将导致表单提交。为显示区分其作用方便理解，必须给出 type 属性。

```
  <button type="submit">提交</button>
  <button type="button">取消</button>
```

* ［建议］减少嵌套层级、标签数量。
编写HTML代码时，尽量避免多余的父元素，如果不需要给img设置遮罩，则应该按照以下推荐书写：

```
<!-- 不推荐 -->
<span class="avatar">
  <img src="...">
</span>

<!-- 推荐 -->
<img class="avatar" src="...">
```

* [建议]在 CSS 可以实现相同需求的情况下不得使用表格进行布局
* [优雅降级]有文本标题的控件必须使用 `label` 标签将其与其标题相关联(注：在ie6,7下第一种方式，在点击label标签是input的radio或checkbox并不会被选上)

>有两种方式：
1.将控件置于 `label` 内。
2.`label` 的 `for` 属性指向控件的 `id`。
推荐使用第一种，减少不必要的 `id`。如果 DOM 结构不允许直接嵌套，则应使用第二种。

```
        <label><input type="checkbox" name="confirm" value="on">我已确认上述条款</label>
        <label for="username">用户名：</label><input type="textbox" name="username" id="username">
```
## 属性
* [强制]自定义属性以 `data-` 为前缀

> 使用前缀有助于区分自定义属性和标准定义的属性。

```
    <ol data-ui-type="Select"></ol>
```
* [强制]属性值必须使用`双引号`包围

```
<!-- 推荐 -->
<script src="esl.js"></script>

<!-- 不推荐 -->
<script src='esl.js'></script>
<script src=esl.js></script>
```
* [强制]布尔类型的属性，不加属性值。布尔型属性可以在声明时不赋值。XHTML 规范要求为其赋值，但是 HTML5 规范不需要。

```
<!-- 推荐 -->
<input type="checkbox" value="1" checked>

<!-- 不推荐 -->
<input type="text" disabled="disabled">
```
## CSS、JavaScript引入规则

* [强制]引入 CSS 时必须指明 rel="stylesheet"

```
    <link rel="stylesheet" src="page.css">
```

* [强制]引入 CSS 和 JavaScript 时无须指明 type 属性

> `text/css` 和 `text/javascript` 是 `type` 的默认值

* [强制]在 `head` 中引入页面需要的所有 CSS 资源。

> 在页面渲染的过程中，新的CSS可能导致元素的样式重新计算和绘制，页面闪烁。

* [强制]JavaScript 应当放在页面末尾，或采用异步加载。

* [建议]在针对移动设备开发的页面时，根据内容类型指定输入框的 `type` 属性。

>根据内容类型指定输入框类型，能获得友好的输入体验。

```
  <input type="date">
```

## 图片

* [强制]禁止 `img` 的 `src` 取值为空。延迟加载的图片也要增加默认的 src

> src 取值为空，会导致部分浏览器重新加载一次当前页面。


* [强制]所有图片添加 `alt` 属性

> 可以提高图片加载失败时的用户体验。

* [强制]为图片添加 `width` 和 `height` 属性，以避免页面抖动，出现重绘和回流，在低版本ie下会出现怪异的图片格式
* [建议]避免为 `img` 添加不必要的 `title` 属性

>多余的 `title` 影响看图体验，并且增加了页面尺寸。

* [建议]有下载需求的图片采用 `img` 标签实现，无下载需求的图片采用 CSS 背景图实现

> 产品 logo、用户产生的图片等有潜在下载需求的图片，以 img 形式实现，能方便用户下载。无下载需求的图片，比如：icon、背景、代码使用的图片等，尽可能采用 css 背景图实现。

# CSS代码规范
* [强制]用`4个空格`来代替制表符（tab） -- 这是唯一能保证在所有环境下获得一致展现的方法。
* [强制]为选择器分组时，将单独的选择器单独放在一行。
* [强制]为了代码的易读性，在每个声明块的左花括号前添加一个空格。
* [强制]声明块的右花括号应当单独成行。
* [强制]每条声明语句的 : 后应该插入一个空格。
* [强制]所有声明语句都应当以分号结尾。最后一条声明语句后面的分号是可选的，但是，如果省略这个分号，你的代码可能更易出错。
* [强制]对于属性值或颜色参数，省略小于 1 的小数前面的 0
  > 例如，`.5` 代替 `0.5；``-.5px` 代替 `-0.5px`。

* [强制]十六进制值应该全部小写，例如，`#fff`。在扫描文档时，小写字符易于分辨，因为他们的形式更易于区分。
```
  /* good */
    .success {
        background-color: #aca;
        color: #90ee90;
    }

    /* bad */
    .success {
        background-color: #ACA;
        color: #90EE90;
    }
```
* [强制]尽量使用简写形式的十六进制值，
  > 例如，用 `#fff` 代替 `#ffffff`。

* [强制]为选择器中的属性添加双引号，例如，input[type="text"]。只有在某些情况下是可选的，但是，为了代码的一致性，建议都加上双引号。
* [强制]避免为 0 值指定单位。
  > 例如，用 `margin: 0;` 代替 `margin: 0px;`。

```
/* 推荐 */
.selector {
  padding: 15px;
  margin-bottom: 15px;
  background-color: rgba(0,0,0,.5);
  box-shadow: 0 1px 2px #ccc, inset 0 1px 0 #fff;
}
```

## 选择器
* [强制]不得为 `id`、`class` 选择器添加类型选择器进行限定。

> 在性能和维护性上，都有一定的影响。

```
  /* good */
    #error,
    .danger-message {
        font-color: #c00;
    }

    /* bad */
    dialog#error,
    p.danger-message {
        font-color: #c00;
    }
```
* [强制]在可以使用缩写的情况下，使用属性缩写。
```
  /* good */
    .post {
        font: 12px/1.5 arial, sans-serif;
    }
```
* [建议]选择器的嵌套层级应`不大于 4` 级，位置靠后的限定条件应尽可能精确。

## 通用
* [强制]清除浮动
当元素需要撑起高度以包含内部的浮动元素时，通过对伪类设置 `clear` 或触发 `BFC` 的方式进行 `clearfix`。不使用增加空标签的方式。

> 触发 BFC 的方式很多，常见的有：

```
float 非 none
position 非 static
overflow 非 visible
```

另需注意，对已经触发 BFC 的元素不需要再进行 clearfix。

* [强制]不使用 `!important` 声明
> 当需要强制指定样式且不允许任何场景覆盖时，通过标签内联和 `!important` 定义样式
必须注意的是，仅在设计上 确实不允许任何其它场景覆盖样式时，才使用内联的 `!important` 样式。通常在第三方环境的应用中使用这种方案。

## 颜色
* [强制]RGB颜色值必须使用十六进制记号形式 #rrggbb。不允许使用 rgb()。

> 带有alpha的颜色信息可以使用 rgba()。


* [强制]颜色值可以缩写时，必须使用缩写形式。

```
  /* good */
    .success {
        background-color: #aca;
    }
```

* [强制]颜色值不允许使用命名色值。

```
    /* good */
    .success {
        color: #90ee90;
    }

    /* bad */
    .success {
        color: lightgreen;
    }
```

## 字体

* [强制]`font-family` 属性中的字体族名称应使用字体的英文 Family Name，其中如有空格，须放置在引号中.

> 所谓英文 Family Name，为字体文件的一个元数据.

```
  h1 {
    font-family: "Microsoft YaHei";
  }
```

* [强制]字体的字号不小于 `12px`。

> 在chrome浏览器中支持的最小字体为12px。以及在windows系统中小于12px的字体渲染就会模糊。

* [建议]`font-weight` 属性可使用数值方式描述。

> CSS 的字重分 100 – 900 共九档，但目前受字体本身质量和浏览器的限制，实际上支持 400 和 700 两档，分别等价于关键词 normal 和 bold。
浏览器本身使用一系列 启发式规则 来进行匹配，在 <700 时一般匹配字体的 Regular 字重，>=700 时匹配 Bold 字重。
但已有浏览器开始支持 =600 时匹配 Semibold 字重 ，故使用数值描述增加了灵活性，也更简短。

| 数值 | 字重  |
| ---  | ----  |
| 400  | normal|
| 700  | bold  |

```
    /* good */
    h1 {
        font-weight: 700;
    }

    /* bad */
    h1 {
        font-weight: bold;
    }
```

* [强制]`line-height` 在定义文本段落时，应使用数值
* [建议]`font-family` 按「西文字体在前、中文字体在后」、「效果佳 (质量高/更能满足需求) 的字体在前、效果一般的字体在后」的顺序编写，最后必须指定一个通用字体族( serif / sans-serif )。（苹果字体在前，微软字体在后）
## 优化
* [强制]需要添加 hack 时应尽可能考虑是否可以采用其他方式解决。

> 如果能通过合理的 HTML 结构或使用其他的 CSS 定义达到理想的样式，则不应该使用 hack 手段解决问题。通常 hack 会导致维护成本的增

* [强制]禁止使用 Expression。
* [强制]`CSS Sprite`

>`CSS Sprite`是一种将数个图片合成为一张大图的技术（既可以是背景图也可以是前景图），然后通过偏移来进行图像位置选取；
  `CSS Sprite`可以减少http请求，提高页面访问速度。

## [建议]语义，统一语义理解和命名
|语义     |命名     |简写    |
|--------|--------|--------|
|文档     |doc     |doc     |
|头部     |head    |hd      |
|主体     |body    |bd      |
|尾部     |foot    |ft      |
|主栏     |main    |mn      |
|主栏子容器|mainc   |mnc     |
|侧栏     |side    |sd      |
|侧栏子容器|sidec   |sdc     |
|盒容器   |wrap/box|wrap/box|
|导航     |nav     |nav     |
|子导航   |subnav   |snav    |
|面包屑   |crumb    |crm     |
|菜单     |menu     |menu    |
|选项卡    |tab     |tab     |
|标题区    |head/title|hd/tt |
|内容区    |body/content|bd/ct|
|列表     |list     |lst     |
|表格     |table    |tb     |
|表单     |form     |fm     |
|热点     |hot      |hot     |
|排行     |top      |top     |
|登录     |login    |log     |
|标志     |logo     |logo    |
|`广告`会被某些浏览器屏蔽掉|`advertise`|`ad`|
|搜索     |search   |sch     |
|幻灯     |slide    |sld     |
|提示     |tips     |tips    |
|帮助     |help     |help    |
|新闻     |news     |news    |
|下载     |download |dld     |
|注册     |regist   |reg     |
|投票     |vote     |vote    |
|版权     |copyright|cprt    |
|结果     |result   |rst     |
|标题     |title    |tt      |
|按钮     |button   |btn     |
|输入     |input    |ipt     |

