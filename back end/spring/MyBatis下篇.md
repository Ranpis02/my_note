[toc]

# 1 原生 API 和注解

1.1 原生 API

前面我们是通过 sqlSession 的方式拿到 mapper 对象，从而对数据库进行操作，但是其实我们也可以通过操纵原生的 api 来实现

```java
public class MyNativeBatis {

    private SqlSession sqlSession;

    @Before
    public void init() {
        sqlSession = MybatisUtils.getSqlSession();
    }

    /**
     * 添加操作
     */
    @Test
    public void insertObj() {
        /**
         * int insert(String statement, Object parameter)
         *  statement 表示接口方法-完整声明
         *  parameter 入参
         */
        Monster monster = new Monster();
        monster.setName("松鼠精");
        monster.setAge(18);
        monster.setGender("0");
        monster.setBirthday(new Date());
        monster.setEmail("ranpis0@qq.com");
        monster.setSalary(100.0);
        int insert = sqlSession.insert("com.xzh.mapper.MonsterMapper.addMonster", monster);
    }

    /**
     * 删除操作
     */
    @Test
    public void delObj() {
        int delete = sqlSession.delete("com.xzh.mapper.MonsterMapper.delMonster", 10);
        if(delete == 1) {
            System.out.println("删除成功");
        }
    }

    /**
     * 查询一条记录
     */
    @Test
    public void querySingle() {
        sqlSession.selectOne("com.xzh.mapper.MonsterMapper.getMonsterById", 11);
    }

    /**
     * 查询多条记录
     */
    @Test
    public void queryAll() {
        List<Monster> monsters = sqlSession.selectList("com.xzh.mapper.MonsterMapper.getAllMonster");
        for(Monster m : monsters) {
            System.out.println(m);
        }
    }

    @After
    public void close() {
        sqlSession.commit();
        sqlSession.close();
    }

}
```

> 注意：即使我们使用的使原生的 API 来操作数据库，但是我们仍然不能缺少对应的 Mapper 和 xml 文件



## 1.2 注解方式

我们前面的操作方式是使用 xml 文件的方式进行配置，其实我们也可以使用注解的方式进行使用，使用方法如下：

01 添加 MonsterAnnotation 类

```java
public interface MonsterAnnotation {
    /**
     * 添加 monster
     */
    @Insert("insert into monster (name, age, gender, birthday, email, salary) values (#{name}, #{age}, #{gender}, #{birthday}, #{email}, #{salary})")
    public void addMonster(Monster monster);

    /**
     * 删除 monster
     */
    @Delete("DELETE FROM `monster` WHERE id = #{id}")
    public void delMonster(Integer id);

    /**
     * 修改 monster 信息
     */
    @Update("UPDATE `monster` " +
            "SET `name` = #{name}, " +
            "`age` = #{age}, " +
            "`gender` = #{gender}, " +
            "`birthday` = #{birthday}, " +
            "`email` = #{email}, " +
            "`salary` = #{salary} " +
            "WHERE " +
            "id = #{id}")
    public void updateMonster(Monster monster);

    /**
     * 根据 id 返回 monster 对象
     */
    @Select("SELECT * FROM `monster` WHERE id = #{id}")
    public Monster getMonsterById(Integer id);

    /**
     * 查询所有的 Monster
     */
    @Select("SELECT * FROM `monster`")
    public List<Monster> getAllMonster();
}
```

修改 mybatis-config.xml，对 MonsterAnnotation 进行注册

```xml
<configuration>
    <mappers>
         <mapper class="com.xzh.mapper.MonsterAnnotation" />
    </mappers>
</configuration>
```

最后我们来进行测试程序

```java
public class MonsterAnnotationTest {

    private SqlSession sqlSession;
    private MonsterAnnotation monsterAnnotation;

    @Before
    public void init() {
        sqlSession = MybatisUtils.getSqlSession();
        monsterAnnotation = sqlSession.getMapper(MonsterAnnotation.class);
    }

    @Test
    public void addMonster() {
        Monster monster = new Monster();
        monster.setName("蟑螂精");
        monster.setAge(18);
        monster.setGender("1");
        monster.setBirthday(new Date());
        monster.setEmail("ranpis0@qq.com");
        monster.setSalary((double) (10000));
        monsterAnnotation.addMonster(monster);
    }

    @Test
    public void deleteMonster() {
        monsterAnnotation.delMonster(3);
    }

    @Test
    public void updateMonster() {
        Monster monster = new Monster();
        monster.setId(4);
        monster.setName("蟑螂精");
        monster.setAge(18);
        monster.setGender("1");
        monster.setBirthday(new Date());
        monster.setEmail("ranpis0@qq.com");
        monster.setSalary((double) (10000));
        monsterAnnotation.updateMonster(monster);
    }

    @Test
    public void querySingle() {
        Monster monsterById = monsterAnnotation.getMonsterById(6);
        System.out.println(monsterById);
    }

    @Test
    public void queryMulti() {
        List<Monster> allMonster = monsterAnnotation.getAllMonster();
        for(Monster m : allMonster) {
            System.out.println(m);
        }
    }

    @After
    public void close() {
        if(sqlSession != null) {
            sqlSession.commit();
            sqlSession.close();
        }
    }
}
```



:warning:**注意事项**

1. 如果是通过注解的方式，我们就不需要使用 MonsterMapper.xml 文件，但需要在 mybatis-config.xml 文件中注册含注解的类 / 接口

2. 使用注解方式添加时，如果要返回自增长 id 值，可以使用 `@Option` 注解，组合使用。我们以 insert 插入语句为例：

   ```java
   @Insert("insert into monster (name, age, gender, birthday, email, salary) values (#{name}, #{age}, #{gender}, #{birthday}, #{email}, #{salary})")
   @Options(useGeneratedKeys = true, keyProperty = "id", keyColumn = "id")
   public void addMonster(Monster monster);
   ```

   > 注意：这里的返回 id 的值，是指我们在进行 DOM 操作之后可以在程序中获取 id 的值，而不是在打印日志中看到相关的信息



