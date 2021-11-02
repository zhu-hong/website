---
title: "js-pink"
date: 2020-02-13T12:58:47+08:00
draft: true
categories: [JavaScript]
---

## ECMA

### 变量

#### 声明多个变量

```javascript
var age = 18,
		name = '王艳艳',
    height = 155;
```

#### 字符用单引号

```JavaScript
var name = 'iu收到回复';
```

#### 可以不声明直接使用(不提倡使用)

```javascript
age = 18;
name = '王艳艳';
```

### 弹出输入框

#### prompt

```javascript
promit('xxxx');
```

获取来的数据默认是字符串类型

### 弹出警示框

#### alert

```javascript
alert();
```

### 判断数据类型

```javascript
typeof active;
```

### 数据类型转换

#### 转换为字符串型

```javascript
var nmu = 10;
1. num.toString();
2. String(num);
3. num + '';	(隐式转换) // 任何值加上字符串类型都会变为字符串类型// 
```

#### 转换为数字型

```javascript
var age = prompt('请输入您的年龄') //字符串类型
1. parseInt(age);//整型
   parseInt('120px');
2. parseFloat(age);//浮点型
3. Number(age);
4. age 加减乘除 一个数字;//隐式转换
```

```javascript
var year = prompt('请输入你的出生年份');
var age = 2018 - year;
alert('您今年' + age + '岁了');
```

#### 转换为布尔型

```javascript
Boolean();
false
   Boolean('');
   Boolean(0);
   Boolean(NaN);
   Boolean(null);
   Boolean(undefined);
其他皆为 true   
```

#### 案例

```javascript
var name = prompt('输入名字');
var age = prompt('输入年龄');
var mann = prompt('输入性别');
alert('名字是' + name + '.\n' + '年龄是' + age + '.\n' + '性别是' + mann + '.');
```

### 运算符

#### 求余

%

#### 浮点型精度问题

0.1+0.1!=0.2

#### 表达式与返回值

3=1+2

#### 前后置递增递减运算符区别

后置先返回原值再自加

#### 比较运算符

+ ＝

1. 赋值

+ ==

```javascript
18 == '18'	//true
```

1. 判断

2. 转型

+ ===

1. 全等

```javascript
18 ==== '18'	//false
```

#### 逻辑运算符

&&	与	都真为真

||	或	一真为真

###### 短路运算(逻辑中断)

```javascript
123 && 456	// 456 该项为真返回下一项

0 && 123	// 0 该项为假返回该项

123 || 456	// 456 该项为假返回该项

0 || 123	// 123 该项为真返回下一项
```

#### 赋值运算符

```javascript
num +=2;

num = num + 2;
```

#### 三元表达式

```javascript
条件表达式 ? 表达式1 : 表达式2

// 真	返回表达式1

// 假	返回表达式2
```

### 断点测试

开发者工具	=>	Sources

点击断点处刷新	=>	F11

### while

```java
while(条件表达式){
	循环体
}
// 当符合条件表达式的时候执行循环体
```

### do while

```javascript
do{
	循环体
}while(条件表达式)
// 先执行一次循环体
```

### 数组

#### 创建数组

```javascript
var 数组名 = new Array();
```

```javascript
var 数组名 = [1,2,'王艳艳',true];
```

#### 数组长度

```javascript
数组名.length
```

#### 数组转字符串

#### 新增数组元素

1. 修改length长度

   默认值是没定义

2. 修改索引号,追加数组元素

### 函数

一段可重复执行调用的代码块

#### return

return返回多个值时只返回最后一个值

#### arguments

伪数组

接收所有参数

1. 具有数组的length特性
2. 按照索引方式进行存储
3. 没有真正数组的一些特性
   1. pop()
   2. push()

###匿名函数(函数表达式)

```javascript
var fun = function(){
    console.log('我是函数表达式');
}
fun();
```

不属于函数声明,相当于一个赋值过程;

### 作用域链

往上就近原则

### 预解析

#### 变量提升

只提升不赋值

#### 函数提升

### 对象

#### 创建对象的三种方式

##### 对象字面量

```javascript
var obj(对象名) ={
    name(属性名) : '朱鸿',
    age : 19,
    sex : '男',
    sayHi(方法名) : function(){
         cnosole.log('hi~');
     }
}
```

##### new object

