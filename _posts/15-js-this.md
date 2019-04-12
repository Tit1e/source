---
title: this指向与闭包原理
date: 2016-11-08 22:42:43
tags: ['学习笔记']
---
今天理解两个难点，闭包和`this`指向，闭包其实现在还是似懂非懂，需要有空运用一下闭包可能会更加理解。

先说下`this`指向问题：
``` javascript
var name = "The Window";
var object = {
    name : "My Object",
　　getNameFunc : function(){
　　　　return function(){
　　　　　　return this.name;
　　　　};
　　}
};
console.log(object.getNameFunc()());//The Window
console.log(object.getNameFunc();
```
学习闭包的时候这段代码看得我很懵，想不清楚为什么this是指向window的。那么我们先了解一下`this`指向问题。

`this`的指向在函数定义的时候是确定不了的，**只有函数执行的时候才能确定**`this`到底指向谁，实际上`this`的最终指向的是`this`所在函数的**上一级对象**。
<!--more-->
上面这句话非常重要，这是`this`非常重要的特点：
``` javascript
function test(){
    this.x = 1;//给对象写入属性x=1,如果this指向全局，则表示创建全局对象x=1
    console.log(this.x);
}
test(); // 1
x;  //1
```
如果这不够直观，那么换种更加直观的写法：
``` javascript
var x = 1;
function test(){
　　this.x = 0; //表示将this指向的对象中的x的值修改为0
}
test();
console.log(x); //0，证明全局中声明的x值被修改了，说明this指向window
```
上面是直接在全局调用函数，`this`指向`window`对象，函数还可以作为对象的方法调用，
``` javascript
function test(){
    console.log(this.x);
}
var o = {};
o.x = 1; //此时o = {x:1}
o.m = test; // 此时o = {x:1,m:test}
o.m(); // 1 说明test执行的时候this指向的是o这个对象，所以才能获取到x
```
再来看个例子
``` javascript
var o = {
    a:10,
    b:{
        a:12,
        fn:function(){
            console.log(this.a); //12 这里可以看出this指向的b对象而不是o对象，所以this只指向它的上一级对象
        }
    }
}
o.b.fn();
```
对上面函数进行下稍微的改造
``` javascript
var o = {
    a:10,
    b:{
        a:12,
        fn:function(){
            console.log(this.a); //undefined
            console.log(this); //window
        }
    }
}
var j = o.b.fn;
j(); //当将方法赋值给一个全局变量后再调用，此时就是window对象调用，由于全局没有a变量，所以this.a返回undefined,this返回window
```
这样看下来后应该对`this`的指向有一个基本的了解了。接下来说闭包：

文章开头引用的代码其实就是一个闭包，是我在看阮一峰闭包博客的时候看到的代码，开始是想了解闭包的，接过被带偏跑去看`this`指向了。
``` javascript
function f1(){
　　var n=999;
　　function f2(){
　　　　alert(n);
　　}
　　return f2;
}
var result=f1();
result(); // 999
```
将`f2`返回出来后，`f2`处于全局环境中，但是由于Js是静态作用域，变量的作用域在声明的时候就已经确定并且不会再改变，所以`f2`函数执行的时候作用域还是`f1`，因此`f2`在全局环境下执行的时候还是可以访问到`f1`中的变量，而`f2`执行后返回的值，由于`f2`处于全局变量，所以返回出来的值也在全局环境中，但是返回后这个值一直存在于全局环境中，而这个值的存在依赖于`f2`，`f2`又依赖于外部的`f1`，所以`f1`函数在调用完成后不会被内存回收机制回收，一直存在于内存中。
``` javascript
function f1(){
　　var n=999;
　　nAdd=function(){n+=1}
　　function f2(){
　　　　alert(n);
　　}
　　return f2;
}
var result=f1();
result(); // 999
nAdd();
result(); // 1000
```
上面代码中`f1`函数调用了两次，返回的结果都是正常的，说明`var n = 999;`一直在内存中，如果被回收了那么第二次调用的时候局部变量n就应该无法被访问到。

参考链接：[码农社](http://www.codeceo.com/article/javascript-this-pointer.html)
[Javascript的this用法](http://www.ruanyifeng.com/blog/2010/04/using_this_keyword_in_javascript.html)
[学习Javascript闭包（Closure）](http://www.ruanyifeng.com/blog/2009/08/learning_javascript_closures.html)