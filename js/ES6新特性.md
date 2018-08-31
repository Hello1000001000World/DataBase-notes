## 变量

### let

####作用

类似于var，用来声明变量，但是所声明的变量，只在*let*命令所在的代码块内,**也就是{}内**有效。

```js
if(true){
    var a = 1;
    let b = 2;
}
document.write(a);
document.write(b);   //报错
```

#### 应用场景

for循环中



## 常量

###const

####作用

定义一个常量  一旦声明，值将是不可变的。

#### 语法

 具有块级{}作用域,

```js
if (true) {
  const max = 5;
}
document.write(max);  //报错,具有块级{}作用域
```

先声明后使用

```js
  document.write(MAX); // 报错
  const MAX = 5;
```

不可与变量同名

```js
var message = "Hello!";
let message = "hi!";
const message = "Goodbye!"; //报错
```



## 字符串

### 是否包含指定字符串

####includes()

返回布尔值，表示是否包含了参数字符串。

#### startsWith()

返回布尔值，表示参数字符串是否在源字符串的头部。

####endsWith()

返回布尔值，表示参数字符串是否在源字符串的尾部。

```js
var str = "Hello world!";
str.startsWith("Hello") // true
str.endsWith("!") // true
str.includes("o") // true
```

支持第二个参数，表示开始搜索的位置。

```js
var str = "Hello world!";
str.startsWith("world", 6) // true
str.endsWith("Hello", 5) // true
str.includes("Hello", 6) // false
```



### 重复输出字符串

####repeat()

```js
var str = "x";
str.repeat(3) // 重复输出三次str,结果为'xxx' 
```



### 模板字符串``

#### 支持插值${}

```js
let first = 'world';
let last = '!';
document.write(`Hello ${first} ${last}!`);
```

#### 可以多行

#### 标签模板tag

模板字符串前面有一个标识名*tag*，它是一个函数。



### 原始字符串

####String.raw()

*反斜线*也不再是特殊字符，*\n* 也不会被解释成换行符



## 数值

### 是否有穷,是否NaN

新提供了*Number.isFinite()*和*Number.isNaN()*两个方法，用来检查*Infinite*和*NaN*这两个特殊值。

```js
Number.isNaN(NaN); // true
Number.isNaN(15); // false
Number.isNaN("15"); // false
Number.isNaN(true); // false

Number.isFinite(15); // true
Number.isFinite(0.8); // true
Number.isFinite(NaN); // false
Number.isFinite(Infinity); // false
Number.isFinite(-Infinity); // false
Number.isFinite("foo"); // false
Number.isFinite("15"); // false
Number.isFinite(true); // false
```

###是否整数

Number.isInteger()  

```js
Number.isInteger(25) // true
Number.isInteger(25.0) // true
```

### Math对象

####去除小数

Math.trunc(-4.1)  //-4

空值或无法截取,返回NaN

####判断正负0

Math.sign()

返回五种值：参数为*正数*，返回*+1*；参数为*负数*，返回*-1*；参数为*0*，返回*0*；参数为*-0*，返回*-0*;*其他值*，返回*NaN*。

#### 计算立方根

`Math.cbrt(2);  // 1.2599210498948732`

####返回float类型

```js
Math.fround(0);     // 0
Math.fround(1.337); // 1.3370000123977661
```

#### 返回所有参数的平方和的平方根

```js
Math.hypot(3, 4);        // 5
Math.hypot(3, 4, 5);     // 7.0710678118654755
```



## 数组

###非数组转为数组

任何有*length*属性的对象，都可以通过*Array.from*方法转为*数组*

```js
let array = Array.from({ 0: "a", 1: "b", 2: "c", length: 3 });
document.write(array);    // [ "a", "b" , "c" ]
```

### 将一组数转为数组

`Array.of(3, 11, 8)     // [3,11,8]`

### 找出数组第一个匹配的值

arr.find(回调)

#### 示例

```js
let array = [1, 5, 10, 15].find(function(value, index, arr) {
  return value > 9;    //返回值为10
}) 
//回调参数:当前的值,当前的数组下标,原数组
```

#### 返回下标

arr.findIndex(回调) 参数同上

### 填充数组

arr.fill(参数1)

将数组全部替换为指定值

```js
let newArr = ['a', 'b', 'c'].fill(7, 1, 2)    //参数2,3为填充的起始下标和结束下标
document.write(newArr);   // ['a', 7, 'c']
```

