<h2><center> MySQL基本使用</center></h2>
<br/>

### 1. 数据库基础
**（一）数据库特征：**

* 数据结构化
* 实现数据共享
* 减少数据冗余
* 数据独立性

**（二）数据库类型：（按数据库模型特点分）**

* 网状型数据库
  * 采用记录类型为节点的网状数据模型
* 层次型数据库
  * 采用层次模型模拟现实世界中按层次组织起来的事物
* 关系型数据库
  * 采用二维表组织结构和管理数据，并规定了表内和表间数据的依赖关系
  * **关系型数据**库是指一些相关的表和其他数据库对象的集合，对于关系数据库来说，关系就是表的同义词3
    * 1. 表时由行和列组成（类似二维数组的结构）
    * 2. 列包含一组命名的**属性（也称字段）**
    * 3. 行包含一组记录，每行包含一条记录
    * 4. 行和列的交集称为**数据项**，指出了某列对应的属性在某行上的值，也称为**字段值**。
    * 5. 列需要定义数据类型，比如整数或者字符型的数据

**（三）数据库介绍**

1. Mysql时一种开放源代码的**关系型数据库管理系统（RDBMS）**，属于Oracle旗下产品
2. Mysql数据库系统使用最常用的数据管理语言----**结构化查询语言（SQL）**进行数据库管理
3. Mysql因为其速度、可靠性、适应性而备受关注
4. SQL语言主要用来操作关系型数据库的一本语言，称之为结构化查询语言语句
5. SQL语句主要分为：
    * DQL:数据查询语言，用于对数据进行查询，如select
    * DML:数据操作语言，对数据进行增加、修改、删除如insert 、update、delete
    * TPL:事务处理语言，对事物进行处理，包括begin transaction ,commit,roolback
    * DCL:数据控制语言，进行授权与权限回收，如grant，revoke
    * DDL:数据定义语言，进行数据库、表的管理等，如create  drop
    * CCL:指针控制语言，通过控制指针完成表的操作，如declare，cursor

**（四）数据库安装**

* MySQL服务端（在linux系统）
  
  * 下载安装 
    * sudo apt-get install mysql-server
  * 启动服务 
    * sudo service mysql start
  * 查看服务是否启动 
    * ps ajxlgrep mysql
    * sudo service mysql status
  * 停止服务
    * sudo service mysql stop
  * 重启服务
    * sudo service mysql restart
  
* 配置
  * 配置问加你目录为 /etc/mysql/mysql.conf.d
  * 进入目录，打开mysqld.cnf,可以看到配置项
  * bind-address 表示服务器绑定的ip，默认为127.0.0.1
  * port表示端口，默认是3306
  * datadir表示数据库目录，默认为/var/lib/mysql
  * general/ogfile表示普通日志，默认为/var/log/mysql/mysql.log
  * log_error表示错误日志，默认为/var/log/mysql/error.log
  
  ```bash
  #查询user所对应的host服务
  select user,host from user ;
  #修改密码
  update user set authentication_string="13467205064" where user="root";
  #配置允许远程访问
  update user set host="%" where user="root" and host="localhost" LIMIT 1;
  
  
  flush privileges;
  ```
  
* MySQL 客户端

  * 客户端为开发人员使用，常用的命令行客户端，navicat图形界面客户端等
  * 下载安装命令行客户端
    * sudo apt install mysql-client
  * 连接数据库
    * mysql -u root -p123456
      * -u后面跟的是数据库的账户名，-p密码， -p与密码之间不能有空格
      * 如果-p后面不加密码，那么回车后要求输入密码



### 2. 数据库和数据表管理

* **（一）连接数据库**
  * mysql -u帐号 -p密码 -h主机地址 -P端口  (端口默认3306)
  * mysql -uroot -pmysql
* **（二）查看数据库版本**
  * select version();
* **（三）显示当前时间**
  * select now();
* **（四）查看所有数据库**
  * show databases;

**数据库管理**

* （五）**创建数据库** 
  * create database 数据库名 charset=utf8；
* （六）**切换数据库**：
  * use 数据库名；
* （七）**查看当前正在使用哪个数据库**
  * select database();
* （八）**删除数据库：**
  * drop database 数据库名；

**数据表管理-数据表设计**

* 数据表设计包括ER图、表的主键、字段、数据类型、约束、表之间关系的设计
* **E-R模**型即实体-关系型主要用于定义 数据的存储需求，该模型已经广泛用于关系数据库设计，E-R模型由实体、属性、和关系三个基本要素构成
* **主键（Primary Key）**
  * 数据库要求表中的每一行记录都必须唯一，即在同一张表中不允需出现完全相同的两条记录
  * 数据库表中的主键有以下两种特征：
    * 表的主键可以由一个字断构成，也可以由多个字段构成，（这种情况称为复合主键）
    * 数据库表中的主键的值具有唯一性且不能取空值（NULL），当数据库表中的主键由多个字段构成时，每个字段的值不能取NULL值
