---
title: table标签相关总结
date: 2019-09-25 18:59:34
tags: Table相关
---

table在后台管理系统中使用很普遍，有时候在使用它时很多属性都记不清，以至于又临时百度，现在总结一下！

#### 1、table中属性

* table一般html写法是：
~~~html

<table border="1" width="400" rules="all" cellpadding="5" cellspaceing="5" summary="各银行的网上银行支付限额总表">
    <caption>
        <h1>远期客源分析表</h1>
    </caption>
    <colgroup>
        <col width="60px">
        <col width="100px">
        <col>
    </colgroup>
    <tr>
        <th>name</th>
        <th>age</th>
        <th>height</th>
        <th>weight</th>
    </tr>
    <tr>
        <td>andy</td>
        <td>18</td>
        <td>180</td>
        <td>66</td>
    </tr>
</table>
~~~

这里面涉及到html属性：border、width、rules、cellpadding、cellspaceing、summary等。20191220补记 [这里有的属性是表格固有的例如border，不信你可以在一个div上写这个标签属性是不会生效的！在百度百科查的 ASCⅡ码发现html中 对 td写的标签属性 

~~~html
<td height="49" align="center" valign="middle">
    <div class="para" label-module="para">Bin</div>
    <div class="para" label-module="para">(二进制)</div>
</td>
~~~



 ],其中rules是我偶然间看别人这样写，该属性是解决table中边框的。加上 ` rules="all" ` 效果如下：