### 遍历数组

#### 三个方法

- entries()  //键值对遍历
- keys()       //键名遍历
- values()   //键值遍历

#### 示例

```js
var arr = ['a','b']

for(let index of arr.keys()){
    document.write(index);    //0 1
}

for(let elem of arr.values()){
    document.write(elem);    //'a' 'b'
}

for(let [index,elem] of arr.entries()){
    document.write(index,elem);    //0 'a'      1 'b'
}
```



## 对象 

### 表达式作属性名或方法名

#### 示例

```js
obj['a'+'bc'] = 123;   //等同于obj.abc=123

let obj = {
  ['h'+'ello']() {
    return 'hi';
  }
};
document.write(obj.hello()); // hi
```

### 严格相等新方法

Object.is(参数一,参数二)

与===基本一致,2个不同:+0不等于-0,NaN等于NaN

###对象属性复制到另一对象

Object.assign(目标对象,源对象1,源对象2...)

对象属性若有重复,以最右边对象为准

#### 示例

```js
var target = { a: 1, b: 1 };   //需先定义
var source1 = { b: 2, c: 2 };
var source2 = { c: 3 };
Object.assign(target, source1, source2);
target // {a:1, b:2, c:3}
```

### __proto__属性

前后两个下划线__proto 用来读取和设置当前对象的**prototype**对象

`obj.__proto__ = someOtherObj;`

不要使用这个属性，而是使用下面的`Object.setPrototypeOf()`（写操作）、`Object.getPrototypeOf()`（读操作）、`Object.create()`（生成操作）代替。

参考:http://es6.ruanyifeng.com/#docs/object#__proto__属性，Object-setPrototypeOf，Object-getPrototypeOf



##函数

### 指定参数默认值

示例

```js
function sayHello(name){
    var name = name||'world';  //传统方法
    document.write('Hello '+name);
}
 
//运用ES6的默认参数
function sayHello2(name='world'){
    document.write(`Hello ${name}`);
}
sayHello();  //输出：Hello world
sayHello('!');  //输出：Hello !
sayHello2();  //输出：Hello world
sayHello2('!');  //输出：Hello !
```



### 参数个数不确定

#### 语法

三个点+数组名: **function fun(...数组名) {}**

#### 示例

```js
function add(...values) {
   let sum = 0;
   for (var val of values) {
      sum += val;
   }
   return sum;
}
add(1, 2, 3) // 6
```

###拓展运算符

#### 作用

将数组展开

#### 示例

```js
var people=['张三','李四','王五'];
 
function sayHello(people1,people2,people3){
    document.write(`Hello ${people1},${people2},${people3}`);
}
sayHello(...people);   //输出：Hello 张三,李四,王五 
//相当于sayHello(people[0],people[1],people[2]);
 
//而在以前，如果需要传递数组当参数，我们需要使用函数的apply方法
sayHello.apply(null,people);   //输出：Hello 张三,李四,王五 
```



### 箭头函数

#### 用法

使用=>简写函数

#### 示例

```js
// 表达式体
var odds = evens.map(v => v + 1);
var nums = evens.map((v, i) => v + i);
var pairs = evens.map(v => ({even: v, odd: v + 1}));
// 语句体
nums.forEach(v => {
  if (v % 5 === 0)
    fives.push(v);
});
```

#### 注意

- 函数体内的*this*对象，绑定定义时所在的对象，而不是使用时所在的对象。
- 不可以当作构造函数，也就是说，不可以使用*new*命令，否则会抛出一个错误。
- 不可以使用*arguments*对象，该对象在函数体内不存在。



###尾调用

#### 作用

节省内存

#### 用法

函数return的时候调用另一个函数

```js
function f(x){
  return g(x);  //只有这种写法属于尾调用,其他都不是
}
```



## 集合

### Set

####作用

数据结构类似于数组,但成员的值是**唯一**的,可以用来去除数组中的重复值  *NaN不等于NaN,{}不等于{},1不等于"1"*

####实例

`let set = new Set();`     //可以接受一个数组作为参数

#### 属性和方法

属性	

- `Set.prototype.constructor`：构造函数，默认就是`Set`函数。
- `Set.prototype.size`：返回`Set`实例的成员总数。

方法

- `add(value)`：添加某个值，返回 Set 结构本身。
- `delete(value)`：删除某个值，返回一个布尔值，表示删除是否成功。
- `has(value)`：返回一个布尔值，表示该值是否为`Set`的成员。
- `clear()`：清除所有成员，没有返回值。

