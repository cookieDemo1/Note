##### React

##### 1.React的引入

- 笔记中的 **script** 皆不写  type="text/babel" 
- Ant-design  react组件库

```html
<script src="https://cdn.staticfile.org/react/16.4.0/umd/react.development.js"></script>
<script src="https://cdn.staticfile.org/react-dom/16.4.0/umd/react-dom.development.js"></script>
<!-- babel的功能是将ES6转换成ES5，将jsx转换成js -->  
<script src="https://cdn.staticfile.org/babel-standalone/6.26.0/babel.min.js"></script>
```

##### 2.React命令行方式引入

```bash
$ cnpm install -g create-react-app
$ create-react-app my-app
$ cd my-app/
$ npm start
```

##### 3.简单使用

```html
<body>
  <div id="test"></div>
  <!-- script中的type值必须是type="text/babel" -->  
  <script>
  	const name = "spark"
    // .1创建虚拟DOM元素，JSX语法
    var vDom = <h1>hello word!</h1>
    // 2.创建虚拟DOM的时候，可以使用外部变量,用{}将外部变量包裹即可
    var vDom2 = <h1 id={name.toUpperCase()}>{name.toLowerCase()}</h1>
    ReactDOM.render(vDom, document.getElementById('test'))
  </script>
</body>
```

##### 4.使用api创建虚拟DOM（这种方式一般不会用）

```html

  <div id="test"></div>
  <script>
    // 1.api创建虚拟DOM
    var element = React.createElement('h2',{id: 'h2'}, 'hello'.toUpperCase()
    // debugger可以打断点，浏览器执行到这里会暂停
    debugger
    ReactDOM.render(element, document.getElementById('test'))
  </script>
```

##### 5.直接开始用脚手架

```bash
# 1.创建项目
$ create-react-app learn1
$ ce learn1

# 2.运行
$ npm start
```

###### 6jsx语法

- 遇到 <> 按照html语法解析
- 遇到 {} 按照javascript语法解析 

```html
<!-- jsx语法: javascript + xml语法 -->
<h1>hello word</h1>
```

###### 7.jsx拼接变量 (index.js文件)

```jsx
import React from 'react';
import ReactDOM from 'react-dom';


// jsx中使用{}拼接变量
const react = "const recat"
// 使用{}拼接变量
ReactDOM.render(<h1>Hello {react}</h1>, document.getElementById('root'))
```

###### 8.元素渲染

```jsx
import React from 'react';
import ReactDOM from 'react-dom';

// 元素渲染
function tick(){
  // 这里是一个小括号
  // ()如果存在标签解构,并且标签结构要换行,需要用小括号括起来
  const element = (
    <div>
      <h1>hello word</h1>
      <h2>It is {new Date().toLocaleTimeString()}</h2>
    </div>
  )
  // 这个渲染需要放在tick()函数里面
  ReactDOM.render(element, document.getElementById('root'))
}

// 这里设置一个定时器,每隔1秒钟渲染一次,就会刷新时间
setInterval(tick, 1000)
```

###### 9.组件(新建文件 app.jsx)

- 一个react项目是由成千上万个组件组成的
- 组件里面可以使用组件

```jsx
import React from "react"
// 用类的形式创建组件,需要继承自React.Component
class App extends React.Component{
  // render()为渲染函数,在render()函数里面,做我们需要做的事情
  render(){
    // 有很多个内容可以加小括号
    return (
      <div>
        <h1>hello react component!</h1>
      </div>
    )
  }
}

// 将App导出
export default App

// ============ index.js中使用 ================
import React from 'react';
import ReactDOM from 'react-dom';

// 导入App,并使用
import App from './app'
ReactDOM.render(<App/>, document.getElementById('root'))
```

###### 10.props属性( 传递数据 ) 父传子

- props不可以修改

