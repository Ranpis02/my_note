[toc]

# 1 面向对象编程

## 1.1 两大编程思想

- 面向过程
- 面向对象

### 1.1.1 面向过程

面向过程编程，英文名为 <font color="orange">Process-oriented programming </font>，简称为 POP，面向过程就是<font color="red">分析出解决问题所需要的步骤，然后用函数把这些步骤一步一步实现，使用的时候一个一个调用即可</font>

**举例：把大象放入冰箱采用POP思想剖析**

1. 打开冰箱门
2. 大象装进去
3. 关上冰箱门



### 1.1.2 面向对象编程

面向对象编程，英文名为 <font color="orange">Object-oriented Programming </font>，简称为 OOP，即<font color="red">把事务分解为一个个对象，然后由对象之间分工与合作</font>

**举例：将大象放入冰箱采用OOD剖析**

1. 大象对象
   - 进去
2. 冰箱对象
   - 打开
   - 关闭
3. 使用大象和冰箱的功能

<font color="red">面向对象是以对象功能来划问题，而不是步骤</font>



## 1.2 OOP

在面向对象程序开发思想中，每一个对象都是功能中心，就有明确的分工

面向对象编程具有==灵活、代码可复用、容易维护和开发==的特点，更适合多人合作的大型软件项目

面向对象的特性：

- 封装性
- 继承性
- 多态性

## 1.3 面向对象和面向过程的对比

**面向过程**

- 优点：<font color="red">性能相比面向对象高</font>，适合跟硬件联系地很紧密的东西，例如单片机就采用面向过程编程
- 缺点：没有面向对象易维护、易使用、易扩展

**面向对象**

- 优点：<font color="red">易维护、易复用、易扩展</font>，由于面向对象有封装、继承、多态性的特性，可以设计出低耦合的系统，使系统更加灵活、更加易于维护
- 缺点：性能比面向过程低

## 1.4 类和对象

### 1.4.1 对象

在 JS 中，对象是<font color="red">一组无序的相关属性和方法的集合</font>，所有的事务都是对象

对象是由属性和方法组成的：

- 属性：事物的==特征==，在对象中用==属性==来表示
- 方法：事务的==行为==，在对象中用==方法==来表示

### 1.4.2 类 class

<font color="red">类抽象了对象的公共部分，它泛指某一大类</font>，我们总结一下就是：

- 类是对对象的抽象
- 对象是类的实例化



## 1.5 创建类和生成实例

创建类

```js
class className {
	// class body
}
```

创建实例：

```js
var xx = new name();
```

> 注意：类必须使用 new 实例化对象

类里面有一个 constructor 函数，可以接收传递过来的参数，同时返回实例对象

**demo**

```js
// 1. 创建类 class
class Star {
    // 使用 constructor 构造函数
    constructor(uname, uage) {
        this.uname = uname;
        this.uage = uage;
    }
}

// 2. 使用类创建对象 new
var ldh = new Star('刘德华', 18);
console.log(ldh);
```

执行结果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221011161215947.png" alt="image-20221011161215947" style="zoom: 50%;" />

## 1.6 类中添加共有方法

我们在类里面添加方法时有以下注意点：

1. 类中所有的函数都不需要写 function
2. 多个函数方法之间不需要添加逗号分隔

```js
// 1. 创建类 class
class Star {
    // 使用 constructor 构造函数
    constructor(uname, uage) {
        this.uname = uname;
        this.uage = uage;
    }

    sing(song) {
        console.log(this.uname + '唱' + song);
    }
}

// 2. 使用类创建对象 new
var ldh = new Star('刘德华', 18);
ldh.sing('冰雨');
```

执行结果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221011162226884.png" alt="image-20221011162226884" style="zoom: 50%;" />

## 1.7 继承

继承：即<u>子类可以继承父类的一些属性和方法</u>，语法如下：

```js
class Fahter {	// 父类

}
class Son extends Father { 	// 子类

}
```

**demo**

```js
// demo1
// 1. 创建父类
class Father {
    constructor(x, y) {
        this.x = x;
        this.y = y;
    }
    // 创建 sum 方法,该方法用于做加法运算
    sum(x, y) {
        console.log(x + y);
    }
}
// 2. 创建子类
class Son extends Father {
} 
var son = new Son();
son.sum(1, 2);
```

执行结果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221011164652216.png" alt="image-20221011164652216" style="zoom:50%;" />

即 Son 这个子类将 Father 父类中所有的属性和方法都继承了过来

```
// 1. 创建父类
class Father {
    constructor(x, y) {
        this.x = x;
        this.y = y;
    }
    // 创建 sum 方法,该方法用于做加法运算
    sum(x, y) {
        console.log(x + y);
    }
}
// 2. 创建子类
class Son extends Father {
    //@ 为 Son 这个子类也添加一个构造函数
    constructor(x, y) {
        this.x = x;
        this.y = y;
    }
    //@@@@@@@@@@@@@@@@@@@@@@@@@@@@
} 
var son = new Son();
son.sum(1, 2);
```

执行结果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221011165644525.png" alt="image-20221011165644525" style="zoom:33%;" />

浏览器提示我们必须在派生类中调用一个 super 构造器来

这是由于 Son 中的 this 指向的是 Son 中的 constructor， 而 Father 中的 sum 方法中的 x 和 y 指向的是 Father 中的 constructor，我们<font color="red">如果想要访问和调用父类上的函数，如果使用 super 关键字</font>

```js
// 1. 创建父类
class Father {
    constructor(x, y) {
        this.x = x;
        this.y = y;
    }
    // 创建 sum 方法,该方法用于做加法运算
    sum(x, y) {
        console.log(x + y);
    }
}
// 2. 创建子类
class Son extends Father {
    //@ 使用 super 调用
    constructor(x, y) {
        super(x, y);      // 调用了父类中的 constructor, 将 x 和 y 传递给父类
    }
    //@@@@@@@@@@@@@@@@@@@@@@@@@@@@
} 
var son = new Son();
son.sum(1, 2);
```

执行结果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221011170804114.png" alt="image-20221011170804114" style="zoom:33%;" />

## 1.8 *super 关键字

<font color="red">super 关键字用于访问和调用对象父类上的函数，也可以调用父类的构造函数，也可以调用父类的普通函数</font>

### 1.8.1 super 关键字调用父类普通函数

在继承中，如果实例化子类执行一个方法，会先去子类中检索是否有这个方法，<font color="red">如果子类中有这个方法就先执行子类，如果子类中没有，就执行父类中的这个方法</font>

```js
// 1. 创建父类
class Father {
    say() {
        console.log('我是爸爸');
    }
}
// 2. 创建子类
class Son extends Father {
    say() {
        console.log('我是儿子');
    }
} 
var son = new Son();
son.say();
```

执行结果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221011172245469.png" alt="image-20221011172245469" style="zoom:50%;" />

如果我们想要调用父类中的这个方法，这时候我们就可以使用 super 关键字

```js
// 1. 创建父类
class Father {
    say() {
        return '我是爸爸';
    }
}
// 2. 创建子类
class Son extends Father {
    say() {
        console.log(super.say() + '的儿子');
    }
} 
var son = new Son();
son.say();
```

执行结果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221011172544720.png" alt="image-20221011172544720" style="zoom:50%;" />

### 1.8.2 使用 super 的注意事项

<font color="red">子类的构造函数中使用 super，必须放在 this 前面（先调用父类中的构造方法，再使用子类构造方法）</font>

```js
class Person {
    // 父类
    constructor(surname) {
        this.surname = surname;
    }
}
class Student extends Person {
    // 子类继承父类
    constructor(surname, firstname) {
        super(surname);      // 调用父类的 constructor 
        this.firstname = firstname;     // 定义子类独有的属性   
    }
}
```

**demo**

```js
// 为子类扩展减法操作
class Father {
    constructor(x, y) {
        this.x = x;
        this.y = y;
    }
    sum() {
        console.log(this.x + this.y);
    }
}

class Son extends Father {
    constructor(x, y) {
        super(x, y);        // 利用 super 调用父类的构造函数
        this.x = x;
        this.y = y;
    }
    subtract() {
        console.log(this.x - this.y);
    }
}

var son = new Son(1, 2);
son.sum();
son.subtract();
```

执行结果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221011184514761.png" alt="image-20221011184514761" style="zoom:33%;" />



## 1.9 使用类的注意点

1. 在 ES6 中类没有变量提升，所以必须先定义类，才能通过类实例化对象
2. 类中共有的属性和方法一定要加 this，this 指向的就是具体的实例对象
3. 注意类里面 this 的指向问题：constructor 里面的 this 指向实例对象，方法里面的 this 指向的是这个方法的调用者

```js
<button>点击</button>
<script>
    var that;
    class Star {
        constructor(uname, uage) {
            that = this;
            this.uname = uname;
            this.uage = uage;
            this.btn = document.querySelector('button');    // 给这个类添加一个属性，这个属性用来注册元素
            this.btn.onclick = this.sing;       // 给 btn 这个属性添加绑定事件和事件处理程序
            // 注意：这里不能给 sing 后面添加圆括号，否则还没等事件触发，sing 方法就已经被调用了
        }
        // 添加一个 sing 方法，sing 方法里面的 this 指向的是方法的调用者
        // 由于前面已经将 sing 方法绑定到 btn 中，所以这里的 this 指向的是 btn
        sing() {
            console.log(this);
            console.log(that.uname);
        }

        // 添加一个 dance 方法，由于 dance 方法没有绑定事件，所以 this 指向的是 实例对象
        dance() {
            console.log(this);
            console.log(this.uage);
        }
    }

    var ldh = new Star('刘德华', 19);
    ldh.dance();
</script>
```

执行结果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221011192705519.png" alt="image-20221011192705519" style="zoom:50%;" />



## 1.10 面向对象 tab 栏切换



# 2 构造函数和原型

在典型的 OOP 的语言中，都存在类的概念，类就是对象的模板，对象就是类的实例，但是在 ES6 之前，JS 并没有引入类的概念

在 ES6 之前，对象不是基于类创建的，而是一种称为<font color="red">构建函数</font>的特殊函数来定义对象和它们的特征

创建对象可以通过以下三种方式：

1. 对象字面量
2. new Object()
3. 自定义构造函数

## 2.1 构造函数

<font color="red">构造函数</font>是一种特殊的函数，主要用来初始化对象，即为对象成员变量赋初值，它总是与 new 关键字一起使用，如下：

```js
// 构造函数
function Star(uname, uage) {
    this.uname = uname;
    this.age = uage;
    this.sing = function() {
        console.log('我会唱歌');
    }
}

var ldh = new Star('刘德华', 19);
console.log(ldh);
```

执行结果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221011200046624.png" alt="image-20221011200046624" style="zoom:50%;" />

在 JS 中，使用构造函数时要注意以下两点：

