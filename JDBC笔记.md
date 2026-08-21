## 1JDBC笔记

## 1 JDBC简介

**JDBC 概念：**

- JDBC 就是使用 Java 语言操作关系型数据库的一套 API
- 全称：(Java DataBase Connectivity) Java 数据库连接

**JDBC 本质：**

- 官方（sun 公司）定义的一套操作所有关系型数据库的规则，即接口
- 各个数据库厂商去实现这套接口，提供数据库驱动 jar 包
- 我们可以使用这套接口（JDBC）编程，真正执行的代码是驱动 jar 包中的实现类

**JDBC 好处：**

- 各数据库厂商使用相同的接口，Java代码不需要针对不同数据库分别开发
- 可随时替换底层数据库，访问数据库的Java代码基本不变

**基础步骤：**

0. 创建工程，导入驱动 jar 包

   `mysql-connector-java-5.1.48.jar`

1. 注册驱动  ==新版不需要此步骤==

   `Class.forName ("com.mysql.jdbc.Driver");`

2. 获取连接

   `Connection conn = DriverManager.getConnection (url, username, password);`

3. 定义 SQL 语句

   `String sql = "update...";`

4. 获取执行 SQL 对象

   `Statement stmt = conn.createStatement ();`

5. 执行 SQL

   `stmt.executeUpdate (sql);`

6. 处理返回结果

7. 释放资源

## 2 API详解

### 2.1 DriverManager

#### 2.1.1 注册驱动

```java
Class.forName("com.mysql.jdbc.Driver");
```

- 查看 Driver 类源码

```java
static {
    try {
        DriverManager.registerDriver(new Driver());
    } catch (SQLException var1) {
        throw new RuntimeException("Can't register driver!");
    }
}
```

- MySQL 5之后的驱动包，可以省略注册驱动的步骤
- 自动加载jar包中`META-INF/services/java.sql.Driver`文件中的驱动类

DriverManager

#### 2.1.2 获取连接

`static Connection getConnection(String url, String user, String password)`

参数

1. **url**：连接路径

   语法：`jdbc:mysql://ip地址(域名):端口号/数据库名称?参数键值对1&参数键值对2...`  
   示例：`jdbc:mysql://localhost:3306/itheima`

   - 如果连接的是本机mysql服务器，并且mysql服务器默认端口是3306，则url可以简写为：`jdbc:mysql:///数据库名称?参数键值对`，即`jdbc:mysql:///itheima`
   - 配置`useSSL=false`参数，禁用安全连接方式，解决警告提示
   
2. **user**：用户名

3. **password**：密码

### 2.2 Connection

#### 2.2.1 获取执行 SQL 的对象

- 普通执行 SQL 对象

  `Statement createStatement()`

- 预编译 SQL 的执行 SQL 对象：防止 SQL 注入

  `PreparedStatement prepareStatement(sql)`

- 执行存储过程的对象

  `CallableStatement prepareCall(sql)`

#### 2.2.2 事务管理

- MySQL 事务管理

  开启事务：BEGIN; / START TRANSACTION;

  提交事务：COMMIT;

  回滚事务：ROLLBACK;

  MySQL 默认自动提交事务

- JDBC 事务管理：Connection 接口中定义了 3 个对应的方法

  开启事务：setAutoCommit (boolean autoCommit)：
  	true 为自动提交事务；false 为手动提交事务，即为开启事务

  提交事务：commit ()

  回滚事务：rollback ()

### 2.3 Statement

**作用**：执行 SQL 语句

```java
int executeUpdate(sql) ：执行 DML、DDL 语句
//返回值:(1) DML 语句: 受影响的行数，失败后返回0
//    	(2) DDL 语句: 执行成功也可能返回 0
```

```java
ResultSet executeQuery(sql)`：执行 DQL 语句
//返回值：ResultSet 结果集对象
```

### 2.4 ResultSet

- ResultSet 方法

```java
ResultSet stateMent.executeQuery(sql)：执行 DQL 语句，返回 ResultSet 对象
```

**获取查询结果**

```java
boolean next()：(1) 将光标从当前位置向下移动一行 (2) 判断当前行是否为有效行
    
//返回值：
//- true：有效行，当前行有数据
//- false：无效行，当前行没有数据
```

```java
xxx getXxx(int/String col)`：获取数据

//xxx：数据类型；如：`int getInt(int col)`；`String getString(int col)`
//col：int：第几列，String：列名
```

