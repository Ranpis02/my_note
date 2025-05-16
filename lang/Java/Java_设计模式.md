[toc]

# 2 创建型模式

## 2.1 单例模式

单例模式：确保一个类只有一个实例，并且自行实例化并向整个系统提供该实例。

根据创建的时机不同，我们又可以将单例模式分为以下两种：

- 饿汉式：在类加载的时候单例就被创建出来

- 懒汉式：在调用的时候单例才被创建出来

  > 懒汉式存在 ==线程安全== 的问题

```java
// 饿汉式
class Singleton {
    private static Singleton singleton = new Singleton();

    private Singleton() {}

    public static Singleton getInstance() {
        return singleton; 
    }
}
```

```java
// 懒汉式
class Singleton {
    private volatile static Singleton singleton;

    private Singleton() {
    }

    public static Singleton getInstance() {
        if (singleton == null) {
            synchronized (Singleton.class) {
                if (singleton == null)
                    singleton = new Singleton();
            }
        }
        return singleton;
    }
}
```

:thinking:思考：

问1：在懒汉式中为什么我们将 `sychronized` 添加到第二重检查中，为什么不直接将其放在 `getInstance()` 上?

> 为了节省资源，使用类锁，将其添加到代码块中显然比直接添加到方法中更加节省资源

问2：在懒汉式中我们为了需要在静态变量 `singleton` 上添加 `volatile` 修饰？

> 避免代码重排导致后续线程访问到一个空对象，实例化对象的步骤如果按照正确程序而言，步骤如下：
>
> 1. 分配内存空间
> 2. 初始化对象
> 3. 将对象指向刚分配的内存空间
>
> 但是有些编译器会进行性能优化，导致 2、3 顺序颠倒，如下：
>
> 1. 分配内存空间
> 2. 将对象指向刚分配的内存空间
> 3. 初始化对象



:herb: **补充**

单例在全局情况下是唯一的，但是在以下两种情况中，也会存在出现多个实例的情况：

- 在分布式系统中，存在多个 JVM ，每个 JVM 都会有一个实例
- 同一个 JVM ，使用多个类加载器同时加载该类，产生了多个实例



<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230622234149706.png" alt="image-20230622234149706" style="zoom:50%;" />

## 2.2 简单工厂模式

简单工厂模式：又称为静态工厂方法模式，属于创建型模式，在简单工厂模式中，可以根据参数的不同返回不同类的实例。简单工厂模式专门定义了一个类来负责创建其他类的实例，被创建的实例通常都具有共同的父类。

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230623001055512.png" alt="image-20230623001055512" style="zoom:50%;" />

```java
/**
 * 简单工厂类
 */
class SimpleFactory {
    public static Product createProduct(String type) {
        if("A".equals(type)) {
            return new ProductA();
        } else if("B".equals(type)) {
            return new ProductB();
        } else {
            return null;
        }
    }
}

/**
 * 产品类
 */
abstract class Product {
    public abstract void print();
}

class ProductA extends Product {
    @Override
    public void print() {
        System.out.println("产品A");
    }
}

class ProductB extends Product {
    @Override
    public void print() {
        System.out.println("产品B");
    }
}

```

简单工厂的优点：

1. 将对象的创建与使用分离，创建的过程交给静态工厂，我们只需要去使用即可

缺点：

1. 违背开放-封闭原则，每新增一个产品，我们就需要修改工厂类的代码



## 2.3 工厂方法模式

工厂方法：定义一个用于创建对象的接口，让子类去决定实例化哪一个类。工厂方法使一个类的实例化延迟到其子类。



![image-20230623002748510](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230623002748510.png)





## 2.4 抽象工厂模式



## 2.5 生成器模式

