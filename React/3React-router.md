###### 1.基础使用

- HashRouter: 锚点链接
- BrowserRouter: h5新特性(history.push 方法) 如果上线之后,需要后台做一些处理: 重定向处理, 因为一刷新可能会有404

```bash
npm install react-router-dom -S
```



```jsx
import React from 'react';
// 1.引入两个组件
import Home from './pages/Home'
import Mine from './pages/Mine'

// 2.引入路由相关的js文件
import {BrowserRouter as Router, Switch, Route, Link} from 'react-router-dom'
// 如果Router是HashRouter的话,则会有#
// import {HashRouter as Router, Switch, Route, Link} from 'react-router-dom'
function App(){
  return(
    // 3.使用路由
    <div>
      {/* 使用Router包裹两个小的Route */}
      <Router>
        <Route path="/home" component={Home}></Route>
        <Route path="/mine" component={Mine}></Route>
      </Router>
    </div>
  )
}

export default App


// 当访问不同的路径的使用,就会出现不同的组件内容
http://localhost:3000/mine
http://localhost:3000/home
```

###### 2.Link的方式点击切换

```jsx
{/* Link最终会被渲染成a标签 */}
<Router>
  <Route path="/home" exact={true} component={Home}></Route>
  <Route path="/mine" exact={true} component={Mine}></Route>
  <button>
    <Link to="/home">home页面</Link> 
  </button>
  <button>
    <Link to="/mine">mine页面</Link> 
  </button>
</Router>
```

###### 3.exact模式(精准匹配路径) 默认模糊匹配,包含这个路径就会匹配到

```jsx
return (
  <div>
    {/* 当一个属性的值为true或者false的时候，只写属性名，它的值就是true */}
    <Route path="/home" exact component={Home}></Route>
    <Route path="/mine" exact={true} component={Mine}></Route>
    <Route path="/mine/ucent" exact={true} component={Mine}></Route>
  </div>
)
```

###### 4.home页面,默认就是home页面

```jsx
// 把Route和Link的path都改成/,并且加 exact={true}. (不加的话所有路径都会匹配到home)
<Router>
  <Route path="/" exact={true} component={Home}></Route>
  <Route path="/mine" exact={true} component={Mine}></Route>
  <button>
    <Link to="/">home页面</Link> 
  </button>
  <button>
    <Link to="/mine">mine页面</Link> 
  </button>
</Router>
```

###### 5.strick路径后面多加一个 / 不匹配  ( 默认可以 )

```jsx
<div>
  <Route path="/" exact={true} component={Home}></Route>
  <Route path="/mine" strict={true} exact={true} component={Mine}></Route>
  <Route path="/mine/ucent" exact={true} component={Mine}></Route>
</div>
```

###### 6.404页面

```jsx
// 1.定义一个404组件
import React from 'react'
const NotFound = ()=>{
  return(
    <div>
      <h2>404页面</h2>
    </div>
  )
}
export default NotFound

// 2.App.jsx中使用, 将Route使用switch包裹起来,NotFound组件放进去,不写path, 则匹配不到的时候,就会显示NoFound组件
// NotFound页面
  <Router>
    <Nav></Nav>
    <Switch>
      <Route path="/" exact={true} component={Home}></Route>
      <Route path="/mine" strict={true} exact={true} component={Mine}></Route>
      <Route path="/mine/ucent" exact={true} component={Mine}></Route>
      <Route component={NotFound}></Route>
    </Switch>
  </Router>
```

###### 7.router和link分离

```jsx
// 1.新建Nav组件, 里面都是link的跳转
import React from 'react'
import {BrowserRouter as Router, Switch, Route, Link} from 'react-router-dom'

export default class Nav extends React.Component{
  render(){
    return (
      <div>
        <Link to="/">home页面</Link> 
        <Link to="/mine">mine页面</Link> 
        <Link to="/mine/ucent">mine/ucent页面</Link> 
      </div>
    )
  }
}

// 2.app.js中引入Nav组件
import React from 'react';
import Home from './pages/Home'
import Mine from './pages/Mine'
import Nav from './pages/index'
import NotFound from './pages/NotFound'
import {HashRouter as Router, Switch, Route, Link} from 'react-router-dom'

function App(){
  return(
    <div>     
      <Router>
        <Nav></Nav>
        <Switch>
          <Route path="/" exact={true} component={Home}></Route>
          <Route path="/mine" strict={true} exact={true} component={Mine}></Route>
          <Route path="/mine/ucent" exact={true} component={Mine}></Route>
          <Route component={NotFound}></Route>
        </Switch>
      </Router>
    </div>
  )
}
export default App
```

###### 8.render函数,不使用创建组件的方式,相当于把组件写在render函数中

```jsx
// render相当于直接创建一个组件,访问localhost:3000/demo就会显示render函数中的内容
<Route path="/demo" render={()=> <div>render函数的组件</div>}></Route>

 // 也可以render另一个组件
<Route path="/demo" render={()=> <Home></Home>}></Route>
```

