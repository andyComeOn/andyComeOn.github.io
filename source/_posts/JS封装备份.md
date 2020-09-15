---
title: JS封装备份
date: 2019-02-22 14:40:54
tags: JS相关
---

### 1、针对一个浮点型数，保留几位小数（符合四舍五入）的方法

**方法：**
~~~javascript
fixto(number, n = 2) {
    let result = number.toString();
    const arr = result.split('.');
    const integer = arr[0];
    const decimal = arr[1];
    result = integer + '.' + decimal.substr(0, n);
    const last = decimal.substr(n, 1);

    // 四舍五入，转换为整数再处理，避免浮点数精度的损失
    if (parseInt(last, 10) >= 5) {
        const x = Math.pow(10, n);
        result = ((parseFloat(result) * x) + 1) / x;
        result = result.toFixed(n);
    }

    return result;
},
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