# 2 配置文件详解

**说明**

mybatis 的核心配置文件为 mybatis.config.xml，比如配置 jdbc 连接信息，注册 mapper 等等，我们需要对这个配置文件有详细的了解

配置文档地址：https://mybatis.org/mybatis-3/zh/configuration.html



## 2.1 引入外部配置文件配置属性

在 mybatis 中属性可以在外部进行配置，并可以进行动态替换

```xml
<!-- 配置硬编码 -->
<properties resource="org/mybatis/example/config.properties">
  <property name="username" value="dev_user"/>
  <property name="password" value="F2Fa3!33TYyg"/>
</properties>

<!-- 引入外部配置文件，动态替换 -->
<dataSource type="POOLED">
  <property name="driver" value="${driver}"/>
  <property name="url" value="${url}"/>
  <property name="username" value="${username}"/>
  <property name="password" value="${password}"/>
</dataSource>
```

我们以之前的 dataSource 配置文件为例，我们在 resources 文件夹下添加如下配置

```properties
jdbc.driver=com.mysql.cj.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/mybatis?useSSL=true&useUnicode=true&characterEncoding=UTF-8
jdbc.user=root
jdbc.pwd=xxx
```

然后修改我们的 mybatis-config.xml 文件，如下：

```xml
<environments default="development">
    <environment id="development">
        <!--配置事务管理器-->
        <transactionManager type="JDBC"/>
        <!--配置数据源-->
        <dataSource type="POOLED">
            <property name="driver" value="${jdbc.driver}"/>
            <property name="url" value="${jdbc.url}"/>
            <property name="username" value="${jdbc.user}"/>
            <property name="password" value="${jdbc.pwd}"/>
        </dataSource>
    </environment>
</environments>
```

**注意事项**

注意在 rebuild project 之后，观察在 classes 文件夹中是否存在 jdbc.properties 文件，如果没有，那么我们需要在父工程中添加如下的资源插件，

```xml
<build>
    <resources>
        <resource>
            <!-- 将 src/main/java 目录的后缀为 properties 和 xml 文件加载到 classes 文件夹中 -->
            <directory>src/main/java</directory>
            <includes>
                <include>**/*.properties</include>
                <include>**/*.xml</include>
            </includes>
            <!--<filtering>false</filtering>-->
        </resource>
		
         <!-- 将 src/main/resources 目录的后缀为 properties 和 xml 文件加载到 classes 文件夹中 -->
        <resource>
            <directory>src/main/resources</directory>
            <includes>
                <include>**/*.properties</include>
                <include>**/*.xml</include>
            </includes>
            <!--<filtering>false</filtering>-->
        </resource>
    </resources>
</build>
```

注意 properties 的文件编码是否为 UTF-8 ，防止出现乱码

![image-20230805233104058](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230805233104058.png)



## 2.2 setting 全局参数设置

setting 全局参数设置中含有非常多的配置，我们可以直接在官网上进行查看

官网地址：https://mybatis.org/mybatis-3/zh/configuration.html#settings



## 2.3 typeAlises 别名处理器

配置别名的目的其实就是为了简化处理，防止出现大量冗长且重复的类的配置

例如，我们之前在 mybatis-config.xml 中添加如下配置用来简化类路径的长度

```xml
<typeAliases>
    <typeAlias type="com.xzh.entity.Monster" alias="Monster" />
</typeAliases>
```

但是考虑一下，如果 entity 下面有很多类的，例如 Person、Animal 类，那么我们当然可以想如下一样配置

```xml
<typeAliases>
    <typeAlias type="com.xzh.entity.Monster" alias="Monster" />
    <typeAlias type="com.xzh.entity.Person" alias="Person" />
    <typeAlias type="com.xzh.entity.Animal" alias="Animal" />
    <!-- …… -->
</typeAliases>
```

当然，还有一种更为简便的方式，如下：

```xml
<typeAliases>
    <package name="com.xzh.entity" />
</typeAliases>
```

这就可以直接使用类名替代掉类路径



## 2.4 类型处理器

typeHandlers 主要是帮助我们完成 Java 类型到 JDBC 类型的映射

不过 typeHandlers 一般我们不需要自己手动配置，mybatis 底层已经帮助我们配置了相应默认类型转换

我们可以查看官网查看具体的类型转换的关系：https://mybatis.org/mybatis-3/zh/configuration.html#typeHandlers



## 2.5 environments 环境的配置

常用配置如下：

- resource：用于注册 Mapper 文件

  ```xml
  <mappers>
      <mapper resource="com/xzh/mapper/xml/MonsterMapper.xml"/>
  </mappers>
  ```

- class：接口注解实现

  ```xml
  <mappers>
       <mapper class="com.xzh.mapper.MonsterAnnotation" />
  </mappers>
  ```

- url：外部路径，使用较少，不推荐，例如：

  ```xml
  <mappers>
      <mapper url="file:///D:xxx.xml" />
  </mappers>
  ```

- package 方式注册

  ```java
  <mappers>
      <!--
          1.当一个包下有很多的 mapper.xml 文件和基于注解实现的接口时，为了方便，我们可以以包的形式进行注册；
          2. 将下面的所有 xml 文件和注解接口都进行注册
      -->
      <package name="com.xzh.mapper" />
  </mappers>
  ```
  



# 3 XML 映射器

MyBatis 强大的地方在于它的语句映射器，利用 XML 映射器可以帮助我们减少 95% 的代码（官网说的）

SQL 映射文件常用的几个顶级元素：

- cache：该命名空间的缓存配置
- cache-ref：引用其他命名空间的缓存配置
- resultMap：描述如何从数据库结果集中加载对象，即返回的数据类型
- parameterType：传入这条语句的参数的类全限定名或别名
- sql：可被其他语句引用的可重用语句块
- insert：映射插入语句
- update：映射更新语句
- delete：映射删除语句
- select：映射查询语句



