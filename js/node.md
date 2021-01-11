### node

##### 1.cnpm的使用

```js
// 1.安装,全局安装
npm install cnpm -g
```

###### 2.导入导出

```js
// 1.导出
let a = 20
let b = 30

// 导出a和b
module.exports = {
  a,
  b
}

// 2.导入
const {a,b} = require("./2导出")
console.log(a,b)
```

###### 3.同步读取文件

```js
// 1.导入fs模块
let fs = require('fs')

// 如果没有设置encoding,默认返回一个缓冲区
let content = fs.readFileSync("./hello.txt", {flag: 'r', encoding: 'utf-8'})

console.log(content)
```

###### 4.异步读取文件

```js
let fs = require('fs')

// readFile, 异步的方法一般都是回调函数的形式,err代表有没有错误, data代表数据
fs.readFile("./hello.txt", {flag: 'r', encoding: 'utf-8'}, (err, data)=>{
  // 如果有错误,打印错误
  if(err){
    console.log(err)
    // 没有错误打印数据
  }else{
    console.log(data)
  }
})

// 异步的方法都是最后执行的,所以这句会在回调函数之前执行
console.log("first")
```

###### 5.封装异步读取文件

```jsx
let fs = require('fs')

// 封装都是返回promise,传入的参数为文件路径
function fsRead(filepath){
  return new Promise((reslove, reject)=>{
    fs.readFile(filepath, {flag: 'r', encoding: 'utf-8'}, function(err,data){
      if(err){
        reject(err)
      }else{
        reslove(data)
      }
    })
  })
}

fsRead('./hello.txt').then(res => {
  console.log(res)
}).catch(err => {
  console.log(err)
})
```

###### 6.async和await方式读取文件

```js
let fs = require('fs')

// 封装都是返回promise,传入的参数为文件路径
function fsRead(filepath){
  return new Promise((reslove, reject)=>{
    fs.readFile(filepath, {flag: 'r', encoding: 'utf-8'}, function(err,data){
      if(err){
        reject(err)
      }else{
        reslove(data)
      }
    })
  })
}
// async和await组合的方式读取文件(相当于同步)
async function readList(){
  let file1 = await fsRead('hello.txt').then(res => res)
  let file2 = await fsRead('hello2.txt').then(res => res)
  let file3 = await fsRead('hello3.txt').then(res => res)
  console.log(file3)
}

readList()
```

###### 7.写入文件

```js
let fs = require('fs')

// flag: (w 覆盖写)  (a 追加写)
// arg1: 文件路径, arg2:写入的内容  arg3:option arg4:callback,参数有没有错误
fs.writeFile("./hello.txt", "晚饭吃啥?\r\n", {flag: 'a', encoding: 'utf-8'}, (err) => {
  if(err){
    console.log("写入内容失败")
    console.log(err)
  }else{
    console.log("写入内容成功")
  }
})
```

###### 8.封装

```js
let fs = require('fs')

// 封装写入
function  fsWrite(filename, content){
  return new Promise((reslove, reject) => {
    fs.writeFile(filename, content, {flag: 'a', encoding: 'utf-8'}, (err)=>{
      if(err){
        reject(err)
      }else{
        reslove("写入成功")
      }
    })
  })
}

// async封装成同步
async function writeList(){
  await fsWrite("hello.txt", "append1...\r\n").then(res => console.log(res)).catch(err => console.log(err))
  await fsWrite("hello.txt", "append2...\r\n").then(res => console.log(res)).catch(err => console.log(err))
  await fsWrite("hello.txt", "append3...\r\n").then(res => console.log(res)).catch(err => console.log(err))
}

writeList()
```

###### 9.删除文件

```js
let fs = require('fs')

// unlink删除文件
fs.unlink('./hello3.txt', function(){
  console.log("成功删除....");
})
```

###### 10.buffer(很少用得到)

