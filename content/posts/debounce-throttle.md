---
title: "函数防抖与节流"
date: 2021-10-12T23:05:52+08:00
draft: true
categories: [JavaSript]
---

## 简介

防抖`debounce`和节流`throttle`其实是两个非常常用的工具类函数,他们两个非常相似又不完全相似,都是为了解决**事件短时间内多次触发导致资源浪费**的问题,比如有一个按钮,给点击事件绑定了一个请求数据的事件处理函数,正常情况,每点击一次就发送一次请求,但是如果有异常行为,短时间内多次请求,比如有恶意脚本一秒内点击1000次,那么真的就发送1000次请求吗,这肯定是不行的,这不符合正常用户的操作,也会给服务器增加压力,而解决这种问题最基础的方法就是函数防抖和节流

## 防抖

防抖是指事件触发n秒后再执行,若n秒内该事件再次触发则重新计时

那么现在我们缕一缕要实现的功能

1. n秒内只触发一次事件,就需要两个参数`要执行的事件` `延迟时间`
2. 是否立即执行一次,这个功能就需要第三个参数

下面是一个防抖函数的基本实现

```javascript
function debounce(fn, time, triggerNow) {
  // 判断是否为首次执行
  let timer = null;

  function debounced() {
    let [__this, args, res] = [this, arguments, undefined];

    if (timer) {
      clearTimeout(timer);
    }

    if (triggerNow) {
      let exec = !timer;

      // 只有初次触发,timer才为null,那么exec为真,才执行函数
      if (exec) {
        res = fn.apply(__this, args);
      }

      // 结束本次防抖
      timer = setTimeout(() => {
        timer = null;
      }, time)
    } else {
      timer = setTimeout(() => {
        res = fn.apply(__this, args);
      }, time)
    }

    return res;
  }

  debounced.remove = function () {
    clearTimeout(timer);
    timer = null;
  }

  return debounced;
}
```

1. 首先函数接收三个参数
   1. `fn`要执行的函数
   2. `time`延迟时间
   3. `triggerNow`是否立即执行一次

2. 定义一个变量`timer`判断函数是否为防抖期间内第一次执行

{{< admonition type=warning title="需要注意的是">}}
这里的timer是在返回函数外定义的,形成了闭包,所以每次执行函数不会重置它的值为null

{{< /admonition >}}

1. 如果是防抖期间内第一次执行,它为`null`

   1. 然后进入判断`if(triggerNow)`是否立即执行一次
      1. 变量`exec`为`!timer=true`,只有`timer`为`null`它才为真,进入判断`if(exec)`执行`fn`
      2. 然后将`timer`设为一个定时器,`time`毫秒后,再将`timer`赋值为`null`

2. 如果不是防抖期间内第一次执行,那么它必定不为`null`

   1. 它会走`if(timer)`将定时器清除
   2. 定时器清除,`timer`就无法执行`timer=null`
   3. `timer`不为`null`,`exec`为`!timer=false`,`fn`就不会执行

   {{< admonition type=warning title="需要注意的是">}}
   若timer不为null,

   每次函数执行永远都会清除定时器,

   清除定时器timer就永远都不会为null,

   不为null,exex即永远为false,

   exec为false,fn就永远不会执行

   这就达到了n秒内只执行一次fn的效果

   {{< /admonition >}}

## 节流

节流是指无论触发多少次事件,事件处理函数稳定n秒触发一次

下面是一个节流函数的基本实现

```javascript
function throttle(fn, delay) {
  let [timer, bigin] = [null, new Date().getTime()];

  function throttled() {
    let [__this, args, res, current] = [this, arguments, undefined, new Date().getTime()];

    if (timer) {
      clearTimeout(timer);
    }

    if (current - bigin >= delay) {
      res = fn.apply(__this, args);
      bigin = current;
    } else {
      timer = setTimeout(() => {
        res = fn.apply(__this, args);
      }, delay);
    }

    return res;
  }

  throttled.remove = function () {
    clearTimeout(timer);
    timer = null;
  }

  return throttled;
}
```

这里的`timer` `bigin`与防抖函数的`timer`一样形成了闭包不会重置

`bigin`记录最近一次执行`fn`函数的事件

每次执行函数都获取一次执行的事件`current`

比较上次执行`fn`是否是在规定时间`delay`之后

如果是的话立马执行`fn`并将本次执行`fn`的时间`current`赋给`bigin`用来延迟下一次执行

## 实践

[debounce-throttle (dev-zh.vercel.app)](https://dev-zh.vercel.app/example/debounce-throttle/)
