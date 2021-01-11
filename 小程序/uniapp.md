###### 1.创建项目(脚手架)

- vue项目名名称不能含有大写字母
- 微信小程序分为三种对象 :
  - 整个应用对象
  - 页面对象
  - 组件对象

```bash
# 尽量使用vue-cli4创建项目
npm install -g @vue/cli
vue create -p dcloudio/uni-preset-vue learnuni
cd learnuni
npm run dev:mp-weixin

.v + tab				生成vue文件的结构,在vscode中
```

###### 2.样式和sass

- uniapp直接支持小程序的rpx和h5的vw,vh

```css
/* rpx:小程序中的单位   750rpx=屏幕的宽度 */
.rpx{
	width: 750rpx;
	height: 100px;
	background-color: aquamarine;
}

/* vw是h5中的单位, 100vw=屏幕的宽度, 100vh=屏幕的高度 */
.vw{
	width: 50vw;
	height: 50vh;
	background-color: coral;
}
```

###### sass的使用

```scss
/* 1.安装sass依赖,不用安装也可以 */
npm i node-sass sass-loader

/*2.在style标签中加上lang="scss"即可使用*/
<style lang="scss">
.content{
	.sass{
		background-color: red;
	}
}
</style>
```

## 基本语法

###### 1.数据的展示

```js
// 1.在vue文件的data中定义数据,和vue一样使用,数据可以是对象,数组,bool,数字
export default {
  data(){
    return{
      title: '前端渣渣'
    }
  }
}

// 2.使用mastache语法进行展示
<view class="sass">{{title}}</view>

// 3.绑定属性和vue一样
<view :class="bind">bind</view>
```

###### 2.数组循环

```html
// 1.data中的数据
names: ['hadoop', 'spark', 'django']
// 2.v-for进行循环,不绑定:key会报错
<view v-for="(name, index) in names" :key="index">{{name}}</view>
```

##### 3. v-if 和 v-show

```HTML 
<view v-if="true">v-if</view>
<view v-show="false">v-show</view>
```

###### 4.计算属性

```js
// 1.定义计算属性
computed:{
  titleAdd(){
  		return this.title + ' add'
	}
}

// 2.使用计算属性
<view>{{titleAdd}}</view>
```

##### 5.事件(和vue一样使用)

```html
<!-- 监听click事件,并且传参 -->
<button @click="handleClick('nice')" size="mini">click事件</button>

<!-- methods中定义方法 -->
methods: {
  handleClick(arg){
  	console.log(arg)
	}
}	
```

###### 6.事件传参($event) data-绑定数据

```js
<button data-index="index属性" @click="handleClick($event)" size="mini">click事件</button>

// $event为事件源对象
handleClick(event){
  console.log(event.currentTarget.dataset.index)		// index属性
}
```

###### 7.组件的基本使用(和vue中一致)

```js
// 1在src下新建components目录，并且创建img-border组件
<template>
	<image src="../static/imgs/0.jpg" mode="" />
</template>

<script>
  export default {
  }
</script>

// 2.页面中引入，并使用组件
import imgBorder from "@/components/img-border"

export default {
    // 注册组件
    components:{
      imgBorder
    },
  ｝
 
 // 3.使用组件,使用的时候，建议不要使用大写
 <img-border/>
```

###### 8.父传子（和vue一类似, 通过属性传递）

```html
<ul-con :list="[1, 2, 3, 4]">

props:{
	list: Array
},
```

###### 9.子传父(通过事件传递)

- 子组件通过**触发事件**的方式向父组件传值
- 父组件通过**监听事件**的方式来接受数据

```js
// 子组件$emit传递
<button @click="handleClick">子传父</button>

methods:{
    handleClick(){
      this.$emit('chilClick','参数')
    }
  }

// 2.父组件监听点击接受传递过来的值
<img-border @chilClick = 'click'></img-border>

methods: {
  click(chilarg){
    console.log(chilarg)			// 参数
  }
}
```

###### 10.全局共享数据(vue原型共享)

```js
// 1.通过vue的原型共享数据
// 在main.js中使用原型定义共享数据(数据可以是任意类型)
Vue.prototype.baseurl = 'http://www.baidu.com'
Vue.prototype.print = ()=>{console.log('print hahahah')}

// 2.在页面的onLoad中使用vue原型共享的数据和函数
onLoad(){
  console.log(this.baseurl)
  this.print()
}
```

###### 11.全局共享数据(globalData共享)

```js
// 1.在App.vue中定义共享的数据(可以定义任意数据类型)
globalData:{
  base: 'www.360.com',
  nice(){console.log('global nice..')},
}

// 2.在页面的onLoad中使用globalData共享的数据和函数
onLoad(){
  // 获取globalData中的数据(getApp()代表获取整个应用的实例)
  console.log(getApp().globalData.base)
  getApp().globalData.nice()
  
  // globalData中的数据是可读可写的
  getApp().globalData.base = 'www.sohu.com'
}
```

###### 12.插槽的使用

```html
<!-- 子组件中通过slot定义插槽 -->
<view class="form">
  <view class="form_title">标题</view>
	<view class="form_content">内容</view>
	<slot></slot>  
</view>

<!-- 父组件使用插槽 -->
// @是uniapp帮我们起好的路径别名,代表src目录
import formSlot from '@/components/form-slot'
<form-slot>
  <view>
    <input type="text">
    <radio></radio>
  </view>
</form-slot>
```

###### 13.生命周期

- uniapp的生命周期结合了vue和微信小程序的生命周期
- 全局的onLaunch表示启动应用时
- 页面中的onLoad和onShow分别表示页面加载完毕时和页面显示时
- 组件中使用mounted 表示组件挂载完毕时

###### 14.uniapp中api的使用

```javascript
// 默认方式
uni.request({
    url: 'https://www.example.com/request',
    success: (res) => {
        console.log(res.data);
    }
});

// Promise
uni.request({
        url: 'https://www.example.com/request'
    })
    .then(data => {//data为一个数组，数组第一项为错误信息，第二项为返回数据
        var [error, res]  = data;
        console.log(res.data);
    })

// Await
function async request () {
    var [error, res] = await uni.request({
        url: 'https://www.example.com/request'
    });
    console.log(res.data);
}
```











 