---
title: MySql基础学习
categories:
  - Programing
  - DataBase
tags:
  - MySQL
date: 2021-04-15 11:39:06
---

# 1. MySQL 简介
-----------

### 1.1 什么是数据库 ？

数据库（_Database_）是按照数据结构来组织、存储和管理数据的仓库，它产生于距今六十多年前，随着信息技术和市场的发展，特别是二十世纪九十年代以后，数据管理不再仅仅是存储和管理数据，而转变成用户所需要的各种数据管理的方式。数据库有很多种类型，从最简单的存储有各种数据的表格到能够进行海量数据存储的大型数据库系统都在各个方面得到了广泛的应用。

主流的数据库有：sqlserver，mysql，Oracle、SQLite、Access、MS SQL Server 等，本文主要讲述的是 MySQL 。

MySQL 是一种开放源代码的关系型数据库管理系统（RDBMS），MySQL 数据库系统使用最常用的数据库管理语言–结构化查询语言（SQL）进行数据库管理。在 WEB 应用方面 MySQL 是最好的 RDBMS (Relational Database Management System，关系数据库管理系统) 应用软件之一。

### 1.2 数据库管理是干什么用的？

*   a. 将数据保存到文件或内存
*   b. 接收特定的命令，然后对文件进行相应的操作

PS：如果有了以上管理系统，无须自己再去创建文件和文件夹，而是直接传递 命令 给上述软件，让其来进行文件操作，他们统称为数据库管理系统（DBMS，Database Management System）

# 2. MySQL 安装
-----------

### 2.1 安装

```
$sudo apt-get update
$sudo apt-get install mysql-server
$sudo apt isntall mysql-client
$sudo apt install libmysqlclient-dev
$sudo mysql_secure_installation  #运行安全脚本，设置 root 密码
```

### 2.2 允许远程连接

修改配置文件 mysqld.cnf `$sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf` 将 `bind-address = 127.0.0.1` 注释掉。 连接数据库 `mysql -u root -p` 执行授权命令：

```
grant all on *.* to root@'%' identified by 'PASSWORD' with grant option;
flush privileges;
```

退出 MySQL，重启 MySQL 服务： `service mysql restart`

# 3. MySQL 操作
-----------

### 3.1 连接数据库

`mysql -u user -p`

退出连接：

`QUIT` 或者 Ctrl+D

### 3.2 查看、创建、使用数据库

*   查看数据库:
    
    `show databases;`
    
  
*   默认数据库：
    *   _mysql_ - 用户权限相关数据
    *   _test_ - 用于用户测试数据
    *   _information_schema_ - MySQL 本身架构相关数据
*   创建数据库:
    
    `create database db_NAME DEFAULT CHARSET utf8 COLLATE utf8_general_ci;`
    
  
*   使用数据库:
    
    `use db_NAME;`
    
  
*   显示当前使用的数据库中所有表：
    
    `SHOW TABLES;`
    

### 3.3 用户管理

*   创建用户
    
    `create user 'user_NAME'@'ip_ADDRESS' identified by 'password';`
    
  
*   删除用户
    
    `drop user 'user_NAME'@'ip_ADDRESS';`
    
  
*   修改用户
    
    `rename user 'user_NAME'@'ip_ADDRESS'; to 'new_USER'@'ip_ADDRESS';`
    
  
*   修改密码
    
    `set password for 'user_NAME'@'ip_ADDRESS' = Password('new_PASSWORD');`
    
  
*   _注：用户权限相关数据保存在 mysql 数据库的 user 表中，所以也可以直接对其进行操作（不建议）_

### 3.4 权限管理

#### 3.4.1 MySQL 用户权限列表：

