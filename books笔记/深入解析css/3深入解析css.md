```
hsl颜色网站
https://www.hslpicker.coom
```

###### 1.媒体查询类型

```css
/*可以将and关键字联合起来组成一个媒体查询*/
@media (min-width: 20em) and (max-width: 35em) { ... }

/*可以用逗号分隔，匹配小于等于20em的视口，以及大于等于35em的视口*/
@media (max-width: 20em), (min-width: 35em) { ... }
```

###### 2.min-width和max-width

```css
/*
min-width匹配视口大于特定宽度的设备，max-width匹配视口小于特定宽度的设备。它们被统称为媒体特征.
*/
```

###### 3.媒体查询使用link标签

```html
只有当min-width满足媒体查询的时候才会将large-screen.css文件的样式应用到页面
<link rel="stylesheet" media="(min-width: 45em)" href="large-screen.css" />
```

###### 4.断点

```css
移动优先的开发方式意味着最常用的媒体查询类型应该是 min-width。在任何媒体查询之前，最先写的是移动端样式，然后设置越来越大的断点。

/* 移动端样式，对所有断点都生效 */
.title {
 ...
}
/* 中等屏幕的断点，覆盖对应的移动端样式 */
@media (min-width: 35em) {
 .title {
  ...
 }
}
/* 大屏幕断点：覆盖对应的小屏幕和中等屏幕断点的样式 */
@media (min-width: 50em) {
 .title {
 ...
 }
} 
```

###### 5.流式布局

```css
响应式设计的第三个也是最后一个原则是流式布局（fluid layout）。流式布局，有时被称作液体布局（liquid layout），指的是使用的容器随视口宽度而变化。它跟固定布局相反，固定布局的列都是用 px 或者 em 单位定义。固定容器（比如，设定了 width: 800px 的元素）在小屏上会超出视口范围，导致需要水平滚动条，而流式容器会自动缩小以适应视口。

在流式布局中，主页面容器通常不会有明确宽度，也不会给百分比宽度，但可能会设置左右内边距，或者设置左右外边距为 auto，让其与视口边缘之间产生留白。也就是说容器可能比视口略窄，但永远不会比视口宽。

在主容器中，任何列都用百分比来定义宽度（比如，主列宽 70%，侧边栏宽 30%）。这样无论屏幕宽度是多少都能放得下主容器。用 Flexbox 布局也可以，设置弹性元素的 flex-grow 和flex-shrink（更重要），让元素能够始终填满屏幕。要习惯将容器宽度设置为百分比，而不是任何固定的值。

```

###### 6.超过最大断点时，字体不再增长

```css
:root {
  box-sizing: border-box;
  font-size: calc(1vw + 0.6em);
}
@media (min-width: 50em) {
 :root {
    font-size: 1.125em;
  }
} 	
```

###### 7.响应式图片

```css
/*
响应式图片的最佳实践是为一个图片创建不同分辨率的副本。如果用媒体查询能够知道屏幕的大小，就不必发送过大的图片，不然浏览器为了适配图片也会将其缩小。
*/

/* 1.为移动设备提供最小的图片 */
.hero {
     padding: 2em 1em;
     text-align: center;
     background-image: url(coffee-beans-small.jpg);
     background-size: 100%;
     color: #fff;
     text-shadow: 0.1em 0.1em 0.3em #000;
} 

/* 2.给中等屏幕提供稍大的图 */
@media (min-width: 35em) {
 .hero {
     padding: 5em 3em;
     font-size: 1.2rem;
     background-image: url(coffee-beans-medium.jpg);
 }
} 

/* 3.给大屏幕提供完整分辨率的图 */
@media (min-width: 50em) {
 .hero {
     padding: 7em 6em;
     background-image: url(coffee-beans.jpg);
   }
}
```

###### 8.img中src的响应式

