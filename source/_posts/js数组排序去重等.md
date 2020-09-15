---
title: js数组排序去重等
date: 2019-04-15 10:32:19
tags: JS相关
---


针对js数组排序，有几种方法，现总结一下，以免自己忘记。

#### 1. js数组的排序之sort()方法

* 数组有一个sort()方法，可用之简单排序，如下代码：
~~~javascript
var tmpArr = [1, 4, 6, 9, 2];
functon sortMethod(a, b) {
    if (a > b) {
        return 1
    } else if (a < b) {
        return -1
    } else { return 0 }
}

console.log( tmpArr.sort(sortMethod) );
~~~

可满足从小到大排序。

* 若需要是从大到小排序，则可以改造为：

~~~javascript
var tmpArr = [1, 4, 6, 9, 2];
functon sortMethod(a, b) {
    if (a > b) {
        return -1
    } else if (a < b) {
        return 1
    } else { return 0 }
}

console.log( tmpArr.sort(sortMethod) );
~~~

#### 2. 冒泡排序

* 冒泡的含义，来源于生活，是一个形象的比喻，就像是锅中烧水，小泡在下面，大炮在上面，小泡慢慢从下面升起来跑到上面。 所谓的冒泡排序就是从最开始的第一个数arr[0]，与他临近的arr[1]比较，若arr[0] > arr[1]，对调位置（类似于冒泡）。这时候新的arr[1]继续与arr[2]比较，若满足条件对调位置，知道把数据循环一遍。循环一遍结束后，这只是把arr[0]与其他元素做了比较，这时候要对新的arr[1]与他之后的元素依次比较，所以这会有双重循环。 看代码：

~~~javascript
var tmpArr = [5, 4, 3, -1];
functon bubbleSort(arr) {
    // 第一次循环，针对每一个元素分别走一遍，让他开始冒泡
    for (var i = 0; i < arr.length - 1; i++ ) {
        // 第二次循环，每一个元素分别比较之后，继续与他右边元素比较，一直到循环到头
        for (var j = 0; j < arr.length - 1 - i; j++) {
            if (arr[j] > arr[j+1]) {
                var tmp = arr[j];
                arr[j] = arr[j+1];
                arr[j+1] = tmp;
            }
        }
    }
    return arr
}

console.log( bubbleSort(tmpArr) );
~~~
解释一下这里的代码含义，为什么是 arr.length - 1 ，arr.length - 1 - i主要是内层的循环，为了代码不进行多余的循环，还有一个重要的原因，上面的代码中有arr[j+1]，要是不限制，有可能数组越界。 第二次循环若是 < 4,则j 有可能等于3，则arr[j+1] 既是arr[4]，所以这是不合理的。

#### 3. 数组去重
* 数组去重，一般是想到使用indexOf('el')找到其索引，在直接对数组使用 arr.splice(index, 'el')即可！我在集团端crs中direct_hotel\room_status.php中使用到数组去重逻辑，具体如下：很多房型是form中的input type="checkbox"，每次选中或者取消checkbox要实时获取数组中有效房型。具体代码：

~~~JavaScript
var roomTypeArr = [];
form.on('checkbox(filterRoomType)', function (data) {
    console.log(data.value);
    if (data.elem.checked) {
        if (roomTypeArr.indexOf(data.value) == -1) {
            roomTypeArr.push(data.value);
        }
    } else {
        var ind = roomTypeArr.indexOf(data.value);
        if (ind != -1) {
            roomTypeArr.splice(ind, 1);
        }
    }
    console.log(roomTypeArr);
})
~~~

原生jq点击事件，针对checkbox模拟上面的场景
~~~html
<form>
    <div class="hobbyWrap">
        <input type="checkbox" name="hobby[]" value="看书" id="checkbox1" class=""> <label
            for="checkbox1">看书</label>
        <input type="checkbox" name="hobby[]" value="写字" id="checkbox2"> <label for="checkbox2">写字</label>
        <input type="checkbox" name="hobby[]" value="学习" id="checkbox3"> <label for="checkbox3">学习</label>
    </div>
</form>
~~~
~~~JavaScript
var hobbyArr = [];
$('.hobbyWrap').on('click', ':checkbox', function () {
    var v = $(this).val();
    console.log(v, $(this).prop("checked"));
    if ($(this).prop("checked")) {
        if (hobbyArr.indexOf(v) == -1) {
            hobbyArr.push(v);
        }
    } else {
        var ind = hobbyArr.indexOf(v);
        if (ind != -1) {
            hobbyArr.splice(ind, 1);
        }
    }
    console.log(hobbyArr, hobbyArr.join(','));
})
~~~

在网页上也看到过对Array的原型链上绑定一个方法，直接可以使用。
~~~javascript
Array.prototype.remove = function (v) {
    var i = this.indexOf(v);
    if (i > -1) this.splice(i, v);
}
var arr = [1, 2, 3];
arr.remove(3);
console.log(arr);
~~~
