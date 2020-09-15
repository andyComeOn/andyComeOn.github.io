---
title: h5中img标签的边框问题
date: 2019-06-10 13:22:11
tags:
---

### 总结一下h5中img标签的边框问题

我们常在一些重置css如：reset.css、normaize.css 看到：` border-style: none; border: 0; `。在网页中搜到img标签的边框问题。自己总结如下：

1. 网上说img有边框问题，我在chrome（版本 74.0.3729.169（正式版本） （64 位））中测试无发现此bug。
2. 网上说解决边框问题方法：
~~~css
img[src=""],
img:not([src]){
    opacity:0;
}
~~~
确实可以解决部门场景的img边框，但这是有条件的。上面css代码指：该img标签无src属性或src属性值为空。试想一下写前端代码的小伙伴谁会忘记写src属性？若忘记写src属性，则图片渲染不出来，Ta也会发现该bug！若开发小伙伴就是如下写法：
~~~css
.imgWrap {
    height: 64px;
	background: antiquewhite;
}
.imgSrc {
    width: 64px;
    height: 64px;
}
~~~
~~~html
<div class="imgWrap">
    <img class="imgSrc" src="" />
</div>
~~~
渲染效果：
![opacity=0效果](http://file.798run.top/img/blog/20190610/img_src1.jpg)

确实img图片无渲染，无边框。opacity:0;把img占位区的透明度给置为0，所以不展示边框。

---

3. 正常情况，对img写了src属性，可是图片没有加载过来，2中的解决方法依然是有边框。代码如下：
~~~css
.imgWrap {
    height: 64px;
	background: antiquewhite;
}
.imgSrc {
    width: 64px;
    height: 64px;
}
~~~
~~~html
<div class="imgWrap">
    <img class="imgSrc" src="favicon.ic" />
</div>
~~~
网上说的方法大多不行，自己思考你给图片写了宽高，但图片没有加载出来， 浏览器就按照你写的宽高给img添加默认的边框。
![无加载出图片依旧有边框](http://file.798run.top/img/blog/20190610/img_src2.jpg)


默认的没有加载出的图片大小是16x16：
![无加载出默认图片大小](http://file.798run.top/img/blog/20190610/img_src3.jpg)


但是一般情况大家都会对图片写宽高啊! 最后得出：只对图片写宽（刚好图片有一个机制是宽度确认高自己是适配），加载不出来，也有一个默认16x16的图片提示，本来浏览器就是这样提示用户的，设计、产品、老板就只顾着说，哈哈。 最后得出网上所谓的方法也是鸡肋办法，不实用。



