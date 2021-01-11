## vscode

##### 1.Emmet语法 

- https://flatuicolors.com/ 颜色网站

```js
// js获取到元素之后，设置属性和获取属性
element.setAttribute("className", "xxxx")
element.getAttribute("name")
```



```js
// 快速生成html模板,
!
    
// 缩放vscode工具栏字体大小(整体大小)
ctrl    + | -

// 按tab,生成h1标签，并且id为title
h1#title	

// 按tab键，生成h1标签，并且class为nice
h1.nice

// 按tab键生成ul标签，里面10个li标签
ul>li*10
div>p*5

// 生成div里面套p标签，再套span标签
div>p>span

// 生成兄弟元素 h2 div p
h2+div+p

// 子代和兄弟共同使用
div>h2+p
div>h2*5+p*5

h1.box				// 生成h1标签并且类为box
h1[title="title"]	 // 生成h1 title属性为title

h1{nice}			// 生成h1标签,内容为nice

h1#main.box{我是内容}	// 组合

div.box$*4			// 生成4个div,class分别为box1,box2,box3,box4
div{woshi$}*5		// 内容从woshi1~5
div{ncie$$}*20		// $$代表两位数,从00 到 99

.box				// 生成div,并且类为box
#box				// 生成div,并且id为box (div是默认的标签)

// css语法
w200				width:200

w200+h200			width:200   height:200

w200+h200+m20+p20	 margin:20px padding: 20px

m10px20px			margin:10px 20px

m10px-20			margin: 10px -20

m20-20-30-40		margin: 20px 20px 30px 40px

p10-20-30			padding 10px 20px 30px

fz20				font-size: 20px

fz1.5				font-size= 1.4em

fw700				font-weight: 700

bgc					background-color

dib					dispaly: inline-block


// 代码快速格式化
alt + shift + F 

// alt + 鼠标左键，选中多行的多个光标
alt + 鼠标左键

// alt + 上下箭头,移动一行代码 
alt + 上下箭头

alt + B 浏览器中打开

ctrl + shift + ` 打开终端

ctrl + alt + 上下箭头    光标选中多行, ESC取消选择
```

### CSS

##### 1.设置css编码和引入样式表

- @import 引入比 link的效率低，框架就用@import，平常用link
- CSS中的值, 如果是一个单词,或者有连接符, 不需要引号.如果中间有空格,则需要引号
- i 是斜体标签,现在一般用来做小图标

```html
<!-- link可以引入favicon.ico -->
<link rel="shortcut icon" href="./img/favicon.ico" type="image/x-icon">
```



```css
/* css文件设置utf-8编码，在文件的开头写 */
@charset "utf-8";

/* css中引入其他样式表 */
@import url("./base.css")

/* html中@import的方式引入css */
<style>
	@import url('./css/style.css')
</style>
        
/* 如果引入了两个样式表，如果有相同的样式，后面的会覆盖前面的 */
<link rel="" href=""/>
<link rel="" href=""/>	
    
 /* link元素的rel属性不能省略，用来指定文档与链接资源的关系 */
```

##### 2.选择器

```css
*  												选择所有
div												选择所有div
html,body {magin:0; padding:0}					   开发中常用的设置
.nice											. 类选择器
#nice											# id选择器

class不要用驼峰标识，建议用中划线
```

##### 3.常用的css属性

```css
color								前景色(文字颜色,边框颜色，删除线，下划线颜色等等)
font-size							文字大小
background-color 	 				 背景色
width								宽度	
height								高度

span元素设置宽度和高度是无效的，宽度和高度不适用于非行内替换元素
```

##### 4.颜色表示

```css
red,blue						关键字
rgb(20,20,20)			 		 rgb表示
rgba(0,0,255,0.5)		 		 最后一个a是alpha,值为0-1，代表透明度,0.1可以写成.1
#ff0000							十六进制表示
#0f1							#00ff11	可以写成#0f1,即两位相同的时候可以使用简写
transparent						 颜色中特殊的取值transparent，代表rgba四个值都为0，即透明
```

##### 5.文字属性

```js
// text-decoration设置文字装饰线
text-decoration: none						    无任何装饰
text-decoration: underline						下划线
text-decoration: overline						上划线
text-decoration: line-through	 	 	 		 中划线

a元素默认有下划线,可以通过 text-decoration:none 取消下划线, 取消a元素默认的蓝色
text-decoration: none
color: red

// letter-spacing设置字母的间距,默认值是0, 可以设置负数,会挤在一起
letter-spacing: 2px

// word-spacing 设置单词之间的间距,可以设置负数,会挤在一起
word-spacing: 10px

// text-transform用于设置文字的大小写转换
text-transform: capitalize		将每个单词的首字母转换成大写
text-transform: uppercase		将所有的字母转换成大写		
text-transform: lowercase		将所有的字母转换成小写

// 首行缩进
text-indent: 32px

// text-align 设置元素内容在元素中的水平对齐方式,不仅仅是文本,图片等也会居中
// 块级元素不能居中!!!但是设置display: inline-block即可
text-align: left;					左对齐
text-align: right;					右对齐
text-align: center;					居中
text-align: justify;				左右两端对齐,中间的间距,自动调整,对最后一行没有效果!!!!
text-aling-last: justify;			对最后一行有效果,需要加上这个属性,当只有一行时也可以用这个!!
```

##### 6.字体

```js
// font-size 字体大小
font-size: 100px				数值 + 单位, 1px代表一个像素, 浏览器默认是16px
font-size: 1em					1em 等于 父元素文本的大小
font-size: 50%					百分比,基于父元素,50%代表是父元素的50%

// font-familly 字体,默认是微软雅黑,"微软雅黑"也可以写成中文
// 设置字体,需要计算机中有这个字体
font-family: "Microsoft YaHei";
font-family: "arial";
// 一般为了防止计算机中没有这个字体,一般设置成多个,会从左到右匹配
font-family: "arial",Courier,monospace 

// font-weight 字体加粗,值: 100|200|300|400|500|600|700|900
// normal:等于400(默认) bold:等于700
font-weight: bold

// font-style 一般用来设置斜体
font-style: italic				斜体
```

##### 7.行高 (line-height)

- 一行文本占据的高度

```js
// 行高一般用来做垂直居中,将行高与父元素设置相同,则垂直居中
line-height: 100px	
```

##### 8.font

- font属性可以跟多个值,是一个缩写属性

```css
/* style varient weight size/line-height font-family 顺序 */
/* 前面3个可以任意调换顺序.也可以省略,
   line-height也可以省略,如果不省略必须跟在font-size后面 */
