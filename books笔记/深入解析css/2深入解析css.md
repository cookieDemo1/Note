```css
/* 字体随着屏幕宽度的缩放而缩放有两种方式 */

1.直接在:root中设置字体为1vw+0.6em。屏幕不管缩放到多大
:root{
    box-sizing: border-box;
    font-size: calc(1vw + 0.6em);
}

2.使用媒体查询
```



###### 1.css网格布局

```css
CSS 网格可以定义由行和列组成的二维布局，然后将元素放置到网格中。有些元素可能只占据网格的一个单元，另一些元素则可能占据多行或多列。网格的大小既可以精确定义，也可以根据自身内容自动计算。你既可以将元素精确地放置到网格某个位置，也可以让其在网格内自动定位，填充划分好的区域。

在普通用户看来，浏览器对网格布局的支持程度为 0，但实际上，浏览器几乎完全实现了网格布局。因为需要开启实验特性才可以使用grid布局

网格布局已具备在生产环境使用的条件
```

###### 2.开启实验特性

```
在浏览器地址栏输入 chrome://flags，按下 Enter 键。然后滚动到“Experimental Web Platform features”(或者使用浏览器的搜索功能)，然后点击开启(Enabled)。
```

###### 3.两行三列网格

```css
<div class="grid">
    <div class="a">a</div>
    <div class="b">b</div>
    <div class="c">c</div>
    <div class="d">d</div>
    <div class="e">e</div>
    <div class="f">f</div>
</div>

*{
    box-sizing: border-box;
}
.grid{
    display: grid;                          /* 将元素设为网格容器 */
    grid-template-columns: 1fr 1fr 1fr;     /* 定义等宽的三列 */
    grid-template-rows: 1fr 1fr;            /* 定义等高的两行 */
    grid-gap: 0.5em;                        /* 给网格的单元格之间加上间隔 */
}

.grid > * {
    background-color: darkgray;
    color: white;
    padding: 2em;
    border-radius: 0.5em;
}
```

###### 4.grid-template-columns和grid-template-rows

```css
/*
grid-template-columns 和 grid-template-rows。这两个属性定义了网格每行每列的大小。本例使用了一种新单位 fr，代表每一列（或每一行）的分数单位（fraction unit）。这个单位跟 Flexbox 中 flex-grow 因子的表现一样。grid-template-columns:1fr 1fr 1fr 表示三列等宽。

不一定非得用分数单位，可以使用其他的单位，比如 px、em 或百分数。也可以混搭这几种单位，例如，grid-template-columns: 300px 1fr 定义了一个固定宽度为 300px 的列，后面跟着一个会填满剩余可用空间的列。2fr 的列宽是 1fr 的两倍。
*/
grid-template-columns: 200px 1fr 2fr;

.grid{
    display: grid;
    grid-template-columns: 200px 1fr 2fr;
    grid-template-rows: 2fr 1fr;
    grid-gap: 0.5em 1em;
}

/* 声明多个网格轨道的简写方式： 定义四个水平轨道，大小为auto */
grid-template-rows: repeat(4, auto);
等价于
grid-template-rows: auto auto auto auto;

 repeat()符号还可以定义不同的重复模式，比如 repeat(3, 2fr 1fr)会重复三遍这个模式，从而定义六个网格轨道，重复的结果是 2fr 1fr 2fr 1fr 2fr 1fr。

还可以将 repeat()作为一个更长的模式的一部分。比如 grid-template-columns: 1fr repeat(3, 3fr) 1fr 定义了一个 1fr 的列，接着是三个 3fr 的列，最后还有一个 1fr 的列（可以用 1fr 3fr 3fr 3fr 1fr 表示）。可以看出来因为展开的写法无法一目了然，所以才产生了repeat()这种简写方式。
```

###### 5.grid-gap

```css
/*
grid-gap属性定义了每个网格单元之间的间距。也可以用两个值分别制定垂直和水平方向的间距
*/
grid-gap: .5em;
grip-gap: .5em 1em;							/* 垂直方向.5em， 水平方向1em */
```

###### 6.网格布局的四个重要概念