<table><thead><tr><th>权限</th><th>说明</th></tr></thead><tbody><tr><td>Select</td><td>可以进行 SELECT 查询</td></tr><tr><td>Insert</td><td>可以进行 INSERT 插入</td></tr><tr><td>Update</td><td>可以进行 UPDATE 修改数据</td></tr><tr><td>Delete</td><td>可以进行 DELETE 删除数据</td></tr><tr><td>Create</td><td>可以创建新的数据库和表</td></tr><tr><td>Drop</td><td>可以删除现有数据库和表</td></tr><tr><td>Reload</td><td>可以执行刷新和重新加载 MySQL 所用各种内部缓存的特定命令，包括日志、权限、主机、查询和表</td></tr><tr><td>Shutdown</td><td>可以关闭 MySQL 服务器</td></tr><tr><td>Process</td><td>可以通过 SHOW PROCESSLIST 命令查看其他用户的进程</td></tr><tr><td>File</td><td>可以执行 SELECT INTO OUTFILE 和 LOAD DATA INFILE 命令</td></tr><tr><td>Grant</td><td>可以将已经授予给该用户自己的权限再授予其他用户</td></tr><tr><td>References</td><td>目前没有作用</td></tr><tr><td>Index</td><td>可以创建和删除表索引</td></tr><tr><td>Alter</td><td>可以重命名和修改表结构</td></tr><tr><td>Show_db</td><td>可以查看服务器上所有数据库的名字</td></tr><tr><td>Super</td><td>可以执行某些强大的管理功能，例如通过 KILL 命令删除用户进程，使用 SET GLOBAL 修改全局 MySQL 变量，执行关于复制和日志的各种命令</td></tr><tr><td>Create_tmp_table</td><td>可以创建临时表</td></tr><tr><td>Lock_tables</td><td>可以使用 LOCK TABLES 命令阻止对表的访问 / 修改</td></tr><tr><td>Execute</td><td>可以执行存储过程</td></tr><tr><td>Repl_slave</td><td>可以读取用于维护复制数据库环境的二进制日志文件</td></tr><tr><td>Repl_client</td><td>可以确定复制从服务器和主服务器的位置</td></tr><tr><td>Create_view</td><td>可以创建视图</td></tr><tr><td>Show_view</td><td>可以查看视图或了解视图如何执行</td></tr><tr><td>Create_routine</td><td>可以更改或放弃存储过程和函数</td></tr><tr><td>Alter_routine</td><td>可以修改或删除存储函数及函数</td></tr><tr><td>Create_user</td><td>可以执行 CREATE USER 创建新的 MySQL 账户</td></tr><tr><td>Event</td><td>可以创建、修改和删除事件</td></tr><tr><td>Trigger</td><td>可以创建和删除触发器</td></tr></tbody></table>

#### 3.4.2 MySQL 数据库权限用法：

<table><thead><tr><th>方法</th><th>说明</th></tr></thead><tbody><tr><td>数据库名.*</td><td>数据库中的所有内容</td></tr><tr><td>数据库名. 表名</td><td>指定数据库中的某张表</td></tr><tr><td>数据库名. 存储过程</td><td>指定数据库中的存储过程</td></tr><tr><td>* . *</td><td>所有数据库的所有内容</td></tr></tbody></table>

#### 3.4.3 MySQL 对于用户和 IP 的限制方法：

<table><thead><tr><th>方法</th><th>说明</th></tr></thead><tbody><tr><td>用户名 @IP 地址</td><td>用户只能在改 IP 下才能访问</td></tr><tr><td>用户名 @192.168.1.%</td><td>用户只能在改 IP 段下才能访问 (通配符 % 表示任意)</td></tr><tr><td>用户名 @%</td><td>用户可以再任意 IP 下访问 (默认 IP 地址为 %)</td></tr></tbody></table>

#### 3.4.4 权限控制

*   查看用户的权限：
    
    `show grants for 'user_NAME'@'ip_ADDRESS';`
    
  
*   授权
    
    `grant 权限 on 数据库.表 to 'user_NAME'@'ip_ADDRESS';`
    
  
*   取消授权
    
    `revoke 权限 on 数据库.表 from '用户名'@'IP地址';`
    

# 4. MySQL 表操作
------------

### 4.1 查看表

*   `show tables; # 查看数据库全部表`
*   `select * from 表名; # 查看表所有内容`

### 4.2 创建表

```
create  table 表名(
   列名  类型  是否可以为空 是否自动增长(可选)，
   列名  类型  是否可以为空，
   指定主键
)ENGINE=InnoDB  DEFAULT  CHARSET=utf8；
```

例：

