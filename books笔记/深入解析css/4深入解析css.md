### 过渡

###### 1.基础

```css
元素可以淡出、菜单可以滑入、颜色可以从一种变为另一种，实现这些效果最简单的方式是过渡（transitions）。过渡可以增强页面的交互效果.

元素属性任何时候发生变化都会触发过渡：可以是状态改变的时候，比如:hover；也可以是 JavaScript 导致变化的时候，比如添加或者移除类影响了元素的样式。
```

###### 2.按钮过渡效果

```css
button{
    background-color: hsl(180, 50%, 50%);   /* 蓝绿色按钮 */
    border: 0;
    outline: none;
    color: white;
    font-size: 1rem;
    padding: .3em .3em;
    transition-property: all;       /* 所有属性都使用过渡动画 */
    transition-duration: 0.5s;      /* 过渡时间为0.5s */
}

/* 悬停状态为带圆角的红色按钮 */
button:hover{
    background-color: hsl(0, 50%, 50%);
    border-radius: 1em;
}
```

###### 3transition属性

```css
/*
transition为简写属性，接受4个参数。
arg1: 生效的属性
arg2: 持续时间
arg3: 定时函数
arg4: 延迟时间（延迟触发过渡，0.5s后开始过渡）
*/
transition: background-color 0.3s linear 0.5s

/* 如果需要为两个不同的属性分别设置不同的过渡，可以添加多个过渡规则，以逗号分隔 */
transition: border-radius 0.3s linear, background-color 0.6s ease;
```

###### 4定时函数

```css
定时函数是过渡中非常重要的一部分。过渡让某个属性从一个值“移动”到另一个值，定时函数用来说明如何移动。
定时函数是基于数学定义的贝塞尔曲线（Bézier curve）
linear: 值以固定的速度改变
ease-in: 变化速度开始变慢，然后一直加速，直到完成
ease-out: 减速，开始时快速变化，结束时比较慢
```

###### 5.自定义定时函数

```css
/* cubic-bezier的值为浏览器检查，自定义效果粘贴出来的定时函数 */
/* 四个参数分别代表两个控制柄的控制点x和y坐标 */
transition: all 2s cubic-bezier(0, 1.07, 0.88, -0.13);
```

###### 6.阶跃

```css
最后再介绍一种定时函数，即 steps()函数。跟前面介绍的从一个值到另一个值的基于贝塞尔曲线的流畅过渡不同，这个函数是一系列非连续性的瞬时“阶跃”（steps）。

阶跃函数需要两个参数：阶跃次数和一个用来表示每次变化发生在阶跃的开始还是结束的关键词（start 或者 end）。第二个参数默认是end.

/* 三次非连续性的阶跃过渡 */
transition: all 1s steps(3);

/* arg2:默认情况下，属性值在每一步结束的时候改变，因此过渡不会立即开始。添加start 关键字 steps(3, start)就可以改变这种行为，这样过渡就会发生在每步开始的时候，而不是结束。 */
```

###### 7.display没有动画效果

```css
不是所有属性都可以添加动画效果，display 属性就是其中之一。你可以在 display: none和 display: block 之间切换，但不能在这两个值之间过渡，因此，任何应用到 display 上的过渡属性都会被忽略。
```

###### 8.使用opacatity和visibility代替display没有动画效果

```css
/*
我们在这里为过渡设置了两组值，定义了淡出行为。第一组值为 opacity 设置 0.2s 的过渡，第二组值为 visibility 设置 0s 的过渡（立即执行），但有 0.2s 的延迟。这就意味着先执行 opacity的过渡，结束之后再执行 visibility 的过渡。这样就实现了菜单的缓慢淡出，当完全透明的时候，可见性切换为 hidden。
*/ 
.dropdown__drawer{
      position: absolute;
      /* display: none; */
      background-color: white;
      width: 10em;
      opacity: 0;
      visibility: hidden;
      transition: opacity 0.2s linear, visibility 0s linear 0.2s;
    }

    .dropdown.is-open .dropdown__drawer{
      /* display: block; */
      opacity: 1;
      visibility: visible;
      transition-delay: 0s;   /* 添加is-open类时移除过渡延迟 */
    }
```

