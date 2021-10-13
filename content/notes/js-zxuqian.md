---
title: "JS学习笔记(B站峰华前端工程师)"
date: 2020-10-23T11:34:01+08:00
draft: true
categories: [JavaSript]
---

## 函数

### 函数与变量的提升

+ 这样函数就可以先调用再定义,结构更清晰

### 默认参数

```JavaScript
function greetings(name = "峰华"){
    console.log("你好," + name);
}
greetings();//你好,峰华
greetings("张三");//你好,张三

function greetingWithWeather(name = "峰华", weather){
    console.log("你好," + name + ",今天是:" + weather);
}
greetingWithWeather(undefined, "晴天");//你好,峰华,今天是晴天
```

### 递归

```JavaScript
function sum(n){
    if(n===1){
        return 1;
    }
    return n + sum(n -1);
}
console.log(sum(100));//5050

//1 1 2 3 5 8 13
function fibnc(num){
    if(num <= 1){
        return 1;
    }
    return fibnc(num -1) + fibnc(num - 2);
}

console.log(fibnc(6));//13
```

### arguments

```javascript
function log(){
    for(let i = 0; i < arguments.length; i++){
        console.log(arguments[i]);
    }
}

log("abc", "def", "ghi");//传多少,打印多少
```

### 箭头函数

```javascript
var greeting = (name, weather) => {
    console.log("Hello," + name + ",今天是" + weather);
}
greeting("峰华", "晴天");//Hello,峰华,今天是晴天

var increment = x => x + 1;
console.log(increment(1));//2
```

### 闭包`函数里面再定义函数`

```javascript
function squareSum(a, b){
    function square(x){
        return x * x;
    }
    
    return square(a) + square(b);
}

console.log(squareSum(2, 3));//13

//高阶函数(Higher-order function)返回函数的函数
function person(){
    let name = "峰华";
    function getName(){
        return name;
    }
    
    return getName;
}

var getName = person();
console.log(getName());//峰华
```

### 柯里化(curry)`接收多个参数的函数变成多个接收一个参数的函数`

```javascript
function addThreeNums(a, b, c){
    return a + b + c;
}

function addThreeNumsCurry(a){
    return function(b){
        return function(c){
            return a + b + c;
        };
    };
}

console.log(addThreeNums(1, 2, 3));//6

console.log(addThreeNumsCurry(1)(2)(3));//6

var fixedTwo = addThreeNumsCurry(1)(2);//3
console.log (fixedTwo(3));//6
console.log (fixedTwo(4));//7
console.log (fixedTwo(5));//8
```

### 自执行函数

```javascript
var num1 = 10;

(function(){
    var num1 = 20;
    console,log(num1);
})();//20

console.log(num1);//10
```

### 回调函数

```javascript
function request(cb){
    console.log("请求数据");
    cb("succee");
    console.log("请求结束");
}

/*
function callback(result){
    console.log("执行回调");
    console.log("执行结果是" + result);
}

request(callback);
*/

request(result => {
    console.log("执行回调");
    console.log("执行结果是" + result);
});
```

## 数组

### 删除元素

```javascript
var arr = [1, 2, 3, 4, 5, 6];

arr.splice(2, 1);
console.log(arr);//[1,2,4,5,6]

arr.splice(1, 2, 3, 7, 8);
console.log(arr);//[1, 3, 7, 8, 5, 6]

arr.splice(1, 0, 9, 10);
console.log(arr);//[1, 9, 10, 3, 7, 8, 5, 6]
```

### 遍历

```javascript
var arr = [1, 3, 5, 7, 9];

for(let ele of arr){
    console.log(ele);
}

arr.forEach((ele, index, self) => {
    console,log(ele, index, self);
});
```

### 栈模式

```javascript
var stack = [1, 2, 3];

stack.push(4);
console.log(stack);//[1, 2, 3, 4]

stack.push(5, 6, 7);
console.log(stack);//[1, 2, 3, 4, 5, 6, 7]

var last = stack.pop();
console.log(last);//7

console.log(stack);//[1, 2, 3, 4, 5, 6]
```

### 队列

```javascript
var queue = [1, 2, 3];
queue.push(4, 5, 6);
console.log(queue);//[1, 2, 3, 4, 5, 6]
var first = queue.shift();console.log(shift);//1
console.log(queue);//[2, 3, 4, 5, 6]
queue.unshift(10, 11, 12);
onsole.log(queue);//[10, 11, 12, 2, 3, 4, 5, 6]
```

