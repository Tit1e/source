---
title: apply和call的区别
date: 2017-03-03 20:01:15
tags: ['学习']
---

今天看原生js的时候看到了`call`和`apply`,对这两个方法印象不是很清楚，只记得这两个方法很相似，作用大概是让A通过`call`或`apply`能够调用B独有的方法，但是具体的使用方法已经模糊，所以网上查了资料重新理了一遍。

先看代码：
``` javascript
    function Createpeople(name, age) {
                this.name = name;
                this.age = age;
            }
    Createpeople.prototype.say = function() {
                console.log('Hello!My name is ' + this.name);
            }

    var Amy = new Createpeople('Amy', 18);
    Amy.say();  //Hello!My name is Amy

    var john = {
        name: 'john',
        age: 20
            }
    john.say();  //john.say is not a function
    Amy.say.call(john);  //Hello!My name is john
    Amy.say.apply(john);  //Hello!My name is john
```

<!-- more -->
`Createpeople`是一个构造函数，并且在原型中写入了一个`say`方法。我们用`Createpeople`创造了`Amy`对象，所以`Amy`理所当然地可以调用`say`方法。而`john`是我们手动创建的一个对象，所以`john`调用`say`方法理所应当会报错。那么如何让`john`能够调用`say`方法呢？那就要通过`call`方法或`apply`方法了。通过`Amy.say.call(john)`，从输出结果来看`john`正确的调用了`say`方法,`apply`也同样。

为什么呢？原因就在于`call`和`apply`在调用的时候会传入一个调用对象，即`this`，它们能将原来不指向传入对象的函数的`this`强行指向传入对象。可能这样说不是很明白，下面我来解释一下：

在其他方法执行的时候会默认传入一个调用对象，比如在`Amy`调用`say`方法的时候，默认传入的调用对象为`Amy`，`say`方法作为`Amy`的一部分正常情况下因为正常情况下`this`只能指向`Amy`，但是通过`call`和`apply`方法能让`say`方法的`this`强行指向传入的`john`，`this`变成了`john`，理所当然，`say`方法就能正常执行了。

#### **那么`call`和`apply`有什么区别呢？**
区别在于**参数**的传入方式不同。
举个例子：
``` javascript
    function add(a, b) {
                return console.log(a + b);
            }

    function sub(c, d) {
                return console.log(c - d);
            }

    sub.apply(add, [2, 3]);  //-1
    sub.call(add, 2, 3);  //-1
    add.apply(sub, [1, 2]);  //3
    add.call(sub, 1, 2);  //3
```
`call`方法参数是一个一个传的，而`apply`方法，除了`this`，其他参数是用数组的方式传的。
由于`apply`传数组这个特点，我们可以很方便地对数组进行某些操作，比如拼接两个数组：
``` javascript
        var list1 = [1, 2, 3];
        var list2 = [4, 5, 6];
        [].push.apply(list1, list2);  //6
        console.log(list1);  //[1, 2, 3, 4, 5, 6]
```
`push`方法可以传很多参数，但是不能传数组，正常情况下凭借数组只能够通过循环一个一个添加，但是`apply`正好需要把多个传入的参数打包成数组，所以正好满足了条件。

