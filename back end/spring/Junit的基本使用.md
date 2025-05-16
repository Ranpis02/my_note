Junit4 注解提供了书写单元测试的基本功能，主要有如下几个注解：

- `@BeforeClass`
- `@Before`
- `@Test`
- `@AfterClass`
- `After`

## @BeforeClass注解

> 被@BeforeClass注解的方法会是：
>
> - 只被执行一次
> - 运行junit测试类时第一个被执行的方法
>
> 这样的方法被用作执行计算代价很大的任务，如打开数据库连接。被@BeforeClass 注解的方法应该是静态的（即 static类型的）。

### @AfterClass注解

> 被@AfterClass注解的方法应是：
>
> - 只被执行一次
> - 运行junit测试类是最后一个被执行的方法
>
> 该类型的方法被用作执行类似关闭数据库连接的任务。被@AfterClass 注解的方法应该是静态的（即 static类型的）。

### @Before注解

> 被@Before 注解的方法应是：
>
> - junit测试类中的任意一个测试方法执行 前 都会执行此方法
>
> 该类型的方法可以被用来为测试方法初始化所需的资源。

### @After注解

> 被@After注解的方法应是：
>
> - junit测试类中的任意一个测试方法执行后 都会执行此方法, 即使被@Test 或 @Before修饰的测试方法抛出异常
>
> 该类型的方法被用来关闭由@Before注解修饰的测试方法打开的资源。

### @Test 注解

> 被@Test注解的测试方法包含了真正的测试代码，并且会被Junit应用为要测试的方法。@Test注解有两个可选的参数：
>
> - **expected** 表示此测试方法执行后应该抛出的异常，（值是异常名）
> - **timeout** 检测测试方法的执行时间



测试方法执行顺序

- 一个单元测试用例执行顺序为： @BeforeClass –> @Before –> @Test –> @After –> @AfterClass 
- 每一个测试方法的调用顺序为： @Before –> @Test –> @After