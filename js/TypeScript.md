###### 1.安装

```bash
# 安装
npm install typescript -g
tsc -v

# 编译成ES5
tsc hello.ts

# 自动编译，一下命令生成tsconfig.json
tsc --init

# 修改tscconfig.json
"outDir": "./js",

# 点击
终端 -> 运行任务 -> typescript -> 监视tsconfig.json
```

###### 2.数据类型

```js
// 定义变量必须指定类型
var flag:boolean = true
flag = false

// 数字类型
var n:number = 123

// 字符串
var str:string = "123"

// 数组定义方式1
let arr:number[] = [1,2,3,4,5]
let arr2:string[] = ["php","java","python"]

// 泛型定义数组
let arr3:Array<number> = [11,22,33,44,55]
console.log(arr3);

// 元组类型:元组会被编译成数组,元组可以指定数据的类型
let arr4:[string,number,boolean] = ["ts",1.23,false]
console.log(arr4);

// 枚举类型,0失败  1成功
enum Flag{success=1, error=0}
console.log(Flag.success)

// red为第一个,没有赋值,默认为1,orange也没有值,它在上一个的基础上加1,为6
enum Color{red, blue=5, orange}
console.log(Color.red, Color.orange);
```

###### 3特殊的数据类型

```js
"use strict";
// 任意类型any
var num = 123;
num = "str";
console.log(num);
// any类型的用法
var box = document.getElementById("box");
box.style.color = "red";
// typescript新版已经有了Object类型
var a = { name: "hcp", age: 123 };
var box = document.getElementById("box");
// null 和 undefined是其他类型的子类型,其他(never类型)
// undefined和js一致
var num2;
console.log(num2);
//  我们可以这样声明,是number或者undefined
var num3;
// null空类型
var num4;
// 一个元素可能是number类型,可能是null,可能是undefined. 设置成any更方便
// 如果没有给变量赋值,那么它就是undefined
var num5;
// void没有任何类型,一般用在定义方法的时候,方法没有返回值
function run() {
    console.log('run');
}
// never类型:是其他类型(包括null和undefined)的子类型,代表从不会出现的值
// 这意味着声明never的变量只能被never类型赋值
var x;
// nerver从不会出现的值,可以把它赋值给一个匿名自执行函数,并且它抛出一个异常
x = (function () {
    throw new Error("抛出异常");
})();

```

###### 函数

```typescript
// 1.同一个文件夹下的不同文件中的函数名不能重复
function run2():string{
  return "Nice"
}

// 2.匿名函数定义方法
var fun2 = function():number{
  return 1232
}

console.log(fun2());
 
// 3.有参数的方法
function getInfo(name:string, age:number):string{
  return `${name}  ${age}`
}
console.log(getInfo("hadoop",12));

// 4.没有参数的方法
function run3():void{
  console.log("renturn void");
}

// 5.方法可选参数  age?:number  代表age可传可不传
// 可选参数必须放在参数的最后面
function getInfo2(name:string, age?:number):string{
  if(age){
    return `${name} ${age}`
  }else{
    return `${name}`
  }
}

// 6.默认参数,默认参数也必须放在最后面
function getInfo3(name:string, age:number=20):string{
  return `${name} ${age}`
}

getInfo3("java")

// 7.剩余参数
function sum(...result:number[]):number{
  var sum = 0
  result.forEach(item=>{
    sum += item
  })
  return sum
}


// 8.函数重载
function css(name:string):string;

function css(age:number):number;

function css(str:any):any{
  return str
}

// 9.箭头函数
setTimeout(()=>{},100)
```

###### 类

```typescript
// 1.ts中定义类
class Person{
  private name:string       // 定义属性,前面省略了public
  // 构造函数
  constructor(name:string){
    this.name = name
  }

  // get方法
  getName():string{
    return this.name
  }

  // set方法
  setName(name:string):void{
    this.name = name
  }


}

let p:Person = new Person("hadoop")
console.log(p.getName());

// 2.继承
class Child extends Person{
  // 调用父类构造
  constructor(name:string){
    super(name)
  }
}

// 子类和父类有相同的方法,会执行子类中的方法
let c = new Child("child")
console.log(c.getName())

// typescript定义属性的时候,给我们提供了三种修饰符
// public       公有:类和子类和类外部可以访问(默认)
// protected    保护:类和子类可以访问
// private      私有:类里面访问,子类没法访问
```