```jsx
// 1.使用的时候传递值 
render(){
    const nav1= ["111","222","333"]
    const nav2= ["web", "node", "java"]
    return (
        <div>
          <h1>hello react component!</h1>
          <h2>学react最重要的是心态要好!</h2>
          <Home></Home>
        	{/* 这里进行传值,可以传递多个值 */}
          <MyNav nav={nav1} content={"nice"}></MyNav>
          <MyNav nav={nav2} content={"perfect"}></MyNav>
        </div>
      )
  }
// 2.遍历渲染值
import React from 'react'

export default class MyNav extends React.Component{
  render(){
    // 传递过来的数据在props中
    console.log(this.props.nav);
    return(
      <div>
        <ul>
          {/* 对props中的nav进行遍历渲染 */}
          {
            this.props.nav.map((item,index) => {
              return <li key={index}>{item}</li>
            })
          }
        </ul>
      </div>
    )
  }
}
```

######  11.state

```jsx
import React from 'react'

export default class StateComponent extends React.Component{
  /**
   * 组件中的状态state
   * 以前我们操作页面元素的变化，都是修改DOM，操作DOM
   * 但是有了React优秀的框架，我们不在推荐操作DOM。页面元素的改变使用State进行处理
   */

  // 定义一个构造函数
  constructor(props){
    super(props);
    // 定义状态
    this.state = {
      count: 10,
      flag: false
    }
  }

  increment(){
    // 通过this.setState()进行修改
    this.setState({
      count: this.state.count += 1
    })
  }

  decrement(){
    this.setState({
      count: this.state.count -= 1
    })
  }

  // 使用箭头函数，按钮就不用绑定this
  // this直接指向StateComponent这个组件
  clickHandler = ()=>{
    console.log(this);
  }

  // change改变state中的flag状态
  change = ()=>{
    this.setState({
      flag : !this.state.flag
    })
  }


  render(){
    // 根据state中flag的值，动态的修改showView数据
    let showView = this.state.flag ? '我是孙悟空' : '我是假的孙悟空'
    return(
      <div>
        <h3>组件的state</h3>
        {/* 使用构造函数中定义的状态 */}
        <p>{this.state.count}</p>
        
        {/* 这里两个按钮绑定函数,需要bind绑定this */}
        <button onClick={this.increment.bind(this)}>增加</button>
        <button onClick={this.decrement.bind(this)}>减少</button>   
        <button onClick={this.clickHandler}>关于this</button>        

        <p>{showView}</p>
        <button onClick={this.change}>修改flag的值</button>
      </div>
    )
  }
}
```

###### 12react生命周期函数

```jsx
import React from 'react'
export default class LifeComponent extends React.Component{
  /**
   * 函数列表
   *    componentWillMount: 在组件渲染之前执行
   *    componentDisMount:  在组件渲染之后执行
   *    shouldComponentUpdate: 返回true或者false, true代表允许改变，false代表不允许改变
   *    compomentWillUpdated: 数据在改变之前执行（state, props）
   *    compomentDidUpdate: 数据修改完成（state, props）
   *    componentWillReveiceProps: props发生改变执行（父组件可以修改props，子组件不可以修改props）
   *    componentWillUnmount: 组件卸载前执行
   */

  constructor(props){
    super(props)
    this.state = {
      count: 10
    }
  }

  changeHandle = ()=>{
    this.setState({
      count: this.state.count += 1
    })
  }

  componentWillMount(){
    console.log("componentWillMount...")
  }
  componentDidMount(){
    console.log("componentDidMount...");
  }
  shouldComponentUpdate(){
    console.log("shouldComponentUpdate...");
    return true
  }
  componentWillUpdate(){
    console.log("componentWillUpdate...");
  }

  componentDidUpdate(){
    console.log("componentDidUpdate...");
  }
  componentWillReceiveProps(){
    console.log("componentWillReceiveProps...")
  }
  componentWillUnmount(){
    console.log("componentWillUnmount...");
    
  }

  render(){
    // 使用Es6的解构赋值,获取到state中的count
    const {count} = this.state
    return(
      <div>
        生命周期函数:{count} - {this.props.title}
        <button onClick={this.changeHandle}>修改</button>
      </div>
    )
  }
}
```

