# 1.JavaScript简介  
1. 什么是JavaScript  
 - JavaScript是一种运行在**客户端**的**脚本语言**（Script）  
 - 特点：**解释型语言**，无需编译，由JS引擎逐行解释执行  
 - JavaScript=**ECMAScript**（标准）+**BOM**（浏览器对象模型）+**DOM**（文档对象模型）  
 - 作用：
   - 表单动态校验（最初目的）
   - 网页特效（轮播图、拖拽等）
   - 数据交互（AJAX）
   - 服务端开发（Node.js）   
2. JS与Java的区别  
   |特性|JavaScript|Java|  
   |---|---|---|  
   |语言类型|脚本语言，解释执行|编译型语言，编译为字节码|   
   |运行环境|浏览器 / Node.js|JVM|  
   |类型系统|动态类型|静态类型|  
   |面向对象|基于原型|基于类|  
3. 三种输出方式  
    1. 弹出警告框  
   `alert("Hello World!");`  
    2. 页面输出  
   `document.write("Hello World!");`  
    3. 控制台输出（推荐）  
   `console.log("Hello World!");`   
4. JS 代码的三种编写位置  
   1. **内联脚本**：直接写在 HTML 标签事件属性中（不推荐）  
   `<button onclick="alert('Hello')">点击我</button>`  
   2. **内部脚本**：写在`<script>`标签中   
   ```HTML  
   <script>
      console.log("Hello World!");
   </script>  
   ```  
   3. **外部脚本**（推荐）：写在.js 文件中，通过`<script>`标签引入  
   `<script src="main.js"></script>`  
   - 注意：外部脚本标签中不能写代码  
   - 引入位置：通常在`<body>`结束前，避免阻塞页面加载  
# 2.基础语法  
1. 语句与标识符
 - 语句：以 `;` 结尾（可选，但推荐）
 - 标识符：变量、函数等的命名规则
    - 可包含字母、数字、下划线_、美元符号$
    - 不能以数字开头
    - 区分大小写
    - 不能使用关键字  
2. 注释  
 - 单行注释:`//`  
 - 多行注释:  
  ```
  /*
  多行注释
  第二行
  */  
  ```  
3. 变量  
 - 变量用于存储数据，可以修改存储的值
 - 声明方式：  
  ```javascript  
   // ES6推荐方式（块级作用域）
   let age = 18;
   // 常量（不可修改）
   const PI = 3.14159;
   // 旧方式（函数级作用域，不推荐）
   var name = "Ce";  
   ```  
 - 变量命名规范：
   - 小驼峰命名法（如userName）  
   - 语义化命名  
# 3.数据类型  
1. 原始数据类型  
   |类型|描述|示例|  
   |---|---|---|  
   |Number|数值（整数 / 浮点数）	|100, 3.14|  
   |String|字符串|"Hello", 'World'|  
   Boolean|布尔值|true, false|  
   |Null|空值|null|  
   |Undefined|未定义|undefined|  
   |Symbol|唯一标识（ES6）|Symbol("id")|  
   |BigInt|大整数（ES11）|100n|  
2. typeof 运算符  
   ```javascript  
   console.log(typeof 100); // "number"
   console.log(typeof "Hello"); // "string"
   console.log(typeof true); // "boolean"
   console.log(typeof null); // "object"（历史遗留bug）
   console.log(typeof undefined); // "undefined"
   console.log(typeof Symbol("id")); // "symbol"
   console.log(typeof 100n); // "bigint"  
   ```  
3. 类型转换  
   1. 隐式转换  
   ```javascript  
   // 数字 + 字符串 = 字符串
   console.log(100 + "abc"); // "100abc"
   // 数字 + 布尔值 = 数字（true=1, false=0）
   console.log(100 + true); // 101
   // 字符串 + 布尔值 = 字符串
   console.log("abc" + true); // "abctrue"
   // 比较时的转换
   console.log(1 == "1"); // true（值相等）
   console.log(1 === "1"); // false（类型+值都相等，推荐）  
   ```  
   2. 显式转换  
   ```javascript  
   // 转为数字
   Number("100"); // 100
   parseInt("100abc"); // 100（取整数）
   parseFloat("3.14abc"); // 3.14（保留小数）
   // 转为字符串
   String(100); // "100"
   (100).toString(); // "100"
   // 转为布尔值
   Boolean(0); // false
   Boolean(""); // false
   Boolean(null); // false
   Boolean(undefined); // false
   Boolean(1); // true
   Boolean("abc"); // true  
   ```  
