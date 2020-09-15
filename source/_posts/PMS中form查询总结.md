---
title: PMS中form查询总结
date: 2019-11-26 14:51:58
tags:
---

写在前面：空格缩进、代码换行、注释描述、结构清晰、 一个系统面对的是形形色色的用户，不要想当然啥，例如loading的添加。最终趋势还是前后端分离！

PMS在很多页面都是查询数据为主，为此在该项目中，前同事封装了一些查询的方法。这里做一些总结！

### 1、页面布局最好遵循下面的说明
页面body中有一个根容器 `<div class="layui-row pad10"></div>`，紧接着是form标签，下面就是table标签。Form是用来提交数据这里面包含查询条件等，table中若有表格名字、一些信息，可看情况写在table中的`<caption><h1>*表</h1><div></div></caption>`。当然这种表格头部信息写法的前提是表格已经存在了，就是最普通的表格，在html上已经全部渲染好了。若是使用下文中table.render方法（此时table字标签还没有渲染好），那就在table标签上面写div，写清楚table表头相关信息即可！



### 2、需要 layui form.verify 验证

这种场景：需要验证form-item中input的name值。所以需写成layui中lay-submit的形式！这样才能很好的使用layui中封装的form.verify方法和效果。补充：使用form.on('submit')，就要阻止 form默认提交，可在form.on('submit')函数中末尾加return false、可把button的type默认提交修改为按钮类型。

代码如下：
~~~javascript

// 验证 满足什么条件
form.verify({
    verifyAddtime: function (value) {
        var temp = value.split(' , '); 
        if (date_subtraction(temp[0], temp[1]) > 90) {
            return '查询时间间隔不能超过 3 个月，请重新选择时间段！';
        } 
    }
});  

// 提交
form.on('submit(filterSubmit)', function(data) { 
	table.reload('lists', {
    	where: data.field,
        page: {
            curr: 1 
        }
    });
}) 

~~~

### 3、提交使用commonjs中封装的 `getListData(); return false;`的情况
* 对于这种情况，不能一概而论，要看场景的需要，归根到底就是form的原生提交与否，是否使用layui中的verify的效果和方法。每次后台套页面之后提交了，测试人员看到，告诉前端时间段要验证，这是事后诸葛亮的表现，事先可以规划好。使用 getListData 也许之前是封装的一个方法，可是没有细究，有些小问题没考虑到。还要注意的是 form 中的默认提交问题，有的是需要使用默认的提交，有的不需要（此种情况，一定要给与阻止！）。对于规则的表格最好是使用 `<table class="layui-table" id="lists" lay-filter="filterLists"></table>`。在js中使用渲染的方式！

~~~javascript
table.render({
    elem: '#lists',
    url: '/ReportStatis/forwardEarningsStatis',
    title: '远期收益分析表', // 发现这个title不起作用！
    totalRow: true,
    cols: [[
        {field: 'business_time', title: '日期', align:'center', totalRowText: '合计'},
        {field: 'week', title: '星期', align:'center'},
        {field:' usable_room_num', title:'可用房数', align:'center', totalRow:true},
    ]],
    response:{
        statusName:'status',
        statusCode:1,
        msgName:'msg',
        countName:'total',
        dataName:'data'
    },
    page: false,
    // limit: 0,
    limit: Number.MAX_VALUE,
    done: function (data) {
        // 取消分页展示
        $('.layui-table-page').hide();
    }
});
~~~

* 切记只有这种第一次页面已经使用render方法的表格，才能使用`table.reload()`函数。
~~~javascript
table.reload('lists', {
    where: data.field,
    page: {
        curr: 1 
    }
});
~~~
 
* 为什么要使用 `<table lay-data="{}"> ... </table>`这种方式？官网上说此种写法是为了 SEO 优化，PMS一般是功能软件，需要SEO优化吗？况且使用这种方式，格式也没有对齐，即使对齐占用很大的篇幅，在done中写回调代码也不高亮！

* 这种最常见的情况（后台也是最快做出页面）阻止了form默认提交，在页面加载完之后，再次输入查询条件，点击查询按钮执行的函数就是commonjs中的getListData方法。getListData是一个封装好的方法，很多页面都在使用，这时候要是测试让前端人员修改verify逻辑怎么办，原方法不能动，只能在该页面重新写，所以我认为慢慢全部把这些东西解耦，说明开始封装的方法有的地方不合适！所以该场景，测试若要验证form，就在页面中单独写逻辑。


### 4、使用原生form提交，table是灌好的数据（report_statis/forwardSourceStatis 远期客源分析表）
该例子使用form原生提交，应用了lay-verify验证，数据及页面结构在后台已经渲染好（我写的后台循环），结构清晰，可做模板使用。注：此类型表格是一个统计表格无分页场景。就是要应用原生form的提交功能。layui中lay-submit应该是封装了，只要是不满足verify，即使是type是默认提交，也不会给你提交！

### 5、筛选条件：按钮 + form
查询条件：按钮选择 + form中的input筛选条件，在测试ar_account/rechargeList页面时候，测试要添加时间段区间验证，把getListData方法删除，直接写。发现按钮区的逻辑依然是正常。20191127补记，就拿rechargeList.php来说，上面有按钮，点击按钮的逻辑：在commonjs中写好的按钮绑定函数（处理样式和筛选出data-name值2个大逻辑），获取到data-name的值之后，再次与form中的input name值组合，进而where查询。
