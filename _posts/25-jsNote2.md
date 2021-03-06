---
title: JavaScript高级程序设计笔记（二）
date: 2017-03-06 13:58:35
tags: ['学习']
---
#### 操作符
- `++`和`--`操作符
  * `age++`和`++age`的区别在于`age++`是先运算后+1，而`++age`是先+1后运算,`--`操作同理
    ```javascript
      var age = 18;
      var nextAge = age++;
      age;  //19;
      nextAge;  //18
      //上面的运算结果顺序其实是这样的：
      var age = 18;
      var nextAge = age;
      age = age + 1；
    ```
  ----------------------------------
      var age = 18;
      var nextAge = ++age;
      age;  //19
      nextAge;  //19
      //这次的运算结果是这样的：
      var age = 18;
      age = age + 1；
      var nextAge = age;
    ```
  <!-- more -->
    ```
- `+`和`-`操作符
  * `+`和`-`操作符放在数字前面，`+`操作符对数字无影响，`-`操作符是将数字变为负数
  * `+`和`-`操作符放在非数字前面，则这两个操作符会像`Number()`一样对这个值进行转换
  * `+`操作符用一个数字和一个字符串相加的时候，数字会先被转换为字符串，然后进行拼接操作
  * 例：
    ```javascript
      var a = -true;
      a; //-1
      var b = +true
      b;  //1
      var c = 1 + '1';
      c;  //"11"
      var d = '1' + 1;
      d;  //"11"
    ```
- `=`、`==`和`===` 
  * `=`表示赋值
  * `==`只比较左右两边的值，不比较数据类型
  * `===`全等，比较左右两边的值和数据类型
  * 例：
    ```javascript
      var a = 5;
      a;  //5
      5 == '5';  //true
      5 === '5'  //false
      5 === 5;  //true
    ```
#### 语句
- `continue`和`break`语句
  * 循环中`continue`会立即停止当前循环，从头开始执行循环
  * 循环中`break`会立即**退出**当前循环，并执行循环后面的代码
  * 例：
    ```javascript
    var num;
    for(var i = 0;i<10;i++){
      if(i==5){
          break;
          }
      num = i;
    }
    alert(num); //4
    ----------------------
      var num;
      for(var i=0;i<10;i++){
          if(i==5){
              continue;
          }
      num =i;
      }
    alert(num);  //9
    ```
- `switch`语句
  * `switch`语句中每个case后面都需要加一个`break;`
  * `default`表示没有上述任何条件匹配是默认执行的动作，同`if`的`else`

#### 函数
- `argunments`对象
  * `argunments`对象是一个类数组，里面存的是调用函数时传入里面的参数，可以在函数内部直接获取到它，并且可以用获取数组元素的方法获取传入的参数
    + 例：
      ```javascript
      function fn(){
          console.log(arguments,arguments.length,arguments[0]);
      }
      fn(1,2,3,4,5);  //[1,2,3,4,5] 5  1
      ```
  * 如果出现两个同名函数，则后定义的函数有效，前定义的函数无效
#### 基本类型和引用类型
- 定义
  + 基本类型：简单的数据段，如Undefined，Null，Number，String，Boolean类型的值，栈内存存放这些对象的地址
  + 引用类型：有可能由多个值构成的对象，如Object，Function，Array等类型的值，堆内存存放这些具体对象
  + 例：
    ```javascript
    var name = "Tit1e";  //基本类型
    var obj = new Object();  //引用类型
    ```
- 复制变量值
  + 将基本类型复制给一个另一个变量，这两个变量互相独立，原变量重新赋值都不会影响复制后的变量
  + 将引用类型复制给一个另一个变量，改变饮用类型值的内容，复制的值也会相应的改变
    * 例：
      ```javascript
      //基本类型复制
      var a = 123;
      var b = a;
      a = 456;
      a;  //456
      b;  //123

      //引用类型复制
      var c = new Object();
      c.name = "Tit1e";
      var d = c;
      c.name;  //"Tit1e"
      d.name;  //"Tit1e"
      c.name = "Evol";
      c.name;  //"Evol"
      d.name;  //"Evol"
      ```
- 函数传递参数是按值传递，也就是说传入函数的值只是一个副本，原值并不会有任何变化
    * 例：
      ```javascript
          var b = 1;
          function a(num){
              return num += 1;
          }
          var c = a(b);
          b;  //1
          c;  //2
          function a(obj){
              obj.name = "Tit1e";
              obj = new Object();
              obj.name = "evol";
              return obj;
          }
          var b = new Object();
          var c = a(b);
          b;  //"evol"
          c;  //"Tit1e"
      ```
- 检测类型
  * `instanceof`用于检测类型
    + 例：
      ```javascript
      var a = new Object();
      a instanceof Object;  //true
      a instanceof Array;  //false
      ```