```css
/*
 网格线（grid line）——网格线构成了网格的框架。一条网格线可以水平或垂直，也可以位于一行或一列的任意一侧。如果指定了 grid-gap 的话，它就位于网格线上。
 网格轨道（grid track）——一个网格轨道是两条相邻网格线之间的空间。网格有水平轨道（行）和垂直轨道（列）。
 网格单元（grid cell）——网格上的单个空间，水平和垂直的网格轨道交叉重叠的部分。
 网格区域（grid area）——网格上的矩形区域，由一个到多个网格单元组成。该区域位于两条垂直网格线和两条水平网格线之间。
*/
```

###### 7.grid例子

```css
.container{
    display: grid;
    grid-template-columns: 2fr 1fr;       /* 定义两个垂直的网格轨道 */
    grid-template-rows: repeat(4, auto);  /* 定义四个水平轨道，大小为auto */
    grid-gap: 1.5em;
    max-width: 1080px;
    margin: 0 auto;
}

/*  grid-row: span 1; 可以用span来指定grid-row和grid-column的值。span 1告诉浏览器元素需要占据一个网格轨道,因为这里没有指出具体是哪一行，所以会根据网格元素的布局算法，自动将其放到合适的位置*/
header,nav{
    grid-column: 1 / 3;   /* 从1号垂直网格线跨越到3号垂直网格线 */
    grid-row: span 1;     /* 刚好占据一条水平网格轨道 */
}

/* grid-column是grid-column-start和grid-column-end的简写, 1 / 2 代表越网格线1-2 */
/* grid-row是grid-row-start和grid-row-end的简写, 3 / 5 代表跨越网格线3-5 */
/* grid-row和grid-column的两个数字之间的/可以省略 */
.main{
    grid-column: 1 / 2;
    grid-row: 3 / 5;
}
.siderbar-top{
    grid-column: 2 / 3;
    grid-row: 3 / 4;
}
.siderbar-bottom {
    grid-column: 2 / 5;
    grid-row: 4 / 5;
}
```

###### 8.grid和flex的异同

```css
/*
1).flex本质上是一维的，而grid是二维的。
2).flex是以内容为切入点由内向外工作的，而网格是以布局为切入点从外向内工作的
*/

在网格中，首先要描述布局，然后将元素放在布局结构中去。虽然每个网格元素的内容都能影响其网格轨道的大小，但是这同时也会影响整个轨道的大小，进而影响这个轨道里的其他网格元素的大小。

用网格给网页的主区域定位是因为我们希望内容能限制在它所在的网格内，但是对于网页上的其他元素，比如导航菜单，则允许内容对布局有更大的影响。也就是说，文字多的元素可以宽一些，文字少的元素则可以窄一些。同时这还是一个水平（一维）布局。因此，用 Flexbox 来处理这些元素更合适。接下来用 Flexbox 给这些元素布局，完成整个页面。

当设计要求元素在两个维度上都对齐时，使用网格。当只关心一维的元素排列时，使用Flexbox。在实践中，这通常（并非总是）意味着网格更适合用于整体的网页布局，而 Flexbox 更适合对网格区域内的特定元素布局。继续用网格和 Flexbox，你就会对不同情况下该用哪种布局方式得心应手。
```

###### 9.命名的网格线

```css
/*有时候记录所有网格线的编号实在太麻烦了，尤其是在处理很多网格轨道时。为了能简单点，可以给网格线命名，并在布局时使用网格线的名称而不是编号。声明网格轨道时，可以在中括号内写上网格线的名称*/

grid-template-columns: [start] 2fr [center] 1fr [end];
/*这条声明定义了两列的网格，三条垂直的网格线分别叫作 start、center 和 end。之后定义网格元素在网格中的位置时，可以不用编号而是用这些名称来声明*/
grid-column: start / center;


还可以给同一个网格线提供多个名称，比如下面的声明
grid-template-columns: [left-start] 2fr [left-end right-start] 1fr [right-end];
/*在这条声明里，2 号网格线既叫作 left-end 也叫作 right-start，之后可以任选一个名称使用。这里还有一个彩蛋：将网格线命名为 left-start 和 left-end，就定义了一个叫作 left 的区域，这个区域覆盖两个网格线之间的区域。-start 和-end 后缀作为关键字，定义了两者之间的区域。如果给元素设置 grid-column: left，它就会跨越从 left-start 到 left-end 的区域。*/

grid-row: row 3 / span 2;
/* 从第三行网格线开始放置元素，跨越两个网格轨道 */

 grid-template-rows: repeat(4, [row] auto);
/* 将水平网格线命名为row,四个水平轨道，有五条网格线。重复使用一个名称完全合法 */

```