###### 9.js实现高度的动画效果

```html
.dropdown__drawer{
    position: absolute;
    background-color: white;
    width: 10em;
    height: 0;
    visibility: hidden;
    transition: opacity 0.2s linear, visibility 0s linear 0.2s;
}

.dropdown.is-open .dropdown__drawer{
    height: auto;
}

<script>
  (function(){
    var toggle = document.getElementsByClassName('dropdown__toggle')[0]
    console.log(toggle)
    var dropdown = toggle.parentElement
    var drawer = document.getElementsByClassName('dropdown__drawer')[0]
    var height = drawer.scrollHeight      // 获取计算得到的抽屉自动高度值
    toggle.addEventListener('click', function(e){
      e.preventDefault()
      dropdown.classList.toggle('is-open')
      if(dropdown.classList.contains('is-open')){
        // 打开的时候精确设置高度值
        dropdown.style.setProperty('height', height + 'px')
      }else{
        // 关闭的时候把高度值重置为0
        dropdown.style.setProperty('height', 0)
      }
    })
  })()
</script>
```

###### 10获取到元素的高度和判断一个元素是否有某个类

```javascript
var element = document.getElementById('node')

// 添加一个类
element.classList.add('red')

// 移除一个类
element.classList.remove('red')

// 切换一个类
element.classList.toggle('red')

// 判断是否有某个类
element.classList.contains('red')
```

### 变换

-  变换通常结合过渡或动画一起使用 

###### 1.变换的几种函数

```css
使用变换的时候要注意一件事情，虽然元素可能会被移动到页面上的新位置，但它不会脱离文档流。你可以在屏幕范围内以各种方式平移元素，其初始位置不会被其他元素占用。

 旋转（Rotate）——元素绕着一个轴心转动一定角度。
 平移（Translate）——元素向上、下、左、右各个方向移动（有点类似于相对定位）。
 缩放（Scale）——缩小或放大元素。
 倾斜（Skew）——使元素变形，顶边滑向一个方向，底边滑向相反的方向。

skew(20deg)——使卡片倾斜 20 度。试试负角度，让卡片向其他方向倾斜。
scale(0.5)——将卡片缩小至初始大小的一半。scale()函数需要一个无单位的数值，小于 1 表示要缩小元素，大于 1 表示要放大元素。
translate(20px, 40px)——使元素向右移动 20px，向下移动 40px。同样，也可以使用负值使元素向相反的方向变换。
```

###### 2.rotate

```css
body{
    background-color: hsl(210, 80%, 20%);
    font-family: Arial, Helvetica, sans-serif;
}
img{
	/* img的max-width以后可以用在项目中 */
    max-width: 100%;
}
.card{
    padding: 0.5em;
    margin: 0 auto;
    background-color: white;
    max-width: 300px;
    transform: rotate(15deg);
}

变换不能作用在<span>或者<a>这样的行内元素上。若确实要变换此类元素，要么改变元素的 display 属性，替换掉 inline（比如 inline-block），要么把元素改为弹性子元素或者网格项目（为父元素应用 display: flex 或者 display: grid）。
```

###### 3.更改变换基点

```css
/*
变换是围绕基点（point of origin）发生的。基点是旋转的轴心，也是缩放或者倾斜开始的地方。这就意味着元素的基点是固定在某个位置上，元素的剩余部分围绕基点变换（但 translate()是个例外，因为平移过程中元素整体移动）。

默认情况下，基点就是元素的中心，但可以通过 transform-origin 属性改变基点位置。
*/

基点也可以指定为百分比，从元素左上角开始测量。下面的两句声明是等价的。
transform-origin: right center;
transform-origin: 100% 50%;
```

