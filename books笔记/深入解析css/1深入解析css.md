###### 1层叠的规则冲突时，会依据三种规则来解决冲突

```
1).样式表的来源：样式是从哪里来的，包括你的样式和浏览器默认样式等
2).选择器优先级：权重
3).源码顺序

你的样式表属于作者样式表，除此之外还有用户代理样式表，即浏览器默认样式。用户代理样式
表优先级低，你的样式会覆盖它们
```

###### 2@规则

```
@规则是指用@符号开头的语法。比如@import规则或者@media查询

!important可以覆盖内联样式

伪类选择器（如:hover）和属性选择器（如[type="input"]）与一个类选择器的优先级相同。通用选择器（*）和组合器（>、+、~）对优先级没有影响。

给链接加样式要按照一定的顺序书写选择器。这是因为源码顺序影响了层叠。
a:link
a:visited
a:hover
a:active
```

###### 3层叠值

```
浏览器遵循三个步骤，即来源、优先级、源码顺序，来解析网页上每个元素的每个属性。如果一个声明在层叠中“胜出”，它就被称作一个层叠值。元素的每个属性最多只有一个层叠值。

两条经验法则
1).在选择器中不要使用 ID。就算只用一个 ID，也会大幅提升优先级。当需要覆盖这个选择器时，通常找不到另一个有意义的 ID，于是就会复制原来的选择器，然后加上另一个类，让它区别于想要覆盖的选择器。

2).不要使用!important。它比 ID 更难覆盖，一旦用了它，想要覆盖原先的声明，就需要再加上一个!important，而且依然要处理优先级的问题。
这两条规则是很好的建议，但不必固守它们，因为也有例外。不要为了赢得优先级竞赛而习惯性地使用这两个方法。
```

###### 4继承

```js
如果一个元素的某个属性没有层叠值，则可能会继承某个祖先元素的值。
```

###### 5.特殊值

```css
有两个特殊值可以赋给任意属性，用于控制层叠：inherit和initial。
/* inherit */
<head>
  <style>
    .parent{
      color: tomato;
    }
    .child{
      /* 继承父元素的颜色 */
      color: inherit;
    }
  </style>
</head>
<body>
  <div class="parent">
    <a href="#" class="child">child</a>
  </div>
</body>

/* initial */ 
/**
你需要撤销作用于某个元素的样式。这可以用 initial 关键字来实现。每一个 CSS属性都有初始（默认）值。如果将 initial 值赋给某个属性，那么就会有效地将其重置为默认值，这种操作相当于硬复位了该值。
*/
a{
      /* 为在大多数浏览器中，黑色是 color 属性的初始值，所以 color: initial 等价于 color: black。 */
    color: initial;
 }
```

###### 6.简写属性

```css
大多数简写属性可以省略一些值，只指定我们关注的值。但是要知道，这样做仍然会设置省略的值，即它们会被隐式地设置为初始值。这会默默覆盖在其他地方定义的样式。比如，如果给网页标题使用简写属性 font 时，省略 font-weight，那么字体粗细就会被设置为 normal

normal值：正常的意思，代表正常的值

阴影
button{
    padding: 5px 10px;
    /* 阴影向右偏移10像素，向下偏移3像素 */
    box-shadow: 10px 3px rebeccapurple;
}
```

###### 7.相对单位

