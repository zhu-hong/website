---
title: "经验整理"
date: 2021-12-27T19:44:43+08:00
draft: true
categories: [JavaScript]
code:
  maxShownLines: 50
---

## axios柯里化

```typescript
const axiosCurrying = (baseURL) => {
  return (method) => {
    return (url, args) => {
      return new Promise(async (resolve) => {
        let config = {
          baseURL,
          method,
          url,
        }
        if (method.toUpperCase() === 'POST') {
          config['data'] = args
        } else {
          config['params'] = args
        }
        let { data } = await axios(config)
        resolve(data)
      })
    }
  }
}

const jsphd = axiosCurrying('https://jsonplaceholder.typicode.com')
const jsphdGet = jsphd('get')

jsphdGet('posts', {
  _limit: 5
}).then((data) => {
  console.log(data)
})
```

```javascript
const curryingHTTP = (baseURL) => {
  const config = {
    baseURL,
  }
  return {
    get(url, args) {
      return new Promise(async (resolve) => {
        const { data } = await axios(Object.assign(config, {
          method: 'GET',
          url,
          params: args,
        }))
        resolve(data)
      })
    },
    post(url, args) {
      return new Promise(async (resolve) => {
        const { data } = await axios(Object.assign(config, {
          method: 'POST',
          url,
          data: args,
        }))
        resolve(data)
      })
    },
  }
}

```

## 打乱数组

```javascript
function shuffleArray(arr) {

  const newArr = [...arr]

  for (let i = newArr.length - 1; i > 0; i--) {
    const rand = Math.floor(Math.random() * (i + 1));
    [newArr[i], newArr[rand]] = [newArr[rand], newArr[i]]
  }

  return newArr
}
```

## 继承

### 原型链

```javascript
// 继承父类的原型链和父类实例的属性和方法
function SuperType() {
  this.property = true
}
SuperType.prototype.getSuperValue = function () {
  return this.property
};

function SubType() {
  this.subproperty = false
}
SubType.prototype = new SuperType() // 继承 SuperType
SubType.prototype.getSubValue = function () {
  return this.subproperty
}

let instance = new SubType()
instance.getSuperValue() // true
```

### 盗用构造函数

```javascript
function SuperType() {
	this.colors = ['red', 'blue', 'green']
}

function SubType() {
	SuperType.call(this) // 继承 SuperType
}

let instance1 = new SubType()
instance1.colors.push('black')
instance1.colors // 'red,blue,green,black'

let instance2 = new SubType()
instance2.colors // 'red,blue,green'
```

### 组合继承

```javascript
function SuperType(name) {
  this.name = name
  this.colors = ['red', 'blue', 'green']
}
SuperType.prototype.sayName = function () {
  console.log(this.name)
}

function SubType(name, age) {
  SuperType.call(this, name) // 继承属性
  this.age = age
}
SubType.prototype = new SuperType() // 继承方法
SubType.prototype.sayAge = function () {
  console.log(this.age)
}

let instance1 = new SubType('Nicholas', 29)
instance1.colors.push('black')
console.log(instance1.colors) // 'red,blue,green,black'
instance1.sayName() // 'Nicholas'
instance1.sayAge() // 29

let instance2 = new SubType('Greg', 27)
console.log(instance2.colors) // 'red,blue,green'
instance2.sayName() // 'Greg'
instance2.sayAge() // 27
```

### 原型式

```javascript
// 等同与原型链继承与Object.Create()等效
function object(o) {
  function F() { }
  F.prototype = o
  return new F()
}

let person = {
  name: 'Nicholas',
  friends: ['Shelby', 'Court', 'Van']
}

let anotherPerson = object(person)
anotherPerson.name = 'Greg'
anotherPerson.friends.push('Rob')

let yetAnotherPerson = object(person)
yetAnotherPerson.name = 'Linda'
yetAnotherPerson.friends.push('Barbie')
console.log(person.friends) // 'Shelby,Court,Van,Rob,Barbie'
```

### 寄生式

```javascript
function createAnother(original) {
  let clone = object(original) // 通过调用函数创建一个新对象
  clone.sayHi = function () { // 以某种方式增强这个对象
    console.log('hi')
  }
  return clone // 返回这个对象
}

let person = {
  name: 'Nicholas',
  friends: ['Shelby', 'Court', 'Van']
}

let anotherPerson = createAnother(person)
anotherPerson.sayHi() // 'hi'
```

### 寄生组合式

```javascript
// 即所谓的圣杯模式
function inheritPrototype(subType, superType) {
	let prototype = object(superType.prototype)
	prototype.constructor = subType
	subType.prototype = prototype
}
```

### 圣杯模式

```javascript
// 只继承父类原型链且不继承原型实例的属性和方法
function inherit(Target, Origin) {
  function Buffer() { }
  Buffer.prototype = Origin.prototype
  Target.prototype = new Buffer()
  Target.prototype.constructor = Target // 构造器指向自身
  Target.prototype.super_class = Origin // 继承源
}
```