1. 构造函数用于创建某一类对象，<font color="red">其首字母需要大写</font>
2. 构造函数要和 <font color="red">new 关键字一起使用</font>



## 2.2 静态成员和实例成员

JS 的构造函数中可以添加一些成员，可以在构造函数本身上添加，也可以在构造函数内部的 this 上添加，通过这两种方式添加的成员，分别被称为<font color="red">静态成员</font>和<font color="red">实例成员</font>

- 静态成员：<font color="orange">在构造函数本身上添加的成员称为静态成员，只能由构造函数本身来访问</font>
- 实例成员：<font color="orange">在构造函数内部创建的对象成员称为实例成员，只能由实例化的对象来访问</font>

```js
function Person(height, weigth) {
    this.height = height;
    this.weigth = weigth;
    this.sing = function() {
        console.log('I can sing, dance and rap');
    }
}
var cxk = new Person(666, 666);
console.log(cxk.height);
cxk.sing();
console.log(Person.height);

Person.heigth = 180;
console.log(Person.heigth);
```

执行结果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221011203005850.png" alt="image-20221011203005850" style="zoom:50%;" />



## 2.3 构造函数原型对象 prototype

构造函数方法很好用，但是存在一个严重的问题：<font color="red">浪费内存</font>

如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221011204059406.png" alt="image-20221011204059406" style="zoom:50%;" />

在 Star 方法中添加了一个方法，我们使用构造函数创建一个对象除了对象本身的空间还需要开辟一额外的空间用于存储函数，当创建的对象足够多的时候，就会十分浪费内存空间，为了解决这一问题，我们引入<font color="red">原型</font>

### 2.3.1 构造函数原型

构造函数通过原型分配的方法是所有对象所<font color="red">共享的</font>

JS 规定，每一个构造函数都有一个 prototype 属性，指向另一个对象，<font color="orange">注意这个 prototype 就是一个对象，这个对象的所有属性和方法，都会被构造函数所拥有</font>

<font color="red">我们可以把那些不变的方法，直接定义在 prototype 对象上，这样所有对象的实例就可以共享这些方法</font>

**demo**

```js
function Star(uname, uage) {
    this.uname = uname;
    this.uage = uage;
}
Star.prototype.sing = function() {
    console.log('我会唱歌');
}
var ldh = new Star('刘德华', 19);
var zxy = new Star('张学友', 20);
ldh.sing();
console.log(ldh.sing === zxy.sing);
```

执行结果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221011205805114.png" alt="image-20221011205805114" style="zoom:50%;" />



**总结**

1. 原型对象是什么？

   答：是一种对象，每一个构造函数都会内置一个原型对象

2. 原型对象的作用？

   答：用来共享方法

3. 使用原型对象注意点

   答：一般情况下，我们的公共属性定义到构造函数里面，而公共的方法我们放到原型对象身上



### 2.3.2 对象原型 `__proto__`

对象都会有一个属性 `__proto__` 指向构造函数的 prototype 原型对象，之所以我们的对象可以使用构造函数 prototype 原型对象的属性和方法，就是因为对象有 `__proto__`属性的存在

- `__proto__`对象原型和原型对象 prototype 是等价的
- `__proto__`对象原型的意义就在于为对象的查找机制提供一个方向，或者说一条路线，但是它是一个非标准属性，因此在实际开发中，不可以使用这个属性，它只是内部指向原型对象 prototype

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221011212218397.png" alt="image-20221011212218397" style="zoom:33%;" />

### 2.3.3 constructor 构造函数

对象原型（`__prototype__`）和原型对象（prototype）里面都有一个 constructor 属性，constructor 我们称之为构造函数，因为它指向构造函数本身

<font color="red">constructor 主要用于记录该对象引用的那个构造函数，它可以让原型对象重新指向原来的构造函数</font>

```js
// demo1
function Star(uname, uage) {
    this.uname = uname;
    this.uage = uage;
}
// 通过 . 的方式来添加方法
Star.prototype.sing = function() {
    console.log('我会唱歌');
}
Star.prototype.dance = function() {
    console.log('我会跳舞');
}
var ldh = new Star("刘德华", 19);

console.log(Star.prototype.constructor);
console.log(ldh.__proto__.constructor);
```

执行结果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221011222120377.png" alt="image-20221011222120377" style="zoom:50%;" />

其中我们可以看出，无论是原型对象还是对象，内置的 constructor 构造函数指向的都是 Star，现在我们更换一种写法，为了使得添加在 prototype 上的方法结构更加清晰，我们使用对象的形式来存储，如下：

```js
// demo2
function Star(uname, uage) {
    this.uname = uname;
    this.uage = uage;
}
// 通过 . 的方式来添加方法
Star.prototype ={
    sing: function() {
        console.log('我会唱歌');
    }, 
    dance: function() {
        console.log('我会跳舞');
    }
}
var ldh = new Star("刘德华", 19);

console.log(Star.prototype.constructor);
console.log(ldh.__proto__.constructor);
```

打印结果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221011223427853.png" alt="image-20221011223427853" style="zoom:50%;" />

这时候，我们就会发现构造函数 constructor 所指向的对象已经发生改变，原因也很简单，是因为<font color="red">我们使用对象存储的方式进行重构了 prototype 这个对象</font>，这时候我们就需要手动设置 constructor 构造函数

```js
// demo3
function Star(uname, uage) {
    this.uname = uname;
    this.uage = uage;
}
// 通过 . 的方式来添加方法
Star.prototype ={
    constructor: Star,
    sing: function() {
        console.log('我会唱歌');
    }, 
    dance: function() {
        console.log('我会跳舞');
    }
}
var ldh = new Star("刘德华", 19);

console.log(Star.prototype.constructor);
console.log(ldh.__proto__.constructor);
```

执行结果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221011223741310.png" alt="image-20221011223741310" style="zoom:50%;" />

现在我们就能知道，我们的对象是通过那一个构造函数创建出来的了



## 2.4 构造函数、实例和原型对象三者之间的关系

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221011225305198.png" alt="image-20221011225305198" style="zoom:50%;" />

- 构造函数中存在一个内置的原型对象，原型对象通过 constructor 指向构造函数
- 通过构造函数创建的实例对象也存在一个内置的对象原型`__proto__`，这与原型对象是等价的，实例对象以对象原型为中介，通过 constructor 也可以指明构造函数

## 2.5 原型链

 我们都知道只要是对象，在 JS 中存在一个原型`__proto__`，那么prototype 这个原型对象的原型是什么呢？

我们直接打印查看，如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221011230753345.png" alt="image-20221011230753345" style="zoom:33%;" />

> 这里的 [[prototype]] 即是 `__proto__`，很明显，其 constructor 指向的是 Object 的 prototype

所以，<font color="red">构造函数里面的原型对象的 `__proto__`指向的是 Object.prototype</font>

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221011231207397.png" alt="image-20221011231207397" style="zoom:50%;" />

> 请牢记上图，十分重要！！！

上图即为原型链：<font color="red">实例对象 → 构造函数原型对象 → Object 原型对象 → null</font>

## 2.6 对象成员查找机制

对象成员的查找机制是基于原型链来检索的，也就是：

<u>对象实例 → 构造函数原型对象 → Object 原型对象</u>

检索时，采用就近原则，找到则立即返回



## 2.7 原型对象的 this 问题

<font color="red">在构造函数和原型对象函数中，this 指向的是实例对象</font>

```js
var that1;
var that2;
function Star(uname, uage) {
    this.uname = uname;
    this.uage = uage;
    that1 = this;
}

Star.prototype.sing = function() {
    that2 = this;
}

var ldh = new Star('刘德华', 19);
ldh.sing();
console.log(that1 == ldh);
console.log(that2 == ldh);
```

执行结果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221011235400863.png" alt="image-20221011235400863" style="zoom:50%;" />



## 2.8 利用原型对象扩展内置对象

可以通过原型对象，对原来的内置对象进行扩展自定义的方法，比如给数组增加自定义求偶数和的功能

```js
// 给 Array 数组对象扩展一个求和的方法
Array.prototype.sum = function() {
    var sum = 0;
    for (var i = 0; i < this.length; i++) {
        sum += this[i];       // this 指向的是实例对象 Array
    }
    return sum;
}
var arr = [1, 2, 3];
console.log(arr.sum()); // 6
// 创建一个 Array 数组
var arr1 = new Array(1, 2, 3, 4);
console.log(arr1.sum());    // 10
```

<font color="red">注意：数组和字符串内置对象不能给原型对象覆盖操作，即用对象存储的形式去设置 prototype，只能通过`.`的形式去添加方法</font>



# 3 继承

在 ES6 之前，JS 没有给我们提供 extends 继承，我们可以通过<font color="red">构造函数 + 原型对象</font>模拟继承，被称为<font color="red">组合继承</font>

## 3.1 call()

JS 中的 call 方法用于调用函数，并且修改函数运行时的 this 指向，语法格式如下：

```js
func.call(thisArg, arg1, arg2, …)
```

- thisArg：当前调用函数 this 的指向对象
- arg1, arg2：传递的其他参数

**demo1**

```js
function fn() {
    console.log('全民制作人们，大家好');
}
// 1. 使用 call 方法调用函数
fn.call();
```

**demo2**

```js
function fn(x, y) {
    console.log(x + y);
    console.log(this);
}
var o = {
    name: 'andy'
};
// 2. 使用 call 方法改变这个函数的 this 指向
fn.call(o, 1, 2);

```

执行结果为：

![image-20221012165005311](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221012165005311.png)

## 3.1 借用构造函数继承父类属性

核心原理：<font color="red">通过 call() 把父类的 this 指向子类的 this，这样就可以实现子类继承父类的属性</font>

```js
//1. 创建父构造函数
function Father(uname, uage) {
    // this 指向父构造函数的实例对象
    this.uname = uname;
    this.uage = uage;
}

//2. 创建子构造函数
function Son(uname, uage) {
    // 调用 Father 方法，并且将其 this 指向子构造函数的实例对象
    Father.call(this, uname, uage);
}
//3. 创建子类对象
var son = new Son('刘德华', 19);
console.log(son);
```

执行结果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221012170430176.png" alt="image-20221012170430176" style="zoom:50%;" />

## 3.2 利用原型对象继承方法

```js
//1. 创建父构造函数
function Father(uname, uage) {
    // this 指向父构造函数的实例对象
    this.uname = uname;
    this.uage = uage;
}
// 为父类添加 money 这个方法
Father.prototype.money = function() {
    console.log('要赚钱');
}
//2. 创建子构造函数
function Son(uname, uage) {
    // 调用 Father 方法，并且将其 this 指向子构造函数的实例对象
    Father.call(this, uname, uage);
}

// 为子类添加 exam 方法
Son.prototype.exam = function() {
    console.log('要考试');
}

Son.prototype = new Father();       // 创建 father 实例对象
Son.prototype.constructor = Son;    // 让 Son 的原型对象指回 Son

//3. 创建子类对象
var son = new Son('刘德华', 19);
son.money();
console.log(Son.prototype.constructor);
console.log(Father.prototype);
```

