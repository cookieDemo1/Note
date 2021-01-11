### CSS

##### 1.选择器

- normalize.css
- http://www.jq22.com/

```css
--font-size: 14px					/* css定义变量 */
body{font-size: var(--font-size)}	  /* css使用变量 var() */
```

###### css尺寸

```css
px		像素
em		元素的字体高度
%		百分比
rem		根元素的font-size
vw		视图宽度, 1vw=视窗宽度的1%
vh		视窗高度，1vh=视窗高度的1%

/* 
em 和 rem的区别:
	rem是相对于根元素（html）的字体大小
	em是相对于其父元素的字体大小
*/
```

###### 基础

```css
h1 {color:red; font-size:14px;}

/* 两种颜色的方式 */
p { color: rgb(255,0,0); }
p { color: rgb(100%,0%,0%); }

/* 如果值为若干单词，则要给值加引号：*/
p {font-family: "sans serif";}

/* 应该在一行写一个属性 */
p {
  text-align: center;
  color: black;
  font-family: arial;
}

/* 选择多个元素 */
h1,h2,h3,h4,h5,h6 {
  color: green;
}

/* 子元素会继承父元素中的内容 */
body  {
     font-family: Verdana, sans-serif;
     }

/* 选择列表中的strong元素 */
li strong {
    font-style: italic;
    font-weight: normal;
  }

/* 选择h2中的strong，h2可以嵌套strong标签 */
h2 strong {
     color: blue;
     }

/* id选择器 */
#red {color:red;}

/* id选择器，通常用于派生选择器 */
#sidebar p {
	font-style: italic;
	text-align: right;
	margin-top: 0.5em;
	}

/* id选择器，选择div里面id为sidebar的元素 */
div#sidebar {
	border: 1px dotted #000;
	padding: 10px;
	}

/* 类选择器 */
.center {text-align: center}

/* 类的派生选择器 */
.fancy td {
    color: #f60;
    background: #666;
}

/* 元素也可以基于类被选择，选择td元素，并且td元素的类为fancy */
td.fancy {
	color: #f60;
	background: #666;
	}

/* 属性选择器：选择有title属性的选择器 */
[title]{
color:red;
}

/* 属性和值选择器。选择具有title属性，且属性的值为W3School的元素 */
[title=W3School]
{
border:5px solid blue;
}

/* 标签选择器和属性选择器的组合 */
input[type="text"]
{
  width:150px;
  display:block;
  margin-bottom:10px;
  background-color:yellow;
  font-family: Verdana, Arial;
}

input[type="button"]
{
  width:120px;
  margin-left:35px;
  display:block;
  font-family: Verdana, Arial;
}
```

##### 2.样式

```css
/* 引入外部样式 */
<link rel="stylesheet" type="text/css" href="mystyle.css" />

/* 单个文档需要特殊的注释时，使用内部样式表 */
<style type="text/css">
  hr {color: sienna;}
</style>

/* 内联样式 */
<p style="color: sienna; margin-left: 20px">

/* 背景图的写法 */
background-image: url("images/back40.gif");
```

##### 3.背景

```css
/* 如果您希望背景色从元素中的文本向外少有延伸，只需增加一些内边距：*/
p {
    background-color: gray;
    padding: 20px;
}

/*
background-color 不能继承，其默认值是 transparent。transparent 有“透明”之意。也就是说，如果一个元素没有指定背景色，那么背景就是透明的，这样其祖先元素的背景才能可见。
*/

/* 为一个段落应用了一个背景，而不会对文档的其他部分应用背景： */
p.flower {background-image: url(/i/eg_bg_03.gif);}

/* 为行内元素设置背景图 */
a.radio {background-image: url(/i/eg_bg_07.gif);}

/* 背景平铺，从左上角开始平铺，repeat-x水平方向平铺，repeat-y垂直方向平铺，no-repeat不允许平铺 */
body
  { 
  background-image: url(/i/eg_bg_03.gif);
  background-repeat: repeat-y;
  }

/* 背景定位，backgrond-position:center,可以使用的关键字top、bottom、left、right 和 center。 */
body
  { 
    background-image:url('/i/eg_bg_03.gif');
    background-repeat:no-repeat;
    background-position:center;
  }

/* 百分号和px方式的定位 */
background-position:66% 33%;
background-position:50px 100px;

/* 防止滚动，即滚动的时候，背景图不断下拉直至消失 */
background-attachment:fixed
```

