###### jQuery操作DOM 

```js
// 获取id然后设置或者移除class
$('#regapnarea').addClass('hide'); 
$('#regapnarea').removeClass('hide');

// 获取表单元素，并设置值
$('#apn_name').val(data.apn_name);
```

###### 只在函数内用到的私有变量可以使用下划线开头（约定俗称命名，并不是实际变成私有属性）

```js
_data = [1,2,3,4,5,6,7]
```

###### 三元表达式赋值

```js
// 判断data.state中有没有值，如果有值则赋值给#state元素的value为"1"，否则为赋值为"0"
$('#state').val(data.state ? "1" : "0");
```

###### 前端存储技术

```js
/**
	localStorage：用于长久保存整个网站的数据，保存的数据没有过期时间，知道手动去除。
	sessionStorage: 用于临时保存同意窗口的数据，在关闭窗口或标签页之后将会删除这些数据。
*/

// 不管是localStorage还是sessionStorage，可使用的API都相同，常用的有如下几个
保存数据： localStorage.setItem(key,value)
读取数据:  localStorage.getItem(key)
删除单个数据: localStorage.removeItem(key)
删除所有数据:	localStorage.clear()
得到某个索引的key: localStorage.key(index)
```

###### 解构赋值，将一个对象的属性赋值给另一个对象

```js
 var params = {
 	name: 12,
 	age: 12
 }

 var obj = {
 	add: 123,
 	...params
 }

 console.log(obj)
```

###### ES6定义函数

```js
class Person{
	// ES6定义函数
	sayHello(){
		console.log('hello word')
	}
}
// 创建对象，需要使用new关键字
var p = new Person()
p.sayHello()
```