font: itailc small-caps bold 30px/50px "宋体";

/* a元素hover的时候,a元素下边的i元素的样式改变 */
#header .nav-right .group2 a:hover i{
  border-top-color: #ff1e32 ;
}

/* logo-search-cart相关样式 */
#header .operation{
  height: 100px;
  background-color: orange;
  position: relative;
}

/* 图片垂直居中,脱标元素垂直居中 */ 
#header .operation img{
  position: absolute;
  top: 0;
  bottom: 0;
  margin: auto 0;
}
```

##### 9.其他选择器

```js
// 1.属性选择器 [], 

// 只要有title属性就选择它
[title] {}

// 有title属性,并且title属性等于"div元素"
[title="div元素"]

// 有title属性,包含"one"三个字母
[title* = "one"]
```

##### 10.后代选择器

```js
// 选中div元素里面的span元素
div span

// div后面的p元素后面的span元素
div p span
```

##### 11.子选择器

```js
// 子选择器,只选择div的直接子span元素
div > span

// 选取box的直接子元素span
.box > span

// 选取.box下的直接.title
.box > .title
```

##### 12.兄弟选择器

```js
// div后面紧挨着的p元素
div+p

// div后面的p元素,并且他们是兄弟(不需要紧挨着)
div~p

// 交集选择器,必须是div元素,且class是one
div.one

// 交集选择器,三个都符合
div.one[title="test"]

// 并集选择器,用逗号分隔,符合其中一个即可
div, .one, [title="test"]
```

##### 13.伪类选择器

- 即不需要给元素加class,也可以通过某种方式选择元素
- 常见的伪类有:
  - 动态伪类:  ( :link :visited  :hover  :active  :focus )
  - 目标伪类:  (:target)            不常用
  - 结构伪类,最重要
  - 否定伪类  :not() 

```html
// 目标伪类,当a标签锚点跳转到指定的h2中,设置跳转到h2的样式
<style>
    :target{
        color: red
    }
</style>

<!-- 锚点：href可以跳转到指定的id -->
<a href="#title1"></a>
<a href="#title2"></a>
<a href="#title3"></a>

<h2 id="title1"></h2>
<h2 id="title2"></h2>
<h2 id="title3"></h2>
```

##### 14.元素伪类 ( 用得少  )

```html
<!-- 选择设置了disabled的元素,即不可用状态的元素 -->
:disabled{
	color: red
}

<!-- 如果属性值是bool类型，只写属性名默认为true -->
<button disabled></button>
```

##### 15.动态伪类 ( 用的稍微多一点 )

```js
// 必须按照这个顺序
a:link			未访问的链接
a:visited		已访问的链接
a:hover			鼠标挪动到链接上
a:active		激活的链接(鼠标在链接上长按未松开)

// :hover和:active可以应用在其他的元素上
p:hover
p:active					// tab键选到p元素就是active状态

// :focus 获取焦点的元素,基本上元素都可以设置focus. tab键选中聚焦. input框可以点击聚焦
:focus{
    color: red
}
<input />

// 去除a元素默认的focus(聚焦状态)效果,即没有边框
:focus{ outline: none; }

// 设置a不能被选中(tab键选中),tabindex设置成-1即可
<a tabindex="-1">nice</a>

// 如果直接给a设置了color: red, 那么相当于给a的所有动态伪类都设置了color: red
a{ color: red }

```

##### 16.结构伪类

###### :nth-child

```js
// 选中文档中所有的第三个子元素
:nth-child(3)

// 必须是p元素,且p元素必须处在文档中的第三个子元素
p:nth-child(3)

// n(自然数) :0,1,2,3,4,5,6...
// 全部都会选中
:nth-child(n)

// 必须是p元素,全部子元素p都会选中,不管第几个子元素,相当于选中所有
p:nth-child(n)

// 所有的偶数都变红(可以用来设置每隔一个改变颜色)
:nth-child(2n)

// even也代表偶数
:nth-child(even)

// 2n+1代表选中所有的位置在奇数的子元素
:nth-child(2n+1)

// odd也代表奇数
:nth-child(odd)

// 0 3 6 9 12 等位置的子元素都会选中
:nth-child(3n)

// 代表取前5个
:nth-child(-n+5)
```

###### :nth-last-child

```js
// 选取文档中所有的倒数第三个子元素
:nth-last-child(3)

// 倒数第1个,倒数第3个,倒数第5个....这样取
:nth-last-child(2n+1)
```

###### :nth-of-type

```js
// 类型,从相同的元素开始数,第三个p元素
p:nth-of-type(3)
```

###### :nth-last-of-type

```js
// 从p元素开始数,倒数第三个元素
p:nth-last-of-type(3)
```

```js
// 选取文档中第一个子元素
:first-child

// 选取文档中最后一个子元素
:last-child

:firt-of-type
:last-of-type

// 如果是父元素中唯一的一个子元素,则选择它
:only-child

// 是父元素中唯一的这种类型的子元素
:only-of-type

// 选中元素里面的内容是空的元素
:empty
```

##### 17.否定伪类

- **:not(x)** , **x**是一个简单选择器

```js
// 选择文档中除了p元素的所有元素
:not(p)

// 一般这样选择,空格代表是后代选择器
body :not(p)
```

##### 18.伪元素(经常用)

- 两种写法,一个冒号和两个冒号(开发中一般写两个,用来和伪类区分)

```js
// 第一行(很少用到)
::first-line
// 第一个字母(很少用到)
::first-letter

// p元素的第一行和p元素的第一个字母
p::first-line
p::first-letter

// ::before在一个元素的前面插入内容(经常用到)
// 在span元素前面加一个数字1, 1的颜色设置为red,右边的外边距设置为20px content为插入的内容,不能省略
// 当content不需要内容时,可以设置为空字符串''
span::before{
    content: "1";
    color: red;
    margin-right: 20px;
}

// ::after在一个元素的后面插入图片(经常用到)
span::after{
    content: url('../iamge/1.png');
    color: red;
    margin-right: 20px;
}

// 伪元素可以看成是行内元素,不可以设置高度和宽度,但是通过设置display:inline-block则可以设置宽度
span::after{
    background-color: purple;
    display: inline-block;
    width: 100px
}
```

##### 18.CSS特性

###### 1.继承: 有些属性可继承. 子元素继承父类的样式.有些属性不会继承,例如width, height

###### 2.层叠: 即子类的样式将父类的样式层叠掉,后写的样式把先写的样式层叠掉

```js
width: 100%					// width的百分比是基于包含块的,即父元素
width: inherit				// 强制继承父元素的宽度