```java
public class Method {
    public static void main(String[] args) {
        House.Builder builder = new House.Builder().setWindow(new Window()).setDoor(new Door()).setWall(new Wall());
    }
}

class Window {}

class Door {}

class Wall {}

class House {
    private Window window;
    private Door door;
    private Wall wall;

    public House(Window window, Door door, Wall wall) {
        this.window = window;
        this.door = door;
        this.wall = wall;
    }

    static final class Builder {
        private Window window;
        private Door door;
        private Wall wall;
        
        public Builder setWindow(Window window) {
            this.window = window;
            return this;
        }
        
        public Builder setDoor(Door door) {
            this.door = door;
            return this;
        }
        
        public Builder setWall(Wall wall) {
            this.wall = wall;
            return this;
        }
    }
}
```



## 2.6 原型模式

原型模式：一种创建型模式，通过复制已有的对象来创建新的对象，而无需显式地指定他们的类。

使用原型模式的原因：

1. 简化创建过程：原型模式可以避免复杂对象的创建过程，尤其是对象的创建有一系列复杂的步骤或者是某一个类的子类的时候
2. 提高程序性能：在大多数情况下，复制对象比创建新的对象要更快



![image-20230623005330140](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230623005330140.png)





代码实现

![image-20230623010324901](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230623010324901.png)

![image-20230623010437179](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230623010437179.png)





# 3 结构型模式

在这一章节中，我们将介绍结构型模式，结构型模式与创建型模式最大的不同就在于，创建型模式主要是用来创建新的对象的，而结构型模式是构建类与类之间的结构，使得一个类

## 3.1 组合模式

组合模式是一种结构型设计模式，又称为 "整体-部分" 模式，它是一种将**对象组合成树状的层次结构**的模式，用来表示 "整体-部分" 的关系，使用户<u>对单个对象和组合对象具有一致的访问性</u>

关于组合模式的经典动画图就是一张长满了多个 "零件" 的树，<strong style="color:red">一棵树上由叶子和树干组成，每个树干又是由叶子和更小的树干组成</strong>

![组合设计模式](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/composite.png)

对于组合模式，下面有一种非常经典的类图如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230608213944947.png" alt="image-20230608213944947" style="zoom:50%;" />





我们以行政区划，自底向上进行人口普查为例

**代码实现**

```java
/**
 * 统计接口
 */
interface Counter {
    int count();
}
```



```java
/**
 * 叶子结点
 */
class City implements Counter {
    private int sum = 0;

    public City(int sum) {
        this.sum = sum;
    }

    @Override
    public int count() {
        return sum;
    }
}
```



```java
/**
 * 容器：里面包含了更多的节点
 */

class Composite implements Counter {
    private List<Counter> counterList  = new ArrayList<>();

    public void add(Counter counter) {
        counterList.add(counter);
    }

    public void delete(Counter counter) {
        counterList.remove(counter);
    }

    public List<Counter> getChild() {
        return counterList;
    }

    public int count() {
        int sum = 0;
        for(Counter c : counterList) {
            sum += c.count();
        }

        return sum;
    }
}
```



```java
public class Test {
    public static void main(String[] args) {
        Counter beijing = new City(100);
        Counter shanghai = new City(200);

        Counter jiangxi = new City(200);
        Counter hubei = new City(300);
        Composite cityList = new Composite();
        cityList.add(jiangxi);
        cityList.add(hubei);

        Composite china = new Composite();
        china.add(beijing);
        china.add(shanghai);
        china.add(cityList);

        System.out.println(china.count());
    }
}
```





## 3.2 装饰器模式

装饰器模式：一种封装模式，在原有类的基础上动态添加新的功能，这种扩展有别于继承或实现，它偏向于 "关联关系"，使用起来更为灵活

![image-20230608221330688](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230608221330688.png)

:thought_balloon:为什么说装饰器模式相较于继承更为灵活？

