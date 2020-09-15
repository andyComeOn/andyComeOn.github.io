---
title: CSS字体换行总结
date: 2019-12-10 16:19:45
tags: CSS相关
---

在一些 UI 框架如 layui 的 col中有默认的换行样式，主要是取决于 CSS 的三个属性 `white-space` `、word-break` `、word-wrap(overflow-wrap)` 下面分别记录一下。

> HTML 中white-space问题总结

首先看下面的 html 代码：

~~~html
<div id="box">
  Hi&nbsp;&nbsp;,
  This     is a incomprehensibilities long word.
  </br>
  你好&nbsp;&nbsp;，
  这   是一个不可思议的长单词
</div>
~~~

我们要清楚，我们的浏览器都有一些默认的 CSS 样式，若我们对某个标签写了 CSS 样式，刚好覆盖着浏览器的默认样式，则浏览器会按照我们写的 CSS 进行渲染。上面代码我们只对box容器加宽加边框。其渲染效果：遇到 `<br>` 会换行，遇到 `&nbsp;` 会保留一个空格，注 代码中我们故意在文字之间多敲上几个空格，但渲染效果还是按照一个空格渲染！如下图：

![white-space默认值效果](http://file.798run.top/img/blog/20191210/ic_0.jpg)

接下来我们看下， 给它上面三个css属性赋值后会出现什么变化。

`white-space` 按照其字面意思来理解，即空白之意、控制空白字符的显示的，同时还能控制是否自动换行。它有五个值：normal | nowrap | pre | pre-wrap | pre-line。因为默认是 normal，所以我们主要研究下其它四种值时的展现情况。

1. nowrap 意思不换行。加上 `white-space: nowrap;` 就是告诉浏览器不要给我换行。效果如下：

![white-space: nowrap; 效果](http://file.798run.top/img/blog/20191210/ic_item1_nowrap.jpg)

原文本的自动换行没了！只有</br>才换行！所以这个属性值我们可以理解为永不换行，即使超出容器宽度也会渲染。

---

2. `pre` 是预定义之意，是 predefined 的简写，指预定义。作用：保留空格、换行等。也就可以了理解是你写的是什么结构的 html 代码他就给你展示啥样。加上 `white-space: pre;` 浏览器会按照原样的代码结构给你展示，也就是说你的代码的每一个小方块，给该小方块上下左右都给你加留白（就是原样展示）。应用场景：后台工作人员查看他们返回前端的数据，在输出 echo 打印时加上一对 `<pre></pre>` 标签，结果很清晰！效果如下：

![white-space: pre; 效果](http://file.798run.top/img/blog/20191210/ic_item1_pre.jpg)

---

3. `pre-wrap` 可理解为在pre的基础上加上换行：

![white-space: pre-wrap; 效果](http://file.798run.top/img/blog/20191210/ic_item1_pre_wrap.jpg)

4. `pre-line` 可理解为在pre的基础上左右的留白给你去掉了，当然也换行，应为我们没有写换行的属性值，浏览器默认排版都是换行的：

![white-space: pre-line; 效果](http://file.798run.top/img/blog/20191210/ic_item1_pre_line.jpg)

---

> HTML 中 word-break 问题总结

从这个名字可以知道，这个属性是控制单词如何被拆分换行的。它有三个值：normal | break-all | keep-all。

`word-break: keep-all;` 效果如下：

![word-break: keep-all 效果](http://file.798run.top/img/blog/20191210/ic_item2_1.jpg)

1. 解释：你写的 &nbsp; 继续给你渲染，但是它会根据你在文本中按下的空格（1个或多个空格都按一个空格处理）把你的文本拆分不同文字段给你渲染，你的文字段若是有点大，会给你继续渲染即使超过了容器宽度也不换行。

`word-break: break-all;` 效果如下：

![word-break: break-all 效果](http://file.798run.top/img/blog/20191210/ic_item2_2.jpg)

2. 解释：你写的 &nbsp; 继续给你渲染，其他的不管是英文单词还是中文、日韩文字都是直接给你截断。空格直接不渲染。这种方式有点粗暴，若是英文单词直接截断换行渲染阅读效果很差！但是我想在一个小容器中要是标签内容都是长数字、长中文，不影响阅读，使用这个属性也没啥不可以！


> HTML 中 word-wrap(overflow-wrap) 问题总结

word-wrap又叫做overflow-wrap：

word-wrap 属性原本属于微软的一个私有属性，在 CSS3 现在的文本规范草案中已经被重名为 overflow-wrap 。 word-wrap 现在被当作 overflow-wrap 的 “别名”。 稳定的谷歌 Chrome 和 Opera 浏览器版本支持这种新语法。

这个属性也是控制单词如何被拆分换行的，实际上是作为word-break的互补，它只有两个值：normal | break-word，那我们看下break-word：

`word-wrap: break-word; overflow-wrap: break-word;` 效果如下：

![overflow-wrap: break-word 效果](http://file.798run.top/img/blog/20191210/ic_item3_1.jpg)

解释：这种场景是为了解决，break-all 属性值不管啥都是直接截断，针对一个长单词如 incomprehensibilities，如 容器宽度小于这个单词宽度，直接在incom下一个字母p截断了，当前行最后显示incom 下一行显示 prehensibilities，这在英文场合下，很不友好，若该长单词新起一行，在新的一行中也显示不了，在换行，这种情况效果比上面“硬换行”效果好一些，我理解，W3C设置该属性就解决此类问题！


补充：（其实前面的word-break属性除了列出的那三个值外，也有个break-word值，效果跟这里的word-wrap: break-word 一样，然而只有Chrome、Safari等部分浏览器支持）

具体的 html 测试文件在testme目录下的 white_space.html

[参考链接](https://www.cnblogs.com/dfyg-xiaoxiao/p/9640422.html)
