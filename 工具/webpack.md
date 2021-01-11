### webpack

- 模块化管理工具，进行模块化打包
- webpack依赖node环境
- node -v 查看node版本

##### 1.安装webpack

```bash
# 指定3.6.0版本，-g全局安装，全局安装只要在shell里面就可以用
npm install webpack@3.6.0 -g
```

##### 2.项目目录结构

 - src 源码
   	- main.js 程序的入口
	- dist 目标文件，src打包后的文件

##### 3.打包

```js
// 1.utils中导出add，common.js的方式
function add(num1,num2) {
    return num1+num2
}
module.exports = {
    add
}

// 2.main.js中导入add，common.js的方式
const {add} = require('./mathUtil.js')
console.log(add(10, 20));

// 3.使用webpack打包man.js
webpack ./src/main.js ./dist/bundle.js

// 4.页面使用打包好的js文件
<script src="./dist/bundle.js"></script>
```

##### 4.自动构建

```js
// 1.项目根目录中执行npm init,  npm init -y可以快速初始化,会生成package.json文件
npm init

// 2.项目根目录下创建webpack.config.js
// path是从node的包中寻找
const path = require('path')
module.exports = {
    // 指定源文件
    entry: './src/main.js',
    // 指定目标文件的路径和文件名，path必须写绝对路径
    output: {
        // ，__dirname是node中的全局变量，用来获取当前文件所在的目录
        path: path.resolve(__dirname,'dist'),
        // 打包的文件名
        filename: 'bundle.js'
    },
}

// 3.在项目根目录下执行webpack命令，会自动把main.js打包到bundle.js
webpack
```

##### 5.package.json配置

- 执行npm init就会有这个文件

```json
{
  "name": "meet",
  "version": "1.0.0",
  "description": "",
  "main": "webpack.config.js",
  // script为脚本，可以通过npm run build 或者 npm run test执行
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    // build为自己添加的脚本，直接npm run build即可代替webpack
    // npm run build会优先使用本地的webpack
    "build": "webpack"
  },
  "author": "",
  "license": "ISC",
  // 开发时依赖
  "devDependencies": {
  	"webpack": "^3.6.0"
  }
  // 运行时依赖
  "dependencies": {}
}
```

##### 6.本地安装webpack

- webpack一般会在每个项目中安装一个本地的，而不使用全局的
- npm  install  webpack@3.6.0  --save-dev     
- --save-dev为开发时依赖，因为项目上线后不需要webpack， -D 和 --save-dev一样

##### 7.loader的使用

- webpack如果要将ES6转成ES5,TypeScript转成ES5，scss转换成css，vue文件转成js文件等就需要配置对应的loader
- loader使用过程
  - 1).通过npm安装需要使用的loader
  - 2).在webpack.config.js中的modules关键字下进行配置

##### 8.webpack中使用css文件

- 新建normal.css文件（src/css/normal.css），index.html中不用引用，直接把css文件打包到bundle.js中

```js
// 1.normal.css
body{
    background-color: antiquewhite;
}

// 2.main.js中依赖css文件。
require('./css/normal.css')

// 3.安装css的loader
npm install --save-dev css-loader
npm install --save-dev style-loader

// 4.webpack.config.js中使用loader
const path = require('path')
module.exports = {
    entry: './src/main.js',
    output: {
        path: path.resolve(__dirname,'dist'),
        filename: 'bundle.js'
    },
    // module中使用css-loader
    module: {
        rules: [
            {
                test: /\.css$/,
                // css-loader只负责加载，不负责解析和将样式应用到html中，所以需要使用style-loader
                // 使用多个loader时是从右往左读，所以基础的要写在右边
                use: [ 'style-loader','css-loader' ]
            }
        ]
    }
}
// 5.打包,打包后样式生效
npm run build
```

##### 9.webpack中less处理

