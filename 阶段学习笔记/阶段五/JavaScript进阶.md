# JavaScript 进阶学习笔记
---
## 一、ES6常用语法
### 1. let变量声明以及特性
`let` 是ES6新增的变量声明方式，用来替代老式的 `var`。
1. **块级作用域**：只在 `{ }`（if、for、代码块等）内部生效，外部访问不到
2. **不存在变量提升**：必须先声明再使用，提前用会直接报错
3. **不允许重复声明**：同一个作用域内不能重复声明同一个变量
4. 变量的值可以修改
```js
if (true) {
  let num = 10
}
// console.log(num) //报错，块外部拿不到
```
### 2. const声明常量以及特点
`const`用来声明**常量**，声明后不能整体重新赋值。
1. 和`let`一样，具备块级作用域、无变量提升、不可重复声明
2. **声明时必须立刻赋值**，不能只声明不写值
3. 基本数据类型的值不能修改；如果存的是对象/数组，可以修改内部的属性和元素
```js
const arr = [1, 2, 3]
arr.push(4)       //可以修改数组内部内容
// arr = [5,6]   //报错，不能整体重新赋值
```
### 3. 变量的解构赋值
快速从数组、对象里把值取出来，赋值给新变量
#### 数组解构
```js
const [a, b, c] = [10, 20, 30]
// a=10  b=20  c=30
```
#### 对象解构
```js
const user = { name: "张三", age: 18 }
const { name, age } = user
// name="张三"  age=18
```
### 4. 模板字符串
用反引号`` ` ``包裹字符串，替代普通的单双引号
1. 支持直接换行写字符串
2. 用`${变量/表达式}`直接嵌入内容，不用字符串拼接
```js
let name = "小明"
let str = `你好，我叫${name}，今年${10 + 8}岁`
```
### 5. 对象的简化写法
对象里属性名和变量名相同时，可以只写一个；函数也可以简写
```js
let name = "李四"
let obj = {
  name,             //等价于 name: name
  say() {           //等价于 say: function(){}
    console.log("hello")
  }
}
```
### 6. 箭头函数以及声明特点
更简洁的函数写法：`(形参) => { 函数体 }`
```js
const add = (a, b) => {
  return a + b
}
```
#### 简写规则
1. 只有一个形参时，小括号可以省略：`n => { return n * 2 }`
2. 函数体只有一行return时，可以省略大括号和return：`n => n * 2`
#### 核心特点
1. **没有自己的 this**，this等于定义箭头函数时，外层作用域的 this
2. 不能当作构造函数，不能用new调用
3. 没有arguments对象
### 7. rest 参数
语法：`...变量名`，用来接收函数剩余的所有实参，得到一个**真正的数组**
必须写在形参列表的最后面。
```js
function sum(first, ...rest) {
  console.log(first)
  console.log(rest) //rest是数组，装了剩下的所有参数
}
sum(10, 20, 30, 40)
```
> 和 arguments 的区别：arguments 是伪数组，rest 是真数组，可以直接用数组方法
### 8. 扩展运算符
符号也是`...`，作用是把数组、对象**拆开打散**
```js
const arr = [1, 2, 3]
console.log(...arr) //等价于 console.log(1, 2, 3)
```
#### 常见应用
1. 数组浅拷贝：`const arr2 = [...arr1]`
2. 合并数组：`const arr3 = [...arr1, ...arr2]`
3. 对象浅拷贝/合并：`const obj2 = { ...obj1, age: 20 }`
### 9. class介绍与初体验
ES6的类是构造函数的语法糖，用来更方便地创建对象、实现面向对象
```js
class Person {
  // 构造方法，new 的时候自动执行
  constructor(name) {
    this.name = name
  }
  sayHi() {
    console.log(this.name)
  }
}
const p = new Person("小王")
p.sayHi()
```
- `constructor`：构造函数，接收new时传的参数，给实例加属性
- 类里写的方法，会自动放到原型对象上
### 10. class的类继承和方法重写
#### 类继承
用`extends`实现继承，子类`constructor`里必须先调用`super()`
```js
class Animal {
  constructor(name) {
    this.name = name
  }
}
class Dog extends Animal {
  constructor(name) {
    super(name) //调用父类的构造函数
  }
}
```
#### 方法重写
子类写一个和父类同名的方法，就会覆盖父类的方法；想调用父类原方法用`super.方法名()`
```js
class Dog extends Animal {
  say() {
    super.say() //调用父类的 say 方法
    console.log("汪汪汪")
  }
}
```
### 11. ES6模块化
把代码拆分到多个js文件里，避免全局变量污染，方便复用
- 导出（暴露）：`export`，对外提供数据和方法
- 导入：`import`，引入其他模块的内容
#### 浏览器使用前提
script 标签必须加`type="module"`，浏览器才识别模块化语法
```html
<script type="module" src="./main.js"></script>
```
#### 三种导出方式
1. 分别导出：`export const a = 10`
2. 统一导出：
```js
const num = 10
function fn() {}
export { num, fn }
```
3. 默认导出（一个模块只能有一个）：
```js
export default {
  num: 100
}
```
#### 三种导入方式
1. 导入分别/统一导出：`import { num, fn } from './xxx.js'`
2. 导入默认导出：`import obj from './xxx.js'`
3. 全部导入：`import * as m from './xxx.js'`
### 12. 可选链操作符 ?.
读取对象深层属性时，不用层层判断是否存在；如果中间某一层是 `undefined/null`，直接返回 `undefined`，不会报错
```js
const user = { info: { name: "小红" } }
console.log(user?.info?.name)
// 如果 user.info 不存在，不会报错，直接返回 undefined
```
> 开发中读取接口返回的嵌套数据非常高频。
---
## 二、JS高级核心概念
### 1. 函数中的this
`this` 是函数内部的关键字，**this 的值由函数的调用方式决定，和定义位置无关**
1. **普通函数全局调用** `fn()`→this指向`window`
2. **对象方法调用** `obj.fn()`→this指向调用方法的对象`obj`
3. **new 构造函数调用**`new Person()`→this指向new出来的实例对象
4. **call/apply/bind**：手动强制修改this指向
   - `call(新this, 参数1, 参数2)`：参数逐个传
   - `apply(新this, [参数数组])`：参数放在数组里
   - `bind(新this)`：返回新函数，不会立刻执行
### 2. 原型与原型链 
#### 显式原型prototype
- 每一个**函数**都自带`prototype`属性，叫做显式原型，它是一个对象
- 作用：把所有实例共用的方法写在原型上，节省内存。
```js
function Person() {}
Person.prototype.say = function() {
  console.log("你好")
}
let p = new Person()
p.say()
```
#### 隐式原型`__proto__`
- 每一个**对象**都有`__proto__`属性，叫做隐式原型
- 实例的隐式原型，等于它构造函数的显式原型：
  `实例.__proto__ === 构造函数.prototype`
#### 原型链
读取对象的属性/方法时：
1. 先在对象自身找
2. 找不到，就去`__proto__`（原型对象）上找
3. 还找不到，继续沿着原型对象的 `__proto__` 向上找
4. 一直找到`Object.prototype`，再往上`Object.prototype.__proto__`是`null`，查找结束
这条向上查找的链条，就叫**原型链**。
#### 原型链重要规则
1. 几乎所有对象最终都继承自`Object.prototype`，所以都能用`toString()`等方法
2. **读取属性顺着原型链向上找；给对象赋值只会改自身，不会修改原型上的内容**
3. 开发习惯：实例存独有的属性，原型放公共的方法
> 注意：不要把数组、对象这类引用类型写在原型上，一个实例修改会影响全部实例
### 3. instanceof
语法：`实例 instanceof 构造函数`
- 判断逻辑：构造函数的`prototype`是否出现在这个实例的原型链上
- 返回布尔值true/false
> 不是判断对象是不是这个函数直接 new 出来的，只要原型链上能找到就返回 true
### 4. 作用域与作用域链
1. **作用域**：变量生效的范围
   - 全局作用域：整个脚本都能访问
   - 函数局部作用域：只有函数内部能访问
2. **作用域链**
   函数嵌套时，内层函数找变量：先看自己内部，找不到就向外层函数逐层找，一直找到全局作用域
> 重点区分：
> - 作用域链：函数定义的时候就确定了，和在哪调用无关
> - this指向：函数调用的时候才确定
### 5. 闭包 
#### 闭包怎么产生
函数嵌套加内部函数访问了外层函数的局部变量加内部函数能被外部使用，就形成了闭包
```js
function outer() {
  let count = 0
  return function inner() {
    count++
    console.log(count)
  }
}
let fn = outer()
fn()
```
#### 闭包特点
1. 函数外部可以访问到函数内部的局部变量
2. 外层函数执行结束后，局部变量不会被销毁，会被闭包保留下来
#### 常见使用场景
1. 计数器
2. 保存私有数据，外部不能直接修改
3. 自定义JS模块：保护内部变量，只向外暴露需要的方法
#### 闭包生命周期
1. 产生：内部函数引用外层局部变量时
2. 存活：只要闭包函数还被引用，变量就一直占着内存
3. 销毁：手动把闭包函数赋值为`null`，解除引用，垃圾回收才会释放内存
### 6. 内存溢出和内存泄露
1. **内存溢出**：程序申请的内存超出了可用内存，程序直接崩溃报错
2. **内存泄露**：已经用不到的数据没有被释放，一直占着内存,不会立刻报错，但程序会越跑越卡
- JS 常见泄露原因：
   闭包用完没有释放引用
   全局变量过多
   DOM 元素已经从页面移除，但代码里还存着 DOM 的引用
> 闭包好用，但用完记得解除引用，避免内存泄露。
### 7. JS 单线程和事件循环
#### JS 是单线程的
JavaScript是单线程语言，同一时间只能做一件事，所有代码在主线程排队执行
遇到异步任务（定时器、网络请求、事件）不会卡住主线程，先放到一边，继续执行后面的同步代码
#### 事件循环模型EventLoop
JS 处理异步任务的运行机制：
1. **同步任务**：优先执行，进入调用栈立刻运行
2. **异步任务**：定时器、ajax、事件回调，不会马上执行，放进任务队列
3. 事件循环流程：
   先把所有同步代码执行完
   调用栈清空后，从任务队列里取出一个异步回调
   放到调用栈里执行
   不断重复这个过程，就是事件循环
> 同步全部做完，再依次处理异步队列里的回调
---
## 三 、Promise与async/await异步编程
### 1. Promise介绍与初体验
Promise 是专门处理异步操作的对象，用来解决回调地狱（回调层层嵌套，代码难读难维护）的问题。
#### 基础语法
```js
const p = new Promise((resolve, reject) => {
  //这里写异步任务：AJAX、定时器等
  //异步成功调用 resolve
  //异步失败调用 reject
  resolve("成功的数据")
})
//获取结果
p.then(res => {
  console.log("成功：", res)
}).catch(err => {
  console.log("失败：", err)
})
```
- `new Promise`接收一个执行器函数，这个函数会**立刻同步执行**
- `.then()`接收成功结果，`.catch()`接收失败错误
### 2. Promise对象的状态与结果值
#### 三种状态
Promise一共三种状态，**只能改变一次，确定后就再也不能修改**：
1. `pending`进行中：初始状态，既没成功也没失败
2. `fulfilled / resolved`已成功：调用`resolve()`后变成成功
3. `rejected`已失败：调用`reject()`后变成失败
#### 结果值
Promise内部保存着异步任务的结果，存在`[[PromiseResult]]`里：
- `resolve()`执行后，存成功的数据
- `reject()`执行后，存失败的错误信息
- 我们不能直接访问这个内部属性，只能通过`.then()` / `.catch()`拿到值
### 3. Promise基础API
1. **构造函数**：`new Promise((resolve, reject) => {})`
2. **.then(成功回调, 失败回调)**
   - 第一个回调处理成功，第二个可选回调处理失败
   - **返回一个全新的 Promise 对象，支持链式调用**
3. **.catch(失败回调)**：专门捕获失败，接收reject的错误
> 推荐写法：`.then(成功回调).catch(失败回调)`
### 4. Promise静态方法
#### Promise.resolve()
快速创建一个成功状态的 Promise。
```js
const p = Promise.resolve(100)
```
#### Promise.reject()
快速创建一个失败状态的 Promise
```js
const p = Promise.reject("出错了")
```
#### Promise.all()
接收一个Promise数组，并发执行多个异步任务
- **全部Promise都成功，整体才成功；只要有一个失败，整体直接失败**
- 成功时返回结果数组，顺序和传入顺序一致
```js
Promise.all([p1, p2]).then(resArr => {
  //按顺序存放所有成功结果
}).catch(err => {
  //第一个失败的错误
})
```
#### Promise.race()
竞速模式：数组里多个Promise，**谁最先改变状态，就采用谁的结果（无论成功还是失败）**。
常用于请求超时控制等场景。
### 5. Promise关键问题
#### then方法的返回结果由什么决定
`.then()` 永远返回一个新的Promise，它的状态由then回调函数的返回值决定：
1. 回调里return普通值→新Promise成功，值就是return的内容
2. 回调里return Promise对象→新Promise状态跟随return的Promise
3. 回调里抛出错误→新Promise失败
#### 链式串联多个异步任务
利用then返回新Promise的特点，可以让异步任务按顺序执行：
```js
sendAjax("/api/1")
  .then(res1 => {
    return sendAjax("/api/2")
  })
  .then(res2 => {
    return sendAjax("/api/3")
  })
  .catch(err => {})
