[toc]

# 1 引言

## 1.1 为什么需要 Jdbc template?

实际需求：如果程序员希望使用 spring 框架来做项目，那么我们应该如何处理对数据库的操作？

1. 方案1： 使用我们在前面项目开发时使用的 JdbcUtils 工具类

2. 方案2：使用 spring 自带的 Jdbc template 来完成对数据库表的操作

   > 没错，spring 提供给了我们一个功能强大的操作数据库的类



## 1.2 基本介绍

**基本构造器**

| `JdbcTemplate()`Construct a new JdbcTemplate for bean usage. |
| ------------------------------------------------------------ |
| `JdbcTemplate(DataSource dataSource)`Construct a new JdbcTemplate, given a DataSource to obtain connections from. |
| `JdbcTemplate(DataSource dataSource, boolean lazyInit)`      |

> JdbcTemplate 构造器我们可以传入数据源 dataSource，也可以指定懒加载 lazyint



**JDBC 环境搭建**

1. 引入 JdbcTemplate 所需的依赖

   ```xml
   <!-- 数据库连接池 -->
   <dependency>
       <groupId>c3p0</groupId>
       <artifactId>c3p0</artifactId>
       <version>0.9.1.2</version>
   </dependency>
   
   <!-- mysql驱动 -->
   <dependency>
       <groupId>mysql</groupId>
       <artifactId>mysql-connector-java</artifactId>
       <version>8.0.33</version>
   </dependency>
   
   <!-- spring-jdbc，里面含有 JdbcTemplate 类 -->
   <dependency>
       <groupId>org.springframework</groupId>
       <artifactId>spring-jdbc</artifactId>
       <version>5.3.8</version>
   </dependency>
   
   <!-- 用于处理对象关系映射-->
   <dependency>
       <groupId>org.springframework</groupId>
       <artifactId>spring-orm</artifactId>
       <version>5.3.8</version>
   </dependency>
   
   <!-- 事务处理 -->
   <dependency>
       <groupId>org.springframework</groupId>
       <artifactId>spring-tx</artifactId>
       <version>5.3.8</version>
   </dependency>
   ```

2. 创建 spring 数据库和 monster 表

   ```sql
   # 创建 spring 数据库
   CREATE DATABASE spring;
   
   USE spring;
   
   # 创建 monster 表
   CREATE TABLE monster(
   	`id` INT PRIMARY KEY,
   	`name` VARCHAR(64) NOT NULL DEFAULT '',
   	`skill` VARCHAR(64) NOT NULL DEFAULT ''
   );
   
   INSERT INTO monster VALUES(100, '青牛怪', '吐火');
   INSERT INTO monster VALUES(200, '黄袍怪', '吐烟');
   INSERT INTO monster VALUES(300, '蜘蛛精', '吐丝');
   ```

3. 创建配置文件 jdbc.properties 和 jdbcTemplate_ioc.xml

   ```properties
   # jdbc.properties
   jdbc.user=root
   jdbc.password=486101620
   jdbc.driverClass=com.mysql.cj.jdbc.Driver
   jdbc.jdbcUrl=jdbc:mysql://localhost:3306/spring
   ```

   ```xml
   <!-- jdbcTemplate_ioc.xml -->
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
       <!--引入外部的 jdbc.properties 文件-->
       <context:property-placeholder location="classpath:jdbc.properties" />
       <!--配置数据源对象-DataSource-->
       <bean class="com.mchange.v2.c3p0.ComboPooledDataSource" id="dataSource">
           <property name="user" value="${jdbc.user}" />
           <property name="password" value="${jdbc.password}" />
           <property name="driverClass" value="${jdbc.driverClass}" />
           <property name="jdbcUrl" value="${jdbc.jdbcUrl}" />
       </bean>
   </beans>
   ```



# 2 JdbcTemplate 的基本使用

## 2.1 添加数据

在添加数据之前，我们首先需要配置好我们的 JdbcTemplate 对象（前面我们只配置了 DataSource ，而没有配置 jdbcTemplate 对象）