```js
// 1.新建special.less(src/css/special.less)文件
@fontSize: 50px;
@fontColor: green;
body{
  font-size: @fontSize;
  color: @fontColor;
}

// 2.main.js中依赖less文件
require('./css/special.less')

// 3.安装loader,安装两个less-loader和less
npm install --save-dev less-loader less

// 4.webpack.config.js中配置loader,配置从webpack官网中复制
module: {
        rules: [
            {
                test: /\.css$/,
                // css-loader只负责加载，不负责解析和将样式应用到html中，所以需要使用style-loader
                use: [ 'style-loader','css-loader' ]
            },
            {
                test: /\.less$/,
                use: [{
                    loader: "style-loader" // creates style nodes from JS strings
                }, {
                    loader: "css-loader" // translates CSS into CommonJS
                }, {
                    loader: "less-loader" // compiles Less to CSS
                }]
            }
        ]
    }

// 5.使用npm run build构建
num run build
```

##### 10.webpack处理图片

```js
// 1.创建iamge文件，放一张time.jpg

// 2.normal.css中使用time.jpg作为背景图
body{
    background: url("../image/time.jpg");
}

// 3.安装url-loader
npm install --save-dev url-loader

// 4.配置loader
{
    test: /\.(png|jpg|gif)$/,
        use: [
            {
                loader: 'url-loader',
                options: {
                    // 当图片小于1000kb的时候回转成base64编码的图片，如果超过则报错,base46编码的图片不会保存
                    limit: 1000,
                    // 图片保存的命名方式：
                    // 在img目录下：原来的名字 + 8位hash值 + 原来的扩展名
                    name: 'img/[name][hash:8].[ext]'
                }
            }
        ]
}

// 5.运行命令
npm run build

// ----------------------------------------------------------------------

// 6.如果要加载大于limit的图片。而且不会报错,则需要安装file-loader，不需要配置
npm install --save-dev file-loader

// 7.因为超过大小的图片需要使用file-loader加载，而file-loader的图片会打包dist目录中，所以需要配置
output: {
        path: path.resolve(__dirname,'dist'),
        filename: 'bundle.js',
        // 在output中配置publicPath，以后只要是跟url路径有关的东西，都会拼接上这个路径
        // 打包的时候index会在这个目录中，这行配置就不需要了
        publicPath: 'dist/'
    },

// 8.执行构建命令
npm run build
```

##### 11.ES6转ES5的loader （babel）

```js
// 1.安装loader,babel-preset-es2015不是按官方的，官方的是babel-parset-env（需要配置babel.rc）
npm install babel-loader babel-core babel-preset-es2015

// 2.配置
 {
     test: /\.js$/,
         // 排除,这些东西不需要转化
         exclude: /(node_modules|bower_components)/,
             use: {
                 loader: 'babel-loader',
                     options: {
                         presets: ['es2015'],
                    }
               }
       }
```

##### 12.webpack配置vue

```js
// 1.安装vue, --save 运行时依赖  -S也可以
npm install vue --save

// 2.main.js中使用Vue
import Vue from 'vue'
const app = new Vue({
    el: '#app',
    data: {
        message: 'hello word!'
    }
})

// 3.index.html中
<div id="app">
  <p>{{message}}</p>
</div>

// 4.编译
npm run build

// 5.没有效果，runtime-only：代码中不可以有任何的template,需要在webpack.config.js中配置这个
 resolve:{
        alias:{
            'vue$':'vue/dist/vue.esm.js'
        }
    }

```

##### 13.el和template的关系

```js
// 1.tempalte会自定替代页面中 id=app 的内容
import Vue from 'vue'
const vm = new Vue({
    el: '#app',
    template:`<h1>nice</h1>`,
    data: {
        message: 'hello word!'
    }
})

// 即代替这个div里面的内容
<div id="app">
</div>
<script src="./dist/bundle.js"></script>
```

##### 14.vue的终极解决方案

