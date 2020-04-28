

# MySQL学习笔记——关系型数据库

## 一、如何使用终端操作数据库？

### 1.如何登陆数据库服务器？

```mysql
C:\Users\lenovo>mysql -u root -p
Enter password: *********

Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 18
Server version: 5.7.10-log MySQL Community Server (GPL)

Copyright (c) 2000, 2015, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
```

### 2.如何查询数据库服务器中的所有数据库？

```mysql
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| glp_ld             |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
5 rows in set (0.00 sec)

```

### 3.如何选中某一个数据库进行操作？

```mysql
mysql> select * from admin;
mysql> use test
```

### 4.如何退出数据库服务器

```mysql
mysql> exit;
```

### 5.如何创建库

```mysql
Microsoft Windows [版本 10.0.17763.557]
(c) 2018 Microsoft Corporation。保留所有权利。

C:\Users\0>mysql -uroot -pglp525536
mysql: [Warning] Using a password on the command line interface can be insecure.
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 10
Server version: 8.0.16 MySQL Community Server - GPL

Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> exit
Bye

C:\Users\0>mysql -uroot -p
Enter password: *********
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 11
Server version: 8.0.16 MySQL Community Server - GPL

Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> use test
ERROR 1049 (42000): Unknown database 'test'
mysql> show database
    -> ;
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'database' at line 1
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sakila             |
| sys                |
| world              |
+--------------------+
6 rows in set (0.07 sec)

mysql> use world;
Database changed
mysql> desc world;
ERROR 1146 (42S02): Table 'world.world' doesn't exist
mysql> describe world;
ERROR 1146 (42S02): Table 'world.world' doesn't exist
mysql> show tables;
+-----------------+
| Tables_in_world |
+-----------------+
| city            |
| country         |
| countrylanguage |
+-----------------+
3 rows in set (0.02 sec)

mysql> create database liping0_0 charset utf8;
Query OK, 1 row affected, 1 warning (0.02 sec)

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| liping0_0          |
| mysql              |
| performance_schema |
| sakila             |
| sys                |
| world              |
+--------------------+
7 rows in set (0.00 sec)

mysql> use liping0_0
Database changed
mysql> show tables;
Empty set (0.00 sec)

mysql> dro database liping0_0;
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'dro database liping0_0' at line 1
mysql> drop database liping0_0;
Query OK, 0 rows affected (0.02 sec)

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sakila             |
| sys                |
| world              |
+--------------------+
6 rows in set (0.00 sec)

mysql>
```



#### --主键约束

唯一确定一张表中的一条记录，使得表中的字段不重复且不为空

```mysql
create table user(
	id int primary key,
	name varchar(20)
);

insert into user values(1, '张三')

select * from user
```

#### --联合主键

在多个字段上添加主键约束，多个主键加起来不重复就可以

```mysql
create table user2(
	id int,
	name varchar(20),
	password varchar(20),
	primary key(id,name)
);

insert into user2 values(1,'张三','123');
insert into user2 values(2,'张三','123');
insert into user2 values(1,'李四','123');
```

#### --自增约束

#### --auto_increment，自动管控id

```mysql
create table user3(
id int primary key auto_increment,
    name varchar(20)
);

insert into user3 (name) values('zhangsan')
```

#### --如果忘记创建主键约束

```mysql
create table user4(
	id int,
    name varchar(20)
);

alter table user4 add primary key(id);
```

#### --如何删除？

```mysql
alter table user4 drop primary key;
```

#### -- 使用modify 修改字段，添加约束

```mysql
alter table user4 modify id int primary key;
```

#### -- 唯一约束

约束修饰的字段的值不可以重复

unique(id,name)表示两个组合不重复就行

```
create table user5(
	id int,
	name varchar(20)
);

alter table user5 add unique(name);

desc user5;

insert into user5 values(1,'lisi');

create table user6(
	id int,
	name varchar(20),
	unique(name)
);

desc user6;


create table user7(
	id int,
	name varchar(20) unique
);


create table user8(
	id int,
	name varchar(20)
    unique(id,name)
);

desc user8;

insert into user8 values(1,'lisi');
insert into user8 values(1,'zhangsan');
insert into user8 values(2,'zhangsan');
```

