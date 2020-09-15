---
title: js的几种继承
date: 2019-04-11 14:41:48
tags: JS相关
---

1. js的构造函数继承

* 我所理解的就是在原有函数声明之后，new了一次。

~~~javascript
var el = new Foo();
~~~

这个时候el 就继承了Foo函数的prototype。