```xml
<!--配置数据源对象-DataSource-->
<bean class="com.mchange.v2.c3p0.ComboPooledDataSource" id="dataSource">
    <property name="user" value="${jdbc.user}" />
    <property name="password" value="${jdbc.password}" />
    <property name="driverClass" value="${jdbc.driverClass}" />
    <property name="jdbcUrl" value="${jdbc.jdbcUrl}" />
</bean>

<!--配置 jdbcTemplate 对象-->
<bean class="org.springframework.jdbc.core.JdbcTemplate" id="jdbcTemplate">
    <!--为我们的 JdbcTemplate 对象配置 dataSource-->
    <property name="dataSource" ref="dataSource" />
</bean>
```

**测试环境**

```java
// 1.  获取 ioc 容器
ApplicationContext ioc = new ClassPathXmlApplicationContext("jdbcTemplate_ioc.xml");
// 2.获取 jdbcTemplate 对象
JdbcTemplate jdbcTemplate = ioc.getBean(JdbcTemplate.class);
// 3. 编写 sql 语句
String sql = "INSERT INTO monster VALUES(400, '红孩儿', '火枪');";
// 4. 调用 execute 方法
jdbcTemplate.execute(sql);
```

上面的添加方式不能防止 sql 注入的问题，如果需要防止产生 sql 注入，我们可以调用 update api 完成插入

```java
String sql = "INSERT INTO monster VALUES(?, ?, ?)";
int affected = jdbcTemplate.update(sql, 500, "白骨精", "美人计");
if(affected == 1) {
    System.out.println("执行成功");
} else {
    System.out.println("执行失败");
}
```



## 2.2 修改数据

```java
// 1. 获取 ioc 容器
ApplicationContext ioc = new ClassPathXmlApplicationContext("jdbcTemplate_ioc.xml");
// 2. 获取 jdbcTemplate 对象
JdbcTemplate jt = ioc.getBean(JdbcTemplate.class);

// 3. 编写 sql 语句
String sql = "UPDATE monster SET skill = ? WHERE id = ?";

// 4. 修改数据
int affected = jt.update(sql, "吐火", 400);
if(affected == 1) {
    System.out.println("修改成功");
} else {
    System.out.println("修改失败");
}
```

> 细节：这里即使数据的值没有发生改变，但是仍然会进行修改，并且返回的 affected(影响行数) 为 1



## 2.3 批量处理

```java
// 1. 获取 ioc 容器
ApplicationContext ioc = new ClassPathXmlApplicationContext("jdbcTemplate_ioc.xml");
// 2. 获取 jdbcTemplate 对象
JdbcTemplate jt = ioc.getBean(JdbcTemplate.class);

// 3. 编写 sql 语句
String sql = "INSERT INTO monster VALUES(?, ?, ?)";
// 4. 创建存放数据的数据结构
List<Object[]> batch = new ArrayList<Object[]>();
// 5. 添加 Object[]
batch.add(new Object[]{1, "老鼠精", "挖洞"});
batch.add(new Object[]{2, "老猫精", "抓老鼠"});

// 6. 调用 jt 的 api
int[] ints = jt.batchUpdate(sql, batch);
boolean res = true;
for(int aInt : ints) {
    if(aInt != 1) {
        res = false;
        break;
    }
}
if(res) {
    System.out.println("sql 批量执行完毕");
} else {
    System.out.println("sql 批量执行异常");
}
```



## 2.4 查询

### 2.4.1 将查询结果封装为一个 java bean

说明需求：<u>查询 id = 100 的 monster 记录并将其封装到 Monster 实体对象中</u>

首先，我们需要创建对应的 Bean 对象

```java
public class Monster {
    /**
     * 妖怪类的主键id
     */
    private Integer monsterId;
    /**
     * 姓名
     */
    private String name;
    /**
     * 技能
     */
    private String skill;
    
    // getter 、setter 、无参、全参构造器、toString 方法
}
```