###### 9.function的方式定义组件

```jsx
// 导入并使用的方式和原来一样
import React from 'react'
const NotFound = ()=>{
  return(
    <div>
      <h2>404页面</h2>
    </div>
  )
}

export default NotFound
```

###### 10.render和function定义的组件之间传值

```jsx
// 1.定义组件
import React from 'react'

// props为使用render函数使用组件的时候，传递的值
const RenderProps = (props)=>{
  console.log(props)
  return(
    <div>
      Render Props {props.name}
    </div>
  )
}
export default RenderProps

// 2.(使用组件)传递的值,props是框架定义好的属性(将在后面解释), name为自己传的
<Route path="/renderprops" render={(props)=> <RenderProps {...props} name={"render"}/>}></Route>
```

###### 11.点击高亮( 将Link换成 NavLink )

```jsx
// 1.将Link换成NavLink
import React from 'react'
// 组件引入css方式
import "./style.css"
import {BrowserRouter as Router, Switch, Route, Link, NavLink} from 'react-router-dom'

export default class Nav extends React.Component{
  render(){
    return (
      <div>
        {/* 需要加上exact否则会匹配到多个,就会有样式污染问题 */}
        <NavLink exact to="/">home页面</NavLink> 
        <NavLink exact to="/mine">mine页面</NavLink> 
        <NavLink exact to="/mine/ucent">mine页面</NavLink> 
        <NavLink exact to="/demo">render demo</NavLink> 
        <NavLink exact to="/renderprops">render props</NavLink> 
      </div>
    )
  }
}

// 2.换了之后选中的NavLink(最终会被渲染成a标签)就会有class = active 属性

// 3.新建style.css
.active{
  text-decoration: none;
  color: #fff;
  padding: 3px;
  background-color: palevioletred;
  font-size: 14px;
}

// 4.在上面的文件中引入
import "./style.css"

// 5.会有样式污染问题,解决方法,在NavLink上加exact
<div>
  {/* 需要加上exact否则会匹配到多个,就会有样式污染问题 */}
  <NavLink exact to="/">home页面</NavLink> 
  <NavLink exact to="/mine">mine页面</NavLink> 
  <NavLink exact to="/mine/ucent">mine页面</NavLink> 
  <NavLink exact to="/demo">render demo</NavLink> 
  <NavLink exact to="/renderprops">render props</NavLink> 
</div>

// 6.行内样式,使用activeStyle
<NavLink exact to="/mine" activeStyle={{color: 'red'}}>mine页面</NavLink> 

// 7.可以修改原来的激活的class名字
<NavLink exact to="/mine" activeClassName="selected">mine页面</NavLink> 
```

###### 12.路径传参

```jsx
// 1.路由跳转的时候传参 :id 为传递路径参数id
<Route path="/mine/ucent/:id" exact={true} component={Ucent}></Route>

// 2.访问路径的时候带参 (123为传递的参数, 将会被映射成id)
http://localhost:3000/#/mine/ucent/123

// 3.组件内获取参数, 传递的参数在 props.match.params.id 里面
import React from 'react'
const Ucent = (props) => {
  console.log(props)
  console.log(props.match.params.id)
  return(
    <div>ucent</div>
  )
}
export default Ucent

// 4.加一个问号代表可传可不传, 不加的话不传会出现404
<Route path="/mine/ucent/:id?" exact={true} component={Ucent}></Route>
  
// 5.传多个参数
<Route path="/mine/ucent/:id?/:name?" exact={true} component={Ucent}></Route>
// 访问路径
http://localhost:3000/#/mine/ucent/122/hcp
// 获取参数
console.log(props.match.params.id)
console.log(props.match.params.name)

// 6.一般不是手动在浏览器上加参数,而是在跳转的时候拼接参数
<NavLink exact to="/mine/ucent/123/hcp">mine/ucent页面</NavLink> 
```

###### 13.常规的路径参数传参( request的params参数 ?name=123&id=12 )

```jsx
// 1.定义组件
import React from 'react'
const Params = (props) => {
  // ?name=hcp&age=12, 这种路径参数需要创建一个UrlSearchParams对象来得到
  const params = new URLSearchParams(props.location.search)
  console.log(params)
  // 通过UrlSearchParams实例的get方法获取
  console.log(params.get("name"))
  console.log(params.get("age"))
  return(
    <div>Params</div>
  )
}
export default Params

// 2.使用组件
<Route path="/params" component={Params} />
  
// 3.访问路径的时候就可以传参
http://localhost:3000/#/params?name=hcp&age=123

// 4.另一种获取请求参数的方式,使用querystring
import React from 'react'
import QueryString from 'querystring'
const Params = (props) => {
  const value = QueryString.parse(props.location.search)
  // 第一个key上多了一个?(?name)
  console.log(value)
  console.log(value["?name"])
  console.log(value.age)
}
```

###### 14.Link参数可以写成对象