我们可以反过来思考，继承模式 "臃肿" 在哪里？如果我们直接拿生活中的应用场景举例，那么就好比，装饰器是 "外物"，而继承是 "内在"，对于装饰器我们可以随取随用，而继承就会跟随，我们必须要带上。在编程领域，spring 的 AOP 切面编程显然采取了装饰器这种模式用于提高灵活性，我们可以在必要的时候添加前置通知、后置通知、返回通知等



对于装饰器模式，其类图简图如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230608234145615.png" alt="image-20230608234145615" style="zoom:50%;" />



我们以机器人迭代为例，如下：

```java
interface Robot {
    void doSomething();
}
```

```java
class FirstRobot implements Robot {
    public void doSomething() {
        System.out.println("唱");
        System.out.println("跳");
    }
}
```

```java
abstract class RobotDecorator implements Robot {
    protected Robot robot;

    public RobotDecorator(Robot robot) {
        this.robot = robot;
    }

    public abstract void doSomething();
}

class SecondRobot extends RobotDecorator {
    public SecondRobot(Robot robot) {
        super(robot);
    }

    public void doSomething() {
        robot.doSomething();
        System.out.println("rap");
    }
}

class ThirdRobot extends RobotDecorator {
    public ThirdRobot(Robot robot) {
        super(robot);
    }

    public void doSomething() {
        robot.doSomething();
        System.out.println("篮球");
    }
}
```



## 3.3 适配器模式

适配器模式：将一个类的接口变换为客户端所期待的另一种接口，从而使得原本因接口不匹配而无法在一起工作的两个类能够在一起工作。

> 区分适配器模式和装饰者模式：
>
> - 适配器模式是原有的接口可以正常使用，但是客户端无法正常使用，需要中间提供一个适配器（或者说是 "转接口"）
> - 装饰者模式会在原有的功能基础上扩展新的功能，但是与单纯的继承或实现不同，装饰者将扩展的功能抽离，灵活使用

我们可以将适配器视作 "翻译官"，示例代码如下：

```java
// 被是适配者（Adaptee）
class Speaker {
    public String speak() {
        return "Hello World";
    }
}
```

```java
interface Translator {
    public String translate();
}

class Adapter implements Translator {
    private Speaker speaker;

    public Adapter(Speaker speaker) {
        this.speaker = speaker;
    }
    @Override
    public String translate() {
        String origin = speaker.speak();
        // 经过翻译之后处理之后，返回，这里作为演示，不做特殊处理，直接将其返回
        return origin;
    }
}
```

适配器模式中适配器与被适配者可以采用两种方式，一种是关联关系，另一种是继承关系，如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230609003810281.png" alt="image-20230609003810281" style="zoom:50%;" />

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230609003826440.png" alt="image-20230609003826440" style="zoom:50%;" />

如果采用继承的方式，那么实现方式如下：

```java
class Speaker {
    protected String speak() {
        return "Hello World";
    }
}

interface Translator {
    public String translate();
}

class Adapter extends Speaker implements Translator {
    
    @Override
    public String translate() {
        String origin = speak();
        // 经过翻译之后处理之后，返回，这里作为演示，不做特殊处理，直接将其返回
        return origin;
    }
}
```



## 3.4 外观模式

外观模式：要求一个子系统的外部与内部的通信必须通过一个统一的对象进行访问，外观模式会提供一个高层次的模块，供子系统使用，这么做的意义是为了减少外部类与内部类的交互，降低耦合度，同时便于使用。

> 这与封装代码基本是类似的，在编程中，我们很少涉及底层数据结构的操作，一般都是在他人的框架基础上或直接调用他人提供的库来完成某些功能，这样使得开发速度快，代码简洁，且更容易维护

![image-20230609005527846](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230609005527846.png)

外观模式类图如下：

![image-20230609092836928](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230609092836928.png)

如果我们不使用外观模式，那么客户端（Client）就需要直接与子系统打交道显然这样造成系统内部之间的关系十分复杂，耦合度大大提高，而使用外观模式就将内部的一系列操作封装起来，只对外暴露一个接口供客户端使用

