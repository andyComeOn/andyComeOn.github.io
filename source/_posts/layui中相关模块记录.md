---
title: layui中相关模块记录
date: 2019-02-13 10:47:10
tags: layui相关
---

写在前面：此文档是在使用 `layui` 过程的一些总结！

### 1. layui的模块化和非模块化使用

下面这个帖子[layui的模块化和非模块化使用](https://www.cnblogs.com/qlqwjy/p/8975931.html)，着重介绍了layui中模块与非模块化的区别，这个帖子讲的很详细，解决了我的下面的疑问：
![layui-on-err](http://file.798run.top/img/blog/20190213/layui-on-err.jpg)

我发现这个错误是由于自己是使用非模块化模式，但是又引入了模块化的js，导致了这个 err。但是有一个疑问，按理说模块化与非模块化的2种模式只是用户的使用方式，但是我发现使用非模块化（只引入layui.all.js）在页面中即使这样写了`required lay-verify=""`出现不了下面这个效果：

![verify-tips](http://file.798run.top/img/blog/20190213/verify-tips.jpg)

这感觉不合理！

#### 2. 模块加载问题

layui只use layer 打印form table 是undefined
layui只use table 打印form table 是OK的
layui只use form 打印form table 仅form是OK的

得出结论，只要use使用的有模块，layer就能使用（全局的），只use table，form也能使用；只ues form，table不能使用；table逻辑嵌入了form逻辑。


#### 3. form的验证问题 
`<div class="layui-form-item" style="display: none;">` 
即使对这种隐藏的元素，`layui`也进行了验证，这种情况会让用户看着很懵，这是我在pms的底部“办理入住”看代码逻辑：

![代码逻辑](http://file.798run.top/img/blog/20190124/layui-form.jpg)
验证问题在修改862bug时效果如下：
![verify-tips](http://file.798run.top/img/blog/20190213/verify-tips.jpg) 这个提示是在验证时候加了`required lay-verify=""`属性，出现此效果还要额外引入`layui.js`。若这样写`required lay-verify="required"`只会提示下面的效果：![required-tips](http://file.798run.top/img/blog/20190213/required-tips.jpg) 
针对verify官方上说是可以串联写：`lay-verify="required|verifyAddtime"`注：这里面别出现空格！

#### 4. filter相关

filter，为 class="layui-form" 所在元素的 lay-filter="" 的值。你可以借助该参数，对表单完成局部更新。
如下：
~~~html
<div class="layui-form" lay-filter="test1">
  …
</div>
 
<div class="layui-form" lay-filter="test2">
  …
</div>
~~~

~~~js
form.render(null, 'test1'); // 更新 lay-filter="test1" 所在容器内的全部表单状态
form.render('select', 'test2'); // 更新 lay-filter="test2" 所在容器内的全部 select 状态
~~~

#### 5. layui-input相关
form中layui-input默认是width: 100%; display: block; 官网中给的有一个价格输入，他是针对input标签又包括了一个标签`<div class="layui-input-inline" style="width: 100px;"></div>`，我在开发crs中的新版房态页涉及到修改房价dialog中有加减按钮和input在一行的效果，我想到官网的该例子，后来想到何必在加一对标签呢？况且加了一对标签（layui-input-inline）该标签默认左浮动。综合考虑直接在input添加display: inline; 加上一个宽度即可，input左侧与右侧的按钮该怎么写就怎么写！

20191028补充，记忆中 `.layui-input-inline`是 `float: left;` 但是在今天的ReportStatis/ondutyIncomeStatisOld页面中发现使用的 layui-input-inline 并没左浮动。细查发现如下css： 

~~~css
.layui-form-item .layui-input-inline {
  float: left;
  width: 190px;
  margin-right: 10px;
}

.layui-input-inline {
  display: inline-block;
  vertical-align: middle;
}
~~~

只有在 layui-form-item 包裹下的html，才有上面的css，具体效果见：testme/layui/form_module_room_status.html

#### 6. laydate
laydate应用场景：一般是在input中起一个id，在js中，进行渲染。如下代码：
~~~javascript
  // 日期的初始化赋值
  laydate.render({
    elem: '#dateArea',
    range: '至',
    value: '2019-09-26 至 2019-09-26',
    // done: function (value, startDate, endDate) {
    // 	$('input[name=business_time]').val(value);
    // 	console.log(value);
    // 	console.log('startDate', startDate);
    // 	console.log('endDate', endDate);
    // }
  });
~~~

在laydate中添加range属性，laydate默认就是给你一个日期选择范围，若不写range属性，默认就是一个单日历，若`range: '至'`，那么你value中赋值就要符合格式，正确的格式如：`value: '2019-09-26 至 2019-09-26'`，当然了欧美国家一般设置为`value: '2019-09-26 , 2019-09-26'`。注：单日历赋值2019-9-1、2019-09-01都能识别。

---

上面所述的是简单的使用方法，有一些非简单的场景如下：

* select 联动场景 参考链接 [laydate日期选择器类型切换](https://blog.csdn.net/qq_43939956/article/details/90749302) 具体使用实例：在 pms 的报表-营业数据表。

* 2个时间段限制场景 [layui中laydate的使用——设置动态时间范围限制、重置时间范围（清空动态限制）](https://blog.csdn.net/cherry_11qianqian/article/details/82259704)

#### 7. 关于form的思考

最早时候网页很简单，就是一些静态页，后来有了简单的form提交数据，这就是原生的 ` <form> ... </form> `，前后端没有分离，都是写在一块，直接使用原生form提交数据，后来技术发展前后端分离之后，使用接口的形式拉取数据。上面的layui中的form模块是写好的美观的form，在业务复杂情况下也可以直接写元素form的标签，form中原生标签如：input=radio 、button、select、input=checkbox、textarea都是可以写css的，也可以写的符合业务要求。注：个人测试select（css生效）下的option（写css不生效）！

#### 8. layui的form是直接按照其官网的写法即可：
~~~html
<form class="layui-form" action="">
  <div class="layui-form-item" >
  <label class="layui-form-label">输入框</label>
    <div class="layui-input-block">
      <input type="text" name="title" required lay-verify="required"  placeholder="请输入标题" autocomplete="off"  class="layui-input"  >
    </div>
  </div>
  <div class="layui-form-item">
    <div class="layui-input-block">
      <button class="layui-btn" lay-submit lay-filter="formDemo">立即提交</button>
      <button type="reset" class="layui-btn layui-btn-primary">重置</button>
    </div>
  </div>
</form>
~~~

别忘了form中要加上`form.layui-form`，form的每一个项是`<div class="layui-form-item"></div>`，其中`lay-verify="required"`这是验证用的，在input的html代码中加入了required属性，当用户的鼠标放到这个input输入框时候会有一个提示，这个在“layui中form的验证记录.md”文档中有提现。`lay-filter="formDemo"`中的lay-filter是提交的对数据的过滤。若你想传哪个字段如desc字段，要如下写法：

在input中的name属性
~~~html
<input type="text" name="desc" required lay-verify="required" placeholder="请输入标题" autocomplete="off"  class="layui-input">
~~~

对layui中的循环出来的input（集团端的循环出来的门店），一般是不加name，用一个input type=hidden的name来接（隐藏作用域），针对radio这种（认为浏览器自己内部封装的有，你点击之后，name就是获取你当前触发的那个input的value） 

layui中的<input type="checkbox" name="switch" lay-skin="switch">这个开关不开，在form中data.field是不会打印switch的，也许这个是layui的二次封装，radio也是一样，没有默认选中，是不会提交该radio对应的name字段的值。

---

layui中的 select ，若是其下的 option 比较多，可以在 select 标签上加上属性 `lay-search`  在 pms 中看到在form 中加的 class 类 search-form，查资料没发现有该 class 对应的 css效果，可理解为是 开发人员自己写的！还回忆起提交按钮查询时候，加了一些不认识的 class 类，估计这也是工作人员copy的。



#### 9. layui的 margin、padding重置css

~~~css
  blockquote, body, button, dd, div, dl, dt, form, h1, h2, h3, h4, h5, h6, input, li, ol, p, pre, td, textarea, th, ul {
    margin: 0;
    padding: 0;
    -webkit-tap-highlight-color: rgba(0,0,0,0);
  }
~~~

#### 10. layui 的报错记录

* 使用 layer 时报错：`layui s.parents is not a function`，我是在使用 `layer.msg` 时报错，这是应为 layer.msg(msg, {icon: 1}) 输出的 msg 值为Object类型（对象），导致报错。详见下链接：
[layui s.parents is not a function](https://blog.csdn.net/Radish999/article/details/88737483)
注：layer.alert() 里面的内容也不能是 obj。

#### 11. layer的confirm

是给的例子是 

~~~
layer.confirm('未到离店日期，请修改离店日期再退房', { title: "" }, function (index) {
   // layer.close(index);
 });
~~~

不选择 title 效果如下

![confirm无标题](http://file.798run.top/img/blog/20191223/confirm_no_title.jpg)



若是配置了title，效果如下

![confirm有标题](http://file.798run.top/img/blog/20191223/confirm_has_title.jpg)

看自己展示需求进行配置。



