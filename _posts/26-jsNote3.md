---
title: JavaScript高级程序设计笔记（三）
date: 2017-03-07 10:00:35
tags: ['学习']
---
#### 引用类型
- 对象
  * 对象拥有多个属性，用逗号隔开，并且最后一个属性后面不能加逗号
  * 访问对象属性的方法有两种，用`.`和用`['XXX']`
    + 例：
    ```javascript
    var obj = {
        'first name' : 'Chen',  //这种情况下属性名要用字符串形式
        age : 10
    }
    obj.age;  //10
    obj['first name'];  //"Chen"
    //方括号形式可以用变量来取值
    var firstName = 'first name';
    obj[firstName];  //"Chen"  
    ```
    <!-- more -->
- 数组
  * 不要使用逗号创建数组某个长度的数组，也不要在数组末尾加逗号，IE和其他浏览器数组长度会产生分歧
    + 例：
        ```javascript
        var a = [,,,,,];  //不要使用这种写法
        var b = [1,2,3,4,];  //不要使用这种写法
        var c = new Array(3);  //使用构造函数创建数组的时候，传入的参数如果只有一个并且是数字，则会创建相应长度的数组
        var d = new Array(1,2);  // [1,2]
        ```
  * 数组元素的读取于修改
    + 例：
        ```javascript
        var num = [1,2,3,4,5,6];
        num[0];  //1
        num[3];  //4
        num[10];  //undefined 不报错

        num[0] = 10;  //修改数组
        num;  //[10,2,3,4,5,6]

        num[6] = 7;  //新增元素
        num;  //[10,2,3,4,5,6,7]

        num.length = 5;  //重新设置数组长度，超出长度的元素会被删除
        num;  //[10, 2, 3, 4, 5]

        num[99] = 100;  //修改数字长度，6-98的元素都是undefined
        num.length;  //100
        ```
  * 操作数组**（例子代码中没有重新声明默认使用之前的变量）**
    + `join()`方法返回字符串，将数组中的元素用指定的元素连接起来
        ```javascript
        var num = [1,2,3,4,5];
        num.join();  //"1,2,3,4,5"
        num.join(' ');  //"1 2 3 4 5"
        num.join('|');  //"1|2|3|4|5"
        ```
    + `push()`方法可以在数组末尾添加新元素，并且返回新数组的长度，也可以一次添加多个元素
        ```javascript
        num.push(6);
        num;  //[1,2,3,4,5,6]
        num.push(7,8);
        num;  //[1,2,3,4,5,6,7,8]
        ```
    + `pop()`方法将移除数组的最后一项，并且返回移除项
        ```javascript
        num.pop();  //8
        num;  //[1,2,3,4,5,6,7]
        ```
    + `shift()`方法将移除数组第一项，并且返回移除项
        ```javascript
        num.shift();  //1
        num;  //[2,3,4,5,6,7]
        ```
    + `unshift()`方法作用类似push(),不过是向数组前端添加一个或多个元素，并返回新数组长度
        ```javascript
        num.unshift(1);  //7 
        num;  //[1,2,3,4,5,6,7]
        ```
    + `reverse()`方法会逆序数组
        ```javascript
        num.reverse();  //[7, 6, 5, 4, 3, 2, 1]
        ```
    + `sort()`方法会将数组中的元素转换为字符串，然后进行比较
        ```javascript
        var num2 = ['a','g','e','v'];
        num2.sort();  //["a", "e", "g", "v"]
        //sort()支持传入一个指定函数来自定义排序顺序，但是遵循以下规则：
            1.若 a 小于 b，在排序后的数组中 a 应该出现在 b 之前，则返回一个小于 0 的值。
            2.若 a 等于 b，则返回 0。
            3.若 a 大于 b，则返回一个大于 0 的值。
        var str = ['basketball','atom','cat','do'];
        str.sort();  //["atom", "basketball", "cat", "do"]
        //如果我们想根据字符串长度来排序，那么我们要写一个比较函数：
        function compare(val1,val2){
            return val1.length-val2.length;
        }
        str.sort(compare);  //["do", "cat", "atom", "basketball"]
        ```
    + `concat()`可以复制目标数组，并且传入新元素到复制后的数组，返回一个新数组，如果不传参数，那么就是单纯的复制一个数组,并且是独立的一个数组
        ```javascript
        var arr = [1,2,3,4,5];
        arr.concat(6);  //[1,2,3,4,5,6]
        ```
    + `slice()`方法可以截取目标数组为一个新数组，接受两个参数，第一个是截取开始位置，第二个是结束位置，但是返回的新数组中不包括结束位置处的元素
        ```javascript
        arr.slice(1,4);  //[2,3,4] 位置4处的元素为5，但是没有取到5
        arr.silice(0);  //[1,2,3,4,5] 只传入起始位置，默认取到最后的元素，如果传入0，则相当于复制整个数组
        ```
    + `splice()`方法能对数组进行删除，插入，替换操作，接受三个参数，第一个是开始删除的位置，第二个是删除的个数，第三个是插入的元素，返回删除的元素，没有删除则返回空数组
        ```javascript
        arr.splice(0,2);  //[1,2];  删除数组前两项
        arr;  //[3,4,5];

        arr.splice(0,0,1,2);  //[]   在数组最开始处新增元素
        arr;  //[1, 2, 3, 4, 5]

        arr.splice(2,1,6);  //[3]  替换索引2处的元素为6
        arr;  //[1, 2, 6, 4, 5]
        ```
    + `indexOf()`和`lastIndexOf()`方法用于查找数组中的某项的索引位置,只是`indexOf()`从第一项开始找，而`lastIndexOf()`从最后一项开始找,若没找到返回-1,`indexOf()`查找时用的是严格相等，可以传两个参数，查找的项和起始查找位置
        ```javascript
        var arr = [1,2,3,4,5];
        arr.indexOf(2);  //1
        arr.lastIndexOf(2);  //1
        arr.indexOf(10);  //-1
        var person = { name: "Nicholas" };
        var people = [{ name: "Nicholas" }];
        var morePeople = [person];
        alert(people.indexOf(person));     //-1  因为person中的{ name: "Nicholas" }和people中的{ name: "Nicholas" }是两个不同的对象，因此严格相等的情况下是不成立的，因此返回-1
        alert(morePeople.indexOf(person)); //0
        ```
    + `every()`方法会对数组中的每一项执行一个给定的函数，如果该函数对每一项都返回true，那么返回true,传入的函数接受三个参数，数组项的值，该项在数组中的位置和数组对象本身
        ```javascript
        var arr = [1,2,3,4,5];
        arr.every(function(item,index,array){
            return (item < 6);
        })  //true
        ```
    + `some()`方法和`every()`方法类似，`some()`方法会对数组中的每一项执行一个给定的函数，如果该函数对任意一项或多项返回true，那就返回true,传入的函数接受三个参数，数组项的值，该项在数组中的位置和数组对象本身
        ```javascript
        var arr = [1,2,3,4,5];
        arr.some(function(item,index,array){
            return (item < 2);
        })  //true
        ```
    + `filter()`方法会对数组中的每一项执行一个给定的函数，返回一个函数执行结果为true项的数组，传入的函数接受三个参数，数组项的值，该项在数组中的位置和数组对象本身
        ```javascript
        var arr = [1,2,3,4,5];
        arr.filter(function(item,index,array){
            return (item < 2);
        })  //[1]
        ```
    + `forEach()`方法会对数组中的每一项执行一个给定的函数，该方法没有返回值
    + `map()`方法会对数组中的每一项执行一个给定的函数，返回每次调用结果组成的数组
        ```javascript
        var arr = [1,2,3,4,5];
        arr.map(function(item,index,array){
            return (item+1);
        })  //[2, 3, 4, 5, 6]
        ```
    + `reduce()`和`reduceRight()`方法会迭代数组所有项，然后构建一个最终返回的值，这两个方法接受两个参数，第一个参数为传入的函数和作为归并的初始值，而传给`reduce()`和`reduceRight()`的函数接受四个参数：前一个值，当前值，项的索引和数组对象
        ```javascript
        var arr = [1,2,3,4,5];
        arr.reduce(function(pre,item,index,array){
            return (pre+item);
        })  //15
        var arr = [1,2,3,4,5];
        arr.reduce(function(pre,item,index,array){
            return (pre+item);
        },10)  //25
        ```