##### 4.文本

```css
/* text-indent首行缩进，所有段落首行缩进5em */
p {text-indent: 5em;}

/* 设置为负值，悬挂缩进 */
p {text-indent: -5em;}

/* 缩进父元素的20% */
p {text-indent: 20%;}

/* text-align 水平对齐 */
text-align:center

/* word-spacing 属性可以改变字（单词）之间的标准间隔,默认值为normal即0
 负值则拉近他们的距离*/
p.spread {word-spacing: 30px;}
p.tight {word-spacing: -0.5em;}

/* 字母间隔 letter-spacing */
h1 {letter-spacing: -0.5em}
h4 {letter-spacing: 20px}

/* text-transform处理文本大小写，uppercase 和 lowercase 将文本转换为全大写和全小写字符
  capitalize 只对每个单词的首字母大写。*/
h1 {text-transform: uppercase}

/*  text-decoration文本装饰，默认为none
    underline下划线
    overline 上划线
    line-through 则在文本中间画一个贯穿线
    blank文本闪烁
*/	
a {text-decoration: none;}

/* 设置字体颜色 */
color：red

/* line-height设置行高 */
line-height：100px
```

##### 5.字体

```css
/*
css内置了5种通用字体
    Serif 字体
    Sans-serif 字体
    Monospace 字体
    Cursive 字体
    Fantasy 字体
*/

/* font-family 指定字体样式 */
font-family: sans-serif;

/* 如果系统中没有Georgia字体则使用serif字体 */
font-family: Georgia, serif;

/* 当属性值出现空格的时候，需要加引号 */
p {font-family: Times, TimesNR, 'New Century Schoolbook'}

/* font-style字体风格
	normal - 文本正常显示
    italic - 文本斜体显示
    oblique - 文本倾斜显示
*/
font-style:normal

/* font-weight字体加粗，400等价于normal，bold=700，900为最大 */
font-weight:normal;
font-weight:bold;
font-weight:900

/* font-size字体大小 */
h1 {font-size:60px;}
h2 {font-size:40px;}

/* em单位设置字体，浏览器中默认的文本大小是 16 像素。因此 1em 的默认尺寸是 16 像素。 */
h1 {font-size:3.75em;} /* 60px/16=3.75em */
h2 {font-size:2.5em;}  /* 40px/16=2.5em */
p {font-size:0.875em;} /* 14px/16=0.875em */
```

##### 6.链接

```css
/* 链接的4种状态的规则：
	a:hover 必须位于 a:link 和 a:visited 之后
	a:active 必须位于 a:hover 之后
*/
a:link {color:#FF0000;}		/* 未被访问的链接 */
a:visited {color:#00FF00;}	/* 已被访问的链接 */
a:hover {color:#FF00FF;}	/* 鼠标指针移动到链接上 */
a:active {color:#0000FF;}	/* 正在被点击的链接 */

/* text-decoration用来去掉链接中的下划线 */
a:link {text-decoration:none;}
a:visited {text-decoration:none;}
a:hover {text-decoration:underline;}
a:active {text-decoration:underline;}

/* background-color用来设置链接的背景色 */
a:link {background-color:#B2FF99;}
a:visited {background-color:#FFFF85;}
a:hover {background-color:#FF704D;}
a:active {background-color:#FF704D;}
```

##### 7.css列表

```css
/* list-style-type设置列表样式 */

/* 将无需列表中的列表项标志设置为方块 */
ul {list-style-type : square}

/* 常规的标志不够可以使用自定义图片 */
ul li {list-style-image : url(xxx.gif)}
```

##### 8.css边框