color: red !important		// 给它最大的权重,不会被层叠掉
```

##### 19.列表常见的css属性

```css
/* span元素不可以设置height和width, 但是可以通过设置padding和margin修改元素大小 */
ul{
    /* none是前面什么都没有,设置为none的时候把padding和margin去掉开发中用的最多 */
    list-style-type: none;
    padding: 0;
    margin: 0;
}

/* list-style-image, list-style-position, list-style-type的缩写属性,顺序可以随意 */
/* 一般直接设置成none */
list-style: none;
```

##### 20.表格

```css
/* 表格的css属性设置 */
table{
    border: 1px solid #000;
    /* 边框合并 */
    border-collapse: collapse;
    /* 表格居中显示 */
    margin: 0 auto;
}

td{
    border: 1px solid #000;
    /* 内容与边框有间距 */
    padding: 10px 20px;
    /* 内容居中 */
    text-align: center;
}
```

##### 21.label的使用

```html
<from>
    <div>
        <label for="name">名字</label>
        <input type="text" id="name">
    </div>
</from>
```

##### 22.textarea不允许拖拽设置大小

```css
/* resize设置成none */
textarea{
	resize: none;
}
```

##### 23元素分类

```bash
块级元素(block-level elements)独占父元素一行:
div p h1-h6 li  ul  ol  dl  dt  dd  header  footer  hr  br ..........

行内级元素(inline-level elements)多个行内级元素可以在父元素的同一行中显示:
a img span  code  ifram button  video ......

替换元素(replaced elements) 元素本身没有实际内容,浏览器会根据元素的类型和属性,
来决定元素的具体显示内容:  <a></a> <input> 即标签里面没有内容
img input iframe video audio object .....

非替换元素(non-replaced elements): 元素本身有实际内容,浏览器会直接将内容显示出来:
div span strong p ....

块级元素一般都是非替换元素
行内元素才分替换元素和非替换元素

行内替换元素可以任意的设置宽高
行内非替换元素不可以设置宽高,而且宽度和高度是由内容确定的(可以通过padding和margin去伪装宽高)
```

##### 24.display属性

- 本身所有的标签都没有样式,是浏览器帮我们加上了样式
- 我们也可以将任何元素模仿成其他元素. 不过为了标签语义化,不会这样使用
- 行内元素的对齐方式: 基线对齐 !!!!

```css
/*块级元素,浏览器默认给块级元素加上了display: blok,它就成了块级元素,这是w3c规定的*/
display: block

/* 如果自己给行内元素加display: block 那么他就成了块级元素 */
span { display: block }

/* li的display为list-item, 有这个属性,也是块级元素.这些是由浏览器默认加的 */
display: list-item

/* display常见的取值 */
block			块级元素
inline			行内级元素,display不写,默认就是inline元素
none			隐藏元素
inline-block	 行内块级元素(拥有块级和行内的特性,不换行,又可以设置宽高)
```

##### 25.列表的样式重置

```css
ul{
    list-style: none;
    padding: 0;
    margin: 0;
}
```

##### 26.visibility

```css
/* display:none隐藏是不再占据空间 */
/* visibility:hidden 隐藏,是不显示,但它仍然会占据空间 */
div{
    height: 200px;
    visibility: hidden;
}
```

##### 27.overflow

- overflow用于控制内容溢出时的行为 

```css
.box{
    /* 图片太大,而div只设置了200px,则图片会溢出, overflow用于处理溢出后的内容 */
    background-color: pink;
    width: 200px;
    height: 200px;

    /* over-flow取值
    	visible: 溢出的内容照样可见(默认值)
    	hidden: 溢出的内容隐藏
    	scroll: 溢出的内容被裁减,但可以通过滚动机制查看(滚动条会占据宽度和高度)
    	auto: 自动根据内容大小是否溢出来决定是否提供滚动机制(显示不完自动提供滚动条)
    */
    overflow: auto;
}

/* overflow-x overflow-y 单独设置x和y轴的滚动或隐藏 */
overflow-x: auto;
overflow-y: hidden;
```

##### 28.元素之间的空格:行内级元素之间都会产生空格

```php+HTML
<!-- 写元素的时候紧挨着不留空格,就不会有空格,不建议!!!! -->
<span>nice</span><strong>nice</strong><div></div>

<!-- 设置浮动也可以 -->
float: left
```

##### 29.盒子模型content属性(内容属性)

```css
<style>
.box{
    background-color: #00cec9;

    /* 设置宽度相关 */
    width: 100px;
    /* min-with 最小宽度 */
    min-width: 1080px;
    /* max-width 最大宽度 */
    max-width: 1080px;

    /* 设置高度相关 */
    height: 100px;
    /* 最小高度 */
    min-height: 600px;
    /* 最大高度 */
    max-height: 200px;
}
</style>
```

##### 30.盒子模型padding	

```css
padding-left: 12px
padding-right: 12px
padding-top: 12px
padding-bottom: 12px
padding: 12px
```

##### 31.盒子模型margin

```css
/* margin的初始值是0 */
margin-left: 14px
margin-right: 13px
margin-top: 13px
margin-bottom: 12px
margin: 12px

/*两个外边距一起会折叠,理解成合并也行,折叠只针对于上下,不针对于左右*/
```

##### 32.margin顶部传递

​	- 父子关系才能传递,并且是子传父

```css
上下magin的传递(左右不会传递):
      margin-top传递:如果块级元素的顶部线和父元素的顶部线重叠,那么这个块级元素的margin-top值会传递给父元素
       margin-bottom传递的两个条件
      1.子元素和父元素的顶部线重叠
      2.父元素的height必须是auto

取消传递的两种方法
	1.给父元素设置border,就不会传递了
	2.触发BFC(BFC理解为结界),触发BFC的方式
		浮动float
		设置一个元素的overflow为 非visible(只要不是visible即可)
```

##### 33.margin和padding的使用

```css
/* margin一般是用来设置兄弟之间的间距 */
/* padding一般是用来设置父子之间的间距 */
```

##### 34.上下margin折叠(左右不会折叠)

```css
/* 上下margin会折叠,如果下为40px,上为20px,则取最大的40px */

/* 防止折叠: 只设置一个即可,不要让它们有折叠的机会 */

/* 父子元素的margin也会折叠,也是上下之间的传递,父子元素是上上折叠,下下折叠 */
```

###### 35.border

- 边框有三个属性:
  - 边框宽度
  - 边框颜色
  - 边框线条

```css
/* 边框宽度 */
border-top-width
border-right-width
border-bottom-width
border-left-width
border-width			/* 简写 */