###### 13.子传父

```jsx
// 1.子组件
<button onClick={this.clickChange}>修改title</button>

clickChange = ()=> {
  // 这里this.props.changeHandleParent()为父组件中的方法
  this.props.changeHandleParent("传参：我是child的数据")
}

// 2.父组件
// 父组件中监听changeHandleParent事件.并调用自己的
<LifeComponent title = {this.state.title} changeHandleParent={this.clickChange}/>
// data为传递过来的数据
clickChange = (data)=> {
    this.setState({
      title: data
    })
  }  
```

###### 14.关于setState是同步还是异步

- setState()会引起视图的重绘
- 在可控的情况下是异步，在非可控的情况下是同步

```jsx
import React from 'react'

export default class SetStateDemo extends React.Component{
  
  constructor(props){
    super(props)
    this.state = {
      count: 0
    }
  }

  increment = () => {
    // this.State()有一个回调函数可以实时的获取到state中的值
    this.setState({
      count: this.state.count += 1
    }, () => {
      console.log("callback",this.state.count)
    })
    
    // 在这里获取的则不是实时更新的state中的count(测试这里也是同步)
    console.log("outer", this.state.count)
  }
  render(){
    return(
      <div>
        setState是同步还是异步问题
        <p>{this.state.count}</p>
        <button onClick={this.increment}>increment</button>
      </div>
    )
  }

}
```

###### 15.条件渲染

```jsx
import React from 'react'
export default class IfDemo extends React.Component{
  /**
   * 条件渲染常见的应用场景
   *    1.对视图条件进行切换
   *    2.做缺省值
   */
  constructor(){
    super()
    this.state = {
      isLogin: false
    }
  }

  clickHandler = ()=>{
    this.setState({
      isLogin: !this.state.isLogin
    })
  }

  render(){
    // 这里进行判断:是否为true,true则返回一个iwenm, false返回一个请登录
    let showView = this.state.isLogin ? <div>iwen</div> : <div>请登录</div>
    return(
      <div>
        {/* 这里使用上面的showView */}
        条件渲染: {showView}
        <button onClick={this.clickHandler}>登录</button>
      </div>
    )
  }
}
```

###### 16.列表渲染

```jsx
import React from 'react'

export default class ListDemo extends React.Component{

  constructor(){
    super();
    this.state = {
      userInfo:[
        {
          name: "hello",
          age: 12,
          sex: "female"
        },
        {
          name: "hello",
          age: 12,
          sex: "female"
        },
        {
          name: "hello",
          age: 12,
          sex: "female"
        },
      ]
    }
  }

  render(){
    return(
      <div>
        <ul>
          {
            this.state.userInfo.map((item, index) => {
            return( 
            // 在最外层绑定key
            <li key={index}>
              <span>{item.name}</span>
              <span>{item.age}</span>
              <span>{item.sex}</span>
            </li>)
            })
          }
        </ul>
      </div>
    )
  }
}
```

###### 17.两层遍历

```jsx
 render(){
    return(
      <div>
        <ul>
          {
            this.state.userInfo.map((item, index) => {
            return( 
            // 在最外层绑定key
            <li key={index}>
              <span>{item.name}</span>
              <span>{item.age}</span>
              <span>{item.sex}</span>
              <div>
                {
                  item.jobs.map((childItem, chilIndex) => {
                  return <span key={chilIndex}>{childItem}</span>
                  }) 
                }
              </div>
            </li>)
            })
          }
        </ul>
      </div>
    )
  }
```

###### 18.遍历加key是为了不 重新渲染之前的数据

