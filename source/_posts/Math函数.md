---
title: Math函数
date: 2019-06-11 17:25:00
tags:
---

### 总结一下Math相关函数

1. 我们在写涉及到钱业务逻辑前端页面时，会有保留2位小数的场景，后台一般返回钱数相关字段的值是字符串，如：` 3.00 `，但是有时候页面需要计算钱数，如 3.00 + 5.00 = > 8，这时候8转为8.00比较符合场景，这时候可能会使用到toFixed(2)。

toFixed用法 ： 

~~~javascript
NumberObject.toFixed(num);
~~~
上面意思是针对Number型，才能使用 ` toFixed() `, 切记这个也是满足四舍五入的数学规则。

2. Math.ceil(num))，是只是向上取整数，但是若是3.00之类的数还是3 如下： 
~~~javascript
Math.ceil(3.12))   // 4
Math.ceil(3.00))   // 3
~~~

同理Math.floor()也是如此。

3. toString() 也是针对Number。但是它好像是有2进制的作用