实现将类的查询结果包装为 bean 实例

```java
// 1. 获取 ioc 容器
ApplicationContext ioc = new ClassPathXmlApplicationContext("jdbcTemplate_ioc.xml");
// 2. 获取 jdbcTemplate 对象
JdbcTemplate jt = ioc.getBean(JdbcTemplate.class);
// 3. 编写 sql 语句
String sql = "SELECT `id` AS `monsterId`, `name`, `skill` FROM monster WHERE `id` = 100";
// 4. 提供记录包装容器
RowMapper<Monster> rowMapper = new BeanPropertyRowMapper<>(Monster.class);
// 5. 调用api——queryForObject
Monster monster = jt.queryForObject(sql, rowMapper);
System.out.println(monster);
```



### 2.4.2 将查询结果封装为一个 java bean 集合

```java
// 1. 获取 ioc 容器
ApplicationContext ioc = new ClassPathXmlApplicationContext("jdbcTemplate_ioc.xml");
// 2. 获取 jdbcTemplate 对象
JdbcTemplate jt = ioc.getBean(JdbcTemplate.class);
// 3. 编写 sql 语句
String sql = "SELECT `id` AS `monsterId`, `name`, `skill` FROM monster WHERE `id` >= ?";
// 4. 提供 rowMapper
RowMapper<Monster> rowMapper = new BeanPropertyRowMapper<>(Monster.class);
// 5. 调用 api
List<Monster> monsterList = jt.query(sql, rowMapper, 200);

for(Monster m : monsterList) {
    System.out.println(m);
}
```

其实 jdbcTemplate 中还有其他的 api 可以将查询返回的结果封装到一个集合中，例如 queryForList



### 2.4.3 返回单行单列

查询返回结果只有单行单列，例如：查询 monsterId = 300 的妖怪的技能是什么?

```java
// 1. 获取 ioc 容器
ApplicationContext ioc = new ClassPathXmlApplicationContext("jdbcTemplate_ioc.xml");
// 2. 获取 jdbcTemplate 对象
JdbcTemplate jt = ioc.getBean(JdbcTemplate.class);
// 3. 编写 sql 语句
String sql = "SELECT `skill` FROM monster WHERE `id` = ?";
/**
 * 参数1: sql 语句
 * 参数2：返回数据类型
 * 参数3：...args, 占位参数
 */
String skill = jt.queryForObject(sql, String.class, 300);
System.out.println(skill);
```



## 2.5 具名参数

首先，我们在使用具名参数之前，需要配置 NamedParameterJdbcTemplate 对象，在 jdbcTemplate_ioc.xml 中配置：

```xml
<!--配置 NamedParameterJdbcTemplate 对象-->
<bean class="org.springframework.jdbc.core.namedparam.NamedParameterJdbcTemplate" id="namedParameterJdbcTemplate">
    <!--通过构造器，设置数据源-->
    <constructor-arg name="dataSource" ref="dataSource" />
</bean>
```

使用示例如下：

```java
ApplicationContext ioc = new ClassPathXmlApplicationContext("jdbcTemplate_ioc.xml");

// 得到 NameParameterJdbcTemplate bean
NamedParameterJdbcTemplate namedParameterJdbcTemplate = ioc.getBean(NamedParameterJdbcTemplate.class);

String sql = "INSERT INTO monster VALUES(:id, :name, :skill)";
Map<String, Object> paramMap = new HashMap<String, Object>();
paramMap.put("id", 800);
paramMap.put("name", "蛇精");
paramMap.put("skill", "法宝");

int update = namedParameterJdbcTemplate.update(sql, paramMap);
if(update == 1) {
    System.out.println("插入成功");
} else {
    System.out.println("插入失败");
}
```



## 2.6 使用 sqlparametersource