**使用步骤**：

1. 游标向下移动一行，并判断该行否有数据：`next()`
2. 获取数据：`getXxx(参数)`

```java
ResultSet rs = stateMent.executeQuery(sql);
ArrayList<Emp> emps = new ArrayList<>;

//循环判断游标是否是最后一行末尾
while(rs.next()){
    //获取数据
    int id = rs.getInt(1);
    String name= rs.getString(2);
    double salary = rs.getDouble(3);
    
    emps.add(new Emp(id,name,salary));
}
```

### 2.5 PreparedStatement

#### 2.5.1 SQL注入

- PreparedStatement 作用：预编译 SQL 语句并执行，预防 SQL 注入问题

- SQL 注入
  - SQL 注入是通过操作输入来修改事先定义好的 SQL 语句，用以达到执行代码对服务器进行攻击的方法。

```java
String name = "suiYI";
String pwd = "' or '1' = '1";

String sql = 
    "select * from tb_user where username = '"+name+"' and password = '"+pwd+"'";

select * from tb_user where username = 'suiYi' and password = '' or '1' = '1'
```

#### 2.5.2 步骤

① 获取 PreparedStatement 对象

```java
// SQL语句中的参数值，使用？占位符替代
String sql = "select * from user where username = ? and password = ?";
// 通过Connection对象获取，并传入对应的sql语句
PreparedStatement pstmt = conn.prepareStatement(sql);
```

② 设置参数值

```java
PreparedStatement 对象：setXxx (参数 1，参数 2)：给？赋值

- Xxx：数据类型；如 setInt (参数 1，参数 2)
- 参数 1：  ？的位置，从 1 开始
- 参数 2：  ？的值
```

③ 执行 SQL

`executeUpdate (); /executeQuery ();` ：不需要再传递 sql

```java
pstmt.setString(1,name);
pstmt.setString(2,password);

ResultSet rs = pstmt.executeQuery();

//这种方法会对字符和关键字进行转译
// ' or '1' = '1
// \' or \'1\' = \'1
select * from tb_user where username = 'suiYi' and password = '\' or \'1\' = \'1'
```

#### 2.5.3 优点

PreparedStatement 好处：

1. 预编译 SQL，性能更高（实现要开启）
2. 防止 SQL 注入：将敏感字符进行转义

① PreparedStatement 预编译功能开启：`useServerPrepStmts=true`

举例：`jdbc:mysql://localhost:3306/itheima？useServerPrepStmts=true`

② 配置 MySQL 执行日志（重启 mysql 服务后生效）

​	位置：mysql安装路径，找到 my.ini 文件，加入下列语句

```
log-output=FILE
general-log=1
general_log_file="D:\mysql.log"
slow-query-log=1
slow_query_log_file="D:\mysql_slow.log"
long_query_time=2
```

- PreparedStatement 原理：

1. 在获取 PreparedStatement 对象时，将 sql 语句发送给 mysql 服务器进行检查，编译（这些步骤很耗时）
2. 执行时就不用再进行这些步骤了，速度更快
3. 如果 sql 模板一样，则只需要进行一次检查、编译

## 3 数据库连接池

### 3.1 介绍

**数据库连接池**是个容器，负责分配、管理数据库连接 (Connection)

它允许应用程序重复使用一个现有的数据库连接，而不是再重新建立一个；

释放空闲时间超过最大空闲时间的数据库连接来避免因为没有释放数据库连接而引起的数据库连接遗漏

好处：

- 资源重用
- 提升系统响应速度
- 避免数据库连接遗漏

### 3.2 数据库连接池实现

- 标准接口：

  DataSource

  - 官方 (SUN) 提供的数据库连接池标准接口，由第三方组织实现此接口。
  - 功能：获取连接

```
Connection getConnection()
```

- 常见的数据库连接池：
  - DBCP
  - C3P0
  - Druid
- Druid (德鲁伊)
  - Druid 连接池是阿里巴巴开源的数据库连接池项目
  - 功能强大，性能优秀，是 Java 语言最好的数据库连接池之一

### 3.3 Druid 使用步骤

1. 导入 jar 包 druid-1.1.12.jar
2. 定义配置文件
3. 加载配置文件
4. 获取数据库连接池对象
5. 获取连接