```css
pt(点，印刷术语)：16px 等于 12pt（16/96×72）。设计师经常用点作为单位，开发人员则习惯用像素。因此跟设计师沟通的时候需要做一些换算。

/* ============================================================ */

em: 是最常见的相对长度单位，适合基于特定的字号进行排版。在 CSS 中，1em 等于当前元素的字号，其准确值取决于作用的元素。
button{
    font-size: 14px;
    padding: 1em;
}
当设置 padding、height、width、border-radius 等属性时，使用 em 会很方便。这是因为当元素继承了不同的字号，或者用户改变了字体设置时，这些属性会跟着元素均匀地缩放。
<span class="box box-mini">mini</span>
<span class="box box-small">small</span>
<style>
    .box{
        background-color: salmon;
        border-radius: 1em;
        padding: 1em;
    }

    .box-mini{
        font-size: 14px;
    }

    .box-small{
        font-size: 20px;
    }
</style>

/* 一个子号不可以等于自己的1.2倍。实际上，这个font-size会根据继承的子号来显示的 */
font-size: 1.2em;

em 用在内边距、外边距以及元素大小上很好，但是用在字号上就会很复杂。值得庆幸的是，我们有更好的选择：rem。

/* ============================================================ */

rem: root em 的缩写。rem 不是相对于当前元素，而是相对于根元素的单位。不管在文档
的什么位置使用 rem，1.2rem 都会有相同的计算值：1.2 乘以根元素的字号。

根节点有一个伪类选择器（:root），可以用来选中它自己。这等价于类型选择器 html，但是 html 的优先级相当于一个类名，而不是一个标签。

/* :root伪类等价于类型选择器html */
:root{
    font-size: 1em;			/* 使用浏览器的默认字号: 16px */
}
ul{
    font-size: .8rem;
}

由于默认的字号对某些用户而言很重要，尤其是对视力受损的人，所以应该始终用相对单位或者百分比设置字号。

一般会用 rem 设置字号，用 px 设置边框，用 em 设置其他大部分属性，尤其是内边距、外边距和圆角（不过我有时用百分比设置容器宽度）。

提示 拿不准的时候，用 rem 设置字号，用 px 设置边框，用 em 设置其他大部分属性。
<style>
:root{
    font-size: 0.875em;
}

span{
    font-size: 1.2rem;          
    padding: 1em;
    background-color: khaki;
    border: 1px solid tomato;
}
</style>

/* 文字转换 */
text-transform: uppercase;
```

###### 8.rem和媒体查询

```css
/* 作用到所有的屏幕，但是大屏幕会被覆盖 */
:root{
    font-size: 0.75em;
}

/* 仅作用到宽度800px以及其以上的屏幕，覆盖之前值 */
@media (min-width: 800px){
    :root{
        font-size: 0.875em;
    }
}

/* 仅作用到宽度1200px及其以上的屏幕，覆盖前面两个值 */
@media (min-width: 1200px){
    :root{
        font-size: 1em;
    }
}
```

###### 9.视口相对单位

```css
vh：视口高度的 1/100。
vw：视口宽度的 1/100。
vmin：视口宽、高中较小的一方的 1/100（IE9 中叫 vm，而不是 vmin）。
vmax：视口宽、高中较大的一方的 1/100（本书写作时IE和Edge均不支持vmax）

calc()函数内可以对两个及其以上的值进行基本运算。当要结合不同单位的值时，calc()特别实用。它支持的运算包括：加（+）、减（−）、乘（×）、除（÷）。加号和减号两边必须有空
白
```

###### 10.calc()定义字号

```css
:root {
 	font-size: calc(0.5em + 1vw);
} 
现在打开网页，慢慢缩放浏览器，字体会平滑地缩放。0.5em 保证了最小字号，1vw 则确保了字体会随着视口缩放。这段代码保证基础字号从 iPhone 6 里的 11.75px 一直过渡到 1200px 的浏览器窗口里的 20px。可以按照自己的喜好调整这个值。
```

###### 11.行高无单位

```css
/* 代表行高的1.2，行高会被继承 */
body{
    line-height: 1.2;
}

title{
    font-size: 2em;						/* 这里font-size计算为32px */
}									   /* 行高继承过来1.2,这里行高为32px * 1.2 */
```

###### 12.自定义属性（即css变量）

```css
如果刚好用了内置变量功能的 CSS 预处理器，比如 Sass 或者 Less，你可能就不太想用 CSS 变量了。千万别这样。新规范里的 CSS 变量有本质上的区别，它比任何一款预处理器的变量功能都多。因此我倾向于称其为“自定义属性”，而不是变量，以强调它跟预处理器变量的区别。

/* 这里定义了一个名叫--main-font变量，将其设置为一些常见的sans-serif字体，变量名前面必须有两个连字符(--)，用来根css属性区分，剩下的部分可以随意命名。 */
:root {
 --main-font: Helvetica, Arial, sans-serif;
} 
/* 使用自定义属性 */
p {
    font-family: var(--main-font)
}
```

###### 13.自定义属性使用

