###### 1.变化侦测

```js
从状态生成DOM，再输出到用户界面显示的一整套流程叫做渲染，应用在运行时会不断地进行重新渲染.而响应式系统赋予框架重新渲染的能力，其重要组成部分是变化侦测。变化侦测是响应式系统的核心，没有它，就没有重新渲染渲染。框架在运行时，视图也就无法随着状态的变化而变化。

简单来说，变化侦测的作用是侦测数据的变化。当数据变化时，会通知视图进行相应的更新。
```

###### 2.侦测变化例子

- 有两种方法可以侦测到变化Object.defineProperty和ES6的proxy
- ES6在浏览器中的支持并不理想。到目前为止vue还是用Object.definePorperty实现的

```js
// 这里的函数defineReactive用来对Object.defineProperty进行封装。从函数名字可以看出，其作用就是定义一个响应式数据。也就是在这个函数张工进行变化追踪。封装后只需要传递data, key, val就行了。
// 封装好之后，每当data的key中读取数据时，get函数触发，每当往data的key中设置数据时，set函数被触发
function defineReactive(data, key, val){
  Object.defineProperty(data, key, {
    enumerable: true,
    configurable: true,
    get: function(){
      return val
    },
    set: function(newVal){
      if(val === newVal){
        return
      }
      val = newVal
    }
  })
}

var data = {}
defineReactive(data, 'name', 'perfect')
console.log(data.name)
```

###### 3.如何收集依赖

- **如果只是把Object.defineProperty进行封装，那其实并没什么实际用处，真正有用的是收集依赖。**

```html
// 该模板使用了数据name,所以当它发生变化时，要向使用了它的地方发送通知
// 在vue2中模板使用数据等同于组件使用数据，即把用到数据name的地方收集起来，然后等属性发生变化时，把之前收集好的依赖循环触发一遍就好了。
// 总结起来就是getter中收集依赖，在setter中触发依赖
<tempalte>
    <h1>{{ name }}</h1>
</template>
```

###### 4.收集依赖代码

```js
// 1.依赖收集类，帮助我们管理依赖(收集依赖，删除依赖或者想依赖发送通知等)
export default class Dep{
  constructor(){
    this.subs = []
  }
  addSub(sub){
    this.subs.push(sub)
  }
  removeSub(sub){
    removeEventListener(this.subs, sub)
  }
  depend(){
    if(window.target){
      this.addSub(window.target)
    }
  }
  notify(){
    // slice是数组切片，不传start和end就相当于拷贝数组
    const subs = this.subs.slice()
    for(let i=0, l=subs.length; i<l; i++){
      subs[i].update()
    }
  }
}

// 删除数组元素
function remove(arr, item){
  if(arr.length){
    const index = arr.indexOf(item)
    if(index > -1){
      return arr.splice(index, 1)
    }
  }
}

// 2.改造一下在变化侦测中收集依赖
function defindReactive(data, key, val){
  let dep = new Dep()
  Object.defineProperty(data, key,{
    enumerable: true,
    configurable: true,
    get: function(){
      dep.depend()          // 修改
    },
    set: function(newVal){
      if(val === newVal){
        return
      }
      val = newVal
      dep.notify()          // 新增
    }
    
  })
}
```

###### 5.依赖

```js
// 上面的代码中收集的依赖是window.target,那么它到底是什么?我们究竟要收集谁呢。
// 收集谁？换句话说，就是当属性发生变化后，通知谁
// 我们要通知用到数据的地方，而使用这个数据的地方有很多，而且类型还不一样，既有可能是模板，也可能是用户写的一个watch,这时需要抽象出一个能集中处理这些情况的类。然后我们在依赖手机阶段只收集这个封装好的类的实例进来。通知也只通知它一个。接着，他再通知其他地方。所以，我们要抽象的这个东西需要先起一个好听的名字。就叫Watcher
```

###### 6.vue.js无法监听的属性

```js
// Vue.js通过Object.defineProperty来将对象的key转换成getter/setter的形式来追踪变化，但getter/setter只能追踪一个数据是否被修改，无法追踪新增属性和删除属性。
// 为了解决这个问题Vue.js提供了两个API：vm.$set和vm.$delete
```

###### 7.总结

```js
/**
变化侦测就是侦测数据的变化.当数据变化时，要能侦测到并发出通知。
Object可以通过Object.defineProperty将属性转换成getter/setter的形式来追踪变化。读取数据时会触发getter,修改数据时会触发setter
*/
```