###### 10.命名网格区域

```css
/*命名网格区域。不用计算或者命名网格线，直接用命名的网格区域将元素定位到网格中。实现这一方法需要借助网格容器的 grid-template 属性和网格元素的 grid-area属性。*/
.container {
     display: grid;
     /* grid-template-areas命名网格区域 */
     grid-template-areas: "title title"
                      	  "nav nav"
                      	  "main aside1"
                     	  "main aside2";
     grid-template-columns: 2fr 1fr;
     grid-template-rows: repeat(4, auto);
     grid-gap: 1.5em;
     max-width: 1080px;
     margin: 0 auto;
}
/* grid-area使用命名网格区域 */
header {
 	grid-area: title;
}
nav {
 	grid-area: nav;
}
.main {
 	grid-area: main;
}
.sidebar-top {
 	grid-area: aside1;
}
.sidebar-bottom {
 	grid-area: aside2;
}

/* 警告: 每个命名的网格区域必须组成一个矩形。不能创造更复杂的形状，比如 L或者 U型。*/
```

###### 11.命名网格区域中空的网格单元

```css
/*还可以用句点（.）作为名称，这样便能空出一个网格单元。比如，以下代码定义了四个网格区域，中间围绕着一个空的网格单元。*/
grid-template-areas:  "top top right"
 					"left . right"
 					"left bottom bottom";
```

###### 12.三种可选方案，编号网格线，命名网格线，命名网格区域

```css
/* 当你构建一个网格时，选择一种舒适的语法即可。网格布局共设计了三种语法：编号的网格线、命名的网格线、命名的网格区域。最后一个可能更受广大开发人员喜爱，尤其是明确知道每个网格元素的位置时，这种方式用起来更舒服。 */
```

###### 13.隐式网格

```css
body{
    background-color: #709b90;
    font-family: Arial, Helvetica, sans-serif;
}
/* 拥有隐式网格行的网格 */
.portfolio{
    display: grid;

    /* 将最小列宽设置为200px,自动填充网格 */
    grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));

    /* 将隐式水平网格轨道的大小设置为1fr */
    grid-auto-rows: 1fr;
    grid-gap: 1em;
    grid-auto-flow: dense;            /* 开启紧凑的布局算法 */
}

/* 将特定图片放大，在水平和垂直方向上各占据两个网格轨道 */
.portfolio .featured{
    grid-row: span 2;
    grid-column: span 2;
}

.portfolio > figure{
    margin: 0;            /* 覆盖浏览器默认的外边距 */
}

.portfolio img{
    max-width: 100%;
}

.portfolio figcaption{
    padding: 0.3em 0.8em;
    background-color: rgba(0, 0, 0, .5 );
    color: #fff;
    text-align: right;
}
```

###### 14.子网格

```css
网格有一个限制是要求用特定的 DOM 结构，也就是说，所有的网格元素必须是网格容器的直接子节点。因此，不能将深层嵌套的元素在网格上对齐。
```

###### 15.img标签的object-fit属性

```css
img{
	flex: 1;
    max-width: 100%;
    object-fit: fill;					/* 默认值，会整个图片缩放，以填满img元素 */
}

object-fil: cover; 扩展图片,让它填满盒子（导致图片一部分被裁减）
object-fil: contain; 缩放图片，让它完整的填充盒子(导致盒子里出现空白)
```

###### 16.特性查询

