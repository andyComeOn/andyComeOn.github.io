---
title: layui中函数使用方法
date: 2019-06-14 10:10:16
tags: layui相关
---

#### 总结一下layui的函数使用方法

1. 针对声明函数如 ` onclick=myClick() `，可在layui.use([], function(){})中写但是必须是window.myClick = function () {}，这能声明成功，但是有时候我们常常使用jq中的 ` $(this) `，这时候this指向不是我们想要的当前元素，而是window。贤心在一篇帖子中回答使用此方法。

2. 还有一种就是使用jq绑定事件写在layui.use([], function(){})中，视情况而定。当然你也可以放到layui.use([], function(){})外面，但是你记得要引入jQuery，否则会报错： Uncaught ReferenceError: $ is not defined。个人感觉就直接写在layui.use之间就可以。

3. 有关table监听有一下几种用法

~~~javascript
// 这下面的 fiterTabname 是指table标签中lay-filter="fiterTabname"
table.on('edit(fiterTabname)'; // table中的input输入
table.on('checkbox(fiterTabname)'; // 点击checkbox
table.on('radio(fiterTabname)'; // 点击radio
table.on('tool(fiterTabname)'; // 这是操作按钮（一般在table中最后一列，通过templet的标识，如：templet: '#barDemo'。针对barDemo是script渲染标签的id，详见下例：）
~~~

~~~html
<script type="text/html" id="barDemo">
    <a class="layui-btn layui-btn-primary layui-btn-xs" lay-event="read">读卡</a>
    <a class="layui-btn layui-btn-danger layui-btn-xs" lay-event="del">删除</a>
</script>
~~~

~~~javascript
// 还要注意table标签中的id="table1"一般是渲染table时候使用，用法如下：
table.render({
    elem: "#table1",
    data: [],
    cols: []
})
~~~

4. 有关form监听有一下几种用法
~~~javascript
form.on('edit(fiterFormname)'; // table中的input输入
form.on('tool(fiterFormname)'; // 这是操作按钮

form.on('checkbox(fiterFormname)'; // 点击checkbox
form.on('radio(fiterFormname)'; // 点击checkbox

// 说明一下，针对上面form监听的几种用法，笔记写的比较早，现在19/09/18发现，有不合理的地方？ ` form.on('tool(***)') `， ` form.on('edit(***)') ` 怎么没发现使用场景？ 在一个页面中form监听常用方法： 

form.on('checkbox(fiterCheckboxName)');
form.on('radio(fiterRadioName)');
form.on('select(fiterSelectName)');
form.on('submit(fiterSubmitName)');

// 细想checkbox使用form监听的场景也很少，本身他就是多选框，就是记录一个状态！场景：选择多标签时候，选择标签数量超过设定数量，禁止点击。

// radio点击之后，切换大状态，改变页面中对应展示！

// 在pms添加消费时候，餐费日期选择，使用jq插入节点，要再次渲染form.render('checkbox');
~~~


5. layui中的css的默认样式
~~~css
button, input, optgroup, option, select, textarea {
    font-family: inherit;
    font-size: inherit;
    font-style: inherit;
    font-weight: inherit;
    outline: 0;
}

.layui-form input[type=checkbox], .layui-form input[type=radio], .layui-form select {
    display: none;
}

.layui-inline, img {
    display: inline-block;
    vertical-align: middle;
}

.layui-btn, .layui-input, .layui-select, .layui-textarea, .layui-upload-button {
    outline: 0;
    -webkit-appearance: none;
    transition: all .3s;
    -webkit-transition: all .3s;
    box-sizing: border-box;
}

.layui-btn, .layui-disabled, .layui-icon, .layui-unselect {
    -webkit-user-select: none;
    -ms-user-select: none;
    -moz-user-select: none;
}

.layui-btn, .layui-edge, .layui-inline, img {
    vertical-align: middle;
}
~~~

针对 ` layui-form-item `，layui中对其写了一个伪类
~~~css
.layui-form-item:after {
    content: '\20';
    clear: both;
    *zoom: 1;
    display: block;
    height: 0;
}
~~~

` layui-form-label `为啥是38px，因为其内部样式写了行高是20px，其padding添加了9px 15px。具体代码如下：

~~~css
.layui-form-label {
    float: left;
    display: block;
    padding: 9px 15px;
    width: 80px;
    font-weight: 400;
    line-height: 20px;
    text-align: right;
}
~~~

在room_pre\的addOccTeamInfo.php 中客户保留了pad-bottom0 pad-top0 lh30，开始估计是不统一，才临时加的css，后来就直接copy 全部嫁接了。

---

在 pms 中的弹框大多是由 `iframe` 形式，在一些弹框中的最后提交的按钮的那一栏 `layui-form-item` 好像不知道谁加入一个 layui-main 样式类，是这个class 有宽度 width: 1140px; margin: 0 auto; 导致弹框出现横向滚动条！如下代码：

~~~html
<div class="layui-form-item layui-main">
    <div class="layui-input-block">
        <button class="layui-btn" lay-submit lay-filter="PostSubmit">立即提交</button>
        <button type="button" class="layui-btn layui-btn-primary" onclick="closeUpWin()">返回</button>
        <input name="do" type="hidden" value="dosubmit"> 
        <input name="id" type="hidden" value="{$StoreInfo.id}">
    </div>
</div>
~~~



6. layui模块使用的demo

~~~javascript
// layui的模块备份
layui.use(['element','table','laydate','form'], function(){
    var element = layui.element;
    var laydate = layui.laydate;
    var layedit = layui.layedit;
    var laypage = layui.laypage;
    var table = layui.table;
    var form = layui.form;
    var $ = layui.$;
});
~~~

在这里补充一下：之前一直理解的是layer，不用引入该模块也可使用，但自己测试 ` layui.use([], function(){ layer.msg(123) }) ` 会提示 layer is not defined，所以可以推理layer是钳在其他模块中，也可理解为use要有模块名字，layer可不写确能使用。偶然间测出了，use table可以不写form，但是只是引入form，不能使用table。猜想是table模块中包含了form逻辑，具体细究。

7. layui的弹框有3个大项：

~~~html
<div class="layui-layer-move" style="cursor: move; display: none;"></div>
<div class="layui-layer-shade" id="layui-layer-shade4" times="4" style="z-index: 19891017; background-color: rgb(0, 0, 0); opacity: 0.5;"></div>
<div class="layui-layer layui-layer-iframe" id="layui-layer4" type="iframe" times="4" showtime="0" contype="string" style="z-index: 19891018; width: 90%; height: 90%; top: 17px; left: 58.5px;"></div>
~~~

8. layui中的html使用demo

~~~html
<div class="layui-row">
    <div class="layui-col-md3">
        <!-- <div class="grid-demo"> -->
            <div class="layui-form-item">
                <div class="layui-inline">
                    <label class="layui-form-label">姓名：</label>
                    <div class="layui-input-block select_after">
                        <input id="nam" type="text" name="user_name" class="layui-input" value="value值" lay-verify="nullStr">
                    </div>
                </div>
            </div>
        <!-- </div> -->
    </div>
</div>
~~~

`<div class="grid-demo"></div>`这个是官网给的例子，在css中这个class无体现！

9. layui中的icon的记录

icon: 
    1绿色对号 
    2红色X号 
    3黄色问好 
    4暗灰色加锁 
    5生气小红脸 
    6开心小绿脸 
    7黄色感叹号

layer.msg('暂无当前日期前7天数据', {icon: 2});

10. layer提供了5种层类型。可传入的值有：0（信息框，默认）1（页面层）2（iframe层）3（加载层）4（tips层）。 若你采用layer.open({type: 1})方式调用，则type为必填项（信息框除外）