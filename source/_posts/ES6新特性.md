---
title: ES6新特性
date: 2020-07-13 09:34:02
tags: JS相关
---

## 1、for of 循环

我们最开始只有 for 循环，for 循环它只适用数组、类数组。如果我们想要循环一个对象，则 for 不能满足需求，我们可用 for in 代替之！for in 亦可循环数组！如下：

```
const digits = [1, 2, 3];

for (const index in digits) {
  console.log(index, '--', digits[index]);
}
```

这样一看，for in 挺好啊，对象与数组通吃！但，有一种场景 for in 就会出现问题。场景：开发过程中，你需要给数组中添加额外方法，如下：

```
Array.prototype.decimalfy = function() {
  for (let i = 0; i < this.length; i++) {
    this[i] = this[i].toFixed(2);
  }
}
```

此时循环上面的数组效果如下：

![for in 会循环出原型链上的属性](http://file.798run.top/img/blog/20200713/forin_proto.png)

forin 直接把原型链上的方法也给循环了。为啥会出现此结果？因为 forin 循环访问所有可枚举的属性！既然 forin 在某些场景下出现我们不期望的结果！es6 就推出一个新的循环方法 for of！用于循环访问任何可迭代的数据类型，其编写方式和 for...in 循环的基本一样，只是将 in 替换为 of，可以忽略索引。于是我就写下了下面的代码：

```
const digits = [11, 12, 13];

for (const index of digits) {
  console.log(index, '--', digits[index]);
}

```

效果如下：

![for of 循环效果](http://file.798run.top/img/blog/20200713/for_of.png)

这一看好怪！ 怎么有 undefined，index 怎么分别是 11, 12, 13。于是我想是不是我写的语法不对？我就直接在 MDN 上查，人家举例：

```
const array1 = ['a', 'b', 'c'];

for (const element of array1) {
  console.log(element);
}

// expected output: "a"
// expected output: "b"
// expected output: "c"
```

我发现自己理解错了，MDN 上 for of 的例子，压根就没有啥 index，人家直接使用的就是 element，恍然大悟明白，for of 就是直接给你把数组中的成员取出来了！所以我们也要按照 MDN 上写法使用该方法！

此时脑袋中闪出一个想法，for of 既然这么强大，我能不能循环对象呢？于是直接写出下面代码：

```
var object = {
  name: 'Tom',
  age: 19
};

for (const iterator of object) {
  console.log(iterator)
}

```

遗憾的是出错了！错误：`Uncaught TypeError: object is not iterable` object 不是迭代类型。所以我们不能使用之循环 object！具体可细看 [MDN for...of](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/for...of)

一直看 MDN 上看到可枚举的属性，这个详细定义在哪？闲了细究！

## 2、import、export

- **export** 写 vue 时候，大家都习惯了 vue 的默认模板，在 script 里面我们直接 `export default {}`，还可以`export default function fn() { return new Date().getTime() }`。在项目中配置 api 时习惯这样写：`export const exchangeApi = '/api/Toutiao/conversion`，我们还可以导出常量如：`export const PI = 3.14`、`export let APP_NAME = "qiuguo"`。若我们在一个 js 文件中写最开始声明一个 let host = ''，最后对其二次赋值，host = 'https://www.baidu.com'，我们可以这样导出 `export { host }`。

- **import** 引入的逻辑简单，有一个场景，在一个文件中有 export default、有非 default 形式。我们可以这样简写：`import fn, { name, PI, host } from "../components/demo.js";`
