---
title: js中的prototype总结
date: 2019-04-12 13:56:28
tags: JS相关
---


序： 在看js的几种继承方式相关文档时候，看到原型(prototype)继承，之前也看到有关的prototype的相关知识，看了就忘了，现自己总结一下。

1. 我们大家在平时写代码时候，一般都是：

~~~javascript
function foo() {}
~~~

但是在看到一些封装的函数一般是：

~~~javascript
function Foo() {}
~~~

后来自己的理解：封装的一些函数，一般是采用的大写命名，醒目、清晰、大家约定俗称（这里针对js，jq的传统js写法）。自己写的函数一般小写命名，固函数大小写命名区别不大。

下面说一下prototype，看代码：

~~~javascript
function Person(name) {
    this.name = name;
    this.foo = function () {};
}

var p1 = new Person('Byron');
~~~
我们把p1称为Person的一个实例，new Person('Byron')，可理解为构造函数。 针对Person函数，我们在函数体中写了
` this.name = name; `,` this.foo = function () {}; `, 这意思就是给Person声明了2个属性 ` name `、` foo `（这是我们通过代码增加的属性）；只要Person函数已经声明了，它就有一个属性prototype（是一个对象-包含1个默认的属性` constructor `）,这个 ` constructor `又指向了Person函数本身！

再来说p1，p1详见上面的代码，p1它具有了2个挂载属性：` name: 'Byron' `， ` foo: function(){}  ` 和一个默认的 ` __proto__ `，这个` __proto__ ` 指向的是Person的` prototype `。而Person的` prototype `又指向自己的` constructor `。 看起来有点绕，每次脑子就乱乱的，那么就看下面图：

![prototype解释图](https://images2015.cnblogs.com/blog/960109/201607/960109-20160712231541998-1991573736.png)

我想只要大家没事就看看上面的图，不断温习就有印象了。

---

针对上面p1它其中一个属性name，其属性值是传的实参'Byron'，他的foo属性是一个函数，这就是继承了父的属性。
若：
~~~javascript
var p2 = new Person('Frank');
~~~
p2同样拥有了name、foo属性。我们可以对p2拥有的属性进行改动` p2.f00 = []; `, 请看下面的代码：
~~~javascript
console.log(p2.f00);  // []
console.log(p1.f00);  // function(){}
~~~
上面的代码打印结果显示， 虽然是p1和p2都是Person的实例，但是他们好像是把Person的属性深copy一份，彼此相互独立！

针对p1,p2这种构造的Person(),他们都有一个__proto__ 属性，好像这种情况，这2个变量才有__proto__ 属性