```css
/*现在你已经大体上掌握了网格布局，你可能会问：难道要等到所有的浏览器都支持网格了才能使用它吗？不。只要你想用，现在就可以，但是要考虑如果浏览器不支持网格时应该如何布局，并给出回退的样式。CSS 最近添加了一个叫作特性查询（feature query）的功能，该功能有助于解决这个问题，如下代码片段所示。*/
@supports (display: grid) {
 ...
} 

/* 特性查询实践 */
@supports 规则可以用来查询所有的 CSS 特性。比如，用@supports (display: flex)来查询是否支持 Flexbox，用@supports (mix-blend-mode: overlay) 来查询是否支持混合模式
    
/* 这句声明既指定了支持非前缀版本属性的浏览器，也指定了要求用-ms-前缀的旧版 Edge 览器。 */
@supports (display: grid) or (display: -ms-grid)
```

###### 17.position属性

```css
/*
position 属性。它可以用来构建下拉菜单、模态框以及现代 Web 应用程序的一些基本效果。

position 属性的初始值是 static。前面的章节里用的都是这个静态定位。如果把它改成其他值，我们就说元素就被定位了。而如果元素使用了静态定位，那么就说它未被定位。

定位：它将元素彻底从文档流中移走。它允许你将元素放在屏幕的任意位置。还可以将一个元素放在另一个元素的前面或后面，彼此重叠。
*/
```

###### 18.固定定位 fixed ( 用来防止模态框，就是dialog )

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    body{
      font-family: Arial, Helvetica, sans-serif;
      min-height: 200vh;    /* 设置网页高度，让页面出现滚动条（为了演示） */
      margin: 0;
    }
    button{
      padding: 0.5em 0.7em;
      border: 1px solid #8d8d8d;
      background-color: white;
      font-size: 1em;
    }
    .top-banner{
      padding: 1em 0;
      background-color: #ffd698;
    }

    .top-banner-inner{
      width: 80%;
      max-width: 1000px;
      margin: 0 auto;
    }

    .modal {
      display: none;      /* 默认隐藏模态框 */
    }

    /* 打开模态框时，用半透明的蒙层挡住网页剩余内容 */
    .modal-backdrop{
      position: fixed;
      top: 0;
      right: 0;
      bottom: 0;
      left: 0;
      background-color: rgb(0, 0, 0, 0.5);
    }

    .modal-body{
      position: fixed;
      top: 3em;
      bottom: 3em;
      right: 20%;
      left: 20%;
      padding: 2em 3em;
      background-color: white;
      overflow: auto;             /* 允许模态框主题在需要时滚动 */
    }

    /* 关闭按钮绝对定位 */
    .modal-close{
      /* 会一层一层网上找最近的已定位的祖先元素 */
      position: absolute;
      top: .3em;
      right: .3em;
      padding: .3em;
      cursor: pointer;
    }

  </style>
</head>

<body>
  <header class="top-banner">
    <div class="top-banner-inner">
      <p>Find out what's going on at Wombat Coffee each
        month. Sign up for our newsletter:
        <button id="open">Sign up</button>
      </p>
    </div>
  </header>
  <div class="modal" id="modal">
    <div class="modal-backdrop"></div>
    <div class="modal-body">
      <button class="modal-close" id="close">close</button>
      <h2>Wombat Newsletter</h2>
      <p>Sign up for our monthly newsletter. No spam.
        We promise!</p>
      <form>
        <p>
          <label for="email">Email address:</label>
          <input type="text" name="email" />
        </p>
        <p><button type="submit">Submit</button></p>
      </form>
    </div>
  </div>

  <script>
    var button = document.getElementById('open')
    var close = document.getElementById('close')
    var modal = document.getElementById('modal')

    button.addEventListener('click', function(event){
      event.preventDefault()
      modal.style.display = 'block'
    })

    close.addEventListener('click', function(event){
      event.preventDefault()
      modal.style.display = 'none'
    })
  </script>
