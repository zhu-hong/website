---
title: "关系型数据库基础"
date: 2020-05-09T13:29:02+08:00
draft: true
categories: [Database]
---

## 数据库(Database)

数据库是按照数据结构来组织,存储和管理数据的仓库

数据库是长期储存在计算机内的,有组织的,可共享的数据集合

### 特点

1. 尽可能小的冗余度
2. 较高的数据独立性
3. 易拓展性
4. 一定范围内多个用户共享

## Engine(存储引擎)

### MyISAM

默认使用,查询速度快,较好的索引优化,数据压缩技术

不支持事务管理

### InnoDB

支持事务管理,可以选用不同的存储引擎

## 数据库访问技术

|  ODBC   | Microsoft |
| :-----: | :-------: |
| OLE-DB  | Microsoft |
|   ADO   | Microsoft |
| ADO.NET | Microsoft |
|  JDBC   | JAVA API  |
|  ODAC   |  Oracle   |

## 关系数据库结构模型

数据结构模型为二元关系(表格形式)

## DB

数据库	database

## DBMS

数据库管理系统 database management system

## SQL

结构化查询语言

## 数据类型

### 字符串类型

#### char(n)

0-255	占n个字节,不足用空格补

#### Varchar(n)

0-65535	占实际字节+1个字节

### 数值类型

#### int

只能存放整数,不用定义长度,若定义了数值,是指显示的宽度

#### float(M, N)

M指总位数,N指小数位数

若超长则四舍五入

### 时间类型

#### date

2020-02-18	年月日

#### time

15:42:32	时分秒

#### datetime

 2020-02-18 15:42:32

### 大数据类型

#### blob

二进制字符串类型,声音视频图片

#### text

非二进制字符串类型,字符型字符串,如文章

## 基础语句

### 打开服务

```sql
net start mysql;
```

### 创建库

```sql
create database 库名;
```

### 查看位于哪个库

```sql
select database();
```

### 打开某数据库

```sql
use 数据库名;
```

### 查看某库的表

```sql
show tables from 库名;
```

### 看表的结构

```sql
desc 表名;
```

## 增删改查

### 创建表

```sql
create table 数据库表名(
	字段名 数据类型 not null,
    字段名 数据类型 not null,
    字段名 数据类型 null
);
```

### 查询表的所有数据

```sql
select * from 表名;
```

### 插入数据

```sql
insert into 表名 (列名1, 列名2) values(插入到列名1的数据, 插入到列名2的数据);
```

### 修改数据

```sql
update 表名 set 列名=要改的数据 where 对应数据的列名=对应的数据;
```

### 删除行

```sql
delete from 表名 where 列名=列名对应的数据;
```

### 增加列(默认添加在最后)

```sql
alter table 表名
	add 列名 数据类型 not null;
```

### 将列添加在指定位置

```sql
alter table 表名
	add 列名 列定义 (after 列名)/(first);
```

### 删除列

```sql
alter table 表名 
	drop 列名;
```

### 更改列名

```sql
alter table 表名
	change 旧列名 新列名 新列定义;
```

### 更改列类型

```sql
alter table 表名
	modify 列名 列定义;
```

### 查询表中的字段

```sql
select 字段名,字段名,字段名 from 表名;
```

## 起别名

```sql
select 字段 as 别名 from 表名;
select 字段 别名 from 表名;
//别名中有特殊符号需用双引号包裹别名
```

## 去重

```sql
select distinct 列名 from 表名;
```

## CONCAT函数

```sql
select concat(last_name,first_name) 姓名 from employees;
```

## IFNULL函数

```sql
ifnull(列名,0)
//如果列名的值为null,输出0
```

## ISNULL函数

```sql
isnull(列名)
//判断是否为null
```

## 条件查询

```sql
select 列名 from 表名 where 条件语句;
```

## 引入数据库文件

source path;

## 模糊查询

### like

```mysql
#查询员工名中带有a的员工的信息
select * from employees where last_name like '%a%';	//%在这里相当于通配符,表示任意多个字符
#查询员工名中第三个字符为e,第五个字符为a的员工的信息
select * from employees where last_name like '__e_a%';//_表示任意单个字符
#查询员工名中第二个字符为_的员工的信息
select * from employees where last_name like '_\_%';
select * from employees where last_name like '_$_%' escape '$';//escape关键字,规定转义字符
```

#### escape

规定转义字符

### between	and

```mysql
#查询员工编号在100到120之间的员工信息
select * from employees where employee_id between 100 and 120;//包含连接值
```

