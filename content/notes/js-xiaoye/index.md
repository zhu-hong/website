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

## 你不知道的Object

### assign

合并`自身`可`枚举`属性

同一引用

```javascript
const merged = Object.assign(target, ...sources);

merged === target // true
```

### create

参数

1. 原型指向
2. OwnProperties

```javascript
Object.create(null) // 无原型

Object.create({}, {
  prop: {
    value,
    configurable: false, // 增加删除
    writable: false, // 修改
    enumerable: false, // 枚举
    get: function() {},
    set: function(valve) {},
  }, //descriptor
})
```

### entries/fromEntries

+ 原型不参与
+ 自身可枚举
+ 只作用在第一层

```javascript
{a: 1, b: 2} <=> [['a', 1], ['b', 2]]
```

### freeze

+ 对象(数组)冻结 自身属性
+ 同一引用
+ 浅冻结

```javascript
{
  value,
  configurable: false, // 增加删除
  writable: false, // 修改
  enumerable: true, // 枚举
}
  
obj.__proto__ = {} // X 严格模式报错
```

### seal

1. 对象(数组)封闭 自身属性
2. 同一引用
3. 浅封闭

```javascript
{
  value,
  configurable: false, // 增加删除
  writable: true, // 修改
  enumerable: true, // 枚举
  get, // X
  set, // X
}
  
obj.__proto__ = {} // X
```

### preventExtensions

1. 禁止扩展

```javascript
{
  value,
  configurable: false, // 增加不可删除可
  writable: true, // 修改
  enumerable: true, // 枚举
  get, // X
  set, // X
}
  
obj.__proto__ = {} // X
```

### 总结(非空对象情况下)

| \            | freeze | seal | preventExtensions |
| ------------ | ------ | ---- | ----------------- |
| isFrozen     | y      | n    | n                 |
| isSealed     | y      | y    | n                 |
| isExtensible | y      | y    | y                 |

##数据树形结构化

> 一个扁平数据列表 => 描述树形结构的字段 PID => 树形结构列表
>
> PID => ID

1. 顶级与子级数据分开
2. 递归或其他操作

```typescript
const data = [
  {
    id: 2,
    pid: 0,
    path: '/course',
    name: 'Course',
    title: '课程管理',
  },
  {
    id: 3,
    pid: 2,
    path: 'operate',
    link: '/course/operate',
    name: 'CourseOperate',
    title: '课程操作',
  },
  {
    id: 4,
    pid: 3,
    path: 'info_data',
    link: '/course/operate/info_data',
    name: 'CourseInfoData',
    title: '课程数据',
  },
  {
    id: 5,
    pid: 2,
    path: 'add',
    link: '/course/add',
    name: 'CourseAdd',
    title: '课程增加',
  },
  {
    id: 6,
    pid: 0,
    path: '/student',
    name: 'Student',
    title: '学生管理',
  },
  {
    id: 7,
    pid: 6,
    path: 'operate',
    link: '/course/operate',
    name: 'StudentOperate',
    title: '学生操作',
  },
  {
    id: 8,
    pid: 6,
    path: 'add',
    link: '/student/add',
    name: 'Course',
    title: '学生增加',
  },
]

console.log(formatDataTree(data))

// 扁平化处理
// 第二种写法,有人说只做了第一层,为什么最后还是形成了树,是因为filter拿的是后面数组元素的引用,后面再给子元素添加它的子元素,已经添加到第一层的子元素也会跟着改
function formatDataTree(data: any[]) {
  let _data: any[] = JSON.parse(JSON.stringify(data))

  return _data.filter((p) => {
    const _arr = _data.filter((c) => c.pid === p.id)
    _arr.length && (p.children = _arr)
    return p.pid === 0
  })
}

// 递归写法
function formatDataTree(data: any[]): any[] {
  let [parents, childrens] = [data.filter(p => p.pid === 0), data.filter(p => p.pid !== 0)]

  dataToTree(parents, childrens)

  function dataToTree(parents: any[], childrens: any[]) {
    parents.forEach((p, i) => {
      childrens.forEach((c) => {
        let _childrens: any[] = JSON.parse(JSON.stringify(childrens))
        _childrens.splice(i, 1)
        dataToTree([c], _childrens)

        if (c.pid === p.id) {
          if (!p.children) {
            p.children = []
          }
          p.children.push(c)
        }
      })
    })
  }

  return parents
}
```

## Array.from

取决于`length`若无默认为零,多裁少补`undefined`

## 参数作用域

1. 

```javascript
var x = 1

function test(x, y = function () {
  x = 3 
  // 问题就在于这个x到底是哪个,是全局的还是函数内部的
  // 然而都不是,这个test函数形参中的那个x,这就是参数作用域
  console.log(x) //
}) {
  console.log(x) // 实参 undefined
  
  var x = 2
  
  y()
  console.log(x) // 2
}

test()
console.log(x) // 1
```

2. 

```javascript
var x = 1

function test(a, y = function () {
  x = 3
  console.log(x) // 3 此x为全局的x,因为参数作用域内无x
}) {
  console.log(x) // undefined 此x为函数内部的var x = 2的预编译变量提升
  
  var x = 2
  
  y()
  console.log(x) // 2
}

test()
console.log(x) // 3
```

3. 

```javascript
var x = 1

function test(x = 4, y = function () {
  x = 3
  console.log(x) // 3 参数内的x
}) {
  console.log(x) // 4
  
  var x = 2
  
  y()
  console.log(x) // 2
}

test()
console.log(x) // 1
```

4. 

```javascript
var x = 1

function yy() {
  x = 3
  console.log(x) // 3 全局的x
}

function test(x, y = yy) {
  console.log(x) // undefined
  
  var x = 2
  
  y() // 
  console.log(x) // 2
}

test()
console.log(x) // 3
```

### 参考

> https://www.bilibili.com/video/BV1Wq4y167UZ