* **实体间的关系与外键（Foreign Key）**
  * 班级实体和班主任实体为一对一的关系，班级实体和学生实体之间为一对多的关系，学生实体和课程实体之间为多对多的关系
  * 实体间的关系可以通过外键来表示，如果A中的一个字段a对应与表B的主键b，则字段a称为表A的外键，此时存储在表A中字段a的值，同时这个字段值也是表B主键b的值
* **约束（Constraint）**
  * 约束是定义在表上的一种强制规则，当为某个表定义约束后，对该表做的所有SQL操作斗必须满足约束的规则要求，否则操作失败

**数据表管理-创建表**
* **查看当前数据库中的表**
  * show tables;
* **创建表**
  * create table 表名();
```bash
#建表主要时前面时字段，字段后面跟单时约束条件
creat table students(
    id int auto_increment primary key not null,
    name varchar(1) not null ,
    gender bit(1) default 0,
    hometown varchar(40) default ""
);
```

* comment注释，在创建表的时候如果字段很多，防止忘记字段时存什么数据的，可以给字段添加注释

```bash
creat table students(
    id int auto_increment primary key not null comment '主键',
    name varchar(1) not null  comment '学生名',
    gender bit(1) default 0 commment '性别',
    hometown varchar(40) default "" comment '家乡地址'
);
```

* **查看创建表的sql语句**
  * show create table 表名;
* **查看表的结构**
  * desc student


**数据表管理-修改表**
* **添加字段**
  * alter table 表名 add 列名 类型;
  * 给student添加一个生日的字段
```bash
alter table student add birthday date;
```
* **删除字段**
  * alter table 表名 drop 字段名字
  * 给student表中的gender字段删除

```bash
alter table student drop gender;
```

* **修改字段**
  * 第一种 不修改字段名只修改类型及约束
    * alter table 表名 modify 列名 类型及约束;
  * 第二种 需要修改字段名字
    * alter table 表名 change 原名 新名 类型及约束;

**数据表管理-删除表**

* **drop table 表名**
  * 删除学生表

```bash
drop table student;
```

**Navicat 连接说明**

Navicat for mysql连接不上mysql服务的原因排除：

1. 确保mysql的数据库服务正确启动

2. 输入的用户名和密码是否正确

3. 数据是否开启远程连接「默认安装mysql 数据完成后，只允许本地连接」

   注意：对mysql配置文件更改完毕后，一定要重启mysql服务，否则连接不上

### 3. 简单查询于数据库操作

**基本查询语句**

* **查询表中所有**
  * select * from 表名;

```bash
select * from students;  查询students表中的所有内容
```

* **指定字段查询**

  * select 字段1 ,字段2 from 表名;

  * 比如只想看id，name这两列

    * ```bash
      select id,name from students;
      ```

**插入数据**

* **全列插入**

  * 全列插入时，有多少个字段，必须插入多少个字段，即使默认可以为空的字段也要占位。**主键自增也需要占位一般使用0占位**

  * insert into 表名 values(.......)

    * ```bash
      insert into students values(0,'韩信',0,'广州');
      ```

* **部分插入**

  * insert into 表名 (字段1，字段2) values (值1,值2)；

    * ```bash
      insert into students (name,gender) values ('程咬金',0);
      ```

* **全列多行插入**

  * 多行插入每一行的内容写在一个小括号内，用逗号分割多行。

  * insert into 表名 values (......),(........),(......);

    * ```bash
      insert into students values(0,'后裔'),(0,1，'广州'),(0,'应征'，0,'广州');
      ```

* **部分列多行插入**

  * insert into 表名(字段1,字段2)values(...),(...);

    * ```bash
      insert into students (name,gender,howtown) values('狄仁杰',0,’深圳),('孙上香',0,'深圳');
      
      ```

**修改数据**

* 修改一行内容，一定要加where限定条件，否则会造成全表修改，除非你想要修改整张表

* update 表名 set 字段=xxx where 字段=xxx;

  * ```bash
    update students set hometown='珠海'  where id =5;
    ```

**删除数据**

* 删除id 为3的程咬金

  * 删除行也要加限定条件，不加的话会造成全表删除

  * ```bash
    delete from students where id =3;
    ```

### 4. 备份和恢复数据库

* **备份数据库的所有表   的数据**

* mysqldump -u root -p 数据库名 >python.sql

* ```bash
  musqldump -u root -p studentDB >studentFile.sql
  ```

* 提示输入密码，mysql的密码

* **备份数据库的某个表的数据**

* mysqldump -u root -p 数据库名 数据表名 > 文件名.sql

* ```bash
  musqldump -u root -p studentDB student >student.sql
  ```

* **恢复**

* 先创建一个数据库

* ```bash
  mysql -u root -p restoreDB < student.sql
  ```