```html
<!-- srcset属性是 HTML 的一个较新的特性。它可以为一个<img>标签指定不同的图片 URL，并指定相应的分辨率。浏览器会根据自身需要决定加载哪一个图片 -->
<img alt="A white coffee mug on a bed of coffee beans"
     src="coffee-beans-small.jpg"
     srcset="coffee-beans-small.jpg 560w,
     coffee-beans-medium.jpg 800w,
     coffee-beans.jpg 1280w"
/>

提示 图片作为流式布局的一部分，请始终确保它不会超过容器的宽度。为了避免这种情况发生，一劳永逸的办法是在样式表加入规则 img { max-width: 100%; }。
```

###### 9.响应式三大原则

```js
// 网页响应式设计的结构实现方式千变万化。最终这些方式都会归纳为三大原则：移动优先、媒体查询、流式布局。

 优先实现移动端设计。
 使用媒体查询，按照视口从小到大的顺序渐进增强网页。
 使用流式布局适应任意浏览器尺寸。
 使用响应式图片适应移动设备的带宽限制。
 不要忘记给视口添加 meta 标签。
```

### 大型应用中的CSS

###### 1.模块化css

```css
模块化 CSS（Modular CSS）是指把页面分割成不同的组成部分，这些组成部分可以在多种上下文中重复使用，并且互相之间没有依赖关系。最终目的是，当我们修改其中一部分 CSS 时，不会对其他部分产生意料之外的影响。

模块化css不是直接编写一个大型网页，而是编写页面的每个部分，然后按照你需要的效果组合在一起。

我们把样式表的每个组成部分称为模块（module），每个模块独立负责自己的样式，不会影响其他模块内的样式。也就是说，在 CSS 里引入了软件封装的原则。
```

###### 2.基础样式

```css
每个样式表的开头都要写一些给整个页面使用的通用规则，模块化 CSS 也不例外。这些规则通常被称为基础样式，其他的样式是构建在这些基础样式之上的。
```

###### 3.模块化就是使用类, 并不是其他特别的技术

```css
/* ---------------- 基础样式 -------------------- */
:root{
    box-sizing: border-box;
}
*,
*::after,
*::before{
    box-sizing: inherit;
}
body{
    font-family: Arial, Helvetica, sans-serif;
}

/* --------------- 消息模块 ----------------- */
.message{
    padding: 0.8em 1.2em;
    border-radius: 0.2em;
    border: 1.5px solid #265559;
    color: #265559;
    background-color: #e0f0f2;
}

/* 成功修饰符 */
.message--success{
    color: #2f5926;
    border-color: #2f5926;
    background-color: #cfe8c9;
}

/* 警告修饰符 */
.message--warning{
    color: #594826;
    border-color: #594826;
    background-color: #e8dec9;
}

/* 错误修饰符 */
.message--error{
    color: #59262f;
    border-color: #59262f;
    background-color: #e8c9cf;
}

/* -------------------- 按钮模块 -------------------- */
.button{
    padding: 0.5em 0.8em;
    border: 1px solid #265559;
    border-radius: 0.2em;
    background-color: transparent;
    font-size: 1rem;
}

.button--success{
    border-color: #cfe8c9;
    color: #fff;
    background-color: #2f5926;
}

.button--danger{
    border-color: #e8c9c9;
    color: #fff;
    background-color: #a92323;
}

/* 尺寸修饰符能够设置字体的大小。通过更改字号来调整元素相对单位em的大小，进而改变内边距和边框圆角的大小，而不需要重写已经定义好的值 */
.button--small{
    font-size: 0.8rem;
}

.button--large{
    font-size: 1.2rem;
}

双连字符的写法很容易区分哪部分是模块名称，哪部分是修饰符。

千万不要使用基于页面位置的后代选择器来修改模块。坚决遵守这个原则，就可以有效防止

样式表变成一堆难以维护的代码。
```

###### 4.多元素模块

```css
模块封装的一个非常重要的原则，我们把它叫作单一职责原则
```

###### 5.下拉列表中的三角形

