---
title: "CORS与跨域"
date: 2021-10-08T19:19:33+08:00
draft: true
categories: [Network]
---

## 简介

`CORS`是一个W3C标准,全称为`Cross-origin resource sharing`,中文意思为**跨域资源共享**

我们知道由于浏览器的安全机制,浏览器会禁止AJAX发起跨域请求,而CORS机制可以帮助我们安全得成功进行跨域请求与响应,前提是`客户端`与`服务端`同时支持.

当浏览器发现AJAX需要跨域时会自动在请求头添加一些信息,无须用户手动添加,所以开发上和普通的AJAX无异,那么关键就在服务端.

## 简单请求

### 条件

> 1. 请求方法
>
>    + GET
>
>    + HEAD
>
>    + POST
>
> 2. 请求头字段集合
>
>    + 浏览器自动配置
>
>    + Accept
>    + Accept-Language
>    + Content-Language
>    + Content-Type
>      + text/plain
>      + multipart/form-dat
>      + application/x-www-form-urlencoded

### 请求头

+ `Origin`(请求来源)

### 响应头

+ `Access-Control-Allow-Origin`(被允许访问的外域集合)
  + `*`表示允许任何外域访问

## 非简单请求

不满足简单请求的条件即为非简单请求,参考上方

在发起非简单请求时,浏览器会先自动发起一个`预检请求`,再发起真正的请求

### 预检请求

预检请求的请求方法为`OPTIONS`是HTTP/1.1协议中定义的方法,用以从服务器获取更多信息,该方法不会对服务器资源产生影响.

### 请求头

+ `Access-Control-Request-Method`(告知服务器实际请求方法)
+ `Access-Control-Request-Headers`(告知服务器实际请求包含的自定义请求头字段)

### 响应头

+ `Access-Control-Allow-Methods`(表明服务器允许使用的请求方法)
+ `Access-Control-Allow-Headers`(表明服务器允许请求中携带的自定义字段)

## 总结

1. CORS分为简单请求与非简单请求
   + 浏览器会在简单请求中加上关键字段`Origin`,服务端根据该字段响应`Access-Control-Allow-Origin`字段来控制是否允许该源访问
   + 非简单请求之前会进行一次预检请求,预检请求会通过`Access-Control-Request-Method`,`Access-Control-Allow-Headers`等字段携带真实请求的相关信息,服务端根据这些字段响应`Access-Control-Allow-Methods`,`Access-Control-Allow-Headers`等字段来告知是否允许访问,浏览器接到预检响应之后再进行真实请求
2. 预检请求(OPTIONS)的本质是询问服务端被允许请求的格式
3. 简单请求不会触发预检请求

## 参考

> [跨源资源共享（CORS） - HTTP | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/CORS)