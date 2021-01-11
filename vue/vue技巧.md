```
ul>li{nice}*100			ul里面生成100个li,li里面的文本为nice

ul>li{nice$}*100		$ 为变量 nice1 - nice100

js的apply()方法!!!
```

```css
/* 滚动到的时候，悬浮并固定 */ 
.tab-control{
    position: sticky;
    top: 44px;
  }
```

```js
const ls1 = [1,2,3,4,5]

ls2 = []

// 这样push,相当于将整个ls1当作一个元素push进去
ls2.push(ls1)

// 这种方式类似解构，将ls1中的元素一个个取出来，push到ls2中
ls2.push(...ls1)
```

##### better实现滚动

- 让移动端的滚动更加流畅的框架

```html
<!--1.安装-->
npm i better-scroll -S

<!--2.在需要使用滚动的vue组件中的script中导入-->
import BScroll from 'better-scroll'

// 3.在moundted生命周期中，new Bscroll
<template>
  <!-- 最外层被Bscroll获取的元素 -->
  <div class="wapper" ref="scroll">
      <!-- 里面也必须只有一个元素，这一个元素里可以包含任意多的元素 -->
      <ul class="content">
        // 实际上这里有100个li
        <li>这里有100个li</li>
      </ul>
  </div>
</template>

<script>
  import BScroll from 'better-scroll'
  export default {
    name: "Category",
    data(){
      return{
        // scroll用来保存创建的BScroll对象
        scroll: null
      }
    },
    // mounted方法中创建BScroll,并且获取到根容器
    // mounted方法中创建BScroll,并且获取到根容器
    // 开启监听滚动需要加上probeType,3代表一直监听
    // 开启监听页面滚动到底部 pullUpLoad: true
    mounted() {
      this.scroll = new BScroll(this.$refs.scroll,{
        probeType: 3,
        pullUpLoad: true
      })

      // 然后this.scroll.on()可以监听事件，scroll代表监听滚动事件
      this.scroll.on('scroll',(position) => {
        console.log(position)
      })

      // 监听页面滚动到底部
      this.scroll.on('pullingUp',()=>{
        console.log("上拉加载更多");
      })
    }
  }
</script>

<style scoped>
  /* 给wapper设置一个高度 */
.wapper{
  height: 200px;
  background: pink;
  overflow: hidden;
}
</style>


```

##### css实现局部滚动

```css
.content{
    height: 150px;
    background-color: pink;
    overflow: hidden;
    overflow-y: scroll;
}
```

```css
/* fixed布局，让一个图片固定在一个地方，固定在距离右边10px,距离下边50px的位置 */
<style scoped>
  .back-top{
    position: fixed;
    right: 10px;
    bottom: 50px;
  }
</style>
```

##### 组件是不能监听点击的，如果想要监听，加上native

```html
<back-top @click.native="backClick"></back-top>
```

##### 子组件中定义的props如果是驼峰，那么父组件传值的时候就用-分割

```js
// 子组件probeType 
props: {
      probeType:{
        type: Number,
        default: 0
      }
    },
        
// 父组件 - 分割
:probe-type="3"
```

##### 传值的时候尽量都用 :

```js
// 如果不加:则传递的是字符串3，加了冒号则传递的是数字3
:nice = "3"
```

##### $emit

```js
// $emit不是事件传递，传递给父组件。而是自定义事件，我们可以在任意代码执行到的位置自定义事件传递给组件
$emit('nice',index)

// 然后父组件就可以接收这个事件
@scroll = "contentScroll"
```

##### 编程技巧

```js
// js中这种语法可以写成
if (2>3):
	a = true

a = 2>3

```

``` html
// vue监听图片加载
<img :src="goodsItem.show.img" alt="" @load="imageLoad">
```

##### vue事件总线

```js
// 需要在main.js中创建事件总线，直接new Vue()给原型即可
Vue.prototype.$bus = new Vue()

// 事件总线发送事件(可以在任意一个vue中发送,可以传参)
this.$bus.$emit('itemImageLoad',arg1)

// 监听事件总线中的事件。item中图片加载完成（可以在任意一个vue中监听）
this.$bus.$on('itemImageLoad',(arg1)=>{
    console.log('----');
})
```

##### 判断前面的不为null再执行后面的用 &&

```js
this.srcoll && this.scroll.scrollTo && this.srcoll.scrollTo(x,y,time)

// 判断现有scroll的值，再执行scroll中的refresh()方法
this.scroll && this.scroll.refresh()
```

##### 防抖函数

```js

// arg1:需要防抖的函数， arg2:需要等多久
debounce(func, delay){
    let timer = null;
    return function (...args) {
        if(timer)  clearTimeout(timer)
        timer = setTimeout(()=>{
            func.apply(this,args)
        }, delay)
    }
}

// 将函数传递给防抖函数，记住，函数不要加括号
const refresh = this.debounce(this.$refs.scroll.refresh,500)
// 因为返回值是一个函数，所以再调用
refresh()
```

##### 函数中的 ...

```js
// ...代表能传多个参数，而没有...则只能传递一个参数
function(...args){}				
```

##### js中先执行同步代码，再执行异步代码

```js
// 打印顺序为 1 3 2， 即使setTimeout没有设置事件。
// 因为js默认先执行同步代码，setTimeout会被放到最后，这叫事件循环 event loop
console.log('1')

setTimeout(()=>{
    console.log('2')
})

console.log('3')
```

##### $el 拿到组件的根元素

```js
// 所有的组件中都有一个$el元素，用于获取组件的中的元素，获取到的是组件中的根元素
this.$refs.tabControl.$el
```

##### 1.封装从接口中拿到的数据，可以使用类（ES6中类的使用）

```js
// 1.定义类
export class Person{
    constructor(item,columns){
        this.name = item.name
        this.height = item.height
        this.key = columns.key
        this.value = columns.value
    }
}

// 2.拿到数据进行封装
import {Person} from '../../detail'
data = res.data
personData = Person(data.item, data.columns)
```

##### 1.v-for遍历数字

```html
<!-- item为0-9 -->
<div v-for="item in 10">{{item}}</div>
```

##### javascript判断对象有没有内容

```js
// 创建一个空对象
cosnt p = {}

// 通过Object.keys获取到对象中所有的key, 如果它的length为0，就代表里面没有key，是一个空对象
Object.keys(p).length === 0 
```

##### props中的属性，如果默认值是数组或者对象，则要写成函数

```js
// props中的数据，默认值对象，则写成函数
props: {
	goods: {
		type: Object,
		default(){
			return {}
		}
	}
}
```