```js
// 1.数组不能进行二进制数据的操作
// 2.js数组不像java等语言效率高
// 3.为了提升数组的性能而有了buffer
// 4.buffer会在内存空间开辟固定大小内存，buffer存储的是二进制数据

// 1).将字符串，存放到buffer缓冲区中
let buff = Buffer.from("hello word")
// 这里打印的是16进制数据
console.log(buff)

// 得到buffer中的字符串，使用toString方法
console.log(buff.toString())


// 2).空的缓冲区, Buffer已经被废弃掉了，取而代之的是Buffer.alloc,
// 1024个字节
let buff2 = new Buffer.alloc(1024)
buff2[0] = 255
console.log(buff2)
```

###### 11.读取目录

```js
let fs = require('fs')
// 读取目录有一个callback, 回调第一个参数为err,第二个参数为一个数组
fs.readdir('../1node基础', (err, files) => {
  if(err){
    console.log(err)
  }else{
    console.log(files)
  }
})
```

###### 12.rmdir删除目录

```js
let fs = require('fs')

fs.rmdir('./abc', ()=>{
  console.log("remove success....")
})
```

###### 13.输入   输出使用console.log( )

```js
// 1.导入readline包
let readline = require('readline')

// 2.实例化接口对象
let r1 = readline.createInterface({
  input: process.stdin,
  output: process.stdout
})

// 3.设置question提问事件, callback(arg) 参数为输入的内容
r1.question("今天中午吃什么:", (answer)=>{
  console.log(answer)
  // 关闭提问事件
  r1.close()
})

// 4.结束提问事件
r1.on("close", function(){
  process.exit(0)
})
```

###### 14.流的方式写入文件

```js
//  当文件很大的时候,就不能直接一次性打开文件,需要使用流
let fs = require('fs')

// 1.创建写入流对象
let ws = fs.createWriteStream('./stream.txt',{flags: 'a', encoding: 'utf8'})

// 监听文件打开事件,callback
ws.on('open', ()=>{
  console.log("文件被打开...")
})


// 监听文件关闭事件
ws.on('close', ()=>{
  console.log('文件关闭....')
})


// 2写入,有一个回调
ws.write("hello word\r\n", (err)=>{
  if(err){
    console.log(err)
  }else{
    console.log("内容写入完成...")
  }
})

// 3.写入结束,也有回调
ws.end(()=>console.log("文件写入关闭....."))
```

###### 15.流的方式读取数据

```js
let fs = require('fs')

// 1.创建读取流, 源是card.jpg
let rs = fs.createReadStream('./card.jpg', {flags: 'r'})

// 监听打开和关闭事件
rs.on('open', ()=>console.log('文件打开....'))
rs.on('close', ()=>console.log('文件关闭....'))

// 监听数据流的每一次流入(读取)
rs.on('data', (data) => {
  // data.length为65536,每次最大流入为65536字节
  console.log('单批数据流入...'+ data.length)
  console.log(data)
})
```

###### 16.读取并写入(相当于拷贝文件)

```js
let fs = require('fs')

// 1.创建读取流, 源是card.jpg
let rs = fs.createReadStream('./card.jpg', {flags: 'r'})

// 创建一个写入流,文件不存在则会新建
let ws = fs.createWriteStream('nice.jpg', {flags: 'w'})

// 监听打开和关闭事件
rs.on('open', ()=>console.log('文件打开....'))
rs.on('close', ()=>console.log('文件关闭....'))

// 监听数据流的每一次流入(读取)
rs.on('data', (data) => {
  console.log('单批数据流入...'+ data.length)
  console.log(data)

  // 将每一次读到的值写入,就相当于拷贝
  ws.write(data)
})
```

###### 17.管道流 ( 复制文件 )

```js
let fs = require('fs')

let rs = fs.createReadStream('./card.jpg', {flags: 'r'})

// 创建一个写入流,文件不存在则会新建
let ws = fs.createWriteStream('nice.jpg', {flags: 'w'})

// pipe直接将rs中的数据读取并写入到ws中 pipe管道流
rs.pipe(ws)
```

###### 18.夜神模拟器根目录运行这个,vscode中就可以flutter run

```js
D:\Program Files\Nox\bin>nox_adb.exe connect 127.0.0.1:62001
```



