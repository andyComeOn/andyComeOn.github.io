---
title: js默认行为和事件冒泡
date: 2019-02-12 14:02:02
tags: JS相关
---

#### js默认行为和事件冒泡的几种方法

1. js阻止事件冒泡

~~~javascript
var el = document.getElementById('el');
el.onclick = function (event) {
    if (event && event.stopPropagation) {
        // 如果提供了事件对象，则这是一个非IE浏览器
        event.stopPropagation();
    } else {
        // 否则，我们需要使用IE的方式来取消事件冒泡
        window.event.cancelBubble = true;
        return false
    }
}

~~~

2. js阻止默认事件
~~~javascript
var el = document.getElementById('el');
el.onclick = function (ev) {
    var ev = ev || event;
    if (ev && ev.preventDefault) {
        // 如果提供了默认事件，则这是一个非IE浏览器
        // w3c阻止默认浏览器动作
        ev.preventDefault();
    } else {
        // IE中阻止函数器默认动作的方式
        window.event.returnValue = false;
        return false
    }
}
~~~

3. jq阻止事件冒泡和默认行为

* 阻止事件冒泡
~~~javascript
$("a").click(function (e) {
    e.stopPropagation();
});
~~~

* 阻止默认行为
~~~javascript
$("a").click(function (e) {
    e.preventDefault();
});
~~~

* 阻止默认和冒泡事件

~~~javascript
$("a").click(function (e) {
    return false;
});
~~~

* 针对上面的click()，在括号内直接写`function () {}` 这里面可传入e，ev，event，evok等形参都可以，函数体中直接写`e.preventDefault()`、`ev.preventDefault()`、`event.preventDefault()`、`evok.preventDefault()`等语句都可以生效，切忌是形参不穿，直接写：`e.preventDefault()`、`ev.preventDefault()`、`event.preventDefault()`；还可以是形参不写，使用`event.preventDefault()`（不报错猜想event是关键字）。

* 上面的click()，都是简写的：
~~~javascript
$("el").click(function () {});
~~~
这种方法虽是看着简单，但是针对document中已经存在的元素，动态添加的元素是绑定不上click事件的，所以一是养成好习惯，二是为了避免click绑定不上。固`$('el').on('click', function () {})`这样写。注：在之前老版本jq是bind关键词，后来改为了on。


#### js默认行为有哪些

很多的网页元素都会有默认的行为，比如说当你点击一下超链接a标签的时候，它会有一个跳转的行为；当你在网页上点鼠标右键时会出现一个右键菜；当你在一个form表单里点击提交按钮时网页会产生提交行为并刷新网页，当你网页上滚动鼠标滚轮时，网页的滚动条会动等等。这些都叫事件的默认行为，如果想把这默认行为取消了，相应的JS代码如下：

~~~javascript
    el.onclick=function(){return false;}//在方法里加个return false，就阻止超链接点击时的跳转行为了
    document.oncontextmenu=function(){
        /* 在这里你还可以加一些代码，实现自定义的右键菜单 */
        return false; // 系统自带的右键菜单就失效了
    }
    // 这样表单就不会产生提交行为了
    Form.onsubmit=function(){return false;}
    // IE和chrome的方式，取消鼠标的滚轮的默认行为，网页的滚动条就不会动了
    document.onmousewheel=function(){return false}
    // 功能同上，火狐的方式。火狐只能用DOM二级的绑定方式，并且用e.preventDefault=true
    document.addEventListener('DOMMouseScroll',function(e){e.preventDefault=true});
~~~

我们要知道常见的事件默认行为有那些，并且要知道阻止默认行为，只要在绑定到这个行为事件的方法里最后加一句：return false;就可以了。
但要强调注意的是：如果你的事件绑定是用addEventListener来实现的，那阻止默认行为必须用e.preventDefault=true。
