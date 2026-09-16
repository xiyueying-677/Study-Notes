## MySQL笔记（下）

[MySQL笔记（上）](MySQL笔记（上）)

## 1 视图

视图（View）是一种虚拟存在的表。视图中的数据并不在数据库中实际存在，

行和列数据来自定义视图的查询中使用的表，并且是在使用视图时动态生成的。
视图只保存了查询的SQL逻辑，不保存查询结果。

### 1.1 基本语法

**1). 创建**

```
CREATE [OR REPLACE] VIEW 视图名称[(列名列表)] AS 
	SELECT语句 [ WITH [CASCADED | LOCAL ] CHECK OPTION ]
```

**2). 查询**

```
查看创建视图语句：SHOW CREATE VIEW 视图名称;
查看视图数据：SELECT * FROM 视图名称 ...... ;
```

**3). 修改**

```
方式一：
CREATE OR REPLACE VIEW 视图名称[(列名列表)] AS 
	SELECT语句 [ WITH[ CASCADED | LOCAL ] CHECK OPTION ]
方式二：
ALTER VIEW 视图名称[(列名列表)] AS 
	SELECT语句 [ WITH [ CASCADED |LOCAL ] CHECK OPTION ]
```

**4). 删除**

```
DROP VIEW [IF EXISTS] 视图名称 [,视图名称,...] ...
```

### 1.2 检查选项

在视图里插入的数据不在视图的条件内，视图中没有对应数据，基表中有

当使用WITH CHECK OPTION子句创建视图时，MySQL会通过视图检查正在更改的每个行，例如 插
入，更新，删除，以使其符合视图的定义。

检查的范围：CASCADED 和 LOCAL，默认值为 CASCADED 。

**1). CASCADED 级联。**

v2视图是基于v1视图的，如果在v2视图指定了检查选项为 cascaded，但是v1视图未指定检查选项。 则在执行检查时，会同时检查v1、v2

**2). LOCAL 本地**

v2视图是基于v1视图的，如果在v2视图指定了检查选项为 local ，但是v1视图未指定检查选项。 则在执行检查时，只检查v2

> 注意：若v2基于v1，只要v1有检查选项，v2都会检查v1的条件

### 1.3 更新

要使视图可更新，视图中的行与基础表中的行之间必须存在一对一的关系。如果视图包含以下任何一
项，则该视图不可更新：

- 聚合函数或窗口函数（SUM()、 MIN()、 MAX()、 COUNT()等）
- DISTINCT
- GROUP BY
- HAVING
- UNION 或者 UNION ALL

### 1.4 视图作用

**1). 简单**
视图不仅可以简化用户对数据的理解，也可以简化他们的操作。那些被经常使用的查询可以被定义为视图，从而使得用户不必为以后的操作每次指定全部的条件。

**2). 安全**
数据库可以授权，但不能授权到数据库特定行和特定的列上。通过视图用户只能查询和修改他们所能见到的数据

**3). 数据独立**
视图可帮助用户屏蔽真实表结构变化带来的影响。

## 2 存储过程

### 2.1 介绍

存储过程是事先经过编译并存储在数据库中的一段 SQL 语句的集合

存储过程的思想就是数据库 SQL 语言层面的代码封装与重用。

- 封装，复用： 可以把某一业务SQL封装在存储过程中，需要用到的时候直接调用即可。
- 可以接收参数，也可以返回数据：在存储过程中，可以传递参数，也可以接收返回值。
- 减少网络交互，效率提升：如果涉及到多条SQL，每执行一次都是一次网络传输。
  而如果封装在存储过程中，我们只需要网络交互一次可能就可以了。

### 2.2 基本语法

**1). 创建**

```mysql
CREATE PROCEDURE 名称 ([ 参数列表 ])
BEGIN
  SQL语句
END ;
```

**2). 调用**

```
CALL 名称 ([ 参数 ]);
```

**3). 查看**

