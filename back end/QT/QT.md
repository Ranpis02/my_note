[toc]

# 1 QT 基础

## 1.1 安装 QT

从 QT5.15 开始，官方网站已经不再提供离线安装包了，推荐我们使用 QT 维护工具（Qt Maintenance Tool）在线安装 QT

> 官方提供的在线安装器地址：https://download.qt.io/archive/online_installers/

![image-20230515230806892](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230515230806892.png)

![image-20230515230932709](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230515230932709.png)

---

:star:中间插入一步（重要）：配置下载器的镜像

下载好安装器后，在如下位置键入 cmd，输入以下命令：

```bash
installer.exe --mirror https://mirrors.tuna.tsinghua.edu.cn/qt
```

<img src="https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230515231621708.png" alt="image-20230515231621708"  />

> installer.exe 换成下载器程序名

---

01 进入下载安装界面（首先我们需要注册一个 QT 账号）

![image-20230515231111723](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230515231111723.png)

02 选择安装目录和安装方式

![image-20230515231942778](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230515231942778.png)

03 选择套件

![image-20230515232658782](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230515232658782.png)

04 之后一路回车下载即可

> 最后特别要注意的一点就是：安装路径中不要存在中文。其实只要我们使用到了 C++ 编译器，那么路径中最好就不要出现中文或其他特殊字符，否则调试会出问题

## 1.2 设置 QT 环境变量

找到 QT 安装目录中 mingw_64 下的 bin 目录

![image-20230515233956398](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230515233956398.png)

将路径添加到系统环境变量（或用户变量，同等情况下，用户环境变量先生效）中

除此之外，工具套件中的 mingw 下的 bin 目录，我们也要将其添加到环境变量中，里面含有 gcc.exe 和 g++.exe

![image-20230515234538520](https://new-blog-image.oss-cn-hangzhou.aliyuncs.com/img/image-20230515234538520.png)

## 1.3 QT 简介

跨平台的 C++ 应用程序开发框架，<u>被广泛应用与 GUI 程序，并且提供丰富的 API 和 2D / 3D 图形渲染，支持 OpenGL，扩展性良好</u>

> 官方文档：https://doc.qt.io/qt-6.2/reference-overview.html

请注意：

- Qt 是标准 C++ 的扩展，C++ 的语法在 Qt 中都是支持的

- 良好封装机制使得 Qt 的模块化程度非常高，可重用性较好，可以快速上手。
- Qt 提供了一种称为 signals/slots 的安全类型来替代 callback（回调函数），这使得各个元件 之间的协同工作变得十分简单。



QT 中各种模块

- Qt 基本模块（Qt Essentials)：提供了 Qt 在所有平台上的基本功能
- Qt 附加模块（Qt Add-Ons)：实现一些特定功能的提供附加价值的模块
- 增值模块（Value-AddModules)：单独发布的提供额外价值的模块或工具
- 技术预览模块（Technology Preview Modules）：一些处于开发阶段，但是可以作为技术预览使用的模块
- Qt 工具（Qt Tools)：帮助应用程序开发的一些工具

QT 基本模块如下：

| 模块                  | 描述                                               |
| --------------------- | -------------------------------------------------- |
| `Qt Core`             | Qt 类库的核心，所有其他模块都依赖于此模块          |
| `Qt GUI`              | 设计 GUI 界面的基础类，包括 OpenGL                 |
| Qt Multimedia         | 音频、视频、摄像头和广播功能的类                   |
| Qt Multimedia Widgets | 实现多媒体功能的界面组件类                         |
| Qt Network            | 使网络编程更简单和轻便的类                         |
| Qt QML                | 用于 QML 和 JavaScript 语言的类                    |
| Qt Quick              | 用于构建具有定制用户界面的动态应用程序的声明框架   |
| Qt Quick Controls     | 创建桌面样式用户界面，基于 Qt Quick 的用户界面控件 |
| Qt Quick Dialogs      | 用于 Qt Quick 的系统对话框类型                     |
| Qt Quick Layouts      | 用于 Qt Quick 2 界面元素的布局项                   |
| Qt SQL                | 使用 SQL 用于数据库操作的类                        |
| Qt Test               | 用于应用程序和库进行单元测试的类                   |
| `Qt Widgets`          | 用于构建 GUI 界面的 C++ 图形组件类                 |



## 1.4 QT 项目分析

当我们新建一个 QT Widgets Designer From Class 时，会得到三份文件，分别是 `.h`， `.cpp` 和 `.ui`



QT 中对象的继承关系如下：

<img src="https://thinkbook16-blog-img.oss-cn-zhangjiakou.aliyuncs.com/img_for_typora/image-20241122005202185.png" alt="image-20241122005202185" style="zoom:50%;" />







# 2 高级功能

## 2.1 信号槽机制

### 2.1.1 原生

信号槽机制的核心思想为：**观察者模式**

