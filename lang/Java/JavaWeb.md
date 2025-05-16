[toc]

# 1 JDBC

## 1.1 JDBC 简介

JDBC 就是<font color="red">使用 Java 语言操作关系型数据库的一套 API</font>

JDBC 全称为 "Java DataBase Connectivity"，即 Java 数据库连接

通过 JDBC ，我们可以<u>使用同一套 Java 代码，操作不同的关系型数据库，例如 MySQL 、SQLServer、ORACLE、DB2等等</u>

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221111112406030.png" alt="image-20221111112406030" style="zoom:33%;" />

```java
// 1. 注册驱动
Class.forName("com.mysql.jdbc.Driver");
// 2. 获取连接对象
String url = "jdbc:mysql://127.0.0.1:3306/db1?userSSL=false";
String username = "root";
Connection conn = "DriverManager.getConnection(url, username, password)";
// 3. 定义 sql
String sql = "update account set money = 2000 where id = 1";
// 4. 获取执行 sql 的对象
Statement stmt = conn.createStatement();
// 5. 执行 sql
int count = stmt.executeUpdate(sql);
// 6. 处理结果
// System.out.println(count);
// 7. 释放资源
stmt.close();
conn.close();
```

> 上述代码属于原来老版本的 java 连接数据库的操作，新版本已经不适用



## 1.2 建立 JDBC 连接

我们建立 JDBC 的连接的流程图如下：

![image-20221111165658057](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221111165658057.png)

**准备工作**

**1.mysql 驱动 jar 包的下载**