```
#### 异常穿透
链式调用多个`.then()`时，**任意位置发生错误，都会一直向后传递，直到遇到 .catch()**
不需要每个then都写错误处理，整条链末尾写一个catch，就能捕获全部错误
### 6. async 函数
`async`关键字写在函数前面，把普通函数变成async函数。
1. async函数**返回值一定是Promise对象**
2. 函数内部return普通值→返回成功的Promise
3. 函数内部throw抛出错误→返回失败的Promise
```js
async function fn() {
  return 666
}
fn().then(res => console.log(res))
```
### 7. await 表达式
- `await`只能写在async函数内部，不能单独用在普通函数里
- `await`后面接Promise对象：会暂停代码，等待Promise完成；成功就拿到结果；失败就抛出错误
```js
async function getData() {
  const res = await sendAjax("/api")
  console.log(res) //拿到异步成功的数据
}
```
### 8. async与await完整用法
标准写法，用`try...catch`捕获await的错误：
```js
async function getList() {
  try {
    const res1 = await sendAjax("/api/list1")
    const res2 = await sendAjax("/api/list2")
    console.log(res1, res2)
  } catch (err) {
    console.error("接口出错", err)
  }
}
```
> async / await 是 Promise 的语法糖，底层依然是 Promise，写起来和同步代码几乎一样，开发最常用。
```