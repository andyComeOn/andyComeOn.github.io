---
title: JS判断对象是否为空
date: 2020-08-31 16:31:41
tags: JS相关
---

## JS 判断对象是否为空

1. 在开发会客厅预定逻辑，渲染门店列表时候后台返回的是一个 `对象`，之前是 `数组`，后台说自己不好处理，那我就对数据进行二次转换把。首先想到如何判断一个变量是`对象`还是`数组`呢？于是就使用最稳妥的办法：`const string = Object.prototype.toString.call(var)`，在对此结果进行 `string.indexOf('Object')`、`string.indexOf('Array')`、`string.includes('Object')`、`string.includes('Array')`进行判断！

2. 此时我又想到判断数组是否为空可以使用 `array.length > 0`，那么怎么判断判断一个对象是否为空呢？

3. 方法如下：

- ```
    for (var i in obj) {
      return true  // 如果不为空，则会执行到这一步，返回true
    }
    return false // 如果为空,返回 false
  ```

- 使用 es6 的方法 `Object.keys(var)` 该方法返回一个数组，若改数组的长度为 0 这说明为空对象！

- 使用 JSON.stringify()，将 JavaScript 值转换为 JSON 字符串

  ```
  if (JSON.stringify(data) === '{}') {
  　　return false // 如果为空,返回false
  }

  return true // 如果不为空，则会执行到这一步，返回true
  ```
