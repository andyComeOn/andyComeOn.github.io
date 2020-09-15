---
title: html一些不常用标签
date: 2019-02-12 16:13:52
tags:
---

#### 记录一下html的一些不常用标签

* blockquote顾名思义是引用引述的意思，它有默认的css，请看下面的代码：

~~~html
Here comes a long quotation:
    <blockquote class="quote1" cite="http://www.baidu.com" style="background: #ccc;">
        This is a long quotation. This is a long quotation. This is a long quotation. This is a long quotation. This is
        a long quotation.
    </blockquote>
~~~

渲染效果如下：

![blockquote-formal](http://file.798run.top/img/blog/20190212/blockquote-formal.jpg)



* 在layui中看到有衍生使用例子，请看下面的代码：

~~~html
Here comes a long quotation:
    <blockquote class="quote1" cite="http://www.baidu.com" style="margin: 0; padding: 15px; border-left: 5px solid #009688;">
        This is a long quotation. This is a long quotation. This is a long quotation. This is a long quotation. This is
        a long quotation.
    </blockquote>
~~~

渲染效果如下：

![blockquote](http://file.798run.top/img/blog/20190212/blockquote.jpg)

---

* 在开发过程中遇到fieldset和legend标签，细查之后发现这个是form中的分组，请看下面的代码：

~~~html
    <form>
        <fieldset class="">
            <legend class="">标题标题标题标题标题标题标题标题</legend>
            身高：<input type="text" />
            体重：<input type="text" />
        </fieldset>
    </form>
~~~

展示效果如下：

![fieldset1](http://file.798run.top/img/blog/20190212/fieldset1.jpg)

这里面的边框是fieldset的。

* fieldset的衍生写法，代码如下：

~~~css
.fieldset {
    padding: 0;
    margin: 0;
    border: 0;
    border-top: 1px solid #e5e5e5;
    text-align: center;
}
.legend {
    padding: 0 6px;
    line-height: 30px;
    background: #009688;
}
~~~
~~~html
<form>
    <fieldset class="fieldset">
        <legend class="legend">标题标题标题标题标题标题标题标题</legend>
        身高：<input type="text" />
        体重：<input type="text" />
    </fieldset>
</form>
~~~

效果如下：
![fieldset2](http://file.798run.top/img/blog/20190212/fieldset2.jpg)
针对这种衍生写法，可以直接使用它做标题使用，但是标题内容不能太长否则会把上边框覆盖了。在网页上也找到fieldset相关的帖子：[fieldset——一个不常用的HTML标签](https://www.cnblogs.com/theWayToAce/p/5456208.html)



我在写这个文档时候，突然想到weui中的底部标题居中，打开网页看代码一开他是父元素写的border-top，里面的内容标签是使用定位`relative`，还纠正了我的理解`relative` 并不能把inline元素转变为块元素，只有`absolute`，`float`能把非block元素转变为block元素。

代码如下：

~~~css
.demo {
    width: 200px;
    height: 24px;
    margin: 40px auto;
    border-top: 1px solid red;
    text-align: center;
    /* position: relative; */
}
.demo span {
    display: inline-block;
    /* display: block; */
    /* position: absolute; */
    position: relative;
    line-height: 24px;
    top: -12px;
    padding: 0 .55em;
    background-color: #FFFFFF;
    color: #808080;
}
~~~
~~~html
<div class="demo"><span>你好</span></div>
~~~

展示效果如下：

![demo](http://file.798run.top/img/blog/20190212/demo.jpg)


分割线

---

分割线

1. 在css中html和body是有区别的，大家常对body写背景色，若对html上写背景色#ccc，这时查找body中css的background属性也没有继承#ccc，但是整个可视区的背景色确是#ccc。猜想：这是浏览器的机制所致。

2. body中对其写height: 100%;是不起作用的！可这样写` body { height: 100vh; } `，这样body的高度就是整个可视区。 vh这个单位我也是使用的不多，后来发现它是针对可视区的高度，在某些场景很实用。如：在移动端的actionsheet中高度可设置为80vh，在对其加上滚动属性，效果很实用。如下图： 

![actionsheet](http://file.798run.top/img/blog/20190509/actionsheet.jpg)

接着上面的聊： body写height: 100%;无效。但html写height: 100%;高度却可以铺满整个屏。这个时候，在对body写height: 100%;body的高度也铺满整个屏，其实这里面的原因：html的height: 100%;生效 => 高度铺满屏 => body继承了其高 => 高度也铺满整个屏。

总结：有些小伙伴就是直接写：
~~~css
html, body {
    height: 100%;
}
~~~
其实这里面body的高度是继承了html而来！











