#### --如何删除唯一约束？

```mysql
alter table user7 drop index name;
alter table user7 drop index name;
desc user7;
alter table user7 modigy name varchar(20) unique;
```

#### --总结：

1. 建表的时候添加约束
2. 可以使用alter，add
3. alter，modify方式添加
4. 删除alter...drop...





### --非空约束

修饰的字段不能为空NULL

```mysql
create table user9(
id int,
name varchar(20) not null
);

desc user9;

insert into user9 (id) values(1,'zhangsan');

insert into user9 (name) value('list')

select * from user9;
```

#### --默认约束

当我们插入字段值的时候，如果没有传值，就会使用默认值

```mysql
create table user10(
	id int,
	name varchar(20),
	age int default 10
);

desc user10;

insert into user10(id, name) values(1, 'zhangsan');

select * from user10

insert into user10 value(1,;'zhangsan',19)

```

#### -- 外键约束

涉及到两个表：主表，副表（父表，子表）

```mysql
--班级表
create table classes(
	id int primary key,
    name varchar(20)
);

--学生表
create table students(
	id int primary key,
    name varchar(20),
    calss_id int,
    foreign key(class_id) references classes(id)
);

insert into classes values(1,'一班');
insert into classes values(2,'二班');
insert into classes values(3,'三班');
insert into classes values(4,'四班');

select * from classes;

insert into student values(1001,'张三',1);
insert into student values(1002,'张三',2);
insert into student values(1003,'张三',3);
insert into student values(1004,'张三',4);

insert into student values(1005,'李四',5); --Error

delete from classes where id=4;--Error

--1.主表calsses中没有的数据值，在副表中，是不可使用的。
--2.主表中的记录被副表引用，主表中的这条记录是不可以被删除的。
```



#### --数据库的三大设计范式，sql

#### 1. 第一范式（拆字段）：1NF，数据表中的所有字段都是不可拆分的原子值？

```mysql
create table student(
	id int primary key
	name varchar(20)
	address varchar(30)
);

insert into student2 values values(1,'张三'，’中国四川省成都市武侯区武侯大道100号);
insert into student2 values values(1,'张三'，’中国四川省成都市武侯区武侯大道100号);
insert into student2 values values(1,'张三'，’中国四川省成都市武侯区武侯大道100号);

select * form student2;

--字段值还可以继续拆分的，就不满足第一范式

create table student3(
	id int primary key,
    name varchar(20),
    cuntry varchar(30),
    provence varchar(30),
    details varchar(30)
);

insert into student3 values(1,'张三'，'中国','四川省','成都市','武侯区武侯大道100号');
insert into student3 values(2,'李四'，’中国','四川省','成都市','武侯区京城大道200号');
insert into student3 values(3,'王五'，’中国','四川省','成都市','高新区天府大道90号);
                            
select * from student3;
                            
--范式，设计的越详细，对于某些实际操作可能更好，但不一定都是好处
```



#### 2.第二范式

--必须是满足第一范式的前提下，第二范式要求，除主键外的每一列都必须完全依赖于主键。

--如果要出现不完全依赖，只可能发生在联合主键的情况下

--订单表

```mysql
create table myorder(
	product_id int,
	product_name int,
	product_name varchar(20),
	customer_name varchar(20),
	primary key(product_id, customer_id)
);

--问题，订单表不满足第二范式
--除主键以外的其他列，只依赖于主键的部分字段。
--拆表

create table myorder(
	order_id int primary key,
	product_id int,
	customer_id int,
);

create table product(
	id int primary key,
	name varchar(20)
);

create table customer(
	id int primary key,
	name varchar(20)
);

--分成三个表后，就满足了第二范式的设计！
```

#### 3.第三范式

-- 必须现满足第二范式，除开主键列的其他列之间不能有传递依赖关系。

```mysql
create table myorder(
	order_id int primary key,
	product_id int,
	customer_id int,
    customer_phone varchar(15)
);

# 不满足第三范式
```



```mysql
create table customer(
	id int primary key,
	name varchar(20),
    phone varchar(15)
);
# 满足第三范式
```

