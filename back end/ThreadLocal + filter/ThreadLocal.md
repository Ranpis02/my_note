[toc]

# 1 ThreadLocal 介绍

1. ThreadLocal 的作用：可以实现<strong style="color:red">在同一个线程数据共享</strong>，从而解决多线程数据安全的问题
2. ThreadLocal 可以给当前线程关联一个数据（普通变量、对象、数组）set 方法
3. ThreadLocal 可以像 Map 一样存取数据，key 为当前线程（通过 `get()` 方法取值）
4. 每一个 ThreadLocal 对象，只能为当前线程关联一个数据，如果要为当前线程关联多个数据，就需要使用多个 ThreadLocal 对象实例
5. 每个 ThreadLocal 对象实例定义的时候，一般为 static 类型
6. ThreadLocal 中保存数据，在线程销毁后会自动释放



## 1.1 环境搭建

目录结构如下：

```
threadLocal        
├─ Dog.java： 测试数据类1        
├─ Pig.java： 测试数据类2
├─ T1.java：线程类        
├─ T1DAO.java：Service调用的 DAO      
└─ T1Service.java：线程调用的 Service  
```

代码如下：

```java
public class T1 {
    // 创建 ThreadLocal 对象
    public static ThreadLocal<Object> threadLocal1 = new ThreadLocal<>();

    // Task 是线程内部类
    public static class Task implements Runnable {
        @Override
        public void run() {
            Dog dog = new Dog();
            Pig pig = new Pig();
            // 给 threadLocal1 对象放入dog
            threadLocal1.set(dog);
            System.out.println("Task 放入了 dog =" + dog);
            System.out.println("在 run 方法中线程：" + Thread.currentThread().getName());
            new T1Service().update();
        }
    }

    public static void main(String[] args) {
        new Thread(new Task()).start(); // 在主线程中启动一个新的线程
    }
}
```

```java
public class T1DAO {
    public void update() {
        // 取出T1线程的数据
        Object dog = T1.threadLocal1.get();
        System.out.println("T1DAO  中取出的数据为 = " + dog);

        String name = Thread.currentThread().getName();
        System.out.println("在T2DAO的update() 线程是：" + name);
    }
}
```

```java
public class T1Service {
    public void update() {
        // 取出T1线程的数据
        Object dog = T1.threadLocal1.get();
        System.out.println("T1Service 中取出的数据为 = " + dog);

        // 获取当前线程名
        String name = Thread.currentThread().getName();
        System.out.println("在T1Service的update() 线程是：" + name);
        // 调用了 dao 的update方法
        new T1DAO().update();
    }
}
```

**说明**

- 我们在 T1 中创建 ThreadLocal 对象，该对象是一个静态的，同时创建一个内部类，该类是实现了 Runnable 接口，因此是一个线程类
- 我们在主线程中开启了 Task 线程类，因此会创建出一个 Thread-0 子线程，在这个子线程中由于 T1 调用 Service，而 Service 又调用了 DAO 
- 我们测试的便是在一个线程中，通过 ThreadLocal 能够拿到同一个数据，即 ThreaLocal 能够在一个线程中保证数据的传递



**测试结果**

```
Task 放入了 dog =com.xzh.threadLocal.Dog@781d0f57
在 run 方法中线程：Thread-0
T1Service 中取出的数据为 = com.xzh.threadLocal.Dog@781d0f57
在T1Service的update() 线程是：Thread-0
T1DAO  中取出的数据为 = com.xzh.threadLocal.Dog@781d0f57
在T2DAO的update() 线程是：Thread-0
```

可以看到，结果是符合预期的，利用 ThreadLocal 我们可以从**同一个线程中保证数据传递的一致性**



## 1.2 源码解读

**set 方法源码解读**

```java
public void set(T value) {
    // 1. 获取当前线程，关联到当前线程
    Thread t = Thread.currentThread();
    // 2. 通过线程对象，获取到 ThreadLocalMap
    ThreadLocalMap map = getMap(t);
    // 3.如果如果没有拿到 t 对应 map , 那么创建一个新的 map 容器，key 为当前线程，value 为要共享的数据；如果拿到对应的map,那么也是同样的道理，其中 this 指向的是当前线程，value 在此时会被替换
    if (map != null) {
        map.set(this, value);
    } else {
        createMap(t, value);
    }
}
```

这个设计还是比较绕的，具体来说就是，当我们创建一个了线程 thread 后，这个 thread 里面有一个属性为 threadLocals，类型为 threadLocalMap，里面维护一个 table,table 里面存放的数据类型为 entry，key 为线程，value 是保存的值



**get 方法解读**

```java
public T get() {
    // 1. 拿到当前的线程
    Thread t = Thread.currentThread();
    // 2. 通过 getMap 得到对应的 map 容器
    ThreadLocalMap map = getMap(t);
    if (map != null) {
        // 3. 如果 map 不为空，那么通过取出当前线程为 key 的 value ,即共享数据
        ThreadLocalMap.Entry e = map.getEntry(this);
        if (e != null) {
            @SuppressWarnings("unchecked")
            T result = (T)e.value;
            return result;
        }
    }
    return setInitialValue();
}
```