```
-- 查询指定数据库的存储过程及状态信息
SELECT * FROM INFORMATION_SCHEMA.ROUTINES WHERE ROUTINE_SCHEMA = '表名'; 
-- 查询某个存储过程的定义
SHOW CREATE PROCEDURE 存储过程名称 ; 
```

**4). 删除**

```
DROP PROCEDURE [ IF EXISTS ] 存储过程名称 ；
```

>注意:
>在命令行中，执行创建存储过程的SQL时，需要通过关键字 delimiter 指定SQL语句的结束符。
>
>```mysql
>delimiter $$
>
>create procedure p1()
>begin
>select count(*) from student; -- select语句自带; 系统会识别为终止符
>end$$
>```



### 2.3 变量

在MySQL中变量分为三种类型: 系统变量、用户定义变量、局部变量

#### 2.3.1 系统变量

系统变量 是MySQL服务器提供，不是用户定义的，属于服务器层面。
分为全局变量（GLOBAL）、会话变量（SESSION）。
**1). 查看系统变量**

```mysql
SHOW [ SESSION | GLOBAL ] VARIABLES ; -- 查看所有系统变量
SHOW [ SESSION | GLOBAL ] VARIABLES LIKE '......'; --
-- 可以通过LIKE模糊匹配方式查找变量
SELECT @@[SESSION | GLOBAL]系统变量名; -- 查看指定变量的值
```

**2). 设置系统变量**

```mysql
SET [ SESSION | GLOBAL ] 系统变量名 = 值 ;
SET @@[SESSION | GLOBAL]系统变量名 = 值 ;
```

>注意:
>如果没有指定SESSION/GLOBAL，默认是SESSION，会话变量。
>
>- mysql服务重新启动之后，所设置的全局参数会变回默认值
>  要想不失效，可以在 /etc/my.cnf 中配置。
>
>A. 全局变量(GLOBAL): 全局变量针对于所有的会话。
>B. 会话变量(SESSION): 会话变量针对于单个会话，在另外一个会话窗口就不生效了。

#### 2.3.2 用户变量

用户定义变量 是用户根据需要自己定义的变量，用户变量不用提前声明，在用的时候直接用 "@变量名" 使用就可以。其作用域为当前连接。

**1). 赋值**

方式一:

```mysql
SET @var_name [:]= expr [, @var_name [:]= expr] ... ;
```

> =可以为比较和赋值
>
> ：=只为赋值
>
> 在select要表示赋值一定要：=

方式二:

```mysql
SELECT @var_name := expr [, @var_name := expr] ... ;
SELECT 字段名 INTO @var_name FROM 表名;
```

> 注意: 用户定义的变量没有对其进行声明或初始化，获取到的值为NULL。

**2). 使用**

```mysql
SELECT  @var_name ;
```

#### 2.3.3 局部变量

局部变量 需要用DECLARE声明。局部变量的生效范围是在其内声明的BEGIN ... END块。

**1). 声明**

```mysql
DECLARE 变量名 变量类型 [DEFAULT 值 ] ;
```

变量类型就是数据库字段类型：INT、BIGINT、CHAR、VARCHAR、DATE、TIME等。

**2). 赋值**

```mysql
SET 变量名 [:]= 值 ;
SELECT 字段名 INTO 变量名 FROM 表名 ... ;
```

### 2.4 if

if 用于做条件判断，具体的语法结构为：

```
IF 条件1 THEN
ELSEIF 条件2 THEN 
.....
ELSE 
END IF;
```

### 2.5 参数

| 类型  | 含义                                         | 备注 |
| ----- | -------------------------------------------- | ---- |
| IN    | 该类参数作为输入，也就是需要调用时传入值     | 默认 |
| OUT   | 该类参数作为输出，也就是该参数可以作为返回值 |      |
| INOUT | 既可以作为输入参数，也可以作为输出参数       |      |

用法