###### 4.多重变换

```css
/*可以对 transform 属性指定多个值，用空格隔开。变换的每个值从右向左按顺序执行，比如我们设置 transform: rotate(15deg) translate(15px, 0)，元素会先向右平移 15px，然后顺时针旋转 15 度。*/

注意修改 translate()的值的时候，元素好像是沿着一个倾斜的坐标轴在移动，而不是正常的方向。这是因为旋转发生在平移之后。

transform: rotate(15deg) scale(0.9) translate(0, 40px);
```

###### 5.背景渐变

```css
/* background-color为背景渐变的备用值，如果浏览器不支持渐变，则会显示这个背景色 */ 
background-color: hsl(200, 80%, 30%);
background-image: radial-gradient(hsl(200, 80%, 30%), hsl(210, 80%, 20%));
```

###### 6.小图标对齐文字

```css
.nav-links__icon{
  height: 1.5em;
  width: 1.5em;
  vertical-align: -0.3em;     /* 将图片稍微向下移动，与文字居中 */
}
```

###### 7.形变和过渡结合

```css
/* 对形变增加过渡效果 */
.nav-likns__icon{
    transition: transform 0.2s ease-out;
}

/* 鼠标悬浮式和激活时，元素开始形变 */
.nav-links a:hover > .nav-links__icon,
.nav-links a:focus > .nav-links__icon{
    transform: scale(1.3);
}
```

###### 8.最终代码

```css
@media(min-width: 30em){
  .nav-links{
    display: block;
    padding: 1em;
    margin-bottom: 0;
  }

  .nav-links > li + li {
    margin-left: 0;
  }
  .nav-links__label{
    display: inline-block;  /* 把标签设置为行内块级元素，这样就可以对其使用变换效果了 */
    margin-left: 1em;
    padding-right: 1em;
    opacity: 0;             /* 开始时隐藏标签 */
    transform: translate(-1em);
      /* 这个cubic-bezier()函数非常漂亮 */
    transition: transform 0.4s cubic-bezier(0.2, 0.9, 0.3, 1.3),
                opacity 0.4s linear;
  }

  /* 鼠标悬停或者激活状态下，设置标签可见，并把它移动会正确的位置 */
  .nav-links:hover .nav-links__label,
  .nav-links a:focus > .nav-links__label{
    opacity: 1;
    transform: translate(0);
  }
    
  /* 根据每个菜单项在列表中的位置，选中他们，然后分别为每个元素设置连续变长的过渡延迟 */
  .nav-links > li:nth-child(2) .nav-links__label{
    transition-delay: 0.1s;
  }
  .nav-links > li:nth-child(3) .nav-links__label{
    transition-delay: 0.2s;
  }
  .nav-links > li:nth-child(4) .nav-links__label{
    transition-delay: 0.3s;
  }
  .nav-links > li:nth-child(5) .nav-links__label{
    transition-delay: 0.4s;
  }

  .nav-likns__icon{
    transition: transform 0.2s ease-out;
  }
  .nav-links a:hover > .nav-links__icon,
  .nav-links a:focus > .nav-links__icon{
    transform: scale(1.3);
  }
}
```

###### 9.动画尽可能使用变换（对变换添加动画效果）

```css
如果我们要实现过渡或动画，无论什么类型，包括定位或大小操作，都应该尽可能考虑使用变换。因为变换的性能比较高。

处理过渡或者动画的时候，尽量只改变 transform 和 opacity 属性。如果有需要，可以修改那些只导致重绘而不会重新布局的属性。只有在没有其他替代方案的时候，再去修改那些影响布局的属性，并且密切关注动画中是否存在性能问题。
```

###### 10.3D变换

```css
/*旋转和平移都可以在三个维度上实现：X 轴、Y 轴和 Z 轴。z轴相当于元素更加原理用户*/
transform: translate(15px, 50px);
transform: translateX(15px) translateY(50px);

/* x, y, z轴 */
transform: translate3d(30px, 30px, 30px);
```

