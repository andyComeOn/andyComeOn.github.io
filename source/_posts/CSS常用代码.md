---
title: CSS常用代码
date: 2019-12-11 16:13:27
tags: CSS相关
---


#### ▲ maring padding 设置为 0
~~~css
* {
    margin: 0;
    padding: 0;
}
~~~


#### ▲ 弹性盒 
~~~css
.boxE {
    box-sizing: border-box;
}
~~~


#### ▲ body bg 颜色2行文字并添加省略号
~~~css
body {
    background: #ccc;
}
~~~

#### ▲ li 重置默认样式
~~~css
.reLi {
    list-style: none;
}
~~~

#### ▲ 宽度 100% 常对 input 使用 
~~~css
.wP {
    width: 100%;
}
~~~

#### ▲ 2行文字并添加省略号
~~~css
.elli2 {
    overflow: hidden;
    text-overflow: ellipsis;
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
}
~~~


#### ▲ bg 复合属性分开写
~~~css
.bg {
    background-repeat: no-repeat;
    background-size: 100%;
    background-position: center;
    background-image: url('');
}
~~~