执行结果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221012182248007.png" alt="image-20221012182248007" style="zoom:50%;" />

:heavy_exclamation_mark:注意：

1. 不可以直接使用 `Son.prototype = Father.prototype`的方法继承父类的方法，因为子类的改变同样会影响到父类

   ```js
   Son.prototype = Father.prototype;
   // 为子类添加 exam 方法
   Son.prototype.exam = function() {
       console.log('要考试');
   }
   ```

   打印 `Father.prototype`，执行结果如下：

   <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221012183448203.png" alt="image-20221012183448203" style="zoom:33%;" />

2. 当我们将 Fahter 的实例对象传给 Son 的原型对象之后，继承了 Father 实例对象的 `__proto__`的 money 方法，但是同时 Son 的原型对象的 constructor 指向了 Father ，我们需要将其更改回来，原理图如下：

   <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221012183726489.png" alt="image-20221012183726489" style="zoom:33%;" />



## 3.3 类的本质

ES6 之前，程序员需要通过<font color="red">构造函数 + 原型 </font>实现面向对象编程；ES6 之后，通过类来实现面向对象编程

类的本质是：<font color="red">function</font>

我们首先回顾一下构造函数的特点：

1. 构造函数有原型对象 prototype
2. 构造函数原型对象 prototype 里面有 constructor 指向构造函数
3. 构造函数可以通过原型对象添加添加方法
4. 构造函数创建的实例对象有`__proto__`原型指向构造函数原型对象

```js
class Star {
    constructor(uname, uage) {
        this.uname = uname;
        this.uage = uage;
    }

}
console.log(typeof Star);
// 1. 类里面也有原型对象
console.log(Star.prototype);
// 2. 类的原型对象里面也有 constructor 指向类本身
console.log(Star.prototype.constructor);
// 3. 类可以通过原型对象添加方法
Star.prototype.sing = function() {
    console.log('冰雨');
}
// 4. 类创建的实例里面也有原型 __proto__，它指向构造函数的原型对象
var ldh = new Star('刘德华', 19);
console.log(ldh.__proto__);
```

执行结果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221012192230490.png" alt="image-20221012192230490" style="zoom:50%;" />

总结：ES6 中的类就是<font color="orange">语法糖</font>:candy:

> 所谓语法糖指的就是一种便捷写法



# 4 ES5 中新增方法

ES5 中给我们新增了一些方法，可以方便的操作数组或者字符串，这些方法主要包括：

- 数组方法
- 字符串方法
- 对象方法

## 4.1 数组方法

迭代（遍历）方法：<u>forEach()、map()、filter()、some()、every()</u>

### 4.1.1 forEach 使用方法

语法如下：

```js
array.forEach(function(currentValue, index, arr){
    // 函数体
})
```

- currentValue：数组当前项的值
- index：数组当前项的索引
- arr：数组对象本身

```js
// demo
var arr = [1, 2, 3];
var sum = 0;
arr.forEach(function(value, index, array) {
    console.log('第' + index + '个元素为：' + value);
    console.log('数组本身为：' + array);
    sum += value;
})
console.log(sum);
```

执行结果为：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221012194602878.png" alt="image-20221012194602878" style="zoom:50%;" />

### 4.1.2 filter 使用方法

语法如下：

```js
array.filter(function(currentValue, index, arr) {
	// 函数体
})
```

filter()  方法创建一个新的数组，新数组中的元素是通过检查指定数组中符合条件的所有元素，主要用于<font color="red">筛选数组，注意它直接返回一个新数组</font>

- currentValue：数组当前项的值
- index：数组当前项的索引
- arr：数组对象本身

```js
var arr = [12, 66, 4, 88];
var newArr = arr.filter(function(value, index) {
    return value >= 20;    // 筛选出所有大于20的数，返回给新数组
    /******************
    如果我们想要返回所有的偶数，可以写 return value % 2 === 0;
    */
});
console.log(newArr);
```

执行结果为：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221012202857584.png" alt="image-20221012202857584" style="zoom:50%;" />



### 4.1.3 some 

语法如下：

```js
array.some(function(currentValue, index, arr) {})
```

some 方法用于检测数组中的元素是否满足指定条件，通俗讲，就是查找数组中是否有满足条件的元素，如果有则返回 true，如果没有就返回 false，这里和 sql 中的 some 函数类似

- currentValue：数组当前项的值
- index：数组当前项的索引
- arr：数组对象本身

```js
// 查找数组中是否有大于 20 的元素
var arr = [12, 66, 4, 88];
var flag = arr.some(function(value) {
    return value >= 20;
});
console.log(flag);

// 查找数组中是否有 pink
var arr1 = ['pink', 'blue', 'red'];
var flag1 = arr1.some(function(value) {
    return value == 'pink';
});
console.log(flag1);
```

执行结果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221012204400600.png" alt="image-20221012204400600" style="zoom:50%;" />

:herb:some 和 filter 的区别

1. filter 是查找满足条件的元素，返回的是一个==数组==，是把所有满足条件的元素返回回来
2. some 也是查找满足条件的元素是否存在，返回的是一个==布尔值==，<font color="red">如果查找到第一个满足条件的元素就终止循环</font>



### 4.1.4 map

`map()` 方法会在原数组的基础上创建一个新数组，平时使用较为广泛，语法如下

```js
map((element) => { /* … */ })
map((element, index) => { /* … */ })
map((element, index, array) => { /* … */ })
```



**示例：将数组中每个元素扩大 2 倍**

```js
let arr = [1, 2, 3, 4];
arr = arr.map(ele => ele*2);
console.log(arr.join());    // 2,4,6,8
```





### 4.1.5 fill 

`fill()` 用于将一个固定值填充一个数组中从起始索引到终止索引内的全部元素，<strong style="color:red">不包括终止索引</strong>

```js
fill(value)
fill(value, start)
fill(value, start, end)
```



### 4.1.6 reduce

`reduce()` 会对数组中每个元素按序执行一个自定义的 reducer 函数，每次运行的结果都会作为下一次运行时的参数传入，最后将结果汇总为单个返回值，语法如下：

```js
reduce((previousValue, currentValue) => { /* … */ } )
reduce((previousValue, currentValue, currentIndex) => { /* … */ } )
reduce((previousValue, currentValue, currentIndex, array) => { /* … */ } )
reduce((previousValue, currentValue) => { /* … */ } , initialValue)
```

示例代码：

```js
// 求和
let arr = [1, 2, 3, 4];
let result = arr.reduce((acc, current) => acc + current, 0) // 10

// 将二维数组转成一维数组
```





### 4.1.6 查询商品案例

需要制作出来的效果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221013000305040.png" alt="image-20221013000305040" style="zoom:33%;" />

我们可以通过 id 和 名称进行查询，并且显示对应商品

```css
/* CSS 样式 */
.search {
    width: 564px;
    height: 20px;
    margin: 20px auto;
}

.search input {
    width: 50px;
}

table {
    /* margin-top: 20px; */
    /* maring-top 居然无效 */
    width: 484px;
    margin: 0 auto;
    border-collapse: collapse;
    border: 1px solid #000;
}

th,
td {
    border: 1px solid #000;
    text-align: center;
    width: 160px;
    height: 30px;
    font-size: 14px;
}
```

```html
<!-- HTML 结构 -->
<div class="search">
    按照价格查询: <input type="text" class="start"> - <input type="text" class="end"> <button
        class="search-price">搜索</button> 按照商品名称进行查询: <input type="text" class="product"> <button
        class="search-product">查询</button>
</div>
<table>
    <thead>
        <tr>
            <th>id</th>
            <th>产品名称</th>
            <th>价格</th>
        </tr>
    </thead>
    <tbody>
    </tbody>
</table>
```

```js
// JS 行为动作
var st = document.querySelector(".start");
var ed = document.querySelector(".end");
var search_btn = document.querySelector('.search-price');
var pdt = document.querySelector('.product');
var query_btn = document.querySelector('.search-product')
// 利用新增数组方法操作数据
var data = [{
    id: 1,
    pname: '小米',
    price: 2999
}, {
    id: 2,
    pname: 'oppo',
    price: 2899
}, {
    id: 3,
    pname: '荣耀',
    price: 1999
}, {
    id: 4,
    pname: '华为',
    price: 3999
}];
// 1. 获取相应的元素
var tbody = document.querySelector('tbody');
// 2. 把数据渲染到页面中
setData(data);
function setData(mydata) {
    // 先清空原来表中数据
    tbody.innerHTML = '';
    mydata.forEach(function (value) {
        var tr = document.createElement('tr');
        tr.innerHTML = '<td>' + value.id + '</td>' + '<td>' + value.pname + '</td>' + '<td>' + value.price + '</td>';
        tbody.append(tr);
    })
}
// 3. 根据价格查询商品
search_btn.addEventListener('click', function () {
    // console.log(1);
    var newData = data.filter(function (value) {
        return value.price >= st.value && value.price <= ed.value
    })
    // console.log(newData);
    // 把 筛选之后的对象渲染到页面中
    
    setData(newData);
})
//4. 如果查询数组中唯一的元素，使用 some 方法更加合适，因为它一旦查找到这个元素就不再进行循环遍历，效率更高
query_btn.addEventListener("click", function() {
    // console.log(1);
    var arr = [];
    data.some(function(value) {
        if(value.pname === pdt.value) {
            arr.push(value);    // 通过这种方式查询到的对象放到 arr 数组中
        }
    })
    // 将查询到的内容渲染到页面中
    setData(arr);
})
```



### 4.1.7 some 和 forEach 的区别

**demo1：我们分析下面代码**

```js
var arr = ['green', 'pink', 'red', 'blue'];
arr.forEach(function(value) {
    if(value == 'pink') {
        console.log('找到了该元素');
        return true;
    }
    console.log('树枝666');
})
```

执行结果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221013001012199.png" alt="image-20221013001012199" style="zoom:50%;" />

我们会发现，判断条件外面的语句竟然一共打印了<u>3次</u>，可见 forEach 语句当查找到指定对象之后，是不会终止循环的，而是会将整个数组的元素都遍历一遍



**demo2：将 forEach 方法更改为 some 方法**

```js
var arr = ['green', 'pink', 'red', 'blue'];
arr.some(function(value) {
    if(value == 'pink') {
        console.log('找到了该元素');
        return true;
    }
    console.log('树枝666');
})
```

执行结果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221013001400875.png" alt="image-20221013001400875" style="zoom:50%;" />

可以看出，some方法通过 `return true;`语句，当查找到指定元素之后就会终止循环遍历

:heavy_exclamation_mark:注意：filter 方法和 forEach 方法类似，即使查找到指定元素，也不会终止循环遍历



## 4.2 字符串方法

### 4.2.1 trim 方法

