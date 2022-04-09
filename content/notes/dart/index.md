---
title: "Dart"
date: 2022-04-09T10:29:02+08:00
draft: true
categories: [Dart]
---

# Flutter
## Dart
### 变量声明
```dart
// var 自动判断类型,类型确定后无法更改
var age = 12;
age = '123'; // 报错

// dynamic 动态类型,不受类型系统约束
dynamic age = 12;
age = '123'; // 合法

// const 编译时常量 如数字字符串
const age = 12;

// final 运行时常量 如时间 程序执行到所处代码才能确定的值
final time = DateTime.now();

// late 变量延迟赋值 该值声明时不确定值
late num age;
// run code...
age = 123;
```
### Number
```dart
// num声明的变量可以为整数也可以为小数
num age = 12;
age = 12.5;

// int声明的变量只能为整数
int age = 12;
age = 12.5; // 报错

// double声明的变量可以为整数也可以为小数但最终都为小数
double age = 12;
print(age); // 12.0
```
### String
```dart
// 单引号/双引号都可 三引号包含转译符\n\t...
String name = 'zhuh';

// 操作字符串的API基本同JS
```
### List(可理解为JS中的Array)
```dart
// 不限制元素类型
List l = [1, '2', true];

// 限制元素类型, 以下方式作用相同
List l = <int>[1, '2', true]; // 报错 '2' 不是int类型
List<int> l = [1, '2', true]; // 报错 '2' 不是int类型
// 不允许这种写法,不明白为什么 原来dart不支持联合类型
List<int | String> = [1, '2', true];
// 支持...展开运算符
[...l];

// 不加growable: true那么List长度不可变即永远为空
List<num> l = new List.empty(growable: true);

// add 在List末尾添加一个元素 无返回值

// addAll 在List依次末尾添加可迭代对象参数的元素 无返回值

// remove 删除List中第一个出现的元素 返回值是否删除了元素

// removeAt 删除索引对应的元素

// insert 在指定位置插入元素 insertAll 同上addAll

// clear 清空List

// where 同JS中filter过滤 返回的值需要toList

// map forEach 同JS
```
### Set
```dart
// 使用大括号创建
Set s = { 1, 1, 1 };

// 有一套得出两个Set交集/并集/差集的API
```
### Map(可理解为JS中的对象)
```dart
Map m = {
  'name': 'zhuh',
  'age': 12,
};

// 访问属性的值(不支持编程语言中普遍的.) 无该属性返回为null
me['name']

me.keys; // 返回对象所有的key 需要toList

// Map在dart中是可迭代对象类似List有forEach/map等API
```