```css
/* 为 table、th 以及 td 设置了蓝色边框 */
table, th, td
  {
  border: 1px solid blue;
  }

/* 将图片设置为单一边框 */
table{
  border-collapse:collapse;
  }

table,th, td{
  border: 1px solid black;
  }

/* 表格文本对齐 */
td{
  text-align:right;
  }

/* vertical-align设置垂直对齐方式,buttom底部，top顶部，center垂直居中 */
td{
  height:50px;
  vertical-align:bottom;
  }

/* 表格内边距，如需控制表格中内容与边框的距离，请为 td 和 th 元素设置 padding 属性： */
td{ padding:15px; }

/* 表格颜色：下面的例子设置边框的颜色，以及 th 元素的文本和背景颜色： */
table, td, th{
  border:1px solid green;
  }

th{
  background-color:green;
  color:white;
  }
```

##### 9.CSS盒子模型

```css 
element(height,width) - padding(内边距)   -border(边框)   -margin(外边距)

/* margin也通常称为白边 */

/* 内边距和外边距默认为0 */
* {
  margin: 0;
  padding: 0;
}
```

##### 10.内边距

```css
/* 上下左右的内边距都是10px */
padding: 10px;

/* 分别设置上下左右的内边距 */
h1 {padding: 10px 0.25em 2ex 20%;}

/* 等同于上面的 */
h1 {
  padding-top: 10px;
  padding-right: 0.25em;
  padding-bottom: 2ex;
  padding-left: 20%;
  }

/* 代表内边距设置为父元素的width的10% */
p {padding: 10%;}
```

##### 11.边框

```css
每个边框有 3 个方面：宽度、样式，以及颜色。边框绘制在“元素的背景之上”。
/* border-style边框的样式 */
border-style: solid

/*定义多个样式，则是按照上右下左的方式进行选择*/
border-style: solid dotted dashed double;

/* border-width: 5px;边框的宽度 */
border-width: 5px;

/* 定义单条边框的宽度，上右下左的方式 */
border-width: 15px 5px 15px 5px;

/* 设置没有边框，border-style设置为none,boder-width不生效 */
p {border-style: none; border-width: 50px;}

/* border-style 的默认值是 none,所以声明边框，必须要设置样式*/

/* 设置边框颜色 */
 border-color: blue red;
```

##### 12.margin外边距

```css
/* margin 属性接受任何长度单位，可以是像素、英寸、毫米或 em。*/
h1 {margin : 10px 0px 15px 5px;}

/* 也可以设置为百分比 */
p {margin : 10%;}

/* 设置为单个边距 */
p {margin-left: 20px;}

/* 设置单个边距 */
p {margin: auto auto auto 20px;}
```

##### 13.底部tabbar

```css
<html>
    <head>
        <style>
            body{
                padding: 0;
                margin: 0;
            }
            .outer{
                
                background-color: blueviolet;
                display: flex;
                position: fixed;
                bottom: 0;
                left: 0;
                right: 0;
            }

            .item{
                flex: 1;
                text-align: center;
                height: 50px;
                line-height: 50px;
            }
        </style>
    </head>
    <body>
        <div class="outer">
            <div class="item">nice</div>
            <div class="item">nice</div>
            <div class="item">nice</div>
        </div>
    </body>
</html>
```

###### 轮播图 

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    .box{
      width: 1000px;
      height: 460px;
      position: relative;
      margin: auto;
      top: 20px;
    }

    .box-img{
      position: absolute;
      top: 0;
      left: 0;
      opacity: 0;
      /* 轮播图切换时候的 */
      transition: opacity linear 1s;
    }

    .box .box-img:nth-child(1){
      opacity: 1;
    }

    .box-left{
      position: absolute;
      width: 35px;
      height: 70px;
      background-color: #ccc;
      color: #333;
      opacity: .5;
      top: 195px;
      border-radius: 0 5px 5px 0;
      text-align: center;
      line-height: 70px;
      font-size: 27px;
    }

    .box-left:hover{
      background-color: #000050;
      color: #fff;
      cursor: default;
    }

    .box-right{
      position: absolute;
      width: 35px;
      height: 70px;
      background-color: #ccc;
      color: #333;
      opacity: .5;
      top: 195px;
      right: 0;
      /* 左上,右上,右下,左下 */
      border-radius: 5px 0 0 5px;

      text-align: center;
      line-height: 70px;
      font-size: 27px;
    }

    .box-right:hover{
      background-color: #000050;
      color: #fff;
      cursor: default;
    }

    .box-zhiding{
      position: absolute;
      right: 30px;
      bottom: 0;
    }

    .box-zhiding>ul{
      padding: 0;
      margin: 0;
      list-style: none;
    }

    .box-zhiding>ul>li{
      float: left;
      margin-left: 3px;
      width: 14px;
      height: 14px;
      border-radius: 100%;
      background-color: #ccc;
    }

    .box-zhiding>ul>li:hover{
      opacity: .5;
    }
  </style>