trim 方法会<font color="red">从一个字符串的两端删除空白字符</font>，语法格式如下：

```js
str.trim()
```

> trim() 方法不会影响字符串本身，它返回的是一个新的字符串

**demo**

```js
var str = ' andy ';
var newStr = str.trim();
console.log(str);
console.log(newStr);
```

执行结果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221013081845607.png" alt="image-20221013081845607" style="zoom:50%;" />



## 4.3 Obect 方法

### 4.3.1 defineProperty 方法

`Object.defineProperty()`定义对象中新属性或修改原有的属性

```js
Object.defineProperty(obj, prop, desriptor)
```

- obj：必须，目标对象
- prop：必须，须定义或修改的属性名
- decriptor：必须，目标属性所拥有的特性，以对象的形式书写
  - value：设置属性的值，默认是 undefined
  - writable：值是否可以重写，true | false，默认是 false
  - enumerable：目标属性是否可以被枚举， true | false，默认是 false
  - configurable：目标属性是否可以被删除或是否可以再次修改特性 true | false，默认是 false

```js
var obj = {
    id: 1,
    name: '小米',
    price: 1999
}

// 将 price 的值修改为 999
Object.defineProperty(obj, 'price', {
    value: 999
})
console.log(obj);
// 添加新的属性值
Object.defineProperty(obj, 'num', {
    value: 1000
})
console.log(obj);

// 设置 id 的值不能修改
Object.defineProperty(obj, 'id', {
    writable: false
})

// 修改 id 的值进行测试
obj.id = 2;
console.log(obj);
```

执行结果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221013085433323.png" alt="image-20221013085433323" style="zoom:50%;" />

:bulb:默认值为 false 的解释！！！

```
writable 的默认值为 false，表示的是通过 defineProperty 添加的属性是无法被修改的，并且必须是通过 . 的形式去修改才不会被修改，如果使用通过 defineProperty 还是可以修改的
```

<font color="red">enumerable 属性用来使得是否能对对象属性进行遍历</font>，如下：

```js
var obj = {
    id: 1,
    name: '小米',
    price: 1999
}
Object.defineProperty(obj, 'num', {
    value: 1000,
    // enumerable: false
})
Object.defineProperty(obj, 'address', {
    value: '中国山东找蓝翔',
    enumerable: false
})
console.log(obj);
console.log(Object.keys(obj));
```

当我们给设置的属性添加了`enumerable: false;`之后，就无法通遍历得到该属性元素

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221013093736005.png" alt="image-20221013093736005" style="zoom:50%;" />

<font color="red">configurable 属性用于设置新建的特性是否允许被修改</font>

```js
var obj = {
    id: 1,
    name: '小米',
    price: 1999
}
Object.defineProperty(obj, 'num', {
    value: 1000,
    configurable: false
})
Object.defineProperty(obj, 'address', {
    value: '中国山东找蓝翔',
    configurable: true
})
delete obj.name
delete obj.num
delete obj.address
console.log(obj);
```

执行结果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221013095301671.png" alt="image-20221013095301671" style="zoom:33%;" />

- 预设置的属性可以被修改，设置了`enumerable: false;`的属性不可以被修改
- 使用 `enumerable: false;` 不仅使得对应的属性不能被修改，其特性，例如 value、writable 的参数特性也不能进行修改

### 4.3.2 keys 方法

`Object.keys()`方法用于获取对象自身所有的属性

```js
Object.keys(obj)
```

- 效果类似于 for…in
- 返回一个由属性名组成的数组

**dmeo**

```js
var obj = {
    id: 1,
    name: '小米',
    price: 1999
}

var arr = Object.keys(obj);
console.log(arr);
```

执行结果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221013092851301.png" alt="image-20221013092851301" style="zoom:50%;" />



#  5 函数进阶

## 5.1 函数的定义

函数定义有如下方式：

1. 函数声明方式 function 关键字（命名函数）

   ```js
   function fn() {};
   ```

2. 函数表达式 （匿名函数）

   ```js
   var fun = function() {};
   ```

3. new Function()

   ```js
   var f = new Function('参数1', '参数2', '函数体')
   ```

   ```js
   // demo
   var fn = new Function('a', 'b', 'console.log(a + b)');
   fn(1, 3);
   // 但是这种写法，代码执行效率比较低，因为 JS 还需要将函数体字符解析为可识别的函数体
   ```

注意：

- 通过第三种方式创建函数，里面参数必须都是字符串
- 第三种创建函数的形式效率低，也不便于书写，不推荐使用
- 所有函数都是 Function 的实例
- 函数也属于对象

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221013105828580.png" alt="image-20221013105828580" style="zoom:50%;" />



## 5.2 函数的调用

到目前为止，我们一共学习了下面六种函数：

1. 普通函数
2. 对象的方法
3. 构造函数
4. 绑定事件函数
5. 定时器函数
6. 立即执行函数

```js
// 函数的调用方式

// 1. 普通函数
function fn() {
    console.log('人生的巅峰');

}
// fn();   fn.call()
// 2. 对象的方法
var o = {
    sayHi: function() {
        console.log('人生的巅峰');

    }
}
o.sayHi();
// 3. 构造函数
function Star() {};
new Star();
// 4. 绑定事件函数
// btn.onclick = function() {};   // 点击了按钮就可以调用这个函数
// 5. 定时器函数
// setInterval(function() {}, 1000);  这个函数是定时器自动1秒钟调用一次
// 6. 立即执行函数
(function() {
    console.log('人生的巅峰');
})();
// 立即执行函数是自动调用
```



## 5.3 函数中 this 的指向

this 的指向，是当我们调用函数的时候确定的，调用方式的不同决定了 this 的指向不同

| 调用方式     | this 指向                                  |
| ------------ | ------------------------------------------ |
| 普通函数调用 | window                                     |
| 构造函数调用 | 实例对象，原型对象里面的方法也指向实例对象 |
| 对象方法调用 | 该方法所属对象                             |
| 事件绑定方法 | 绑定事件对象                               |
| 定时器函数   | window                                     |
| 立即执行函数 | window                                     |

```js
// 1. 普通函数里面的 this 指向的是 window 这个顶级对象
function test() {
    console.log(this);
}
test();

// 2. 对象方法里面的 this 指向的是对象本身
var o = {
    name: 'ldh',
    age: 19,
    sayHi: function() {
        console.log(this);
    }
}
o.sayHi();
// 3. 构造函数里面的 this 指向的是通过该构造函数创建出来实例对象
function Star(uname, uage) {
    this.uanme = uname;
    this.uage = uage;
}
var ldh = new Star('刘德华', 19);
// 这里的 this 指向的是 ldh
// 4. 绑定事件函数里面的 this 指向的是绑定事件对象
```

执行结果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221013114718745.png" alt="image-20221013114718745" style="zoom:50%;" />

## 5.4 改变函数内部的 this 指向

JS 为我们专门提供了一些函数方法来帮助我们处理函数内部的指向问题，常见的有 <u>bind()、call()、apply()</u> 三种方法

### 5.4.1 call() 方法

call 方法的语法格式如下：

```js
func.call(thisArg, arg1, arg2, …)
```

call 方法的两个作用：

1. 改变 this 指向
2. 调用函数

> 我们可以利用这两个特性实现继承

### 5.4.2 apply() 方法

语法格式如下：

```js
fn.apply(thisArg, [arrArray])
```

- thisArg: 在 fun 函数运行时指定的 this 值
- argsArray:  传递的值，必须包含在==数组==里面
- 返回值就是函数的返回值，因为它就是调用函数
- apply 方法的作用同 call 方法，也是用于改变 this 的指向和调用函数

**demo**

```js
o = { 
    name: 'andy'
}
function fn(arr) {
    console.log(this);
    console.log(arr);
}
fn.apply(o, ['pink']);
```

执行结果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221013123205853.png" alt="image-20221013123205853" style="zoom:50%;" />

通过 apply 改变了函数中 this 的指向，使其指向对象 o，同时传递一个 伪数组 ，但是最终在函数中的打印结果为一个字符串，而非数组

但是 apply 方法的主要应用：

1. 利用 apply 借助数学内置对象求最大值

   ```js
   var arr = [1, 11, 66, 123];
   var Max = Math.max.apply(Math, arr);
   console.log(Max);
   ```

### 5.4.3 bind() 方法

bind() 不会调用函数，但是能改变函数内部 this 的指向

```js
fn.bind(thisArg, arg1, arg2,…)
```

- thisArg： 在 fn 函数运行时指定的 this 值
- arg1，arg2：传递的其他的参数
- 返回由指定的 this 值和初始化参数改造的<font color="red">原函数拷贝</font>

```js
// demo
var o = {
    name: 'andy'
};
function fn() {
    console.log(this);
}
var f = fn.bind(o);
f();
```

执行结果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221015091324673.png" alt="image-20221015091324673" style="zoom:50%;" />

> bind 方法，我们在实际开发中使用较多，如果在实际开发中，有的函数我们不需要立即调用，但是又想改变这个函数内部的 this 指向时，我们可以利用 bind 进行重指向的原函数拷贝

:bell:**bind 应用案例**

案例介绍：<u>现在我们有一个按钮，当我们点击之后，就禁用这个按钮，3秒钟之后开启这个按钮</u>

```js
<button>开启</button>
<script>
    var btn = document.querySelector('button');
    btn.onclick = function() {
        this.disabled = true;
        setTimeout(function() {
            this.disabled  = false;
        }.bind(this), 3000);
    }
</script>
```

> 注意：定时器函数里面的 this 指向的是 window 顶级对象，我们需要在函数外通过 bind 修改 this 指向对象



### 5.4.4 bind 应用面向对象 tab 栏



## 5.5 严格模式

### 5.5.1 什么是严格模式

JS 除了提供正常模式外，还提供了严格模式（strict mode）,ES6 的严格模式是采取有限制性的 JS 变体的一种方式，即在严格 的条件下运行 JS 代码

> 严格模式在 IE10 以上版本的浏览器中才会被支持，在旧版本的浏览器中会被忽略

严格模式对正常的 JS 语义做出一些更改：

1. 消除了 JS 语法的一些不合理、不严谨之处
2. 消除了代码运行的一些不安全之处
3. 提高了编译器的效率，增加了运行速度
4. 禁用了 ECMAscript 的未来版本中可能会定义的一些语法，例如 一些保留字，像 ：class、enum、export、extends、import、super 等关键字都不能充当变量名

### 5.5.2 开启严格模式

严格模式可以应用到<font color="red">整个脚本</font>或<font color="red">个别函数</font>中

**为脚本开启严格模式**

如果我们想要为整个脚本文件开启严格模式，需要<font color="red">在所有的语句之前放一个特定的语句`"use strict;"`或者`'use strict;'`</font>

```js
<script>
    // dmeo1
    'use strict';
    // 代码段
</script>
```

```js
<script> 
    (function() {
        'use strict';
        // 代码段
    }())
</script>
```



