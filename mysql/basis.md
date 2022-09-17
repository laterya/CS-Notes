# Mysql基础语句

## DDL语句

数据定义语言 ，这些语句定义了不同的数据段，数据库，表，列，索引等数据库对象的定义。常用的语句关键字主要包括：create，drop，alter，rename，truncate

```mysql
# DDL
# 数据库操作
# 查询所有数据库
show databases;
# 查询当前数据库
select database();
# 创建
create database if not exists student;
# 删除
drop database if exists student;
# 使用
use student;

# 表操作
# 查询当前数据库所有表
show tables;
# 查询表结构 desc 表名
desc class1;
# 查询指定表的建表语句 show create table 表名
show create table class1;
# 创建表
create table class1(
    id int comment '编号',
    name varchar(50) comment '姓名',
    gender varchar(1) comment '性别'
) comment '班级1';
# 添加字段
alter table class1 add age int comment '年龄';
# 修改数据类型
alter table class1 modify name varchar(10);
# 修改字段名和字段类型
alter table class1 change gender genders int;
# 删除字段
alter table class1 drop genders;
# 修改表名
alter table class1 rename to students;
# 删除表
drop table if exists students;
# 删除指定表，并重新创建该表
truncate table class1;
```

## DML语句

数据操纵语句 ，用于添加，删除，更新和查询数据库记录，并检查数据完整性，常用的语句关键字有：insert，delete，update等等

```mysql
# DML 对数据库中表的数据记录进行增删改

# 给指定字段添加数据
insert into class1(id, name, gender) values (2,'小黄','女');
# 给全部字段添加数据
insert into class1 values(1,'小明','男');
# 批量添加
insert into class1 values(1,'小明','男'),(2,'小黄','女');

# 修改
update class1 set id = 9999 where id = 1;

# 删除
delete from class1 where id = 2;
```

## DQL语句

数据查询语句 ，用于从一个或多个表中检索信息。主要的语句关键字包括：select

```mysql
# DQL 数据查询语言

# 基本查询
# 查询多个字段
select * from class1;
# 设置别名
select id as qw from class1;
# 去除重复记录
select distinct id from class1;

# 条件查询
select * from class1 where id>1 && gender = '男';

# 聚合函数 将一列数据作为整体，进行纵向计算
select count(name) from class1;
select max(id) from class1;
select avg(id) from class1;
select sum(id) from class1;

# 分组查询
select * from class1 where id >= 1 group by gender having gender = '女';
# 排序查询
select * from class1 order by id desc;
select * from class1 order by id asc;
# 分页查询
select * from class1 limit 1,2;
```

## DCL语句

数据控制语句 ，用于控制不同数据段直接的许可和访问级别的语句。这些语句定义了数据库，表，字段，用户的访问权限和安全级别。主要的语句的关键字包括：grant，revoke等等

```mysql
# DCL 用来管理数据库 用户，控制数据库访问权限
# 创建用户
 create user 'laterya'@'localhost' identified by '123456';
 create user 'laterya'@'%' identified by '123456';
# 修改用户密码
alter user 'laterya'@'localhost' identified with mysql_native_password by '1234';
# 删除用户
drop user 'laterya'@'%';
drop user 'laterya'@'localhost';

# 权限控制
# 查询权限
show grants for 'laterya'@'localhost';
# 授予权限
grant select on student.class1 to 'laterya'@'localhost';
grant all on student.* to 'laterya'@'localhost';
# 撤销权限
revoke all on student.* from 'laterya'@'localhost';
```

