---
title: sql笔记
date: 2023-05-01 23:46:28
tags:
- sql
- 数据库
categories:
- 数据库
---


## 基本指令

* 创建数据库

  ```sql
  create database dbname
  ```

* 创建表

  ```sql
  create table tablename(
  deptNo int primary key,  #primary key为主键，主键的值是唯一的
  deptName varchar(32)
  )
  ```

* 创建外码约束

  ```sql
  #表级外码约束可以直接在创建表的时候定义，一般开头为FK_
  create table dept(
  deptNo int primary key,
  deptName varchar(32)
  );
  
  create table staff(
  staffNo INT primary key, 
  staffName varchar(32),
  gender char(1),
  dob date,
  Salary numeric(8,2),
  deptNo int,
  #定义外码约束
  constraint FK_staff_deptNo foreign key(deptNo) references dept(deptNo)
  );
  ```

* 创建CHECK约束

  只有在CHECK的表达式为TRUE时数据插入才有效

  ```sql
  #当需要命名约束的名称时
  CREATE TABLE Persons
  (
  P_Id int NOT NULL,
  LastName varchar(255) NOT NULL,
  FirstName varchar(255),
  Address varchar(255),
  City varchar(255),
  CONSTRAINT chk_Person CHECK (P_Id>0 AND City='Sandnes')
  );
  #当表已经创建后，需要添加约束时
  ALTER TABLE Persons
  ADD CONSTRAINT chk_Person CHECK (P_Id>0 AND City='Sandnes')
  ```

* 创建default约束

  default约束用于向列中加入默认值

  ```sql
  CREATE TABLE Persons
  (
      P_Id int NOT NULL,
      LastName varchar(255) NOT NULL,
      FirstName varchar(255),
      Address varchar(255),
      City varchar(255) DEFAULT 'Sandnes'
  )
  
  #default也可以使用函数插入系统值
  CREATE TABLE Orders
  (
      O_Id int NOT NULL,
      OrderNo int NOT NULL,
      P_Id int,
      OrderDate date DEFAULT GETDATE()
  )
  
  #创建表后加入default约束
  ALTER TABLE Persons
  ADD CONSTRAINT ab_c DEFAULT 'SANDNES' for City
  
  
  #撤销default
  ALTER TABLE Persons
  ALTER COLUMN City DROP DEFAULT
  ```


* UNIQUE约束

  UNIQUE约束和主键约束类似，但是可以允许为NULL。

  ```sql
  create table department(
    dno char(10) primary key,
    dname varchar(32) NOT NULL UNIQUE
  );
  ```

  

* 插入数据

  ```sql
  INSERT INTO table_name
  VALUES (value1,value2,value3,...);
  
  #也可以指定插入的列
  INSERT INTO table_name (column1,column2,column3,...)
  VALUES (value1,value2,value3,...);
  
  #如需命名 UNIQUE 约束，并定义多个列的 UNIQUE 约束，请使用下面的 SQL 语法
  CREATE TABLE Persons
  (
  P_Id int NOT NULL,
  LastName varchar(255) NOT NULL,
  FirstName varchar(255),
  Address varchar(255),
  City varchar(255),
  CONSTRAINT uc_PersonID UNIQUE (P_Id,LastName)
  );
  
  #当表已被创建时，如需在 "P_Id" 列创建 UNIQUE 约束，请使用下面的 SQL:
  ALTER TABLE Persons
  ADD CONSTRAINT uc_PersonID UNIQUE (P_Id,LastName)
  
  #如需撤销 UNIQUE 约束，请使用下面的 SQL：
  ALTER TABLE Persons
  DROP CONSTRAINT uc_PersonID
  ```

* 删除表

  ```sql
  drop table table_name
  ```

* 更改表的内容

  ```sql
  alter table table_name
  
    ADD [COLUMN] 列名 数据类型 [列约束]
         [FIRST | AFTER col_name]
    | ADD {INDEX|KEY} [索引名] [类型] (列1,...) 
    | ADD [CONSTRAINT [约束名]] 主码约束
    | ADD [CONSTRAINT [约束名]] UNIQUE约束
    | ADD [CONSTRAINT [约束名]] 外码约束
    | ADD [CONSTRAINT [约束名]] CHECK约束
    | DROP {CHECK|CONSTRAINT} 约束名
    | ALTER [COLUMN] 列名 {SET DEFAULT {常量 | (表达式)} | DROP DEFAULT}
    | CHANGE [COLUMN] 列名 新列名 数据类型 [列约束]
          [FIRST | AFTER col_name]
    | DROP [COLUMN] 列名
    | DROP {INDEX|KEY} 索引名
    | DROP PRIMARY KEY
    | DROP FOREIGN KEY fk_symbol
    | MODIFY [COLUMN] 列名 数据类型 [列约束]
          [FIRST | AFTER col_name]
    | RENAME COLUMN 列名 TO 新列名
    | RENAME {INDEX|KEY} 索引名 TO 新索引名
    | RENAME [TO] 新表名
  ```


* sql-service中更改表名使用 exec sp_rename oldname,nername; 语句





### 跨表查询

* select * from t_1,t_2 where()

### group by 

 group by 可以将表格按照某些元素聚集起来。如可以将表单中id号相同的人聚集起来，这样方便对某一元素进行集中操作。如查询某一id下所有资产的总和就可以用group by

### order by 

order by 可以让数据集按照某个数据的大小进行排序 如果要降序则在最后加上desc

### in() 

in()可以用在筛选数据的where语句中，表示判断某个字段是否在某个数据集中，与之相反的有not in()

### limit a offset b

limit表示将查询的记录分页，每页a条记录，b表示显示第几页的数据

### rank() over(...)

可以根据over内的排序生成序号,序号为间断的

### dense_rank()

对结果集进行排序，排名值没有间断。 特定行的排名等于该特定行之前不同排名值的数量加一。

### opengauss 返回星期

#### 几个重要函数

- date_part('week',d1) - 日期d1在当年的周次。week('2022-2-7') = 6;
- extract(DOW FROM cast(d1 as TIMESTAMP) - 返回d1在本周的次序(0 = Sunday, 1 = Monday, …, 6 = Saturday)
- case when expr1 then expr2 else expr3 end - 若expr1为TRUE，返回expr2，否则返回expr3.