```jsx
import React from 'react'

export default class ListDemo extends React.Component{

  constructor(){
    super();
    this.state = {
      userInfo:[
        {
          name: "hello",
          age: 12,
          sex: "female",
          jobs: ["lanqiu", "feiqiu", "pic"],
        },
        {
          name: "hello",
          age: 12,
          sex: "female",
          jobs: ["lanqiu", "feiqiu", "pic"],
        },
        {
          name: "hello",
          age: 12,
          sex: "female",
          jobs: ["lanqiu", "feiqiu", "pic"],
        },
      ]
    }
  }

  clickHandle = ()=>{
    this.setState({
      // 不能使用push,因为push返回的是数组的长度
      userInfo: this.state.userInfo.concat([
        {
          name: 'Nice',
          age: 12,
          sex: '男',
          jobs: [1,2,3,4,5]
        }
      ])
    })
  }

  render(){
    return(
      <div>
        <ul>
          {
            this.state.userInfo.map((item, index) => {
            return( 
            // 在最外层绑定key
            <li key={index}>
              <span>{item.name}</span>
              <span>{item.age}</span>
              <span>{item.sex}</span>
              <div>
                {
                  item.jobs.map((childItem, chilIndex) => {
                  return <span key={chilIndex}>{childItem}</span>
                  }) 
                }
              </div>
            </li>)
            })
          }
        </ul>
        <button onClick={this.clickHandle}>添加数据</button>
      </div>
    )
  }
}
```

###### 19.表单受控组件

- 表单受控组件有时候可能会很麻烦,因为每一个input都需要绑定onChange时间,当input框变多之后,就会绑定很多事件
- 大多数情况下,推荐使用受控组件

```tsx
import React from 'react'
// 表单受控组件,代表表带里面的value值是通过state来帮我们管理的
// 非受控组件则是我们自己操作DOM管理
export default class Form1 extends React.Component{
  constructor(){
    super()
    this.state = {
      // value为onChange中绑定的值
      value: ""
    }
  }
  // 4.表单onSubmit事件绑定的函数
  handleSubmit = (event)=>{
    // 阻止表单默认的事件
    event.preventDefault();
    // 这里打印input中的值
    console.log(this.state.value)
  }

  // 2.onChange里面又event对象,通过event则可以拿到input中输入的值
  onChangeHandler = (event)=>{
    this.setState({
      value: event.target.value
    })
  }

  render(){
    return(
      <div>
        {/* 3.表单绑定onSubmit事件 */}
        <form onSubmit={this.handleSubmit}>
          {/* 1.这里input必须绑定一个onChange时间,因为input的value值绑定了state中的值,而state中的值只能有setState改变 */}
          名字:<input type="text" value={this.state.value} onChange={this.onChangeHandler}/>
          <input type="submit" value="提交"/>
        </form>
      </div>
    )
  }
}

```

###### 20.refs和DOM ( 非表单控件,必须先学习这两个 )

- react并不推荐我们直接操作DOM

```tsx
import React from "react"
export default class RefsAndDom extends React.Component{
  /**
   * refs的使用场景,即操作Dom的场景
   *    1.管理焦点,文本选择或媒体播放
   *    2.触发强制动画
   *    3.集成第三方DOM库
   */
  constructor(){
    super()
    // 1.创建一个DOM对象
    this.myRef = React.createRef()
  }

  // 3.在生命周期函数中获取ref绑定的dom对象
  componentDidMount(){
    // current获取当前的DOM, 即下面的div
    console.log(this.myRef.current);
    // 将当前dom的color设置成红色
    this.myRef.current.style.color = "red";
  }

  render(){
    return(
      <div>
        refs and dom
        {/* 2.使用ref绑定创建好的DOM对象 */}
        <div ref={this.myRef}>
          hello
        </div>
      </div>
    )
  }
}
```

###### 21.非表单受控组件

```tsx
import React from 'react'

export default class Form2 extends React.Component{

  constructor(){
    super()
    // 1.创建ref对象
    this.username = React.createRef()
  }

  // 3.通过dom获取到input元素，然后获取到input中的value
  clickHandler = (eve)=> {
    console.log(this.username.current.value)
  }
  
  render(){
    return(
      <div>
        {/* 2.表单绑定ref对象 */}
        <input type="text" ref={this.username}/>
        <button onClick={this.clickHandler}>事件</button>  
      </div>
    )
  }
}
```

