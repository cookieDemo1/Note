### webapck

##### 1.指令

```bash
# 开发环境,index为入口文件
webpack ./src/index.js -o ./build/buit.js --mode=development

# 生产环境,比开发环境多一个压缩
webpack ./src/index.js -o ./build/buit.js --mode=production
```

##### 2.webpack.config.js和src下的js的区别

```js
// 构建工具都是基于node.js平台运行的，模块化采用common.js，所以
// package.config.js用的是common.js的语法
// src下的自己写的js文件则是用的ES6语法
```

##### 3.打包其他资源

- 即打包一些svg图片，icon图标库等资源

```js
npm install file-loader
// 使用file-loader,exclude要排除打包的资源
// 名字太长了，取10位hash值，加上原来的扩展名
{
    exclude:/\.(css|js|html|less|vue|scss)$/,
    loader: 'file-loader'
    options:{
        name: '[hash:10].[ext]'
    }
}
```

##### 4.开发环境配置

- 即将所有的loader dev-server 配置完整

```js
// 1.需要安装的loader
npm install style-loader css-loader less-loader -D
npm install html-webpack-plugin -D
npm install url-loader file-loader -D
npm i html-loader -D
npm install webpack-dev-server -D
npm install --save-dev webpack		
```

```js
/** 开发环境的配置，能让代码运行起来 */
const {resolve} = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')
module.exports = {
  entry: './src/index.js',
  output:{
    filename: 'built.js',
    path: resolve(__dirname, 'build')
  },
  mode: 'development',
  module:{
    // module配置
    rules:[
      // 处理less资源
      {
        test: '/\.less$/',
        use:['style-loader','css-loader','less-loader']
      },
      // 处理css资源
      {
        test:'/\.css$/',
        use:['style-loader','css-loader']
      },
      // 只能处理css中的图片资源
      {
        test: '/\.(jpg|png|gif)$/',
        loader: 'ulr-loader',
        options: {
          limit: 8 * 1024,
          name: '[hash:10].[ext]',
          // 关闭esModule,开启commonjsModule
          // 因为url-loader是使用es中的导入方式，而html-loader是采用common.js的导入方式
          // 为了让两者统一，所以需要关闭esModule，开启common.js
          esModule: false
        }
      },
      // 处理html中的图片资源
      {
        test: /\.html$/,
        loader: 'html-loader'
      },
      // 处理其他资源,例如图标，字体等资源
      {
        exclude: '/\.(html|js|css|less|jpg|png|gif)$/',
        loader: 'file-loader',
        options: {
          name: '[hash:10].[ext]'
        }
      },
    ]

  },

  plugins: [
    // 打包html文件的插件
    new HtmlWebpackPlugin({template: './src/index.html'})
  ],

  devServer:{
    contentBase: resolve(__dirname, 'build'),
    compress: true,
    port: 3000,
    open: true
  }
}

// 运行配置
"build": "webpack",
"dev": "webpack-dev-server"
```

##### 5.提取css成单独文件(css文件不打包到built.js中)

- 使用MiniCssExtractPlugin：使用这个插件可以将js的css文件提取出来，并且index.html会自动引用

```js
// 1.安装插件
npm i mini-css-extract-plugin

// 2.导入
const MiniCssExtractPlugin = require('mini-css-extract-plugin')

// 3.plugin中创建实例
plugins:[
    new MiniCssExtractPlugin = new MiniCssExtractPlugin()
]

// 4. module下的rules中，将MiniCssExtractPlugin.loader取代style-loader
{
    test: /\.css$/,
    // MiniCssExctractPlugin.loader将js中的css提取成单独文件
    use: [MiniCssExtractPlugin.loader, 'css-loader']
}
```

##### 6.压缩css

```js
// 1.下载插件
npm i optimize-css-assets-webpack-plugin

// 2.引用
const = OptimizeCssAssetsWebpackPlugin = require('optimize-css-assets-webpack-plugin')

// 3.plugins中创建
plugins:[
    new OptimizeCssAssetsWebpackPlugin()
]
```

##### 7.js压缩和html压缩

- 生产模式下，会自动压缩js代码。而我们只需要压缩html即可
- webpack.config.js中配置mode: 'production' 即是生产模式

```js
// html压缩
new HtmlWebpackPlugin({
    template: './src/index.html',
    // 移除空格
    collapseWhitespace: true,
    // 移除注释
    removeComments: true
})
```

##### 8.devServer开启热模块替换，当修改一个文件，只对一个文件重新打包。而不会打重新打包所有模块

```js
devServer: {
	contentBase: resolve(__dirname, 'build')
	// 开启HRM功能
	hot: true
}
```

##### 9.

```

```