# 4.运算符  
1. 算术运算符  
   ```javascript  
   + - * / % // 加、减、乘、除、取余
   ++ -- // 自增、自减  
   ```  
2. 赋值运算符  
   `=` 、`+=`、`*=`、`/=`、`%=`  
3. 比较运算符  
   `>`、`<`、`>=`、`<=`、`==`、`===`、`!=`、`!==`  
   - ==：值相等（会自动类型转换）
   - ===：值和类型都相等（推荐）  
4. 逻辑运算符  
   `&&`、`||`、`!`  
   - 短路求值：  
  ```javascript  
  // &&：只要有一个false，结果为false
  console.log(true && false); // false
  // ||：只要有一个true，结果为true
  console.log(true || false); // true
  // !：取反
  console.log(!true); // false  
  ```  
5. 三元运算符  
   `条件 ? 条件成立时的值 : 条件不成立时的值`  
# 5.流程控制语句  
1. 代码块与作用域
 - 代码块：用`{}`包裹的一组代码
 - let/const 声明的变量具有**块级作用域**
 - var 声明的变量具有**函数级作用域**（不推荐使用）  
  ```javascript  
  {
  let a = 10; // 块级作用域
  var b = 20; // 函数级作用域
  }
  console.log(a); // 报错：a未定义
  console.log(b); // 20  
  ```  
2. 条件语句
   - if 语句  
   - switch 语句(`break`用于跳出 switch，否则会继续执行下一个 case)  
3. 循环语句
   - while 循环
   - do...while 循环(特点：至少执行一次循环体)  
   - for 循环  
   - 跳转控制  
       - **break**：跳出当前循环 /switch
     - **continue**：跳过当前循环，继续下一次循环  
  ```javascript  
  for (let i = 0; i < 10; i++) {
  if (i === 5) break; // 当i=5时跳出循环
  console.log(i);
  }

  for (let i = 0; i < 10; i++) {
  if (i % 2 === 0) continue; // 跳过偶数
  console.log(i);
  }  
  ```  
# 6.对象基础  
1. 对象简介  
 - 对象是 JS 中的**复合数据类型**，用于存储多个相关数据和功能
 - 键值对集合：key（属性名）: value（属性值）  
2. 创建对象的三种方式  
   1. 对象字面量  
   ```javascript  
   let person = {
     name: "Ce",
     age: 18,
     gender: "female",
     sayHello: function() {
       console.log(`大家好，我是${this.name}`);
    }
   };  
   ```  
   2. 构造函数  
   ```javascript  
   let person = new Object();
   person.name = "Ce";
   person.age = 18;
   person.sayHello = function() {
     console.log(`大家好，我是${this.name}`);
   };  
   ```  
   3. 工厂函数  
   ```javascript  
   function createPerson(name, age) {
     return {
       name: name,
       age: age,
       sayHello: function() {
         console.log(`大家好，我是${this.name}`);
        }
     };
   }
   let person = createPerson("Ce", 18);  
   ```  
3. 对象的操作  
```javascript    
let person = { name: "Ce", age: 18 };
// 1. 访问属性
console.log(person.name); // "Ce"
console.log(person["age"]); // 18
// 2. 修改属性
person.age = 10086;
// 3. 添加属性
person.gender = "female";
// 4. 删除属性
delete person.gender;
// 5. 调用方法
person.sayHello = function() {
  console.log(`大家好，我是${this.name}`);
};
person.sayHello();   
``` 
4. 遍历对象
```javascript  
let person = { name: "Ce", age: 18, gender: "female" };
// 1. for...in循环
for (let key in person) {
  console.log(key + ": " + person[key]);
}
// 2. Object.keys()
Object.keys(person).forEach(key => {
  console.log(key + ": " + person[key]);
});  
```  
# 7.函数  
1. 函数的声明与调用   
2. 函数参数
 - 形参：函数定义时的参数
 - 实参：函数调用时的参数
 - JS 函数参数特性：
   - 可传任意数量参数
   - 可传任意类型参数
   - 有默认参数（ES6）  
3. 作用域与作用域链
 - **全局作用域**：在函数外部定义的变量，整个程序可访问
 - **局部作用域**：在函数内部定义的变量，仅函数内部可访问
 - **作用域链**：内部函数可访问外部函数变量，外部函数不能访问内部函数变量  