```css
:root{
    --main-font-size: 30px; 
    --main-border: 1px solid red;
}

p{
    font-size: var(--main-font-size);
    border: var(--main-border);
}

/* var函数接收第二个参数，它指定了备用值。如果第一个参数指定的变量未被定义，那么就会使用第二个值 */
h1 {
    font-size: var(--main-font-size, 14px)
}

/* 如果var()函数算出来的是一个非法值，对应的属性就会设置为其初始值 */

自定义属性只不过为减少重复代码提供了一种便捷方式，但是它真正的意义在于，自定义属性的声明能够层叠和继承：可以在多个选择器中定义相同的变量，这个变量在网页的不同地方有不同的值。

可以定义一个变量为黑色，然后在某个容器中重新将其定义为白色。那么基于该变量的任何样式，在容器外部会动态解析为黑色，在容器内部会动态解析为白色。

自定义属性就像作用域变量一样，因为它的值会被后代元素继承。
```

###### 14.动态切换整体样式

```
在点击切换主题的时候，使用javascript获取根元素的class,动态的修改为dark或者light。
```

###### 15盒模型

```css
/*IE 有一个 bug，它会默认将<main>元素渲染成行内元素，而不是块级元素，所以代码中我们用声明 display: block 来纠正。*/
main{
    display: block
}

/* 当设置左侧宽度为30%，右侧宽度为70%的时候，元素会换行，是因为有border和padding，margin的作用，此时可以用calc()计算它的宽度，或者使用box-sizing */
```

###### 16.box-sizing

```css
/* 全局设置box-sizing,给页面上所有元素和伪元素设置border-box */
*,
::before,		
::after {
    box-sizing: border-box;
}

/*
如果在网页中使用了带样式的第三方组件，就可能会因此破坏其中一些组件的布局，尤其是当第三方组件在开发 CSS 的过程中没有考虑到使用者会修改盒模型时。因为全局设置border-box 时使用的通用选择器会选中第三方组件内的每个元素，修改盒模型可能会有问题，所以最终需要写另外的样式将组件内的元素恢复为 content-box。有一种简单点的方式，是利用继承改一下修改盒模型的方式。
*/
:root {
 	box-sizing: border-box;
}
*,
::before,
::after {
 	box-sizing: inherit;
} 
```

###### 17.外边距留白的两种方式（float），这里不涉及flex

```css
/* 1.使用百分比，宽度要减去相应的1 */
 width: 29%;
 margin-left: 1%;

/* 2.基于calc从宽度中减去间距 */
width: calc(30% - 1.5em);
margin-left: 1.5em;
```

###### 18.元素高度问题

```css
处理元素高度的方式跟处理宽度不一样。之前对 border-box 的修改依然适用于高度，而且很有用，但是通常最好避免给元素指定明确的高度。普通文档流是为限定的宽度和无限的高度设计的。内容会填满视口的宽度，然后在必要的时候行。因此，容器的高度由内容天然地决定，而不是容器自己决定。
```

###### 19.控制溢出行为

```css
overflow
visible（默认值）—— 所有内容可见，即使溢出容器边缘。
hidden —— 溢出容器内边距边缘的内容被裁剪，无法看见。
scroll —— 容器出现滚动条，用户可以通过滚动查看剩余内容。在一些操作系统上，会出现水平和垂直两种滚动条，即使所有内容都可见（不溢出）。不过，在这种情况下，滚动条不可滚动（置灰）。
auto —— 只有内容溢出时容器才会出现滚动条。

/* 推荐使用auto而不是scroll,因为大多数情况下，不希望滚动条一直出现 */

水平方向溢出：除了垂直溢出，内容也可能在水平方向溢出。一个典型的场景就是在一个很窄的容器中放一条很长的URL。溢出的规则跟垂直方向上的一致。
可以用 overflow-x 属性单独控制水平方向的溢出，或者用 overflow-y 控制垂直方向溢出。这些属性支持 overflow 的所有值，然而同时给 x 和 y 指定不同的值，往往会产生难以预料的结果。
```

###### 20百分比高度

```css
/* 百分比的高度是基于父元素的，所以一般会设置html和body的height为100%，和100vh等价 */
html{
    height: 100%;
    background-color: lightcoral;
}
```

###### 21flex布局

```css
/* 你可以给子元素设置宽度和外边距，尽管加起来可能超过 100%，Flexbox 也能妥善处理。 */
.container{
    display: flex;
}
main{
    display: block;     /* 修复IE的bug */
    width: 70%;
    background-color: #fff;
    border-radius: .5em;
    box-sizing: border-box;
}

.sidebar{
    width: 30%;
    margin-left: 1.5em;	
    padding: 1.5em;
    background-color: #fff;
    border-radius: .5em;
    box-sizing: border-box;
}
```

