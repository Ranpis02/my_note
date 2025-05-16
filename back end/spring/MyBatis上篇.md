[toc]

# 1 MyBatis 基本介绍

## 1.1 传统方式操作 DB 分析

![image-20230802154102341](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230802154102341.png)

总结一下就是：

1. 连接数据库部分代码，不同程序员标准不同
2. 程序不满足 OOP ，并且 SQL 语句是写在程序中，属于硬编码。换言之，我们更希望是 `insert(obj)` 而不是 `insert(sql)`



## 1.2 基本介绍

1. MyBatis 是一个持久层框架
2. 前身是 ibatis，在 ibatis3.x 时，更名为 MyBatis
3. MyBatis 在 java 和 sql 之间提供了更灵活的映射方案
4. MyBatis 可以将对数据表的操作（sql, 方法）等等直接剥离，写到 xml 配置文件，实现和 java 代码的解耦

工作流程图如下：

![image-20230802160137442](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230802160137442.png)



## 1.3 快速入门

需求说明：开发一个 MyBatis 项目，通过 MyBatis 的方式可以完成对 monster 表的 CRUD 操作

整体目录结构

```
src                                       
├─ main                                   
│  ├─ java                                
│  │  └─ com                              
│  │     └─ xzh                           
│  │        ├─ entity                     
│  │        │  └─ Monster.java Monster 实体类      
│  │        ├─ mapper                     
│  │        │  ├─ xml                     
│  │        │  │  └─ MonsterMapper.xml Monster 的配置文件    
│  │        │  └─ MonsterMapper.java  操作 Monster 的接口类
│  │        └─ utils                      
│  │           └─ MybatisUtils.java 工具类，用于获取 mybatis 连接
│  └─ resources                           
│     └─ mybatis-config.xml   mybatis 的配置文件      
└─ test                                   
   ├─ java                                
   │  └─ com                              
   │     └─ xzh                           
   │        └─ mapper                     
   │           └─ MonsterMapperTest.java  测试程序
   └─ resources                           
```



01 创建工程和子模块（这里仅展示对应的 pom.xml）

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <!--
        1. 将 mybatis 作为父项目管理多个子模块/子项目
        2. 父项目的完整坐标 groupId 和 artifactId
        3. 后面该父项目会管理多个子模块/子项目，将父项目中引入的依赖可以直接给子项目使用
        4. <packaging>pom</packaging> 表示父项目以多个子模块/子项目管理工程
    -->
    <groupId>com.xzh</groupId>
    <artifactId>mybatis</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>pom</packaging>
    <modules>
        <module>mybatis-quickstart</module>
    </modules>

    <properties>
        <mybatis.version>3.5.6</mybatis.version>
        <mysql.version>8.0.33</mysql.version>
        <junit.version>4.13.1</junit.version>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>


    <dependencyManagement>
        <dependencies>
            <!--mybatis 依赖-->
            <dependency>
                <groupId>org.mybatis</groupId>
                <artifactId>mybatis</artifactId>
                <version>${mybatis.version}</version>
            </dependency>

            <!--mysql 驱动-->
            <dependency>
                <groupId>mysql</groupId>
                <artifactId>mysql-connector-java</artifactId>
                <version>${mysql.version}</version>
            </dependency>

            <!--junit 单元测试-->
            <dependency>
                <groupId>junit</groupId>
                <artifactId>junit</artifactId>
                <version>${junit.version}</version>
                <!--如果这里有一个 scope:test 表示该 jar 的作用范围在 test 目录-->
                <!--<scope>test</scope>-->
            </dependency>
        </dependencies>
    </dependencyManagement>
    <!--以下的资源插件代表的含义是将 src/main/java 和 src/main/resources 下的资源文件全部放入到 target/classes 中-->
    <build>
        <resources>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <!--<filtering>false</filtering>-->
            </resource>
			
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
</project>
```

```xml
<!-- 子模块的 pom -->
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <!--
        1. parent 指定了该模块的父项目的完整坐标 groupId + artifactId
        2. 这里配置后，该模块就可以使用/引用父项目的依赖
     -->
    <parent>
        <groupId>com.xzh</groupId>
        <artifactId>mybatis</artifactId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <!--子项目的项目名-->
    <artifactId>mybatis-quickstart</artifactId>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

    <dependencies>
        <dependency>
            <groupId>com.xzh</groupId>
            <artifactId>mybatis</artifactId>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>

        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <!--<scope>test</scope>-->
        </dependency>
    </dependencies>
