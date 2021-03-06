---
title: 详解prototype与__proto__
date: 2016-11-28 19:51:59
tags: ['学习']
---
### `prototype`与`__proto__`详解
`prototype`和`__proto__`在JS中一直是一个比较头痛的东西。JS的继承是基于原型的继承。

在JavaScript中，我们创建一个函数a(就是声明一个函数), 那么浏览器就会在内存中创建一个对象b，而且每个函数都默认会有一个属性 `prototype` 指向了这个对象( 即：`prototype`的属性的值是这个对象 )。这个对象b就是函数a的原型对象，简称函数的原型。这个原型对象b 默认会有一个属性`constructor`指向了这个函数a ( 意思就是说：`constructor`属性的值是函数a)。

代码：
```javascript
function a(){}
a;//function a(){}

//创建一个函数a，在chrome控制台中打印这个函数，显示的就是这个函数本身，
//但实际上这个函数有一个隐藏属性prototype，并且prototype是一个对象，
//prototype中还有一个constructor属性，constructor指向构造函数（这里指向a函数）。

//所以实际上a函数的结构是这个样子的：
function a(){
    prototype:{
        constructor:function a(){}
    }
}
```
<!--more-->
当使用构造函数a创建对象b后，b对象会自动添加一个不可见的`prototype`属性，这个属性指向构造方法的原型对象，即`a.prototype`。但是个别浏览器（chrome、火狐支持（其它浏览器暂不清楚），IE不支持）提供了对这个属性的访问方式，即`__proto__`。
代码：
```javascript
var b = new a();

//此时b对象的结构：
b = {
    __proto__:{
        constructor:function a(){}
    }
}

b.prototype //undefined
b.__proto__ //Object {}
```
所以用a构造函数创建的对象原型都指向`a.prototype`，因此修改`a.prototype`，通过a函数创建的对象也会受到影响。
代码：
```javascript
a.prototype.sex = "boy";

//此时的a函数结构：
function a(){
    prototype:{
        constructor:function a(){},
        sex:"boy"
    }
}

//此时b对象的结构：
b = {
    __proto__:{
        constructor:function a(){},
        sex:"boy"
    }
}

//由于b函数的原型指向a.prototype，因此b.__proto__.sex也也可以访问sex属性
b.__proto__.sex //"boy"
//需要说明的是，如果我们访问一个对象的属性，如果在对象中找到了属性，
//就会直接返回，如果没有找到则会去对象指向的原型中找，如果原型中找到，
//则返回，因此，b.sex也可以访问到sex属性
b.sex //"boy"
//如果向b对象添加一个sex属性，那么新添加的sex属性会屏蔽原型中的sex属性
b.sex = "girl"

//此时b对象的结构：
var b = {
    sex:"girl",
    __proto__:{
        constructor:function a(){},
        sex:"boy"
    }
}

b.sex //"girl"
//这个时候如果要获取原型中的sex属性需要通过b.__proto__.sex
b.__proto__.sex //"boy"
```
因为`prototype`可以修改，所以你也可以根据需要指定新的对象为a函数的原型对象
```javascript
a.prototype = {
    name:"jack",
    age:"19"
}

//此时的a函数结构：
function a(){
    prototype:{
        name:"jack",
        age:"19"
    }
}

//这时你会发现默认指向构造函数的constructor属性没有了。
//如果你需要constructor重新指向构造函数，则需要手动添加。
a.prototype.constructor = a;

//此时的a函数结构：
function a(){
    prototype:{
        constructor:a;
        name:"jack",
        age:"19"
    }
}

a.prototype.constructor //function a(){}
```

### hasOwnProperty方法
从上面我们已经知道了一个对象的属性可以来自自身也可以来自原型，那么怎么区分呢？`hasOwnProperty`方法就是用来区分某个属性是否来自自身。
```javascript
b.sex //"girl" //这是我们自己添加的属性，不是从原型中继承过来的
b.hasOwnProperty("sex") //true
b.__proto__.name //undefined 为什么这里会出现这种情况呢？
//原因是之前执行了：
a.prototype = {
    name:"jack",
    age:"19"
}
//这个操作将整个prototype对象替换了，因此现在的prototype已经不是之前的prototype了，
//而b还是指向之前的prototype，而之前的prototype中没有name属性，因此返回undefined。
//如果此时再创建一个c对象：
var c = new a()
c.__proto__.name //"jack"
//此时创建的对象原型指向新的a.prototype属性，因此可以访问到name属性。
//那么怎样能让b的原型重新指向a.prototype呢？
//只要执行：
b.__proto__ = a.prototype
b.__proto__.name //"jack"这样b就可以访问到新的属性了。
//回归原题
b.hasOwnProperty("name") //false 因为name在原型中，所以返回false
```

### in 操作符
in操作符用来判断一个属性是否存在于这个对象中。查找顺序为现在对象自身中查找，自身找不到就到原型中查找，所以只要对象的原型中存在属性，`in`操作都返回`true`。
```javascript
"sex" in b //true
"name" in b //true
```

### 组合原型模型和构造函数模型创建对象

在构造函数中添加的属性和方法，每个对象都有自己独有的一份，大家不会共享。这个特性对属性比较合适，但是对方法又不太合适。因为对所有对象来说，他们的方法应该是一份就够了，没有必要每人一份，造成内存的浪费和性能的低下。
```javascript
    function Person() {
        this.name = "李四";
        this.age = 20;
        this.eat = function() {
            alert("吃完东西");
        }
    }
    var p1 = new Person();
    var p2 = new Person();
    //每个对象都会有不同的方法
    alert(p1.eat === p2.eat); //fasle
```
解决方法是将方法提取出来：
```javascript
    function Person() {
        this.name = "李四";
        this.age = 20;
        this.eat = eat;
    }
    function eat() {
        alert("吃完东西");
    }
    var p1 = new Person();
    var p2 = new Person();
    //因为eat属性都是赋值的同一个函数，所以是true
    alert(p1.eat === p2.eat); //true
```

但是上面的这种解决方法具有致命的缺陷：封装性太差。eat函数是暴露在全局中的。
原型模式适合封装方法，构造函数模式适合封装属性，综合两种模式的优点就有了组合模式。

```javascript
    //在构造方法内部封装属性
    function Person(name, age) {
        this.name = name;
        this.age = age;
    }
    //在原型对象内封装方法
    Person.prototype.eat = function (food) {
        alert(this.name + "爱吃" + food);
    }
    Person.prototype.play = function (playName) {
        alert(this.name + "爱玩" + playName);
    }
    var p1 = new Person("李四", 20);
    var p2 = new Person("张三", 30);
    p1.eat("苹果");
    p2.eat("香蕉");
    p1.play("志玲");
    p2.play("凤姐");
```

### 动态原型模式创建对象

```javascript
    //构造方法内部封装属性
    function Person(name, age) {
        //每个对象都添加自己的属性
        this.name = name;
        this.age = age;
            //判断this.eat这个属性是不是function，如果不是function则证明是第一次创建对象，
            //则把这个funcion添加到原型中。
            //如果是function，则代表原型中已经有了这个方法，则不需要再添加。
            //perfect！完美解决了性能和代码的封装问题。
        if(typeof this.eat !== "function"){
            Person.prototype.eat = function () {
                alert(this.name + " 在吃");
            }
        }
    }
    var p1 = new Person("志玲", 40);
    p1.eat();
```
内容参考：[http://blog.csdn.net/u012468376/article/details/53121081](http://blog.csdn.net/u012468376/article/details/53121081)
