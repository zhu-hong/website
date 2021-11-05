---
title: "call,apply,bind,new重写"
date: 2021-11-05T11:41:59+08:00
draft: true
tags: [rewrite]
categories: [JavaScript]
---

```javascript
Function.prototype.callRw = function (ctx) {

  ctx = ctx ? Object(ctx) : window;

  /**
   * this为callRw的调用函数
   * 将this赋给ctx下的一个方法
   * 让ctx调用该方法
   * 那么该方法的this指向就变成了ctx
   * 因为谁调用该函数,该函数的this就指向谁,箭头函数除外
   **/
  ctx.originFunc = this;

  var args = [];

  /**
   * 不能将参数数组直接放入函数执行
   * call的参数是平铺的
   * 使用使用字符串拼接,使用eval执行
   **/
  for (var i = 1; i < arguments.length; i++) {
    args.push('arguments[' + i + ']');
  }

  var res = eval('ctx.originFunc(' + args + ')');
  delete ctx.originFunc;

  return res;
}

Function.prototype.applyRw = function (ctx, args) {

  ctx = ctx ? Object(ctx) : window;
  ctx.originFunc = this;

  if (typeof args !== 'object' && typeof args !== 'function') {
    throw new TypeError('CreateListFromArrayLike called on non-object');
  }

  if (!args || typeOf(args) !== 'Array') {
    var res = ctx.originFunc();
    delete ctx.originFunc;
    return res;
  }

  var _args = [];

  for (var i = 0; i < args.length; i++) {
    _args.push('args[' + i + ']');
  }

  var res = eval('ctx.originFunc(' + _args + ')');
  delete ctx.originFunc;

  return res;
}

function typeOf(value) {
  if (value === null) {
    return 'null';
  }

  return typeof value === 'object' ? {
    '[object Object]': 'Object',
    '[object Array]': 'Array',
    '[object Number]': 'Number',
    '[object String]': 'String',
    '[object Boolean]': 'Boolean',
  }[Object.prototype.toString.callRw(value)] : typeof value;
}

Function.prototype.bindRw = function (ctx) {
  var originFunc = this;
  var args = Array.prototype.slice.callRw(arguments, 1);
  var tempFunc = function () { };

  var newFunc = function () {
    var newArgs = Array.prototype.slice.callRw(arguments);
    return originFunc.applyRw(this instanceof newFunc ? this : ctx, args.concat(newArgs));
  }

  // 圣杯模式
  tempFunc.prototype = this.prototype;
  newFunc.prototype = new tempFunc();

  return newFunc;
}

function newRw() {
  var constructor = Array.prototype.shift.call(arguments);
  var _this = {};

  _this.__proto__ = constructor.prototype;
  var res = constructor.apply(_this, arguments);

  return typeOf(res) === 'Object' ? res : _this;
}
```