## 3.1 parameterType —— 输入参数类型

1. 传入简单类型，例如通过 id 查询 Monster
2. 传入 POJO 类型，查询时需要多个筛选条件
3. 当有多个条件时，传入的参数就是 POJO 类型的 Java 对象，比如这里的 Monster
4. 当传入的参数类时 String 时，也可以使用 `${}` 来接收参数



**示例**

01 多条件筛选

```xml
<mapper namespace="com.xzh.mapper.MonsterMapper">
    <select id="getMonsterByNameORId" parameterType="Monster" resultType="Monster">
        SELECT * FROM `monster` WHERE `id` = #{id} OR `name` = #{name}
    </select>
</mapper>
```



02 模糊查询

```xml
<!--
    在模糊查询中，我们取值需要使用 ${value} 进行解析
-->
<select id="getMonsterByName" parameterType="String" resultType="Monster">
    SELECT * FROM `monster` WHERE `name` LIKE '%${name}%'
</select>
```



03 HaspMap 传参

HashMap 传入相较于普通传参更加灵活，比如可以灵活地增加查询的属性，二部受限 pojo 本身，示例如下：

示例1：声明一个方法，按传入参数是 HashMap 的方式，查询 id > 10 并且 salary 大于 40 的所有妖怪

```java
/**
 * mapper 接口配置内容
 * 查询 id > 10, 并且 salary 大于 40，要求传入的参数是 HashMap
 */
public List<Monster> getMonsterByHashMap(Map<String, Object> mp);
```

```xml
<!--
	MonsterMapper.xml 配置内容
    1. 查询 id > 10 并且 salary 大于 40，要求传入的参数是 HashMap
    2. 如果以 map 形式传入参数，那么下述的 id 和 salary 就属于 Map 里面的键
-->
<select id="getMonsterByHashMap" parameterType="map" resultType="Monster">
    SELECT * FROM `monster` WHERE `id` > #{id} AND `salary` > #{salary}
</select>
```

**测试程序**

```java
@Test
public void getMonsterByHashMap() {
    Map<String, Object> mp = new HashMap<>();
    /**
     * 这里如果出现 Diamond types are not supported at language level '5'
     * 那么我们需要在 pom.xml 手动配置 jdk 和 maven 的版本
     *     <properties>
     *         <maven.compiler.source>8</maven.compiler.source>
     *         <maven.compiler.target>8</maven.compiler.target>
     *         <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
     *         <java.version>1.8</java.version>
     *     </properties>
     */
    mp.put("id", 10);
    mp.put("salary", 40);
    List<Monster> monsters = monsterMapper.getMonsterByHashMap(mp);
    for (Monster monster : monsters) {
        System.out.println(monster);
    }

}
```



示例2：遇到传入和返回的类型都是 HashMap

需求：我们还以示例1的需求为例，要求返回的结果以 map 的形式返回，即 "name=xxx, age=xxx" 这种形式存储

```xml
/**
 *  查询 id > 10, 并且 salary 大于 40，要求传入的参数是 HashMap，返回的类型也是 Map
 */
public List<Map<String, Object>> getMonsterByReturnMap(Map<String, Object> mp);
```

```xml
<!-- MonsterMapper.xml -->
<select id="getMonsterByReturnMap" parameterType="map" resultType="map">
    SELECT * FROM `monster` WHERE `id` > #{id} AND `salary` > #{salary}
</select>
```

```java
// 测试程序
@Test
public void getMonsterByReturnMap() {
    Map<String, Object> mp = new HashMap<>();
    mp.put("id", 10);
    mp.put("salary", 40);
    List<Map<String, Object>> monsters = monsterMapper.getMonsterByReturnMap(mp);
    for(Map<String, Object> monsterMap : monsters) {
        //System.out.println(monsterMap);

        //Set<String> keys = monsterMap.keySet();
        //for (String key : keys) {
        //    System.out.println(key + "-" + monsterMap.get(key));
        //}

        for(Map.Entry<String, Object> entry : monsterMap.entrySet()) {
            System.out.println(entry.getKey() + "=>" + entry.getValue());
        }
    }

}
```



## 3.2 resultMap 结果集映射

基本介绍：当实体类的属性和表的字段名字不一致时，我们可以通过 resultMap 进行映射，从而**屏蔽实体类属性名和表的字段名的不同的问题**

**案例演示**

准备工作

01 创建 user 表

```sql
CREATE TABLE `user` (
	`user_id` INT NOT NULL AUTO_INCREMENT,
	`user_name` VARCHAR ( 255 ) CHARACTER 
	SET utf8mb3 NOT NULL DEFAULT '',
	`user_email` VARCHAR ( 255 ) CHARACTER 
	SET utf8mb3 COLLATE utf8mb3_general_ci NOT NULL DEFAULT '',
PRIMARY KEY ( `user_id` ) 
) ENGINE = INNODB DEFAULT CHARSET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci;
```



02 设置实体类

```java
public interface UserMapper {
    /**
     * 添加用户
     */
    public void addUser(User user);

    /**
     * 查询所有的用户
     */
    public List<User> queryAllUser();
}
```