```css
/* 下拉列表中的三角形使用伪元素，父元素relative,伪元素absoulte。然后伪元素就可以根据当前元素定位 */
.dropdown__toggle::after{
    content: "";
    position: absolute;
    right: 1em;
    top: 1em;
    border: 0.3em solid;
    border-color: black transparent transparent;
}
```

###### 6.工具类

```css
有时候，我们需要用一个类来对元素做一件简单明确的事，比如让文字居中、让元素左浮动，或者清除浮动。这样的类被称为工具类.

/* 工具类样式 */
.text-center {
 	text-align: center !important;
}
.float-left {
 	float: left;
}
.clearfix::before,
.clearfix::after {
 	content: " ";
 	display: table;
}
.clearfix::after {
	clear: both;
}
.hidden {
 	display: none !important;
} 

这里用到了两次!important。工具类是唯一应该使用 important 注释的地方。事实上，工具类应该优先使用它。这样的话，不管在哪里用到工具类，都可以生效。我敢肯定，任何时候为元素添加 text-center 类，都是想让文本居中，不想让其他样式覆盖它。用了 important 注释就可以确保这一点。
```

#### 模式库

###### 1.模式库

```js
把模块清单整合成一组文档，在大型项目中已经成为通用做法。这组文档被称为模式库（pattern library）或者样式指南（style guide）。模式库不是网站或者应用程序的一部分，它是单独的一组 HTML 页面，用来展示每个 CSS 模块。模式库是你和你的团队在建站的时候使用的一个开发工具。

通常来讲，CSS 是一门“只增不减”的语言。开发者害怕去编辑或者删除那些已有的样式，因为他们没办法完全预知修改造成的结果。这时他们就会添加更多的代码到样式表的底部，来覆盖之前的样式规则，或者增加选择器优先级，最终导致样式表变成了一堆难以维护的乱码。

以模块化的方式来组织 CSS 代码，再维护一套与之相应的模式库，就可以避免陷入这样尴尬的境地。你总是知道某个模块的样式位于何处。每个模块只用来做一件事。同时，模式库还有助于开发者密切监视样式表开发过程中的进展情况。
```

### 背景、阴影和混合模式

###### 1.背景线性渐变

```css
/* 渐变方向：to right, to left, to bottom, to bottom right */
.fade{
	background-image: linear-gradient(to right, white, blue);
}

/* 多个颜色节点渐变 */
.fade{
	background-image: linear-gradient(to right, green, white, blue); 
}

/* 节点的位置可指定 */
.fade{
    background-image: linear-gradient(to right, green 0%, white 30%, blue 100%);
}
```

###### 2.条纹

```css
/* 如果同一个位置设置两个颜色节点，那么渐变会直接从一个颜色变换到另一个，而不是平滑过渡 */
.fade{
    width: 200px;
    height: 50px;
    background-image: linear-gradient(to right, red 40%, white 40%, white 60%, blue 60%);
}
```

###### 3.渐变创建条纹进度条

```css
.fade{
    height: 1em;
    width: 400px;
    background-image: repeating-linear-gradient(-45deg, #57b, #57b 10px, #148 10px, #148 	20px);
    border-radius: 0.3em;
}
```

###### 4.基础径向渐变

```css
/* 径向渐变：从一个点开始，全方位向外扩展 */
.fade{
    height: 300px;
    width: 300px;
    border-radius: 50%;
    background-image: radial-gradient(white, blue);
}
```

###### 5.阴影

```css
/*
arg1: 水平偏移
arg2: 垂直偏移
arg3: 模糊半径
arg4: 扩展半径
arg5: 颜色

模糊半径用来控制阴影边缘模糊区域的大小，可以为阴影生成一个更柔和、有点透明的边缘。扩展半径用来控制阴影的大小，设置为正值可以使阴影全方位变大，设为负值则会变小。
*/
box-shadow: 2px 2px 2px 1px black;
```

###### 6.阴影实现拟物化按钮效果