###### 11.rotate

```js
rotate()就可以被称为 rotateZ()，它就是绕着 Z 轴旋转的。函数 rotateX()和 rotateY()分别围绕着水平方向上的 X 轴（使元素向前或者向后倾斜）和垂直方向上的 Y 轴（使元素向左或者向右转动或偏移）旋转。
```

###### 12.控制透视距离

```js
为页面添加 3D 变换之前，我们需要先确定一件事情，即透视距离（perspective）。变换后的元素一起构成了一个 3D 场景。接着浏览器会计算这个 3D 场景的 2D 图像，并渲染到屏幕上。我们可以把透视距离想象成“摄像机”和场景之间的距离，前后移动镜头就会改变整个场景最终显示到图像上的方式。

如果镜头比较近（即透视距离小），那么 3D 效果就会比较强。如果镜头比较远（即透视距离大），那么 3D 效果就会比较弱。

 /* x, y, z轴 */
/* perspective设置透视距离，如果没有透视距离或透视距离太小是比较难看出3d变换的效果 */
transform: perspective(100px) translate3D(30px, 30px, -30px);
```

###### 13.透视距离

```css
 <div class="row">
    <div class="box">One</div>
    <div class="box">Two</div>
    <div class="box">Three</div>
    <div class="box">Four</div>
  </div>

.row{
    display: flex;
    justify-content: center;
}

.box{
    box-sizing: border-box;
    width: 150px;
    margin: 0 2em;
    padding: 60px 0;
    text-align: center;
    background-color: hsl(150, 50%, 40%);
    /* 添加透视距离 */
    transform: perspective(200px) rotateX(30deg);
}
```

###### 14.建立统一的透视距离(父元素中添加)

```css
/*通过为父容器（或其他祖先元素）设置统一的透视距离，父容器包含的所有应用了 3D 变换的子元素，都将共享相同的透视距离。*/ 
<div class="row">
    <div class="box">One</div>
    <div class="box">Two</div>
    <div class="box">Three</div>
    <div class="box">Four</div>
  </div>

.row{
    display: flex;
    justify-content: center;
    /* 建立统一的透视距离在父元素中添加perspective属性 */
    perspective: 200px;
}

.box{
    box-sizing: border-box;
    width: 150px;
    margin: 0 2em;
    padding: 60px 0;
    text-align: center;
    background-color: hsl(150, 50%, 40%);
    transform: rotateX(30deg);
}
```

###### 15.高级3D变换

```css
perspective: 200px;
/* 把镜头位置移动到元素的左下方 */
perspective-origin: left bottom;
```

###### 16.元素翻转

```html
 <style>
    .box{
      width: 800px;
      height: 500px;
      margin: 0 auto;
      background-image: url(bear.jpg);
      background-repeat: no-repeat;
      background-size: cover;
      
      /* Y轴旋转180°，会看到元素反转过来了 */
      transform: rotateY(180deg);
    }

    .box h2{
      text-align: center;
      border-top: 5px solid tomato;
      border-bottom: 5px solid tomato;
    }
  </style>
</head>
<body>
  <div class="box">
    <h2>Hello Word</h2>
  </div>
</body>
```

### 动画

###### 1.关键帧动画

```css
过渡是直接从一个地方变换到另一个地方，相比之下，我们可能希望某个元素的变化过程是迂回的路径。有时，我们可能需要元素在动画运动后再回到起始的地方。这些事情无法使用过渡来实现。为了对页面变化有更加精确的控制，CSS 提供了关键帧动画。

关键帧（keyframe）是指动画过程中某个特定时刻。我们定义一些关键帧，浏览器负责填充或者插入这些关键帧之间的帧图像
```

###### 2css中的动画

```css
CSS 中的动画包括两部分：用来定义动画的@keyframes 规则和为元素添加动画的 animation属性。
```