```javascript
var obj = new Object();
obj.name = '朱鸿';
```

##### 构造函数

```javascript
function 构造函数名(){
    this.属性名 = 值;
    this.方法 = function(){}
}

function Star(name, age, sex){
    this.name = name;
    this.age = age;
    this.sex = sex;
}
var ldh = new Star('刘德华', 18, '男');

// 构造函数名的首字母大写

// 称为对象的实例化
```

#### 调用对象的属性

1. 对象名.属性名
2. 对象名['属性名']

#### 调用对象的方法

+ 对象名.方法名();

#### 遍历对象

```javascript
var obj ={
    name : '朱鸿',
    age : 19,
    sex : '男',
    sayHi : function(){
         cnosole.log('hi~');
     }
}

for (var k in obj){
    console.log(k);		得到属性名
    console.log(obj[k]);	得到属性值
}
```

#### 内置对象

##### Math对象

```javascript
Math.abs();	// 取绝对值

Math.floor();	// 向下取整

Math.ceil();	// 向上取整

Math.round();	// 四舍五入取整

Math.random();	// 随机返回一个0 <= x < 1的数
```

##### 日期对象(Date())

```javascript
var date = new Date();
date.getFullYear()
date.getMonth()//实际月份要加一
date.getDate()
date.getDay()//0为周日
```

###### 时间戳

##### 数组对象

###### 添加数组元素

```javascript
var arr = [1,2,3];
arr.push(4, 'pink');
//返回值是数组长度,新元素加在数组后面
arr.unshift(4,'red');
//新元素加在数组前面
```

###### 删除数组元素

```javascript
var arr = [1,2,3];
arr.pop();
//返回值是删除的那个元素,删除数组最后一个元素
arr.shift();
//返回值是删除的那个元素,删除数组第一个元素
```

###### 翻转数组

arr.reverse();

###### 数组排序

arr.sort();

###### 数组索引方法

```javascript
var arr = [33,53,53,9];
arr.indexOf(53);//返回第一个
arr.lastIndexOf(53);//返回最后一个

// 如果没有返回-1
```

###### 数组去重

```javascript
var arr = [33, 55, 55, 34, 7, 89,7, 3];
var newArr=[];
for(var i =0; i < arr.length; i++){
    if(newArr.indexOf(arr[i])){
        newArr.push(arr[i]);
    }
}
```

###### 数组转换为字符串

```javascript
var arr = [1, 2, 3];
arr.toString()
arr.join('&')//分隔符

// concat() splice()
```

##### 字符串对象

###### 字符串不可变

字符串变量重新赋值,变量只是换了指向对象,之前的值还在占内存

###### 设定查找起始位置

```JavaScript
str.indexOf('春',3)
```

###### 根据位置返回字符

```javascript
str.charAt(索引号)//返回字符
str.charCodeAt(索引号)//返回ASCII码
```

###### 截取字符串

```javascript
var str = '改革春风吹满地';
str.substr(2, 2)//春风
```

### 数据类型内存分配

#### 堆和栈

复杂数据类型在栈里面存放一个地址,地址指向存放在堆里的数据

## DOM

### 简介

Docume Object Model(文档对象模型)

W3C推荐的处理可拓展标记语言的标准编程接口

#### DOM树

文档:Document	元素:element	节点:node

### 获取元素

#### ID选择器

```javascript
const active = document.getElementById('ID名');
console.dir(active);
```

#### 标签名

```javascript
const actives = document.getElementByTagName('标签名');
//获取的是一个集合[伪数组]
```

#### 类名

```javascript
const actives = document.getElementByClassName('类名');
```

#### 通用选择器

```javascript
const active = document.querySelecto('');
```

```javascript
const actives = document.querySelectoAll('');
```

### 事件基础

####  事件三要素

1. 事件源
2. 事件类型
3. 事件处理程序

```javascript
btn.onclick = function(){
    this.style = 'width="100px";';
}
//this指的是函数调用者,如上面为btn
```

### 修改元素内容

```javascript
const text = document.querySelector('p');
text.innerHTML = '<strong>今年是:2020</strong>';
```

### 自定义属性

```javascript
//获取
document.getAttribute("");
//修改属性值
Element.setAtrribute('属性','值');
//移除
Element.removeAtrribute('');
```

### 阻止链接跳转

