---
title: Vuex总结
date: 2019-02-26 15:35:27
tags:
---

#### 这是对Vuex的一次小总结

##### vuex是什么？

1. vuex它是解决：针对vue中的组件，当应用遇到多个组件共享状态时，单向数据流的简洁性很容易被破坏：

    * 多个视图依赖于同一状态。
    * 来自不同视图的行为需要变更同一状态 。
总结起来就是：有些场景如兄弟组件，通信很麻烦；还有就是多层嵌套Son.vue > Father.vue > Grandfa.vue孙子给爷爷通信也很困难。有困难，为了解决困难，Vuex应运而生！

2. 每次看每次忘！到底怎么梳理

    * 首先记住就三个大的状态管理部门：`action`，`state`，`view`最开始我们定义的state，就是显示在页面上(view)的状态，在页面(view)上用户操作之后，就是我们常说的action。
    * 详解：Father.vue中引入了Son.vue，在Father.vue页面对Son.vue组件进行操作，我们引入了dispatch的概念，即：`$store.diapatch('switch_dialog')`，代码的意思是分发`switch_dialog`方法，该方法就是store中action的方法。
    * store中action下的switch_dialog方法，又进行了`context.commit('switch_dialog')`，这个context.commit中的switch_dialog，就是mutation中的switch_dialog，在mutation的switch_dialog终于进行了代码的逻辑操作，如：`state.show = true`，最开始show的状态是false。
    * 这就是store的过程，下面把代码截图展示一下。

3. 截图如下：

Father.vue中有2个容器（加了click事件）代码：

![father.vue](http://file.798run.top/img/blog/20190226/father.vue.jpg)

其中的2个组件分别是`dialog.vue`、`dialogTab.vue`，在这两个组件中代码截图（只截图dialog.vue）如下：

![son-dialog.vue](http://file.798run.top/img/blog/20190226/son-dialog.jpg)

最后的store中的`dialog.js`、`dialogTab.js`的代码截图如下：

![store-dialog.jpg](http://file.798run.top/img/blog/20190226/store-dialog.jpg)

![store-dialog-tab.jpg](http://file.798run.top/img/blog/20190226/store-dialog-tab.jpg)