###### 静态方法

```typescript
class Animal{

  // 静态属性,可以通过类访问,可以通过静态方法访问
  static init_arg:string = "Animal"
  public name:string
  
  constructor(name:string){
    this.name = name
  }

  // 静态方法,通过类名调用
  // 静态方法,只能使用静态属性
  static init(){
    console.log("init....",this.init_arg);
    
  }
}

Animal.init()
```

###### 多态

```typescript
// 多态:父类定义一个方法不去实现,让继承它的子类去实现

class Animal1{
  public name:string
  constructor(name:string){
    this.name = name
  }

  eat(){}

}

// 这里相当于重写
class Dog extends Animal1{
  constructor(name:string){
    super(name)
  }
  eat(){
    console.log(`${this.name}吃狗粮....`)
  }
}

let d = new Dog("Tom")
d.eat()
```

###### 抽象类

```typescript
abstract class A {
  public name:string
  constructor(name:string){
    this.name = name
  }
  abstract eat():any;

}

// 必须实现抽象类中的方法
class B extends A{
  constructor(name:string){
    super(name)
  }
  eat(){
    console.log("实现抽象类中的方法....");
  }
}

let b:B = new B("B")
b.eat()
```

###### 属性接口

```typescript
// 属性接口,对Json的约束,要求传入的labelInfor必须是一个对象,并且必须有label属性
function printLabel(labelInfo:{'label':string}):void{
  console.log(labelInfo);
}
// 不传有label属性的对象则会报错
printLabel({"label":'Nice'})
```

###### 对批量方法进行约束

```typescript
// interface对批量方法进行约束
interface FullNamn{
  // 注意,分号结束
  firstName: string;
  lastName: string;
}

// 要求必须传入一个对象,且有firstName和lastName属性
// 直接让name属性私信啊FullName接口
function printName(name:FullNamn){
  console.log(name.firstName);
  console.log(name.lastName);  
}

// 这样传,只能有firstName和lastName两个属性,将对象提出去,可以传其他数据,但是不能使用
printName({firstName:'Hello',lastName: "Word"})


// ============ 可选接口 ============
interface FullNamn{
  firstName: string;
  // ?:为接口的可选属性,这个接口可传可不传
  lastName?: string;
}
```

###### 函数类型接口

```typescript
// 函数类型接口:对方法传入的参数以及返回值进行约束

// 函数类型接口,传入的值必须是两个string,返回值也必须是string 
interface encrypt{
  (key:string, value:string):string;
}

// 使用函数类型接口
var md5:encrypt = function(key:string, value:string){
  return key+value
}

var s:string = md5("123","456")
console.log(s);
```

###### 类类型接口

```typescript
// 类类型接口[对类的约束] 和抽象类有点相似
// 和java的接口类似,继承这个类,必须有string的name属性和eat方法(必须有一个string参数,且没有返回值)
interface Animal4{
  name:string
  eat(str:string):void
}

class Fish implements Animal4{
  name: string
  constructor(name:string){
    this.name = name
  }
  eat(str:string):void{
    console.log("fish吃草....");
  }
}

let f:Fish = new Fish("Nice")
f.eat("草")
```

###### 接口继承

```typescript
// 1定义接口
interface ADC{
  eat():void;
}

// 2继承接口
interface Xia extends ADC{
  work():void;
}

// 3.实现Xia接口,还需要实现其父类中的抽象方法
class Ren implements Xia{
  eat():void{
    console.log("eat....");
  }
  work():void{
    console.log("work");
  }
}
```

###### 泛型

```typescript
// 这里的泛型和java中的类型有点相似

// 实现一个传入number必须返回number类型,传入string类型,必须返回string类型
// 第一个T代表传入的类型,第二个T代表参数也是这种类型,第三个T代表返回值也是这种类型
function getData<T>(value:T):T{
  return value
}

// 调用的时候指定泛型的类型

let n3:number = getData<number>(2)
console.log(n3);
```

###### 泛型类

