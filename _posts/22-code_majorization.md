---
title: 一个前端面试题引发的思考
date: 2017-03-03 13:12:09
tags: ['学习']
---
昨晚群上有个小伙伴扔出了一个前端面试题，内容是这样的：
>写个js函数func(str)，传参str为一个字符串，实现把这个字符串语句中的单词（空格隔开的）次序逆序。比如把 I am a coder变成 coder a am I，不允许使用reverse，join，substring，split。

<!-- more -->
如果没有附加条件实现起来很容易，当然加了条件做出这题也不是很难。这都不是重点。重点是我从这个题引发出来的想法，当然这些想法只代表我个人。

昨晚看到题目的时候手上没有电脑，所以不了了之，今天就做了一下。

我写完后的代码是下面这个样子的：
``` javascript
function str2(str) {
            var arr = [];
            var words = '';
            for (var i = 0; length = str.length, i < length; i++) {
                if (str[i] != " " && i != length - 1) {
                    words += str[i];
                } else if (str[i] === " ") {
                    arr.unshift(words);
                    arr.unshift(str[i]);
                    words = "";
                } else if (str[i] != " " && i === length - 1) {
                    words += str[i];
                    arr.unshift(words);
                    words = "";
                }
            }
            var result = "";
            for (var j = 0; length2 = arr.length, j < length2; j++) {
                result += arr[j];
            }
            return result;
        }
```
我的思路是循环传入的字符串，然后根据空格开分割字符串，将空格之间的单词拼成新的小字符串，然后添加到一个数组中，空格单独添加为一个数组元素，最后循环数组，将数组中的元素拼成一个新的字符串。

然后我将自己的代码发到了群上，然后有个人默默的发了个自己的代码的截图出来，看到后我愣了一下，第一反应是：这么短？！然后看了下他的思路，自己敲了一遍，没啥毛病。

这是他的代码：
``` javascript
function strr(start) {
            let temp = '';
            let re = '';
            for (let i = 0; i < start.length; i++) {
                if (start[i] !== ' ') {
                    temp = temp + start[i];
                } else {
                    re = temp + ' ' + re;
                    temp = '';
                }
            }
            re = temp + ' ' + re;
            return re;
        }
```
他的思路和我差不多，也是根据空格判断，但他是遇到空格直接拼成字符串了，而不是像我一样要经过数组，所以就少了一个循环，节省了时间。

虽然两个函数在字符串较短的情况下运行时间差不多，但是一旦字符串变长或者处理大量字符串的时候，运行速度上的差距就会体现出来。我觉得这不是代码能力上的问题，而是思维上的问题。实现一个效果、功能可以有很多种方法，但是能立马想到用一种比较好的方法就不是每个人能做到的了。这就需要拓展自己的思维，多看别人的代码，从别人的代码中学习他人的思维，或者不断地积累，从项目中总结。

**仍需努力,你我共勉。**