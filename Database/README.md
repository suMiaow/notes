# Database

数据的管理主要经历过3个阶段

1. 人工管理阶段：安全性 数据的持久保存
2. 文件系统阶段：真正意义上实现了数据的永久保存，安全性，数据的并发访问，不方便统计和查找查找库存大于100的记录
3. 数据库系统阶段：真正解决了数据的持久性保存，安全性，并发访问的问题

## 数据库

管理数据的仓库。

虽然我们可以使用文件来保存数据，但是当数据量很大的时候，使用文件就非常难以管理，但是使用数据库可以很迅速的完成对数据的增、删、改、查等操作

## Relational database（关系型数据库）

关系型数据库使用表来存放数据，
使用表和表之间的关系来描述和查找数据

* 关系：一张二维表，关系必须有关系名
* 字段/属性：二维表中的一列，字段必须有字段名
* 记录/元祖：二维表中的一行

SQL term | Relational database term | Description
--- | ---| ---
*Row* | *Tuple* or *record* | A data set representing a single item
*Column* | *Attribute* or *field* | A labeled element of a tuple, e.g. "Address" or "Date of birth"
*Table* | *Relation* or *Base relvar* | A set of tuples sharing the same attributes; a set of columns and rows
*View* or *result set* | *Derived relvar* | Any set of tuples; a data report from the RDBMS in response to a query

![Relational database terminology](Relational_database_terms.svg)

* super key（超键）：在关系中能唯一标识元祖的属性集
* candidate key（候选键）：不含有多余属性的超键
* primary key（主键）：用户选作作为元祖唯一标识的候选键
* foreign key（外键）：用于关键两个表的属性

## Database normalization

范式：符合某一种级别的规范，构造数据库表必须遵循一定的范式

 第一范式 1NF：
  数据库表中的每一列都是不可分割的数据项，同一列中不能有多个值

 第二范式 2NF：必须得有主键
  在满足1NF的基础上，
  数据库表中的每个实例都可以被唯一的区分。
  非主键字段要完全依赖于主键字段。

 第三范式 3NF：
  满足2NF的基础上，
  数据库表中不包含在其他表中已包含的非主关键字字段

## SQL

Structured query Language（结构化查询语言）：具有对数据库操作的所有功能

优点：

不是某个数据库的特定语言，几乎所有的数据库都使用SQL语言进行操作

非过程化语言，只要告诉它“做什么”，而不需要指定“怎么做”

1. DDL：数据定义语言

   建库、建表、修改库、修改表....

2. DML：数据操纵语言

   对数据的增、删、改、查

3. DCL：数据控制语言

   创建用户、赋予权限

## MYSQL

mysql.com

mysql -u root -p
123456
