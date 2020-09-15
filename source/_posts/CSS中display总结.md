---
title: CSS中display总结
date: 2020-07-10 23:27:23
tags: CSS相关
---

> ### CSS 中 display 有很多的属性值。记录一下！

#### display 在 css 中包含很多属性。我们合理使用之能带来很好的效果。

- 1. display 的最常用的 none、block 就不在记录了。

- 2. display 在开发过程中，看到在写 gddyg crs 欢迎页时候使用 `display: flex` 时候，发现了 display: -webkit-box，很是疑问（缘由是项目中有 normalize.css 自动给加上 -webkit-box 了）于是查资料发下：

  - flex 是 2012 年的语法，也将是以后标准的语法，大部分浏览器已经实现了无前缀版本。

  - box 是 2009 年的语法，已经过时，是需要加上对应前缀的。

  - 所以兼容性的代码，大致如下：

  display: -webkit-box; /_ Chrome 4+, Safari 3.1, iOS Safari 3.2+ _/
  display: -moz-box; /_ Firefox 17- _/
  display: -webkit-flex; /_ Chrome 21+, Safari 6.1+, iOS Safari 7+, Opera 15/16 _/
  display: -moz-flex; /_ Firefox 18+ _/
  display: -ms-flexbox; /_ IE 10 _/
  display: flex; /_ Chrome 29+, Firefox 22+, IE 11+, Opera 12.1/17/18, Android 4.4+ _/

  - 都是当时各阶段草案命名。

  - 既然后出现的 flex 浏览器已经支持，个人感觉就没有必要在使用 box 了，再说 normalize.css 已经帮我们做了！

  - 参考博文 [display：flex 和 display：box 有什么区别](https://www.cnblogs.com/websmile/p/11607884.html)！

- 3. flex 一些用法

  - **父元素的属性**

  - 妈的很容易忘记。不理会特殊复杂的情况。flex-direction 就是你的 item 排列方向。默认是水平排列，就别贱贱的写代码。若想主轴是垂直排列：写上 `flex-direction: column`

  - justify-content 属性：定义项目在主轴上的对齐方式。切记是主轴！`justify-content: flex-start | flex-end | center | left | right | space-between（两端对齐，项目之间间隔相等，C 端订房入住、离店、几晚可使用此属性值） | space-around（每个项目两侧的间隔相等，即使是first、last与父元素都有间隔） |` 这个还有很多，没写完但是不常用！

  - align-items 属性：定义在交叉轴上的对齐方式，这个有一个单词是 items，可能认为是不止一个 item 所以加 items。`align-items: flex-start | flex-end | center | baseline | stretch;`

  - align-content 属性：定义多根轴线的对齐方式（如果项目只有一根轴线，该属性不起作用。所以容器必须设置 flex-wrap，flex-wrap 是写在父元素上的，一般也是不常用！`flex-wrap: nowrap | wrap | wrap-reverse;`）

  - flex 中的子元素还有一个 order 写法，就是直接可以使用 order: 1。css 把子元素的位置按照你写的 order 顺序排列！个个感觉也没多大的用处，妈的不会把 子元素 顺序写好吗？非得要使用 order 吗？

  - **子元素的属性**

  - 设置在项目上的属性也有 6 个。
    order
    flex-grow
    flex-shrink
    flex-basis
    flex
    align-sel

- 4. 个人总结

  - 其实很多场景下我们就是简单的运用。例如我们在一个容易，想快速把里面一个图片垂直水平居中，直接就写了管它啥轴不轴的，直接：`justify-content: center; align-items: center;` 就完事了。

- 5. 参考了解 [前端布局神器display:flex] (https://www.cnblogs.com/qingchunshiguang/p/8011103.html)  