```html
<a href = "javascript:;"></a>
```

### 节点

#### 父节点

```javascript
Element.parentNode
```

#### 子节点

```javascript
Element.childNodes//所有子节点,包含元素节点,文本节点等.Element.children//所有子元素节点
```

#### First And Last

```javascript
parentNode.firstChild//第一个子节点
parentNode.firstElementChild//第一个子元素节点(兼容性问题)
parentNode.children[0]//实际开发当中的用法
parentNode.lastChild//最后一个子节点
parentNode.lastElementChild//最后一个子元素节点(兼容性问题)
parentNode.children[parentNode.children.length - 1]//实际开发当中的用法
```

#### 兄弟节点

```javascript
node.nextSibling//下一个兄弟节点,可能是文本节点
node.nextElementSibling//下一个兄弟元素节点(兼容性问题)
node.previonsSibling//上一个兄弟节点,可能是文本节点
node.previousElementSibling//上一个兄弟元素节点(兼容性问题)
```

#### 创建与添加

```javascript
var li = Document.createElement('li');//创建节点
var ul = Document.querySelect('ul');ul.appendChild(li);//添加在最后面
ul.insertBefore(li,指定元素);//添加在指定元素前面
```

#### 删除节点

```javascript
parentNode.removeChild(chlidNode);//删除父节点中的某子节点
```

#### 复制节点

```javascript
node.cloneNode(false);//浅拷贝,只复制标签不复制内容node.cloneNode(true);//复制标签和内容
```

### 文档流

#### 捕获阶段

自顶向下

```js
someNode.addEventListener("Event",Func,false);
```

#### 冒泡阶段

自下而上``实际开发中常用``

```js
someNode.addEventListener("Event",Func,true/null);
```

#### 事件对象

```js
someNode.addEventListener("Event",(e)=>{    
  e.target;//触发事件的对象    
  e.type;//事件类型    
  e.preventDefault();//阻止默认行为    
  e.stopPropagation();//阻止事件冒泡
});
```

#### 事件委托

给父节点添加监听事件,作用在每个子节点上

#### 事件拓展

``contextmenu``右键菜单事件

``selectstart``开始选中事件

#### 鼠标事件对象

`鼠标位置`

```javascript
document.addEventListener("click",(e)=>{    
  e.clientX/Y;//返回鼠标相对于浏览器窗口可视区的X/Y坐标    
  e.pageX/y;//返回鼠标先对于文档页面的X/Y坐标    
  e.screenX/Y;//返回鼠标先对于电脑屏幕的X/Y坐标
});
```

Mouseover与Mouseenter的区别

+ mouseover经过子元素也会触发，mouseenter不会，因为不会有冒泡，搭配mouseleave使用

#### 键盘对象

1. `keydown` `keypress`

   一直按着一直触发

2. 执行顺序

`keydown` -----> `keypress`-----> `keyup`

3. `krydown` `keyup`事件不区分大小写

##### 识别按下的是哪个键

```javascript
document.addEventListener("keyup",(e)=>{    
  e.keyCode;//ASCII码值
})
```

## BOM

### 简介

`BOM` `Browser Object Model` `浏览器对象模型`

+ BOM比DOM大

+ window对象是浏览器的顶级对象
  + 它是JS访问浏览器窗口的一个接口
  + 它是一个全局对象,定义在全局作用域中的变量,函数都会变成他的属性和方法

### window对象的常见事件

#### 加载事件

```js
window.onload = function(){}//页面全部加载完毕再执行
window.addEventListener("load",function(){})
```

#### 调整窗口大小事件

```js
window.addEventListener("resise",function(){})
window.innerWidth;//浏览器窗口当前宽度
```

### 同步异步

+ 事件循环

> 主线程执行栈
>
> 异步进程处理
>
> > 消息队列

### Location对象

#### 属性

+ href:获取或设置整个URL
+ host:返回域名
+ port:返回端口号,如果未写返回空字符串,默认为`80`
+ pathname:返回路径
+ search:返回参数`?`
+ fragment:片段`#

## 参考

> [黑马程序员JavaScript全套教程，Web前端必学的JS教程，零基础入门JavaScript_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1ux411d75J)
>
> [黑马程序员JavaScript核心教程，前端基础教程，JS必会的DOM BOM操作_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1k4411w7sV)