### 反转数组

```javascript
console.log("hello".split("").reverse().join(""));//olleh
```

### sort

```javascript
var arr = [1, 5, 3, 2, 4];
arr.sort();console.log(arr);//[1, 2, 3, 4, 5]
arr.sort((a, b) => {  
  if(a > b){     
    return 1;  
  }else if(a < b){     
    return -1;  
  }else{     
    return 0;  
  }
});
arr.sort((a, b) => b -a);
console.log(arr);//[5, 4, 3, 2, 1]
```

### concat

```javascript
var arr1 = [1, 2, 3];
var arr2 = [4, 5, 6];
console.log(arr1.concat(arr2));//[1, 2, 3, 4, 5, 6]
```

### slice

```javascript
var arr = [1, 2, 3, 4, 5];
console.log(arr.slice(1));//[2, 3, 4, 5]
console.log(arr.slice(1, 3));//[3, 4]
```

### map

```javascript
var arr = [1, 2, 3, 4];
var mapperArr = arr.map(ele => ele * 2);
console.log(mapeedArr);//[2, 4, 6, 8]
console.log(arr)//[1, 2, 3, 4]
```

### reduce

```javascript
var arr = [1, 2, 3, 4];

var result = arr.reduce((previous, current) => previous + current, 0);
console.log(result);//10

var result2 = arr.reduce((first, second) => first + second);
console.log(result2);//10
```

### 过滤

```javascript
var arr = [1, 2, 3, 4, 5, 6];

var filteredAerr = arr.filter(item => item > 4);
console.log(filteredAerr);//[5, 6]
```

### 测试

```javascript
var arr = [1, 2, 3, 4, 5, 6];

var result = arr.every(item => item > 0);
console.log(result);//true,判断是否全部大于0

var resultSome = arr.some(item => item > 7);
console.log(resultSome);//false,判断是否有大于7的元素
```

### 解构操作符(destructuring)

```javascript
var arr = [1, 2, 3];

var [a, b, c] = arr;
console.log(a, b, c);//1 2 3

var [d, e] = arr;
console.log(d, e);//1 2

var [, f] = arr;
console.log(f);//2

function multipleReturns(){
    let name = "峰华";
    let position = "前端工程师";
    
    return [name, position];
}

var [myName, myPosition] = multipleReturns();
console.log(myName, myPosition);//峰华 前端工程师
```

### rest操作符

```javascript
var arr = [1, 2, 3, 4, 5, 6, 7, 8];
var [a, b, ... rest] = arr;
console.log(a, b, rest);//1 2 [3, 4, 5, 6, 7, 8]
var [a, , b, ... rest] = arr;console.log(a, b, rest);//1 3 [4, 5, 6, 7, 8]
variousArgs(...args){    
  console.log(args)
}
variousArgs(1, 2, 3);//[1, 2, 3]
```

### 多维数组

```javascript
var arr = [];
for(let i = 0; i < 5; i++){    
  arr[i] = [];    
  for(let j = 0; j < 4; j++){        
    arr[i][j] = i + j;    
  }
}
```

## 对象

### 创建

```javascript
var employee = {    
  name: "张三",   
  age: 20,    
  position: "前端工程师",   
  signIn: function(){     
    console.log("张三打卡!");  
  }
};
var employee2 = new Object();
employee2.name = "李四";
employee2.signIn = function(){   
  console.log("张三打卡!");
};
```

### 属性

```javascript
var employee = {
    name: "张三",
    age: 20,
    position: "前端工程师",
    signIn: function(){
        console.log("张三打卡!");
    }
};

console.log(employee.name);//张三
console.log(employee["name"]);//张三

employee.name = "王五";
console.log(employee.name);//王五

employee["name"] = "张六";
console.log(employee.name);//张六

console.log(employee.sex);//undefined
```

### 省略key

```javascript
var name = "张三";
var employee = {
    name,
    signIn(){
        console.log("张三打卡!");
    }
};

console.log(employee.name);//张三
employee.signIn();//张三打卡!
```

### 遍历

```javascript
var employee = {
    name: "张三",
    age: 20,
    position: "前端工程师",
    signIn: function(){
        console.log("张三打卡!");
    }
};

console.log(Object.keys(employee));//["name", "age", "position", "signIn"]

for(key in employee){
    console.log(key);
}//name age position signIn
```

### 删除属性

