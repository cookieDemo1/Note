###### 1安装依赖

```bash
mkdir learnBabel

cd learnBabel

npm init -y

npm install --save-dev @babel/core @babel/cli @babel/preset-env
npm install --save-dev @babel/polyfill
```

###### 2.配置文件

```json
// 项目根目录下创建babel.config.json配置文件
{
  "presets": [
    [
      "@babel/env",
      {
        "targets": {
          "edge": "17",
          "firefox": "60",
          "chrome": "67",
          "safari": "11.1",
          "ie": "8"
        },
        "useBuiltIns": "usage",
        "corejs": "3.6.5"
      }
    ]
  ]
}
```

###### 3.执行命令

```bash
# node_modules\.bin\babel src --out-dir lib的缩写
npx babel src --out-dir lib
```

###### 4.babelrc和babel.config.js的区别

```js
// .babelrc只会影响本项目中的代码
// babel.config.js会影响整个项目中的代码，包含node_modules中的代码

// 推荐使用babel.config.js(babel.config.js和bable.config.json类似。)
// babel.config.js可以使用一些Node的API 
```