**Array.from方法可以将Set结构转为数组**

#### 遍历

同数组遍历类似,由于没有键值一致,所以keys()与values()一样

- *keys()*：返回一个键名的遍历器
- *values()*：返回一个键值的遍历器
- *entries()*：返回一个键值对的遍历器
- *forEach()*：使用回调函数遍历每个成员



###WeakSet

####只能存对象的集合

WeakSet结构有以下三个方法。

- *WeakSet.prototype.add(value)*：向WeakSet实例添加一个新成员。
- *WeakSet.prototype.delete(value)*：清除WeakSet实例的指定成员。
- *WeakSet.prototype.has(value)*：返回一个布尔值，表示某个值是否在

WeakSet没有*size*属性，没有办法遍历它的成员。



### Map

Map 是键值对的集合,是一个“超对象”，其键与值可以是任意类型（如：对象,函数）

#### 属性和方法

和Set类似

属性

- size：返回成员总数。

方法

- set(key, value)：设置一个键值对。
- get(key)：读取一个键。如果找不到*key*，返回*undefined*。
- has(key)：返回一个布尔值，表示某个键是否在Map数据结构中。
- delete(key)：删除某个键。
- clear()：清除所有成员。

遍历器

- keys()：返回键名的遍历器。
- values()：返回键值的遍历器。
- entries()：返回所有成员的遍历器。
- forEach():与数组forEach()类似

```js
map.forEach(function(value, key, map)) {
  document.write("Key: %s, Value: %s", key, value);
};
```

#### 快速转为数组

```js
let map = new Map([
  [1, 'one'],
  [2, 'two'],
  [3, 'three'],
]);
[...map.keys()]
// [1, 2, 3]
[...map.values()]
// ['one', 'two', 'three']
[...map.entries()]
// [[1,'one'], [2, 'two'], [3, 'three']]
[...map]
// [[1,'one'], [2, 'two'], [3, 'three']]
```



### WeakMap

####键名只接受对象

WeakMap与Map在API上的区别主要是两个:

- 一是没有遍历操作（即没有key()、values()和entries()方法），也没有size属性；
- 二是无法清空，即不支持clear方法。这与WeakMap的键不被计入引用、被垃圾回收机制忽略有关。

####方法

*get()*、*set()*、*has()*、*delete()*



## 遍历器

### 作用

为不同数据结构提供统一的访问接口



##Generator

Generator函数是一个普通函数，但是有两个特征。

- 一是，function命令与函数名之间有一个*星号*；
- 二是，函数体内部使用*yield*语句，定义遍历器的每个成员，即不同的内部状态。

```js
function* helloWorldGenerator() {
  yield 'hello';
  yield 'world';
  return 'ending';
}
```

```js
var a = new helloWorldGenerator();
a.next()  //返回Object    { value: 'hello', done: false }
a.next()  // { value: 'world', done: false }
a.next()  // { value: 'ending', done: true }
```



##Promise

### 解决的问题

回调函数嵌套过深，会使代码难以阅读

```js
const fs = require('fs')

fs.readFile('./data/a.txt', 'utf8', (err, dataA) => {
  fs.readFile('./data/b.txt', 'utf8', (err, dataB) => {
    fs.readFile('./data/c.txt', 'utf8', (err, dataC) => {
      fs.writeFile('./data/d.txt', dataA + dataB + dataC, err => {
        if (err) {
          throw err
        }
        console.log('success')
      })
    })
  })
})
```

### 基本用法

```js
var p = new Promise(function(resolve,reject){
	异步函数｛
		成功时：resolve（参数）
		失败时：reject｛参数｝
	｝
})
```

```js
p.then()  //可以传一个或两个参数，回调函数，分别是resolve()与reject（）

p.then(function(data){},function(err){})
//只会调用其中一个函数,不是成功就是失败
```

解决嵌套问题

```js
var p = new Promise(function(resolve,reject){}))
var p2 = new Promise(function(resolve,reject){}))
var p3 = new Promise(function(resolve,reject){}))

p.then(function(data){
    
    return p2;
})
.then()    //这一行就是执行完p的then函数，再执行p2的then函数，还能在括号后继续 .then()方法
```

## async

可以让一个方法变成异步方法

