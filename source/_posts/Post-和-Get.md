---
title: Post 和 Get
date: 2017-09-24 21:27:31
tags:
---

> [本文来源自](http://mp.weixin.qq.com/s?__biz=MzI3NzIzMzg3Mw==&mid=100000054&idx=1&sn=71f6c214f3833d9ca20b9f7dcd9d33e4#rd)

## 常规的区别就是大家常说的这些：

- GET 在浏览器回退时是无害的，而 POST 会再次提交请求。

- GET 产生的 URL 地址可以被 Bookmark，而 POST 不可以。

- GET请求会被浏览器主动 cache，而 POST 不会，除非手动设置。

- GET 请求只能进行 url 编码，而 POST 支持多种编码方式。

- GET 请求参数会被完整保留在浏览器历史记录里，而 POST 中的参数不会被保留。

- GET 请求在 URL 中传送的参数是有长度限制的，而 POST 么有。

- 对参数的数据类型，GET 只接受 ASCII 字符，而 POST 没有限制。

- GET 比 POST 更不安全，因为参数直接暴露在 URL 上，所以不能用来传递敏感信息。

- GET 参数通过URL传递，POST 放在 Request body 中。

## 隐藏了个重大区别

GET 产生一个 TCP 数据包,POST 产生两个 TCP 数据包。

对于 GET 方式的请求，浏览器会把 http header 和 data 一并发送出去，服务器响应200（返回数据）；
而对于 POST，浏览器先发送 header，服务器响应 100 continue，浏览器再发送 data，服务器响应 200 ok（返回数据）。

因为 POST 需要两步，时间上消耗的要多一点，看起来 GET 比 POST 更有效。
因此 Yahoo 军规有推荐用 GET 替换 POST来优化网站性能。
但是需要考虑下，因为：

- GET 与 POST 都有自己的语义，不能随便混用。
- 据研究，在网络环境好的情况下，发一次包的时间和发两次包的时间差别基本可以无视。而在网络环境差的情况下，两次包的TCP在验证数据包完整性上，有非常大的优点。
- 并不是所有浏览器都会在POST中发送两次包，Firefox就只发送一次

