###### 1.rest参数（rest参数必须放在参数的最后一个位置，默认参数也必须放在参数的最后一个位置）

```js
// ..values（三个点的为rest参数，用于获取函数多余的参数，把多余的参数放在数组中，values就指向这个数组）
function add(...values){
    let sum = 0
    for(var val of values){				// 数组可以通过for of进行遍历
        sum += val;
    }
    return sum
}

add(2, 5, 5)
```

### 箭头函数

###### 2.箭头函数只返回一个对象（只写一条语句），不能只写对象，需要用括号包裹

```js
// 返回一个对象，且用简写形式，需要在对象后面加小括号
let getItem = (id) => ({id:id, name: name})
```

###### 3.箭头函数的参数也可以写成解构的形式

```js
const full = ({first, last}) => first + '' + last
```

###### 4.箭头函数和rest参数结合    tip: 箭头函数没有自己的this，所以导致内部的this就是外层代码块的this，正式因为它没有this所以就不能用作构造函数

```js
// 将箭头函数的参数写成rest的形式
const numbers = (...nums) => nums
// 调用
numbers(1,2,3,4,5)
```

###### 5.箭头函数没有自己的this，所以不能用call(), apply(), bind()方法去改变this指向。

```js
// 可以通过外边套一层函数，来执行外层的函数this(这种方法并不实用)
function foo(){
    return ()=> {
        console.log(this)
    }
}
```

### 数组

###### 6.扩展运算符进行数组拷贝, 和数组合并

```json
const a1 = [1, 2, 3]
const a2 = [...a1]				// 数组拷贝
const a3 = [...a1, ...a2]		 // 数组合并
```

###### 7.扩展运算符可以将默写数据结构转换为数组

```js
// arguments对象
function foo(){
    const args = [...arguments]
}

// NoseList对象
[...document.querySelectAll('div')]
```

###### 8.Array.form(arg)将一个雷数组的对象，转换为真正的数组

```js
Array.form('hello')
Array.from(new Set([1,2]))

// Array.form可以接收第二个参数，作用类似于数组的map方法，用于对每个元素进行处理，将处理后的值放入返回的数组
Array.form(arrayLike, x => x*x)	
```

###### 9.Array.of 用于将一组值，转换为数组

```js
Arrat.of(3 ,11, 8)				// 返回[3, 11, 8]
```

###### 10.数组实例判断是否包含指定的值

```js
[1,2,3].includes(2)				// true
```

###### 11.flat将嵌套数组拉成扁平化, 即拉成一维数组

```js
[1,2,[3,4]].flat()				// [1,2,3,4]

[1,2,[[3,4],4,5]].flat(2)		// 拉平两层嵌套的数组
[1,2,[3,4]].flat(Infinity)		// 参数传递Infinity不管有多少层数组都拉成一维数组
```

###### 12.flatMap	拉平数组，只能拉平一个双层数组

```js
// faltMap参数 当前数组成员, [当前数组成员的位置], 原数组
[1,2,3,[3,4]].flatMap(x => x*2)
```

###### 13.sort排序

```js
[2,6,4,9,0].sort()
```

### 对象

###### 14. 对象的属性和方法的简洁写法

```js
const name = 'hadoop'
// 定义对象
const o ={
    name,					// 属性的简洁写法，相当于 name:hadooop			
    say(){					// 函数的简洁写法，相当于 say:function(){return 'hello'}
        return "hello"
    }
}
```

###### 15 . common.js导出一组变量

```js
let num1=1, num2=2, num3=3
module.exports = {num1, num2, num3}
```

###### 16. 属性名表达式

```js
let val = 'nice'
obj{
    ['a'+'ge']: 12,			// 等价于 'age': 12,
    [val]: 'perfect'		// 属性名还可以引用外部变量
}
obj['name'] = 'hadoop'		// 等价于 obj.name = 'hadoop'

console.log(obj['name'])	// 获取属性名的时候也可以用表达式
```

###### 17.对象解构赋值

- 对象的解构赋值的拷贝是浅拷贝，即如果一个键的值是复合类型的值（数组、对象、函数），那么解构赋值拷贝的是这个值得引用，而不是这个值得副本。
- 扩展运算符的解构赋值，不能复制继承自原型对象的属性

```js
// 解构赋值必须在最后一个位置， 如果解构的是null或者undefined则会报错，因为null和undefined无法转换为对象
let {x, y, ...z} = {x:1, y:2, a:3, b:4}
x	// 1
y	// 2
z	// {a:3, b:4}
```

###### 18.扩展运算符合并两个对象

```js
let a = {name: 'hadoop'}
let b = {age: 12}
let ab = {...a, ...b}
```

###### 19.扩展运算符增加对象属性

```js
let ap = {...a, ...{header: 'no', sex: '0'}}		
```

###### 20.创建对象的时候使用扩展运算符

```js
let newVersion = {
    ...a,
    name: 'new Name'				// 如果和a中的属性名相同，则会覆盖掉
}
```

###### 21.链判断运算符

