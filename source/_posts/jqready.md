---
title: jq中ready函数总结
date: 2019-02-12 11:14:09
tags:
---

#### 记录一下jq的ready的几种写法

1. 最常用也是最标准

代码如下：

~~~javascript
$(document).ready(function(){
    // todo sth
})
~~~

2. 是上面的简写

~~~javascript
$(function(){
    // todo sth
})
~~~

这种写法可能有很多人有疑问，jq的源码中：

~~~javascript
// jQuery的构造函数； 
var jQuery = function( a, c ) { 
// $(document).ready()的简写形式，只有在$(function(){...})下才会执行； 
if ( a && typeof a == "function" && jQuery.fn.ready ) return jQuery(document).ready(a); 
// 确保参数a非空，默认值为document； 
a = a || jQuery.context || document; 

// 耶！找到了,我们再看下$这个方法的参数 
// $(selector,context) 
// 第一个为选择器,第二个是容器 
// 如果不填就默认为document k
~~~

3. 这个帖子上说是打酱油的

~~~javascript
    jQuery(document).ready(function(){
        // todo sth
    })
~~~

4. 看清楚

~~~javascript
jQuery(function($){
    // todo sth
})

~~~

这种写法是为了避免其他插件中也使用了美元符号$，造成冲突，才传入参数，通过`jQuery.noConflict()`这个方法,我们可在代码中通过jQuery来代替$来使用,但又习惯了使用$怎么办?看下面代码: 
~~~javascript
jQuery.noConflict(); 
jQuery(function($){ 
    alert($("#ready1").html()); //我们又能用上$符号了 
}); 
~~~
上面的第二个jQuery不能使用$。

