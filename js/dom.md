###### 1.绑定事件的两种方式

```js
// 1.获取元素
let btn1 = document.getElementById('btn1')

// 2.addEventListener绑定事件
btn1.addEventListener(' click', ()=>{
  alert("点击事件")
})

// 3.直接.onmousemove绑定事件
btn1.onmousemove = function(){
  alert("获取焦点....")
}
```

###### 2.window.onload

```html
<!-- 浏览器会从上到下执行代码，所以script放在body最后 -->
<!-- 可以使用onload绑定当页面的onload，页面加载完毕之后触发 -->
<script>
  window.onload = function(){ 
    let btn = document.getElementById("btn")
    btn.onclick = function(){
      alert("btn被点击")
    }
  }
```

###### 3.获取元素节点

```js
document.getElementById()
document.getElementsBTagName()

// 当input中的checkbox中会有name相同的元素
document.getElementsByName()
```

###### 4.getElements获取到类数组对象

```js
window.onload = function(){
  // 1.getEelementsByTagName()返回一个类数组对象
  let lis = document.getElementsByTagName('li')

  for(let i=0; i<lis.length; i++){
    // 2.获取到元素之后，直接.属性名，获取到属性的值
    alert(lis[i].title)
    lis[i].innerHTML = `for遍历${i}`
  }
}
```

###### 5.获取元素属性,直接.属性名

```js
let btn = document.getElementById("btn")
btn.name
btn.title
btn.style
// class为js关键字,通过className获取
btn.className
```

###### 6.获取元素节点的子节点

```js
let ul = document.getElementById('ul')

ul.getElementsByTagName()
ul.childNodes								// 表示当前节点的所有子节点
ul.firstChild								// 表示当前节点的第一个子节点
ul.lastChild								// 表示当前节点的最后一个子节点

window.onload = function(){
  let ul = document.getElementById('ul')
  console.log(ul)
  // childNodes获取到所有子节点，不管是文本还是元素
  let lis = ul.childNodes
  console.log(lis)

  // children获取到元素的所有子元素(不包括文本节点，推荐使用)
  let lis2 = ul.children
  console.log(lis2)

  let first = ul.firstChild
  let last = ul.lastChild
  }
```