###### 3.关键帧动画例子

```css
/* 为动画命名为over-and-back */
@keyframes over-and-back {
    /* 第一个关键帧声明 */
    0% {
        background-color: hsl(0, 50%, 50%);
        transform: translate(0);
    }
    /* 第二个关键帧发生于动画进行到一半时 */
    50% {
        transform: translate(50px); 
    }
    /* 最后一个关键帧 */
    100% {
        background-color: hsl(270, 50%, 90%);
        transform: translate(0);
    }
}

/* 为box应用动画 */
.box{
    width: 100px;
    height: 100px;
    background-color: green;
    /* 最后一个参数是动画持续的次数 */
    animation: over-and-back 1.5s linear 3;
}
```

###### 4.animation属性

```css
/*
arg1: 动画名称，@keyframes规则定义的动画名称
arg2: 动画持续时间
arg3: 定时函数
arg4: 代表动画重复的次数，初始值默认是1
*/
animation: over-and-back 1.5s linear 3;

如果出现样式层叠，那么动画中设置的规则比其他声明拥有更高的优先级。
```

###### 5.动画前缀

```css
浏览器对动画的支持情况比较好，仅有小部分移动浏览器需要使用-webkit-前缀，动画属性（-webkit-animation）和关键帧@规则（@-webkit-keyframes）都要用到。
```

###### 6.flex允许折行，允许flex-grow,并设置最大宽度

```css
/* 响应式布局的断点 */
@media (min-width: 30em){
  .flyin-grid{
    /* 建立允许折行的弹性容器 */
    display: flex;
    flex-wrap: wrap;
    margin: 0 5rem;
  }

  .flyin-grid__item{
    /* 允许flex-grow,设置flex-basis为3000px */
    flex: 1 1 300px;
    margin-left: 0.5em;
    margin-right: 0.5em;
    max-width: 600px;
  }
}
```

###### 7.grid布局优先，不支持回退flex布局

```css
/* 响应式布局的断点 */
@media (min-width: 30em){
  .flyin-grid{
    /* 建立允许折行的弹性容器 */
    display: flex;
    flex-wrap: wrap;
    margin: 0 5rem;
  }

  .flyin-grid__item{
    /* 允许flex-grow,设置flex-basis为3000px */
    flex: 1 1 300px;
    margin-left: 0.5em;
    margin-right: 0.5em;
    max-width: 600px;
  }

  /* 在媒体查询快内部检测网格布局的支持情况,如果支持则覆盖flex的布局 */
  @supports(display: grid){
    .flyin-grid{
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(300px 1fr));
      grid-gap: 2em;
    }

    .flyin-grid__item{
      max-width: initial;
      /* 移除回退布局中设置的外边距 */
      margin: 0;            
    }
  }
```

###### 8.为卡片添加动画

```css
/* 添加飞入动画 */
@keyframes fly-in{
  /* 以旋转后的状态从远处开始 */
  0%{
    transform: translateZ(-800px) rotateY(90deg);
    opacity: 0;
  }
  /* 已经很近了，但几乎还是旋转状态 */
  60%{
    transform: translateZ(-160px) rotateY(87deg);
  }
  /* 在正常位置结束 */
  100%{
    transform: translateZ(0) rotateY(0);
  }
}

/* 在容器上设置共享的透视距离 */
.flyin-grid{
  margin: 0 1rem;
  perspective: 500px;
}

/* 为每个元素添加动画 */
.flyin-grid__item{
  animation: fly-in 600ms ease-in;
}
```

###### 9.动画延迟和填充模式