</body>
</html>
```

###### 19.相对定位

```css
/*
跟固定或者绝对定位不一样，不能用 top、right、bottom 和 left 改变相对定位元素的大小。这些值只能让元素在上、下、左、右方向移动。可以用 top 或者 bottom，但它们不能一起用（bottom 会被忽略）。同理，可以用 left 或 right，但它们也不能一起用（right 会被忽略）。
*/
```

###### 20.绝对定位和相对定位实现下拉菜单

```css
/* 鼠标悬停的时候显示菜单 */
.dropdown:hover .dropdown-menu{
	display: block;	
}
```

###### 21.下拉菜单完整版

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    .container{
      width: 80%;
      max-width: 1000px;
      margin: 1em auto;
    }
    .dropdown{
      display: inline-block;
      position: relative;     /* 创建包含快 */         
    }
    .dropdown-label {
      padding: .5em 1.5em;
      border: 1px solid #ccc;
      background-color: #eee;
    }
    .dropdown-menu{
      display: none;        /* 最初隐藏菜单 */
      position: absolute;
      left: 0;
      top: 2.1em;           /* 将菜单移动到下拉菜单下面 */
      min-width: 100%;
      background-color: #eee;
    }

    /* 鼠标悬停的时候显示菜单 */
    .dropdown:hover .dropdown-menu{
      display: block;
    }
    .submenu {
      padding-left: 0;
      margin: 0;
      list-style-type: none;
      border: 1px solid #999;
    }
    .submenu > li + li {
      border-top: 1px solid #999;
    }
    .submenu > li > a{
      display: block;
      padding: .5em 1.5em;
      background-color: #eee;
      color: #369;
      text-decoration: none;
    }
    .submenu > li > a:hover{
      background-color: #fff;
    }
  </style>
</head>
<body>
  <div class="container">
    <div class="dropdown">
      <nav>
        <div class="dropdown-label">Main Menu</div>
        <div class="dropdown-menu">
          <ul class="submenu">
            <li><a href="#">Home</a></li>
            <li><a href="#">Coffees</a></li>
            <li><a href="#">Brewers</a></li>
            <li><a href="#">Specials</a></li>
            <li><a href="#">About</a></li>
          </ul>
        </div>
      </nav>
    </div>
  </div>
  <h1>Wombat Coffee Roasters</h1>
</body>
</html>
```

###### 22.css三角形

```css
/* 元素没有宽和高，每条边都成了一个三角形 */
    div{
      width: 0;
      height: 0;
      border: 10px solid;
      border-color: tomato transparent transparent transparent;
    }
```

###### 23.css梯形

```css
div{
    /* 元素有宽度就是梯形,没有宽度就是三角形 */
    width: 10px;
    height: 10px;

    border: 10px solid;
    border-color: transparent tomato transparent transparent;
}
```

###### 24.层叠上下文和z-index

```css
浏览器会先绘制所有非定位的元素，然后绘制定位元素。默认情况下，所有的定位元素会出现在非定位元素前面。

通常情况下，模态框要放在网页内容的最后，</body>关闭标签之前。大多数构建模态框的JavaScript 库会自动这样做。因为模态框使用固定定位，所以不必关心它的标记出现在哪里，它会一直定位到屏幕中间。

一个层叠上下文包含一个元素或者由浏览器一起绘制的一组元素。其中一个元素会作为层叠上下文的根，比如给一个定位元素加上 z-index 的时候，它就变成了一个新的层叠上下文的根。所有后代元素就是这个层叠上下文的一部分。
```

###### 25.z-index控制层叠顺序

```css
z-index 属性的值可以是任意整数（正负都行）。拥有较高 z-index 的元素出现在拥有较低 z-index 的元素前面。拥有负数 z-index 的元素出现在静态元素后面。

/*
z-index注意点：
	1). z-index只在定位元素上生效，不能用它控制静态元素
	2). 给一个定位元素加上z-index可以创建层叠上下文
*/
```

###### 26.粘性定位

```css
浏览器还提供了一种新的定位类型：粘性定位（sticky positioning）。它是相对定位和固定定位的结合体：正常情况下，元素会随着页面滚动，当到达屏幕的特定位置时，如果用户继续滚动，它就会“锁定”在这个位置。最常见的用例是侧边栏导航。

.affix{
    position: sticky;		/* 给sticky添加粘滞定位，它会停留在视口1em的位置 */
    top: 1em;
}
```