```jsx
// 写成对象需要两个大括号
/* to可以写成对象形式，pathname为路径，search为请求参数，hash, state */
<NavLink exact to={{
    pathname: '/mine',
    search: "?name=hcp",
    hash: "#zh-cn",
    state: { isshow : true}
  }}>mine页面</NavLink>

// 组件中通过props获取到参数
import React from 'react'
const Mine = (props) => {
  console.log(props)
  console.log(props.location.pathname)
  console.log(props.location.search)
  console.log(props.location.hash)
  console.log(props.location.state)
  return (
    <div>
      Mine
    </div>
  )
}
export default Mine
```

###### 15.重定向

```jsx
// 1.路由中引入重定向,并使用 index.jsx
import {HashRouter as Router, Switch, Route, Link, Redirect} from 'react-router-dom'
<Redirect from="/hellomine" to="/mine"/>

// 2.nav.jsx
<NavLink exact to="/hellomine">这个路径重定向到mine</NavLink>
    
    
    <div className="App">
      {/* 使用Router包裹Route */}
      <Router>
        <Nav></Nav>
        <Switch>
          {/* Redirect放在Router标签里面 */}
          <Redirect from="/about" to="/redirectFaker"></Redirect>
          <Route path="/" exact component={Home}></Route>
          <Route path="/mine" exact component={Mine}></Route>

          {/* render函数，把组件写在render函数中 */}
          <Route path="/render" render={()=> <h1>render函数</h1>}></Route>

          {/* render函数中也可以直接返回一个组件 */}
          <Route path="/about" render={()=> <About></About>}></Route>

          {/* 路径传参 */}
          <Route path="/ucent/:id" exact={true} component={Ucent}></Route>

          {/* Params */}
          <Route path="/params" component={Params}></Route>

          {/* 演示重定向 */}
          <Route path="/redirectFaker" component={RedirectFaker}></Route>

          {/* 404页面，需要使用Switch将Route给包裹起来，最终匹配不到会显示这个组件 */}
          <Route component={Page404}></Route>
        </Switch>
      </Router>
    </div>
```

###### 16.重定向判断是否登录

```jsx
import React from 'react'
import { Redirect } from 'react-router-dom'
export default class Shop extends React.Component{
  // 1.这里有一个状态，判断是否登录， false为未登录
  state = {
    isLogin: false
  }
  render(){
    const {isLogin} = this.state
    return(
      // 2.如果登录了就显示本页面，否则使用Redirect重定向到首页
      <div>
        {isLogin?<div>Shop</div> : <Redirect to="/"/>}
      </div>
    )
  }
}

// 4.该组件需要在路由中注册
```

###### 17.重定向push和replace

```jsx
import React from 'react'
const Mine = (props) => {
  // 函数的形式定义组件，里面的方法这样写，触发这个事件跳转到home
  const clickHandler = (eve)=>{
    // props里面有history有push replace go goback方法是用来跳转的
    props.history.push("/home")
    // push和replace的区别是push是叠加的，上一个页面依然存在，可以返回，而repalce则是替换，不可以返回
    props.history.replace("/home")
  }

  return (
    <div>
      Mine
      {/* 监听事件的时候，直接写方法名字即可，不需要加this */}
      <button onClick={clickHandler}>回到home</button>
    </div>
  )
}

export default Mine
```

###### 18.没有定义在Route中的组件如何使用重定向

```jsx
import React from 'react'
import {withRouter} from "react-router-dom"
// 这个组件，被Mine组件使用，没有定义在Router里面，所以没有路由对象，无法进行重定向
// 但是可以导出的时候通过withRouter(MineDemo)导出，即有路由对象，则可以进行重定向
class MineDemo extends React.Component{
  clickHandler(){
    console.log(this.props)
    this.props.history.push("/home")
  }
  // 当前组件没有直接被路由管理，所以没有路由对象
  // 使用withRouter
  render(){
    return(
      <div>
        <button onClick={this.clickHandler.bind(this)}>回到home</button>
      </div>
    )
  }
}
// 高阶组件的处理方案
export default withRouter(MineDemo)
```

###### 19.promt， 在离开页面时候做某些事情

```jsx
import React from 'react'
// 导入Promt, Promt相当于钩子函数，在离开页面的时候，做某些事情
import { Redirect,Prompt } from 'react-router-dom'
export default class Shop extends React.Component{
  // 1.这里有一个状态，判断是否登录
  state = {
    isLogin: true,
    name: ''
  }
  render(){
    const {isLogin} = this.state
    return(
      <div>
        {isLogin?<div>Shop</div> : <Redirect to="/"/>}
        
        {/* 这里一个input输入框 ，一个Promt*/}
        {/* when触发条件，对name两次取反，判断是否存在，当输入框中没有值，则when条件不成立，不会触发 */}
        {/* message会出现一个弹窗，提示确定要离开吗。确定就会离开，取消还是在这个页面 */}
        <Prompt
          when = {!!this.state.name}
          message = {"确定要离开吗"}
        />
        <input type="text" value={this.state.name} onChange={(e)=> this.setState({name: e.target.value})}/>
      </div>
    )
  }
}
```