```java
ApplicationContext ioc = new ClassPathXmlApplicationContext("jdbcTemplate_ioc.xml");

// 得到 NameParameterJdbcTemplate bean
NamedParameterJdbcTemplate namedParameterJdbcTemplate = ioc.getBean(NamedParameterJdbcTemplate.class);

/**
 * 注意：使用 sqlParameterSource 这种方式的具名参数，需要保证具名参数名与java bean中的字段名保持一致
 */
String sql = "INSERT INTO monster VALUES(:monsterId, :name, :skill)";
Monster monster = new Monster(910, "乌龟精", "赛跑");
SqlParameterSource sqlParameterSource = new BeanPropertySqlParameterSource(monster);
int update = namedParameterJdbcTemplate.update(sql, sqlParameterSource);
if(update == 1) {
    System.out.println("插入成功");
} else {
    System.out.println("插入失败");
}
```





## 2.7 实战：DAO 中使用 jdbcTemplate

创建 MonsterDao

```java
@Repository
public class MonsterDao {
    @Resource
    private JdbcTemplate jt;

    /**
     * 保存
     */
    public int save(Monster monster) {
        String sql = "INSERT INTO monster VALUES(?, ?, ?)";
        return jt.update(sql, monster.getMonsterId(), monster.getName(), monster.getSkill());

    }
}
```

> 注意：一定要现在 xml 配置文件中设置扫描路径
>
> ```xml
> <!--配置要扫描的包-->
> <context:component-scan base-package="com.xzh.spring.jdbcTemplate" />
> ```

测试程序

```java
public class DaoTest {
    @Test
    public void saveTest() {
        ApplicationContext ioc = new ClassPathXmlApplicationContext("jdbcTemplate_ioc.xml");
        MonsterDao monsterDao = ioc.getBean(MonsterDao.class);
        Monster monster = new Monster(404, "黑客", "攻击电脑");
        int affected = monsterDao.save(monster);
        if(affected == 1) {
            System.out.println("插入成功");
        } else {
            System.out.println("插入失败");
        }
    }
}
```



# 3 事务

## 3.1 事务分类简述

1. 编程式事务

   ```java
   // 1. 获取连接
   Connection connection = JdbcUtils.getConnection();
   try {
       // 2. 先手动关闭自动提交
       connection.setAutoCommit(false);
       // 3. 进行一些 crud 操作
       // 4. 提交事务
       connection.commit();
   } catch(Exception e) {
       // 5. 捕获异常，进行回滚
       connection.rollback();
   } finally {
       // 6. 关闭连接
       connection.close();
   }
   ```

2. 声明式事务 ，即通过配置的方式，告诉 spring ，那些方法需要 spring 帮忙管理事务，开发者只需要关注业务代码，而事务则交给 spring 帮助我们进行控制



## 3.2 声明式事务实例

需求说明：用户购买商品

1. 通过商品 ID 获取价格
2. 购买商品（某人购买商品，修改用户的余额）
3. 修改库存量

环境搭建

```sql
-- 用户表
CREATE TABLE `user_account` (
  `user_id` int unsigned NOT NULL AUTO_INCREMENT,
  `user_name` varchar(32) NOT NULL DEFAULT '',
  `money` double unsigned NOT NULL DEFAULT '0',
  PRIMARY KEY (`user_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb3;

-- 商品表
CREATE TABLE `goods` (
	`goods_id` INT NOT NULL AUTO_INCREMENT,
	`goods_name` VARCHAR ( 32 ) NOT NULL DEFAULT '',
	`price` DOUBLE NOT NULL DEFAULT '0',
PRIMARY KEY ( `goods_id` ) 
) ENGINE = INNODB DEFAULT CHARSET = utf8mb3;