```
CREATE PROCEDURE 存储过程名称 ([ IN/OUT/INOUT 参数名 参数类型 ])
BEGIN
-- SQL语句
END ;
```

### 2.6 case

语法1

```
CASE value
	WHEN when_value1 THEN statement_list1
	[ WHEN when_value2 THEN statement_list2] ...
	[ ELSE statement_list ]
	END CASE;
```

>类似于switch语句，但没有穿透行为
>
>即条件成立后，执行后面所有语句

语法2

```
CASE
	WHEN condition1 THEN statement_list1
	[WHEN condition2 THEN statement_list2] ...
	[ELSE statement_list]
END CASE;
```

> 类似于if else语句

### 2.7 循环

**1）while**

```mysql
-- 先判定条件，如果条件为true，则执行逻辑，否则，不执行逻辑
WHILE 条件 DO
	SQL逻辑...
END WHILE;
```

**2）repeat**

```mysql
-- 先执行一次逻辑，然后判定UNTIL条件是否满足，如果满足则退出。如果不满足则继续下一次循环
REPEAT
	SQL逻辑...
	UNTIL 条件
END REPEAT;
```

**3）loop**

```mysql
[begin_label:] LOOP
	SQL逻辑...
END LOOP [end_label];

LEAVE label; -- 退出指定标记的循环体
ITERATE label; -- 直接进入下一次循环
```

### 2.8 游标

游标（CURSOR）是用来存储查询结果集的数据类型 , 
在存储过程和函数中可以使用游标对结果集进行循环的处理。

1.声明游标

`DECLARE 游标名称 CURSOR FOR 查询语句 ;`

2.打开游标

`OPEN 游标名称 ;`

3.获取游标记录

`FETCH 游标名称 INTO 变量 [, 变量 ] ;`

4.关闭游标

`CLOSE 游标名称 ;`

### 2.9 handler

条件处理程序（Handler）：用来定义在流程控制结构执行过程中遇到问题时相应的处理步骤

语法

```mysql
DECLARE handler_action HANDLER FOR 
condition_value [, condition_value]... statement ;

handler_action 的取值：
	CONTINUE: 继续执行当前程序
	EXIT: 终止执行当前程序
	
condition_value 的取值：
	SQLSTATE sqlstate_value: 状态码，如 02000,表示游标无读取内容的报错
	SQLWARNING: 所有以01开头的SQLSTATE代码的简写
	NOT FOUND: 所有以02开头的SQLSTATE代码的简写
	SQLEXCEPTION: 除去01，02开头的SQLSTATE代码的简写
```

关于错误码的官方文档

https://dev.mysql.com/doc/refman/8.0/en/declare-handler.html
https://dev.mysql.com/doc/mysql-errors/8.0/en/server-error-reference.html

### 2.1 存储函数

存储函数是有返回值的存储过程，存储函数的参数只能是IN类型的

```mysql
CREATE FUNCTION 存储函数名称 ([ 参数列表 ])
RETURNS type [characteristic ...]
BEGIN
	-- SQL语句
	RETURN ...;
END ;
```

characteristic说明：

- DETERMINISTIC：相同的输入参数总是产生相同的结果
- NO SQL ：不包含 SQL 语句。
- READS SQL DATA：包含读取数据的语句，但不包含写入数据的语句。

## 3 触发器

### 3.1 介绍

触发器是与表有关的数据库对象，指在insert/update/delete之前或之后，
触发并执行触发器中定义的SQL语句集合。
能完成日志记录 , 数据校验等操作

|   触发器类型    | NEW 和 OLD                                             |
| :-------------: | :----------------------------------------------------- |
| INSERT 型触发器 | NEW 表示将要或者已经新增的数据                         |
| UPDATE 型触发器 | OLD 表示修改之前的数据，NEW 表示将要或已经修改后的数据 |
| DELETE 型触发器 | OLD 表示将要或者已经删除的数据，无NEW                  |