![tab_rules_all](http://file.798run.top/img/blog/20190925/tab_rules_all.png)
若是不写该属性，tab有外边框及每个td都有边框，效果很不好看，如下：
![tab_rules_no](http://file.798run.top/img/blog/20190925/tab_rules_no.png)
所以大家一般都加上该属性！场景：你在本地临时想测试table问题，没有引入reset.css等库，加上该属性，table效果就好看多了。我在有些reset.css库中发现有对table加上如下css代码：

~~~css
table {
    /* 重置库添加下面代码 */
    border-collapse: collapse;  
    /* table中默认的css */
    border-collapse: separate;
}
~~~

看到上面的css代码才知晓table默认样式是每个td分割的，加上collapse（塌陷），就是让他“塌陷”，不分割（去掉双边框）！可以理解 `border-collapse: collapse;`、`rules="all"`所起的作用一致。 细查一下rules有三种情况：`rules="all"`、`rules="rows"`、`rules="cols"` 在w3c文档中显示效过如下：
![tab_rules_w3c](http://file.798run.top/img/blog/20190925/tab_rules_w3c.png)
这与我本地的效果不一样，我理解只要是加上rules，不论啥属性值，表格最外面都ok的。

* caption 是表头标签，一个完整的表格应该有表头。该表头可以理解为是一个div，里面可以写其他标签。 
* colgroup 标签用于对表格中的列进行组合，以便对其进行格式化。 其内部的子标签是 ` <col span="2" style="background-color:red" width="300px"> ` 宽度可以不带px单位。


#### 2、table中td合并

[<table>标签总结（colspan跨列 ，rowspan跨行）](https://www.cnblogs.com/mmzuo-798/p/6732738.html)

20191028补充，table中的 td 合并是后台管理系统中最常见，使用频率最高的需求。一直以来，对于前后端没有分离的系统，拿php来说，一般是foreach循环出的tr。具体的合并，我们根据原型图需求，并根据后台中的数据结构，巧妙的合并单元格，也可以针对渲染好的返回前端的页面，在用 js 进行单元格二次合并。

今天在给同事套 reportStatis/forwardSourceStatis 的表格时候，出现了理解偏差问题。原型如下：
![远期客源分析表原型需求图](http://file.798run.top/img/blog/20190925/tab_proto.png)
我本想的是，直接循环出每个item(tr)，在针对同类型的item中的不同的房型进行合并rowspan，于是给后端写了静态的tr，后端说这不能使用，没法循环，我们开始交流。后来后端找了一个之前做好的报表（全天收入表ReportStatis/ondutyIncomeStatisOld），说之前是这样弄的，我就结合浏览器页面效果开始看他们的代码，发现他们写的没有我想象的复杂，使我也认识到table 渲染可以这样写（这里展示的是 `全天收入表` 中tbody中相关代码）：

~~~html
<tbody>
    <tr>
        <td rowspan="<?php echo count($data['receptionArr'])+1;?>">前厅</td>
    </tr>
    {volist name="data['receptionArr']" id="vo"}
    <tr>
        <td>{$vo.system_name}</td>
        <td>{$vo.bankcard_amount}</td>
        <td>{$vo.cash_amount}</td>
        <td>{$vo.weixin_amount}</td>
        <td>{$vo.ali_amount}</td>
        <td>{$vo.zhipiao_amount}</td>
        <td>{$vo.tire_transfer_amount}</td>
        <td>{$vo.free_single_amount}</td>
        <td>{$vo.pay_punch_amount}</td>
        <td>{$vo.transfer_account_amount}</td>
        <td>{$vo.sum_total}</td>
    </tr>
    {/volist}

    <tr>
        <td rowspan="<?php echo count($data['financeArr'])+1;?>">Ar账</td>
    </tr>
    {volist name="data['financeArr']" id="vo"}
    <tr>
        <td>{$vo.system_name}</td>
        <td>{$vo.ar_bankcard_amount}</td>
        <td>{$vo.ar_cash_amount}</td>
        <td>{$vo.ar_weixin_amount}</td>
        <td>{$vo.ar_ali_amount}</td>
        <td>{$vo.ar_zhipiao_amount}</td>
        <td>{$vo.ar_tire_transfer_amount}</td>
        <td></td>
        <td></td>
        <td></td>
        <td>{$vo.ar_sum_total}</td>
    </tr>
    {/volist}
</tbody>
~~~

仔细想了一下，这样写不仅简洁，还把合并表格的任务减轻了。想想后端的数据也是来自于sql中查询，后端也不想麻烦进行二次封装，况且他们查出的数据，针对不同的部门，循环各自部门的数据，针对一个部门的数据，数组长度是变化的。直接把该部门名称写死，用一个tr包裹，该tr下仅有一个td，至于td的rowspan的值可以取该部门中数组长度加 1 即可。效果如下：

![全天收入表的效果](http://file.798run.top/img/blog/20190925/tab_all_day.png)

看图中，一般我们认为是6个tr，其实是8个tr，在红线圈起来的也是2个tr，这样很巧妙！

---

接着说上面的需求，由于数据也不好组装，在29号一天，后台同事不在工位，我自己尝试写 循环逻辑。后来感觉基本套好了，现在把后台返回的数据的一部分记录一下：

~~~html
Array(
    [0] => Array(
        [和风间] => Array(
            [0] => Array(
                [team_name] => 微平台预付AR挂账
                [uid] => 147
                [count] => 0
            )

            [1] => Array(
                [team_name] => 携程担保
                [uid] => 148
                [count] => 0
            )
            [2] => Array(
                [team_name] => 美团预付
                [uid] => 150
                [count] => 0
            )
        )
        [动感影音房] => Array(
            [0] => Array(
                [team_name] => 微平台预付AR挂账
                [uid] => 147
                [count] => 0
            )
        )
    )

    [1] => Array(
        [明星商务间] => Array(
            [0] => Array(
                [team_name] => 微平台预付AR挂账
                [uid] => 147
                [count] => 0
            )
        )
    )
)
~~~

最外层数组的索引指的是，预定类型，预订类型对应的是 k : val形式，k指的是8个房型，val是数组（长度24，指的是中介）。


我在使用php的 json_encode() 方法转化一下数据，在控制台打印如下效果：

![远期客源分析表返回的后台数据1](http://file.798run.top/img/blog/20190925/json_ecncode1.png)
![远期客源分析表返回的后台数据2](http://file.798run.top/img/blog/20190925/json_ecncode2.png)

---

#### 3、php的后台返回的数组
如下是基本的房型数组
~~~html
Array
(
    [0] => Array
        (
            [id] => 578
            [name] => 和风间
            [is_check] => 0
        )

    [1] => Array
        (
            [id] => 579
            [name] => 动感影音房
            [is_check] => 0
        )

    [2] => Array
        (
            [id] => 580
            [name] => 明星商务间
            [is_check] => 0
        )
)
~~~
使用 json_encode() 转换之后显示如下：

![远期客源分析表返回的后台数据2](http://file.798run.top/img/blog/20190925/json_ecncode3.png)

个人理解后台的关联数组，转换为json之后，控制台都是显示数组，还是索引数组。可能这就是规范！

#### 4. table的固有的标签属性