```javascript
var employee = {    
  name: "张三",   
  age: 20, 
  position: "前端工程师",  
  signIn: function(){     
    console.log("张三打卡!");
  }
};
delete empolyee.name;
```

### 构造函数

```javascript
function Employee(name, position){   
  this.name = name; 
  this.position = position;
};
var emp1 = new Employee("峰华", "前端工程师");
console.log(emp1);//Employee{name: "峰华", position: "前端工程师"};
```

### this

```javascript
var employee = {  
  name: "李四", 
  position: "后端工程师", 
  signIn(){    
    console.log(this.name + "上班打卡");   
  }
}
employee.signIn();//李四上班打卡
```

### getters与setters

```javascript
var penson {
    firstName: "三",
    lastName: "张",
    get fullName(){
        return this.lastName + this.lastName;
    },
    set fullName(fullName){
        let [lastName, firstName] = fullName.split(",");
        this.lastName = lastName;
        this.firstName = firstName;
    }
};

console.log(person.fullName);//张三
person.fullName = "李, 四";
console.log(penson.fullName);//李四
console.log(person.lastName, person.firstName);//李 四
```

```javascript
function Employee(name, position){
    this.name = name;
    this.position = position;
}

var emp1 = new Employee("赵六", "前端工程师");

Object.defineProperty(emp1, "info", {
  get: function(){
    return this.name + " " + this.position;
  },
  set: function(info){
    let [name, position] = info.split(" ");
    this.name = name;
    this.position = position;
  }
});

console.log(emp1.info);//赵六 后端工程师
emp1.info = "赵七 后端工程师";
console.log(emp1.name);//赵七
console.log(emp1.position);//后端工程师
```

### Spread操作符

```javascript
var post = {
    id: 1,
    title: "标题1",
    content: "这是内容"
};

var postClone = { ...post }; 

console.log(post === postClone);//False

var post2 = {
    ...post,
    author: "峰华"
};

var arr = [1, 2, 3];
var arrClone = [...arr];

console.log(arrClone);//[1, 2, 3]

var arr2 = [...arr, 4, 5, 6];

console.log(arr2);//[1, 2, 3, 4, 5, 6]

function savePost(id, title, content){
    console.log("保存了文章: ", id, title, content);
}

savePost(...[2, "标题", "内容"]);//保持了文章: 2, "标题", "内容"
```

### Destructuring & Rest

```javascript
var post = {
    id: 1,
    title: "标题1",
    content: "这是内容",
    comments: null
};

var {id, title} = post;

console.log(id, title);//1, "标题"

var {id, title: headline} = post;//冒号取别名

console.log(id, headline);//1, "标题"

var {id, title, comments = "没有评论"} = post;

console.log(comments);//null

var [a, b = 2] = [1];

console.log(a, b);//1, 2

var post2 = {
    id: 2,
    title: "标题2",
    content: "这是内容",
    comments: [
        {
            userId: 1,
            comments: "评论1"
        },
        {
            userId: 2,
            comments: "评论2"
        },
        {
            userId: 3,
            comments: "评论3"
        },
    ]
}

var {
    comments: [, {comments}]
} = post2;

console.log(comments);//评论2

function getId(idKey, obj){
    let {[idKey]: id} = obj;
    return id;
}

console.log(getId("userId", { userId: 3 }));//3

var { comments, ...rest } = post2:

console.log(rest);//{ id: 2, title: "标题2", content: "这是内容" }

function savePostObj({ id, title, content, ...rest}){
    console.log("保存了文章: ", id, title, content);
    console.log(rest);
}

savePostObj({ id: 4, title: "标题4", content: "内容4", author: "峰华" });
//保持了文章: 4, "标题4", "内容4"
//峰华
```

### 原型

```javascript
Employee.prototype.age = 20;
//使用Employee构造的对象都添加了age属性
//同理添加方法
```

### Object.create()

```javascript
const manager = Object.create(emp1);
//manager继承自emp1
//拥有父类的属性和方法,默认值和父类一样
```

### 原型链

null ==> object ==> ...

### 修改原型指向

```javascript
function Manager(){}

Manager.prototype.department = "技术部";

Object.setPrototypeOf(manager, Manager.prototype);

console.log(manager.department);//技术部
```

### call&apply&bind

