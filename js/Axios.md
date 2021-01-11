### Axios

##### 1.Axios概念

```js
1.Axios是第三方网络模块
2.我们需要对第三方网络框架进行封装，不要直接使用第三方框架
```

##### 2.Axios功能

```js
1.在浏览器中发送XMLHttpRequests请求
2.在node.js中发送http请求
3.支持Promise API
4.拦截请求和响应
5.转换请求和响应
```

##### 3.Axios使用

```js
// 1.创建vue项目
vue create learnaxios

// 2.安装axios
npm i axios -S

// 3.main.js中写axios代码,访问其他地址会产生跨域的问题，所以只能访问本地的8080端口
import axios from 'axios'
// axios()会返回一个Promise对象，所以可以调用then方法
axios({
  url: ' http://localhost:8080/'
}).then(res => console.log(res))

// 4.启动，查看控制台有没有打印
npm run serve
```

##### 4.指定post请求

```js
axios({
  url: ' http://localhost:8080/',
  method: 'post'
}).then(res => console.log(res))
```

##### 5.params传递参数

```js
axios({
  url: ' http://localhost:8080/',
  // params进行参数传递，?name=nice&age=12 拼接url也可以
  params:{
    name: 'nice',
    age: 12
  }
}).then(res => console.log(res))
```

##### 6.axios处理两个并发请求(可以处理多个)

```js
// axios并发请求,两个请求都成功后,才对结果进行处理
axios.all([
  axios({
    url: 'http://localhost:8080'
  }),
  axios({
    url: 'http://localhost:8080'
  }),
]).then(results => {
  // results返回的是一个数组，数组里面有两个元素，分别是两个请求返回的结果
  console.log(results)
})
```

##### 7.两个并发请求，then中也可以通过spread将两个结果返回，传一个回调函数

```js
axios.all([
  axios({
    url: 'http://localhost:8080'
  }),
  axios({
    url: 'http://localhost:8080'
  }),
// then中将两个结果分割，传一个callback,arg1:第一个请求的结果，arg2:第二个请求的结果
]).then(axios.spread((res1, res2) =>{
  console.log(res1)
  console.log(res2)
}))
```

##### 8.Axios全局配置

```js
// 全局配置（配置一些公共的东西）
axios.defaults.baseURL = 'http://localhost:8080'
axios.defaults.timeout = 5000
axios.defaults.method = 'get'
// params和get请求对应
axios.defaults.params= {name: 'hadoop'}
// data(请求体)和post请求对应
//axios.defaults.data = {}

// 因为有全局配置，所以url可以不用写
axios.all([
  axios({
    url: ''
  }),
  axios({
    url: ''
  }),
]).then(axios.spread((res1, res2) =>{
  console.log(res1)
  console.log(res2)
}))
```

##### 9.Axios实例，我们之前使用是全局的Axios

- 如果全局的axios都设置了baseUrl，但是有的请求不是这个url开头，则会出乱，所以axios可以实例化

```js

// 1.创建axios实例
const axios1 = axios.create({
  // 实例中的全局配置
  baseURL: 'http://localhost:8080',
  method: 'get'
})

// 2.使用axios实例
axios1({
  url: ''
}).then(res => console.log(res))
```

##### 10.网络请求进行封装，开发中一定要进行封装!!!!!!

- 第三方框架一般都要进行封装!!!!!

##### 11.网络请求封装1（使用这种最简单）!!!!

```js
// 1.在src下新建network目录,新建request.js文件
import axios from 'axios'
// 如果想导出多个，最好不要使用default
export function request(config){
  // 1).创建axios实例
  const instance = axios.create({
    baseURL:'http://localhost:8080',
    timeout: 5000
  })

  // 2).我们将结果直接返回，因为它本身就是一个Promise,调用这个函数的人就可以直接then
  // 这样调用request(config).then().catch()
  return instance(config)
}


// 2.使用
import {request} from './network/request'
request({url:'',params:{name:'hadoop'}}).then(res=>console.log(res))
```

##### 11.网络请求封装2（锻炼回调）

```js
// 1.调用直接传入3个参数，config为配置，success为函数(成功时的回调)，fail(失败时的回调)
export function request(config,success,fail){
  const instance = axios.create({
    baseURL:'http://localhost:8080',
    timeout: 5000
  })
  instance(config).then(res => {
    success(res)
  }).catch(err=>{
    fail(err)
  })
}

// 2.使用，传递了三个参数,arg2为成功的回调 arg3为失败的回调
import {request} from './network/request'
request({url:''}, res=>console.log(res), err=>console.log(err))
```

##### 5.Axios拦截器的使用

```js
export function request(config){
  const instance = axios.create({
    baseURL:'http://localhost:8080',
    timeout: 5000
  })

  // axios拦截器
  // instance.interceptors  拦截实例的
  // axios.interceptors     拦截全局的
  // interceptors.request   拦截请求
  // interceptors.request   拦截响应

  // 1请求拦截!!
  // 请求发送出去，来到第一个函数，没有发送出去来到第二个函数，一般不会来到第二个
  instance.interceptors.request.use(config => {
      // 拦截之后可以判断有没有传入一些token的信息，判断config中的一些信息符不符合服务器要求
      // config就是一些url,params，timeout的数据,和axios传入的数据是一样的
      console.log(config)
      // 拦截之后，要return出去
      return config
  }, err =>console.log(err))
    
  // 响应拦截!!成功的拦截直接拿到响应的数据,拦截之后也要return回去可以直接return res.data
  // 响应成功来到第一个函数，响应失败来到第二个函数
  instance.interceptors.response.use(res=>{console.log(res),return res.data},err=>console.log(err))

  return instance(config)

}

// main.js还是一样调用
request({url:''}).then(res => console.log("sueess")).catch(err => console.log("error"))
```