```js
function fun(){
	return "hahah"
}
async function fun1(){
	return "hahah"
}
console.log(fun())		//hahah
console.log(fun1())		//Promise { 'hahah' }
fun1().then(function(data){
	console.log(data)
})
```

## await

把异步操作变成同步

await必须用在异步方法里面

```js
//错误
async function fun(){
	return "hahah"
}
var f = await fun();
console.log(f)
//正确
async function fun(){
	return "hahah"
}
async function fun1(){
	var f = await fun();
	console.log(f)
}
fun1();
```

promise

```js
function getdata(){
	return new Promise(function(resolve,reject){
		setTimeout(function(){
			resolve("成功")
		},100);
	})
}
async function fun1(){
	var data = await getdata();
	console.log(data)
}
fun1();	//成功
```





## 类

### 定义方式

```js
class point{
    //构造方法
    constructor(x, y) {
    this.x = x;
    this.y = y;
  }
    //属性方法等
}
```

### 实例化

```js
var point = new Point(2, 3);
```

### 继承

```js
class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }
}
 
class ColorPoint extends Point {
    //子类的构造方法必须调用super()方法,否则子类实例化会失败
  constructor(x, y, color) {
    this.color = color; // ReferenceError 调用super()之前不能使用this
    super(x, y);
    this.color = color; // 正确
  }
}
```

### getter和setter

对内部某个属性设置存值函数和取值函数, 也就是**自定义赋值和读取行为**

```js
class MyClass {
  get prop() {
    return 'getter';
  }
  set prop(value) {
    document.write('setter: '+value);
  }
}
 
let inst = new MyClass();
 
inst.prop = 123;
// setter: 123
 
inst.prop
// 'getter'
```

### 静态方法

方法前加static关键字,表示该方法**无法被实例化**,而是通过类名直接调用

```js
class Foo {
  static classMethod() {
    return 'hello';
  }
}
Foo.classMethod() // 'hello'
```

父类静态方法,子类可以调用,也可以通过super对象上调用

```js
class Foo {
  static classMethod() {
    return 'hello';
  }
}
class Bar extends Foo {
  static classMethod() {
    return super.classMethod() + ', too';
  }
}
Bar.classMethod();
```

### 判断实例是否通过new

new.target属性,不是通过new,会返回undefined   // if (new.target===undefined)

```js
class Shape {
  constructor() {
    if (new.target === Shape) {
      throw new Error('本类不能实例化');
    }
  }
}
```

### 类修饰符



## Module

**不可处于块级作用域内**,否则报错

- *export*命令用于用户自定义模块，规定对外接口；
- *import*命令用于输入其他模块提供的功能，同时创造命名空间（namespace），防止函数名冲突。

###严格模式

ES6 的模块自动采用严格模式，不管你有没有在模块头部加上`"use strict";`。

- 变量必须声明后再使用
- 函数的参数不能有同名属性，否则报错
- 不能使用`with`语句
- 不能对只读属性赋值，否则报错
- 不能使用前缀 0 表示八进制数，否则报错
- 不能删除不可删除的属性，否则报错
- 不能删除变量`delete prop`，会报错，只能删除属性`delete global[prop]`
- `eval`不会在它的外层作用域引入变量
- `eval`和`arguments`不能被重新赋值
- `arguments`不会自动反映函数参数的变化
- 不能使用`arguments.callee`
- 不能使用`arguments.caller`
- 禁止`this`指向全局对象
- 不能使用`fn.caller`和`fn.arguments`获取函数调用的堆栈
- 增加了保留字（比如`protected`、`static`和`interface`）

### import

使用export命令定义了模块的对外接口以后，其他JS文件就可以通过*import*命令加载这个模块（文件）。

ES6支持多重加载，即所加载的模块中又加载其他模块。

```js
import {firstName, lastName, year} from './profile';

//将输入的变量重命名
import { lastName as surname } from './profile';
```

### export

```js
export命令除了输出变量，还可以输出方法或类（class）,下面是一个circle.js文件
export function area(radius) {
  return Math.PI * radius * radius;
}
export function circumference(radius) {
  return 2 * Math.PI * radius;
}
```

输入circlek.js模块

```js
import { area, circumference } from 'circle';
document.write("圆面积：" + area(4));
document.write("圆周长：" + circumference(14));
```

### 整体输入

1. import*

```js
//整体输入
import * as profile from './profile';
//使用
profile.属性	profile.方法
```

2. module命令可以取代import语句，达到整体输入模块的作用。

```js
module profile from './profile';
```