**为函数开启严格模式**

```js
function fn() {
	'use strict';
	// 代码段
}
```

### 5.5.3 严格模式中的变化

**变量规定**

1. 在正常模式中，如果一个变量没有声明就赋值，默认是全局变量（又称为隐性全局变量），而严格模式禁止这种写法，变量都必须使用 let 或 var 命令声明，然后再使用
2. 严禁删除已经声明的变量，例如：`delete x;`语法是错误的

**严格模式下 this 指向问题**

1. 以前在全局作用域函数中的 this 指向 window 对象；<font color="red">严格模式下全局作用域中函数的 this 指向的是 undefined</font>
2. ~~严格模式下，如果构造函数不加 new 调用，this 会报错~~

**函数变化**

1. 函数不能有重名的<font color="red">参数</font>

2. 函数必须声明在顶层。新版本的 JS 会引入 "块级作用域"（ES6 中已引入），为了与新版本接轨，不允许在==非函数的代码块==内声明函数

   > 这里的"非函数代码块"指的是在 for 或 if 语句中嵌套函数，但是允许在函数中嵌套函数



## 5.6 高阶函数

<font color="red">高阶函数</font>是对其他函数进行操作的函数，它<font color="red">接收函数作为参数或将函数作为返回值</font>输出

```js
// 高阶函数 - 函数可以作为参数传递
function fn(a, b, callback) {
    console.log(a + b);
    callback && callback();
}
fn(1, 2, function() {
    console.log('我是最后调用的');
})
```

```js
// 高阶函数 - 将函数作为参数返回
function fn() {
    return function() {}
}
fn();
```

> 函数也是一种数据类型，同样可以作为参数，传递给另外一个参数使用，最典型的就是作为回调函数



# 6 闭包

## 6.1 变量作用域

变量根据作用域的不同，分为两种：<font color="red">全局变量和局部变量</font>

1. 函数内部可以使用全局变量
2. 函数外部不可以使用局部变量
3. 当函数执行完毕，本作用域内的局部变量会销毁



## 6.2 什么是闭包

<font color="red">闭包（closure）</font>指<u>有权访问另一个函数作用域中变量的函数</u>

```js
// demo
function fn() {
    var num = 10;

    function fun() {
        console.log(num);
    }
    fun();
}
fn();
```

执行结果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221013210658860.png" alt="image-20221013210658860" style="zoom:50%;" />

fun 函数中，可以获取 fn 函数中 num 的值，这就形成了一个闭包



## 6.3 闭包的作用

闭包的主要作用：<font color="red">延伸了变量的作用范围</font>

示例如下：

```js
function fn() {
    var num = 10;

    function fun() {
        console.log(num);
    }

    return fun;
}
var f = fn();
f();
```

我们利用闭包，可以使得可以通过外面的函数调用 fn 内部的函数 fun，并在 fn 中产生闭包，使得 fun 函数可以获取 num 

## 6.4 闭包应用

### 6.4.1 循环注册事件

```js
<ul>
    <li>1. 苹果</li>
    <li>2. 香蕉</li>
    <li>3. 榴莲</li>
    <li>4. 橘子</li>
</ul>
<script>
    var lis = document.querySelector('ul').querySelectorAll('li');
    for (var i = 0; i < lis.length; i++) {
        (function(i){
            lis[i].onclick = function() {
                console.log(i);
            }
        })(i)
        // lis[i].index  = i;
        // lis[i].onclick = function() {
        //     console.log(this.index);
        // }            
    }
</script>
```

效果图如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221013224104158.png" alt="image-20221013224104158" style="zoom:50%;" />

当我们点击，对应列表之后会打印出对应序号，这里需要注意一点：<font color="red">for 循环是同步任务，而 点击事件属于异步任务</font>，当我们书写完代码之后，，for 循环体里面的内容就已经执行完毕了，在上述代码中，i 就变成了4 ，所以当我们添加点击事件后，无论点击那个列表，打印出来的都是 4 ，解决方法如下：

1. 在 for 循环内，点击事件外，添加额外的一个属性，来记录当前列表的序号
2. 使用立即函数，将整个点击事件包裹起来形成闭包，将立即函数外面的 i 传入到绑定事件中



### 6.4.2 定时器中的闭包

```js
<ul>
    <li>0. 苹果</li>
    <li>1. 香蕉</li>
    <li>2. 榴莲</li>
    <li>3. 橘子</li>
</ul>
<script>
    var lis  = document.querySelector('ul').querySelectorAll('li');
    for(var i = 0; i < lis.length; i++) {
        (function(i) {
            setTimeout(function() {
                console.log(lis[i].innerHTML);
            },1000);
        })(i);
    }
</script>
```

执行结果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221013231455668.png" alt="image-20221013231455668" style="zoom:50%;" />

这里需要注意的是：setTimeout 函数也是异步任务，所以具体思路与前面的类似

### 6.4.3 打车价格

具体要求：

1. 打车起步价为 13 元（3公里内），超过 3 公里的部分按每公里 5 元来计算
2. 如果出现拥堵情况，总价格多收取 10 元拥堵费

```js
var car = (function() {
    var start = 13;     // 起步价
    var total = 0;      // 总价
    return {
        price: function(n) {
            if (n <= 3) {
                total = start;
            } else {
                total = (n - 3)*5 + start;
            }
            return total;
        } ,
        block: function(flag) {
            return flag ? total + 10 : total;
        }
    }
})();
// car.price(5)
console.log(car.price(5));  // 23
console.log(car.block(true));   // 33
```



# 7 递归

## 7.1 什么是递归

如果<font color="red">一个函数内部可以调用其本身</font>，那么这个函数就是递归函数

递归函数的作用和循环效果类似，由于递归很容易发生<font color="red">"栈溢出"（stack overflow）</font>错误，所以必须要加退出条件 return 



## 7.2 利用递归求解一些数学题

**求解 n 的阶乘**

```js
function fn(n) {
    if(n === 0 || n === 1) {
        return 1;
    }
    
    return n * fn(n-1);
}
```



**求解 Fibonacci 数列**

```js
function fb(n) {
    if (n === 1 || n === 2) {
        return 1;
    }
    return fb(n-1) + fb(n-2);
}
```

> 利用该函数可以求解第 n 个斐波那契数列的值



**遍历数据**

```js
var data = [{
    id: 1,
    name: '家电',
    goods: [{
        id: 11,
        gname: '冰箱'
    }, {
        id: 12,
        gname: '洗衣机'
    }]
}, {
    id: 2,
    name: '服饰'
}];
// 利用 forEach 函数去遍历内外两层的数据对象
function getID(json, id) {
    json.forEach(function(item) {
        if (item.id == id) {
            console.log(item);
            // 如果存在 item.good 这个数组，并且长度大于 0
        } else if (item.goods && item.goods.length > 0) {
            getID(item.goods, id);
        }
    })
}

getID(data, 1);
getID(data, 11);
```

打印的执行结果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221014205535436.png" alt="image-20221014205535436" style="zoom:50%;" />

可以看到，无论是查询内层的 id 还是 data 对象数组里面的 goods 的 id，都可以通过递归将其查找出来

但是，我们这么做，只是将查询到的对象打印出来，这在实际开发中意义不大，为了得到对应的对象，我们需要将其返回，所以要添加一个空对象用来接收

```js
var data = [{
    id: 1,
    name: '家电',
    goods: [{
        id: 11,
        gname: '冰箱'
    }, {
        id: 12,
        gname: '洗衣机'
    }]
}, {
    id: 2,
    name: '服饰'
}];
// 利用 forEach 函数去遍历内外两层的数据对象
function getID(json, id) {
    var o = {};
    json.forEach(function(item) {
        if (item.id == id) {
            o = item;
            // 如果存在 item.good 这个数组，并且长度大于 0
        } else if (item.goods && item.goods.length > 0) {
            o = getID(item.goods, id);
        }
    })
    return o;
}

console.log(getID(data, 1));
console.log(getID(data, 11));
```

执行结果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221014210739499.png" alt="image-20221014210739499" style="zoom:50%;" />



# 8 深拷贝和浅拷贝

1. 浅拷贝只是拷贝一层，<font color="red">更深层次对象级别的只是拷贝引用</font>
2. 深拷贝拷贝多层，每一级别的数据都会拷贝
3. `Object.assign(target, …sources)`,ES6 中新增的可以拷贝的方式

## 8.1 浅拷贝

```js
// 浅拷贝示例
var o = {};
var obj = {
    name: 'json',
    age: 19,
    msg : {
        length: 20,
        height: 80
    }
};
for (k in obj) {
    o[k] = obj[k];
}

console.log(o);
```

执行结果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221014212830771.png" alt="image-20221014212830771" style="zoom:50%;" />

但是浅拷贝对深层次的对象级别只是引用，如下图

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221014213439267.png" alt="image-20221014213439267" style="zoom: 33%;" />

当我们对拷贝浅拷贝对象的数据进行修改，同时也会影响到原数据的值，ES6 中提供了一种语法糖的浅拷贝，语法如下：

```js
Object.assign(target, sources)
```

:bell:<font color="red">通过循环动态获取对象元素我们更推荐使用`对象['属性']`的方式获取，而不是通过`对象.属性`的方式获取，原因有如下两点：</font>

1. 如果属性名是以空格分隔的符合属性名，例如：first name，这样我们肯定就不能直接使用`.`的方式获取
2. 通过`.`的形式获取的是固定属性值，而不会随循环动态变化，例如`obj.k`，那么 JS 解析器就会认为 k 是 obj 的属性，不是当做键值对的形式获取

## 8.2 深拷贝

如果我们想要实现深拷贝，需要使用递归的方式实现：

```js
// 深拷贝示例
var o = {};
var obj = {
    name: 'json',
    age: 19,
    msg : {
        length: 20,
        height: 80
    },
    color: ['pink', 'red']
};
function deepCopy(newobj, oldobj) {
    for (var k in oldobj) {
        // 判断我们的属性值属于那种数据类型
        // 1. 获取属性值
        var item = oldobj[k];
        // 2. 判断这个值是否属于数组
        if (item instanceof Array) {
            newobj[k] = [];
            deepCopy(newobj[k], item);
        // 3. 判断这个值是否属于对象
        } else if (item instanceof Object) {
            newobj[k] = {};
            deepCopy(newobj[k], item);
        } else {
            // 4. 如果是简单数据类型就插入
            newobj[k] = item;   
        }
    }
}

deepCopy(o, obj);
console.log(o);
```

深层拷贝会对底层的数据进行一比一的拷贝，而不再是像浅拷贝一样的对更深一级的数组或者对象进行引用拷贝，原理图如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221014223940252.png" alt="image-20221014223940252" style="zoom:50%;" />



# 9 正则表达式

## 9.1 什么是正则表达式

正则表达式（regular expression）是<font color="orange">用于匹配字符串中字符组合的模式</font>。在 JS 中，正则表达式也是对象