```css
.button{
    padding: 1em;
    border: 0;
    font-size: 0.8rem;
    color: white;
    border-radius: 0.5em;
    outline: none;
    background-image: linear-gradient(to bottom, #57b, #148);
    /* 带0.5em模糊半径的深蓝色阴影 */
    box-shadow: 0.1em 0.1em 0.5em #124;
}

.button:active{
    /* 两个内部盒阴影,inset关键字替换之前的盒阴影 */
    box-shadow: inset 0 0 0.5em #124,
        	    inset 0 0.5em 1em rgb(0, 0, 0, 0.4)
}
```

###### 7.阴影实现扁平化按钮

```css
.button{
    padding: 1em;
    border: 0;
    color: white;
    background-color: #57b;
    font-size: 1rem;
    outline: none;
    padding: 0.8em;
    /* 淡淡的阴影 */
    box-shadow: 0 0.2em 0.2em rgb(0, 0, 0, 0.15);
}

/* 鼠标悬停和计划状态下，颜色稍微加深 */
.button:hover{
    background-color: #456ab6;
}

.button:active{
    background-color: #148;
}
```

###### 8.当下流行的按钮样式

```css
.button{
    padding: 0.8em;
    border: 0;
    font-size: 1rem;
    color: white;
    outline: none;
    border-radius: 0.5em;           /* 改回圆角效果 */
    background-color: #57b;
    box-shadow: 0 0.4em #148;     /* 按钮下方添加阴影，无模糊效果 */
    text-shadow: 1px 1px #148;    /* 添加细小的文本阴影 */
}
.button:active{
    background-color: #456ab5;
    transform: translateY(0.1em);   /* 点击按钮时，按钮下移 */
    box-shadow: 0 0.3em #148;     /* 减小阴影的大小，用来抵消按钮的位移 */
}
```

###### 9.tip

```css
渐变和阴影可以以各种各样的方式组合使用。随着时间的推移，又会有新的设计流行起来。我们要做的是，在看到某个网站上的新设计时，停下来花些时间，用浏览器的开发者工具检查一下，看看这是如何实现的。
```

#### 混合模式

###### 1.基础

```css
大部分情况下，不论是使用真正的图片还是渐变，元素一般只会使用一张背景图片。但某些情况下你可能想要使用两张或者更多的背景图片，CSS 是支持这么做的。

background-image 属性可以接受任意数量的值，相互之间以逗号分隔
background-image: url(bear.jpg), linear-gradient(to bottom, #57b, #148);
```

###### 2.混合两张背景图片

```css
<div class="blend"></div>

.blend{
    min-height: 400px;
    background-image: url(bear.jpg), url(bear.jpg);
    /* 把背景图像扩展至足够大，以使背景图像完全覆盖背景区域。*/
    background-size: cover;
    background-repeat: no-repeat;
    /* 为每张背景图片设置不同的初始位置 */
    background-position: -30vw, 30vw;
    /* 应用混合模式 */
    background-blend-mode: multiply;
}

/*
background-size属性接受了两个特殊的关键字值，分别是cover和contain。使用 cover值可以调整背景图片的大小，使其填满整个元素，这样会导致图片的边缘被裁切掉一部分；使用contain值可以保证整个背景图片可见，尽管这可能导致元素的一些地方不会被背景图片覆盖。该属性也可以接受长度值，用来明确设置背景图片的宽度和高度。
*/
```

###### 3.将背景颜色的色相混合到背景图片上

```css
/*明度混合模式将前景层（大熊图片）的明度，与背景层（蓝色背景色图层）的色相和饱和度混合。也就是说，最终是完全使用背景色图层的颜色，但是明暗程度来自大熊图片。明度混合模式（还有其他几种类似的混合模式）最终的渲染结果，取决于哪个图层在其他图层之上，这是非常重要的。背景色图层始终在最下层，其他背景图片叠放在背景色图层之上。*/
<div class="blend"></div>
.blend{
    min-height: 400px;
    background-image: url(bear.jpg);
    background-color: #148;
    background-size: cover;
    background-repeat: no-repeat;
    background-position: center;
    /* 使用明度混合模式 */
    background-blend-mode: luminosity;
}
```

