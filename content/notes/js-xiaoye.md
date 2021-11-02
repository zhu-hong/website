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