###### 22.不设置高度和无宽度原则

```css
除非别无选择，否则不要明确设置元素的高度。先寻找一个替代方案。设置高度一定会导致更复杂的情况。
```

###### 23.min-height和max-height

```css
/* 接下来介绍的两个是很有用的属性：min-height 和 max-height。你可以用这两个属性指定最小或最大值，而不是明确定义高度，这样元素就可以在这些界限内自动决定高度。 */

/*
	min-width,min-height,max-width,max-height不仅可以设置在html,body中，也可以使用在其它的元素中
*/
```

###### 24.垂直居中内容

```css
为什么 vertical-align 不生效： 如果开发人员期望给块级元素设置 vertical-align: middle 后，块级元素里的内容就能垂直居中，那么他们通常会失望，因为浏览器会忽略这个声明。vertical-align 声明只会影响行内元素或者 table-cell 元素。对于行内元素，它控制着该元素跟同一行内其他元素之间的对齐关系。比如，可以用它控制一个行内的图片跟相邻的文字对齐。对于显示为 table-cell 的元素，vertical-align 控制了内容在单元格内的对齐。如果你的页面用了 CSS 表格布局，那么可以用 vertical-align 来实现垂直居中。

1).只给出上下pading,不给高度会居中
header{
    padding: 4em 0;
}

2).flex方式垂直居中
.parent{
    display: flex;
}

.child{
    align-self: center;
}

3).行高实现居中(只针对一行文字的时候)
header{
    height: 30px;
    line-height: 30px;
}

4).tabel方式实现居中
header{
    display: table-cell;
    vertical-align: middle;
}

5).绝对定位结合形变（transform）居中 （前面方法都无法实现时，使用此方法实现居中）
```

##### 25.负外边距

```css
负外边距的具体行为取决于设置在元素的哪边，如果设置左边或顶部的负外边距，元素就会相应地向左或向上移动，导致元素与它前面的元素重叠，如果设置右边或者底部的负外边距，并不会移动元素，而是将它后面的元素拉过来。
```

###### 26.外边距折叠（上下外边距才会折叠）

```js
当顶部和底部的外边距相邻时，就会重叠，产生单个外边距。这种现象称为折叠。外边距折叠取最大的值。
只有上下外边距会产生折叠，左右外边距不会折叠。
```

###### 27.多个外边距折叠（上下外边距才会折叠）

```html
<h2></h2>
<div>
	<p></p>
</div>

即使两个元素不是相邻的兄弟节点也会产生外边距折叠。即使将这个段落用一个额外的 div包裹起来,即以上的h2 div和p标签三者会寻找最大的外边距进行折叠。

即使将段落放在多个 div 中嵌套，渲染结果都一样：所有的外边距都会折叠到一起。

flexbox布局的弹性子元素的外边距不会折叠
```

###### 28.防止外边距折叠

```css
/*
如下方法可以防止外边距折叠
	1).对容器使用overflow:auto(或者非visible的值)，防止内部元素的外边距根容易外部的外边距折叠（这种方式副作用最小）
	2).在两个外边距之间加上边框或者内边距，防止他们折叠
	3).如果元素为浮动元素、内联块、绝对定位或固定定位时，外边距不会在它外面折叠
	4).当使用flexbox布局时，弹性布局内的元素之间不会发生外边距折叠（grid布局同理）
	5).当元素显示为tabel-cell是不具备外边距属性，因此它们不会折叠。
*/
```

###### 29.内边距

```css
/* 让顶层a标签没有margin-top,其他的a标签有margin-top,这样最顶上的标签就不会多出一个margin-top来 */
<aside class="sidebar">
    <a href="#" class="button-link">follow us on Twiter</a>
    <a href="#" class="button-link">like us on Facebook</a>
    <a href="#" class="button-link">like us on Facebook</a>
</aside>		

/* 只给紧跟其他button-link后面的button-link加上顶部外边距。即第一个.button-link不会选中 */
.button-link + .button-link{
    margin-top: 1.5em;
}
```

###### 30.猫头鹰选择器