/* 边框颜色也有上右下左和颜色 */
border-top-color
border-right-color
border-color

/* 边框样式,也有上右下左和简写
	取值:soild, duble, none, dotted
*/
border-top-style:
border-right-style:
border-style:

/* 一个缩写属性:不区分顺序 */
border: 1px solid red; 

/* 四个缩写属性 */
border-top
border-right
border-bottom
border-left
```

##### 36.以下属性对行内非替换元素不起作用!!!!!

```css
/*
	以下属性对行内非替换元素,不起作用,
	width, height, margin-top, margin-bottom
*/

/*
	以下属性对行内非替换元素的效果比较特殊
	padding-top, padding-bottom, border-top, border-bottom
	行内元素的上下padding不占据空间,上下会多出区域,区域不占据空间
	行内元素的上下border不占据空间
*/

/* 解决方式,设置display为inline-block */
display: inline-block;
```

##### 37.元素的圆角效果

```css
.box{
    height: 100px;
    width: 100px;
    background-color: #e00;

    /* 四个角落都可以单独设置圆角效果 左上,右上,左下,右下*/
    border-top-left-radius: 30px;
    border-top-right-radius: 30px;
    border-bottom-left-radius: 30px;

    /* 直接给全部设置一个30px */
    border-radius: 30px;

    /* border-radius直接画一个圆,百分比参考的是当前元素的border+padding+width*/
    border-radius: 50%;

}
```

##### 38元素的外轮廓

- outline外轮廓:不占据空间

```css
/* 查看网站的布局,使用谷歌浏览器给所有的div设置outline,为了防止被层叠掉,加!important */
/* 不要使用border, border会占据空间,有可能会使布局变乱 */
div {
	outline: 1px solid red !important
}

/* outline取值 */
outline-width:	2px;
outline-style:	solid;		/*dotted*/
outline-color:	#000;
/* outline缩写 */
outline: 2px solid #000;

/* outline应用在a元素,input元素的focus轮廓效果, 可以放在base.css中*/
a, input, textarea{
    outline: none;
}
```

##### 39.box-shadow

- box-shodow可以给盒子设置一个或者多个阴影

```css
.box{
    height: 100px;
    width: 100px;
    background-color: coral;

    /* 水平方向向右偏移5px,垂直方向向下偏移5px, 如果是-5px则是相反方向的偏移 */
    box-shadow: 5px 5px;

    /* -5px则水平方向向左偏移 */
    box-shadow: -5px 5px;

    /* 第三个值为模糊半径,即阴影的模糊效果,会向外扩散进行模糊 */
    box-shadow: -5px 5px 2px;

    /* 第四个值为向外延伸,在原来的基础上,向四周延伸 */
    box-shadow: -5px 5px 5px 10px;

    /* 可以写阴影的颜色 */
    box-shadow: -5px 5px 5px 10px pink;

    /* inset为阴影向里扩散,一般不用 */
   
}

/* 阴影可以设置多个值 */
.box{
    width: 200px;
    height: 300px;
    background-color: #fff;
    border: 1px solid pink;

    /* 设置右下左三个方向有阴影,阴影可以设置成多个.所以可以实现三个方向的阴影 */
    box-shadow: 5px 5px 10px palegreen,
        -5px 0 10px palegreen;
    margin: 0 auto;
}

/* 设置透明效果的阴影,颜色使用rgba即可 */
box-shadow: 0 10px 5px 10px rgba(0, 0, 0, .3);
```

##### 40.text-shadow(文字阴影)

```css
.box{
	font-size: 25px;
	font-weight: 700;

	/* 取值跟box-shadow一样 */			
	text-shadow: 5px 5px 3px pink;
}
```

##### 41.box-sizing盒子尺寸

```css
.box{     
      background-color: pink; 
      /* box-szing的默认值是content-box, */
      /* content-box; 它的含义是设置宽度是只是指定content(内容)的宽高 */
      /* border-box; 它的含义是设置宽度和高度是设置内容+内边距+边框的宽度 */
      box-sizing: border-box;
      width: 100px;
      height: 100px;
      padding-bottom: 10px;
}
```

##### 42.水平居中

```css
 /* 父元素中设置,普通的文本,行内元素,行内替换元素,行内块级元素inline-blocks都会居中 */
text-align: center;

/* 在子元素中设置:块级元素:block  */
margin: 0 auto;
```

##### 43.背景图

```css
.box{
    /* background-image用于设置元素的背景图片:会盖在background-color的上面 */
    width: 800px;
    height: 550px;
    background-color: sandybrown;

    /* 默认背景图太小显示了还有空余空间,则平铺满元素剩余的空间 */
    background-image: url('../imgs/1.jpg');

    /* 普通效果,默认平铺,no-repeat不平铺 */
    background-repeat: no-repeat;
}


/* background: 默认是auto,图片多大就显示多大,图片太小就平铺 */
/* cover为覆盖,覆盖整个元素,图片太小会将图片拉伸 */
background-size: cover;

/* 保持原来图片的宽高比,拉伸到垂直方向撑满元素 */
background-size: contain;

/* 设置width为父元素的50% */
background-size: 50%;

/* 设置width为父元素的50%, height为父元素的50% */
background-size: 50% 50%;

/* 设置背景图width为300px  height为300px */
background-size: 300px 300px;


/* background-position设置图片的位置 */
/* 100px 100px 距离坐标轴x轴100 y轴100, (0,0)的位置为元素的左上角 */
background-position: 100px 100px;

/* -100px -100px 则就会隐藏一部分 */
background-position: -100px -100px;

/* x轴居中,水平方向居中 */
background-position: center 0;

/* x轴y轴居中 */
background-position: center center;

/* x轴为最右边,y轴为最上边,则是右上角 */
background-position: right top;

/* 只写一个方向,默认另一个方向是center. 值设置一个80px也一样,另一个方向也是center*/
background-position: right;
```

###### 44.css sprite

1. css sprite是一种css图像合成技术,将各种小图片合并到一张图片上,然后利用CSS的背景定位来显示对应的图片部分

##### 45.background-attchment (很少用)

```css
/* scroll,默认只有内容会滚动 */
/* local 图片随内容的滚动而滚动 */
/* fixed 背景是固定的,不会随着内容的滚动而滚动 */
background-attachment: fixed;
```

##### 46.background简写属性

```css
.box{
    /* 
    background是一系列背景相关属性的简写属性,常用格式
    顺序是任意的，只有postion和size不是，size需要写在position后面，并且需要加上斜杠
    image position/size repeat attchment color 
    */
    background: url('../imgs/2.jpg') no-repeat red;
}
```

##### 47.cursor光标的形状

- cursor可以设置鼠标指针(光标)在元素上面时的显示样式

```css
/* auto 默认值,浏览器根据上下文决定指针的显示样式 */

