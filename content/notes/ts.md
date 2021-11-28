---
title: "ts"
date: 2021-11-25T07:47:44+08:00
draft: true
categories: [TypeScript]
code:
  maxShownLines: 100
---

## any/unknown/void/never

### any

此类型变量和写js无异,表示它可以是任何类型,该变量的操作不受任何限制,在ts中如果一个声明只声明了,没有赋值或者规定类型,那么这个变量默认为`any`类型,任何类型的值可以赋给它,它也可以赋给任何类型的值,自由度太高,建议从不使用

```typescript
let go: any

go = '123'
go.co
go.cao()

const co: (a: number) => number = (a) => a * 2

co(go)

let me: number
me = go
```

### unknown

该类型声明之后再赋值不会自动进行类型判断,它始终只会是`unknown`类型,任何类型的值可以赋给它但是没有意义,它只能赋给同为`unknown`类型的值

```typescript
let go: unknown

go.co // Property 'cao' does not exist on type 'unknown'
go.cao() // Property 'cao' does not exist on type 'unknown'

go = 123
const co: (a: number) => number = (a) => a * 2
co(go) // rgument of type 'unknown' is not assignable to parameter of type 'number'

let me: number
me = go // Type 'unknown' is not assignable to type 'number'
```

只有断言能再次改变它的类型,所以使用`unknown`类型数据时必须先断言其类型

```typescript
let go: unknown

go.co // Property 'cao' does not exist on type 'unknown'
go.cao() // Property 'cao' does not exist on type 'unknown'

go = 123
const co: (a: number) => number = (a) => a * 2
co(go as number)

let me: number
me = go // Type 'unknown' is not assignable to type 'number'
```

### void

表示一个空值,没有意义,可以在`strictNullChecks`为`false`的情况下能且只能赋给`undefined`/`null`,一般表示没有任何返回值的函数的返回值类型

```tsx
let me: void
me = undefined
me = null
me = 'qwert' // Type 'string' is not assignable to type 'void'
me = 123 // Type 'number' is not assignable to type 'void'
me = true // Type 'boolean' is not assignable to type 'void'
me = {} // Type '{}' is not assignable to type 'void'

function go(): void {
  alert('cao')
}
```

### never

此类型表示一个不该存在的值的类型,非常的抽象,看代码

```typescript
type me = number & string // type me = never
// 此处me表示取number和string的交集为自己的类型,但是众所周知这个交集为空或者说不存在,所以为never
type co = number | string
function go(cao: co) {
  if(typeof cao === "string") {
    // 这里 cao 被收窄为 string 类型
  } else if(typeof cao === "number") {
    // 这里 cao 被收窄为 number 类型
  } else {
    // cao 在这里是 never,因为co只能为number或者string类型,而cao能走到这里表示它既不是number也不是string
    // 那它就是一个不存在的类型,既为never
  }
}

// 一个函数正常执行完肯定是有返回值的,没有显示return也会隐式返回undefined,那么没有正常执行完返回的就是never
// 没有正常执行完,总是抛出一个错误
function error(message: string): never {
  throw new Error(message)
}
// 返回一个never
function fail() {
  return error("Some error happened")
}
// 这个函数陷入死循环没有返回值
function infiniteLoop(): never {
  while (true) { }
}
```

## 联合类型 |

需要一个共有且值不同的类型来做判断,比如下方的`kind`

```typescript
interface Square {
  kind: 'square'
  size: number
}

interface Rectangle {
  kind: 'rectangle'
  width: number
  height: number
}

type Shape = Square | Rectangle

function area(s: Shape) {
  if (s.kind === 'square') {
    // 现在 TypeScript 知道 s 的类型是 Square
    // 所以你现在能安全使用它
    return s.size * s.size
  } else {
    // 不是一个 square ？因此 TypeScript 将会推算出 s 一定是 Rectangle
    return s.width * s.height
  }
}
```

> https://jkchao.github.io/typescript-book-chinese/typings/discrominatedUnion.html

## 函数

```typescript
const co: (cao: number) => number = (cao) => Math.pow(cao, cao)

const co = (cao: number): number => Math.pow(cao, cao)

type co = (cao: number) => number
const go: co = (cao) => Math.pow(cao, cao)

interface co {
  (cao: number): number
  fc: string
}
const go: co = (cao) => Math.pow(cao, cao)
go.fc = 'qwert'
```