## mysql查询练习-数据准备

```mysql
-- 学生表
student
学号
姓名
性别
出生年月日
所在班级
create table student(
	sno varchar(20) primary key,
    sname varchar(20) not null,
    ssex varchar(10) not null,
    sbirthday datetime,
    class varchar(20)
);

```



```mysql
-- 课程表
Course
课程号
课程名称
教师编号
create table course(
    cno varchar(20) primary key,
    cname varchar(20) not null,
    tno varchar(20) not null,
    foreign key(tno) references teacher(tno)
);
```



```mysql
-- 成绩表
Score
学号
课程号
成绩
create table score(
	sno varchar(20) not null,
    cno varchar(20) not null,
    degree decimal,
    foreign key(sno) references student(sno),
    foreign key(cno) references course(cno),
    primary key(sno,cno)
);
```



```mysql
-- 教师表
Teacher
教师编号
教师姓名
教师性别
出生年月日
职称
所在部门
create table teacher(
	tno varchar(20) primary key,
	tname varchar(20),
	tsex varchar(10) not null,
	tbirthday datetime,
	prof varchar(20) not null,
	depart varchar(20) not null
);
```

## 往数据表中添加数据

```mysql
-- 添加学生信息
insert into student values('101','赵一','男','1974-09-01','12345');
insert into student values('102','钱二','男','1987-09-14','14445');
insert into student values('103','孙三','女','1987-07-15','12445');
insert into student values('104','李四','男','1972-06-11','54345');
insert into student values('105','周五','女','1977-09-05','12315');
insert into student values('106','吴六','女','1977-06-21','14345');
insert into student values('107','郑七','男','1977-03-11','11245');
insert into student values('108','王八','男','1977-01-05','12312');
insert into student values('109','冯九','女','1977-09-10','12545');
```

```mysql
-- 添加教师信息
insert into teacher values('101','陈十','女','1958-12-03','教授','计算机系');
insert into teacher values('102','褚十一','男','1962-09-03','副教授','土木系');
insert into teacher values('103','魏十二','男','1956-10-03','副教授','机电系');
insert into teacher values('104','蒋十三','女','1967-12-21','讲师','运输系');
```



```mysql
-- 添加课程信息
insert into course values('101-1','高等数学','101');
insert into course values('102-2','线性代数','102');
insert into course values('103-3','概率论','103');
insert into course values('104-4','工程力学','104');
```



```mysql
-- 添加成绩信息
insert into score values('103','101-1','86');
insert into score values('103','102-2','85');
insert into score values('103','103-3','87');

insert into score values('104','101-1','66');
insert into score values('104','102-2','87');
insert into score values('104','103-3','76');

insert into score values('105','101-1','76');
insert into score values('105','102-2','66');

insert into score values('106','101-1','44');

insert into score values('107','101-1','34');
insert into score values('107','102-2','60');

insert into score values('108','101-1','77');

insert into score values('109','101-1','82');

insert into score values('101','101-1','80');
insert into score values('101','102-2','85');
insert into score values('101','103-3','89');
insert into score values('101','104-4','90');

insert into score values('102','101-1','89');
insert into score values('102','102-2','93');
insert into score values('102','103-3','99');
insert into score values('102','104-4','75');


```

## P29-查询练习-子查询

```mysql
-- 18.查询选修“101-1”课程成绩高于“105”号同学该课程成绩的所有同学记录。
select degree from score where sno='105' and cno='101-1';

select * from score where cno='101-1' and degree>=(select degree from score where sno='105' and cno='101-1');


```

## 

```mysql
-- 添加成绩信息
insert into score values('103','101-1','86');
insert into score values('103','102-2','85');
insert into score values('103','103-3','87');

insert into score values('104','101-1','66');
insert into score values('104','102-2','87');
insert into score values('104','103-3','76');

insert into score values('105','101-1','76');
insert into score values('105','102-2','66');

insert into score values('106','101-1','44');

insert into score values('107','101-1','34');
insert into score values('107','102-2','60');

insert into score values('108','101-1','77');

insert into score values('109','101-1','82');

insert into score values('101','101-1','80');
insert into score values('101','102-2','85');
insert into score values('101','103-3','89');
insert into score values('101','104-4','90');

insert into score values('102','101-1','89');
insert into score values('102','102-2','93');
insert into score values('102','103-3','99');
insert into score values('102','104-4','75');


```