/* default由操作系统决定,一般是一个小箭头,可以设置在具体的元素上 */
body{
    cursor: default;
}

/* point 是一只小手 */

/* text一条竖线 */

/* none没有任何指针显示在元素上面 */
```

## 定位

##### 1.标准流 ( normal flow )

```css
/* 
position常用的取值有4个:
    static    静态定位，默认值(按照标准流排列)
    relative  相对定位(相对于自己)原来的位置进行定位
    absolute  绝对定位,往上找父元素,如果父元素不是static,则相对它进行定位.如果是static则继续往上找
    如果一直找到HTML都是static,则相对于视口(视口在后面将会讲到)进行定位.
    fixed:    固定定位,相对于浏览器的视口进行定位

position设置之后,通过left right top bottom进行定位
*/
```

##### 2.relative

```css
/* relative定位,原来占据的空间不会改变.如果定位的地方有其他元素,则盖在其他元素上面 */
strong{
    position: relative;
    left: 50px;
    top: 50px;
}
/* 是距离bottom向下50个px, 等价于top:50px */
bottom: -50px
```

##### 3.不管如何缩放,让图片的中间部分始终显示在屏幕上

```css
.box{
    /* 盒子的宽度和浏览器的宽度保持一致,而不会被图片撑大 */
    overflow: hidden;
}

/* 让图片的中间显示在父元素(div)中 */
.box img{
    position: relative;
    /* 向左移动image的一半,如何相对于自己的50%呢,
    通过transform: translate(-50%) 相当于left: -50%(左移50%) */
    transform: translate(-50%);
    /* 向右移动父元素(div)元素的一半,margin是基于父元素的宽度的*/
    margin-left: 50%;
}
```

##### 4.固定定位fixed

```css
/* 固定定位会脱离文档流，并且设置top bottom right left属性是相对于视口的 */
a{
    position: fixed;
    top: 20px;
    right: 20px;
}
```

##### 5.脱标元素的特点（脱离标准流：固定定位，绝对定位，浮动都可以实现脱标）

脱离标准流的元素 position: fixed/absolute    float

- 可以随意设置宽度和高度
- 宽高默认由内容决定（div如果脱离标准流，它的宽度就不将是一行，一脱离就由内容决定）
- 不再给父元素汇报宽高数据

脱标元素和display的关系

- 脱标的元素一般就是变成block元素了，但是它没有占据父元素的整行，是因为block元素的宽高是auto，而脱标的元素没有父元素可参考，所以他就设置宽高为包裹内容

###### 6.绝对定位absoulte

- 开发中最常用到的就是
  - 父元素设置定位为relative,不脱离标准流
  - 子元素设置为absoulte, 脱离标准流并基于父元素进行绝对定位

```css
/* 父元素设置相对定位,父元素不脱离标准流 */
.outer{
    position: relative;
    width: 500px;
    height: 500px;
    background-color: #e6e6e6;
}

/* 子元素设置绝对定位，相对于父元素绝对定位 */
.inner{
    width: 100px;
    height: 100px;
    background-color: thistle;
    position: absolute;
    left: 100px;
    bottom: 100px;
}
```

##### 7.absolute的元素居中

```css
.beauty-left ul {
    position: absolute;
    bottom: 20px;
    width: 300px;
    background-color: yellowgreen;
    text-align: justify;
    text-align-last: justify;

    /* ul居中显示,因为这个元素是absolute的,所以如果想设置它水平局中,必须先给left和right一个固定的值,
    然后设置margin: 0 auto;
    */
    left: 0;
    right: 0;
    margin: 0 auto;
}
```

##### font

```css
/* 字体:大小12px/line-hight1.5  1.5代表是字体的1.5倍 */
font: 12px/1.5
```

##### 显示和隐藏

```css
/* 显示和隐藏 */

/* 默认是隐藏 */
.phone .code {
	display: none;
}

/* 鼠标悬停的时候显示 */
.phone span:hover + .code {
	display: inline;
}
```

##### 绝对定位公式的使用

- 定位元素永远是层叠在非定位元素上面的

```html
<!-- 
  对于绝对定位的元素来说
  定位参照对象的宽度 = left + right + margin-left + margin-right + 绝对定位元素的宽度
  定位参照对象的高度 = top + bottom + margin-top + margin-bottom + 绝对定位元素的高度
 -->


<style>
  .outer {
    position: relative;
    height: 600px;
    width: 600px;
    background-color: palevioletred;
  }

  /* 公式的使用情景,元素居中 */
  .inner {
    position: absolute;
    height: 300px;
    width: 300px;
    background-color: palegreen;

    /* 水平居中 */
    left: 0;
    right: 0;

    /* 垂直居中 */
    top: 0;
    bottom: 0;
    margin: auto auto;
  }
</style>
```

##### 定位元素的层叠关系z-index

- 定位元素永远覆盖在非定位元素上面
- 在后面的定位元素会覆盖在后面的定位元素上面(如果位置重叠了) 
- 可以通过设置 z-index, 改变层叠顺序

```css
/*
z-index属性用来设置定位元素的层叠顺序(仅对定位元素有效)
取值可以是正整数 负整数 0,
比较原则:如果是兄弟关系:z-index越大,层叠在越上面
如果不是兄弟关系:各自从元素自己以及祖先元素中,找出最邻近的2个定位元素进行比较
*/
z-index: 9;	
```

## 浮动

##### float介绍

- 在css中有3中常用的方法对元素进行定位,布局:
  - normal flow: 标准流, 常规流, 文档流
  - absolute position: 绝对定位
  - float: 浮动
- 绝对定位, 浮动都会让元素脱离文档流,以达到灵活布局的效果
- float常用的取值:
  - none: 不浮动,默认值
  - left: 向左浮动
  - right: 向右浮动

##### 浮动的规则

- 层叠关系: 标准元素 --> 浮动元素 --> 定位元素
- 脱离标准流的元素一般都是block元素

```js
/**
	元素一旦浮动后: 
	1.脱离标准流
	2.浮动之后朝着向左或者向右方向移动,直到自己的边界紧贴着包含块(一般是父元素)或者其他浮动元素的边界为止
	3.浮动元素不能与行内级元素层叠,行内元素将会被浮动元素挤出(如行内元素, inline-block)
	4.行内级元素,inline-block元素浮动后,其顶部将与所在行的顶部对齐
	5.块级元素不会被推出去,而会产生层叠现象
	6.浮动的元素只在当前所在行浮动
	7.如果父元素没有设置高度,里面的元素都进行了浮动,那么父元素的高度会消失(高度的坍塌)
	8.如果元素是向左浮动,浮动的元素左边界不能超出包含块的左边界(右边同理)
	9.浮动元素之间不能层叠
	10.如果水平方向剩余的空间不够显示浮动元素,则会换行浮动
	11.浮动元素的顶端不能超过包含块的顶端,也不能超过之前所有浮动元素的顶端(不管左浮还是右浮)
	12.浮动元素之间可以设置margin padding
	13.浮动可以解决行内,inline-block元素的水平间隙问题
*/
```

```css
/* 浮动只有三个取值 */
float: none
float: left
float: right
```

##### 前面的元素有margin-right, 最后一行的最后一个元素没有margin-right

```css
.container {
    width: 990px;
    height: 600px;
    background-color: darkorange;
    margin: 0 auto;
}