### in

```mysql
#查询员工的工种编号是 IT_PROG,AD_VP,AD_PRES,中的一个的员工信息
select * from employees where job_id in('IT_PROG','AD_VP','AD_PRES');//类型要统一,比支持通配符
```

## is	null

```mysql
#查询没有奖金的员工
select * from employees where commission_pct is null;
#查询有奖金的员工
select * from employees where commission_pct is not null;
```

= ,<> 不能判断null值

## 安全等于	<=>

```mysql
#查询没有奖金的员工
select * from employees where commission_pct <=> null;
```

## 排序查询

```mysql
select 字段名 from 表名 order by 字段名 asc(升序)|desc(降序));//默认升序
#加上条件排序查询
select * from employees where department_id > 90 order by hiredate asc;
#按年薪的高低显示员工信息
select *,salary*12*(1+ifnull(commission_pct,0)) 年薪 from employees order by 年薪;
#按姓名长度排序
select last_name from employees order by length(last_name);//length函数
#先按工资排序再按员工编号排序
select * from employees order by salary,department_id;
```

### length函数

返回参数的长度

## LIMIT关键字

## 常见函数

### 单行函数

#### 字符函数

##### length

```mysql
length();
#获取参数的字节长度
```

##### concat

```mysql
concat(,);
#拼接字符串
```

##### upper/lower

```mysql
upper();
#变大写
lower();
#变小写
```

##### substr

```mysql
substr();
#截取字节,两个或多个参数,两个参数时一个为截取对象,另一个为开始截取的索引
#三个参数时一个为截取对象,一个为开始截取的索引,一个截取的长度
#MySQL中索引从1开始
```

##### instr

```mysql
instr('abck','c');//返回3
#返回第二个参数在第一个参数首次出现的索引
```

##### trim

```mysql
trim();
#去空格
trim(参数一 from 参数二);
#去掉参数二前后的参数一
```

##### lpad

```mysql
lpad(参数一,参数二,参数三);
#用参数三在参数一前面将参数一填充到参数二的长度
```

##### rpad

```mysql
rpad(参数一,参数二,参数三);
#用参数三在参数一后面将参数一填充到参数二的长度
```

##### replace

```mysql
replace(参数一,参数二,参数三);
#将参数一中的参数二用参数三替换
```

### 数学函数

#### round

```mysql
round(参数);#四舍五入
round(-1.55);#-2
round(1.567, 2);#1.57
```

#### ceil

```mysql
ceil(参数);#向上取整
ceil(-1.02);#-1
```

#### floor

```mysql
floor(参数);#向下取整
```

#### truncate

```mysql
truncate(参数,参数);#截断
truncate(1.69999, 1);#1.6
```

#### mod

```mysql
mod(参数,参数);#求余
mod(-10, -3);#-1
```

# 注意事项

`貌似只接受单引号`

路径使用`/`

数据类型:

+ serial     自动增长整形
+ integer   整型
+ real        浮点型

# 库/表

## 创建

```sql
create database basename;
```

```sql
create table tableName(
	ColumnName ColumnType Constraints,
    ...
);
```

## 约束

### 主键

``自带 NOT NULL && UNIQUE``

```sql
create table tableName(
	ID INT PRIMARY KEY,
    [CONSTRAINT <约束名>] PRIMARY KEY(ID)
);

# 添加
ALTER TABLE tableName ADD CONSTRAINT PK_ID PRIMARY KEY(ColumnName);

# 删除
ALTER TABLE tableName DROP CONSTRAINT PK_ID;
```

### 外键

```sql
create table tableName(
	ID INT REFERENCES tableName(ColumnName),
    CONSTRAINT FK_ID FOREIGN KEY ColumnName REFERENCES tableName(ColumnName)
);

# 添加
ALTER TABLE tableName ADD CONSTRAINT FK_ID FOREIGN KEY (ColumnName) REFERENCES tableName(ColumnName);

# 删除
ALTER TABLE tableName DROP CONSTRAINT FK_ID;
```

### CHECK

...↥

### EXCLUSION

略

### AUTO INCREMENT

使用`serial`数据类型

## 修改表

### 改表名

```sql
alter table table_name rename to new_name;
```

### 添加列

```sql
alter table table_name add column_name datatype;
```

### 删除列

```sql
alter table table_name drop column column_name;
```

### 改字段名

```sql
alter table table_name rename Column-name to new_name;
```

### 改字段类型

```sql
alter table table_name alter column column_name type new_type;
```