```typescript
// 类泛型
class MinClass<T>{
  // 数组里面的类型是泛型
  public list:T[] = []
  // add的参数是泛型
  add(value:T):void{
    this.list.push(value)
  }
}

// 实例化类,比执行泛型
let min = new MinClass<number>()
min.add(3)
min.add(3)
min.add(3)
console.log(min.list);
```

######  将类作为泛型

```typescript
/**
 * 定义一个User类,作用是映射数据库字段
 * 然后定义一个MysqlDB这个类用于操作数据库
 * 然后把User类作为参数传入到MysqlDB中
 */

class User{
  // 如果不写|undefined会报错，ts担心我们不给usename赋值
  username:string | undefined
  password:string | undefined
}

class MysqlDB<T>{
  // add必须要接受User对象
  add(user:T):boolean{
    console.log("add方法.....");
    console.log(user);
    return true
  }
}

var u = new User();
u.username = "zs"
u.password = "123456"

// 将自定义类作为泛型的类型
var my_db = new MysqlDB<User>()
my_db.add(u)
```

###### typescript操作数据库接口

```typescript
// 定义一个操作数据库的库 支持MySQL MongoDB
interface DBI<T>{
  add(info:T):boolean
  update(info:T, id:number):boolean
  delete(id:number):boolean
  get(id:number):any[]
}

// 定义一个操作Mysql数据库的类
class MysqlDB2<T> implements DBI<T>{
  add(info: any): boolean {
    console.log(info);
    return true
    
    throw new Error("Method not implemented.");
  }
  update(info: any, id: number): boolean {
    throw new Error("Method not implemented.");
  }
  delete(id: number): boolean {
    throw new Error("Method not implemented.");
  }
  get(id: number): any[] {
    throw new Error("Method not implemented.");
  }
}

// 定义一个操作msSql数据库的类
class MsSqlDB<T>implements DBI<T>{
  add(info: T): boolean {
    console.log(info)
    return true
    throw new Error("Method not implemented.");
  }
  update(info: T, id: number): boolean {
    throw new Error("Method not implemented.");
  }
  delete(id: number): boolean {
    throw new Error("Method not implemented.");
  }
  get(id: number): any[] {
    throw new Error("Method not implemented.");
  }
}

// 定义一个Duck类,作为数据库中对应的实体类
class Duck{
  username:string | undefined
  password:string | undefined
}

// 实例化实体类
let duck = new Duck()
duck.password = "123"
duck.username = "456"

// 创建MysqlDB类,泛型为Duck
let mysqldb2 = new MysqlDB2<Duck>()
mysqldb2.add(duck)

// 创建MsSql类,泛型为duck
let mssqldb = new MsSqlDB<Duck>()
mssqldb.add(duck)
```

###### typescript模块化 (即导入和导出)

```typescript
// 导出 modules/db.ts
export function getData():any[]{
  return [1,2,3,4]
}

// 导入,不用写后缀
import {getData} from './modules/db'
```

###### 也可以这样统一暴露

```typescript
export {func1, func2}

// 导入的时候,使用as可以使用别名
import {func1, func2 as fn} import './modules/db'
```

###### default导出,一个模块只能使用一次

```typescript
export default func():void{
  console.log("one")
}

// 导入export default的属性,不需要加{}
import func from './modules/db'
```

###### 命名空间

```typescript
// 两个命名空间中的变量和函数可以重复
// 命名空间中属性默认是私有的,我们需要export才可以给外部使用
namespace D{
  export class Person{
    name:string | undefined
    constructor(){
      this.name = "D Person"
    }
  }
}

namespace E{
  export class Person{
    name:string | undefined
    constructor(){
      this.name = "E Person"
    }
  }
}

// 使用名称空间中的东西,使用名称.
let p1 = new E.Person()
let p2 = new D.Person()

console.log(p1.name);
console.log(p2.name);
```

###### 名称空间也可以export

```typescript
// 导出名称空间
export namespace D{
  export class Person{
    name:string | undefined
    constructor(){
      this.name = "D Person"
    }
  }
}
  
// 导入名称空间并且使用,使用名称空间里的东西,也需要加名称.
import {D} from './namespace_test'
let p = new D.Person()
console.log(p.name)
```

###### 