###### 22.状态提升

```tsx
// 1.parent

import React from 'react'
import Child1 from './状态提升1'
import Child2 from './状态提升2'

export default class Parent extends React.Component{

  constructor(){
    super();
    // money是父级的,money将分发给俩个state
    this.state = {
      money: 1
    }
  }
  changeHandler = (e)=> {
      this.setState({
        money: e.target.value
      })
  }

  render(){
    return(
      <div>
        <input type="text" value={this.state.money} onChange = {this.changeHandler}/>
        <p>Parent</p>
        child1人民币:<Child1 money={this.state.money}/>
        child2美元:<Child2 money={this.state.money}/>
      </div>
    )
  }
}

// 2.child1
import React from 'react'

export default class Child1 extends React.Component{

  constructor(){
    super();
    this.state = {
      input1: 0
    }
  }

  changeHandler = (e)=> {
    this.setState({
      input1: e.target.value
    })
    console.log(e.target.value)
  }

  componentDidMount(){
    this.setState({
      inpu1: this.refs.money
    })
  }

  
  render(){
    return(
      <div>
        {/* 这里接受parent中的money,因为是RMB所以不需要改动 */}
        <p>{this.props.money}</p>
        <input type="text" value={this.state.input1} onChange = {this.changeHandler}/>
      </div>
    )
  }
}

// 3.child2
import React from 'react'

export default class Child2 extends React.Component{

  constructor(){
    super();
    this.state = {
      input2: 0
    }
  }

  changeHandler = (eve)=>{
    this.setState({
      input2: eve.target.value*7
    })
    console.log(eve.target.value*7)
  }

  componentDidMount(){
    this.setState({
      input2: this.props.money*7
    })
  }

  render(){
    return(
      <div>
        {/* 这里接受parent中的money的值,因为是美元,所以要乘以7 */}
        <p>{this.props.money * 7}</p>
        <input type="text" valu={this.state.input2} onChange={this.changeHandler}/>
      </div>
    )
  }
}
```

###### 23.组合

```jsx
// 1.app.jsx中使用Compose组件
<Compose>
  {/* 这里的div可以被Compose,然后将这个div组合起来 */}
  <div>我是组合效果</div>
</Compose>

// 2.Compose组件
import React from 'react'
export default class Compose extends React.Component{
  render(){
    return(
      <div>
        {/* this.props.childre为使用组件的使用,包裹的div */}
        哈哈哈哈哈{this.props.children}
      </div>
    )
  }
}
```

###### 24.使用propTypes进行类型检查 ( 为了增强程序的兼容性 )

```jsx
// 1.app.jsx使用组件并传值,子组件期望有一个string,而我们传递number,会有警告,但是不会报错
<PropsTypeDemo title={123}/>

// 2.子组件
import React from 'react'
// 这里需要引入验证字段的包
import PropTypes from 'prop-types'
export default class PropsTypeDemo extends React.Component{
  render(){
    return(
    <div>hello {this.props.title}</div>
    )
  }
}
// 验证字段,验证title必须是string ,那么父组件传title的时候则必须为string
PropsTypeDemo.propTypes = {
  title: PropTypes.string
  // 加 .isRequired为必须传,默认不传值不会报错
  // title: PropTypes.string.isRequired
}

// 默认值:如果没有值,则props中的title为这个默认值
PropsTypeDemo.defaultProps = {
  title: "我是默认值"
}

/**
 类型验证有这几种类型:
 		PropTypes.array
 		PropTypes.bool
 		PropTypes.func
 		PropTypes.object
 		PropTypes.string
 		PropTypes.symbol
 */


PropsTypeDemo.propTypes = {
  // 加 .isRequired为必须传,默认不传值不会报错
  title: PropTypes.string.isRequired
}

// 默认值:如果没有值,则props中的title为这个默认值
PropsTypeDemo.defaultProps = {
  title: "我是默认值"
}
```

###### 