---
title: Form的相关记录
date: 2019-12-18 14:14:09
tags: Form表单相关
---

> 在 web早期，大家使用 form 提交数据居多，现对 form 相关知识进行一下总结。

## 提交方式 Get Post

表单有两种提交方式，POST和GET。通常我们会使用POST方式，一是因为形式上的安全 ；二是可以上传文件。下面先看一段 html 代码：

~~~html
<form action="/xxxx" method="" enctype="application/x-www-form-urlencoded">
  username: <input type="text" name="username"><br>
  password: <input type="text" name="password"><br>
  <input type="submit" value="提交">
</form>
~~~

针对一个 form 想到三大件 `action`、`method`、`enctype` 其中 action 是你的表单提交的地址，method 是表单的提交方式，大家一般就是使用 2 种 get、post。method的属性值若是不写就会默认为 get。 enctype 是指你的表单编码类型，这个单词我是这样记忆 en(使能) + c(content编码) + type ，针对 html 表单 enctype的常见类型详见下表：

| **enctype**                       | 说明                                      |
| :-------------------------------- | ----------------------------------------- |
| application/x-www-form-urlencoded | 默认编码方式， key1=value1&key2=value2    |
| multipart/form-data               | 普通表单提交，以及表单文件上传            |
| text/plain                        | 该方式不常用，数据获取方式 getInputStream |

其实 Content-Type 是 http/https 发送信息至服务器时的内容编码类型，Content-Type 用于表明发送数据流的类型，服务器根据编码类型使用特定的解析方式，获取数据流中的数据。上面表格仅仅是 html 表单支持的编码方式，实际上面还有存在各种文件类型的编码方式：

~~~
image/jpeg, image/png, image/gif, text/css, text/javascript,text/html,text/xml 等等
~~~

表单的 get 请求方式，在浏览器控制台可以看到 参数编码前后的数据：



![get_type_url_encoded](http://file.798run.top/img/blog/20191219/get_type_url_encoded.jpg)



![get_type_url_decoded](http://file.798run.top/img/blog/20191219/get_type_url_decoded.jpg)

在其请求头里不会添加 `Content-Type` 字段。

## POST 请求方式

```html
<form action="/xxxx" method="post">
  username: <input type="text" name="username"><br>
  password: <input type="text" name="password"><br>
  <input type="submit" value="提交">
</form>
```

1. post 请求 即使你没有在表单里写 `enctype="application/x-www-form-urlencoded"` ，也会在请求头里默认给你加上application/x-www-form-urlencoded ，这是 post 默认类型。

2. 还有一种类型是 `enctype="application/form-data"`这种类型是为了解决上面的类型在传送 非 ASC2码效率低问题，此类型还可以传送文件！这种传输方式有 在 form data 中有边界提示：

   ![post_type_multipart](http://file.798run.top/img/blog/20191219/post_type_multipart.jpg)



3. post 主要是这 2 种使用较多。

---

[参考链接]: https://blog.csdn.net/yamadeee/article/details/84250297	"表单提交 multipart/form-data 和 x-www-form-urlencoded的区别"