</project>
```



02 创建 monster 表和对应的 entity

```sql
# sql 建表语句
CREATE TABLE `monster` (
	`id` INT NOT NULL AUTO_INCREMENT,
	`name` VARCHAR ( 255 ) CHARACTER 
	SET utf8mb3 COLLATE utf8mb3_general_ci NOT NULL,
	`age` TINYINT UNSIGNED NOT NULL,
	`gender` CHAR ( 1 ) CHARACTER 
	SET utf8mb3 COLLATE utf8mb3_general_ci NOT NULL COMMENT '0=female,1=male',
	`birthday` DATE DEFAULT NULL,
	`email` VARCHAR ( 255 ) CHARACTER 
	SET utf8mb3 COLLATE utf8mb3_general_ci NOT NULL,
	`salary` DOUBLE NOT NULL,
PRIMARY KEY ( `id` ) 
) ENGINE = INNODB AUTO_INCREMENT = 5 DEFAULT CHARSET = utf8mb3;
```



```java
/**
 * 怪兽实体
 */
@Getter
@Setter
@ToString
@NoArgsConstructor
@AllArgsConstructor
public class Monster {
    /*怪兽 ID */
    private int id;
    /*怪兽名*/
    private String name;
    /*怪兽年龄*/
    private Integer age;
    /*怪兽性别。0=female,1=male*/
    private String gender;
    /*怪兽生日*/
    private Date birthday;
    /*怪兽电子邮件*/
    private String email;
    /*怪兽薪水*/
    private Double salary;
}
```



03 添加对应的 MonsterMapper 用来作为 DAO 层操作数据库以及使用 xml 配置文件建立映射

```java
public interface MonsterMapper {
    /**
     * 添加 monster
     */
    public void addMonster(Monster monster);
}
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
    1. 这是一个 mapper xml 文件，该问价用于实现对应的接口方法
    2. namespace 指定该 xml 文件和那个接口对应