```css
/*可以使用animation-delay 属性推迟动画开始的时间，该属性行为和transition-delay类似。利用这个属性，我们可以设置动画交错发生*/

/* 在容器上设置共享的透视距离 */
.flyin-grid{
  margin: 0 1rem;
  perspective: 500px;
}

/* 为每个元素添加动画 */
.flyin-grid__item{
  animation: fly-in 600ms ease-in;
}

/* 把每个元素的动画开始时间设置 */
.flyin-grid__item:nth-child(2){
  animation-delay: 0.15s;
}

.flyin-grid__item:nth-child(3){
  animation-delay: 0.3s;
}

.flyin-grid__item:nth-child(4){
  animation-delay: 0.45s;
}
```

###### 10.使用动画向后填充模式

```css
/*在浏览器中加载页面，你可能已经发现了问题。动画确实是在预期的时间播放的，但有些元素提前展示在了页面上，一小段时间后它们消失然后播放动画。这有点不合理，不是我们想要的效果。我们希望所有的元素在开始的时候都不可见，只有在各自的动画执行的时候才会出现。*/

/* 添加动画向后填充模式解决 */
.flyin-grid__item{
  animation: fly-in 600ms ease-in;
  /* 动画向后填充模式:动画开始之前应用第一帧上的动画样式 */
  animation-fill-mode: backwards;
}
```

###### 11.animation-fill-mode可选值

```css
none: 初始值是，意思是动画执行前或执行后动画样式都不会应用到元素上。
backwards: 在动画执行之前，浏览器就会取出动画中第一帧的值，并把它们应用在元素上
forwards: 会在动画播放完成后仍然应用最后一帧的值
both: 会同时向前和向后填充。
```

###### 12.max-width有多重用途

```css
响应式的网页可以设置在图片或者表单中
/* 限制表单最大宽度 */
form{
	max-width: 500px;
}
```

###### 13.按钮正在加载动画

```css
 /* 按钮显示正在加载的图标 */
    button.is-loading{
      position: relative;
      /* 隐藏文字 */
      color: transparent;
    }

    button.is-loading::after{
      position: absolute;
      content: '';
      display: block;
      width: 1.4em;
      height: 1.4em;
      top: 50%;
      left: 50%;
      margin-left: -0.7em;
      margin-top: - 0.7em;
      border-top: 2px solid white;
      border-radius: 50%;
      /* infinite代表重复循环动画 */
      animation: spin 0.5s linear infinite;
    }

    @keyframes spin{
      0%{
        transform: rotate(0deg);
      }
      100%{
        /* 设置每次循环都旋转一周 */
        transform: rotate(360deg);
      }
    }
```

###### 14.按钮在用户1s未输入文字，摇晃提示用户提交的动画

```css
 /* 按钮在用户输入文字过多，摇晃提示用户提交的动画.需要添加js判断输入框文字的多少，动态的加.shake类 */
.shake{
    animation: shake 0.7s linear;
}

@keyframes shake{
    /* 动画执行期间，在多个位置使用相同的关键帧定义 */
    0%,
    100%{
        transform: translateX(0);
    }

    /* 向左移动元素 */
    10%,
    30%,
    50%,
    70%{
        transform: translateX(-0.4em);
    }
    /* 向右移动 */
    20%,
    40%,
    60%{
        transform: translateX(0.4em);
    }
    /* 最后一次晃动的时候降低移动幅度 */
    80%{
        transform: translateX(0.3em);
    }
    90%{
        transform: translateX(-0.3em);
    }
}
```

###### 15.以上摇晃的js代码

```javascript
  // 1.之前点击按钮触发loding的代码
  var input = document.getElementById('trip')
  var button = document.getElementById('submit-button')

  button.addEventListener('click', function(e){
    e.preventDefault()
    button.classList.add('is-loading')
    button.disabled = true
    input.disabled = true
  })

  // 2.定义一个变量指向超时函数
  var timeout = null
  input.addEventListener('keyup', function(){
    clearTimeout(timeout)
    // 1s后添加shake类
    timeout = setTimeout(function(){
      button.classList.add('shake')
    }, 1000)
  })

  // 动画结束后提出shake类
  button.addEventListener('animationend', function(){
    button.classList.remove('shake')
  })
```



