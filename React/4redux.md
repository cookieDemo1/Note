###### 1.使用bootstrap (class需要写成className)

```jsx
// 1.public/index.html中引入bootstrap
<link href="https://cdn.bootcdn.net/ajax/libs/twitter-bootstrap/4.5.0/css/bootstrap.min.css" rel="stylesheet">

// 2.app.jsx中使用, class必须写成className
import React from 'react';
import Parent from './components/coms/Parent'


export default class App extends React.Component{
  render(){
    return (
      <div className="container">
        {/* <Parent/> */}
        <button className="btn btn-success">increment</button>
        <button className="btn btn-primary">decreament</button>
      </div>
    )
  }
}
```

###### 2.安装redux和使用 ( redux单一状态 )

- redux和react-redux的区别
- redux本身是一个js的控制器
- react-redux是封装了一个针对于react应用的控制器

```jsx
// 1.安装
cnpm i redux -S
```

###### 3.使用redux ( 当触发事件之后,store中的值就会改变,改变之后就会重新渲染数据 ) 

```jsx
// 1.新建这个文件src/reducers/counter.js (这个文件为reducer,是创建redux)
// 这个为reducer(参数有两个state和action)
// 创建redux的时候counter为必须的
const counter = (state = 0, action)=>{
  switch(action.type){
    case "INCREMENT":
      return state+1;
    case "DECREMENT":
      return state-1
    default:
      return state;
  }
}
export default counter

// 2.src/index.js中使用
// 1).从redux的包中，引入createStore
import {createStore} from 'redux'

// 2).引入写好的reducer
import reducer from './reducers/counter'

// 3.创建store仓库,有三个参数可以传递，但是我们先传一个reducer
const store = createStore(reducer)

// 4.使用store,给App子组件传递onIcrement事件,这个事件会进入store中的reducer中,将counter-1 或 +1
// 并将reducer中的数据传递过去
ReactDOM.render(
  <App 
    onIncrement ={()=> store.dispatch({type: "INCREMENT"})}
    onDecrement ={()=> store.dispatch({type: "DECREMENT"})}
    value = {store.getState()}
  />,
  document.getElementById('root')
);



// 上面只可以渲染，但是不完善，不可以渲染改变后的数据。这里增加代码之后，可以
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';


// 1).从redux的包中，引入createStore
import {createStore} from 'redux'

// 2).引入写好的reducer
import reducer from './reducers/counter'

// 3.创建reducer,有三个参数可以传递，但是我们先传一个reducer
const store = createStore(reducer)
// 监听store数据变化
// store.subscribe(()=>console.log("state:", store.getState()))

// 将渲染数据封装成一个render
const render = ()=>{
ReactDOM.render(
  // 当触发onIncrement事件的时候，进入reducer对应的case中，进行对应的操作
    <App 
      onIncrement ={()=> store.dispatch({type: "INCREMENT"})}
      onDecrement ={()=> store.dispatch({type: "DECREMENT"})}
      value = { store.getState() }
    />,
    document.getElementById('root')
  );
}

// 进来先渲染一次
render()
// 每次store的数据改变之后重新渲染
store.subscribe(render)

// 3.App.js中监听事件
import React from 'react';
export default class App extends React.Component{
  render(){
    return (
      <div className="container text-center">
        {/* 使用index.js中传递过来的value, 这个value为store中的counter数据 */}
        <h1 className="">{this.props.value}</h1>
        {/* 当触发click事件，就会调用父组件的onIncrement方法，将state-1或+1，并且父组件中会打印 */}
        <button onClick={this.props.onIncrement} className="btn btn-success">increment</button>
        <button onClick={this.props.onDecrement} className="btn btn-primary">decreament</button>
      </div>
    )
  }
}
```

###### react-redux

- 引入了reduct-redux, redux仍然需要

```jsx
// 1.安装
npm install -S react-redux

// 1.index.js
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
// react-dom的使用
import {Provider} from 'react-redux'
import reducer from './reducers/counter'
import { createStore } from 'redux';

const store = createStore(reducer)

ReactDOM.render(
  {/* 使用privoder */}
  <Provider store={store}>
    <App/>
  </Provider>,
  document.getElementById('root')
);

// 2.App.jsx
import React from 'react';
// 需要导入conect将reducer连接起来
import {connect} from 'react-redux'
// 引入两个action
import {increment, decrement} from './actions/counter'


class App extends React.Component{
  render(){
    console.log(this.props)
    // 将increment和decrement从props中提取出来
    const {increment, decrement, counter}  = this.props
    return (
      <div className="container text-center">
        <h1 className="">{counter}</h1>
        <button onClick={()=>increment()} className="btn btn-success">increment</button>
        <button onClick={()=>decrement()} className="btn btn-primary">decreament</button>
      </div>
    )
  }
}
const mapStateToProps = (state) => {
  return{
    counter: state
  }
}

// dispatch参数是用来分发事件的,触发action中的类型对应的reducer中的事件
const mapDispatchToProps = (dispatch) => {
  return{
    // 用dispatch去触发increment和decrement事件
    increment: ()=> {dispatch(increment())},
    decrement: ()=> {dispatch(decrement())}
  }
}

// 将这个组件和app连接,将上面创建的mapState和mapDispatch传递过去,这两个参数顺序不能颠倒
export default connect(mapStateToProps, mapDispatchToProps)(App) 


// 3.原来的reducer,仍然存在  /src/reducers/counter.js
const counter = (state = 0, action)=>{
  switch(action.type){
    case "INCREMENT":
      return state+1;
    case "DECREMENT":
      return state-1
    default:
      return state;
  }
} 
export default counter

// 4.react-redux需要创建一个新的action  /src/actions/counter.js
export function increment(){
  return{
    type: "INCREMENT"
  }
}

export function decrement(){
  return{
    type: "DECREMENT"
  }
}
```