###### 4.混合模式5大类(background-blend-mode的取值)

```css
变暗 multiply: 前景色越亮，背景色显示出来的越多
	darken: 选择两个颜色中较暗的那个
	color-burn: 加深背景色，增加对比度


变亮 screen: 前景色越暗，背景色显示出来的越多
	lighten: 选择两个颜色中较亮的那个
	color-dodge: 加亮背景色，降低对比度 

对比 overlay: 对暗色使用multiply，对亮色使用screen，以增加对比度，对比效果较柔和
	hard-light: 大幅增加对比度，有点像叠加，但是使用加强版的multiply或者screen，对比效果明显
	soft-light: 有点类似于hard-light，但是使用burn/dodge来代替multiply/screen

复合 Hue: 将上层颜色的色相混合到下层颜色上
	saturation: 将上层颜色的饱和度混合到下层颜色上
	luminosity: 将上层颜色的明度混合到下层颜色上
	color: 将上层颜色的色相和饱和度混合到下层颜色上

比较 difference: 从亮色中减去暗色
	exclusion: 类似于difference，但对比度稍弱
```

###### 5.为图片添加纹理

```css
混合模式的另一个应用场景就是为图片添加纹理效果。比如你有一张富有现代气息的清晰图片，但有时候出于样式考虑，你想让图片与众不同。这时候就可以使用灰度图为图片手动添加胶片噪点效果或者其他纹理。
```

###### 6.使用混合融合模式

```css
mix-blend-mode，可以融合多个元素。这样不仅可以混合图片，还可以把元素的文本和边框与容器的背景图片混合在一起。使用融合混合模式，可以把标题显示在图片上方，但遮住的图片部分依然可以显示出来


<div class="blend">
	<h1>Ursa Major</h1>
</div>

.blend{
    background-image: url(bear.jpg);
    background-size: cover;
    background-position: center;
    padding: 5em 0 10em;
}
.blend > h1{
    margin: 0;
    font-family: Arial, Helvetica, sans-serif;
    font-size: 6rem;
    text-align: center;
    mix-blend-mode: hard-light;     /* 使用强光混合模式 */
    background-color: #c33;
    color: #808080;
    border: 0.1em solid #ccc;
    border-width: 0.1em 0;
}
```

#### 对比颜色和间距

###### 1HSL颜色

```css
HSL 是一种更适合人类读取的颜色表示法，代表色相、饱和度和明度（或者光度）。HSL的语法看起来就像 hsl(198, 73%, 46%)这样，相当于十六进制表示的#2097c9。

/*
hsl()函数需要 3 个参数。
第一个参数表示色相，是一个 0~359 的整数值。这代表色相环上的 360 度，从红色（0）、黄色（60）、绿（120）、青色（180）、蓝色（240）、洋红色（300）依次过渡，最后回到红色。

第二个参数表示饱和度，是一个代表色彩强度的百分数，100%的时候颜色最鲜艳，0%就意味着没有彩色，只是一片灰色。

第三个参数表示明度，也是百分数，代表颜色有多亮（或者多暗）。大部分鲜艳的颜色是使用 50%的明度值。明度值设置得越高，颜色越浅，100%就是纯白色；设置得越低，颜色越暗，0%就是黑色。

熟悉 HSL 最好的办法，就是去使用它。推荐一个网站 HSLColor Picker，这个网站提供了一个交互式颜色选择器，有 3 个滑动条分别用来选择 3 个参数值，还有一个用来设置透明度。可以尝试拖动一下滑动条，看看它们是如何影响最终生成的颜色的。

RGB和 HSL表示法都有一个对应的包含 alpha通道的表示法：rgba()和 hsla()。它们接受第四个参数，一个取值范围在 0～1 的数字，用来表示透明度。

总之，在实际项目开发中，我们没有必要把所有的颜色都转换成 HSL 格式。什么时候有需要了，使用 HSL 可以轻松处理颜色。
*/
```

