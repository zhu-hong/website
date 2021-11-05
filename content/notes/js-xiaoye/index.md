---
title: "js-xiaoye"
date: 2021-11-01T22:12:08+08:00
draft: true
categories: [JavaScript]
---

## 预编译

1. 通篇检查语法错误
2. 解释一行,执行一行

## AO(activation object,活跃对象,函数执行期上下文)

1. 寻找形参和变量声明
2. 实参赋值给形参
3. 寻找函数声明,赋值
4. 执行

## GO(global object,全局执行期上下文)

1. 寻找变量
2. 寻找函数声明
3. 执行

### 一些题目

```javascript
if(typeof(a) && (-true) + (-undefined) + ''){
  console.log('pass')
} else {
  console.log('unpass')
}
// pass

console.log(!!' ' + !!'' + !!false || 'pass')
// 1
```

## 作用域链

![scope-chain0](./scope-chain0.webp)

![scope-chain1](./scope-chain1.webp)

函数在被定义还没执行时就已经有了全局作用域

`[[scope]]`函数执行完即被销毁,使用外部正常情况下无法访问函数内部数据

当内部函数被返回到外部并保存时,一定会产生闭包,闭包会产生原来的作用域链不释放

## 原型

`prototype` 函数的一个属性

`__proto__` Object的一个属性 类似于链条,连着上一层

object的`__proto__`保存着构造函数的`prototype`

```javascript
Function.__proto__ === Function.prototype // true 底层规定
Object.__proto__ === Function.prototype // true
```

## new

过程

1. 生成一个空对象 this -> {}
2. `this.__proto__` -> 构造函数.prototype
3. 返回this

## 生成器

```javascript
// 生成器的存在为了返回一个迭代器
```

![generator](./generator.webp)

![iterator](./iterator.webp)