MYSQL只支持行级触发，不支持语句级触发。

### 3.2 语法

1）创建

```mysql
CREATE TRIGGER trigger_name
BEFORE/AFTER INSERT/UPDATE/DELETE
ON tbl_name FOR EACH ROW -- 行级触发器
BEGIN
	trigger_stmt ;
END;
```

2）查看

`SHOW TRIGGERS ;`

3）删除

`DROP TRIGGER [schema_name.]trigger_name ;`

### 3.3 案例

表结构准备

```mysql
-- 准备工作 : 日志表 user_logs
create table user_logs(
id int(2) not null auto_increment,
operation varchar(20) not null comment '操作类型, insert/update/delete',
operate_time datetime not null comment '操作时间',
operate_id int(2) not null comment '操作的ID',
operate_params varchar(500) comment '操作参数',
primary key(`id`)
)engine=innodb default charset=utf8;
```

触发器创建

```mysql
create trigger tb_user_insert_trigger
after insert on tb_user for each row
begin
	insert into user_logs(id, operation, operate_time, operate_id, operate_params)
	VALUES(null, 'insert', now(), new.id, concat('插入的数据内容为:
	id=',new.id,',name=',new.name, ', phone=', NEW.phone, ', email=', NEW.email, ',
	profession=', NEW.profession));
end;
```

concat：把字符串首尾拼接

## 4 锁

### 4.1 概述

**锁** ：计算机协调多个进程或线程并发访问某一资源的机制.

MySQL中的锁，按照锁的粒度分，分为以下三类：

- 全局锁：锁定数据库中的所有表。
- 表级锁：每次操作锁住整张表。
- 行级锁：每次操作锁住对应的行数据。

### 4.2 全局锁

#### 4.2.1 概述

**全局锁**：对整个数据库实例加锁，加锁后整个实例就处于只读状态，后续的DML，DDL语句，已经更新操作的事务提交语句都将被阻塞。

**使用场景**：做全库的逻辑**备份**，对所有的表进行锁定，从而获取一致性视图，保证数据的完整性。

#### 4.2.2 语法

**1). 加全局锁**

`flush tables with read lock ;`

**2). 数据备份**

`mysqldump -uroot –p123456 itcast > itcast.sql`

**3). 释放锁**

`unlock tables ;`

#### 4.2.3 特点

**缺陷**：

- 如果在主库上备份，那么在备份期间都不能执行更新，业务基本上就得停摆。
- 如果在从库上备份，那么在备份期间从库不能执行主库同步过来的二进制日志，会导致主从延迟。

在InnoDB引擎中，我们可以在备份时加上参数 --single-transaction 参数来完成不加锁的一致
性数据备份。

`mysqldump --single-transaction -uroot –p123456 itcast > itcast.sql`

### 4.3 表级锁

#### 4.3.1 介绍

**表级锁**：每次操作锁住整张表。锁定粒度大，发生锁冲突的概率最高，并发度最低

**分类**：

- 表锁
- 元数据锁（meta data lock，MDL）
- 意向锁

#### 4.3.2 表锁

**分类**：

- 表共享读锁（read lock）
- 表独占写锁（write lock）

**语法**：

- 加锁：`lock tables 表名... read/write`
  - 读锁：所有客户端只能读，不能写
  - 写锁：其他客户端不能读和写，本客户端可读写
- 释放锁：`unlock tables ` / 客户端断开连接

#### 4.3.3 元数据锁

meta data lock , 元数据锁，简写MDL。

- 系统自动控制，无需显式使用，在访问一张表的时候会自动加上。

- 作用：维护表元数据的数据一致性，在表上有活动事务的时候，不可以对元数据进行写入操作。
  **为了避免DML与DDL冲突，保证读写的正确性。**

> 元数据，简单理解就是一张表的表结构。
>
> 某一张表涉及到未提交的事务时，是不能够修改这张表的表结构的。

