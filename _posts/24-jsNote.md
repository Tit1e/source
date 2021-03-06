---
title: JavaScript高级程序设计笔记（一）
date: 2017-03-04 11:46:34
tags: ['学习']
---

「JavaScript高级程序」这本书知道的不用说太多。但是这书厚得跟板砖一样，而且长时间阅读又枯燥无味，啃下来的确很难。这段时间打算把JS原生捡起来复习一遍，因为jQuery用的太多导致原生都快忘光了。以后笔记可能会做的比较精炼，这里就记一些比较重点的东西。

#### 完整的JavaScript组成
- `ECMAScript`（核心）
- `DOM`（文档对象模型）
- `BOM`（浏览器对象模型）

但在通常人们口中所说的JavaScript常常指的是ECMAScript。
<!-- more -->
#### 插入JavaScript代码方法
- 通过`<script></script>`标签直接在网页中写JS代码

 ```javascript
    <script type="text/javascript">
        // code...
     </script>
 ```
- 通过`<script>`标签的`src`属性引入外部JS代码
```javascript
    <script type="text/javascript" src="url"></script>
```
**注：为了不影响网页的加载速度，JS代码一般放在网页尾部。**

#### JavaScript基本概念
- 区分大小写

  * JavaScript中的变量，函数名或操作符都区分大小写
- 标识符
  * 标识符：指的是JS代码中的变量、函数、属性、函数参数的名字
  * 标识符的**第一个字符**必须是字母、下划线(_)或者美元符号($)
  * 标识符的**其他组成**可以是字母、下划线、美元符号或者数字
  * 一般情况下标识符采用驼峰法命名，如myPage、helloWorld
- 注释
  * 单行注释 

    - `//单行注释`
  * 多行注释
    - ```
      /*
       *多行注释
       *多行注释
       *／
      ```

#### 严格模式
  - 全局使用严格模式

    + 脚本顶部加入`'use strict';`
  - 函数内使用严格模式
    + ```
      function fn(){
        'use strict';
        //code...
      }
      ```


#### 变量
  - 变量声明
    * var定义的变量会成为定义这个变量作用域中的局部变量（在全局中声明就是全局对象，在函数中声明，函数执行完成后变量自行销毁）
    * 不用var声明的变量，无论在函数内或函数外声明，都是全局变量
    * 可以一次声明多个变量，用`,`隔开：`var a = 1,b = "123",c = true;`
  - JavaScript的变量是松散类型，所以通过重新赋值可以改变变量的类型
  ```javascript
  var a = 1;
  typeof a;  //number，typeof操作符下文数据类型中有解释
  a = "你好";
  typeof a;  //string
  ```
  **在实际开发中不建议直接改变某个变量的类型。**

#### 数据类型
- JavaScript中有五种基本数据类型和一种复杂数据类型
  + Undefined
  + Null
  + Boolean
  + Number
  + String
  + Object(复杂类型)
- typeof操作符用于检测变量的数据类型，返回的类型有以下几种
  + `'undefined'`
  + `'boolean'`
  + `'string'`
  + `'number'`
  + `'object'`
  + `'function'`
  ```javascript
  typeof null  //'object'
  typeof {}  //"object"
  typeof []  //"object"
  typeof undefined  //"undefined"
  typeof "你好"  //"string"
  typeof 1  //"number"
  typeof NaN  //"number"
  typeof function a(){}  //"function"
  typeof true  //"boolean"
  ```
  由上代码可以看出typeof操作符并不是可以非常准确地返回数据的类型。那么如何区分`null`、`[]`、`{}`的数据类型呢？
  我们可以用`Object.prototype.toString.call(变量)`来区分：
  ```javascript
  Object.prototype.toString.call([])  //"[object Array]"
  Object.prototype.toString.call({})  //"[object Object]"
  Object.prototype.toString.call(null)  //"[object Null]"
  Object.prototype.toString.call(NaN)  //"[object Number]"
  Object.prototype.toString.call(1)  //"[object Number]"
  ```
  我们发现`NaN`和`1`还是无法区分，那么这个又怎么区分呢？
  我们可以用一个专门用来辨别`NaN`的方法来区分：
  ```javascript
  isNaN(NaN)  //true
  isNaN(1)  //false
  ```
#### Null类型
- `Null`类型只有一个值：`null`
- `null`是一个空对象指针，用`typeof`检测返回object
- 当一个变量将来要储存为对象的时候，最好先将这个变量设置为`null`
- null == undefined

#### Boolean类型
- Boolean类型只有true和false两个值
- `true == 1`，`false == 0`，但是true不一定等于1，false也不一定等于0
- JavaScript中的任何一个值都可以通过`Boolean()`方法转换为Boolean值。
  + 转换为true的值有：
    * Boolean类型的**true**
    * String类型的**任何非空字符串**
    * Number类型的**任何非零数值，包括无穷大**
    * Object类型的**任何对像**，包括{}，从中可以看出{}与null不同
  + 转换为false的值的有：
    * Boolean类型的**flase**
    * String类型的**空字符串**
    * Number类型的**0和NaN**
    * Object类型的**null**
    * Undefined类型的**undefined**
    