#### Function
- 函数定义方式
  + 函数声明
    ```javascript
    function a(){
        //code...
    }
    ```
  + 函数表达式(这种方式定义的函数需要在结尾加`;`)
    ```javascript
    var a = function (){
        //code...
    };
    ```
  + 函数声明方式定义的函数的函数，声明可以写在调用之后，因为无论写在哪，在执行前声明的函数都会被提升到顶部，然后执行下面的代码。而表达式定义的函数不行。
    ```javascript
    fn();
    function fn(){
        alert("Hello!");
    }   //正常执行
    
    fn2();
    var fn2 = function(){
        alert("World!");
    }  //报错
    ```
  + 函数中的`this`指向调用的对象，如果全局下调用，则`this`指向`window`，如果是某个对象调用则是指向这个对象
  + 函数有两个属性，`length`和`prototype`，`length`代表函数传入参数的个数，而`prototype`是保存引用类型所有实例方法的真正所在。像`toString()`和`valueOf()`等方法都保存在这个属性中。`prototype`属性不可枚举，无法用`for in `发现。
  + 每个函数都包含两个不是通过继承的来的方法`call()`和`apply()`，关于这两个方法的详细解释可参考[apply和call的区别](http://tit1e.xyz/2017/03/03/25.apply-and-call/)