```javascript
var emo = {
    id: 1,
    name: "峰华",
    /*printInfo(){
        console.log("员工姓名: " + this.name);
    },
    department: {
        name: "技术部",
        printInfo(){
            console.log("部门姓名: " + this.name);
        }
    }*/
};

//emp.printInfo();//员工姓名: 峰华
//emp.department.printInfo();//部门名称: 技术部

function printInfo(dep1, dep2, dep3){
    console.log("员工姓名: " + this.name, dep1, dep2, dep3);
}

printInfo();//员工姓名: 

printInfo.call(emp,"技术部", "IT事业部", "总裁办公室"]);//员工姓名: 峰华 技术部 IT事业部 总裁办公室

printInfo.apply(emp,["技术部", "IT事业部", "总裁办公室"]);//员工姓名: 峰华 技术部 IT事业部 总裁办公室

var empPrintInfo = printInfo.bind(emp,"技术部", "IT事业部", "总裁办公室");//不会立即执行

empPrintInfo();//员工姓名: 峰华 技术部 IT事业部 总裁办公室
```

### class

```javascript
class Employee {
    constructor(name, position){
        this.name = name;
        this.position = position;
    }
    
    signIn(){
        console.log(this.name + "打卡上班");
    }
    
    get info(){
        return this.name + " " + this.position;
    }
    
    set info(info){
        let [name, position] = info.split(" ");
        this.name = name;
        this.position = position;
    }
}

var emp = new Employee("峰华", "前端工程师");

emp.info = "李四 后端";
```

### 继承

```javascript
class Manager extends Employee {
    constructor(name, position, dept){
        super(name, position);
        this.dept = dept;
    }
    
    signIn(){
        super.signIn();
        console.log("额外信息: 经理打卡");
    }
}

var manager = new Manager("王五", "经理", "技术部");

manager.signIn();
//王五打卡上班
//额外信息: 经理打卡
```

## 字符串

### 定义与转义

```javascript
var str = "hello";

var str2= new String("你好");

console.log("\u1010");//unicode
```

### 遍历

```javascript
for(let i = 0; i < str.length; i++){
    console.log(str.charAt(i));
}

for(let c of str){
    console.log(c);
}
```

### 裁切

```javascript
slice("开始索引", "结束索引");//左闭右开

substring("开始索引", "截取长度");
```

### 拼接

推荐用``+``性能更高

### 大小写

```javascript
toUpperCase();//大写
toLocaleLowerCaseCase();//小写
```

### 除空格

```javascript
trim();//去除开头和结尾的空格
```

### 模板字符串

```javascript
`${变量}`;
```

## 正则表达式

### 创建

```javascript
var str = "where when what";

var re = /wh/g;
var re2 = new RegExp("wh");

console.log(re.exec(str));//返回第一次出现的索引,正则表达式加上g全局搜索
console.log(re.test(str));//true
```

### 特殊字符匹配

`.`:`任意字符`

`\d`:`数字` `0-9`单个

`\w`:`字母` `下划线`

`\s`:`空格`

大写相反

### 匹配次数

### 常见正则表达式

`手机号`

```javascript
const mobileRe = /^1[3-9]\d{9}/g;
```

`邮箱`

```javascript
const emailRe = /^([a-zA-Z0-9_\.]+)@([a-zA-Z0-9_\.]+)\.([a-zA-Z]{2,5})$/g;
```

`用户名`

```javascript
var userNameRe = /^[a-zA-Z][a-zA-Z0-9_]{5-14}$/g;
```

## JOSN

```javascript
var postJSON = `{
	"id": 1,
	"title": "标题",
	"comments": [
	 {
		"userId": 1,
		"comment": "评论1"
	 },{
		"userId": 2,
		"comment": "评论2"
	 }
    ],
	"published": true,
	"author": null
}`;

console.log(JSON.parse(postJSON));//转化成对象

var person = {
    id: 1,
    name: "峰华",
    skills: ["React", "Java"]
};

console.log(JSON.stringify(person, null, 2));//转化成JSON
```

## Set

```javascript
var set = new Set();

set.add(1);
set.add(3);
set.add(5);

console.log(set);//{1, 3, 5}

set.add(3);//无法添加,已经有3

console.log(set.has(4));//false

set.delete(3);//删除3

set.clear();//清空

//可以添加对象,不能重复添加指向同一对象的对象

set.forEach(value =>{
    console.log(value);
});
```

## Map

```javascript
var map = new Map();

map.set('name', 'zhwyy');
map.get('name'); //'zhwyy'
map.delete('name');
map.has('name'); //false
map.clear();

map.forEach((value, key) =>{
    console.log(key, value);
});
```

## 参考

> [JavaScript 基础语法教程 | 2020年最新版_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1P741147CT)