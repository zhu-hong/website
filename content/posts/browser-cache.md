---
title: "浏览器缓存"
date: 2021-10-24T11:43:47+08:00
draft: true
tags: [HTTP]
categories: [Network]
---

## 简介

首先我们发出这样的疑问,浏览器缓存有什么作用,我们知道HTTP协议的过程大概是这样的,客户端发起资源请求,服务端响应资源,而组织一个页面是需要大量的资源的,如果每次请求一个页面都重新请求所有资源,那么这是极其浪费资源和消耗服务器性能的,这个问题如何解决呢,这就要使用到浏览器缓存了

{{< admonition type=warning title="需要注意的是">}}
缓存指令是单向的，这意味着在请求中设置的指令，不一定被包含在响应中。

{{< /admonition >}}

## 准备

首先我们来了解一些HTTP头部

1. `Cache-Control` 缓存指令
2. `Last-Modified` `If-Modified-Since` 通过资源修改时间判断资源是否过期
3. `ETag` `If-None-Match` 通过资源版本号判断资源是否过期
4. `Expires` 缓存过期时间

## 强制缓存

强制缓存是指客户端第一次请求该资源后,以后再请求该资源时,直接使用该资源在浏览器中的缓存,不再去服务器请求

实现强制缓存的关键是响应头中的`Cache-Control`字段,使用强制缓存时,服务端会将`Cache-Control`设置为`max-age=number`number的值为一个数字,单位为秒,表示该资源的缓存时间,如经典的`max-age=31536000`缓存保存一年

强缓存适用与一些长期不会改变的资源,例如css文件

## 协商缓存

强制缓存可以达到缓存效果,那为什么还需要协商缓存呢,我们知道强制缓存在过期时间之内都不会再去服务器请求该资源,那么该资源修改了,例如你给应用新增了一个功能,客户端还在缓存中获取该资源,那么这个新功能就无法应用在客户端,像这类资源就需要协商缓存

实现协商缓存的关键也是响应头中的`Cache-Control`字段,使用协商缓存时,服务端会将`Cache-Control`设置为`no-cache`,注意不要被no-cache的字面意思所迷惑,no-cache不是不缓存,而是可以缓存,但是每次使用该资源前都要和服务器进行确认,服务器进行对比该缓存是否过期之后再响应是否可以使用该缓存,如果缓存资源已经过期,服务器会返回200状态码和对应的新资源,如果资源没有过期,服务器会返回`304`状态码,客户端就会继续使用缓存的资源,那么服务器是如何对比的呢,实现方式有两种

### Modify

响应头`Last-Modified`的值为请求的资源做出修改的日期及时间

请求头`If-Modified-Since`的值为响应头`Last-Modified`的值

服务端通过请求头的`If-Modified-Since`的值判断资源是否有修改

这种实现方法的弊端是使用时间为判断依据,如果资源文件重新生成了一遍而内容实际上没变,也会被认为缓存过期了

### Tag

响应头`ETag`的值为请求的资源对应的唯一字符串,相当于版本号,由服务器生成,每次更新资源都会改变

请求头`If-None-Match`的值为响应头`ETag`的值

服务端通过请求头的`If-None-Match`的值判断资源是否有修改

弱验证/强验证???这个后期再深入了解

{{< admonition type=warning title="需要注意的是">}}
如果请求头中同时含有`If-Modified-Since`和`If-None-Match`,前者是会被忽略的

{{< /admonition >}}

## 参考

> https://developer.mozilla.org/zh-CN/docs/Web/HTTP
>
> https://www.bilibili.com/video/BV1Ch411q79q
>
> https://www.bilibili.com/video/BV17Q4y127We