-->
<mapper namespace="com.xzh.mapper.MonsterMapper">
    <!--这里的 id 对应的就是 MonsterMapper 接口中的方法名-->
    <insert id="addMonster" parameterType="com.xzh.entity.Monster">
        insert into monster (name, age, gender, birthday, email, salary) values (#{name}, #{age}, #{gender}, #{birthday}, #{email}, #{salary});
    </insert>
</mapper>
```



04 配置好 mybatis 的配置文件 —— mybatis-config.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!--&lt;!&ndash;配置 MyBatis 自带的日志输出&ndash;&gt;-->
    <settings>
       <setting name="logImpl" value="STDOUT_LOGGING" />
    </settings>

    <!--配置类型别名-->
    <typeAliases>
        <typeAlias type="com.xzh.entity.Monster" alias="Monster" />
    </typeAliases>

    <environments default="development">
        <environment id="development">
            <!--配置事务管理器-->
            <transactionManager type="JDBC"/>
            <!--配置数据源-->
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <!--
                    1. &amp; 即 "&" 符号，为了防止解析错误
                    2. useUnicode=true 使用 unicode 作用是防止编码错误
                    3. characterEncoding=UTF-8" 防止中文乱码
                 -->
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=true&amp;useUnicode=true&amp;characterEncoding=UTF-8"/>
                <property name="username" value="root"/>
                <property name="password" value="486101620"/>
            </dataSource>
        </environment>
    </environments>
    <!--这里配置我们需要管理的 mapper 文件-->
    <mappers>
        <mapper resource="com/xzh/mapper/xml/MonsterMapper.xml"/>
    </mappers>
</configuration>
```



05 配置工具类获取连接，最后测试代码

```java
/**
 * 获取连接工具类
 */
public class MybatisUtils {
    private final static SqlSessionFactory sqlSessionFactory;

     // 下面的静态代码块用于初始化 sqlSessionFactory
     static {
         // 指定配置文件
         String resource = "mybatis-config.xml";
        // 使用 Stream 流获取对应的 inputStream(这里使用的新的异常处理风格 Try-with-resources, 用完后会自动关闭流，不需要我们在 finally 中手动关闭)
         // 加载文件时，resources 目录下的文件 => 工作目录 target-classes 下面
        try(InputStream resourceAsStream = Resources.getResourceAsStream(resource)) {
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }

    /**
     * 从 sqlSessionFactory 中取出 session
     */
    public static SqlSession getSqlSession() {
        return sqlSessionFactory.openSession();
    }
}
```

```java
/**
 * 测试程序
 */
public class MonsterMapperTest {
    private SqlSession sqlSession;
    private MonsterMapper monsterMapper;

    @Before
    public void init() {
        // 1. 获取到 sqlSession
        sqlSession= MybatisUtils.getSqlSession();
        // 2. 获取到 MonsterMapper 对象
        monsterMapper = sqlSession.getMapper(MonsterMapper.class);
    }

    @After
    public void close() {
        if(sqlSession != null) {
            sqlSession.commit();
            // 注意：下面的 close 我们应该理解为释放连接资源，而不是真正的关闭连接
            sqlSession.close();
        }
    }
    @Test
    public void addMonster() {
        for(int i = 0 ; i < 2; ++i) {
            Monster monster = new Monster();
            monster.setName("第" + i + "号松鼠精");
            monster.setAge(i + 18);
            monster.setGender( Integer.toString(i % 2));
            monster.setBirthday(new Date());
            monster.setEmail("ranpis0@qq.com");
            monster.setSalary((double) (10000 + 10 * i));
            monsterMapper.addMonster(monster);
            System.out.println("添加到表中，自增长的 id 的值为：" + monster.getId());
        }
    }
}
```



:hole:坑人点

1. 我们在最后测试的时候很有可能会报无法找到 mapper 配置文件的异常信息，这是因为我们的 mapper 配置文件不是放在 resources 下面，所以不会放入到 target/classes

   解决方案：在根目录的 pom.xml 中添加资源插件，将 mapper 配置文件编译到 classes 中。可能这时候就算配置了也还是无法生效，可能是 maven 缓存的缘故，我们可以在 IDEA 中的配置中更换 maven ，然后在 lifecycle 中 clean 一下

   ![image-20230803023317710](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230803023317710.png)



## 1.4 返回自增长

我们如果想要对数据库表中的某些自增长字段（例如主键）进行配置，那么我们可以可以 使用如下属性配置：

![image-20230803103425930](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230803103425930.png)

我们在 mapper 文件添加对自增长属性的配置即可

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.xzh.mapper.MonsterMapper">
    <!--useGeneratedKeys 设置为 true, keyProperty 指定自增长主键-->
    <insert id="addMonster" parameterType="com.xzh.entity.Monster" useGeneratedKeys="true" keyProperty="id">
        insert into monster (name, age, gender, birthday, email, salary) values (#{name}, #{age}, #{gender}, #{birthday}, #{email}, #{salary});
    </insert>
</mapper>
```

这样配置后，我们就可以在 Java 后端通过 `getXXX()` 方法获取到自增长的键的值

```java
for(int i = 0 ; i < 2; ++i) {
    Monster monster = new Monster();
    monster.setName("第" + i + "号松鼠精");
    monster.setAge(i + 18);
    monster.setGender( Integer.toString(i % 2));
    monster.setBirthday(new Date());
    monster.setEmail("ranpis0@qq.com");
    monster.setSalary((double) (10000 + 10 * i));
    monsterMapper.addMonster(monster);
    System.out.println("添加到表中，自增长的 id 的值为：" + monster.getId());
}
```



## 1.5 CRUD

### 1.5.1 删除

我们在学会了前面的插入后，其实删除操作也非常简单，首先在 MonsterMapper 接口中添加抽象方法    

`delMonster(int id)`

```java
/**
 * 删除 monster
 */
public void delMonster(Integer id);
```

之后在 MonsterMapper.xml 文件中进行相关配置

```java
<!--
    1. 配置 / 实现 delMonster 方法
    2. 如果我们的 parameterType 是包装类，例如 Integer, 那么我们的完整写法应该是 java.lang.Integer
 -->
<delete id="delMonster" parameterType="int">
    DELETE FROM `monster` WHERE id = #{id}
</delete>
```

最后我们单元测试一下看看是否可以删除成功

```java
@Test
public void delMonster() {
    monsterMapper.delMonster(2);
}
```



### 1.5.2 修改

MonsterMapper 接口中添加抽象方法

```java
/**
 * 修改 monster 信息
 */
public void updateMonster(Monster monster);
```

MonsterMapper.xml 中添加如下配置

```xml
<!--配置修改语句，下面的 name 等字段对应的 Monster 对象的-->
<update id="updateMonster" parameterType="com.xzh.entity.Monster">
    UPDATE `monster`
    SET `name` = #{name},
    `age` = #{age},
    `gender` = #{gender},
    `birthday` = #{birthday},
    `email` = #{email},
    `salary` = #{salary}
    WHERE
    id = #{id}
</update>
```

测试程序

```xml
<!--配置修改语句，下面的 name 等字段对应的 Monster 对象的-->
<update id="updateMonster" parameterType="com.xzh.entity.Monster">
    UPDATE `monster`
    SET `name` = #{name},
    `age` = #{age},
    `gender` = #{gender},
    `birthday` = #{birthday},
    `email` = #{email},
    `salary` = #{salary}
    WHERE
    id = #{id}
</update>
```



### 1.5.3 查询操作

#### 设置类型别名

例如，由于 com.xzh.entity.Monster 这个类型名过长，所以我们希望为其起一个别名，减小冗余度，如下：

```xml
<!--配置类型别名-->
<typeAliases>
    <typeAlias type="com.xzh.entity.Monster" alias="Monster" />
</typeAliases>
```

> 注意：该配置是在 resources 下的 mybatis-config.xml 中进行配置

配置完成之后，后面的 parameterType 对应的类型名，我们可以简写为 Monster





01 配置两个方法，分别是根据 id 返回 monster 对象

```java
/**
 * 根据 id 返回 monster 对象
 */
public Monster getMonsterById(Integer id);

/**
 * 查询所有的 Monster
 */
public List<Monster> getAllMonster();
```



02 在 mapper 文件中做出相应的配置

```xml
<!--配置/实现getMonsterById, resultType 声明期望从这条语句中返回结果的类全限定名或者别名-->
<select id="getMonsterById" resultType="Monster">
    SELECT * FROM `monster` WHERE id = #{id}
</select>

<!--配置/实现getAllMonster方法, 注意，对于集合类型，我们只需要设置为集合包含的类型，而不是集合本身的类型-->
<select id="getAllMonster" resultType="Monster">
    SELECT * FROM `monster`
</select>
```



03 测试程序

```java
@Test
public void querySingle() {
    Monster monster = monsterMapper.getMonsterById(6);
    System.out.println(monster);
}

@Test
public void queryAll() {
    List<Monster> allMonster = monsterMapper.getAllMonster();
    for(Monster m : allMonster) {
        System.out.println(m);
    }
}
```



## 1.6 日志输出配置

在开发 MyBatis 程序时，如果我们想要查看那程序底层发给 MySQL 的 SQL 语句，那么我们应该如何处理？



### 1.6.1 使用 mybatis 内置的日志

```xml
<!--配置 MyBatis 自带的日志输出-->
<settings>
   <setting name="logImpl" value="STDOUT_LOGGING" /> 
</settings>
```

> 注意 settings 的配置位置，需要放在 `<configuration>` 的最前面

当我们添加上面的配置信息后，我们就可以直接看到效果了

![image-20230803210759772](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230803210759772.png)

# 2 手动实现 MyBatis 的底层机制