```js
// 如果要读取对象内部某个属性，往往需要判断一下该对象是否存在

// 错误的写法
const firstName = message.body.user.firstName		

// 正确的写法
const firstName = (message
                  && message.body
                  && message.body.user
                  && message.body.user.firstName) || 'default';

// ES2020改进写法，链式运算符 (链判断运算符暂时无法使用)
const firstName = message?.body?.user?.firstName || "default"
```

###### 22.Null判断运算符

```js
const a = response.setting.header ?? 'default'

// ??有一个优先级的问题，它与 && || 的优先级孰高孰低，它们没有高低之分，使用的时候必须加括号
(a && b) ?? c
```

### 对象的新增方法

###### 23. Object.is

- == 有缺点，会自动转换类型
- === 有缺点， NaN不等于NaN, +0不等于-0

```js
Object.is('foo', 'foo')			 // true
Object.is({}, {})				// false
Object.is(+0, -0)				// false
Object.is(NaN, NaN)				// true
```

###### 24. Object.assign 用于对象的合并，将源对象的所有可枚举属性，复制到目标对象

```js
const target = {a:1}
const source1 = {b:2}
const source2 = {c:3}
// arg1是源对象， arg2,arg3...等是目标对象。目标对象的可枚举属性将被拷贝到目标对象中
Object.assign(target, source1, source2)

target  	// {a:1, b:2, c:3}

// Object.assign()方法实行的是浅拷贝
const obj1 = {a:{b:1}}
const obj2 = Object.assign({}, obj1)			// 浅拷贝，嵌套的对象拷贝的是地址
```

###### 26. Object.values()	返回一个数组，成员是参数对象自身的（不含继承的）多有可遍历属性的键值

```js
const obj = {foo: 'bar', baz: 12}
Object.values(obj) 						// ["bar", 24] 返回数组成员的顺序，是按照对应的key来排序的
```

###### 27.Object.entries()  返回一个数组，成员是参数对象自身（不含继承的）的所有可遍历属性的键值对数组

```js
const obj = {a: 1, b: 2}
let arr = Object.entries(obj)
arr				// [['a', 1], ['b', 2]]

// entries用法1，遍历
for(let [k,v] of Object.entries(obj)){
    console.log(k,v)
}

// entries用法2，将对象转换为真正的map结构
const map = new Map(Object.entries(obj))
map			// Map {a: 1, b: 2}
```

###### 28.Object.fromEntries() 是Object.entries的逆操作，用于将一个键值对数组转为对象

```js
// 例1：将键值数组转换为对象
Object.fromEntries([			
    ['foo', 'baz'],
    ['baz', 42]
])							// {foo: 'bar', baz: 42}


// 例2：将ma转换为对象
const map = new Map().set('foo', true).set('bar', false)
Object.fromEntries(map)		  // {foo: 'bar', baz: 'qux'}

```

### Symbol

###### 29. symbol使用

```js
// ES6引入了一种新的原始数据类型Symbol,表示独一无二的值，用来作为对象属性名。因为ES5的对象属性名都是字符串，这容易造成属性名的冲突

// symbol通过Symbol函数生成
let mySymbol = Symbol()

// 第一种写法：Simbol的使用，作为对象属性名，symbol放在方括号内，不放则会被当成字符串
let a = {}
a[mySymbol] = 'hello'

// 第二种写法：simbol作为属性名方式2, symbol必须放在方括号内，不放则会被当成普通的字符串
let a = {
    [mySymbol]: 'hello !'
}

// 第三种写法
let a= {}
Object.defineProperty(a, mySymbol, {value: 'hello !'})

// 取值
console.log(a[mySymbol])
```

###### 30. symbol用在switch中

```js
const COLOR_RED = Symbol()
const COLOR_GREEN = Symbol()

function getComplete(color){
    switch(color){
        case COLOR_RED:
            return COLOR_GREEN;
        case COLOR_GREEN:
            return COLOR_RED;
        default:
            throw new Error('undefined color')
    }
}
```

###### 31.js中0.5可以简写为.5

```js
const area = .5 * .5
```

### set

###### 32. set去重

```js
// 数组去重
let arr = [1,1,2,3,4,4,5]
arr = [... new Set(arr)]

// 字符串去重
[...new Set('abbcc').join('')]
```

###### 33. set方法

```js
let s = new Set()

s.add(1)			// add添加一个数据
s.size				// size返回set的大小
s.delete(1)			// 删除某个值，返回一个布尔值，表示删除是否成功
s.has(1)			// 返回一个布尔值，表示该值是否为Set的成员
s.clear()			// 清除所有成员，没有返回值
```

###### 34. set的遍历方法

```js
// set的keys和values相同，都是值
keys()
values()	
entries()

// forEach方法
set.forEach(item => console.log(item))

// for of遍历
for (item of set){console.log(item)}
```

### Map

###### 35. map基本用法