</head>
<body>
  <div class="box">
    <div class="box-img"><img src="./imgs/1.jpg"/ width="1000" height="540"></div>
    <div class="box-img"><img src="./imgs/2.jpg"/ width="1000" height="540"></div>
    <div class="box-img"><img src="./imgs/3.jpg"/ width="1000" height="540"></div>
    <div class="box-img"><img src="./imgs/7.jpg"/ width="1000" height="540"></div>
    <div class="box-img"><img src="./imgs/5.jpg"/ width="1000" height="540"></div>
    <div class="box-img"><img src="./imgs/6.jpg"/ width="1000" height="540"></div>
    <div class="box-left">&lt;</div>
    <div class="box-right">&gt;</div>
    <div class="box-zhiding">
      <ul>
        <li class="botton"></li>
        <li class="botton"></li>
        <li class="botton"></li>
        <li class="botton"></li>
        <li class="botton"></li>
        <li class="botton"></li>
      </ul>
    </div>
  </div>
</body>

<script>
  function lunbo(){
    var index = 0
    // 1.获取到所有的图片
    let imgs = document.getElementsByClassName('box-img')
    
    // 2.设置定时器,每2秒切换一次图片
    var inter = dingshiqi() 
    function dingshiqi(){
      return setInterval(function(){
        if(index === 5){
          index = 0
        }
        for(let i=0; i<imgs.length; i++){
        imgs[i].style.opacity = '0'
        }

        imgs[index].style.opacity = '1'
        index ++
      },2000)
    }

    // 3.点击切换图片功能
    // 先获取到元素,再增加一个监听click事件
    let left = document.getElementsByClassName('box-left')[0]
    left.addEventListener("click", function(){
      clearInterval(inter)
      console.log('click....')
      if(index === 0){
        index = imgs.length-1
        for(let i=0; i<imgs.length; i++){
          imgs[i].style.opacity = '0'
        }

        imgs[index].style.opacity = '1'
      }else{
        index--
        for(let i=0; i<imgs.length; i++){
          imgs[i].style.opacity = '0'
        }

        imgs[index].style.opacity = '1'
      }
      // 重启定时器
      inter = dingshiqi()
      
     
    });

    // 4.轮播图的右切换
    let right = document.getElementsByClassName('box-right')[0]
    right.addEventListener("click", function(){
      // 当点击事件触发的时候,清除原来的定时器事件
      clearInterval(inter)
      console.log('click....')
      if(index === imgs.length-1){
        index = 0
        for(let i=0; i<imgs.length; i++){
          imgs[i].style.opacity = '0'
        }

        imgs[index].style.opacity = '1'
      }else{
        index++
        for(let i=0; i<imgs.length; i++){
          imgs[i].style.opacity = '0'
        }

        imgs[index].style.opacity = '1'
      }
      // 重启定时器
      inter = dingshiqi()
    });

    // 5.小圆点点击切换图片
    let bottons = document.getElementsByClassName('botton')
    // 给每个按钮绑定事件
    for(let i=0; i<bottons.length; i++){
      bottons[i].addEventListener('click', function(){
        // 关闭定时器,否则可能会变乱
        clearInterval(inter)
        // 将图片的索引赋值为当前小圆点的索引
        index = i
        for(let i=0; i<imgs.length; i++){
          // 图片和小圆点先隐藏
          imgs[i].style.opacity = '0'
          bottons[i].style.opacity = '1'
        }
        // 让对应图片显示
        imgs[index].style.opacity = '1'
        // 让对应索引的小圆点变透明
        bottons[index].style.opacity = '.5'
        // 切换到小圆点对应的图片之后,重启定时器
        inter = dingshiqi()        
      })
    }
  }
  lunbo()
</script>

<style>
</style>

</html>
```

