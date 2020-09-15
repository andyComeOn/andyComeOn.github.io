---
title: js日期设置总结
date: 2019-09-17 16:55:00
tags: JS相关
---

js日期相关处理函数，时间久了就会忘记怎么使用，每次都是临时在网上搜，写个笔记记录一下：

---

### 1、 日期的基本处理。

一般针对前端而言，后端（PHP、Java）传给你的时间字段，常见的有2种：如1：` 2018-09-09 00:00:00 `，如2：` 1568711273205 `。 

情况1是他们处理过，情况2是直接返回的时间戳。情况1中已处理过，可直接用js处理字符串的方法处理成自己所需要的格式。

情况2直接 ` new Date(timestamp) ` 打印之是：` Tue Sep 17 2019 17:07:53 GMT+0800 (中国标准时间) `。时间戳必须是数字否则提示：` Invalid Date `。若是想使用其中的年月日信息则可：` _data = new Date(timestamp) ` ，切记月份记得要加 1 ， 若需要月、日都是2位 ，写一个过滤函数处理之即可。过滤器函数如下


~~~javascript
// 月、日是一位数字的补齐2位，若函数名重复可shift用_padding
function _fill(arg) {
    // 使程序更严谨
    if (arg == undefined || arg == '') {
        return '';
    }
    var re = arg + '';  // 转为字符串为了使用字符串 length 属性
    if (re.length < 2) {
        re = '0' + re;
    }
    return re;
}
~~~

补记：其实 Date() 有一些封装好的方法，如：

~~~javascript
const timeString = new Date(this.data.timestamp).toLocaleString() // 代码块且命名好
new Date().toLocaleString() // "2020/9/15 下午3:42:04"
~~~
展示遵循简单，2020/9/15，切莫追求2020/09/15。

### 2、 JS中date日期初始化的几种方法

在标题1中讲述到一般后端返回的是 ` timestamp `、` 2018-09-09 00:00:00 `2种，针对这两种初始化可使用如下5种方法：

~~~Javascript
var objDate = new Date([arguments list]);
~~~

1) ` new Date("month dd,yyyy hh:mm:ss"); `
2) ` new Date("month dd,yyyy"); `
以下情况必须是数字型参数！
3) ` new Date(yyyy, mth, dd, hh, mm, ss); `
4) ` new Date(yyyy, mth, dd); `
5) ` new Date(ms); `

代码如下：
1) ` new Date("January 12,2006 22:19:35"); `
2) ` new Date("January 12,2006"); `
3) ` new Date(2006,0,12,22,19,35); `
4) ` new Date(2006,0,12); `
5) ` new Date(1137075575000); `

上面的5种例子是网上说的，个人感觉不是很实用，一般后台传过来的格式是 ` 2018-09-09 00:00:00 ` ，可以直接 ` new Date(2018-09-09 00:00:00); ` 也可以不加 hh:mm:ss。

但是我在其他帖子上看到 ` Date.parse() ` 这种解析方式，如下：

~~~javascript
console.log('2019/09/17', Date.parse('2019/09/17'), new Date(Date.parse('2019/09/17'))); 
// 2019/09/17 1568649600000 Tue Sep 17 2019 00:00:00 GMT+0800 (中国标准时间)
console.log('2019-09-17', Date.parse('2019-09-17'), new Date(Date.parse('2019-09-17')));
// 2019-09-17 1568678400000 Tue Sep 17 2019 08:00:00 GMT+0800 (中国标准时间)
console.log('2019- 9-17', Date.parse('2019-9-17'), new Date(Date.parse('2019-9-17')));
// 2019- 9-17 1568649600000 Tue Sep 17 2019 00:00:00 GMT+0800 (中国标准时间)

// 上面的三种写法，Date.parse 获得的时间戳不一样， 说明了时间格式的最简写、加默认的0 解析的该天的 hh:mm:ss 不一样。如果加上hh:mm:ss 三种写法会保持一致。 也可观察出 '2019-9-17' '2019/09/17' 这2中写法获取到的是该天凌晨的时间戳。
~~~

！！！既然` new Date() ` 也可以解析数字字符串，那就没必要使用` Date.parse() `。何必要多记忆这个方法呢？

### 3、 日期，在原有日期基础上，增加（减少）days天数，默认增加1天，若是减一天days传负数即可。

~~~javascript
function addDate(date, days) {
    if (days == undefined || days == '') {
        days = 1;
    }
    var _date = new Date(date);
    // 这时候 _data 可根据自己需求处理 
    _date.setDate(_date.getDate() + days);
}
~~~

上面的可以拓展加（减）年、加（间）月

~~~javascript
function addYear(date, years) {
    if (years == undefined || years == '') {
        years = 1;
    }
    var _date = new Date(date);
    // 这时候 _data 可根据自己需求处理 
    _date.setYear(_date.getFullYear() + years);
}

function addMonth(date, months) {
    if (months == undefined || months == '') {
        months = 1;
    }
    var _date = new Date(date);
    // 这时候 _data 可根据自己需求处理 
    _date.setMonth(_date.getMonth() + months);
}
~~~

