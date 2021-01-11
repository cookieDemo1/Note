######  1.安装

```bash
$ create-create-app  react-ant

$ cd react-ant

$ npm install antd --save
```

###### 2.App.js中使用ant

```tsx
import React from 'react';
// 1.引入Button组件(如果在index.js中引入,就相当于全局引入,任何组件都可以使用)
import {Button} from 'antd'
// 2.引入ant中的css样式
import 'antd/dist/antd.css'
function App() {
  return (
   <div>
     {/* 3.使用ant组件 */}
     <Button type="primary">Button</Button>
   </div>
  );
}

export default App;
```

###### 3.Ant按需加载

```jsx
//1.手动加载方式
import Button from 'antd/es/button'
import 'antd/es/button/style/css'

// 2.配置文件方式
// 1).拉取react配置文件(运行报错的话,需要把git目录删除,即取消git管理)
npm run eject

// 2).安装这个插件
npm install babel-plugin-import --save-dev

// 3).在package.json中的 "babel中加入插件"
"babel": {
    "presets": [
      "react-app"
    ],
    "plugins": [
      [
          "import",
          {
              "libraryName": "antd",
              "libraryDirectory": "es",
              "style": "css"
          }
      ],
      "transform-runtime"
  	]
  },
  // 4).组件中就可以不引入css,该插件会自动根据引入的组件,自动引入相关的css样式 
```

###### 4.网络请求(使用fetch)

- react可以直接使用fetch, fetch是基于peomise的

```jsx
  // 在生命周期方法中发送请求 Get
  componentDidMount(){
    fetch("http://iwenwiki.com/api/blueberrypai/getIndexInteresting.php").then(res => {
      console.log(res.status);
    }).catch(err => {
      console.log(err);
    })
  }
```

###### 5.渲染请求的数据 Get

```jsx
import React from 'react';

export default class App extends React.Component {

  constructor(){
    super() 

    this.state = {
      status: ''
    }
  }

  // 在生命周期方法中发送请求
  componentDidMount(){
    fetch("http://iwenwiki.com/api/blueberrypai/getIndexInteresting.php").then(res => {
      // 将请求的数据放到state的status中
      this.setState({
        status: res.status
      })
    }).catch(err => {
      console.log(err);
    })
  }

  render(){
    return(
      <div>
        {this.state.status}
      </div>
    )
  }
}
```

###### 6.post请求

```jsx
  componentDidMount(){
    fetch("http://iwenwiki.com/api/blueberrypai/login.php",{
      method: "POST",
      headers: {
        'content-type': 'application/x-www-form-urlencode',
        "Accept": "application/json,text/plain,*/*"
      },
      // 1.body: "user_id=iwen@qq.com&password=iwen123&verification_code=crfvw"    fetch中post请求体参数,需要写成这样
      
      // 1.如果想写成对象类型,则需要导入querystring的包
      body: qs.stringify({
        user_id: "iwen@qq.com",
        password: "iwen123",
        verification_code: 'crfvw'
      })
    }).then(res => res.json()).then(data => {
      console.log(data)
    })
  }
```

###### 7.开发模式下解决跨域问题

```jsx
// 1.package.json中增加配置
  "proxy": "http://www.baiud.com"
}

// 2.发送请求的时候,就不用加上http://www.baidu.com, 直接加后面的url
 componentDidMount(){
    fetch('/sf').then(res => res.json()).then(res => {
      console.log(res)
    }).catch(err => {
      console.log(new Error(err))
    })
  }
  render(){
    return (
      <div></div>
    )
  }

// 增加配置需要重启
npm start
```

###### 封装fetch网络请求

```js
// 1.基础封装, http.js
import qs from 'querystring'
export function httpGet(url){
  const result = fetch(url);
  return result
}

export function httpPost(url,params){
  const result = fetch(url,{
    method: 'POST',
    headers: {
      'content-type': 'application/x-www-form-urlencode',
      "Accept": "application/json,text/plain,*/*"
    },
    body: qs.stringify(params)
  })
}
```

### react-router

###### 1安装

```bash
$ npm install react-router-dom --save
```