> 外观模式最大的缺点就是不满足开闭原则，每次当我们需要新增子系统，那么我们就需要在对外暴露的接口（高层次模块）中进行修改代码

**示例：验证 "好男人系统"**

```java
/**
 * 内部子系统
 */
class SubFlow1 {
    boolean isTrue() {
        return true;
    }
}

class SubFlow2 {
    boolean isOk() {
        return true;
    }
}

class SubFlow3 {
    boolean isGoodMan() {
        return true;
    }
}
```

```java
/**
 * 向外提供的类接口：直接通过调用 isAuth 方法即可完成
 */
class Facade {
    SubFlow1 subflow1 = new SubFlow1();
    SubFlow2 subflow2 = new SubFlow2();
    SubFlow3 subflow3 = new SubFlow3();

    public boolean isAuth() {
        return subflow1.isTrue() && subflow2.isOk() && subflow3.isGoodMan();
    }
}
```



## 3.5 享元模式（FlyWeight）

享元模式：<strong style="color:red">运用共享技术有效地支持大量细类度的对象</strong>

> 享元模式最为典型的应用——池技术，例如：字符串常量池、数据库连接池、线程池等

**优缺点分析**

- 优点：
  - 通过池技术极大减少内存中对象数量，使得相同或相似对象在内存中只保存一份，同时还避免了频繁的对象创建与销毁，节约了系统资源，提高了系统性能
  - 外部状态（Extrinsic State）相对独立，不会影响内部状态（Intrinsic State），内外状态相互分离，使得享元对象可以在不同环境中被共享
- 缺点：
  - 对象共享容易引发线程安全问题
  - 对象的状态分离增加了系统的复杂性

享元模式的关键点：<u>引入一个享元工厂，该工厂负责创建和管理享元对象</u>

类图示例

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230609170038347.png" alt="image-20230609170038347" style="zoom:50%;" />



```java
/**
 * 抽象享元类
 */
abstract class BikeFlyWeight {
    // state: 内部状态，0-空闲，1-占用
    protected Integer state = 0;

    // 外部状态
    abstract void ride(String name);

    abstract void back();

    public Integer getState() {
        return state;
    }
}
```



```java
/**
 * 具体享元类
 */
class MoBikeFlyWeight extends BikeFlyWeight {
    // 定义新的内部状态
    private String bikeId;

    public MoBikeFlyWeight(String bikeId) {
        this.bikeId = bikeId;
    }

    @Override
    void ride(String name) {
        state = 1;
        System.out.println(name + "骑着" + bikeId + "自行车出行...");
    }

    @Override
    void back() {
        state = 0;
    }
}
```



```java
/**
 * 享元工厂类
 */

class BikeFlyWeightFactory {
    // 这里使用饿汉式，提前创建好工厂实例，并设置为静态，供外部使用
    private static BikeFlyWeightFactory instance = new BikeFlyWeightFactory();

    // 准备池
    private Set<BikeFlyWeight> pool = new HashSet<>();

    private BikeFlyWeightFactory() {
        for (int i = 0; i < 2; ++i) {
            pool.add(new MoBikeFlyWeight(i + "号"));
        }
    }

    // 暴露公共方法返回实例
    public static BikeFlyWeightFactory getInstance() {
        return instance;
    }

    // 获取池中享元
    public BikeFlyWeight getFlyWeight() {
        for (BikeFlyWeight bike : pool) {
            if (bike.getState() == 0) return bike;
        }
        return null;
    }
}
```



```java
/**
 * 测试程序
 */
BikeFlyWeight bike1 = BikeFlyWeightFactory.getInstance().getFlyWeight();
if (bike1 != null) bike1.ride("张三");

BikeFlyWeight bike2 = BikeFlyWeightFactory.getInstance().getFlyWeight();
if(bike2 != null) bike2.ride("李四");

BikeFlyWeight bike3 = BikeFlyWeightFactory.getInstance().getFlyWeight();
if(bike3 == null) System.out.println("已经没有多余的共享单车...");


// 归还共享单车
assert bike1 != null;
bike1.back();
assert bike2 != null;
bike2.back();
```