```
CREATE TABLE `tab_NAME`(
 `nid` int(11) NOT NULL auto_increment,
 `name` varchar(255) NOT NULL,
 `email` varchar(255),
 PRIMARY KEY(`nid`)
)ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

*   _注_：
    *   默认值，创建列时可以指定默认值，当插入数据时如果未主动设置，则自动添加默认值。
    *   自增 (auto_increment)，如果为某列设置自增列，插入数据时无需设置此列，默认将自增（表中只能有一个自增列）注意：对于自增列，必须是索引（含主键），对于自增可以设置步长和起始值。
    *   主键，一种特殊的唯一索引，不允许有空值，如果主键使用单个列，则它的值必须唯一，如果是多列，则其组合必须唯一。

### 4.3 删除表

`drop table table_NAME;`

### 4.4 清空表内容

```
delete from table_NAME;
truncate table table_NAME;
```

### 4.5 修改表

*   添加列：
    
    `alter table 表名 add 列名 类型;`
    
  
*   删除列：
    
    `alter table 表名 drop column 列名;`
    
  
*   修改列：
    *   `alter table table_NAME modify column 列名 类型;`
    *   `alter table table_NAME change 原列名 新列名 类型;`
*   添加主键：
    
    `alter table table_NAME add primary key(列名);`
    

### 4.6 基本数据类型

MySQL 中定义数据字段的类型对数据库的优化是非常重要的。 MySQL 支持多种类型，大致可以分为三类：数值、日期 / 时间和字符串 (字符) 类型。

#### 4.6.1 数值类型

MySQL 支持所有标准 SQL 数值数据类型。 这些类型包括严格数值数据类型 (INTEGER、SMALLINT、DECIMAL 和 NUMERIC)，以及近似数值数据类型 (FLOAT、REAL 和 DOUBLE PRECISION)。

关键字 INT 是 INTEGER 的同义词，关键字 DEC 是 DECIMAL 的同义词。  
BIT 数据类型保存位字段值，并且支持 MyISAM、MEMORY、InnoDB 和 BDB 表。

作为 SQL 标准的扩展，MySQL 也支持整数类型 TINYINT、MEDIUMINT 和 BIGINT。下面的表显示了需要的每个整数类型的存储和范围。

<table><thead><tr><th>类型</th><th>大小</th><th>范围（有符号）</th><th>范围（无符号）</th><th>用途</th></tr></thead><tbody><tr><td>TINYINT</td><td>1 字节</td><td>(-128，127)</td><td>(0，255)</td><td>小整数值</td></tr><tr><td>SMALLINT</td><td>2 字节</td><td>(-32 768，32 767)</td><td>(0，65 535)</td><td>大整数值</td></tr><tr><td>MEDIUMINT</td><td>3 字节</td><td>(-8 388 608，8 388 607)</td><td>(0，16 777 215)</td><td>大整数值</td></tr><tr><td>INT 或 INTEGER</td><td>4 字节</td><td>(-2 147 483 648，2 147 483 647)</td><td>(0，4 294 967 295)</td><td>大整数值</td></tr><tr><td>BIGINT</td><td>8 字节</td><td>(-9 233 372 036 854 775 808，9 223 372 036 854 775 807)</td><td>(0，18 446 744 073 709 551 615)</td><td>极大整数值</td></tr><tr><td>FLOAT</td><td>4 字节</td><td>(-3.402 823 466 E+38，-1.175 494 351 E-38)，0，(1.175 494 351 E-38，3.402 823 466 351 E+38)</td><td>0，(1.175 494 351 E-38，3.402 823 466 E+38)</td><td>单精度浮点数值</td></tr><tr><td>DOUBLE</td><td>8 字节</td><td>(-1.797 693 134 862 315 7 E+308，-2.225 073 858 507 201 4 E-308)，0，(2.225 073 858 507 201 4 E-308，1.797 693 134 862 315 7 E+308)</td><td>0，(2.225 073 858 507 201 4 E-308，1.797 693 134 862 315 7 E+308)</td><td>双精度浮点数值</td></tr><tr><td>DECIMAL</td><td>对 DECIMAL(M,D) ，如果 M&gt;D，为 M+2 否则为 D+2</td><td>依赖于 M 和 D 的值</td><td>依赖于 M 和 D 的值</td><td>小数值</td></tr></tbody></table>

#### 4.6.2 日期和时间类型

表示时间值的日期和时间类型为 DATETIME、DATE、TIMESTAMP、TIME 和 YEAR。 每个时间类型有一个有效值范围和一个 "零" 值，当指定不合法的 MySQL 不能表示的值时使用 "零" 值。

<table><thead><tr><th>类型</th><th>大小 (字节)</th><th>范围</th><th>格式</th><th>用途</th></tr></thead><tbody><tr><td>DATE</td><td>3</td><td>1000-01-01/9999-12-31</td><td>YYYY-MM-DD</td><td>日期值</td></tr><tr><td>TIME</td><td>3</td><td>'-838:59:59'/'838:59:59'</td><td>HH:MM:SS</td><td>时间值或持续时间</td></tr><tr><td>YEAR</td><td>1</td><td>1901/2155</td><td>YYYY</td><td>年份值</td></tr><tr><td>DATETIME</td><td>8</td><td>1000-01-01 00:00:00/9999-12-31 23:59:59</td><td>YYYY-MM-DD HH:MM:SS</td><td>混合日期和时间值</td></tr><tr><td>TIMESTAMP</td><td>4</td><td>1970-01-01 00:00:00/2038 结束时间是第 <strong>2147483647</strong> 秒，北京时间 <strong>2038-1-19 11:14:07</strong>，格林尼治时间 2038 年 1 月 19 日 凌晨 03:14:07</td><td>YYYYMMDD HHMMSS</td><td>混合日期和时间值，时间戳</td></tr></tbody></table>

#### 4.6.3 字符串类型

字符串类型指 CHAR、VARCHAR、BINARY、VARBINARY、BLOB、TEXT、ENUM 和 SET。该节描述了这些类型如何工作以及如何在查询中使用这些类型。

<table><thead><tr><th>类型</th><th>大小</th><th>用途</th></tr></thead><tbody><tr><td>CHAR</td><td>0-255 字节</td><td>定长字符串</td></tr><tr><td>VARCHAR</td><td>0-65535 字节</td><td>变长字符串</td></tr><tr><td>TINYBLOB</td><td>0-255 字节</td><td>不超过 255 个字符的二进制字符串</td></tr><tr><td>TINYTEXT</td><td>0-255 字节</td><td>短文本字符串</td></tr><tr><td>BLOB</td><td>0-65 535 字节</td><td>二进制形式的长文本数据</td></tr><tr><td>TEXT</td><td>0-65 535 字节</td><td>长文本数据</td></tr><tr><td>MEDIUMBLOB</td><td>0-16 777 215 字节</td><td>二进制形式的中等长度文本数据</td></tr><tr><td>MEDIUMTEXT</td><td>0-16 777 215 字节</td><td>中等长度文本数据</td></tr><tr><td>LONGBLOB</td><td>0-4 294 967 295 字节</td><td>二进制形式的极大文本数据</td></tr><tr><td>LONGTEXT</td><td>0-4 294 967 295 字节</td><td>极大文本数据</td></tr></tbody></table>

CHAR 和 VARCHAR 类型类似，但它们保存和检索的方式不同。它们的最大长度和是否尾部空格被保留等方面也不同。在存储或检索过程中不进行大小写转换。  
BINARY 和 VARBINARY 类似于 CHAR 和 VARCHAR，不同的是它们包含二进制字符串而不要非二进制字符串。也就是说，它们包含字节字符串而不是字符字符串。这说明它们没有字符集，并且排序和比较基于列值字节的数值值。

BLOB 是一个二进制大对象，可以容纳可变数量的数据。有 4 种 BLOB 类型：TINYBLOB、BLOB、MEDIUMBLOB 和 LONGBLOB。它们区别在于可容纳存储范围不同。

有 4 种 TEXT 类型：TINYTEXT、TEXT、MEDIUMTEXT 和 LONGTEXT。对应的这 4 种 BLOB 类型，可存储的最大长度不同，可根据实际情况选择。

# 5 MySQL 表内容操作
-------------

### 5.1 增加内容

*   `insert into table_NAME(列名,列名...) values (值,值,...)；`
*   `insert into table_NAME(列名,列名...) values (值,值,...),(值,值,值...)；`
*   `insert into table_NAME(列名,列名...) select (列名,列名...) from table_NAME；`
*   例： `insert into table_NAME(name,email) values('wh','wh@gmail.com');`

### 5.2 删除内容

*   `delete from table_NAME; # 删除table_NAME表里全部数据`
*   `delete from table_NAME where id＝1; # 删除 ID =1 那一行数据`
*   `delete from table_NAME where id＝1 and name＝'wh'; # 删除 ID =1 并且 name='wh' 那一行数据`