#### Number类型
- Number类型可以用十进制表示，也可以用八进制和十六进制表示
- 用八进制表示时，如果含有数字超过8则会被解析成十进制
- 算数计算事八进制和十六进制都会被转换成十进制数值
- 浮点数，即带小数的数值
  + 浮点数的两种写法
  ```javascript
   var floatNum1 = 0.1;
   var floatNum2 = .1;//这种写法有效，但是不推荐
  ```
  + 下列两种情况下，浮点会被转换成整数保存
  ```javascript
  var floatNum1 = 1.; // 小数点后面没有数字——解析为 1 
  var floatNum2 = 10.0; // 整数——解析为 10
  ```
- 浮点数支持科学计数法表示
- 浮点数最高精确度是17位小数
- JavaScript最小能表示的数值为`5e-324`
- JavaScript最大能表示的数值为`1.7976931348623157e+308`，如果超出这个范围会被转换成`Infinity`（正无穷），如果这个值是负的，那就被转换成`-Infinity`（负无穷）
- 0除以0会得到`NaN`，**并且`NaN`不等于`NaN`**
- `number()`、`parseInt()`、`parseFloat()`可以把非数值转换成数值
  + `number()`转换规则：
    * `Boolean`类型的`true`和`false`会被转换成0和1
    * `Number`类型返回传入的值
    * `Null`类型的`null`返回0
    * `Undefined`类型的值返回NaN
    * `String`类型的如果只有数字，包括浮点数、八进制表示的、十进制表示的，负数，都会被转换成对应的十进制数值，如果包含其他字符，则返回NaN，空字符串返回0
  + `parseInt()`转换规则
    * `parseInt()`可用来提取字符串中的有效数字
    ```javascript
    var num1 = parseInt("1234blue");  // 1234
    var num2 = parseInt("");  // NaN
    var num3 = parseInt("0xA");  // 10(十六进制数)
    var num4 = parseInt(22.5);  // 22  小数点不是数字，所以遇到小数点停止解析
    var num5 = parseInt("070");// 56(八进制数)
    var num6 = parseInt("70");  // 70(十进制数)
    var num7 = parseInt("0xf");  // 15(十六进制数)
    var num8 = parseInt("a123")  //NaN  遇到非数字就停止解析
    var num8 = parseInt("  123")  //123  前面空格会被忽略
    ```
    * `parseInt()`可以指定传入的数字的进制
    ```javascript
    var num1 = parseInt("AF", 16);  //175
    var num2 = parseInt("AF");  //NaN
    var num3 = parseInt("10", 2);  //2 (按二进制解析)
    var num4 = parseInt("10", 8);  //8 (按八进制解析)
    var num5 = parseInt("10", 10);  //10(按十进制解析)
    var num6 = parseInt("10", 16);  //16(按十六进制解析)
    ```
  + `parseFloat()`转换规则
    * `parseFloat()`只能解析十进制
    * 十六进制格式的字符串始终会被转换成0
    * `parseFloat()`转换
    ```javascript
    var num1 = parseFloat("1234blue");  //1234 (整数)
    var num2 = parseFloat("0xA");  //0
    var num3 = parseFloat("22.5");  //22.5
    var num4 = parseFloat("22.34.5");  //22.34
    var num5 = parseFloat("0908.5");  //908.5
    var num6 = parseFloat("3.125e7");  //31250000
    ```
#### String类型
- String 类型用于表示由零或多个 16 位 Unicode 字符组成的字符序列，即字符串
- 字符串可以由单引号(')或者双引号(")表示，但是前后引号必须对应
- \用于转义
- 字符串创建后是不可变的，要改变某个字符串的内容，必须先销毁原字符串
- `toString()`方法用于将其他类型的值转换为字符串类型，除了`null`和`undefined`
  * 例：
  ```javascript
  (11).toString();  //"11"  使用()将数字包裹起来
  11..toString();  //"11"  因为第一个点会被解析为小数点，第二个点才是调用方法
  true.toString();  //"true"
  ```
- `toString()`可以传入参数，但是只有在对数字使用`toString()`时才可以传入参数，传的参数是返回的字符串中的数字用什么进制数表示
  * 例：
  ```javascript
  var num = 10;
  num.toString();  //"10"
  num.toString(2);  //"1010"
  num.toString(8);  //"12"
  num.toString(10);  //"10"
  num.toString(16);  //"a"
  ```
- `String()`方法可以将包括null和undefined在在内的所有数据类型转换为字符串类型。
  * 如果值有 `toString()`方法，则调用该方法(没有参数)并返回相应的结果;
  * 如果值是 `null`，则返回"null";
  * 如果值时 `undefined`，则返回"undefined";

#### Object类型
- 创建对象
  * 例：
  ```javascript
  var obj1 = new Object();
  var obj2 = new Object;  //不推荐这种方法
  ```
- Object是所有对象的基础`Object`的每个实例都具有下列属性和方法
  * `constructor`:保存着用于创建当前对象的函数。对于前面的例子而言，构造函数(`constructor`)就是 `Object()`。
  * `hasOwnProperty(propertyName)`:用于检查给定的属性在当前对象实例中(而不是在实例 的原型中)是否存在。其中，作为参数的属性名(`propertyName`)必须以字符串形式指定(例 如:`o.hasOwnProperty("name")`)
  * `isPrototypeOf(object)`:用于检查传入的对象是否是传入对象的原型
  * `toLocaleString()`:返回对象的字符串表示，该字符串与执行环境的地区对应
  * `toString()`:返回对象的字符串表示
  * `valueOf()`:返回对象的字符、串布尔值、或数值。