### 响应式设计

###### 1.响应式设计三大原则

```
1). 移动优先：这意味着在实现桌面布局之前先构建移动版的布局。
2). @media规则：使用这个样式规则，可以为不同大小的视口定制样式。用这一语法，通常叫做媒体查询，写的样式只在特定条件下才会生效。
3). 流式布局：这种方式允许容器根据视口宽度缩放尺寸
```

###### 2.移动优先

```css
响应式设计的第一原则就是移动优先（mobile first），顾名思义就是构建桌面版之前要先构建移动端布局。这样才能确保两个版本都生效。

重点 做响应式设计时，一定要确保 HTML 包含了各种屏幕尺寸所需的全部内容。你可以对每个屏幕尺寸应用不同的 CSS，但是它们必须共享同一份 HTML。

提示 当设计移动触屏设备的时候，确保所有的关键动作元素都足够大，能够用一个手指轻松点击。千万不要让用户放大页面，才能点中一个小小的按钮或者链接。
```

###### 3.text-indent

```css
/* text-indent规定文本块中首行的缩进 */
text-indent: 5em;
white-space: nowrap;
overflow: hidden;
/* 上例代码让元素里的文本隐藏 */
```

###### 4.汉堡包unicode字符

```css
content: "\2261";
```

###### 5.js动态的给元素切换某个类

```javascript
// js动态的给元素切换某个类, 根据这个类css动态的决定是否显示元素
var menu = document.getElementById('main-menu')
menu.classList.toggle('is-open')
```

###### 6.给视口加上meta标签

```html
现在移动版设计已经完成，但是还差一个重要细节：视口的 meta 标签。这个 HTML 标签告诉移动设备，你已经特意将网页适配了小屏设备。如果不加这个标签，移动浏览器会假定网页不是响应式的，并且会尝试模拟桌面浏览器，那之前的移动端设计就白做了。

<meta name="viewport" content="width=device-width, initial-scale=1">

meta 标签的 content 属性里包含两个选项。首先，它告诉浏览器当解析 CSS 时将设备的宽度作为假定宽度，而不是一个全屏的桌面浏览器的宽度。其次当页面加载时，它使用initial-scale 将缩放比设置为 100%。
```

###### 7.媒体查询

```css
响应式设计的第二个原则是使用媒体查询。媒体查询（media queries）允许某些样式只在页面满足特定条件时才生效。这样就可以根据屏幕大小定制样式。可以针对小屏设备定义一套样式，针对中等屏幕设备定义另一套样式，针对大屏设备再定义一套样式，这样就可以让页面的内容拥有多种布局。
/* 当设备的视口宽度大于或等于560px的时候，样式才会生效。如果视口宽度小于560px,那么里面的所有规则会忽略 */
@media (min-width: 560px) {
     .title > h1 {
     	font-size: 2.25rem;
     }
} 
```

###### 8.媒体查询建议使用em

```css
/* 在媒体查询断点中推荐使用 em 单位。在各大主流浏览器中，当用户缩放页面或者改变默认的字号时，只有 em 单位表现一致。*/
/* 媒体查询中更适合用em,em是基于浏览器默认字号(通常是16px),下面将560px改成35em */
@media (min-width: 35em){
    .title > h1 {
        font-size: 2.25rem;
    }
}
```

###### 9.断点

```css
:root{
    box-sizing: border-box;
    font-size: calc(1vw + 0.6em);
}
.title > h1 {
 color: #333;
 text-transform: uppercase;
 font-size: 1.5rem;
 margin: .2em 0;
} 

@media (min-width: 35em) {
 .title > h1 {
 font-size: 2.25rem;
 }
} 

/*
通过缩放浏览器窗口就能测试标题样式。当窗口很窄的时候，标题是适应移动端的小字号。慢慢放大浏览器窗口，字号会平滑地改变，因为网页被设置了响应式（calc()）字号。只要网页宽度达到 35em（或者 560px），标题的字号马上就会变成 2.25rem。

在这里，560px 这个临界值被称为断点。大多数情况下，整个样式表里的媒体查询只会复用少数几个断点。
*/
```