连接信号和槽的 `connect()` 函数原型如下, 其中 `PointerToMemberFunction` 是一个指向函数地址的指针

```c++
QMetaObject::Connection QObject::connect(
    	const QObject *sender, PointerToMemberFunction signal, 
        const QObject *receiver, PointerToMemberFunction method, 
		Qt::ConnectionType type = Qt::AutoConnection);
参数:
  - sender:   发出信号的对象
  - signal:   属于sender对象, 信号是一个函数, 这个参数的类型是函数
              指针, 信号函数地址
  - receiver: 信号接收者
  - method:   属于receiver对象, 当检测到sender发出了signal信号, 
              receiver对象调用method方法，信号发出之后的处理动作
 
//  参数 signal 和 method 都是函数地址, 因此简化之后的 connect() 如下:
connect(const QObject *sender, &QObject::signal, 
        const QObject *receiver, &QObject::method);
```



下面以一个 `点击按钮实现关闭窗口` 的案例进行示范使用方法：

1. 创建一个新项目，打开 `.ui` 文件，将一个 `Push Button` 拖进去，注意查看窗口的 objectName 信息，我们将其修改为 `closeBtn`

   <img src="https://thinkbook16-blog-img.oss-cn-zhangjiakou.aliyuncs.com/img_for_typora/image-20241122003754894.png" alt="image-20241122003754894" style="zoom:50%;" />

2. 在 MainWindow 中添加对应的连接函数

   ```c++
   MainWindow::MainWindow(QWidget* parent)
       : QMainWindow(parent)
       , ui(new Ui::MainWindow)
   {
       ui->setupUi(this);
   
       /**
        * 信号的发送者：ui->closeBtn, 其中 ui 对象对应我们的 UI 文件
        * 发送时间函数的引用：&PushButton::clicked
        * 信号的接受者：当前窗口的实例
        * 接收信号后处理时间的函数：MainWindow::close
        */
       connect(ui->closeBtn, &QPushButton::clicked, this, &MainWindow::close);
   }
   ```

3. 编译运行，当我们点击窗口中的关闭按钮时，可以看到窗口就已经被关闭了



### 2.1.2 自定义信号槽

如果想要在QT类中自定义信号槽, 需要满足一些条件, 并且有些事项也需要注意:

1. 要编写新的类并且让其继承 Qt 的某些标准类
2. 这个新的子类必须从 `QObject` 类或者是 `QObject` 子类进行派生
3. 在定义类的头文件中加入 `Q_OBJECT` 宏

```c++
// 在头文件派生类的时候，首先像下面那样引入Q_OBJECT宏：
class MyMainWindow : public QWidget
{
    Q_OBJECT
    ......
}
```

#### 自定义信号

自定义信号的要求和注意事项：

1. 信号是类的成员函数
2. 返回值必须是 void 类型
3. 信号的名字可以根据实际情况进行指定
4. 参数可以随意指定, 信号也支持重载
5. 信号需要使用 signals 关键字进行声明, 使用方法类似于 public 等关键字
6. 信号函数只需要声明, 不需要定义(没有函数体实现)
7. 在程序中发射自定义信号: 发送信号的本质就是调用信号函数
   1. 习惯性在信号函数前加关键字: emit, 但是可以省略不写
   2. emit只是显示的声明一下信号要被发射了, 没有特殊含义
   3. 底层 emit == #define emit

```c++
// 举例: 信号重载
// Qt中的类想要使用信号槽机制必须要从QObject类派生(直接或间接派生都可以)
class Test : public QObject
{
    Q_OBJECT
signals:
    void testsignal();
	// 参数的作用是数据传递, 谁调用信号函数谁就指定实参
	// 实参最终会被传递给槽函数
    void testsignal(int a);
};
```



#### 自定义槽

1. 返回值必须是 void 类型

2. 槽也是函数, 因此也支持重载

3. 槽函数需要指定多少个参数, 需要看连接的信号的参数个数

4. 槽函数的参数是用来接收信号传递的数据的, 信号传递的数据就是信号的参数

   - 举例:
     - 信号函数: `void testsig(int a, double b);`
     - 槽函数: `void testslot(int a, double b);`
   - 总结:
     - 槽函数的参数应该和对应的信号的参数个数, 从左到右类型依次对应
     - 信号的参数可以大于等于槽函数的参数个数 == 信号传递的数据被忽略了
       - 信号函数: `void testsig(int a, double b);`
       - 槽函数: `void testslot(int a);`

5. Qt中槽函数的类型是多样的
   - Qt中的槽函数可以是类的成员函数、全局函数、静态函数、Lambda表达式（匿名函数）

6. 槽函数可以使用关键字进行声明: slots (Qt5中slots可以省略不写)
   1. `public slots`
   2. `private slots` –> 这样的槽函数不能在类外部被调用
   3. `protected slots` –> 这样的槽函数不能在类外部被调用