4. 闭包
 - 闭包：函数嵌套中，内部函数引用外部函数变量，形成的封闭环境
 - 特点：内部函数可访问外部函数变量，即使外部函数已执行完毕  
 ```javascript  
 function createCounter() {
  let count = 0;
  return function() {
    count++;
    return count;
  };
}

const counter = createCounter();
console.log(counter()); // 1
console.log(counter()); // 2
console.log(counter()); // 3  
```  
5. 递归函数
```javascript  
// 阶乘
function factorial(n) {
  if (n === 1) return 1; // 基线条件
  return n * factorial(n - 1); // 递归条件
}
console.log(factorial(5)); // 120
```  
# 8.数组
1. 数组的创建  
```javascript  
// 1. 数组字面量（推荐）
const arr = [1, 2, 3, 4, 5];

// 2. 构造函数
const arr = new Array(1, 2, 3, 4, 5);

// 3. 创建指定长度数组
const arr = new Array(5); // 长度为5，元素均为undefined  
```  
2. 数组常用方法   
 
|方法|描述|  
|---|---|  
|push()|末尾添加元素，返回新长度|  
|pop()|末尾删除元素，返回删除元素|  
|unshift()|开头添加元素，返回新长度|  
|shift()|开头删除元素，返回删除元素|  
|slice()|截取数组，返回新数组|  
|splice()|添加 / 删除元素，返回被删除元素|  
|forEach()|遍历数组|  
|map()|映射数组，返回新数组|  
|filter()|过滤数组，返回新数组|  
|reduce()|归并数组，返回最终结果|  

eg:  
```javascript  
const arr = [1, 2, 3, 4, 5];
// forEach遍历
arr.forEach(item => {
  console.log(item);
});
// map映射
const newArr = arr.map(item => item * 2);
console.log(newArr); // [2, 4, 6, 8, 10]
// filter过滤
const evenArr = arr.filter(item => item % 2 === 0);
console.log(evenArr); // [2, 4]  
```  
# 9.DOM 操作  
1. 获取元素
```javascript  
// 1. 通过ID获取（返回单个元素）
const box = document.getElementById("box");
// 2. 通过类名获取（返回HTMLCollection）
const items = document.getElementsByClassName("item");
// 3. 通过标签名获取（返回HTMLCollection）
const divs = document.getElementsByTagName("div");
// 4. 通过选择器获取（ES6）
const box = document.querySelector("#box"); // 返回第一个匹配元素
const items = document.querySelectorAll(".item"); // 返回NodeList  
```   
2. 修改元素内容
```javascript  
const box = document.getElementById("box");
// 1. 修改文本内容
box.textContent = "新内容";
// 2. 修改HTML内容
box.innerHTML = "<h1>新内容</h1>";  
```  
3. 修改元素样式  
```javascript  
const box = document.getElementById("box");
// 1. 修改行内样式
box.style.width = "100px";
box.style.height = "100px";
box.style.backgroundColor = "red";
// 2. 添加/移除类
box.classList.add("active");
box.classList.remove("active");
box.classList.toggle("active"); // 切换类  
```  
4. 事件绑定  
```javascript  
const btn = document.getElementById("btn");
// 1. 行内事件（不推荐）
<button onclick="handleClick()">点击</button>
// 2. DOM属性绑定
btn.onclick = function() {
  console.log("按钮被点击了");
};
// 3. addEventListener（推荐）
btn.addEventListener("click", function() {
  console.log("按钮被点击了");
});  
```  
# 10.BOM 操作  
1. window 对象
 - BOM 核心对象，代表浏览器窗口
 - 常用属性：  
  ```javascript  
   console.log(window.innerWidth); // 窗口宽度
   console.log(window.innerHeight); // 窗口高度  
   ```  
2. 弹出框
```javascript  
// 1. 警告框
alert("警告！");
// 2. 确认框
let isConfirm = confirm("确定要删除吗？");
if (isConfirm) {
  console.log("删除成功");
}
// 3. 输入框
let name = prompt("请输入您的名字：");
console.log("您的名字是：" + name);  
```  
3. 定时器  
```javascript  
// 1. 一次性定时器（setTimeout）
let timer1 = setTimeout(() => {
  console.log("1秒后执行");
}, 1000);
// 清除定时器
clearTimeout(timer1);
// 2. 周期性定时器（setInterval）
let count = 0;
let timer2 = setInterval(() => {
  count++;
  console.log("每1秒执行一次：" + count);
  if (count >= 5) {
    clearInterval(timer2); // 执行5次后停止
  }
}, 1000);  
```  
4. 页面跳转与刷新
```javascript  
// 跳转页面
window.location.href = "https://www.bilibili.com";
// 刷新页面
window.location.reload();
// 返回上一页
window.history.back();  
```  


















 

 






 
 
   
 

     






 







  



  
   
 