```css
/* 猫头鹰选择器会选中直接跟在其他元素后面的任何元素。也就是说，它会选中页面上有着相同父级的第一个子元素，将body放在选择器前面，这样选择器就只能选中body内的元素。如果直接使用猫头鹰选择器，它还会选中body元素，因为它是head元素的相邻兄弟节点 */
body * + * {
    margin-top: 1.5em;
}

/* 猫头鹰选择器的其他组合 */
.sidebar > * + * {
    margin-left: 1.5em;
}
```

###### 31.双容器模式

```css
<body>
	<div class='container'></div>
</body>

/* 让container最大宽度为1080px,并且始终居中显示。这里使用了max-width而不是width.因此如果视口宽度小于1080px的话，内层容器就能缩小到1080px一下。换句话说，在小视口上，内层容器会填满屏幕，在大视口上它会扩展到1080px.这种方式能有效避免在小屏幕上出现水平滚动条 */
.container{
    max-width: 1080px;
    margin: 0 auto;
}
```

###### 32.浮动

```css
/*
只有 IE10 和 IE11 支持 Flexbox，而且还有一些 bug。如果不想碰到 bug，或者需要支持旧版浏览器，浮动也许是更好的选择。
此外，要实现将图片移动到网页一侧，并且让文字围绕图片的效果，浮动仍然是唯一的方法。

为浮动元素不同于普通文档流的元素，它们的高度不会加到父元素上。这可能看起来很奇怪，但是恰好体现了浮动的设计初衷。
*/
```

###### 33.清除浮动

```css
.clearfix::after{
    display: block;
    content: ' ';
    clear: both;
}

将类clearfix加在浮动元素的父元素上面（即要给包含浮动的元素清除浮动）
```

###### 34.BFC(块级格式化上下文)

```css
/*
BFC是网页的一块区域，元素基于这块区域布局。虽然BFC本身是环绕文档流的一部分，但它将内部的内容与外部的上下文隔离开。这种隔离为创建BFC元素做了三件事情
	1).包含了内部所有元素的上下外边距。它们不会跟BFC外面的元素产生外边距折叠
	2).包含了内部所有的浮动元素
	3).不会跟BFC外面的浮动元素重叠

给元素添加一下的任意属性值都会创建BFC
	float: left或right, 不为none即可。
	overflow: hidden、auto或scroll,不为visible即可。
	display: inline-block、table-cell、table-caption、flex、inline-flex、grid或inline-grid.拥有这些属性的元素成为块级容器。
	position: absoulte或position: fixed

说明：网页的根元素也创建了一个顶级的BFC
*/

BFC是一个独立的布局环境，其中的元素布局是不受外界的影响，并且在一个BFC中，块盒与行盒（行盒由一行中所有的内联元素所组成）都会垂直的沿着其父元素的边框排列。

/* 使用overflow: auto通常是创建BFC最简单的一种方式 */
overflow: auto;	 
```

###### 35.felxbox导航栏布局

```css
还可以用 display: inline-flex。它创建了一个弹性容器，行为类似于inline-block 元素。它会跟其他行内元素一起流式排列，但不会自动增长到 100%的宽度。


/* 以下代码，父元素需开启了flex布局 */
/* li标签除开第一个，每一个与左侧都有1.5em的间距 */
.site-nav > li + li {
    margin-left: 1.5em;
}

/* 设置最后一个为Auto会移动到最右侧 */
/* 如果希望将最后两个元素推到最右侧，则将倒数第二个元素添加margin-left */
.site-nav > .nav-right{
    margin-left: auto;
}
```

###### 36.flex属性

```css
/*
	flex属性是flex-group,flex-shrink和flex-basis的缩写，只写一个值会被赋值给flex-group,剩下两个属性是默认值（分别是1和0%）
*/
flex: 2
等于
flex-grow: 2;
flex-shrink: 1;
flex-basis: 0%;
```

###### 37.flex-basis属性

```css
/*
flex-basis 定义了元素大小的基准值，即一个初始的“主尺寸”。flex-basis 属性可以设置为任意的 width 值，包括 px、em、百分比。它的初始值是 auto，此时浏览器会检查元素是否设置了 width 属性值。如果有，则使用 width 的值作为 flex-basis 的值；如果没有，则用元素内容自身的大小。如果 flex-basis 的值不是 auto，width 属性会被忽略。
*/
```

###### 38.flex-group属性