正则表达式的作用：用于<font color="orange">查找、替换哪些符合正则表达式的文本</font>，许多语言都支持正则表达式

正则表达式的应用场景：

- 验证表单
- 过滤掉页面的一些敏感词汇（==替换==）
- 从字符串中获取我们想要的特定部分（==提取==）

## 9.2 定义正则表达式语法

```js
let 变量名 = /表达式/
```

- 其中 `/` `/` 是正则表达式字面量



## 9.3 判断是否有符合规则的字符串

我们是用"正则对象"中的 `test()`方法可以查看正则表达式与指定字符串是否匹配，语法如下：

```js
regObj.test(被检测字符串)
```

- 如果匹配则返回 true，否则返回 false

**示例1:**

```js
let reg = /前端/
let str = '我要学习前端'
console.log(reg.test(str))		// true
```



除此之外，还有一个 exec() 方法用于检索（查找）符合规则的字符串，如果找到则返回数组，，否则返回 null，语法如下;

```js
regObj.exec(被检测字符串)
```

**示例2：**

```js
// 数据参考例1
console.log(reg.exec(str))
```

显示结果如下

```
[ '前端', index: 4, input: '我要学习前端', groups: undefined ]
```



## 9.4 元字符

在正则表达式中，将字分为两种：

- 普通字符：仅能描述本身，例如所有的字母和数字
- 元字符：是一些具有特殊含义的字符，极大提高灵活性和强大的匹配功能，例如规定用户只能输入 26 个英文字母，用元字符可以写作[a-zA-Z]

参考文档：