###### 2.chrome调试获取颜色值

```css
F12调试按钮，查看颜色有个小方块，按住shift按钮点击，会变换16进制的值和rgb的值和hsl的值
```

###### 4.inline实现自定义列表

```css
.tag-list{
  list-style: none;
  padding-left: 0;
}

/* 行高可以影响垂直间距 */
.tag-list > li {
  display: inline;					
  padding: 0.3rem 0.5rem;
  font-size: 0.8rem;
  border-radius: 0.2rem;
  background-color: var(--light-gray);
  /* 设置很大的行高，这行的时候增加垂直间距.只有行内元素有这种行为 */
  line-height: 2.6;
}
```

###### 5.总结

```css
在你写 CSS 时，我一直在建议你花些时间来完善设计细节。即使你做的没有设计师那么专业，也要相信自己的眼睛。试着这里调大一点距离或者那里调小一点距离，看看哪种看起来更好，多花点儿时间来调试。不要过分使用颜色，只是有选择地放在需要吸引用户注意的地方。创建一致的模式，然后打破这些规律的模式就可以把用户的注意力吸引到页面最重要的东西上了。
```

### 排版

###### 1.web字体

```css
Web 字体使用@font-face 规则，告诉浏览器去哪里找到并下载自定义字体，供页面使用。

/* www.googlefonts.net 可以去谷歌字体下载字体 */
```

###### 2.字型和字体

```js
字型（typeface）和字体（font）这两个术语经常被混为一谈。字型通常是指字体（比如 Roboto）的整个家族，一般由同一个设计师创造。一种字型可能会存在多种变体和字重（比如细体、粗体、斜体、压缩，等等），这些变体的每一种可称之为一种字体（font）。
```

###### 3.网络字体使用

```css
/* 1.谷歌字体(www.googlefonts.net)中选中字体，并复制链接*/
<link href="https://fonts.font.im/css?family=Lato" rel="stylesheet">

/* 2.标签中使用 */
h1{
    font-family: 'Lato', sans-serif;
}
```

###### 4.line-height

```js
line-height 属性的初始值是关键字 normal，大约等于 1.2（确切的数值是在字体文件中编码的，取决于字体的 em 大小），但是在大部分情况下，这个值太小了。对于正文主体来说，介于 1.4 和 1.6 之间的值比较理想。
```

###### 5.letter-spacing

```css
/* 为字符之间添加0.01em的额外间距，letter-spacing一般每次值增加 1/100 em */
letter-spacing: 0.01em;

/* letter-spacing设置成负数可以让字体更紧凑 */
letter-spacing: -0.01em;
```

###### 6.把设计稿中的行距和字距转换成css

```
行距转换成line-hight： 把点值乘以 1.333，就可以把pt转化为 px，然后除以字号，得到无单位的行高值，即 24px/16px=1.5。

字距转换成letter-spacing: 字距通常会给定一个字数，比如 100。因为这个数字表示 1em 的千分之一，所以除以 1000 就可以转化成 em 单位，即 100/1000=0.1em。
```

###### 7.字体的加载

```css
大多数的浏览器供应商为了尽可能快地渲染页面，使用了可用的系统字体。然后，一小段时间过去了，Web 字体加载完成，页面会使用 Web 字体重新渲染一次。
```

###### 8.使用Font Face Observer监测字体加载

``` html
<script type="text/javascript">
     var html = document.documentElement;
     var script = document.createElement("script");
     script.src = "fontfaceobserver.js";
     script.async = true;
     script.onload = function () {
         var roboto = new FontFaceObserver("Roboto");
         var sansita = new FontFaceObserver("Sansita");
         var timeout = 2000;
         Promise.all([
            roboto.load(null, timeout),
            sansita.load(null, timeout)
         ]).then(function () {
            html.classList.add("fonts-loaded");
         }).catch(function (e) {
            html.classList.add("fonts-failed");
         });
     };
     document.head.appendChild(script);
</script>
```