```js
import Vue from 'vue'

// 1.将template抽离成一个组件
const cnp = {
    template: `<h1>nice</h1>`,
    data(){
        return{
            message: 'hello word!'
        }
    }
}

// 2.Vue中注册这个组件，并且应用到template中
const vm = new Vue({
    el: '#app',
    template:`<cnp/>`,
    components:{
        cnp
    }
})

```

##### 15.终极解决方案2

```js
// 新建app.js文件,将他导出
export default {
    template: `<h1>nice</h1>`,
    data(){
        return{
            message: 'hello word!'
        }
    }
}

// main.js文件中将他导入，然后注册
import Vue from 'vue'
import App from './vue/app'
const vm = new Vue({
    el: '#app',
    template:`<App/>`,
    components:{
        App
    }
})

```

##### 16.终极解决方案3

```js
// 1.新建app.vue文件
<template>
  `<h1>{{message}}</h1>`,
</template>

<script>
    export default {
        name: "app",
        data(){
            return{
                message: "nice"
            }
        }
    }
</script>

<style scoped>
h1{
  color: yellow;
}
</style>

// 2.安装vue的loader
npm install vue-loader vue-template-compiler --save-dev

// 3.配置loader
{
	test:/\.vue$/,
	use:{
		loader: 'vue-loader'
	}
}

// 4.配置polugin，在配置loader同一个文件中
const VueLoaderPlugin = require('vue-loader/lib/plugin')
plugins: [
	new VueLoaderPlugin()   //15版本需指定plugin
]

// 5.npm run build
```

##### 17.配置导入文件的时候不用写后缀

```js
// 在webpack.config.js中配置
resolve:{
    alias:{
        'vue$':'vue/dist/vue.esm.js'
    },
        // 指定我们需要导入的时候，这些后缀不用写
    extensions: ['js','css','vue']
},
```

##### 18.plugin(将index.html也放到dist目录中，目前是存在项目根目录下)

```js
// 1.安装
npm install html-webpack-plugin --save-dev

// 2.webpack.config.js中导入
const HtmlWebpackPlugin = require('html-webpack-plugin')

// 3.配置webpack.config.js
 plugins: [
        new VueLoaderPlugin(),   //15版本需指定plugin
        new webpacke.BannerPlugin("nice!nice!nice!nice!nice!"),
        // 将html-web-pack-plugin降级为^2.0.0，否则会报错
        new HtmlWebpackPlugin({
            // 将index作为模板
            template: 'index.html'
        })
    ]
// 4.webpack.json中将html-webpack-plugin降级为^2.0.0，然后执行npm install
 "html-webpack-plugin": "^2.0.0",
     
// 5.修改index.html将script删除，因为会自动生成
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>

<body>
<div id="app"></div>

</body>
</html>

// 6.将原先配置的publicPath注释，在output中
publicPath: 'dist/'

// 7.打包
npm run build
```

##### 19.压缩js的插件

```js
// 1.安装插件
npm install uglifyjs-webpack-plugin@1.1.1 --save-dev

// 2.webpack.config.js中配置
const uglifyjsWebpackPlugin = require('uglifyjs-webpack-plugin')

plugins: [
        new uglifyjsWebpackPlugin(),
    ]
```

##### 20.搭建本地服务器 webpack-dev-server

- 相当于热部署，我们的修改的文件会存在于内存中，只有npm run build才会写到磁盘上

```js
 // 1.安装
npm install --save-dev webpack-dev-server@2.9.3

// 2.webpack.config.json中配置dev-server
// 和plugins,moudle同一级目录配置
devServer:{
    // 服务于哪个文件夹,根目录下的dist文件夹
    contentBase: './dist',
    // 是否需要实时的监听
    inline: true,
    // 指定端口
    port: 9999,
    // 自动打开浏览器
    open:true,
    // 启动gzip压缩
    compress: true
}

// 3.在package.json中配置命令
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack",
    "dev": "webpack-dev-server"
  },
      
// 4.执行命令
npm run dev
```