```js
// 对象也相当于是键值对，为了解决对象的key值能使字符串的问题，引入了Map, Map的key可以是Object类型
const m = new Map()
const o = {name: 'hadoop'}
m.set(o, 'content')		 // 将对象o作为key，值'content'作为value存进map中
m.get(o)				// content

m.hash(o)				// 判断map中是否存在key为o的值
m.delete(o)				// 删除key为o的键值对
```

### 导入和导出

###### 36.导入

```js
import {addition as add, del} from './utils'			// 导入以及重命名

import nice from './hello'							  // export default导出的，可以这样导入					
export {firstName, lastName, year}					   // 导出多个
```

### promise

###### 37.promise会立即执行， then会在当前脚本所有代码执行之后执行

```js
let p = new Promise((reslove, reject) => {
	console.log('promise....')					// 1.立即执行
	reslove()
})

p.then(()=> {								  // 3. then最后执行
	console.log('then....')
})

console.log('脚本....')						 // 2.执行脚本中的其他代码

// ======================================================================

// 写成这种形式依然是then最后执行
let p = new Promise((reslove, reject) => {		
	console.log('promise....')
	reslove()
}).then(() =>{
	console.log('then....')						// 最后执行
})
console.log('脚本....')
```

###### 38. promise一般放在函数里面，然后该函数直接返回这个promise对象

```js
// 定义一个函数，返回promise
cosnt getJSON = function(url){
    const promise = new Promise((resolve, reject)=>{
        if(url === 'hello'){
            resolve('获取数据成功....')
        }else{
            reject('获取数据失败....')
        }
    })
    
    return promise;
}

// 调用这个函数会返回promise,然后可以直接then或者catch
getJson('hello').then(res => {
    console.log(res)
}).catch(err => {
    console.log(err)
})
```

###### 39. resolve或者reject并不会终结Promise的采纳数函数的执行

```js
new Promise((resolve, reject) => {
    resolve(1)				// resolve不会终结promise函数的执行，后面的语句仍然会执行
    console.log(2)			// 这里仍然会执行
})
```

###### 40. promise直接reject一个异常

```js
const promise = new Promise((resolve, reject) => {
    reject(new Error('me is error .....'))
})

promise().catch(err => console.log(err))

// 注意：如果没有指定catch()方法指定错误处理的回调函数，Promise对象抛出的错误不会传递到外层代码。即不会有任何反应。
```

###### 41.  finally用于指定不管Promise对象最后状态如何，都会执行的操作， ES2018引入的finally方法

```js
// 在执行完then或catch指定的回调函数以后，都会执行finally方法指定的回调函数
promise.then().catch().finally(console.log('finally方法....'))
```

###### 42. Promise.all([p1, p2 ,p3])

```js
const p = Promise.all([p1,p2,p3])

/** 
p的状态由p1,p2,p3决定，分为两种情况
	1: 只有p1,p2,p3的状态都为fufilled, p的状态才会变成fulfilled,此时p1,p2,p3的返回值组成一个数组，传递给p的回调。
	2：只要p1,p2,p3之中有一个被rejected，p的状态就变成rejected,此时第一个被rejecte的实例的返回值，会传递给p的回调函数
*/

// 1.创建一个promise数组
let promises = [1,2,3,4,5].map(item => {
	return new Promise((resolve, reject) => {
		resolve(item)
	})
})

// 2.将promise数组传递进Prmise.all() 
// Promise.all()不能再前面加 new 关键字
let promise_all = Promise.all(promises)
promise_all.then(res => console.log(res))
```

### Generator函数

###### 1.基础使用

```js
// 生成器函数
function* hellowordGenerator(){
	yield 'hello'
	yield 'word'
	return 'ending'
}

let hw = hellowordGenerator()

console.log(hw.next())		// { value: 'hello', done: false }
console.log(hw.next())		// { value: 'word', done: false }
console.log(hw.next())		// { value: 'ending', done: true }
console.log(hw.next())		// { value: undefined, done: true }
```

###### 2. generato函数可以不用yield表达式，这时就变成了一个单纯的暂缓执行函数

```js
// 没有 yield 变成一个暂缓执行函数
// yield 必须写在生成器函数内部
function* f(){
    console.log('执行了！')
}
// 不会执行，需要next()才会执行
let generator = f()

setTimeout(function(){
    generator.next()
}, 2000)
```

###### 3. Generator例子2

```js
function* gen(){
    for(let i=0; true; i++){
		yield i
    }
}

let g = gen()
g.next()
g.next()
g.next()
```

###### 4. for of循环

```js
function * foo(){
    yield 1
    yield 2
    yield 3
    yield 4
    yield 5
    yield 6
    return 7	// 使用for of循环， return的值获取不到。因为一判断done属性为true时循环就会终止
}

for (let v of foo()){
    console.log(v)
}
```

###### 合并对象

```js
let o1 = {name:"hcp", age: 12}

let o2 = {name: "hadoop", address:"广东省河源市龙川县"}

// 如果o2和o1中有重复的属性，那么将取o2中的属性
let o3 = {...o1, ...o2}
console.log(o3)
```