/* 多包一层wrap.并设置margin-right: -10px */
.wrap {
    margin-right: -10px;
}

.item {
    width: 190px;
    height: 100px;
    margin-top: 10px;
    background-color: dodgerblue;
    float: left;
    margin-right: 10px;
}
```

##### margin设置负值

```css
span {
    float: left;
    background-color: orchid;

    /* margin设置成负值,可以让后面浮动的元素,往这个元素层叠10px, 这个属性可以用来覆盖边框 */
    margin-right: -10px;
}

strong {
    float: left;
    background-color: palegreen;
}
```

##### 浮动高度坍塌(清除浮动)

- 由于浮动元素脱离了标准流变成了脱标元素,所以不再向父元素汇报高度
- 父元素计算高度时,就不会计算浮动子元素的高度,导致了高度坍塌的问题
- 解决父元素高度坍塌问题的过程,一般叫做清除浮动

```css
/* 使用clear清除浮动 */

/* 要求元素的顶部低于之前生成的所有左浮动元素的底部 */
clear: left

/* 要求元素的顶部低于之前生成的所有右浮动元素的底部 */
clear: right

/* 要求元素低于之前的所有左浮动和右浮动元素的底部 */
clear: both


/* 1.需要增加无意义的标签 */
<div class="clear-fixed"></div>
.clear-fixed {
    clear: both;
}

/* 2.在父元素最后一行增加br,并设置clear属性,也会多一个标签 */
<br clear="all">

/* 3.解决高度坍塌最终方案,给父元素设置一个::after伪元素,并设置clear属性 */
.container::after {
    /* 伪元素必须有content */
    content: ""
    clear: both;
    /* 必须设置伪block */
    display: block;
}

/* 4.一般的做法是定义一个类,然后哪个元素要解决高度坍塌,就加上这个类 */
.clear-fixed::after {
    content: "";
    clear: both;
    display: block;
}
```

##### 浮动的垂直居中

```css
/* 水平居中 */
position: fixed;
left: 0
right: 0
margin: 0 auto;

/* 垂直居中 */
position: fixed;
bottom: 0;
top: 0;
margin: auto 0;
```

##### 定位方案对比

```css
标准流(normal flow)			垂直布局
绝对定位(absolute position)		层叠布局
浮动(float)					水平布局
```

## transform

- tansform属性是用来做形变的,transform属性允许你旋转, 缩放, 倾斜或平移给定元素

```css
transform常见的函数
平移: translate(x,y)
缩放: scale(x,y)
旋转: rotate(deg)
倾斜: skew(deg, deg)
```

##### translate, 平移

```css
/* 水平向右位移10, 向下位移20, 左上角为坐标系的(0,0) */
/* 写一个值为只移动x轴 */
/* 百分比:参照元素本身的像素 */
transform: translate(10px, 20px);
```

##### scale缩放

```css
/* 
scale(x,y)
一个值时,设置x轴和y轴相同的缩放
二个值时,设置x,y轴上的缩放
值:
1: 保持不变
2: 放大一倍
0.5: 缩小一半
*/
.box {
    height: 100px;
    width: 100px;
    background-color: pink;
    margin: 100px auto 0;
}
.box:hover {
    /* x轴放大一倍,y轴 */
    transform: scale(2, 0.5);
}
```

##### transform-origin 缩放的原点(默认是中心点缩放)

```css
/* 缩放的原点设置为x轴上为center, y轴上为top */
transform-origin: center top;

transform-origin: left top;
```

##### rotate旋转

```css
/*
rotate旋转:
一个值:表示旋转的角度
deg:旋转的角度
正数为顺时针
负数为逆时针
*/
.box:hover {
    transform: rotate(45deg);
    /* 设置旋转原点 */
    transform-origin: top left;
}
```

##### skew倾斜

```css
/* 
一个值时表示x轴上的倾斜
二个值时表示x和y轴上的倾斜

值类型为
deg: 旋转的角度
正数为顺时针
负数为逆时针
*/
.box:hover {
    transform: skew(50deg, 50deg);
}
```

## transition动画

- 用来做过渡动画
- transition是transition-property, transition-duration, transition-timing-function和transition-delay的一个简写属性
- transition-property指定应用过渡属性的名称(如transform, width),可以写all表示所有可动画的属性
- transition-duration 指定过渡动画所需的时间,可以是秒(s)  毫秒(ms)
- transtion-timing-function 指定动画的变化曲线(ease, ease-in)动画的快慢
- transition-delay 指定过渡动画执行之前的等待时间

##### transition演示

```css
.box {
    width: 100px;
    height: 100px;
    background-color: brown;
    margin: 100px auto 0;

/* 宽度变化的时候执行动画,动画执行的时间为1秒,执行的动画曲线为linear(匀速),延迟500毫秒执行*/
    transition: width 1s linear 500ms;
}

.box:hover {
    width: 200px;
}
```

##### transition

```css
/* 第一个参数为all的话,则所有可执行动画的属性,都会执行动画 */
transition: all 1s ease 300ms;
```

## vertical-align (垂直对齐)

- vertical-align会影响行内级元素在一个行盒中垂直方向的位置
- 行盒必须把里面的元素包裹起来,所以行盒就有了高度

```html
<!-- 只有一个文本的时候,行高撑起了行盒(inline-box),行盒撑起了div -->
<div class="box">
    普通的文本
</div>
```

##### vertical-align值的理解

```css
/* 默认值是baseline 基线对齐 */
.outer {
    background-color: greenyellow;
    vertical-align: baseline;
}

/** 以下三种方式都可以让图片底部的像素消失 */
/* 顶部对齐 */
vertical-align: top;
/* 中间对齐 */
vertical-align: middle
/* 底部对齐 */
vertical-align: bottom