## 3.6 代理模式

代理模式：为其他对象提供一种代理以控制对这个对象的访问

简化类图如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230609192541235.png" alt="image-20230609192541235" style="zoom: 50%;" />





```java
interface Subject {
    void doWork();
}

/**
 * 被代理类
 */
class RealSubject implements Subject {
    public void doWork() {
        System.out.println("Hello World");
    }
}

/**
 * 代理类
 */
class ProxySubject implements Subject {
    private RealSubject realSubject;

    public ProxySubject() {
        try {
            this.realSubject = (RealSubject) this.getClass().getClassLoader().loadClass("proxy.RealSubject").newInstance();
        } catch (ClassNotFoundException | InstantiationException | IllegalAccessException e) {
            throw new RuntimeException(e);
        }
    }

    public void doWork() {
        realSubject.doWork();
        System.out.println("woc");
    }
}
```



## 3.7 桥接模式

桥接模式：<strong style="color:red">==抽象==与==实现==解耦，使得两者可以独立地变化</strong>

上面的话有点难以理解，我们换一种表达方式，我们在日常生活中经常涉及金钱交易，我们如果按支付方式可以分为：银行卡支付、微信支付、支付宝支付等；如果按照支付模式，可以分为：一次性支付、分期付款等，他们之间可以相互组合，如果我们按照一对一的方式组合（这里是指具体类一对一），那么除去两个抽象类，一共有 $m\times n$ 种组合方式

```java
/**
 * 支付方式 + 支付模式接口
 */
// 抽象接口
interface PayMethod {
    String method();
}

// 实现接口
interface  PayPattern {
   String pattern();
}

/**
 * 支付类
 */
class PayStrategy {
    private final PayMethod method;
    private final PayPattern pattern;

    public PayStrategy(PayMethod method, PayPattern pattern) {
        this.method = method;
        this.pattern = pattern;
    }

    public void use() {
        System.out.println("我现在使用" + method.method() + "中的" + pattern.pattern() + "的方式进行支付");
    }
}

/**
 * 具体支付方式 + 具体支付模式
 * 这里仅写出两个作为示范
 */
class WeChat implements PayMethod {
    public String method() {
        return "微信支付";
    }
}

class OneTime implements PayPattern {
    public String pattern() {
        return "一次性支付";
    }
}
```

**测试程序**

```java
new PayStrategy(new WeChat(), new OneTime()).use();
```



:sweat_smile:这里偷懒过头了，坏例子应该是两两组合，单独成为一个具体的类，这样除去2个抽象类，一组合就会形成 $m \times n$ 个具体实现类，虽然上述的方式是 $m+n+1$，将原本二阶级别降为一阶级别，但还是和 "正宗" 的桥接模式有所区别，桥接模式的核心是：<strong style="color:red">解耦抽象和实现，两者通过关联关系建立联系，而不是继承关系</strong>，像前面的装饰器也是将继承关系替代为关联关系，只不过桥接模式最终的目的在于==实现==，而装饰器是为了==升级==

> 注意：这里没有区分抽象类和接 口类，其实在 C++ 中也没有接口的概念，Java 中接口的出现主要还是为了解决单继承机制问题

一图胜前言，在没有使用桥接模式之前，我们的组合方式如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230609213658061.png" alt="image-20230609213658061" style="zoom:50%;" />

而架构是这样的

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230609214100961.png" alt="image-20230609214100961" style="zoom:50%;" />

类图就更不需要说了，光是上层就是需要 2 个高层次模块 +  具体实现类 $m + n$ 个模块 + 组合类 $m\times n$ 个模块，相当冗余，使用桥接模式就可以化繁为简，结构也更为清晰，类图如下：