03  添加对应的 mapper 文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.xzh.mapper.UserMapper">
    <!--
        完成添加用户的映射配置
    -->
    <select id="addUser" parameterType="User">
        INSERT INTO `user`(`user_name`, `user_email`) VALUES(#{userName}, #{userEmail})
    </select>

    <!--
        完成查询所有用户信息的映射配置
    -->
    <select id="queryAllUser" resultType="User">
        <!-- 1. 这种方式虽然能够正常运行，但是由于 userName 与 userEmail 字段与库表字段对不上，所以无法获取值-->
        <!--select * from user-->
        <!--2. 通过别名的方式可以正常获取对应的属性值-->
        <!--SELECT user_id, user_name userName, user_email userEmail FROM `user`-->
    </select>
</mapper>
```

上述方法我们会用 resultType 进行获取属性值，除此之外，我们还可以使用 resultMap 来进行配置

```xml
<!--
    配置 resultMap 进行字段的映射，其中对于相同的字段，我们可以不需要进行设置,我们对其中一些参数进行解析：
    1. resultMap 中的 id 为标识，提供给 XML 映射器使用， type 表示映射的 entity 名
    2. result 标签中的 column 表示数据库中字段名，property 表示对应的 entity 属性名
-->
<resultMap id="queryAllUserMap" type="User">
    <result column="user_name" property="userName" />
    <result column="user_email" property="userEmail" />
</resultMap>

<select id="queryAllUser" resultMap="queryAllUserMap">
    SELECT * FROM `user`
</select>
```

注意事项

1. 解决表字段和对象属性名不一致，也支持使用字段别名

   > 虽然在官网上说  resultMap 比较强大，但是由于 resultMap 复用率很低而且代码相对比复杂，没有直接使用 resultType 简便，所以我们不推荐使用 resultMap 

2. 如果实体类有默认值，而我们又因为没有映射成功而得到值，那么则会返回默认值

3. 如果我们使用的使 MyBatis-plus，这个处理就比较简单，可以直接使用注解 `@TableField` 来解决实体字段名和表在字段名不一致的问题，还可以使用 `@TableName` 来解决实体类名与表名不一致的问题



# 4 动态 sql 语句

## 4.1 基本介绍

我们为什么需要动态 SQL？

1. 动态 SQL 是 MyBatis 的强大特性之一
2. 使用 JDBC或者其他类似的框架，根据不同条件拼接 SQL 语句十分复杂，特别是遇到像是空格这类特殊符号额定处理
3. SQL 映射语句汇总强大的动态 SQL 语言可以很好地解决这个问题



动态 SQL 必要性

1. 比如我们查询 Monster 时，如果程序员输入的 age 不大于 0，那么我们的 sql 语句就不需要带 0

2. 更新 Monster 对象时，当我们没有设置新的属性值时就保持原来的值，如果设置了新值，才进行更新

   > 注意：这一点和以前很不一样，在以前我们无论是使用 jdbc template 还是原生的 jdbc ，无论值是否是新的，我们都会直接进行更新，并返回对应的影响行数



## 4.2 if 标签

引入需求：请查询 age 大于 10 的所有妖怪，如果程序员输入的 age 不大于 0，则输出所有的妖怪

```java
public interface MonsterMapper {
    /**
     * 根据 age 进行查询
     */
    public List<Monster> getMonsterByAge(@Param("age") Integer age);

}
```

```xml
<mapper namespace="com.xzh.mapper.MonsterMapper">
    <select id="getMonsterByAge" parameterType="Integer" resultType="Monster">
        select * from monster
        <!--下面注意，test 判断条件 age 我们通过 #{} 是无法获取的，如果想要获取，需要在 mapper 抽象方法中参数列表中添加 @Param(value = "")-->
        <if test="age >= 0">
            WHERE age > #{age}
        </if>
    </select>
</mapper>
```

> 注意事项
>
> 1. `@Param("age")` 完整的写法为 `@Param(value = "age")`
> 2. 如果注解中写的是 "agex"，那么在 if 判断标签中也应该写的是 "agex"，也就是说 `@Param` 实质上是将参数 age 存一份到别名 "age" 中，然后在  动态 sql 语句的 if 判断中使用
> 3. if 标签中如果判断为 true，那么则会将里面包裹的内容与外面的 sql 进行==拼接== 



##  4.3 where 标签

需求：查询 id 大于 20 的，并且名字是 "松鼠精" 的所有妖怪，如果名字为空或者输入的 id 小于 0，则不拼接相应的条件

```java
/**
 * 查询 id 大于20，并且名字是 "松鼠精" 的所有妖怪
 */
public List<Monster> getMonsterByAgeAndId(Monster monster);
```

```xml
<!--
    1. where 标签会自动帮助我们在动态 sql 部分添加上 where 语句
    2. if 判断中的 id、name 字段由于传入的使 Monster 对象，所以我们不需要过多操作，
    例如添加@Param 标签什么的，可以直接拿到，属性名默认为对象的字属性名
    3. if 判断里面包裹的 and 或者 or 或者其他 ，mybatis 底层会自动帮助我们去除多余的关键字
-->
<select id="getMonsterByAgeAndId" parameterType="Monster" resultType="Monster">
    select * from `monster`
    <where>
        <if test="id >= 0">
            and `id` > #{id}
        </if>
        <if test="name != '' || name != null" >
            and `name` = #{name}
        </if>
    </where>
</select>
```



## 4.4 choose、when、otherwise 标签

需求

1. 如果给的 name 不为空，就按名字查询妖怪
2. 如果指定 age > 0，就按 age 来查询妖怪
3. 如果前面两个条件都不满足，就默认查询 salary > 100 的

```java
public List<Monster> getMonsterByChoose(Map<String, Object> mp);
```

```xml
<select id="getMonsterByChoose" parameterType="map" resultType="Monster">
    select * from monster
    <choose>
        <when test="name != '' || name != null">
            where name = #{name}
        </when>
        <when test="id > 0">
            where age > #{age}
        </when>
        <otherwise>
            where salary > 100
        </otherwise>
    </choose>
</select>
```

```java
@Test
public void getMonsterByChoose() {
    Map<String, Object> mp = new HashMap<>();
    mp.put("name", "松鼠精");
    List<Monster> monsters = monsterMapper.getMonsterByChoose(mp);
    for (Monster monster : monsters) {
        System.out.println(monster);
    }
}
```



## 4.5 foreach 标签

动态 SQL 的另一个常见的使用场景就是对集合进行遍历，尤其是在构建 IN 条件语句的时候

示例：<u>查询 id 为 10、12、14 的妖怪</u>

```java
public List<Monster> getMonsterByForeach(Map<String, Object> mp);
```

```xml
<!--
    1. 入参 map 中会传入 k-v，其中 key 为 ids ，value 为 list 集合 [10, 12, 14]
-->
<select id="getMonsterByForeach" parameterType="map" resultType="Monster">
    SELECT * FROM monster
    <if test="ids != null and ids != ''">
        <where>
            id IN
            <!--
                1. WHERE id IN (10, 12, 14)
                2. foreach 标签中的 collection 标签集合名， item 为取出其中的一个元素，open 表示起始符号，close 表示
                结束符号，separator 表示分隔符，里面为分隔的内容
                3. 假如 map 中插入 Map.entry<String, Object>("ids", List<Integer>(1, 10, 12, 14))，那么经过下面的
                解析，就会成为 where id in (10, 12, 14)
            -->
            <foreach collection="ids" item="id" open="(" separator="," close=")">
                #{id}
            </foreach>
        </where>
    </if>
</select>
```

```java
// 测试程序
@Test
public void getMonsterByForeach() {
    Map<String, Object> mp = new HashMap<>();
    mp.put("ids", Arrays.asList(10, 12, 14));
    List<Monster> monsters = monsterMapper.getMonsterByForeach(mp);
    for (Monster monster : monsters) {
        System.out.println(monster);
    }
}
```



## 4.6 trim 标签

`trim` 标签可以替换一些关键字，例如如果要求按照名字查询妖怪，如果 sql 语句开头有 and | or  那么就替换为 where，但是如果碰上其他关键字，如果我们也希望能够实现动态替换的功能，那么我们就需要使用到 trim 标签

需求：现在我们以前面的一个需求为例，即查询所有 name 为 "松鼠精"，age > 20 的 Monster 集合，要求使用 trim 标签完成

```java
public List<Monster> getMonsterByTrim(Monster monster);
```

```xml
<select id="getMonsterByTrim" parameterType="Monster" resultType="Monster">
    select * from monster
    <!--注意：prefixOverrides 中不要进行格式化处理，例如 and | or | xxx 这种写法-->
    <trim prefix="where" prefixOverrides="and|or|xxx">
        <if test="name != null || name != ''">
            xxx name = #{name}
        </if>
        <if test="age > 0">
            and age > #{age}
        </if>
    </trim>
</select>
```

> 注意：动态 sql 只能处理第一条语句的关键字，无法处理后面语句的关键的智能转换

```java
@Test
public void getMonsterByTrim() {
    Monster monster = new Monster();
    monster.setName("松鼠精");
    monster.setAge(20);
    List<Monster> monsters = monsterMapper.getMonsterByTrim(monster);
    for (Monster m : monsters) {
        System.out.println(m);
    }
}
```



## 4.7 set 标签（重点）

需求：请对指定 id 的妖怪进行修改，如果没有设置新的属性，则保持原来的值

```java
public void updateMonster(Map<String, Object> mp);
```

```xml
<select id="updateMonster" parameterType="map" resultType="Monster">
    update monster
    <set>
        <if test="name != null || name != ''">
            name = #{name},
        </if>
        <if test="age != null || age >= 0 ">
            age = #{age},
        </if>
        <if test="gender != '0' || gender != '1'">
            gender = #{gender},
        </if>
        <if test="birthday != null || birthday != ''">
            birthday = #{birthday},
        </if>
        <if test="email !=  null || email != ''">
            email = #{email},
        </if>
        <if test="salary >= 0">
            salary = #{salary},
        </if>
    </set>
    where id = #{id}
</select>
```

> 注意：除了最后一条 set 语句，其他 set 语句里面的内容的最后必须添加 `,`，并且我们需要添加至少一条 set 语句，否则就会出现语法错误

```java
// 测试程序
@Test
public void updateMonster() {
    Map<String, Object> mp = new HashMap<>();
    mp.put("id", 23);
    mp.put("name", "zhangSan");
    mp.put("age", 10);
    mp.put("gender", "1");
    mp.put("birthday", new Date());
    mp.put("email", "2112@souhu.com");
    mp.put("salary", 1000.0);
    monsterMapper.updateMonster(mp);

    if(sqlSession != null) {
        sqlSession.commit();
        sqlSession.close();
    }
}
```



# 5 映射关系

## 5.1 一对一映射

### 5.1.1 方式一：级联查询

表与表之间一对一的关系是比较基础，下面我们以 person - idencard 两张表进行说明

01 搭建环境

```sql
# 创建 person 表
CREATE TABLE `person` (
	`id` INT NOT NULL AUTO_INCREMENT,
	`name` VARCHAR ( 32 ) CHARACTER 
	SET utf8mb3 NOT NULL DEFAULT '',
	`card_id` INT NOT NULL,
	PRIMARY KEY ( `id` ),
	KEY `iden` ( `card_id` ),
CONSTRAINT `iden` FOREIGN KEY ( `card_id` ) REFERENCES `idencard` ( `id` ) ON DELETE RESTRICT ON UPDATE RESTRICT 
) ENGINE = INNODB DEFAULT CHARSET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci;

# 创建 id_card 表
CREATE TABLE `id_card` (
	`id` INT NOT NULL AUTO_INCREMENT,
	`card_sn` VARCHAR ( 32 ) CHARACTER 
	SET utf8mb3 NOT NULL DEFAULT '',
	PRIMARY KEY ( `id` ),
KEY `card_sn` ( `card_sn` ) 
) ENGINE = INNODB AUTO_INCREMENT = 2 DEFAULT CHARSET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci;

-- 插入语句
INSERT INTO `idencard` VALUES(1, '12121313131');
INSERT INTO `person` VALUES(1, '张三', 1);
```



02 创建对应的 entity

```java
/**
 * @Description: 人类
 */
@Data
@NoArgsConstructor
@AllArgsConstructor
public class Person {
    /*主键id*/
    private int id;
    /*姓名*/
    private String name;
    /*身份证外键*/
    private IDCard card;
}

```

```java
/**
 * @Description: 身份证类
 */
@Data
@NoArgsConstructor
@AllArgsConstructor
public class IDCard {
    /*主键id*/
    private int id;

    /*身份证号码*/
    private String cardSn;
}
```



03 添加相应的 mapper 和 xml

```java
// IDCard 实体类对应的 mapper 接口
public interface IDCardMapper {

    /**
     * 根据 id 查询身份证序列号
     */
    public IDCard queryCardNumberByID(int id);
}
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.xzh.mapper.IDCardMapper">
    <select id="queryCardNumberByID" parameterType="int" resultType="IDCard">
        select id, card_sn cardSn from id_card where id = #{id}
    </select>
</mapper>
```

```java
// Person 实体类所对应的接口
public interface PersonMapper {
    /**
     * 通过person的id获取到person,包括这个person关联的IDCard对象【级联查询】
     */
    public Person queryPersonByID(int id);
}
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.xzh.mapper.PersonMapper">
    <resultMap id="PersonResultMap"  type="Person">
        <result property="id" column="id" />
        <result property="name" column="name" />
        <!-- assocation 用来指派复杂类型 -->
        <association property="card" javaType="IDCard">
            <result property="id" column="id" />
            <result property="cardSn" column="card_sn" />
        </association>
    </resultMap>

    <select id="queryPersonByID" parameterType="int" resultMap="PersonResultMap">
        SELECT
        *
        FROM
        person,
        id_card
        WHERE
        person.card_id = id_card.id
        AND `person`.id = #{id};
    </select>
</mapper>
```

:herb:拓展

mybatis 中可以通过设置 `id` 来提供性能，类似于 mysql 中的索引，我们可以将上述 PersonMapper.xml 中的 resultMap 修改为如下形式以提高性能

```xml
<resultMap id="PersonResultMap"  type="Person">
    <!--- 修改主键id的标签为 id 标签->
    <id property="id" column="id" />
    <!--<result property="id" column="id" />-->
    <result property="name" column="name" />
    <association property="card" javaType="IDCard">
        <result property="id" column="id" />
        <result property="cardSn" column="card_sn" />
    </association>
</resultMap>
```



### 5.1.2 方式二：子查询

解决一对一映射的方式一前面我们已经了解到应该如何设置，接下来，我们来用另一种方式来完成一对一映射，这种方式的本质其实就是子查询

> 虽然 mysql 中不推荐使用子查询，但是在 xml 配置文件中使用子查询方式代码更加简洁且易于维护，而且还能够复用，所以推荐这种使用方式

```xml
<resultMap id="personResultMap" type="Person">
    <id property="id" column="id" />
    <result property="name" column="name" />
    <!--
        注意：
        1. select 指向 IDCardMapper.xml 中配置的query方法
            <select id="queryCardNumberByID" parameterType="int" resultType="IDCard">
                select id, card_sn cardSn from id_card where id = #{id}
            </select>
        2. 下面的 column 指向的是从下面 <select id="queryPersonByID"> 中查询得到的参数,并作为入参传入 queryCardNumberByID
    -->
    <association property="card" column="card_id" select="com.xzh.mapper.IDCardMapper.queryCardNumberByID" />
</resultMap>
<select id="queryPersonByID" parameterType="int" resultMap="personResultMap">
    select * from `person` where id = #{id}
</select>
```



### 5.1.3 方式三：注解方式

> 说明：在实际开发过程中，还是推荐使用 **xml配置** 的方式

```java
// IDCardMapper 注解类
public interface IDCardMapperAnnotation {
    @Select("select `id`, `card_sn` `cardSn` from `id_card` where id = #{id}")
    public IDCard queryCardNumberByID(int id);
}
```

```java
// PersonMapper 注解类
public interface PersonMapperAnnotation {
    @Select("select `id`, `name`, `card_id` from `person` where `id` = #{id}")
    @Results({
            @Result(id = true, property = "id", column = "id"),
            @Result(property = "name", column = "name"),
            // @One 用来指向另一个 select
            @Result(property = "card", column = "card_id", one = @One(select = "com.xzh.mapper.IDCardMapperAnnotation.queryCardNumberByID"))
    })
    public Person queryPersonByID(int id);
}
```

> 其实注解方式其实就是对配置方式的变式，本质并无区别



### 5.1.4 一对一映射的注意事项

1. 表与表之间是否设置外键对 mybatis 映射并无影响，设置外键其实是数据库中一张表对另外一张表的约束





## 5.2 一对多（或者多对一）映射

### 5.2.1 配置文件方式实现

基本介绍

1. 项目中多对一的关系是一个基本的映射关系，多对一也可以理解为一对多
2. User - Pet：一个用户可以养多只宠物
3. Dep - Emp：一个部门可以有多个员工

注意：<u>多对多的关系其实就是双向的一对多关系</u>，所以我们的示例将以双向的一对一为例

**环境搭建**

01 创建表

```sql
# mybatis_pet
CREATE TABLE `mybatis_pet` (
  `id` int NOT NULL AUTO_INCREMENT,
  `nickname` varchar(32) CHARACTER SET utf8mb3 DEFAULT NULL,
  `user_id` int DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `user_id` (`user_id`),
  CONSTRAINT `mybatis_pet_ibfk_1` FOREIGN KEY (`user_id`) REFERENCES `mybatis_user` (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;
```

```sql
# mybatis_user
CREATE TABLE `mybatis_user` (
  `id` int NOT NULL AUTO_INCREMENT,
  `name` varchar(32) NOT NULL DEFAULT '',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;
```

```sql
INSERT INTO mybatis_user VALUES(NULL, '宋江'), (NULL, '张飞');
INSERT INTO mybatis_pet VALUES(NULL, '小白', 1), (NULL, '小黑', 1);
INSERT INTO mybatis_pet VALUES(NULL, '波斯猫', 2), (NULL, '加菲猫', 2);
```



02 创建实体类

```java
// Pet 宠物类
@Getter
@Setter
@AllArgsConstructor
@NoArgsConstructor
public class Pet {
    /*主键*/
    private int id;
    /*宠物名*/
    private String name;
    /*主人id*/
    private User user;
}
```

```java
// 用户类
@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {
    /*主键*/
    private int id;
    /*用户名*/
    private String name;
    /*拥有的宠物*/
    private List<Pet> pets;
}
```



03 配置对应的接口文件和 xml 配置文件

```java
public interface UserMapper {
    /**
     * 通过 id 获取 user 对象
     */
    public User getUserById(int id);
}
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.xzh.mapper.UserMapper">
    <resultMap id="userResultMap" type="User">
        <id property="id" column="id" />
        <result property="name" column="name" />
        <!--对于复杂类型，我们使用的是 associate,而对于复杂类型的集合，我们使用 collection-->
        <!--
            1. ofType 是指返回的集合类型
            2. column = "id" 表示 select * from mybatis_use where id = #{id} 返回的 id 对应的值，同时 也是 select 的入参
        -->
        <collection property="pets" column="id" ofType="Pet"
                    select="com.xzh.mapper.PetMapper.getPetByUserId" />
    </resultMap>

    <select id="getUserById" parameterType="int" resultMap="userResultMap">
        select `id`, `name` from `mybatis_user` where `id` = #{id}
    </select>
</mapper>
```

```java
public interface PetMapper {
    /**
     * 通过 user 的 id 获取 pet 对象，可能会有多个
     */
    public List<Pet> getPetByUserId(int userId);
}
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.xzh.mapper.PetMapper">
    <resultMap id="getPetByUserIdMap" type="Pet">
        <id property="id" column="id" />
        <result property="name" column="nickname" />
        <association property="user" column="user_id" select="com.xzh.mapper.UserMapper.getUserById" />
    </resultMap>

    <select id="getPetByUserId" parameterType="int" resultMap="getPetByUserIdMap">
        select * from `mybatis_pet` where `user_id` = #{userId}
    </select>
</mapper>
```



### 5.2.2 注解方式实现

与配置文件类似，注解实现方式其实就是配置文件的另一种实现方式，唯一需要关注就是如果字段类型为复杂对象，那么就需要使用 `@One` 进行处理，如果字段类型为复杂对象的集合，那么就需要使用 `@Many` 进行处理

```java
// PetMapperAnnotation
public interface PetMapperAnnotation {
    @Select("select * from `mybatis_pet` where `user_id` = #{userId}")
    @Results({
            @Result(id = true, property = "id", column = "id"),
            @Result(property = "name", column = "nickname"),
            @Result(property = "user", column = "user_id", one = @One(select = "com.xzh.mapper.UserMapperAnnotation.getUserById"))
    })
    public List<Pet> getPetByUserId(int userId);
}
```

```java
// UserMapperAnnotation
public interface UserMapperAnnotation {
    @Select("select `id`, `name` from `mybatis_user` where `id` = #{id}")
    @Results({
        @Result(id = true, property = "id", column = "id"),
        @Result(property = "name", column = "name"),
        @Result(property = "pets", column = "id", many = @Many(select = "com.xzh.mapper.PetMapperAnnotation.getPetByUserId"))
    })
    public User getUserById(int id);
}
```



# 6 缓存

## 6.1 一级缓存

### 6.1.1 一级缓存基本介绍

1. 默认情况下，mybatis  是启用一级缓存的 / 本地缓存 / local cache，它是 SqlSession级别的
2. 同一个 SqlSession 接口对象调用了相同的 select 语句，会直接从缓存里面获取，而不是再去查询数据库

![mybatis原理-一级缓存详解和注意事项 － 小专栏](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/8db730a8134a28f1de030c9a35426c8e.png)



### 6.1.2 一级缓存失效分析

1. 一级缓存是 session 会话级别的，因此当我们关闭 sqlSession 会话后，前面会话的一级缓存自然而然会失效
2. 手动销毁缓存：使用 `sqlSession.clearCache()` 可以使得一级缓存失效

3. 当我们对同一个对象进行修改时，该对象在一级缓存中会失效



## 6.2 二级缓存

### 6.2.1 基本介绍

1. 二级缓存和一级缓存都是为了提高检索效率
2. 他们之间最大的区别就是作用域范围不同，一级缓存的作用域是 sqlSession 会话级别，在一次会话有效，而二级缓存作用域是全局范围，正对不同的会话都有效

![MyBatis实战(三)-二级缓存原理解析_慕课手记](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/5bcb28ef00019c9210000723.jpg)



### 6.2.2 快速入门

1. mybatis-config.xml 中配置中开启二级缓存

   ```xml
   <settings>
       <setting name="cacheEnabled" value="true" />
   </settings>
   ```

2. 使用二级缓存时 entity 类实现序列化接口（serializable），因为二级缓存可能使用到序列化技术（可以暂时不需要配置）

   ```java
   public class Monster implements Serializable {}
   ```

3. 在 sql 映射文件中添加如下配置

   ```xml
   <cache
   eviction="FIFO"
   flushInterval="60000"
   size="512"
   readOnly="true"/>
   ```

   - eviction：清除策略，有 FIFO —— 先进先出算法、LRU —— 最近最少使用算法（default）、SOFT —— 软引用、WEAK —— 弱引用
   - flushInterval：缓存刷新间隔，以毫秒为单位
   - size：引用数目，默认是 1024
   - readOnly：制度，只读的缓存会给所有调用者返回缓存对象的相同实例。 因此这些对象不能被修改。这就提供了可观的性能提升。而可读写的缓存会（通过序列化）返回缓存对象的拷贝。 速度上会慢一些，但是更安全，因此默认值是 false



### 6.2.3 二级缓存注意事项

01 关闭缓存的三种方式

1. 在 mybatis-config.xml 中关闭，即将 `cacheEnabled` 这个总开关给关闭掉
2. 在 sql 映射文件去掉 `cache` 标签
3. 更加细粒度的操作，在配置方法上指定 `<select useCache="false">`

> 注意：insert 、update、delete 操作数据后需要刷新缓存，方法中默认会有 `<update flushCache="true>` 的操作，如果不执行刷新缓存就会出现脏读的情况，如果我们确定某 sqlSession 中只会存在查询操作，那么我们在 sql 映射文件中将 readOnly 设置为 true，以提高性能



:star:02 特别注意：不会存在一级缓存和二级缓存中有同一个数据。因为二级缓存数据是在一级缓存的 sqlSession 关闭之后才会放进去的



## 6.3 EhCache

推荐阅读：https://www.cnblogs.com/zqyanywn/p/10861103.html

### 6.3.1 基本介绍

1. EhCache 是一个纯 Java 的缓存框架，具有快速、精干等特点
2. MyBatis 有自己默认的二级缓存，但是在实际项目中，往往使用的是更加专业的第三方缓存产品作为 MyBatis 的二级缓存，例如 EhCache

01 引入相关依赖

```xml
<!--引入 ehcache 核心库-->
<dependency>
    <groupId>net.sf.ehcache</groupId>
    <artifactId>ehcache-core</artifactId>
    <version>2.6.11</version>
</dependency>
<!--引入需要的 slf4j-->
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-api</artifactId>
    <version>2.0.7</version>
</dependency>
<!--引入 ehcache 与 mybatis 关联的库-->
<dependency>
    <groupId>org.mybatis.caches</groupId>
    <artifactId>mybatis-ehcache</artifactId>
    <version>1.2.3</version>
</dependency>
```

02 在 resources 中新建 ehcache.xml 配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:noNamespaceSchemaLocation="http://ehcache.org/ehcache.xsd"
         updateCheck="false">
    <!--
       diskStore：为缓存路径，ehcache分为内存和磁盘两级，此属性定义磁盘的缓存位置。参数解释如下：
       user.home – 用户主目录
       user.dir  – 用户当前工作目录
       java.io.tmpdir – 默认临时文件路径
     -->
    <diskStore path="java.io.tmpdir/Tmp_EhCache"/>
    <!--
       defaultCache：默认缓存策略，当ehcache找不到定义的缓存时，则使用这个缓存策略。只能定义一个。
     -->
    <!--
      name:缓存名称。
      maxElementsInMemory:缓存最大数目
      maxElementsOnDisk：硬盘最大缓存个数。
      eternal:对象是否永久有效，一但设置了，timeout将不起作用。
      overflowToDisk:是否保存到磁盘，当系统当机时
      timeToIdleSeconds:设置对象在失效前的允许闲置时间（单位：秒）。仅当eternal=false对象不是永久有效时使用，可选属性，默认值是0，也就是可闲置时间无穷大。
      timeToLiveSeconds:设置对象在失效前允许存活时间（单位：秒）。最大时间介于创建时间和失效时间之间。仅当eternal=false对象不是永久有效时使用，默认是0.，也就是对象存活时间无穷大。
      diskPersistent：是否缓存虚拟机重启期数据 Whether the disk store persists between restarts of the Virtual Machine. The default value is false.
      diskSpoolBufferSizeMB：这个参数设置DiskStore（磁盘缓存）的缓存区大小。默认是30MB。每个Cache都应该有自己的一个缓冲区。
      diskExpiryThreadIntervalSeconds：磁盘失效线程运行时间间隔，默认是120秒。
      memoryStoreEvictionPolicy：当达到maxElementsInMemory限制时，Ehcache将会根据指定的策略去清理内存。默认策略是LRU（最近最少使用）。你可以设置为FIFO（先进先出）或是LFU（较少使用）。
      clearOnFlush：内存数量最大时是否清除。
      memoryStoreEvictionPolicy:可选策略有：LRU（最近最少使用，默认策略）、FIFO（先进先出）、LFU（最少访问次数）。
      FIFO，first in first out，这个是大家最熟的，先进先出。
      LFU， Less Frequently Used，就是上面例子中使用的策略，直白一点就是讲一直以来最少被使用的。如上面所讲，缓存的元素有一个hit属性，hit值最小的将会被清出缓存。
      LRU，Least Recently Used，最近最少使用的，缓存的元素有一个时间戳，当缓存容量满了，而又需要腾出地方来缓存新的元素的时候，那么现有缓存元素中时间戳离当前时间最远的元素将被清出缓存。
   -->
    <defaultCache
            eternal="false"
            maxElementsInMemory="10000"
            overflowToDisk="false"
            diskPersistent="false"
            timeToIdleSeconds="1800"
            timeToLiveSeconds="259200"
            memoryStoreEvictionPolicy="LRU"/>
  
    <cache
            name="cloud_user"
            eternal="false"
            maxElementsInMemory="5000"
            overflowToDisk="false"
            diskPersistent="false"
            timeToIdleSeconds="1800"
            timeToLiveSeconds="1800"
            memoryStoreEvictionPolicy="LRU"/>
  
</ehcache>
```

- timeToIdleSeconds：简称为 TTI， 即最长不使用的存活时间
- timeToLiveSeconds：简称为 TTL，即最长存活时间（无论使用还是不使用）



03 在 mapper 中启动 ehcache

```xml
<cache type="org.mybatis.caches.ehcache.EhcacheCache" />
```



### 6.3.2 EhCache 和 Mybatis 缓存的关系

1. MyBatis 提供了一个 Cache 接口，只要实现了该 Cache 接口，就可以作为二级缓存产品和 MyBatis 整合使用，Ehcache 就是实现了该接口的类
2. MyBatis 默认情况（即一级缓存）是使用 PerpetualCache 类实现 Cache 接口，这是一个核心类
