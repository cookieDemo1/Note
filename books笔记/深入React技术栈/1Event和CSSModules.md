###### 1.合成事件的实现机制

```js
在 React 底层，主要对合成事件做了两件事：事件委派和自动绑定

1.事件委托
在使用 React 事件前，一定要熟悉它的事件代理机制。它并不会把事件处理函数直接绑定到真实的节点上，而是把所有事件绑定到结构的最外层，使用一个统一的事件监听器，这个事件监听器上维持了一个映射来保存所有组件内部的事件监听和处理函数。当组件挂载或卸载时，只是在这个统一的事件监听器上插入或删除一些对象；当事件发生时，首先被这个统一的事件监听器处理，然后在映射里找到真正的事件处理函数并调用。这样做简化了事件处理和回收机制，效率也有很大提升。

2.自动绑定
在 React 组件中，每个方法的上下文都会指向该组件的实例，即自动绑定 this 为当前组件。而且 React 还会对这种引用进行缓存，以达到 CPU 和内存的最优化。在使用 ES6 classes 或者纯函数时，这种自动绑定就不复存在了，我们需要手动实现 this 的绑定。
```

###### 2.数据单向流动

```js
在 React 中，数据是单向流动的。从示例中，我们能看出来表单的数据源于组件的 state，并通过 props 传入，这也称为单向数据绑定。然后，我们又通过 onChange 事件处理器将新的表单数据写回到组件的 state，完成了双向数据绑定。

实际上，在 React 内部拦截了浏览器的原生事件，这得益于 Virtual DOM 以及合成事件系统。
```

###### 3.非受控组件

```js'
在 React 中，非受控组件是一种反模式，它的值不受组件自身的 state 或 props 控制。通常，需要通过为其添加 ref prop 来访问渲染后的底层 DOM 元素。
```

###### 4.受控组件和非受控组件的区别

```js
在受控组件中，可以将用户输入的英文字母转化为大写后输出展示，而在非受控组件中则不会。而如果不对受控组件绑定 change 事件，我们在文本框中输入任何值都不会起作用。多数情况下，对于非受控组件，我们并不需要提供 change 事件。通过上面的示例可以看出，受控组件和非受控组件的最大区别是：非受控组件的状态并不会受应用状态的控制，应用中也多了局部组件状态，而受控组件的值来自于组件的 state。
```

### 样式处理

###### 1.style

```jsx
render(){
    const style = {
        color: '#FFF',
        backgroundColor: '#000'
    }

    return(
        <div style={style}>Hello Style</div>
    )
}
```

###### 2.像素值省略React会自动帮我们加

```jsx
// 渲染成 height: 10px
const style = { height: 10 };
ReactDOM.render(<Component style={style}>Hello</Component>, mountNode); 
```

###### 3.CSS Modules

```jsx
CSS Modules。依旧使用 CSS，但使用 JavaScript 来管理样式依赖。CSS Modules 能最大化地结合现有 CSS 生态和 JavaScript 模块化能力，其 API 非常简洁，学习成本几乎为零。发布时依旧编译出单独的 JavaScript 和 CSS 文件。现在，webpack css-loader 内置 CSSModules 功能。
```

###### 4.CSS MOdules模块化方案

```jsx
CSS Modules 内部通过 ICSS 来解决样式导入和导出这两个问题，分别对应 :import 和 :export 两个新增的伪类：

:import("path/to/dep.css") {
   localAlias: keyFromDep;
   /* ... */
}
:export {
   exportedKey: exportedValue;
   /* ... */
} 
```

###### 5.使用css Module方式1

- creat-react-app默认已经实现了css modules,只要把后缀变成.module.css或者.module.scss即可

```jsx
// 1.button.module.css,文件后缀名改为.module.css即可以使用模块化css
/* 默认是局部样式，只对改组件内生效 */
.button{
  width: 100px;
  height: 40px;
  background-color: chocolate;
  color: white;
  outline: none;
  border-radius: 4px;
  border: none;
}

/* 2.全局样式，可以在其他组件中使用 */
:global(.btn){
  width: 200px;
  height: 100px;
}

// 3.在组件内使用局部样式
import styles from './button.module.css'
// 这里绑定的时候style.button
<button className={styles.button}>submit</button>

// 4.其他组件内可以直接用全局样式
<button className={btn}></button>
```

###### 6.使用CSS Module方式2

```jsx
// 1.暴露webpack配置
npm run eject

// 2.找到config.webpack.config.js

use: getStyleLoaders({
    importLoaders: 1,
    modules: true,    				// 增加这一行
    sourceMap: isEnvProduction
    ? shouldUseSourceMap
    : isEnvDevelopment,
}),

// 3.新建p.css文件，这是不需要加module后缀
.title{
  color: lightcoral;
  font-size: 20px;
  font-weight: 700;
}

// 4.组件中直接使用css module语法
import styles from './p.css'

<p className={styles.title}>hello word</p>
```

###### 7.composes组合样式(基于CSS Module)

```jsx
// 1.index.css
/* 基本样式 */
.base{
  font-size: 18px;
  font-weight: bold;
  padding: 2em;
  color: #FFF;
  border-radius: 0.3em;
}

/* 组合base样式，直接base,省略. */
.info{
  composes: base;
  background-color: lightslategray;
}

.success{
  composes: base;
  background-color: lightgreen;
}

.danger{
  composes: base;
  background-color: tomato;
}

// 2.index.jsx
import React,{Component} from 'react'
// 模块化方式导入css
import styles from './index.css'
class Composes extends Component{
  constructor(props){
    super(props)
  }

  render(){
    return(
      <div>
        {/* 使用组合的样式 */}
        <p className={styles.info}>info</p>
        <p className={styles.success}>success</p>
        <p className={styles.danger}>danger</p>
      </div>
    )
  }
}
```

###### 8.css与js变量共享(基于CSS Module)

```jsx
// 1 index.css
:export{
  color: white,
}

// 2 index.jsx
import React,{Component} from 'react'
import styles from './index.css'
class Composes extends Component{
  constructor(props){
    super(props)
  }
  // 获取index.css中:export中导出的color变量
  componentDidMount(){
    console.log(styles.color)
  }
}
```

###### 9.CSS Modules使用技巧

```jsx
/*
CSS Modules 是对现有的 CSS 做减法。为了追求简单可控，作者建议遵循如下原则：
 不使用选择器，只使用 class 名来定义样式；
 不层叠多个 class，只使用一个 class 把所有样式定义好；
 所有样式通过 composes 组合来实现复用；
 不嵌套。
**/
```

###### 10.CSS Modules模块化注意点

```jsx
(1) 如果我对一个元素使用多个 class 呢？
    样式照样生效。
(2) 如果我在一个 style 文件中使用同名 class 呢？
    这些同名 class 编译后虽然可能是随机码，但仍是同名的。
(3) 如果我在 style 文件中使用了 id 选择器、伪类和标签选择器等呢？
	所有这些选择器将不被转换，原封不动地出现在编译后的 CSS 中。也就是说，CSS Modules只会转换 class 名相关的样式。
```

###### 11.使用CSS Module之后也可以全局引入

```jsx
// 将import from 改成 import即可
import "./index.css"
```





 