![image-20230609222123276](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230609222123276.png)

**代码实现**

```java
/**
 * 实现接口
 */
interface PayPattern {
    String pay();
}

/**
 * 实现类
 */
class OneTime implements PayPattern {
    public String pay() {
        return "一次性结清";
    }
}

class Instalment implements PayPattern {
    public String pay() {
        return "分期付款";
    }
}

/**
 * 抽象类
 */
abstract class PayMethod {
    protected PayPattern pattern;

    public PayMethod(PayPattern pattern) {
        this.pattern = pattern;
    }
    abstract String pay();

    public void use() {
        System.out.println("我正在使用" + this.pay() + "中的" + pattern.pay() + "作为支付方式");
    }
}

/**
 * 扩展抽象类
 */
class WeChat extends PayMethod {
    public WeChat(PayPattern pattern) {
        super(pattern);
    }

    String pay() {
        return "微信";
    }
}

class AliPay extends PayMethod {
    public AliPay(PayPattern pattern) {
        super(pattern);
    }

    String pay() {
        return "支付宝";
    }
}
```



**测试程序**

```java
PayMethod weChat_OneTime = new WeChat(new OneTime());
PayMethod alipay_Instalment = new AliPay(new Instalment());
weChat_OneTime.use();
alipay_Instalment.use();
```



:thinking:**分析**

1. 优点
   - 分离了抽象与实现
   - 提高了可扩展性
   - 符合开闭原则和合成复用原则
2. 缺点
   - 桥接模式强调对象要拥有两个维度的变化，简化了多级继承关系，但同时增加了聚合对象的内聚方法，具有一定的局限性，并且提高了系统程序的复杂程度





# 4 行为模式

## 4.1 责任链模式

责任链模式（Chain Of Responsibility）：<strong style="color:red">是一种处理请求的模式，它让多个处理器都有机会处理该请求，直到其中某个处理请求成功为止。责任链模式把多个处理器串成链，然后让请求在链上传递</strong>

> 责任链模式的经应用场景有：OA 系统中的审批制度、Java Web 中的过滤器

**类图**

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230616150942811.png" alt="image-20230616150942811" style="zoom:50%;" />



**示例代码**

```java
abstract class Handler {
    protected Handler nextHandler;

    public void setNextHandler(Handler nextHandler) {
        this.nextHandler = nextHandler;
    }

    public abstract void process(Integer info);
}

class Leader extends Handler {
    public void process(Integer info) {
        if(info >= 1 && info < 10) {
            System.out.println("Leader 直接处理...");
        } else {
            nextHandler.process(info);
        }
    }
}

class Boss extends Handler {
    public void process(Integer info) {
        System.out.println("事件等级过高，交由Boss 处理");
    }
}
```



**测试程序**

```java
Handler leader = new Leader();
Handler boss = new Boss();
leader.setNextHandler(boss);


leader.process(6);
leader.process(10);
```



**总结**

- 优点：将请求和处理分离，请求者不需要知道由谁来处理，也不需要知道请求的全貌；其次可以提高系统的灵活性，新增处理器，对整条请求链的代价是非常小的
- 缺点：会将降低系统的性能，每个请求都需要从链头走到链尾，无法一步到尾，当链的长度过长时，系统的性能将大幅度下降；由于责任链的实现方式类似于递归，所以将给调试带来极大不便



## 4.2 命令模式

命令模式：将一个请求封装为一个对象，从而让你使用不同的请求把客户端参数化，对请求排队或者记录请求日志，可以提供命令的撤销和恢复功能。

**类图**

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230616172739189.png" alt="image-20230616172739189" style="zoom:50%;" />



**实现代码**