1. 进入 maven 官网：[传送入口](https://mvnrepository.com/artifact/commons-httpclient/commons-httpclient/3.1)

2. 在搜索框中，搜索 mysql

   <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221111160237434.png" alt="image-20221111160237434" style="zoom: 20%;" />

3. 选择版本

   - MySQL 数据库版本为 `5.x`，驱动 jar 包建议使用 `5.1.47`

   - MySQL 数据库版本为 `8.x`，驱动 jar 包建议使用 `8.0.x`（以下以 `8.0.26` 为例）

4. 选择 jar 包下载

   <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221111160748767.png" alt="image-20221111160748767" style="zoom: 20%;" />



**2.创建工程，导入驱动的 jar 包**

1. 创建一个项目

2. 在项目的根目录下创建一个空的文件，我们取名为 lib，表示 library 库

3. 将下载好的 jar 包放在里面，将驱动的 jar 包设置为 library，选择驱动 jar 包 --> 右键 --> Add as Library --> OK

   <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221111162752077.png" alt="image-20221111162752077" style="zoom: 50%;" />



**编写程序**

1. 注册驱动

   ```java
   // 1. 注册驱动(驱动 jar 包中的驱动类 Driver 的路径)
   Class.forName("com.mysql.cj.jdbc.Driver");
   ```

   > 通过反射机制，将驱动 jar 文件中提供的驱动类载入到 JVM 中

   8.0 以后的驱动 jar 包，位置在 com.mysql → cj → jdbc → Driver，右键学着复制路径

2. 创建连接（主要通过数据库驱动器 DriverManager）

   ```java
   // 2. 创建连接：通过数据库驱动器 DriverManager(java.sql.Driver)
   // JDBC JDK 提供的数据库连接的规范 --- java.sql
   // java.sql.Connection 接口，一个对象就表示一个数据库的连接
   Connection connection = DriverManager.getConnection(url, userName, password);
   ```

   - JDBC 是 JDK 提供的连接数据库的规范，这个规范就在 java.sql 这个包中

   - DriverManager 这个接口对象有个方法 getConnection 用以连接数据库

   - getConnection 方法中的参数 url 的格式如下：

     ```java
     // 协议://域名:端口号/数据库名?扩展部分
     jdbc:mysql://localhost:3306/db_test
     /* 
     * @localhost 我们也可以写成 127.0..0.1
     * @后面可以添加的扩展部分可以是
     	1.?characterEncoding=utf8: 用于处理字符的解码和编码格式，如果项目的字符集和MySQL 数据库中的字符集设置为同一字符集则 url 可以不加此参数
     	2.?usessl = true: 使用 SSL 通信
     	3.?useTimezone = true: 设置客户端和服务器的时区相同
         4.参数，例如 "?username=root&password=1234" 
     */
     ```
     

3. 编写 sql 指令

   ```java
   String sql = "update account set money = 1000 where id = 1";
   ```

   > 我们还可以通过 sql 语句的拼接来完成一条 sql 语句，当然这会导致 sql 注入的问题，这里留到后面解决:small_red_triangle:

4. 加载 sql 指令：获取 sql 指令的加载器

   ```java
   Statement statement = connection.createStatement();
   // 也可以通过 java.sql.PreparedStatement 对象，这个是 sql 指令的 "预编译加载器"
   ```

5. 加载、执行 SQL 获取执行结果

   ```java
   // a. 如果 sql 指令为 DQL 语句, 返回 ResultSet
   int i = statement.executeQuery(sql);
   // b. 如果 sql 指令为 DML 语句，则 statement.executeUpdate(sql), 返回 int 值，表示受影响的行数
   ```

6. 处理结果（业务）

   ```java
   // 例如
   System.out.println(i>0 ? "修改成功":"修改失败");
   ```

7. 关闭连接

   ```java
   // 1. 如果执行 DML操作，需要关闭 Statement 和 Connection，由于 statement 是通过 connection 创建出来的，因此首先得判断 statement 是否为空，再来判断 statement 是否关闭，如果都满足，则关闭 statement
   if(statement != null && !statement.isClosed()) {
       statement.close();
   }
   if(connection != null && !connection.isClosed()) {
       statement.close();
   }
   // 2. 如果执行的是 DQL 操作，需要关闭ResultSet、Statement 和 Connection
   ```

全部代码如下：

```java
package com.xzh.demo1;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;

public class JDBCDemo {
    public static void main(String[] args) throws ClassNotFoundException, SQLException {
        String url = "jdbc:mysql://localhost:3306/test_db"; // 包括数据库协议和
        String userName = "root";
        String password = "486101620";


        // 1. 注册驱动(驱动 jar 包中的驱动类 Driver 的路径)
        Class.forName("com.mysql.cj.jdbc.Driver");

        // 2. 创建连接：通过数据库驱动器 DriverManager(java.sql.Driver)
        // JDBC JDK 提供的数据库连接的规范 --- java.sql
        // java.sql.Connection 接口，一个对象就表示一个数据库的连接
        Connection connection = DriverManager.getConnection(url, userName, password);
        System.out.println(connection);

        // 3. 编写要执行的 sql 语句,sql 指令中需要的参数可以通过字符串拼接的形式(但是这种方式会导致 sql 注入的问题)
        String sql = "update account set money = 1000 where id = 1";

        // 4. 加载 sql 指令：通过创建好的数据库连接对象加载 sql 指令
        // java.sql.PrepareStatement 对象 SQL 指令的 "预编译加载器"
        Statement statement = connection.createStatement();

        // 5. 加载 sql，获取执行结果
        // a. 如果 sql 指令为 DQL，则 statement.executeQuery(sql), 返回 ResultSet
        // b. 如果 sql 指令为 DML，则 statement.executeUpdate(sql), 返回 int 值，表示影响的数据行数
        int i = statement.executeUpdate(sql);

        // 6. 处理结果(业务)
        System.out.println(i>0 ? "修改成功":"修改失败");

        // 7. 关闭连接
        // 如果执行 DML 操作，需要关闭 Statement 和 Connection
        // 如果执行 DQL 操作，需要关闭 ResultSet、Statement 和 Connection
        // 关闭之前首先判空，由大到小关闭
        if(statement != null && !statement.isClosed()) {
            statement.close();
        }
        if(connection != null && !connection.isClosed()) {
            statement.close();
        }
    }
}
```



## 1.3 JDBC 实现增删查改

> 是 JDBC 完成数据库的 CRUD （Create、Read、Update、Delete）

对 JDBC 的修改操作都是一样的，也就是说创建、更新、删除操作的步骤都是差不多的，修改的都是其中的 sql 语句，我们现在拿查询语句来举例：

查询语句在修改语句中有三处不同：

1. 加载、执行 sql 时采用的是 `statement.executeQuery` 来进行处理 DQL 语句

2. 打印查询的结果集使用 while 语句结合指针进行操作guan

   ```java
   while(rs.next()) {
       String name = rs.getString("name");
       int money = rs.getInt("money");
       double price  = rs.getDouble("price");
       System.out.println(name + "\t" + money + "\t" + price);
   }
   ```

3. 关闭多增加对查询的结果集的关闭

   ```java
   if(rs != null && !rs.isClosed()) {
       rs.close();
   }
   ```

完整代码如下：

```java
package com.xzh.demo1;

import java.sql.*;

public class JDBCDemo {
    public static void main(String[] args) throws ClassNotFoundException, SQLException {
        String url = "jdbc:mysql://localhost:3306/test_db"; // 包括数据库协议和
        String userName = "root";
        String password = "486101620";



        Class.forName("com.mysql.cj.jdbc.Driver");

        Connection connection = DriverManager.getConnection(url, userName, password);

        String sql = "select name, money from account";
        Statement statement = connection.createStatement();
        ResultSet rs = statement.executeQuery(sql);

while(rs.next()) {
    String name = rs.getString("name");
    int money = rs.getInt("money");
    System.out.println(name + "\t" + money);
}

if(rs != null && !rs.isClosed()) {
    rs.close();
}
        if(statement != null && !statement.isClosed()) {
            statement.close();
        }
        if(connection != null && !connection.isClosed()) {
            statement.close();
        }
    }
}
```



## 1.4 JDBC 核心类与接口

JDBC 的核心类与接口主要有以下几类：

1. java.sql.DriverManager 类：驱动管理器
2. java.sql.Connection 接口：数据库连接
3. java.sql.Statement 接口：SQL 指令的加载/执行器
4. java.sql.ResultSet 接口：结果集



### 1.4.1 DriverManager

作用：

1. 加载驱动（注册驱动）

   <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221111223944432.png" alt="image-20221111223944432" style="zoom:20%;" />

   在 Driver 类中的静态初始化块中，注册驱动，即之间的第一行代码

   ```java
   Class.forName("com.mysql.cj.jdbc.Driver");
   // 这行手动注册驱动的代码也可以不写，如果我们没有手动注册驱动，驱动管理器在获取连接的时候会自动在 servericejava.sql.Driver 文件配置驱动类路径进行注册
   ```

   会在加载的同时，注册驱动

2. 创建数据库连接，即通过 `getConnection` 来建立与数据库之间的连接，里面有三个参数

   - url：数据库服务器的地址
   - username：对数据库连接用户名
   - password：数据库连接密码



### 1.4.2 Connection 接口

Connection 对象表示 <font color="red">Java 应用程序与数据库之间的连接</font>，其作用有：

1. 通过 Connection 接口对象，获取执行 SQL 语句的 Statement 对象

   ```java
   Statement statement = connection.createStatemenet();
   // statement 接口编译执行静态 sql 指令
   ```

   除了可以得或 Statement 对象，还可以得到 PreparedStatement 对象，该对象可以解决 ==sql 注入==的问题 ：

   - PreparedStatement接口继承了 statement 接口，预编译执行动态 sql 指令

     ```
     PreparedStatement preparedStatement = connection.prepareStatement(sql);
     ```

   - CallableStatement 接口：继承了 PreparedStatement 接口，可以==调用存储过程==

     ```java
     CallabeleStatement callableStatement = connection.prepareCall(sql);
     ```

2. 完成数据库的事务管理

   ```java
   // 开启事务（关闭事务自动提交）
   connection.setAutoCommit(false);
   
   // 事务回滚
   connection.rollback();
   
   // 提交事务
   connection.commit();
   ```



### 1.4.3 Statement 接口

> 用于编译、执行 sql 指令

statement 接口的具体作用如下图：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221111233943654.png" alt="image-20221111233943654" style="zoom:50%;" />

首先，我们要明白一件事，那就是 sql 指令分为<u>静态指令</u>和<u>动态指令</u>，他们的区别主要在于：一个参数值确定，另一个不确定

- 静态 sql 指令：`delete from books where book_id = 1`

- 动态 sql 指令：`delete from books where book_d = ?`

  > 这个 `?` 就是一个占位符

<font color="red">像 Statemet 接口只能实现静态的 sql 指令，而 PreparedStatement 接口可以实现动态的 sql 指令</font>

而 CallableStatement 接口用于**预编译**带有**参数占位符**的**调用存储过程**的**动态 sql**



### 1.4.4 ResultSet 接口

> resultSet 接口对象，表示查询操作返回的结果集，提供了便利的方法用于获取结果集中的数据

- 判断结果集中是否还有数据未取出

  ```java
  Result rs = statement.executeQuery(select_statement);
  
  // 判断 rs 中是否还有数据
  rs.next();
  ```

- 获取 rs 指向的结果集中某行的数据

  ```java
  int id = rs.getInt(String columnLabel);	// 通过列标签获取
  int id = rs.getInt(int columnIndex);	// 通过索引号获取
  
  // 通过 rs 获取特定类型的数据
  rs.getString();
  rs.getDouble();
  …
  rs.getDate();
  rs.getInt();
  ```

:bell:结果集关系的是当前 java 程序中 sql 查询的结果还是数据库中相对应的全部数据？

```
答：是当前 java 程序中 sql 查询的结果。假如当前 sql 语句中使用了别名或者只查询部分字段，那么我们只能通过该字段或只能在有的字段范围内查询使用
```



## 1.5 sql 注入

:question:什么是 sql 注入？

在 JDBC 操作 SQL 指令编写的过程中，如果 SQL 指令中需要数据，我们可以通过字符串的形式将参数拼接到 SQL 指令中，如果 String sql = "delete from books where book_id = " + s;(s 就是拼接到 sql 中的变量)；

使用<font color="red">字符串拼接变量的形式来设置 sql 语句中的数据，可能会导致因变量值的改变而引起 SQL 指令的原意发生改变</font>

例如：

- 如果 s 的值为 "1"，则 SQL 指令没有问题
- 如果 s 的值为 "1 or 1 = 1"，此时 where 条件恒满足，会将 books 表中所有的数据全部删除掉，这是一个很严重的错误

:bell:**解决的 SQL 注入的方法!**

<font color="green">使用 PreparedStatement 进行 SQL 预编译来解决该问题</font>

- 在编写 sql 指令时，如果指令中需要参数，一律使用 `?` 参数占位符
- 如果 sql 中有 `?` ，在 JDBC 操作步骤中不再使用 Statement，而是从 Connection 对象中获取 prepareStatement

- 预编译完成之后，通过 PreparedStatement 对象给预编译后的 SQL 指令的 `?` 赋值
  - `preparedStatement.setInt(参数占位符序号, 值);`
  - `preparedStatement.setString(参数占位符序号, 值);`
- SQL 指令中的所有 `?` 完成赋值之后，通过 PreparedStatement 执行 SQL 时不再加载 SQL
  - `int i = preparedStatementUpdate();`

改动如下：

1. 编写 SQL 指令

   ```java
   String sql = "insert into account(id, name, money) values(?,?,?)";
   ```

2. 加载 SQL 指令

   ```java
   PreparedStatement preparedStatement = connection.prepareStatement(sql); // 预编译
   preparedStatement.setInt(1, testId);
   preparedStatement.setString(2, testName);
   preparedStatement.setInt(3, testMoney);
   ```

3. 执行 sql 语句获取结果(执行的时候不再加载 sql)

   ```java
   int i = preparedStatement.executeUpdate();
   ```



完整代码示例如下：

```java
package com.xzh.demo1;

import java.sql.*;

public class JDBCDemo {
    public static void main(String[] args) throws ClassNotFoundException, SQLException {
        String url = "jdbc:mysql://localhost:3306/test_db"; // 包括数据库协议和
        String userName = "root";
        String password = "486101620";

        // 准备插入的数据
        int testId = 4;
        String testName = "赵六";
        int testMoney = 3000;
        Class.forName("com.mysql.cj.jdbc.Driver");

        Connection connection = DriverManager.getConnection(url, userName, password);

        String sql = "insert into account(id, name, money) values(?,?,?)";
        //Statement statement = connection.createStatement();
        PreparedStatement preparedStatement = connection.prepareStatement(sql); // 预编译
        preparedStatement.setInt(1, testId);
        preparedStatement.setString(2, testName);
        preparedStatement.setInt(3, testMoney);
        
        int i = preparedStatement.executeUpdate();  // 如果 i大于0 ，则表示插入成功，如果 i=0 表示插入失败

        if (i > 0) {
            System.out.println("插入成功");
        } else {
            System.out.println("插入失败");
        }
        if(preparedStatement != null && !preparedStatement.isClosed()) {
           preparedStatement.close();
        }
        if(connection != null && !connection.isClosed()) {
            connection.close();
        }
    }
}
```



## 1.6 工具类的封装

在之前的学习中，我们了解到从 JDBC 编程具有一个固定步骤，如下：

1. <font color="blue">注册驱动</font>
2. <font color="blue">创建连接</font>
3. 编写 sql
4. 获取 Statement 对象
5. 执行 sql
6. 处理事务
7. <font color="blue">关闭连接</font>

在这些步骤中，第 1、2、6、7 步都是代码是几乎固定的，为了提高的复用性，我们可以考虑将其封装成为一个<font color="red">工具类（DBHelper）</font>来使用

封装类如下：

```java
package com.xzh.demo1;

import java.sql.*;

public class DBHelper {
    /*
    * 将注册驱动与创建进行封装
    * 返回数据库连接对象
    * @return
    */
    public static Connection getConnection() {
        Connection con = null;
        try {
            Class.forName("com.mysql.cj.jdbc.Driver");
            String url = "jdbc:mysql://localhost:3306/test_db?characterEncoding=utf8";
            con = DriverManager.getConnection(url, "root", "486101620");
        } catch (ClassNotFoundException e) {
            //throw new RuntimeException(e);
            System.out.println("-------注册驱动失败-------");
        } catch (SQLException e) {
            //throw new RuntimeException(e);
            System.out.println("-------注册驱动失败-------");
        }
        return con;
    }

    /*
    * 关闭连接
    */
    public static void close(Statement statement, Connection connection) {
        try {
            if (statement != null && !statement.isClosed()) {
                statement.close();
            }
            if (connection != null && !connection.isClosed()) {
                connection.close();
            }
        } catch (SQLException e) {
            //throw new RuntimeException(e);
            System.out.println("关闭数据库失败！");
        }
    }

    public static void close(PreparedStatement preparedStatement, Connection connection) {
        try {
            if (preparedStatement != null && !preparedStatement.isClosed()) {
                preparedStatement.close();
            }
            if (connection != null && !connection.isClosed()) {
                connection.close();
            }
        } catch (SQLException e) {
            //throw new RuntimeException(e);
            System.out.println("关闭数据库失败！");
        }
    }

    public static void close(ResultSet resultSet, PreparedStatement preparedStatement, Connection connection) {
        try {
            if (resultSet != null && resultSet.isClosed()) {
                resultSet.close();
            }
            if (preparedStatement != null && !preparedStatement.isClosed()) {
                preparedStatement.close();
            }
            if (connection != null && !connection.isClosed()) {
                connection.close();
            }
        } catch (SQLException e) {
            //throw new RuntimeException(e);
            System.out.println("关闭数据库失败！");
        }
    }

    public static void close(ResultSet resultSet, Statement statement, Connection connection) {
        try {
            if (resultSet != null && resultSet.isClosed()) {
                resultSet.close();
            }
            if (statement != null && !statement.isClosed()) {
                statement.close();
            }
            if (connection != null && !connection.isClosed()) {
                connection.close();
            }
        } catch (SQLException e) {
            //throw new RuntimeException(e);
            System.out.println("关闭数据库失败！");
        }
    }
}

```

> 这里我们直接将其与数据库连接的文件放在一起，其实最好还是在 src 下单独创建一个 utils 包，用来存放封装类

当我们使用的时候，直接调用这个包即可：

```java
// 1. 注册驱动并且创建连接
Connection connection = DBHelper.getConnection();

// 2. 关闭连接
DBHelper.close(preparedStatement, connection);
```

**封装类的改进**

1. 将创建数据库所需的参数定义为常量，集中管理

   ```java
   public static final String DRIVER = "com.mysql.cj.jdbc.Driver";
   public static final String URL = "jdbc:mysql://localhost:3306/test_db?characterEncoding=utf8";
   public static final String USER = "root";
   public static final String PASSWORD = "486101620";
   ```

2. 注册驱动只需执行一次，因此我们放在帮助类的静态化初始模块中完成

   ```
   static {
       try {
           Class.forName(DRIVER);
       } catch(ClassNotFoundException e) {
           System.out.println("注册驱动失败");
       }
   }
   ```

   > 如果操作之后，只在 DBHelper 这个模块被加载的时候进行注册一次，否则像之前那么写，每次在创建连接的时候都要进行注册，浪费资源

3. 使用 Statement 接口作为参数，既可以传递 Statement 接口对象，也可以传递 PreparedStatement 对象（PreparedStatement 对象继承自 Statement）

   ```java
   public static void close(Statement statement, Connection connection) {
       close(null, statement, connection);
   }
   
   public static void close(ResultSet resultSet, Statement statement, Connection connection) {
       try {
           if (resultSet != null && resultSet.isClosed()) {
               resultSet.close();
           }
           if (statement != null && !statement.isClosed()) {
               statement.close();
           }
           if (connection != null && !connection.isClosed()) {
               connection.close();
           }
       } catch (SQLException e) {
           //throw new RuntimeException(e);
           System.out.println("关闭数据库失败！");
       }
   }
   ```



**改进版如下：**

```java
package com.xzh.demo1;

import java.sql.*;

public class DBHelper {
    /*
    * 将注册驱动与创建进行封装
    * 返回数据库连接对象
    * @return
    */
    public static final String DRIVER = "com.mysql.cj.jdbc.Driver";
    public static final String URL = "jdbc:mysql://localhost:3306/test_db?characterEncoding=utf8";
    public static final String USER = "root";
    public static final String PASSWORD = "486101620";
static {
    try {
        Class.forName(DRIVER);
    } catch(ClassNotFoundException e) {
        System.out.println("注册驱动失败");
    }
}
    public static Connection getConnection() {
        Connection con = null;
        try {
            con = DriverManager.getConnection(URL, USER, PASSWORD);
        } catch (SQLException e) {
            //throw new RuntimeException(e);
            System.out.println("-------注册驱动失败-------");
        }
        return con;
    }

    /*
    * 关闭连接
    */
public static void close(Statement statement, Connection connection) {
    close(null, statement, connection);
}

public static void close(ResultSet resultSet, Statement statement, Connection connection) {
    try {
        if (resultSet != null && resultSet.isClosed()) {
            resultSet.close();
        }
        if (statement != null && !statement.isClosed()) {
            statement.close();
        }
        if (connection != null && !connection.isClosed()) {
            connection.close();
        }
    } catch (SQLException e) {
        //throw new RuntimeException(e);
        System.out.println("关闭数据库失败！");
    }
}
}
```



## 1.7 DAO 和 DTO 的封装

- DAO：Data Access Object ，即数据访问对象
- DTO：Data Transfer Object，即数据传输对象

### 1.7.1 CRUD 方法的封装

###  