- [MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions)
- [正则测试工具](https://tool.oschina.net/regex/) 

由于元字符众多，我们将元字符进行分类：

1. 边界符（表示位置、开头和结尾吗，必须用什么开头、用什么结尾）
2. 量词（表示重复次数）
3. 字符类（比如 `\d`表示 0-9）

### 9.4.1 边界符

| 边界符 | 说明                           |
| ------ | ------------------------------ |
| `^`    | 表示匹配行首的文本（以谁开始） |
| `$`    | 表示匹配行尾的文本（以谁结束） |

:herb: 如果同时有`^`和`$`则表示精确匹配，示例如下：

```js
console.log(/^哈$/.test('哈哈哈'))	// false
```

在这种情况下只有当里面的文本为 "哈" 的时候，返回的结果为 true，其他匹配字符返回的结果都为 false



### 9.4.2 量词

量词用来<font color="red">设定某个模式出现的次数</font>

| 量词     | 说明              |
| -------- | ----------------- |
| `*`      | 重复零次或更多次  |
| `+`      | 重复一个或更多次  |
| `?`      | 重复零次或一次    |
| `{n}`    | 重复 n 次         |
| `{n,}`   | 重复 n 次或更多次 |
| `{n, m}` | 重复 n 到 m 次    |

### 9.4.3 字符类

1. `[]`匹配字符集合
   - 例如，`[abc]`后面的字符串只要包含 abc 中的任意一个字符，都返回 true
2. `[-]`匹配连续范围字符集合
   - [a-z]：表示 a 到 z 的 26个英文字母
   - [a-zA-Z]：表示匹配所有的26个英文字母，包含大小写
   - [0-9]：表示 0-9 的数字
3. `[^]`匹配取反字符集的集合
   - `[^a-z]`匹配除小写以外的其他的字符
   - 注意要写到中括号中（写到中括号外面就成了精确匹配的开头符号了）
4. `.`匹配除了换行符之外的任何单个字符

:heavy_exclamation_mark: 这里区分一下 `/^[abc]$/` 和 `/[abc]/`

```
前面一个表示只有匹配字符有且只有一个并且只能是 a、b、c 三者之一的时候才能生效（用到了精确匹配）；
而后面一个表示只要匹配字符包含a、b、c 三者之后的时候才能生效
```

特殊情况：`/[a-zA-Z0-9-_]/`则用来匹配字符包含了大小写字母、数字以及下划线和连字符



**练习**： 腾讯 QQ 号：`/^[1-9][0-9]{4,}$/`

解释：腾讯 QQ 号以 10000 开始往后的数字



**预定义字符**

在字符类中还具有一些预定好的字符格式，他们都具有各自的含义

| 预定类 | 说明                                                     |
| ------ | -------------------------------------------------------- |
| `\d`   | 匹配 0-9 之间的任意数字，相当于 `[0-9]`                  |
| `\D`   | 匹配所有 0-9 以外的字符，相当于 `[^0-9]`                 |
| `\w`   | 匹配任意的字母、数字和下划线，相当于`[a-zA-Z0-9]`        |
| `\s`   | 除所有字母、数字和下划线以外的字符，相当于`[^a-zA-Z0-9]` |
| `\S`   | 匹配非空格的字符，相当于 `[^\t\r\n\v\f]`                 |

- 日期格式：`/^d{4}-\d{1,2}-\d{1,2}`



### 9.4.4 修饰符

正则表达式中有些语法还是比较复杂的，我们在实际开发过程中，可以直接去查看总结好的用于各种场景的正则表达式，这里推荐以下查阅网站：

- [菜鸟教程](https://www.runoob.com/regexp/regexp-syntax.html)

接下来，我们来介绍以下**修饰符**

<font color="red">修饰符约束正则执行的某些细节行为、如是否区分大小写、是否支持多行匹配</font>

语法如下：

```js
/表达式/修饰符
```

- i 是单词 ignore 的缩写，正则匹配时字母不区分大小写
- g 是单词 global 的缩写，匹配所有满足正则表达式的结果

```js
console.log(/a/i.test('a'))	// true
console.log(/a/i.test('A'))	// true
```



**字符串替换  replace 方法**

```js
str.replace(/正则表达式/, '替换的文本')
```

`g `的作用就体现在全局查找替换上，和 VS code 中输入快捷键 Ctrl + H 打开查找替换功能，替换好文本后按下 Ctrl + Alt + Enter实现全局替换



# 10 变量声明关键字

## 10.1 let 

let 是 ES6 中新增的声明变量的关键字

- let 声明的变量只在所处于的块级有效

- 不存在变量提升

  ```js
  console.log(a);
  let a = 20;
  ```

  <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221014230442311.png" alt="image-20221014230442311" style="zoom:50%;" />

- 暂时性死区

  ```js
  var num = 10
  if(true) {
  	console.log(num);
  	let num = 20;
  }
  ```

  <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221014233831943.png" alt="image-20221014233831943" style="zoom:50%;" />

  当我们使用 let 关键字在块级作用域中声明一个块级变量后，这个块级变量就会与这个块级作用域绑定，当在作用域中找不到这块级变量后，也不会向上一级作用域中进行查找



## 10.2 经典面试题

```js
// 代码1
var arr = [];
for (var i = 0; i < 2; i++) {
    arr[i] = function() {
        console.log(i);
    }
}
arr[0]();   // 2
arr[1]();   // 2
```

通过 var 关键字定义的变量 i 为全局变量，循环完成之后，i = 2，同时 arr 里面的函数输出的是全局作用域下的 i，因此，打印结果都是 2



```js
var arr = [];
for (let i = 0; i < 2; i++) {
    arr[i] = function() {
        console.log(i);
    }
}
arr[0]();   // 0
arr[1]();   // 1
```

通过 let 关键定义的 i 会与当前的块级作用域绑定，也就是产生一个暂时性死区，这就导致 arr 函数中输出的依旧是之前的绑定的 i，而不是后来发生变化的 i，打印的结果为 0, 1



## 10.3 const 关键字

作用：<u>声明常量，其值不能更改</u>

**特点：**

1. 具有块级作用域

   ```js
   if (true) {
       const a = 10;
       if (true) {
           const a = 20;
           console.log(a);
       }
       console.log(a);
   }
   console.log(a);
   ```

   打印的结果为：

   <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221015000608166.png" alt="image-20221015000608166" style="zoom:50%;" />

2. 声明常量时必须赋值

   如果我们有赋初值，就会报错，如下：

   <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221015001102755.png" alt="image-20221015001102755" style="zoom:50%;" />

3. 常量赋值后，值不能修改

## 10.4 let、cons、var 的区别

| var          | let            | const          |
| ------------ | -------------- | -------------- |
| 函数级作用域 | 块级作用域     | 块级作用域     |
| 变量提升     | 不存在变量提升 | 不存在变量提升 |
| 值可更改     | 值可更改       | 值不可更改     |



# 11 解构赋值

ES6 中允许从数组中提取值，按照对应位置，对变量赋值，对象也可以实现解构

## 11.1 数组解构

```js
let [a, b, c] = [1, 2, 3];
console.log(a);     // 1
console.log(b);     // 2
console.log(c);     // 3
```

如果解构不成功，变量的值为 undefined

```js
let [foo] = [];
let [a, b] = [1];
console.log(foo);   // undefined
console.log(b);     // undefined
```



## 11.2 对象解构

对象结构允许我们使用变量对应的属性的名字去匹配对象的属性值，如果匹配成功则返回对应的值

```js
// demo1
let {name, age} = {name: 'zs', age:18}
console.log(name);  // zs  
console.log(age);   // 18
```

```js
// demo2
let {age, name} = {name: 'zs', age:18}
console.log(name);  // zs  
console.log(age);   // 18
```

```js
// demo3
let {a, b} = {name: 'zs', age:18}
console.log(a);   // undefined
console.log(b);   // undefined
```

对象结构的另外一种写法，这种写法允许对象的名字和属性名不一致，写法如下：

```js
let {name: myName, age: myAge} = {name: 'zs', age:18}
console.log(myName);   // zs
console.log(myAge);   // 18
```

我们以 myName 为例，使用 name 去匹配右侧对象中的 name，匹配成功之后，将 name 的值赋值给 myName



# 12 箭头函数

ES6 中新增的定义函数的方式

```js
() => {}
const fn = () => {}
```

箭头函数的特点：

1. 函数体中只有一句代码，且代码的执行结果就是返回值，那么可以省略大括号和 return

   ```js
   const fn = (num1, num2) => num1 + num2;
   ```

2. 如果形参只有一个，可以省略小括号

   ```js
   const fn = v => v;
   ```



## 12.1 箭头函数的调用问题

箭头函数不绑定 this 关键字，箭头函数中的 this，指向的是<font color="red">函数定义位置的上一级的上下文 环境的 this</font>

```js
var obj = {name: 'zs', age: 18};
function fn() {
    console.log(this);
    return () => {
        console.log(this);
    }
}
let f = fn.bind(obj);

f()();
```

执行结果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221015091459443.png" alt="image-20221015091459443" style="zoom:50%;" />



## 12.2 箭头函数面试题

```js
var age = 100;
var obj = {
    age: 20,
    say: () => {
        console.log(this.age);
    }
};
obj.say();		// 100
```

由于箭头函数没有自己的 this，所以会从函数定义位置的上一级上下文环境中去查找 this，也就是 window，如果我们将箭头函数替换为普通的函数方法，那么 this 指向的就是上下文环境

```js
var age = 100;
var obj = {
    age: 20,
    // say: function() {
    //     console.log(this);
    // }
    say() {
        console.log(this);
    }
};
obj.say();
```

执行结果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221015095053370.png" alt="image-20221015095053370" style="zoom:50%;" />

根据查看结果可以知道，这里的 this 指向的 obj



# 13 剩余参数和扩展运算符

## 13.1 剩余参数

剩余参数语法允许我们将一个不定数量的参数表示为一个数组

```js
function sum(first, ...args) {
    console.log(first);  // 10
    console.log(args);   // [20, 30]
}
sum(10, 20, 30)
```



## 13.2 扩展运算符

扩展运算符可以将数组或者对象转为用逗号分隔的参数序列，在 Go 语言中，这也叫做打散，它与剩余参数恰好是一个相反的过程

```js
let ary = [1, 2, 3];
console.log(...ary);	// 1 2 3
```

### 13.2.1 扩展运算符的作用

作用1：用于<font color="red">合并数组</font>

```js
let ary1 = [1, 2, 3];
let ary2 = [4, 5, 6];

// 方法1
let ary3 = [...ary1, ...ary2];

// 方法2
ary1.push(...ary2)
```



作用2：<font color="red">将伪数组或可遍历的对象转换为真正的数组</font>

```js
let oDivs = document.querySelectorAll('div');
oDivs = {...oDivs};
```

将伪数组转化为真正数组的目的：<u>可以使用数组对象中的一些内置方法</u>



# 14 ES6 的内置对象扩展

## 14.1 Array 的扩展方法

**构造函数方法：`Array.from()`**

作用：<font color="red">将类数组（或称伪数组）或可遍历对象转换为真正的数组</font>

```js
let arrayLike = {
    "0": "a",
    "1": "b",
    "2": "c",
    "length": "3"
};

let arr2 = Array.from(arrayLike);
console.log(arr2);
```

`Array.from` 方法中还可以接收第二个参数，作用类似于数组的 map 方法，用来对每个元素进行处理，将处理后的元素返回

```
let arrayLike = {
    "0": 1,
    "1": 2,
    "2": 3,
    "length": 3
};

let arr2 = Array.from(arrayLike, item => item*2);
console.log(arr2);
```



**实例方法：`find()`**

作用：<font color="red">用于找出==第一个==符合条件的数组成员，如果没有找到返回 undefined</font>

```js
let ary = [{
    id: 1,
    name: '张三'
},
{ 
    id: 2, 
    name: '李四'
}];

let target = ary.find((item, index) => item.id == 2);
console.log(target);
```



**实例方法：`findIndex()`**

作用：<font color="red">用于找到第一个符合条件的数组成员的位置，如果没有找到则返回 -1，使用方法和 `find()` 方法类似</font>



**实例方法：`includes()`**

作用：<font color="red">用于判断数组中是否包含指定值，如果有则返回 true，否则返回 false</font>

```js
console.log([1, 2, 3].includes(2)); // true
console.log([1, 2, 3].includes(4)); // false
```



## 14.2 String 的扩展方法

### 14.2.1 模板字符串

ES6 中新增的创建字符串的方式，使用==反引号==对字符串进行初始化

```js
let name = `zhangsan`;                       
```

模板字符串中可以<font color="red">解析变量</font>

```js
let name = 'zhangsan';
let sayHi = `hello, my name is ${name}`;
```

模板字符串支持<font color="red">换行和空格</font>

```js
let result = {
    name: '张三', 
    age: 20, 
}

let html = `<div>
    <span>${result.name}</span>
    <span>${result.age}</span>
<div>`;
console.log(html);
```

在模板字符串中可以<font color="red">调用函数</font>

```js
const sayHello = function() {
	return '你好';
};
let greet = `${sayHello()}`;
console.log(greet);
```



### 14.2.2 `startWith()` 和 `endWith()` 方法

- `startsWith()`：表示参数字符串是否在原字符的头部，返回布尔值
- `endsWith()`：表示参数字符串是否在原字符的尾部，返回布尔值

```js
let str = 'Hello World';
console.log(str.startsWith('Hello'));  // true
console.log(str.endsWith('World'));    // true
```



### 14.2.3 `repeat()`方法

repeat 方法用于将字符串重复 n 次，然后拼接完成后返回拼接好的字符串

```js
console.log('x'.repeat(5)); // xxxxxx
```



# 15 set 和 map 数据结构

## 15.1 set

ES6 中提供了新的数据结构 Set，即集合，用于存放==不重复==的==无序==的值

Set 本身是一个构造函数，用来生成 Set 数据结构

```js
const s = new Set();
```

Set 函数可以接收一个数组作为参数，用来初始化

```js
const set = new Set([1, 2, 3, 4]);
```

```js
const a = new Set();
console.log(a.size);    // 0

const b = new Set([1, 2, 3, 4]);
console.log(b.size);    // 4
```



我们利用 Set 数据结构，可以实现==数组去重==



### 15.1.1 Set 实例方法

- add(value)：添加某个值，返回 Set 结构本身
- delete(value)：删除某个值，返回一个布尔值，表示删除是否成功
- has(value)：判断 value 是否属于 Set 的成员
- clear()：清除所有的成员，没有返回值

```js
const s = new Set();
s.add(1).add(2).add(3);	// 添加
s.delete(1);			// 删除
s.has(1);				// 判断存在
s.clear();				// 清空	
```



### 15.1.2 遍历 Set

Set 结构的实例和数组一样，也拥有 `forEach()` 方法，用于对每个成员执行某种操作没有返回值

```js
s.forEach(value => concole.log(value));
```



### 15.1.3 获取键和值

Set 中的键和值是相同的，也即通过 `keys()` 和 `values()` 返回数组是相同

```typescript
let s = new Set([1, 2, 3])  
console.log(s.keys())       // { 1, 2, 3 }
console.log(s.values())     // { 1, 2, 3 }
console.log(s.entries())    // { [ 1, 1 ], [ 2, 2 ], [ 3, 3 ] }
```





### 15.1.4 扩展运算符

set 实例具有 iterator 接口，可以使用扩展运算符将其转换为数组

```js
const s = new Set([1, 2, 3])
console.log([...s]) // [ 1, 2, 3 ]
```



### 15.1.5 WeakSet

WeakSet 和 Set 类似，同样是限制值不能重复，同样具有 `iterator` 接口，但是不同的是：

- 值必须是对象
- 弱引用，空闲的时候垃圾回收机制会自动回收 ，无需手动回收
- 没有 size 属性，只有 `add()`、`delete()`、`has()` 方法

```js
var weakset = new WeakSet()
var arr = [[1]]
weakset.add(arr) // WeakSet {Array(1)}
weakset.size // undefined
weakset.has(arr) // true
weakset.delete(arr) // WeakSet {}
```

> 这个 WeakSet 用的让人很迷啊

## 15.2 map

### 15.2.1 map 的基本使用

map 数据结构在很多语言中都有，主要就是用来存储键值对的数据结构，讲到这有人可能会很好奇，对象不也可以存储键值对吗，为什么还要引入 map，这肯定是对象具有局限性，<strong style="color:red">对象只能使用字符串作为 key，即使使用对象作为 key，底层会调用 `toString()` 先将其转换为字符串</strong>

**属性**

- size：获取 map 中元素个数

**方法**

1. set(key, value)：属性键值对
2. get(key)：获取某个键对应的值
3. has(key)：判断map是否有某个键
4. delete(key)：删除map的键
5. clear(key)：清空map

```js
let map = new Map([
    [{a: 1, b: 2}, 'value'],
    ['a', 1]
])
console.log(map)    // { { a: 1, b: 2 } => 'value', 'a' => 1 }
```

```js
let map = new Map()
map.set('a', 1)
map.set('b', 2)

console.log(map.get('a'))   // 1
map.delete('b')
console.log(map.size)       // 1
map.clear()
```

此外还有 `keys()`, `values()`, `entries()`,`forEach()` 遍历方法。



### 15.2.2 WeakMap

`WeakMap` 的用法跟Map类型，可以使用对象当作key。 但是**`WeakMap` 限制key只能是对象（不包含null）**，不能是其他类型。







# 16 模块化

模块化是指<u>将一个大的程序文件，拆分为许多的小文件，然后将小文件组合起来</u>

模块化的优点：

1. 防止命名冲突
2. 代码复用
3. 高维护性

模块化的产品

1. CommonJS    =>	NodeJS、Browserify
2. AMD               =>    requireJS
3. CMD               =>    seaJS

## 16.1 ES 模块化语法

模块功能主要是由两个命令构成：`export` 和 `import`

- `export` 命令用于规定模块的对外接口
- `import` 命令用于输入其他模块提供的功能

### 16.1.1 暴露模块

暴露模块主要是通过 `export` 来实现暴露，有以下三种形式：

1. 分别暴露

   ```js
   export let school = 'xxxx';
   export function findJob(){xxx};
   ```

2. 统一暴露

   ```js
   let school = 'xxx';
   function findJob(){xxx};
   
   export{school, findJob};
   ```

3. 默认暴露

   ```js
   export default {
   	let school = 'xxx';
   	function findJob(){xxx};
   }
   ```

   > 使用默认暴露后，要使用 default  进行访问



### 16.1.2 导入模块

导入模块主要通过 `import` 关键字实现，有如下方式：

1. 通用的导入方式

   ```js
   import * as alias from 'url';
   ```

2. 解构导入方式

   ```js
   import {module1, module2} from 'url';
   ```

3. 简便形式（仅针对默认暴露使用）

   ```js
   import alias from 'url';
   ```



## 16.2 模块化的另外一种方式

设置一个入口文件 app.js，里面汇集了其他许多的模块，使用的时候直接引入这一个入口文件即可

```js
<script src="../js/app.js" type="module"></script>
```

> 这里一定要添加`type="module"`，表示要要进行模块化开发

## 16.3 babel 将 ES6 转为 ES5

由于有的浏览器存在兼容性问题，我们有时候不能直接像上面那样使用`type=module`直接进入模块化开发，而是要先利用 babel 将 ES6 转为 ES5 ，再进行使用

babel 浏览器官网：[传送入口](https://www.babeljs.cn/)

使用步骤：

1. 安装 babel-cli、babel-preset-env、browserify（webpack）

   ```bash
   # 初始化工作目录
   npm init -y
   
   # 安装外部包
   npm i babel-cli babel-preset-env browserify -D
   # -D 表示开发依赖，我们还可以添加 -g 来设置为全局配置
   ```

2. 语法转换

   ```bash
   # 如果上一步使用了全局配置，这里直接用 babel  命令即可
   npx babel src/js -d dist/js --preset=babel-preset-env
   ```

   转换完毕之后，我们可以看到生成了一个 dist 输出文件夹，里面包含转换的 JS 模块文件

   <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221023153655572.png" alt="image-20221023153655572" style="zoom:20%;" />

3. 打包文件

   ```bash
   npx browserify dist/js/app.js -o dist/bundle.js
   ```

   > :cry:报错如下：
   >
   > ```
   > ParseError: 'import' and 'export' may appear only with 'sourceType: module'
   > ```



# 17 异常处理

异常处理，又称为例外处理，是<strong style="color:red">为了防止程序在运行过程中出现某些意外情况而导致整个程序完全无法运行</strong>，最终使得程序可以让异常处理代码与正常脚本代码（特别是核心代码）相分离，即使发生某些意外情况，程序的核心代码仍然可以正常执行，提高程序的健壮性。



## 17.1 error 对象

我们在定义异常处理结构前首先需要了解到在 JS 中有哪些 error 对象可供我们捕获，JS 中 error 对象一共有如下 6 类：

1. `EvalError`：`eval() `的使用与定义不一致（不常见）
2. `RangeError`：越界错误
3. `ReferenceError`：非法或不能识别的引用数值，即引用错误，一般发生在某个变量未定义
4. `SyntaxError`：语法解析错误
5. `TypeError`：操作数类型错误
6. `URIError`：URI 处理函数使用不当（不常见）

Error 对象的属性（常用）

| 属性    | 描述                                    |
| ------- | --------------------------------------- |
| name    | 设置或返回错误名                        |
| message | 设置或返回错误消息                      |
| stack   | 整个 Error 的错误对象，包括函数的调用栈 |

例如：

```js
try {
    console.log(a);
} catch(e) {
    console.log(e.name);
    console.log(e.message);
    console.log(e.stack);
}
```

执行结果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221222125408399.png" alt="image-20221222125408399" style="zoom:50%;" />





## 17.2 异常处理结构

在 JS 中，我们使用 try…catch…finally 语句来执行异常处理，即通过它来捕获异常，语法如下：

```js
try {
	// 调试代码语句
} catch(e) {
	// 捕获异常，并进行异常处理的代码块
} finally {
	// 出口语句
}
```

**示例**

```js
try {
   console.log("开始执行try语句 --->");
   console.log(a);
} catch(err) {
   console.log("捕捉到异常，开始执行catch语句 --->");
   console.log("错误名称: " + err.name+" ---> ");
   console.log("错误信息: " + err.message+" ---> ");
} finally {
   console.log("开始执行finally块语句 --->")
}
```

执行结果如下：

![image-20221129154312599](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221129154312599.png)



## 17.3 throw 抛出异常

`throw` 语句的作用是创建异常（exception），搭配 try…catch 语句以达到<font color="red">控制程序流并产生精确错误消息的目的</font>，相较于 return 语句单纯的中止优化不少

语法如下：

```js
throw exception
```

exception 的可选参数类型：

1. 基本数据类型，包括了 Number、String、Boolean
2. 引用数据类型，主要是对象，例如 Error 对象



**示例1**

```js
try {
    var x = 10;
    if (x > 15) {
        throw "x > 15"
    } else {
        throw "x < 15"
    }
} catch(e) {
    console.log(e);
} finally {
    console.log("异常处理完毕 -->");
}
```

执行结果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221129163243818.png" alt="image-20221129163243818" style="zoom:50%;" />



**示例2**

```js
try {
  throw new Error('Whoops!')
} catch (e) {
    console.log(e.name);
    console.log(e.message);
    console.error(e.name + ': ' + e.message)
}
```

执行结果如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221129164620837.png" alt="image-20221129164620837" style="zoom:50%;" />



## 17.4 异常处理的执行顺序





# 18  cookie 和 localstorage

## 18.1 cookie

HTTP Cookie（也叫作 Web Cookie 或浏览器 Cookie）<font color="red">是服务器发送到用户浏览器并保存在本地的一小块数据</font>。浏览器会存储 cookie 并在下次向同一服务器发送时携带并发送到服务器上。

> cookie 的存在使得<u>无状态性</u>的 HTTP 记录稳定的状态信息成为了可能

Cookie 的作用：

- **会话状态管理**
- 个性化设置
- 浏览器行为跟踪
- …

**cookie 的特点**

1. 大小限制（不能超过 4k）
2. 每个域下的 cookie 个数不能超过 50 个
3. 具有过期时间（有效期 Max-Age ）expires，超过有效期后被删除，常见的值有：session（表示当前会话有效）和一般时间格式
4. 





# 19 判等

## 19.1 非严格判断

https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Equality_comparisons_and_sameness#%E4%B8%A5%E6%A0%BC%E7%9B%B8%E7%AD%89



# 20 模块化编程

传统的模块化开发存在以下的缺点：

1. 命名冲突
2. 文件依赖

JS 代码越来越大，JS 引入模块化编程，开发者只需要实现核心的业务逻辑，其他的都可以加载别人已经写好的模块。

JS 使用 "模块"（module）的概念来实现模块化编程，解决非模块化编程问题。

![image-20230327225116059](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230327225116059.png)

> JS 中的 "module" 类似于 Java 中的 "package"



## 20.1 common.js 模块化编程介绍

在 ES5 中，每个 JS 文件就是一个模块，有自己的作用域。在文件中定义的变量、函数、类都是私有的，对其他 JS 文件不可见

`CommonJS` 使用 **`module.exports={}`** 或者 **`exports={}`** 导出模块，使用 **`let / const 名称 = require("xxx.js")`** 导入模块

**示例**

![image-20230327233018172](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230327233018172.png)

**文件层级结构**

```
module          
├─ function.js  
└─ use.js       
```

**代码实现**

```js
// function.js
// 定义对象、变量、常量、函数
const sum = function(a, b) {
    return parseInt(a)  + parseInt(b)
}

const sub = function(a, b) {
    return parseInt(a) - parseInt(b)
}

const name = 'Alice'

// 使用module.exports 将成员封装在对象中，以对象的形式导出
module.exports = {
    sum: sum,
    sub: sub,
    myName: name
}
```

```js
// use.js
const m = require('./function')
/**
 * 使用 ES5 的 require 导入模块可以直接省略后缀名.js
 */

console.log(m.sum(2, 1))
console.log(m.sub(2, 1))
console.log(m.myName)
```

:alarm_clock:**模块化细节**

1. 如果我们导入时，不需要引入导入对象的所有成员，部分导入成员可以使用如下写法：

   ```js
   const {sub} = require('./function')
   console.log(sub(1, 2))
   ```

2. 如果导出的成员别名和成员名相同，可以使用如下简写形式：

   ```js
   module.exports = {
       sum,
       sub
   }
   ```

3. ~~`module.exports={}` 我们可以简写为 `exports={}`~~

   



## 20.2 ES6 模块的导出和导入

1. ES6 导出（暴露）的方法有如下三种：

   - **`export {名称 / 对象 / 函数 / 变量 / 常量}`**
   - **`export 定义 = {}`**
   - **`export default {}`**

2. ES6 导入语法

   - `import {} from "xx.js"`

     > 该方法对应导出中的第一和第二种方法

   - `import 名称 from "xx.js"`

     > 该方法对应导出中的第三种方法（主要针对默认导入）

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230328003214752.png" alt="image-20230328003214752" style="zoom:50%;" />

```js
// 导出语法
// 方式一：集中到一些
export {
    sum,
    sub
}

// 方式二：分开导出
export const myName = "Alice"

// 导入语法
/**
 * es6导入模块使用 import 关键字
 * 注意事项：
 * 1. 如果是在 node 环境下使用es6导入语法，需要先通过node init生成package.json
 *    之后通过添加 "type": "module"(默认是 commonjs)
 * 2. 导入时，需要带上文件后缀 .js
 * 3. 导入的成员名和导出的需要保持一致
 */
import { sum, sub } from './function.js'
console.log(sum(1, 2))
console.log(sub(1, 2))
```

```js
// 默认导出
// 这种形式，和之前的 module.exports 类似，都是将导出模块包装成一个对象导出
export default {
    sum(a, b) {
        return parseInt(a) + parseInt(b)
    },
    sub(a, b) {
        return parseInt(a) - parseInt(b)
    }
}

// 导入形式
import m from './function.js';
// 默认导入的可以使用别名来接受默认导出的对象
console.log(m.sub(1, 2))
console.log(m.sum(1, 2))
```



# 21 rest 参数

ES6 中引入 rest 参数，用于获取函数的实参，用来替代 arguments 

```js
function fn(...args) {
    console.log(arguments) // { '0': 'alice', '1': 'bob', '2': 'jack' }
    console.log(args)      // [ 'alice', 'bob', 'jack' ]
}

fn('alice', 'bob', 'jack')  
```



# 22 迭代器

## 22.1 迭代器基础

迭代器（iterator）是一个接口，为各种不同的数据结构提供统一的访问机制，任何数据结构只要部署了 iterator 接口，就可以完成遍历操作

1. ES6 中提供了 `for…of` 循环专供部署链了 iterator 接口的数据结构消费

   > 区分 `for…in` 和 `for…of`：`for……in` 遍历的 value 是 key(索引)，而 `for…of` 遍历的 value 得到的是 value(值)

2. 原生具备 iterator 接口的数据

   - Array
   - Arguments
   - Set
   - Map
   - String
   - TypedArray
   - NodeList

3. 工作原理
   1. 创建一个指针对象，指向当前数据结构的起始位置
   2. 第一次调用对象的 `next()` 方法，指针自动指向数据结构的第一个成员
   3. 接下来不断调用 `next()` 方法，指针后移，直到指向最后一个成员

```js
const team = ['唐僧', '孙悟空', '猪八戒', '沙僧']
let iter = team[Symbol.iterator]()
console.log(iter.next())
console.log(iter.next())
console.log(iter.next())
console.log(iter.next())
console.log(iter.next())
/* { value: '唐僧', done: false }
{ value: '孙悟空', done: false }
{ value: '猪八戒', done: false }
{ value: '沙僧', done: false }
{ value: undefined, done: true } */
```



## 22.2 迭代器自定义遍历

场景：请对下面的对象中的 stu 属性进行遍历

```js
const cl = {
    name: '3年2班',
    stu: [
        'xiaoming',
        'xiaohong',
        'xiaozhang',
        'xiaohua'
    ],
    [Symbol.iterator]() {
        // 建立一个索引变量，控制遍历范围
        let index = 0
        return {
            next: () => {
                if(index < this.stu.length) {
                    const res = {
                        value: this.stu[index],
                        done: false // 为 false 表示当前还有元素
                    } 
                    index++
                    return res
                } else {
                    return {
                        value: undefined,
                        done: true
                    }
                }
            }
        }
    }
}

for(v of cl) {
    console.log(v)
}
```

> 这里之所以不直接使用 `cl.stu.forEach`，主要是考虑到面向对象原则