```java
interface Command {

    void execute();
}

class ConcreteCommand implements Command {
    private Receiver receiver;

    public ConcreteCommand(Receiver receiver) {
        this.receiver = receiver;
    }
    @Override
    public void execute() {
        receiver.action();
    }

}

/**
 * 接收者：真正执行命令的人
 */
class Receiver {
    public void action() {
        System.out.println("command action!!!");
    }
}

/**
 * 调用者：传达命令给接收者
 */
class Invoker {
    private Command command;

    public Invoker(Command command) {
        this.command = command;
    }

    public void action() {
        command.execute();
    }
}
```



**测试程序**

```java
ConcreteCommand concreteCommand = new ConcreteCommand(new Receiver());
Invoker invoker = new Invoker(concreteCommand);
invoker.action();
```



## 4.3 迭代器模式

迭代器模式用于提供一个顺序访问集合对象的元素，使用者无需知道其底层的具体实现。迭代器模式在编程语言中被广泛运用，包括 .Net、Java、C++、python 等



## 4.4 中介者模式

中介者模式：一种行为型设计模式，能够<strong style="color:red">减少对象之间混乱无序的依赖关系</strong>。该模式会限制对象之间的直接交互，迫使它们通过一个中介者对象进行合作。



<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230616204521226.png" alt="image-20230616204521226" style="zoom:50%;" />

**代码实现**

```java
// 模拟婚姻中介事务所

/**
 * 中介接口
 */
interface MarriageAgency {
    void register(Person person);    // 注册
    void pair(Person person);    // 配对
}

enum Gender {
    MALE,
    FEMALE
}
class Person {
    String name;    // 姓名
    int age;    // 年龄
    Gender gender;  // 性别
    int requestAge; // 要求年龄
    MarriageAgency marriageAgency; // 中介

    public Person(String name, int age, Gender gender, int requestAge, MarriageAgency marriageAgency) {
        this.name = name;
        this.age = age;
        this.gender = gender;
        this.requestAge = requestAge;
        this.marriageAgency = marriageAgency;
        this.marriageAgency.register(this);
    }

    // 寻找对象
    public void findPartner() {
        marriageAgency.pair(this);
    }
}

/**
 * 中介实现类
 */
class MarriageImpl implements MarriageAgency {
    List<Person> member = new ArrayList<>();


    @Override
    public void register(Person person) {
        member.add(person);
    }

    @Override
    public void pair(Person person) {
        for(Person m : member) {
            if(person.gender != m.gender && m.age == person.requestAge) {
                System.out.println(person.name + "与" + m.name + "结成姻缘！！！");
                break;
            }
        }
    }
}
```



**测试程序**

```java
MarriageAgency agency = new MarriageImpl();

Person reba = new Person("迪丽热巴", 20, Gender.FEMALE, 20, agency);
Person mi = new Person("杨幂", 24, Gender.FEMALE, 26, agency);
Person zhao = new Person("赵丽颖", 22, Gender.FEMALE, 20, agency);
Person guo = new Person("郭老师", 30, Gender.FEMALE, 40, agency);

Person gIao = new Person("gIao", 40, Gender.MALE, 30, agency);
gIao.findPartner();
```



## 4.6 状态模式

状态模式：允许一个对象在其内部状态改变时改变它的行为，对象看起来似乎修改了它的类，其别名为状态对象，状态对象是一种对象行为型模式。

**类图**

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230617112009908.png" alt="image-20230617112009908" style="zoom:50%;" />



**代码实现**

```java
/**
 * 抽象状态类
 */
abstract class State {
    abstract void doWork();
}

/**
 * 具体状态类
 */
class Happy extends State {
    @Override
    public void doWork() {
        System.out.println("积极主动");
    }
}

class Angry extends State {
    @Override
    public void doWork() {
        System.out.println("无精打采");
    }
}

class Sad extends State {
    @Override
    public void doWork() {
        System.out.println("消极怠工");
    }

}

/**
 * 上下文类：对状态进行引用，并且可以改变内部的状态
 */
class Context {
    private State state;

    public void changeState(State state) {
        this.state = state;
    }

    public void doSomething() {
        state.doWork();
    }
}
```