-- 商品库存表
CREATE TABLE `goods_amount` (
  `goods_id` int unsigned NOT NULL AUTO_INCREMENT,
  `goods_num` int unsigned NOT NULL DEFAULT '0',
  PRIMARY KEY (`goods_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb3;
```

```sql
INSERT INTO `user_account` VALUES(NULL, '张三', 1000);
INSERT INTO `user_account` VALUES(NULL, '李四', 2000);

INSERT INTO `goods` VALUES(NULL,'小风扇', 10.00); 
INSERT INTO `goods` VALUES(NULL,'小台灯', 12.00); 
INSERT INTO `goods` VALUES(NULL,'可口可乐', 3.00);

INSERT INTO `goods_amount` VALUES(1,200); 
INSERT INTO `goods_amount` VALUES(2,20); 
INSERT INTO `goods_amount` VALUES(3,15);
```



创建 dao 层和 service 层

```java
@Repository
public class GoodsDao {
    private final JdbcTemplate jt;

    public GoodsDao(JdbcTemplate jt) {
        this.jt = jt;
    }

    /**
     * 根据货物 id 获取价格
     */
    public Double queryPriceById(Integer id) {
        String sql = "SELECT `price` FROM goods WHERE `goods_id` = ?";
        return jt.queryForObject(sql, Double.class, id);
    }

    /**
     * 修改用户的账户余额
     */
    public void updateBalance(Integer id, Double money) {
        String sql  ="UPDATE user_account SET `money` = `money` - ? WHERE `user_id` = ?";
        jt.update(sql, money, id);
    }

    /**
     * 修改商品库存
     */
    public void updateStore(Integer id, int amount) {
        String sql = "UPDATE goods_amount SET `goods_num` = `goods_num` - ? WHERE `goods_id` = ?";
        jt.update(sql, amount, id);
    }
}
```

```java
@Service
public class GoodsService {
    private final GoodsDao goodsDao;

    public GoodsService(GoodsDao goodsDao) {
        this.goodsDao = goodsDao;
    }

    public void buyGoods(int userId, int goodsId, int amount) {
        // 1. 得到商品的价格
        Double price = goodsDao.queryPriceById(goodsId);
        // 2. 减少用户的余额
        goodsDao.updateBalance(userId, amount * price);
        // 3. 减少商品的库存
        goodsDao.updateStore(goodsId, amount);
    }
}
```

在 GoodsService 中还是我们以前的问题，在 `bugGoods()` 方法中，里面的三条对数据库的操作应该要么全部成功，要么全部失败，否则就会出现数据不可靠 的情况出现，换言之，==就是要我们保证 bugGoods 方法具有原子性==



如果实在 servlet 中，完成事务管理控制需要我们结合 filter + ThreadLocal，但是在 spring 中，实现事务的控制非常简单，只需要我们添加一个 `@Transactional` 注解即可，但是如果我们想要使用该注解之前，需要在 xml 配置文件中进行相应的配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context" xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd">
    <!--配置要扫描的包-->
    <context:component-scan base-package="com.xzh.spring.jdbcTemplate" />
    <!--引入外部的 jdbc.properties 文件-->
    <context:property-placeholder location="classpath:jdbc.properties" />
    <!--配置数据源对象-DataSource-->
    <bean class="com.mchange.v2.c3p0.ComboPooledDataSource" id="dataSource">
        <property name="user" value="${jdbc.user}" />
        <property name="password" value="${jdbc.password}" />
        <property name="driverClass" value="${jdbc.driverClass}" />
        <property name="jdbcUrl" value="${jdbc.jdbcUrl}" />
    </bean>

    <!--配置 jdbcTemplate 对象-->
    <bean class="org.springframework.jdbc.core.JdbcTemplate" id="jdbcTemplate">
        <!--为我们的 JdbcTemplate 对象配置 dataSource-->
        <property name="dataSource" ref="dataSource" />
    </bean>

    <!--配置 NamedParameterJdbcTemplate 对象-->
    <bean class="org.springframework.jdbc.core.namedparam.NamedParameterJdbcTemplate" id="namedParameterJdbcTemplate">
        <!--通过构造器，设置数据源-->
        <constructor-arg name="dataSource" ref="dataSource" />
    </bean>

    <!--
        配置事务管理器对象
        1. DataSourceTransactionManager 这个对象是进行事务管理
        2. 一定要配置数据源属性，这样才可以指定该事务管理器是对那个数据源进行事务控制
     -->
    <bean class="org.springframework.jdbc.datasource.DataSourceTransactionManager" id="transactionManager">
        <property name="dataSource" ref="dataSource" />
    </bean>

    <!--配置启用基于注解的声明式事务编程-->
    <tx:annotation-driven transaction-manager="transactionManager" />
</beans>
```

> 注意：`annotation-driven` 不要引入错了包，引入的命名空间路径为：`xmlns:tx="http://www.springframework.org/schema/tx"`



## 3.3 声明式事务的传播机制

### 3.3.1 引出问题

当有多个事务处理并存时，如何控制？例如用户去购买两次商品（使用不同的方法），每个方法都是一个事务，那么应该如何控制？

<img src="https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230729163000988.png" alt="image-20230729163000988" style="zoom: 67%;" />

<img src="https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230729163141741.png" alt="image-20230729163141741" style="zoom: 67%;" />



### 3.3.2 事务传播机制种类

| 传播属性          | 描述                                                         |
| ----------------- | ------------------------------------------------------------ |
| REQUIRED(default) | 如果有事务在运行，当前的方法就在这个事务内运行，否则，就启动一个新的事务，并在自己的事务内运行 |
| REQUIRED_NEW      | 当前的方法必须启动新事务，并在它自己的事务内运行，如果有事务正在运行，应该将其挂起 |
| SUPPORTS          | 如果有事务在运行，当前的方法就在这个事务内运行，否则他不可以不运行在事务中 |
| NOT_SUPPORTED     | 当前的方法不应该运行在事务中，如果有运行的事务，将它挂起     |
| MANDATORY         | 当前的方法必须运行在事务内部，如果没有正在运行的事务，就抛出异常 |
| NEVER             | 当前的方法不应该运行在事务中，如果有运行的事务，就抛出异常   |
| NESTED            | 如果有事务在运行，当前的方法就应该在这个事务的嵌套事务内运行，否则，就启动一个新的事物，并在它自己的事务内运行 |

在上表的七种声明式事务的传播机制中，最常用的是前面两种，即：`REQUIRED` 和 `REQUIRED_NEW`，对于前一种事务传播机制，事务中嵌套的子事务会成为一个原子，一旦子事务中出现异常，那么整个事务就会进行回滚；而后一种事务，每次执行到子事务时都会新开一个事务，原来的事务（包括父事务）也会被挂起，事务与事务之间相互独立，互不影响

使用方式如下：

```java
@Transactional(propagation = Propagation.REQUIRES_NEW)
public void buyGoods(int userId, int goodsId, int amount) {
    // 1. 得到商品的价格
    Double price = goodsDao.queryPriceById(goodsId);
    // 2. 减少用户的余额
    goodsDao.updateBalance(userId, amount * price);
    // 3. 减少商品的库存
    goodsDao.updateStore(goodsId, amount);
}
```



### 3.3.3 事务隔离级别

`Transactional` 注解中同样可以设置事务的隔离级别，我们查看源码，可以看到有哪些可配置属性，如下：

```java
public enum Isolation {
    DEFAULT(-1),
    READ_UNCOMMITTED(1),
    READ_COMMITTED(2),
    REPEATABLE_READ(4),
    SERIALIZABLE(8);

    private final int value;

    private Isolation(int value) {
        this.value = value;
    }

    public int value() {
        return this.value;
    }
}
```

```java
// demo
@Transactional(propagation = Propagation.REQUIRES_NEW, isolation = Isolation.DEFAULT)
```



### 3.3.4 超时回滚

基本介绍

- 如果一个事务执行的时间超过某个时间限制，就让事务回滚
- 可以直接通过设置事务的超时回滚来实现

```java
@Transactional(propagation = Propagation.REQUIRES_NEW, isolation = Isolation.DEFAULT, timeout = 5)
```

> `timeout` 默认是 -1