##### js-cookie插件

```js
https://www.jianshu.com/p/6e1bacd35f59
```

###### 1)UT-9082封装

```js
import Axios from 'axios'

Axios.defaults.timeout = 200000
Axios.defaults.headers['Content-Type'] = 'application/json;charset=UTF-8'

// 配置了跨域之后，这里就需要配置baseurl
// Axios.defaults.baseURL = 'http://172.16.14.69'

const service = Axios.create({

})

service.interceptors.request.use(
  (config) =>{
    // 如果有token，让每个请求的请求头带上token。即登录之后的所有请求都会带上token
    let token = localStorage.getItem('token')
    if(token){
      config.headers['token'] = token
    }
    return config
  },
  error =>{
    console.log(error)
    return Promise.reject(error)
  }
)

service.interceptors.response.use(
  (response) => {
    // 在response中进行判断状态码，如果是401则token过期，将token移除，并跳转到login组件
    console.log(response.status)
    if(response.status == 401){
      localStorage.removeItem('username')
      localStorage.removeItem('token')
      this.router.push('/login')
      return
    }
    return response
  },
  error => {
    console.log(error)
    return Promise.reject(error)
  }
)

export default service
```

###### 2)ut9082跨域设置 （vue.config.js）

```js
module.exports = {
  // 打包部署需要加publicPath
  publicPath: './',

  // productionSourceMap:false, 打包优化(打包后文件会缩小)
  productionSourceMap:false,
  configureWebpack: {
    resolve: {
      alias:{
        // 默认已经配置了'@'别名
        // '@': 'src',

        // vue cli3中别名可以使用@
        'assets': '@/assets',
        'common': '@/common',
        'components': '@/components',
        'network': '@/network',
        'views': '@/views',
        'api': '@/api',
        'utils': '@/utils'
      }
    }
  },

  // 配置跨域
  devServer: {
    open: true,
    host: 'localhost',
    // 原先配置的是8080
    port: 8080,
    https: false,
    hotOnly: false,
    proxy: {
      '/cgi-bin':{
        target: "http://172.16.14.52/cgi-bin/",
        ws: true,           // websocket
        changeOrigin: true,
        pathRewrite: {
          '^/cgi-bin': ''
        }
      },
    }
  }
}
```

###### 3)使用

```js
import request from '@/utils/http'

// 获取首页的全部数据

export function getAll() {
  return request({
    url: 'cgi-bin/home',
    method: 'get',
    params:{
      cmd: '0xFF'
    },
  })
}
```

###### 4)路由

```js
import Vue from 'vue'
import VueRouter from 'vue-router'

// 子路由，单独写在文件中
import fourGSetting from './child/4gSetting'
import wizard from './child/wizard'
import wifiSetting from './child/wifiSetting'
import network from './child/network'
import security from './child/security'
import deviceManager from './child/deviceManager'
import networkProtocol from './child/networkProtocol'
import cloudPlatform from './child/cloudPlatform'

Vue.use(VueRouter)

  const routes = [
  {
    path: '/',
    name: 'layout',
    redirect: '/home',
    component: ()=> import('@/layout/index'),

    children:[
      {
        path: '/home',
        name: 'home',
        component: () => import('views/home/index'),
        meta:{
          title: 'home'
        }
      },

      {
        path: '/wizard',
        name: 'Wizard',
        component: () => import('../views/wizard/index'),
        meta:{
          title: 'Wizard'
        },
        children: wizard

      },

      {
        path: '/wifiSetting',
        name: 'wifiSetting',
        component: () => import('../views/wifiSetting/index'),
        meta:{
          title: 'wifiSetting'
        },

        children: wifiSetting
      },

      {
        path: '/network',
        name: 'network',
        component: () => import('../views/network/index'),
        meta:{
          title: 'network'
        },
        children: network
      },

      {
        path: '/deviceManager',
        name: 'deviceManager',
        component: () => import('../views/deviceManager/index'),
        meta:{
          title: 'deviceManager'
        },
        children: deviceManager
      },

      {
        path: '/4gSetting',
        name: '4gSetting',
        component: () => import('../views/4gSetting/index'),
        meta:{
          title: '4gSetting'
        },
        children: fourGSetting
      },

      {
        path: '/networkProtocol',
        name: 'networkProtocol',
        component: () => import('../views/networkProtocol/index'),
        meta:{
          title: 'networkProtocol'
        },
        children: networkProtocol
      },

      {
        path: '/security',
        name: 'security',
        component: () => import('../views/security/index'),
        meta:{
          title: 'security'
        },

        children: security
      },


      {
        path: '/cloudPlatform',
        name: 'cloudPlatform',
        component: () => import('../views/cloudPlatform/index'),
        meta:{
          title: 'cloudPlatform'
        },

        children: cloudPlatform
      },

    ]
  },
  {
    path: '/login',
    name: 'login',
    component: () => import('views/login/index'),
    meta: {
      title: 'login'
    }
  }
]

const router = new VueRouter({
  // 打包需要将mode注释，默认值为hash
  // mode: 'history',
  base: process.env.BASE_URL,
  routes
})

router.beforeEach((to, from, next) => {
  // 路由发生变化修改页面title
  if (to.meta.title) {
    document.title = to.meta.title
  }

  // 如果是登陆页面直接跳转
  if(to.path === '/login'){
    next()
    return
  }

  // 如果是其他页面则判断有没有token
  let token = localStorage.getItem('token')
  // 如果localStorage中没有token则跳转到/login页面
  if(token === '' || token === null ||　token === undefined){
    next({path: '/login'})
  }else{
    next()
  }

})

export default router

```







