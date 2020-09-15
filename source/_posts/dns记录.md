---
title: dns记录
date: 2020-07-22 12:36:46
tags: 工具使用总结
---

> 记录一次有关 dns 范畴的知识

## 1、github 不能打开部分图片

在访问 github 时候，一些图片一直打不开看着很难受，在没有买 vpn 的情况下，于是找到的方法是：配置 host 。于是配置如下：

```
172.217.160.0	www.google.com.hk

199.232.68.133 raw.githubusercontent.com

151.101.56.133 avatars0.githubusercontent.com
151.101.56.133 avatars1.githubusercontent.com
151.101.56.133 avatars2.githubusercontent.com
151.101.56.133 avatars3.githubusercontent.com
151.101.56.133	avatars4.githubusercontent.com

151.101.56.133	avatars5.githubusercontent.com
151.101.56.133	avatars6.githubusercontent.com
151.101.56.133	avatars7.githubusercontent.com
151.101.56.133	avatars8.githubusercontent.com
```

图片不能打开的问题已解决！但发现 https://api.github.com/_private/browser/stats 链接不能访问，一直提示 403 rate limit exceeded。很少见！

## 2、思考 api.github.com 不能访问的原因

我直接配置 host 不就完事了， 于是:

![api.github.png](http://file.798run.top/img/blog/20200722/api.github.png)

随后配置 dns 依然是没用！于是打开访问助手，直接访问 https://api.github.com/_private/browser/stats ，看到返回的信息中有一个 ip，接着配置 host ，依然是不行！查到原因是：[How to fix the Runtime Error 403 Rate-limit exceeded](https://www.errorvault.com/en/troubleshooting/runtime-errors/google-inc/picasa/error-403_rate-limit-exceeded)、[解决 gdrive 报错 Error 403: Rate Limit Exceeded](https://bugxia.com/1234.html)

## 3、总结

- 访问助手使用，还没有我配置 host => 151.101.56.133 avatars0.githubusercontent.com 访问快！还想收费这也太不要脸了！
