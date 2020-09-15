---
title: eval总结
date: 2019-03-15 10:07:55
tags:
---

#### eval的一些总结

##### eval的用法

1. 在很多的js代码中，可以看到有eval的身影，在菜鸟教程中：“eval() 函数计算 JavaScript 字符串，并把它作为脚本代码来执行。”这个言外之意是处理字符串。可是下面还有一句话：“如果参数是一个表达式，eval() 函数将执行表达式。如果参数是Javascript语句，eval()将执行 Javascript 语句。” 总结一下就是eval()无论你是处理什么，他就会执行，大不了执行不了的就报错！

2. 在w3school中写 ：eval() 函数可计算某个字符串，并执行其中的的 JavaScript 代码。eval(string)，既然都是这样定义的，那为什呢会让他执行什么语句啥的呢？ 我个人猜想一定是后来把js引擎中的eval()处理方法更改了，赋予它更多的功能。

3. eval的巧用

    * 如果data是字符串，使用eval("("+data+")")可以将其转换为json对象，和JSON.parse的功能一样。
    如果data是json对象，使用eval("("+data+")")会报错，正如你描述的错误。eval一个json对象，没有什么作用，这个时候不需要使用eval方法，直接用data即可。

    * 所以，如果你那边能确定后台返回的是字符串，就使用eval("("+data+")")(eval会带来很多问题，不建议使用，如果想实现转化用JSON.parse更好)，如果后台返回的是json对象，什么操作都不需要，直接使用data即可。如果你是用的jquery, 将type（一般为这个配置属性）设为json，或者利用$.getJSON()方法获得服务器返回，那么就不需要eval()方法了，因为这时候得到的结果已经是json对象了，只需直接调用该对象即可

    * 另外多说一点：为什么eval要添加括号呢？原因：eval本身的问题。 由于json是以{}的方式来开始以及结束的，在JS中，它会被当成一个语句块来处理，所以必须强制性的将它转换成一种表达式。加上圆括号的目的是迫使eval函数在处理JavaScript代码的时候强制将括号内的表达式(expression)转化为对象，而不是作为语句(statement)来执行。举一个例子，例如对象字面量{}，如若不加外层的括号，那么eval会将大括号识别为JavaScript代码块的开始和结束标记，那么{}将会被认为是执行了一句空语句。

    ~~~javascript
    console.log(eval("{}"); // undefined 
    console.log(eval("({})");// object[Object] 
    ~~~
    * 这使我想到为什么有时候后台返回的数据是一个字符串，我使用`JSON.parse()`处理（目的转为json对象使用）之后会出现：`Uncaught SyntaxError: Unexpected token r`，这里报错的根本原因是你后台返回的数据不是标准格式，以至于`JSON.parse()`解析不了。上面无序列表第二条是说用jq的json方法，我认为这是jq里面帮你处理数据了。所以大家只要是各个工种都按照严格的规范来，怎么不出现这个问题呢？

4. 我来做最后的总结：记住，eval()解析字符串参数时,会把参数以执行语句的方式返回，请看下面例子。

例子1：

~~~javascript
var str = 2;
var str = '2';
console.log(eval(str));  // 2
~~~

上面会输出2，但是这样做没有什么意义。2和'2'都被eval作为参数以执行语句的方式返回。

例子2：

~~~javascript
var str = 'a:3';
var str = 'a=3';
console.log(eval(str));  // 3
~~~

上面会输出3，我认为这和例子1差不多，'a:3'和'a=3'也都被eval作为参数以执行语句的方式返回。

例子2：

~~~javascript
var str = 'a:3';
var str = 'a=3';
var str = '{a:3}';
console.log(eval(str));  // 3
~~~

上面会输出3，我认为这和例子1差不多，'a:3'、'a=3'、'{a:3}'也都被eval作为参数以执行语句的方式返回。但是这是简单的字符串，请看下面的例子。

例子3：

~~~javascript

var str = '{a:3,b:4}';
console.log(eval(str));  // Uncaught SyntaxError: Unexpected token :
~~~

为什么报错了，我理解的是eval处理不了，本身这是`'{ , }'`这是对象，比较复杂，在eval解析的时候，解释器遇到{},它会认为这个是一个代码块，会执行它，处理不了报错，有人说例子2中为什么可以，我认为例子2中固然有'{a:3}'例子，但是该对象只有一对键值对。比较简单。复杂的`'{ , }'`，解释器把它当成代码块处理就会出错，我们为此用一个()包裹起来，把代码块转为表达式，这样解释器就把`'{ , }'`作为一个对象正常解析。

例子4：

~~~javascript

var str = '{}';
console.log(eval(str));  // undefined 由于他也是简单的对象，所以js输出了undefined
console.log(eval('('+str+')'));  // {} ,把代码块加上()，当做表达式来处理，固输出{}

~~~

例子5：

~~~javascript
eval("x=10;y=20;console.log(x*y)");

~~~
这种可以执行，打印出200。

5. 详细见这篇帖子[关于eval（data）和eval("("+data+")")](https://segmentfault.com/q/1010000006965715)