```

##### 图片的垂直居中(相对于父元素居中)

```css
img{
    posistion: relative;
    top: 50%;
    transform: translate(0,-50%)
}
```

## 媒体元素

##### video

```html
<!-- controls 工具栏(快进等拖动条),不加这个无法播放 -->
<!-- autoplay 自动播放(存在兼容性问题,谷歌浏览器不生效) -->
<!-- loop循环播放 -->
<!-- muted静音,生成静音自动播放就生效了 -->

<video controls autoplay loop muted src="https://vdept.bdstatic.com/5246.mp4?">
</video>


<!-- 在里面添加source也可以 -->
<video controls>
    <!-- 浏览器会找第一个source,如果不支持则寻找第二个source,如果不支持第三个source则寻找第四个,以此类推 -->
    <source src="https://vdept.bdstatic.com/46.mp4">
    <source src="https://vdept.bdstatic.com/527.mp4">
</video>
```

##### audio元素

```html
<!-- audio为音频, controls添加控制条, loop循环播放, muted静音 -->
<audio src="" controls loop autoplay></audio>

<!-- audio也可以使用source -->
<audio controls>
    <source src="" />
    <source src="" />
</audio>
```

##### input新增type

```html
<!-- 
type: text/password/checkbox/radio/button/sumit/reset
-->
<!-- html5新增的input类型 -->
<input type="date">
<input type="time">
<input type="number">
<input type="tel">
<input type="color">
<input type="email">
```

## flex布局

##### 1.开启flex布局的两种方式

```css
.box{
    /* 两种开启flex的模式,一种叫做flex(块级元素) 第二种: inline-flex(行内元素) */
    display: flex;
    display: inline-flex;
}
```

### ①flex-container的属性

```css
flex-flow				是flex-direction和flex-wrap的缩写
flex-direction			决定主轴的方向
flex-wrap				元素放不下的时候,换行显示
justify-content			决定了flex-item在main axis上的对齐方式
align-items				决定了flex-item在交叉轴(cross)上的对齐方式
align-content			决定了多行flex-item在cross axis上的对齐方式,用法与justify-content类似
```

##### flex-direction (决定主轴的方向, 也就决定了排布的方向)

```css
/* flex items默认是沿着主轴(main axis)从main start开始往main end方向排布 */
/* flex-direction决定了主轴(main axis)的方向,有4个取值 */
flex-direction:row					默认值(item从左向右)
flex-direction:row-reverse			 反转(item从右向左)
flex-direction:column				item从上到下排布
flex-direction:column-reverse		 item从下到上排布
```

##### justify-content ( 决定了item在main axis上的对齐方式  )

```css
/**
    justify-content (决定了item在main axis上的对齐方式,保持direction的排列顺序)
        flex-start    (默认值,main axis开始位置对齐)
        flex-end      (main axis结尾位置对齐)
        center        (居中对齐)
        space-between (等分对齐,左右靠两边)
        space-evenly  (等分对齐,左右两边也等分出空隙)
        space-around  (等分对齐,两边距离比较窄)
*/	
justify-content: center;
justify-content: flex-end;
justify-content: space-between;
justify-content: space-evenly;
justify-content: space-around;
```

##### align-items( 决定了flex-item在交叉轴(cross)上的对齐方式 )

```css
/**
align-items   	 决定了flex-item在交叉轴(cross)上的对齐方式
normal      	(默认值，效果和strech一样)
streth      	(当flex items在cross axis方向的size为auto时，会自动拉伸至填充flex container,前提：没有设置高度)
flex-start  	(交叉轴start方向对齐,和streth效果一致)
flex-end    	(交叉轴end方向对齐)
center      	(交叉轴的中间对齐)
baseline    	(与基线对齐,它的基线永远是文本第一行的基线)
*/

align-items: stretch;
align-items: flex-start;
align-items: flex-end;
align-items: center;
align-items: baseline;
}
```

##### flex-wrap (元素放不下的时候,换行显示)

```css
display: flex;
flex-wrap: wrap;
/**
flex-wrap: 当元素一行放不下的时候,希望换行显示(默认不会换行,item会收缩)
nowrap   		默认值,不换行显示
wrap     		换行显示
wrap-resever	换行的时候交叉轴进行反转
*/
```

##### flex-flow ( 是flex-direction和flex-wrap的缩写 )

```css
flex-wrap: wrap;
/**
flex-flow: flex-direction和flex-wrap的缩写属性
row-reserve wrap  反转并换行
*/
flex-flow: row-reverse wrap;
```

##### align-content ( 决定了多行flex-item在cross axis上的对齐方式,用法与justify-content类似 )

```css
flex-wrap: wrap;
/**
align-content ( 决定了多行flex-item在cross axis上的对齐方式,用法与justify-content类似 )
strech        默认值，cross轴上进行拉伸，有高度的情况下不会拉伸
flex-start    在交叉轴start上一次排布多行
flex-end      在交叉轴end上一次排布多行
cneter        在交叉轴的中心一次排布多行
space-between 在交叉轴start end位置进行排布，慢慢往中间排
space-evenly  在交叉轴上平均进行排布，左右两边的空隙也平均
space-around  在交叉轴上平均排布，左右两边的空隙是中间空隙的一半
*/
align-content: strech;
align-content: flex-start;
align-content: flex-end;
align-content: center;
align-content: space-between;
align-content: space-evenly;
align-content: space-around;
```



### ②flex-items上的属性

```css
flex
flex-grow
flex-basis
flex-shrink
order				决定了flex items的排布顺序
align-self
```

##### order ( 决定了flex items的排布顺序 )

```css
/* 
order: 决定了flex items的排布顺序，可以设置任意整数(正整数，负整数,0),值越小越排在前面，默认值是0, 会按照顺序排
*/
order: 1;			/* 在每一个小的item中设置 */
```

##### align-self (item在cross轴的位置)

```css
/* flex-container设置align-items为设置全部,而flex-container设置可以为一个item单独设置对齐 */
/* 会覆盖父元素的align-items属性 */
align-self: flex-end;
```

##### flex-grow ( 元素的宽度拉伸将父元素撑满, 平分剩下的宽度 )

```css
/*如果值都是正数,则按照值的大小对剩余的宽度进行等分*/
flex-grow: 1;
flex-grow: 1;
flex-grow: 1;

/* 写小数不会将剩余的空间分完,每个item分剩下元素的20% */
flex-grow: .2;
flex-grow: .2;
flex-grow: .2;
```

##### flex-shrink ( 收缩item,收缩的最小宽度,不能小于内容的宽度 )

```css
/*
在flex-container没有设置flex-wrap: wrap; 的情况下,item一行放不完就会收缩(平分收缩)
*/