##### 21.webpack配置文件分离

- 两个配置文件：
  - 1.开发时配置文件
  - 2.发布时配置文件

```js
// 1.在目录下新建build目录，所有的配置文件都写在里面

// 2.新建base.config.js,将webpack.config.js的内容拷贝进去，并且只留一些公共的东西
const path = require('path')
const VueLoaderPlugin = require('vue-loader/lib/plugin')
const webpacke = require('webpack')
const uglifyjsWebpackPlugin = require('uglifyjs-webpack-plugin')

const HtmlWebpackPlugin = require('html-webpack-plugin')

module.exports = {
    entry: './src/main.js',
    output: {
        // path必须是绝对路径，__dirname是node中的全局变量，用来获取当前文件所在的目录
        path: path.resolve(__dirname,'dist'),
        filename: 'bundle.js',
        // publicPath: 'dist/'
    },
    module: {
        rules: [
            {
                test: /\.css$/,
                // css-loader只负责加载，不负责解析和将样式应用到html中，所以需要使用style-loader
                use: [ 'style-loader','css-loader' ]
            },
            {
                test: /\.less$/,
                use: [{
                    loader: "style-loader" // creates style nodes from JS strings
                }, {
                    loader: "css-loader" // translates CSS into CommonJS
                }, {
                    loader: "less-loader" // compiles Less to CSS
                }]
            },
            {
                test: /\.(png|jpg|gif|jpeg)$/,
                use: [
                    {
                        loader: 'url-loader',
                        options: {
                            limit: 1000,
                            name: 'img/[name][hash:8].[ext]'
                        }
                    }
                ]
            },
            {
                test: /\.js$/,
                // 排除,这些东西不需要转化
                exclude: /(node_modules|bower_components)/,
                use: {
                    loader: 'babel-loader',
                    options: {
                        presets: ['es2015'],
                    }
                }
            },
            {

                test:/\.vue$/,
                use:['vue-loader']
            }
        ]
    },

    resolve:{
        alias:{
            'vue$':'vue/dist/vue.esm.js'
        },
        // 指定我们需要导入的时候，这些后缀不用写
        extensions: ['js','css','vue']
    },

    plugins: [
        new uglifyjsWebpackPlugin(),
        new VueLoaderPlugin(),   //15版本需指定plugin
        new webpacke.BannerPlugin("nice!nice!nice!nice!nice!"),
        // 将html-web-pack-plugin降级为^2.0.0，否则会报错
        new HtmlWebpackPlugin({
            template: 'index.html'
        })
    ],

    // devServer:{
    //     // 服务于哪个文件夹,根目录下的dist文件夹
    //     contentBase: './dist',
    //     // 是否需要实时的监听
    //     inline: true,
    // }
}

// 3.安装webpack-merge
npm install webpack-merge --save-dev

// 4.新建dev.config.js
// 1) .导入webpack-merge
const webpackeMerge = require('webpack-merge')

// 2).导入base.config
const baseConfig = require('./base.config')

// 3).使用webpackMerge将baseConfig和以下的配置合并
module.exports = webpackeMerge(baseConfig,{
    devServer:{
        // 服务于哪个文件夹,根目录下的dist文件夹
        contentBase: './dist',
        // 是否需要实时的监听
        inline: true,
    }
})


// 5.新建prod.config.js文件
const uglifyjsWebpackPlugin = require('uglifyjs-webpack-plugin')
const webpackeMerge = require('webpack-merge')
const baseConfig = require('./base.config')
module.exports = webpackeMerge(baseConfig,{
    plugins: [
        new uglifyjsWebpackPlugin(),
    ],
})

// 6.修改脚本，指定命令的配置文件
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack --config ./build/prod.config.js",
    "dev": "webpack-dev-server --open --config ./build/dev.config.js"
  },

// 7.修改output的path
path: path.resolve(__dirname,'../dist'),

// 8.执行build命令
npm run build
```















