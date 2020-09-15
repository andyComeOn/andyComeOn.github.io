---
title: jq中一些总结
date: 2019-02-22 14:40:54
tags:
---

### 1、标签下的data-**总结

**写法一：**
~~~html
<div class="el1" data-isdata = "1">标签1</div>
<div class="el2" data-isData = "2">标签2</div>
<div class="el3" data-is_Data = "3">标签3</div>

~~~
~~~javascript
console.log($('.el1').data('isdata'));  // 1
console.log($('.el2').data('isData'));  // undefined
console.log($('.el3').data('isdata'));  // undefined

// 正确的用法
console.log($('.el2').data('isdata')); // 2
console.log($('.el3').data('is_data')); // 3

// 总结就是遇见大写的就转为小写即可
~~~

说明：保留几位小数点，参数n设置为几！

---

### 2、简单的分页逻辑

**方法：**

~~~javascript
function page(total, pagesize = 10){
    var tmp = (total / pagesize).toString();
    if (tmp.indexOf('.') == -1) {
        return tmp - 0;
    } else {
        return Math.ceil(tmp);
    }
}

console.log(page(21, 22));
~~~
说明：第一个参数是数据的总数量，第二个参数是每页数量，该函数抛出的是总共有几页！

---

### 3、数字字符串转数字
**方法：**
~~~javascript
var a = '1';
console.log(a - 0);
console.log(Number(a));
~~~

---

### 4、函数传参
**方法：**
~~~javascript
function todo(a, b = 2) {
    return a + '|' + b;
}
console.log(todo(1, 3)); // '1|3'
console.log(todo(1)); // '1|2'
~~~

---