/* 
flex-shrink ( 收缩item,收缩的最小宽度,不能小于内容的宽度 )
在flex-container没有设置flex-wrap: wrap; 的情况下,item一行放不完就会收缩(平分收缩)
默认值是1
*/

/*三个item按照1:2:3进行收缩*/
flex-shrink: 1;
flex-shrink: 2;
flex-shrink: 3;

/* 每个item收缩20%,收缩不完的挤出父元素 */
flex-shrink: .2;
flex-shrink: .2;
flex-shrink: .2;
```

##### flex-basic ( 用来设置在主轴上元素的大小 )

```css
/* 也就是设置item的宽度,比width的优先级高 */
flex-basic: 200px;
```

##### flex是flex-grow和flex-shrink和flex-basic的简写,flex属性可以指定1个2个或3个值

```css
/* 设置1个值时就是设置的flex-grow */
flex: 1

/* 写两个值时,第一个值就是flex-grow,如果第二个参数有单位,那么他就是flex-basic */
flex: 1 200px;

/* 写两个值时,第一个值就是flex-grow,如果第二个参数没有单位,那么他就是flex-shrink */
flex: 1 2;

/* 写三个值时,arg:flex-grow  arg2:flex-shrink  arg3:flex-basic */
flex: 1 2 300px;
```

## CSS补充

##### 网络字体的使用

```html
<style>
@font-face {
    /* 字体命名 */
    font-family: codewhy;
    /* 字体地址 */
    src: url('.\xxxx.ttf');
}
p{
    /* 使用地址 */
    font-family: codewhy;
}
</style>
```

#####  关键帧动画

```html
<style>
    .box{
      height: 100px;
      width: 100px;
      background-color: darkorange;
    }

    .box:hover{
      /* 使用关键帧动画
        arg1: 动画的名字
        arg2: 动画执行的时间
        arg3: 动画执行的快慢  
      */
      animation: test  3s linear;
    }

    /* 定义关键帧动画 */
    @keyframes test {
      /* 把状态分为100等分,0%为初始状态 */
      0% {
        transform: translate(0,0);
      }
      25%{
        transform: translate(200,0);
      }
      50%{
        transform: translate(200,200);
      }
      75%{
        transform: translate(0,200);
      }
      100%{
        transform: translate(0,0);
      }
    }
  </style>
```

##### 文字省略号

```css
p {
    background-color: pink;
    color: seagreen;
    width: 100px;
    height: 100px;
    margin: 0 auto;

    /* 让文本只显示一行,剩余的文本使用...省略 */
	
    /* 文字超出父元素宽度不自动换行 */
    white-space: nowrap;
    /* 溢出那行的结尾处用省略号表示 */
    text-overflow: ellipsis;
    /* text-overflow生效的前提是overflow: hidden */
    overflow: hidden;
}
```

##### 移动端适配

```html
/* 视口的设置 */
  <!-- 
    name="viewport"     对视口进行配置
    width=device-width  视口的宽度等于设备的宽度
    initial-scale=1.0   初始化缩放比例1.0
   -->
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
```

##### rem使用

```css
 
```

##### rem进行适配

```css
/*webpack进行打包的时候使用postcss-pxtorem插件，即可进行适配*/
```

###### 设置元素的从左到右的渐变色

```css
background: linear-gradient(left, #FE9543, #FF664F);

/* 90deg就是代表从左边往右边渐变,left在uniapp中生效,在原生中不生效 */
background: linear-gradient(90deg, #FE9543, #FF664F);
```

###### transform对行内元素无效

```css
span{
  /* 行内元素设置成inline-block则可以形变 */
  display: inline-block;
  transform: translate(50%)
}
```

 ###### inline行内元素

```js
// 不能设置宽度和高度, 不能设置margin-top/margin-bottom
// border-top/bottom, padding-top/bottom比较特殊,不占据空间
```

###### 进度条

```html
<!-- 进度条:两个div,都有背景,子div的盖住父div的就形成了一个进度条 -->
<div class="line">
  <div class="progress"></div>
</div>

#main .brand .left .line{
  height: 3px;
  background-color: #333;
}

#main .brand .left .line .progress{
  height: 3px;
  width: 100px;
  background-color: #ff1e32;
}
```

###### relative margin

```CSS
.phone{
  /* 当元素设置了position: relative的时候,希望改变他的位置,又希望它在标椎流中排布,使用margin */
  position: relative;
  /* 为了它还在标准流中排布,不适用left,而使用margin-left */
  margin-left: 300px;
}
```

###### width  auto

```css
  .down{
    /*display: flex;*/
    width: auto;                  /* 设置margin并且，宽度随之缩小，宽度需要设置为auto */
    height: 248px;
    background-color: green;
    margin: 0 30px;

  }
```

###### 使用border模仿子元素设置margin,其余宽高占满父元素剩余的宽高。因为直接设置margin会超出父元素，设置padding需要boxsizing,但是padding和内容之间不能区分背景颜色，所以使用border ( 下策 )

```css
  /* parent的目的是为了让子元素的border占自身空间， 子元素不设置margin来设置与父元素的编剧，使用border，因为margin会超出父元素的位置 */
  .parent{
    width: 100%;
    height: 100%;
    box-sizing: border-box;
    /*background-color: pink;*/
  }

  /* boder达成padding的效果 */
  .home{
    height: 100%;
    width: 100%;

    background-color: #FFF;
    box-sizing: border-box;
    
    border-top: 20px solid #F1F2F6;
    border-right: 30px solid #F1F2F6;
    border-bottom: 20px solid #F1F2F6;
    border-left: 20px solid #F1F2F6;
  }
```

###### box-sizing的用法

```css
// 用来解决替换元素宽度自适应问题，因为input标签设置宽高，会超出，有默认的padding和border
input, textarea, img, video, object{
    box-sizing: border-box;
}
```

###### div充满屏幕

```html
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    html,body{
      height:  100%;
      margin: 0;
      padding: 0;
      min-width: 1366px;
      min-height: 166px;
    }
	
    /* div设置高度100%是无效的，需要将html,body的高度设置成100%, 仅仅设置body也是无效的，因为body也没有具体的宽度值 */
    .my-div{
      height: 100%;
      background-color: hotpink; 
    }
    
  </style>
</head>
<body>
  <div class="my-div"></div>
</body>
</html>
```

###### 元素垂直居中

```css
.parent{
    display: flex;
}

.child{
    align-self: center;
}
```