```css
/*
每个弹性子元素的 flex-basis 值计算出来后，它们（加上子元素之间的外边距）加起来会占据一定的宽度。加起来的宽度不一定正好填满弹性容器的宽度，可能会有留白。多出来的留白（或剩余宽度）会按照 flex-grow（增长因子）的值分配给每个弹性子元素，flex-grow 的值为非负整数。如果一个弹性子元素的 flex-grow 值为 0，那么它的宽度不会超过 flex-basis 的值；如果某个弹性子元素的增长因子非 0，那么这些元素会增长到所有的剩余
空间被分配完，也就意味着弹性子元素会填满容器的宽度
*/
```

###### 39.flex-shrink属性

```css
/*
flex-shrink 属性与 flex-grow 遵循相似的原则。计算出弹性子元素的初始主尺寸后，它们的累加值可能会超出弹性容器的可用宽度。如果不用 flex-shrink，就会导致溢出。每个子元素的 flex-shrink 值代表了它是否应该收缩以防止溢出。如果某个子元素为flex-shrink: 0，则不会收缩；如果值大于 0，则会收缩至不再溢出。按照 flex-shrink 值的比例，值越大的元素收缩得越多。
*/
```

###### 40.弹性方向

```css
Flexbox 的另一个重要功能是能够切换主副轴方向，用弹性容器的 flex-direction 属性控制。默认值是row
flex-direction: row;
flex-direction: row-reverse;
flex-direction: column;
flex-direction: column-reverse;

内部的弹性盒子的弹性方向为 column，因此主轴发生了旋转，现在变成了从上到下（副轴变成了从左到右）。也就是对于弹性子元素而言，flex-basis、flex-grow 和 flex-shrink现在作用于元素的高度而不是宽度。
```

###### 41.给文本类型的输入框（不包含复选框和单选按钮）添加样式

```css
.login-form input:not([type=checkbox]):not([type=radio]) {
     display: block;
     width: 100%;
     margin-top: 0;
} 
```

###### 42.字体大写

```css
字体大写建议使用css属性，而不是在html中写死大写
text-transform: uppercase;
```

###### 43.弹性容器的其他属性

```css
/*
1). flex-direction指定主轴方向，副轴垂直于主轴
可选值: row, row-reverse, column, column-reverse

2). flex-wrap指定了弹性子元素是否会在弹性容器内折行显示，如果flex-direction则折列显示
可选值：nowrap, wrap, wrap-reverse

3).flex-flow: flex-direction和flex-wrap的简写

4).justify-content: 控制子元素在主轴上的位置
可选值: flex-start, flex-end, center, space-between, space-around

5).align-items: 控制子元素在副轴上的位置
可选值: stretch, flex-start, flex-end, baseline, center

6).align-content:如果开启了flex-wrap,align-content就会控制弹性子元素在副轴上的间距。如果子元素没有换行就会忽略align-content
可选值：flex-start, stretch, flex-end, space-between, center, space-around
*/
```

###### 44.弹性子元素的容器

```css
/*
1). flex-group: 整数，指定增长因子，决定子元素在主轴方向上的扩展的大小，用于填充未使用的空间。
2). flex-shrink: 整数，指定“收缩因子”，决定子元素在主轴方向收缩的大小，防止溢出。如果弹性容器开启了flex-wrap，则会忽略该属性。
3). flex-basis: 指定子元素未受flex-grow或flex-shrink影响时的初始大小
4). flex: flex-grow, flex-shrink, flex-basis的简写
5). align-self：控制子元素在副轴上的对齐方式。它会覆盖容器上的align-items值。如果子元素副轴方向上的外边距为auto,则会忽略该属性
6).Order: 整数，将弹性子元素从兄弟节点中移动到指定位置，覆盖源码顺序
初始状态下，所有的弹性子元素的 order 都为 0。指定一个元素的值为−1，它会移动到列表的最前面；指定为 1，则会移动到最后。可以按照需要给每个子元素指定 order 以便重新编排它们。这些值不一定要连续。
*/
```

###### 45.flex注意的地方

```css
Flexbox 的实现是 CSS 的一大进步。一旦你熟悉了它，你可能想要在页面的每个地方都开始使用，不过你应该依靠正常的文档流，只在必要的时候才使用 Flexbox。这么说并不是让你不用它，而是希望你不要拿着锤子满世界找钉子。
```