当对一张表进行增删改查的时候，加MDL读锁(共享)；
当对表结构进行变更操作的时候，加MDL写锁(排他)。

| 对应SQL                                       | 锁类型                                  | 说明                                             |
| --------------------------------------------- | --------------------------------------- | ------------------------------------------------ |
| lock tables xxx read / write                  | SHARED_READ_ONLY / SHARED_NO_READ_WRITE |                                                  |
| select、select ...<br />lock in share mode    | SHARED_READ                             | 与SHARED_READ、SHARED_WRITE兼容，与EXCLUSIVE互斥 |
| insert、update、delete、select ... for update | SHARED_WRITE                            | 与SHARED_READ、SHARED_WRITE兼容，与EXCLUSIVE互斥 |
| alter table ...                               | EXCLUSIVE                               | 与其他的MDL都互斥                                |

#### 4.3.4 意向锁

为了避免DML在执行时，加的行锁与表锁的冲突。
在InnoDB中引入了意向锁，使得表锁不用检查每行数据是否加锁，使用意向锁来减少表锁的检查。

> 每次添加行锁，会自动添加表锁

分类

- 意向共享锁(IS): 由语句select ... lock in share mode添加 。 
  与表锁共享锁(read)兼容，与表锁排他锁(write)互斥。
- 意向排他锁(IX): 由insert、update、delete、select...for update添加 。
  与表锁共享锁(read)及排他锁(write)都互斥，意向锁之间不会互斥。

```mysql
## 查看意向锁及行锁的加锁情况
select object_schema,object_name,index_name,lock_type,lock_mode,lock_data 
fromperformance_schema.data_locks;
```



### 4.4 行级锁

行级锁，每次操作锁住对应的行数据。
锁定粒度最小，发生锁冲突的概率最低，并发度最高。应用在InnoDB存储引擎中。

**分类：**

- 行锁（Record Lock）：锁定单个行记录的锁，防止其他事务对此行进行update和delete。
  在RC、RR隔离级别下都支持。
- 间隙锁（Gap Lock）：锁定索引记录间隙（不含该记录），确保索引记录间隙不变，
  防止其他事务在这个间隙进行insert，产生幻读。在RR隔离级别下支持。
- 临键锁（Next-Key Lock）：行锁和间隙锁组合，同时锁住数据，并锁住数据前面的间隙Gap。
  在RR隔离级别下支持。

InnoDB实现了以下两种类型的行锁：

- 共享锁（S）：允许一个事务去读一行，阻止其他事务获得相同数据集的排它锁。
- 排他锁（X）：允许获取排他锁的事务更新数据，阻止其他事务获得相同数据集的共享锁和排他锁。

![MySQl-11-行锁](img/MySQL/MySQl-11-行锁.png)

| SQL                           | 行锁类型 | 说明                                        |
| ----------------------------- | -------- | ------------------------------------------- |
| INSERT                        | 排他锁   | 自动加锁                                    |
| UPDATE                        | 排他锁   | 自动加锁                                    |
| DELETE                        | 排他锁   | 自动加锁                                    |
| SELECT（正常）                | 不加锁   |                                             |
| SELECT ... LOCK IN SHARE MODE | 共享锁   | 需要手动在 SELECT 之后加 LOCK IN SHARE MODE |
| SELECT ... FOR UPDATE         | 排他锁   | 需要手动在 SELECT 之后加 FOR UPDATE         |



- 索引上的等值查询(唯一索引)，给不存在的记录加锁时, 优化为间隙锁 。
- 索引上的等值查询(普通索引)，向右遍历时最后一个值不满足查询需求时，临键锁退化为间隙锁。
- 索引上的范围查询(唯一索引)--会访问到不满足条件的第一个值为止。

>注意：间隙锁唯一目的是防止其他事务插入间隙。
>
>间隙锁可以共存，一个事务采用的间隙锁不会阻止另一个事务在同一间隙上采用间隙锁。