### 5.3 更改内容

`update table_NAME set name＝'wh' where id=1;`

### 5.4 查询内容

*   `select * from table_NAME;`
*   `select * from table_NAME where id > 1;`
*   `select nid,name,gender as gg from table_NAME where id > 1;`

例：

*   a. 条件判断 where
    *   `select * from table_NAME where id > 1 and name != 'wh' and num = 12;`
    *   `select * from table_NAME where id between 5 and 16;`
    *   `select * from table_NAME where id in (11,22,33);`
    *   `select * from table_NAME where id not in (11,22,33);`
    *   `select * from table_NAME where id in (select nid from table_NAME);`
*   b. 通配符 like
    *   `select * from table_NAME where name like 'www%'; # www开头的所有（多个字符串）`
    *   `select * from table_NAME where name like 'www_'; # zhang开头的所有（一个字符）`
*   c. 限制 limit
    *   `select * from table_NAME limit 5; # 前5行`
    *   `select * from table_NAME limit 4,5; # 从第4行开始的5行`
    *   `select * from table_NAME limit 5 offset 4; # 从第4行开始的5行`
*   d. 排序 asc、desc
    *   `select * from table_NAME order by nid asc; # 根据 nid 从小到大排列`
    *   `select * from table_NAME order by nid desc; # 根据 nid 从大到小排列`
    *   `select * from table_NAME order by nid desc,nid1 asc; # 根据 nid 从大到小排列，如果相同则按 nid1 从小到大排序`
*   e. 分组 group by
    *   `select num from table_NAME group by num;`
    *   `select num,nid from table_NAME group by num,nid;`
    *   `select num,nid from table_NAME where nid > 10 group by num,nid order nid desc;`
    *   `select num,nid,count(*),sum(score),max(score),min(score) from table_NAME group by num,nid;`
    *   `select num from table_NAME group by num having max(id) > 10;`
    *   注意：group by 必须在 where 之后，order by 之前

本文部分内容来源与 [“马哥教育”](http://www.magedu.com/)