## P30-查询练习-子查询

```mysql
-- 19.查询成绩高于学号为“105”，课程号为“3-105”的成绩的所有记录。

select degree from score where sno='105' and cno='101-1';

select * from score where degree>(select degree from score where sno='105' and cno='101-1');

+-----+-------+--------+
| sno | cno   | degree |
+-----+-------+--------+
| 101 | 101-1 |     80 |
| 101 | 102-2 |     85 |
| 101 | 103-3 |     89 |
| 101 | 104-4 |     90 |
| 102 | 101-1 |     89 |
| 102 | 102-2 |     93 |
| 102 | 103-3 |     99 |
| 103 | 101-1 |     86 |
| 103 | 102-2 |     85 |
| 103 | 103-3 |     87 |
| 104 | 102-2 |     87 |
| 108 | 101-1 |     77 |
| 109 | 101-1 |     82 |
+-----+-------+--------+
```

## P31-查询练习-year函数与带in关键字的子查询

```mysql
-- 20. 查询和学号为108、101的同学同年出生的所有学生的sno、sname和sbirthday
select * from student where sno in (108,101);
select  year(sbirthday) from student where sno in (108,101);

select * from student where year(sbirthday) in (select  year(sbirthday) from student where sno in (108,101));
+-----+-------+------+---------------------+-------+
| sno | sname | ssex | sbirthday           | class |
+-----+-------+------+---------------------+-------+
| 101 | 赵一  | 男   | 1974-09-01 00:00:00 | 12345 |
| 105 | 周五  | 女   | 1977-09-05 00:00:00 | 12315 |
| 106 | 吴六  | 女   | 1977-06-21 00:00:00 | 14345 |
| 107 | 郑七  | 男   | 1977-03-11 00:00:00 | 11245 |
| 108 | 王八  | 男   | 1977-01-05 00:00:00 | 12312 |
| 109 | 冯九  | 女   | 1977-09-10 00:00:00 | 12545 |
+-----+-------+------+---------------------+-------+
```

## P32-查询练习-多层嵌套子查询

```mysql
-- 21.查询“陈十”教师任课的学生成绩
select * from teacher;
select cno from course where tno=(select tno from teacher where tname='陈十');
select * from score where cno=(select cno from course where tno=(select tno from teacher where tname='陈十'));
```

## P33-查询练习-多表查询

```mysql
-- 22.查询选修某课程的同学多于5人的教师姓名

select * from score;
select cno from score group by cno having count(*)>5;
select tno from course where cno in (select cno from score group by cno having count(*)>5);
select tname from teacher where tno in (select tno from course where cno in (select cno from score group by cno having count(*)>5));

```

## P34-查询练习-in表示或者关系

```mysql
-- 23.查询12312班和12345班全体同学的记录。
select * from student;
select * from student group by class;
select * from student where class in (12345, 14445);
```

## P35-查询练习-where条件查询

```mysql
-- 24.查询存在有85分以上成绩的课程cno。
select * from score where degree>85;
```

## P36-查询练习-子查询

```mysql
-- 25.查询出“计算机系”教师所教课程的成绩表。
select tno from teacher where depart="计算机系";
select cno from course where tno=(select tno from teacher where depart="计算机系");
select * from score where cno=(select cno from course where tno=(select tno from teacher where depart="计算机系"));
```

## P37-查询练习-union的使用

```mysql
-- 24.查询“计算机系”与“电子工程系”不同职称的教师的tname与prof。
-- union求并集
select * from teacher;
select prof from teacher where dapart="计算机系";
select tname,prof from teacher where depart="计算机系" and prof not in("机电系")
union
select tname,prof from teacher where depart="机电系" and prof not in("计算机系");
```

## 

## 二、如何在可视化工具操作数据库?

## 三、如何在编程语言中操作数据库？

