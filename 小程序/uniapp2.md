###### 1.hover状态的css样式

```html
<!-- uniapp中的hover状态使用hover-class -->
<view class="text" hover-class="hover">
  
  .hover{
    color: #09BB07;
    font-size: 50rpx;
    background-color: #09BB07;
  }
```

###### 2.text换行(\n换行)

```html
<!-- text中使用\n进行换行 -->
<text>line1 \n line2</text>

<!-- :selectable="true"文本可选 -->
<text :selectable="true">index \n line1</text>
```

###### 3.可以直接使用scss

```scss
<style lang="scss">
	.content{
		.image{
			height: 100px;width: 100px;
		}
	}
<style>
```

###### 4.样式绑定

```html
<!-- class绑定data中的多个值,使用数组(数组里面绑定的data属性不需要再加单引号) -->
<view class="box" :class="[class1,class2]">
  
<!-- 三元运算符绑定,age大于30才有class1对应的类名 -->
<view class="box" :class="[age>30 ? class1 : '',class2]">

<!-- 如果isActive为true则给它加bor类名 -->
<view :class="{'bor': isActive}">nice</view>
  
<!-- 可以写多个判断属性 -->  
<view :class="{'bor': isActive, 'fs': isActive}">nice</view>
```

###### 5.条件渲染可以使用<template>包裹起来

```html
<template v-if="true">
  <view>hello v-if template</view>
</template>
```

###### 6.@tap手指轻触事件

```html
<button  @tap="changeActive">显示隐藏</button>
```

###### 7.循环对象

```html
<view v-for="(value,key) in obj" :key="key">{{key}}: {{value}}</view>

obj: {
  name: 'hcp',
  age: '22',
  join: '前端'
}
```

###### 8.事件

```js
// 事件映射表，左侧为 WEB 事件，右侧为 ``uni-app`` 对应事件
{
    click: 'tap',
    touchstart: 'touchstart',
    touchmove: 'touchmove',
    touchcancel: 'touchcancel',
    touchend: 'touchend',
    tap: 'tap',
    longtap: 'longtap', //推荐使用longpress代替
    input: 'input',
    change: 'change',
    submit: 'submit',
    blur: 'blur',
    focus: 'focus',
    reset: 'reset',
    confirm: 'confirm',
    columnchange: 'columnchange',
    linechange: 'linechange',
    error: 'error',
    scrolltoupper: 'scrolltoupper',
    scrolltolower: 'scrolltolower',
    scroll: 'scroll'
}
```

###### 9.阻止默认行为使用stop

```html
<!-- @tap.stop -->
<button type="warn" @tap.stop="clickevent">按钮</button>
```

###### 10.监听属性

```js
watch:{
  // 监听title属性,当title属性的值发生改变时,将改变的值打印到控制台
  title: (val)=>{
    console.log(val)
  }
}
```

###### 11.计算属性

```js
computed:{
  titlePro(){
    return this.title + "pro"
  }
}

// 计算属性在页面中跟普通属性一样使用
<view>{{titlePro}}</view>
```

###### 12.封装网络请求

- 1.创建http或者request文件,封装基础请求
- 2.创建api文件,封装具体的请求
- 3.将api对象在main.js中导入,并挂载到Vue原型对象中 Vue.prototype.$api = api
- 4.页面中使用this.$api.functionname()调用api中的方法

```js
// 1.项目根目录下新建utils文件下,新建request.js
export default (params)=>{
	
	// 请求的时候，显示正在加载中
	uni.showLoading({
		title:"加载中..."
	})
	
	return new Promise((resolve, reject) => {
		wx.request({
			...params,
			success(res){
				resolve(res)
			},
			fail(err){
				reject(err)
			},
			complete(){
				uni.hideLoading()
			}
		})
	})
}

// 2.main.js中将request挂载到原型上
import request from './utils/request'
Vue.prototype.request = request

// 3.在页面中使用请求
onLoad(){
  // 挂载到原型对象中,直接使用this即可
  this.request({
    url: 'http://www.baidu.com'
  }).then(res => {
    console.log(res)
  }).catch(err => {
    console.log(err)
  })
}
```