补记一个小细节，看下例：

~~~JavaScript
var d = new Date();
var dd = d.setDate(d.getDate() + 1);
console.log(dd); // 这时候 dd 是一个时间戳
~~~

若这样写：
~~~JavaScript
var d = new Date();
d.setDate(d.getDate() + 1);
var dd = d;
console.log(dd); // 这时候 dd 是一个 Date 对象，打印结果是中国标准时间
~~~

### 4、 日期之间相差多少天。
很多的帖子就是直接把startTime endTime 直接转换为时间戳相减之后在取整（或者向下取整）在除以86400000，就是相差的天数。


~~~javascript
// 
function _subtraction (start, end) {
    var a = new Date(start);
    var b = new Date(end);
    return Math.floor((b - a) / 86400000);
}

~~~

### 5、 有初末日期，获取其中之间的日历（场景：酒店系统 餐费的时间日历选择）
~~~javascript
function selectMealDate (s, e) {
    // 月、天不够2位补0
    this._fill = function (arg) {
        // 使程序更严谨
        if (arg == undefined || arg == '') {
            return '';
        }
        var re = arg + '';  // 转为字符串为了使用字符串 length 属性
        if (re.length < 2) {
            re = '0' + re;
        }
        return re;
    }
    // 初末日期相差多少天
    this._subtraction = function (startArg, endArg) {
        var a = new Date(startArg);
        var b = new Date(endArg);
        return Math.floor((b - a) / 86400000);
    }
    // 抛出的值
    var _arr = [];
    // 循环出餐费日历数组
    for (var i = 1; i <= this._subtraction(s, e); i++) {
        var temp = new Date(s);
        temp.setDate(temp.getDate() + i ); 
        _arr.push(temp.getFullYear() + '-'+ this._fill(temp.getMonth() + 1) + '-' + this._fill(temp.getDate()));
    }
    return _arr;
};
~~~


### 6、 参考的链接，未完，相差小时，分钟，秒 继续看参考的帖子

[js 日期相差计算](https://www.cnblogs.com/summer-qd/p/10103984.html)
[JS日历循环，指定开始结束日期，自动向后计算](http://www.miaoqiyuan.cn/p/date-range)
[JavaScript Date（日期） 对象](http://www.runoob.com/js/js-obj-date.html)     


### 7、 日期相关补记

1. 一般服务器的时间是很少出错，我们在后台存时间戳其实是不准确的，应为时间戳若是时区不一样，解析的会有误差！可以看这篇帖子 [MySQL date、datetime和timestamp类型的区别](https://zhuanlan.zhihu.com/p/23663741)，在看了帖子：[时区是怎么划分的？世界各时区的时间如何统一表达？GMT、UTC、UNIX有什么区别？](https://learn.blog.csdn.net/article/details/102728563)之后发现一个完善web系统，可以存datetime(它是绝对值是最准确的！)

2. > 用户的浏览器从服务器获取HTML文档，开始翻译解析，解析 “new Date()” 时就是用户系统的时间，也就是你说的用户系统的时间；一般情况下用户操作系统的时间是准确的，但也有不准确的情况，也可以在服务器获取时间，这就有点麻烦了，应该有相应的API吧，没试过。  

上面一段说的js是获取本机的系统使劲，我想若不是一个时区，或者用户自己更改自己电脑时间，也会倒置时间不一致！

3. 现在服务器一般是linux 都是使用的是 UTC （世界时间协调），格式： 2019-11-11T00:00:00.000Z 我想数据库也可以这样存，返回给前端之后，前端可使用 [js处理时间时区问题](https://www.cnblogs.com/goloving/p/10514914.html) 中的方法处理！其中的举例很好，公式：其他时区时间 + 其他时区时差 = 本地时间 + 本地时差

4. 在网上搜到一个这样的帖子 [苹果手机浏览器时区与系统时区不一致](https://zhidao.baidu.com/question/1964112958349855060.html)  其中的回答综合我理解是没有同步时间！

5. 2019-11-11T00:00:00.000Z T 代表是世界标准时间  Z是指偏移量，没有写数值就是指偏移为 0 ，也就是 0 时区。北京在东8区、比GMT要早8小时，写作2019-11-11T08:00:00.000+0800  参考上面帖子，“时区是...”

6. 各大语言处理时间函数 [UTC时间、GMT时间、本地时间、Unix时间戳](https://www.cnblogs.com/xwdreamer/p/8761825.html)

### 8、记录一次 iOS 手机上日期使用bug

1. 在微平台的 C 端由于房券使用的逻辑选房日期赋值了这种格式 2020-05-01 06:00:01，进行了 var t = new Date('2020-05-01 06:00:01') 的操作，可是在取 t.getFullYear() 时候一直提示 NaN，搜资料发现 ios 手机上 new Date(param)，该 param 要么是时间戳，要么是这种格式的字符串 '2019/09/17 06:01:00' 才能被 ios 手机解析出来！

2. 详见 [解决js newDate()苹果手机日期格式显示NaN](https://blog.csdn.net/chelen_jak/article/details/98215109)