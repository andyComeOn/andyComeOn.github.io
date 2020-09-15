---
title: this-bind-call-apply
date: 2020-06-27 21:51:48
tags: JS相关
---

> ### 针对 this 指向问题，时间久了就会忘记！现总结以下文档温故而知心！

> ### 透彻认识 function 的 this 在不同调用环境下的指向

1. #### 事件调用环境

   - ```
     <div class="box"></div>
     let el = document.querySelector('.box')
     function move() {
        console.log(this)
        this.style.left = '100px'
     }
     el.onclick = move
     // 这是最早的 this 应用！总结：谁触发事件，函数里面的 this 就指向谁！试想在多个 box 出现的场景，我们只需要把 move 赋给每个 box 的点击事件，这样我们就精简了很多代码，而不是给每个 box 的点击事件都写一个 move 函数！
     ```

2. #### 全局环境

   - 在浏览器环境下：直接打印 this，其 this 指向的是 window。如，我们直接在全局声明一个函数如下：

   ```
   function fn () {
      console.log(this)
   }

   fn() // 此时我们这样调取函数，非严格模式是这样的 window.fn()。所以函里面的 this 依然是满足原则：“谁调用它，它指向谁”。但是在代码最开始若是加上了 use strict 此时打印的 this 显示 undefined！这个小细节要注意！
   ```

   - 在 node 环境下：直接打印 this，打印结果 {} 。具体测试步骤，新建一个 test.js 直接执行 `node test.js`。 若我们打印 console.log(this === module.exports) 其结果是 true。 这说明：node 环境下 this 指向的是 module.exports

3. #### 函数内部

   - 情况一：[this 最终指向的是调用他的对象]
     请看下面代码：

   ```
   var obj = {
      a: 10,
      b: function() {
         console.log(this)
      }
   }
   obj.b() // 我们调用对象中 b 属性上的函数，此时打印出的 this 是：{a: 10, b: f}，这不就是 obj 吗？所以正好验证了上面的那句话：“this 最终指向调用他的对象”

   ```

   此时有人针对上面的代码并举例说，我这样调用 `window.obj.b()`，此刻 this 不是应该指向 window 吗？但是打印的结果依然是 {a: 10, b: f}。所以此时有了第二条总结如下：

   - 情况二：[函数被多层对象所包含，如果函数被做外层对象调用，this 指向的依然是它上一级的对象]
     - 多层对象中函数的 this 指向
     - 对象中的函数被赋值给另外一个变量

   针对情况二，我们把代码层级在加深：

   ```
   var obj = {
      a: 10,
      b: {
         fn: function() {
            console.log(this)
         }
      }
   }

   window.obj.b.fn() // fn 被调用，他的上级就是 b，所以此时 this 打印结果：{fn: f}
   ```

   - 有变量接收 fn 时，`var a = obj.b.fn` 此时我们调用 a()。此时大家看着有点懵，我们还是按照总结结论来分析（一切都要按照章程来）。a() 其实就是 window.a()。也就是说 window 调用 a 函数，则 this 指的就是 Window！在浏览器控制台一看，就是 Window！（补充：this 本身就有啥意义，只有 this 所在的环境被调用时候，this 才有意义！）

4. #### 构造函数应用

   - 构造函数

   ```
   function fn() {
      this.num = 10
      console.log(this)
   }
   var obj = new fn()
   ```

   执行上面代码后，打印的 this 是：fn { num: 10 }，这是一个对象，对象的名字是 fn。上面使用了 new 关键字，下面总结一下 new 关键字作用：

   - new 关键字作用

     - 调用函数
     - 自动创建一个对象 {隐式创建我们看不到}
     - 把创建出来的对象与 this 进行绑定
     - 如果构造函数没有返回值，隐式返回 this 对象

   - 面试题

   ```
   function fn() {
      this.num = 10
   }

   fn.num = 20
   fn.prototype.num = 30
   fn.prototype.method = function () {
      console.log(this)
   }

   var prototype = fn.prototype
   var method = prototype.method

   new fn().method() // fn {num: 10}
   prototype.method() // protoptype {num: 30, method: f, constructor: f}
   method() // Window
   ```

   - 结论：构造函数中的 this 指向的是实例对象，如果构造函数中有 return value，value 若是基本类型，则返回的实例还是隐式的对象（null 也属于此情况）；若是引用类型，则返回的就是该对象。举例：

   ```
   function fn() {
      this.num = 10
      return ''
   }
   var obj = new fn()
   console.log(obj.num) // 10

   // 若是上面的 return [] || {} || function() {}，此时返回的就是对象，打印 obj.num 就是 undefined。
   ```

5. #### 箭头函数

   - es6 中定时器涉及到 this 的例子：

   ```
   function move() {
      setTimeout(() => {
         this.style.left = '100px'
      }, 500)
   }

   let el = document.querySelector('#block')

   el.onclick = move
   ```

   箭头函数：本身没有 this 和 arguments，在 箭头函数内部使用 this 实际上是根据词法作用域，代码写好之后，其函数里面的 this 指向就已固定，就是其上一层函数作用域的 this，此例子中 this 指向 move 函数作用域中的 this。

   - es5 写法

   ```
   function move() {
      let _this = this
      setTimeout(function () {
         _this.style.left = '100px'
      }, 500)
   }

   let el = document.querySelector('#block')

   el.onclick = move
   ```

   在之前 es5 中，调用定时器中的函数，此时该函数被 window 调用！所以定时器函数体内 this.style.left = '100px' 该 this 指向的是 window（其并没有 style 属性，故而报错！）。若想要实现 this.style.left = '100px' 的执行，需要我们修改 this 指向。则需要在 move 函数里面添加 let \_this = this，进而调用 \_this.style.left = '100px'

   - 对象中属性值是箭头函数

   ```
   var obj = {
      a: 1,
      fn: () => {
         console.log(this)
      },
   }

   obj.fn()
   ```

   // 此时 this 指向的是 Window 解释：在该例子中，箭头函数作为 obj 的一个属性值，根据箭头函数 this 指向的是其上一层函数作用域中的 this ，但是 obj 是一个对象，并不是函数！所以继续向上找，则 this 指向的就是 Window！注：对象是不能形成独立作用域的！

6. #### call 含义
   - call 可以改变 this 的指向，看下面例子：
   ```
   var box = document.querySelector('.box')

   var obj = {
      fn: function () {
         console.log(this)
      },
   }
   obj.fn.call(box) // this 指 box 对象 
   
   ```
   正常情况下，obj.fn() 执行时，this 指向的是 obj，我们使用 call 方法可改变 调用 fn 时的 this 指向！  

   - 把 fn 改为箭头函数：
   ```
   var box = document.querySelector('.box')

   var obj = {
      fn: () => {
         console.log(this)
      },
   }
   obj.fn.call(box) // this 指向的是 Window
   
   ```
   虽然在使用 call 方法，可是写的时候把 fn 声明成了箭头函数，此时 this 已经固定，再加上 obj 不是函数作用域，所以 this 指向的是 Window。结论：call 改变 this 的指向，特指普通的函数而非箭头函数！

   - call 与 apply 的作用是一样的，他们的区别是传参的方式不同，call 是可以一个一个的传参，而 apply 参数要求是一个数组 `apply(this, [a, b, c])`

   - bind 亦是可改变 this 的指向，只是它特殊在 bind 返回的是一个函数的 copy 如：`obj.fn.bind(box)` （仅是一个 copy 的函数），我们还调用对其调用 `obj.fn.bind(box)()` 

   - 没事了看下此文章 [JS中的bind 、call 、apply](https://www.cnblogs.com/liu-di/p/10690335.html)