**测试类**

```java
Context zs = new Context();
zs.changeState(new Happy());
zs.doSomething();
zs.changeState(new Angry());
zs.doSomething();
zs.changeState(new Sad());
zs.doSomething();
```



**分析**

- 优点：容易新增新的状态类，封装了状态转移规则，每个状态可以被共享和复用，避免了大量的 if……else 判断
- 缺点：实现较为复杂，状态过多时会导致类的对象的个数增多

## 4.7 策略模式

策略模式：定义一组算法，将每个算法都封装起来，并且使他们之间可以互换。策略模式让算法独立于使用它的客户变化而变化，也称为政策模式（Policy）

:bell:**区分策略模式和状态模式**

- 驱动条件不同：策略模式中类的功能是根据当前的条件主动更改，而状态模式中则是根据状态的不同发生变化
- 关联度不同：策略模式中状态之间相互独立，而状态模式各状态之间存在关联，状态机就是描述他们之间的关联关系

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230617152500042.png" alt="image-20230617152500042" style="zoom:50%;" />

**代码实现**

```java
/**
 * 抽象策略接口
 */
interface Compression {
    void doCompression();
}

/**
 * 快速压缩算法
 */
class Rapid implements Compression {
    @Override
    public void doCompression() {
        System.out.println("采用快速压缩算法");
    }
}

/**
 * 高效压缩算法
 */
class Efficient implements Compression {
    @Override
    public void doCompression() {
        System.out.println("采用高效压缩算法");
    }
}

/**
 * 环境类：压缩算法集成软件 WinRar
 */
class WinRar {
    private Compression compression;

    public WinRar(Compression compression) {
        this.compression = compression;
    }

    public void changeStrategy(Compression compression) {
        this.compression = compression;
    }

    public void compress() {
        compression.doCompression();
    }
}
```



## 4.8 访问者模式

访问者模式：表示一个作用于某对象结构中的各元素的操作，他可以在不改变这个对象结构的前提下定义作用于其内部各个元素的新操作。

**类图**

![image-20230623203810179](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230623203810179.png)



**代码实现**

```java
/**
 * 接收类：即被访问类
 */
abstract class Hardware {
    String command;

    public Hardware(String command) {
        this.command = command;
    }

    // 使用方法
    public void run() {
        System.out.println(command);
    }

    // 提供被访问的入口方法
    public abstract void accept(Visitor visitor);
}

class CPU extends Hardware {
    public CPU(String command) {
        super(command);
    }

    public void accept(Visitor visitor) {
        visitor.visitCPU(this);
    }
}

class Disk extends Hardware {
    public Disk(String command) {
        super(command);
    }

    public void accept(Visitor visitor) {
        visitor.visitDisk(this);
    }
}
/**
 * 访问接口
 */
interface Visitor {
    void visitCPU(Hardware hw);

    void visitDisk(Hardware hw);
}

class UpdateVisitor implements Visitor {
    @Override
    public void visitCPU(Hardware hw) {
        hw.command += "1 + 1 = 2";
    }

    @Override
    public void visitDisk(Hardware hw) {
        hw.command += "记住 1 + 1 = 2";
    }
}
```

**测试程序**

```java
Hardware cpu = new CPU("1 + 1 = 1");
Hardware disk = new Disk("记住 1 + 1 = 1");

System.out.println("--------------");
cpu.run();
disk.run();
Visitor updateVisitor = new UpdateVisitor();
cpu.accept(updateVisitor);
disk.accept(updateVisitor);

System.out.println("--------------");
cpu.run();
disk.run();
```



## 4.9 备忘录模式

备忘录模式：一种行为设计模式，允许在不暴露对象实现细节的情况下保存和恢复对象之间的状态。

