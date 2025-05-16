> 使用 project 版本为 2021

[toc]

# 1 project 基础

## 1.1 project 界面

当我们第一次打开 project ，默认的视图为【甘特图视图】，分为左右两块，左边为【工作表区域】

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230524204801092.png" alt="image-20230524204801092" style="zoom:50%;" />

右边的甘特图是自动生成的，==甘特图的起点为任务的开始时间，甘特图的终点为任务的结束时间，甘特图的长度代表工期长度==

我们在甘特图时间刻度区域或者灰色条纹区域双击可以打开时间刻度窗口，如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230524205918411.png" alt="image-20230524205918411" style="zoom:50%;" />

我们可以在软件界面左侧右键打开视图切换窗格进行视图切换

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230524210757190.png" alt="image-20230524210757190" style="zoom:50%;" />

其实严格意义上来讲，首页打开的甘特图视图是指【带日程表的甘特图视图】

我们可以在【视图】=> 【日程表】中对其进行打开和关闭操作



## 1.2 project 功能介绍

project 的功能有很多，但是归纳起来，核心功能其实就三个，分别是进度管理、资源管理和成本管理

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/centeral.png" alt="centeral" style="zoom: 33%;" />

在 project 中，<strong style="color:red">进度是一切的基础</strong>，所以上图中进度管理位于金字塔的基层，没有进度管理我们也就无法对资源和成本进行管理

依托上述结构，我们将对应的功能进行拆解

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/funciton%20point.png" alt="funciton point" style="zoom: 33%;" />

我们要掌握如何利用 project 进行进度管理就必须掌握如下内容：

1. 甘特图如何去绘制以及其中设计的信息（例如各个颜色块的意义以及长度越长意味着什么？）
2. 日历安排
3. 依赖关系
4. 视图
5. **关键路径**



## 1.3 project 工作表和  Excel 工作表之间的区别

1. project 中所有的列都是内置的，一共 ==415== 列，不能删除，只能隐藏,并且每一列只能识别特别数据类型的输入

   <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/column_2.png" alt="column_2" style="zoom: 33%;" />

2. 在 project 中不可以为预置列设置公式



## 1.4 project 背后蕴含的思想与原理

**进度管理原理**

1. 任务越早开始越好

2. 要有风险思维，做计划要留有 Buffer 余量

3. 设置基准线

4. 利用关键路径压缩工期

5. 考虑期限，并且要区分工作日和非工作日

   > 在期限中就包括里程碑，里程碑中可能存在强制性完成的节点

6. 针对企业创建日历，例如法定节假日、调休、5天工作制、6天工作制、全年无休、大小周交替等
7. 进度受资源配置影响



**成本管理原理**

在项目成本管理中，我们更加在乎的时项目的直接成本和毛利，其中：

- 直接成本 = 材料成本 + 人工成本 + 其他直接成本

  > project 中将项目的直接成本通过三类不同的资源来实现，分别是<u>工时类资源、材料资源、成本资源</u>



估算成本的方式 => 自下而上估算

![成本](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/%E6%88%90%E6%9C%AC.png)



$成本差异 = (当前)成本 - 基线成本$



**挣值管理分析**

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230524234733303.png" alt="image-20230524234733303" style="zoom:50%;" />



## 1.5 project 默认配置的修改

对 project 配置的修改主要包括 【任务模式】和【任务类型】 的修改

1. 将手动计划 => 自动计划

   <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230525000328664.png" alt="image-20230525000328664" style="zoom:50%;" />

2. 将任务类型由【固定单位】 => 【固定工期】

   <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230525000749985.png" alt="image-20230525000749985" style="zoom: 50%;" />





其他常规设置

**01 日期格式的设置**

【选项】 => 【常规】 => 【日期格式】

![image-20230525001419446](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230525001419446.png)



**02 设置日程**

【选项】 => 【日程

![image-20230525001929571](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230525001929571.png)



**03 设置编辑栏显示**

【选项】 => 【显示】

![image-20230525002120874](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230525002120874.png)



:warning:**思维误区**

如果我们希望将每日工作 5 天变成每周工作 6 天，我们能否直接通过修改每周工时/ 每日工时实现？

其实是**不可以**的，如果我们真的希望修改工作制，例如从标准日历改成 6 天工作制或者说 7 天工作制，我们应该在日历中的 【更改时间窗口】 进行设置，而不应该在【选项】中设置，选项中的设置影响的是工期换算出来的工作小时数

> 因为在同一份甘特图视图左侧的工作表中的工期，其单位有可能不同，而我无论单位是工作日、还是周或者月，最后都需要转换成工时，选项中的设置最终影响就是这个转换成工时最终影响的结果



## 1.6 设置项目信息

当我们在创建一个 project 项目时，第一步就是设置对应的项目信息，在 【项目】 => 【项目信息】中进行设置，我们可以在其中设置开始时间、完成时间、日历等，如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230525085007882.png" alt="image-20230525085007882" style="zoom:50%;" />

如果我们设置的是开始日期，那么这种计划安排就被称为是【正排计划】；如果我们设置的是结束日期，那么这种计划安排就被称为是【倒排计划】

项目优先级的范围是：[1, 1000]，我们通过设置项目的优先级来进行调配资源

项目信息的设置十分重要，如果我们经常忘记设置项目信息，可以在【选项】 => 【高级】 => 【常规】 中强制在创建新项目时提示项目信息，这样每次新建项目时都会自动弹出配置项目信息的模态框



## 1.7 设置项目日历

在项目信息中的日历设置中可能没有我们想要的日历，这个时候就需要我们自己手动新建一个日历，接下来，我们将在 project 中设置以下三种日历

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230525091206418.png" alt="image-20230525091206418" style="zoom:50%;" />

首先，我们需要在 【项目】 => 【更改工作时间】 => 【新建日历】

具体操作和注意事项如下：

![image-20230529003411722](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230529003411722.png)



**设置六天工作日或七天工作日**

我们只需要在工作周中进行设置即可，操作如下

![image-20230529102135089](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230529102135089.png)

之后设置【对所列日期设置以下特定工作时间】即可

注意：<strong style="color:red">无论是选择六天工作日还是七天工作日还是全年无休，我们都需要到工作周中设置，其中需要特别注意的是例外日期如果和工作周发生冲突，那么例外假期的优先级更高</strong>



如果我们想要设置大小周交替，那么我们就需要到【例外假期】中单独设置

![image-20230529110726473](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230529110726473.png)



## 1.8 检查日历设置是否正确

检查日历设置的方式是否正确，我们有两种方式：

1. 通过自定义任务进行检验
2. 通过在甘特图中进行检验，其中默认灰色的是非工作日

![image-20230529111410336](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230529111410336.png)

> 在甘特图中检验时，特别注意：项目日历和时间刻度日历的设置可能使用的不是同一个日历，但是项目中的日历不会影响进度计算，但是时间刻度的日历并不会影响进度，只是显示会出问题



## 1.9 保存设置的日历永久使用、删除或重命名日历

我们如果想要某些自己设置的日历能够永久保存，那么我们可以到【信息】 => 【管理器】=> 【日历】 中进行设置

![image-20230529124055972](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230529124055972.png)



## 1.10 创建任务清单

创建一个初始的项目计划分为三步，分别是：

1. 创建任务分解结构（WBS）
2. 设置任务之间的依赖关系/层级关系
3. 估算工期



**一、创建任务分解结构**

01 如何修改字段显示名

右键列 => 选择【域设定】 => 在标题字段中进行修改

<img src="../../../../AppData/Roaming/Typora/typora-user-images/image-20230529161159418.png" alt="image-20230529161159418" style="zoom:33%;" />





**二、设置任务之间的层级关系**

01 升级与降级的操作

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230529162109045.png" alt="image-20230529162109045" style="zoom:33%;" />



02 显示大纲数字

我们找到菜单栏下面的格式，勾选【大纲数字】即可

![image-20230529163904470](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230529163904470.png)



03 区分摘要任务和子任务

![image-20230529164459391](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230529164459391.png)

摘要任务对应 PMBOK 中的工作包，而子任务则对应工作包中的活动，摘要任务本身是虚拟的，只是为了方便查看 WBS 设置的



04 查看 WBS 和 大纲数字列

我们右键某一列，选择插入列，分别插入【WBS】 和 【大纲数字】，其实可以发现，这两列是完全一样的

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230529171717530.png" alt="image-20230529171717530" style="zoom:33%;" />

但是，他们实际上是有区别的，大纲数字是按照任务的层级结构进行设置的，也就是是由程序计算得来的，该列的内容是冻结的，我们无法对其进行修改，而 WBS 是可以修改的，我们可以将其换成其他内容，如果我们想要恢复原来的数字内容，可以直接通过 delete 还原



05 项目摘要任务

项目摘要任务是一个特殊的工作包，我们可以在【格式】 => 【项目摘要任务】中将其开启显示

![image-20230529172657576](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230529172657576.png)

它显示了该项目整个工期，我们可以可以修改其项目名（摘要名）



:bulb:**设置大纲级别的另外一种方式**

我们除了使用【升级】和【降级】的方式改变任务的级别，我们还可以通过设置【大纲级别】来修改

方式：插入【大纲级别】列，之后修改级别数字即可，对应关系为："1" 代表【第一级别】，"2" 代表【第二级别】，"3" 代表第三级别…



**三、设置任务备注**

设置方式：双击任务名称行，弹出【任务信息】窗口，找到【备注】，输入要添加备注的信息内容

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230529183118728.png" alt="image-20230529183118728" style="zoom:50%;" />

当我们设置完备注之后，可以看到在【标记】列中会出现小图标

![image-20230529183246274](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230529183246274.png)

我们还可以插入【备注】列，然后在单元格中设置，如果想要删除备注，可以在【任务】菜单栏中找到橡皮擦工具，使用该工具将备注删除



**四、估算任务工期**

估算工期常用的方法主要有如下几种：

- 专家估算法

- 类比算法

- 根据工作量估算（参数估算）

- 三点估算：根据最乐观时间、最可能时间、最悲观时间来进行估算

  ![image-20230529222335238](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230529222335238.png)



:herb:**帕金森定律**

在估算项目工期时，有一个==帕金森定律==，也即一个人或组织总是要到项目截止时间临近的时候才开始去做，给多长的时间就会花费多长的时间

![image-20230529223650741](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230529223650741.png)

所以我们在安排项目时间的时候一定要秉持 "前紧后松" 的原则，对每个任务分配的时间应尽可能在预估范围左右





:thought_balloon:**思考：我们为什么要设置任务之间的依赖关系？**

我们在设置了任务的依赖关系之后，整个项目的任务就会形成一个有机的整体，当你一个任务发生延迟，我们就能看到对整个计划的影响，这样在更新计划时就会有更好的警示性和预见性

![image-20230529184731507](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230529184731507.png)

 

## 1.11 任务之间的依赖关系

任务之间的依赖关系主要有四种：

- 完成开始（FS）：前面的任务完成之后，后面的任务才开始

  ![image-20230529185410318](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230529185410318.png)

- 开始开始（SS）：任务可以同步开始

  ![image-20230529193455598](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230529193455598.png)

  > 设置方式：在对应任务的前置任务单元格中输入 `xss`  即可（其中 "x" 代表标号），如果有多个前置任务，那么标号之间以逗号分隔

- 完成完成（FF）：前置任务完成后，后置任务也需要完成

  ![image-20230529200350353](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230529200350353.png)

  > FF 关系，我们不推荐使用，因为他不满足 "越早开始越好" 的原则

- 开始完成（SF）：前置任务开始之前，后置任务就已经完成（不推荐）

  ![image-20230529200606490](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230529200606490.png)



**设置前置任务**

- 直接在【前置任务】列中设置，里面填入任务的标号即可（对应行号）

- 在下拉菜单中显示的树形任务结构中选择

  <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230529192819425.png" alt="image-20230529192819425" style="zoom:50%;" />

- 选中两个任务后点击【建立链接】的按钮

  <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230529193044050.png" alt="image-20230529193044050" style="zoom:50%;" />

- 双击任务，打开【任务信息】窗格，里面有设置前置任务的表格

  <img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230529193314381.png" alt="image-20230529193314381" style="zoom:50%;" />



:warning:**特别注意**

虽然摘要任务之间也是可以设置层级关系的，但是我们不推荐给摘要任务之间或摘要任务与子任务之间设置层级关系，因为这会对后面使用==分层==求==关键路径==产生影响



**检查前置任务设置是否正确**

检查的方式有很多，其中一种便是我们通过显示==后续任务==（注意：是 "后续任务"， 而不是 "WBS 后续任务"），==后续任务==  与 ==前置任务== 相对，例如 B 的前置任务是 A ，那么 A 的后续任务就是 B，我们可以通过审查==后续任务==来判断自己的前置任务设置有没有出问题



# 2 提升技能

## 2.1 去掉工期中的问号、将任务的工期单位设置为周或者月

在默认情况下，如果我们没有给任务设置工期，那么它默认是1个工作日后面添加上问号，如果我们想要去掉该问号，可以到【选项】 => 【日程】中设置

![image-20230529230435526](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230529230435526.png)

我们也可以到【任务】 => 【信息】中将 "估计" 取消勾选

![image-20230529230626597](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230529230626597.png)



如果我们想要修改默认的工期显示单位，同样也是到【日程】中进行设置，修改工期显示单位即可

同时，我们在输入的时候，可以通过键入  "1w" 代表 1 周工时，"1mon" 代表一月工时



## 2.2 工期设置原理

摘要工期设置原理如下：

![image-20230529234456446](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230529234456446.png)

即：$子任务中最晚完成时间 - 子任务中最早开始时间 = 工期$



我们在使用 project 进行进度计划设置的时候会发现，我们几乎是不需要设置任务的开始时间和完成时间的，只需要录入任务的工期即可

![image-20230529234958134](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230529234958134.png)

这是由于 project 是基于 "项目越早开始越好" 原则，保证项目拥有最大的总浮动时间，即应急避险时间

![image-20230530000311914](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230530000311914.png)



## 2.3 设置任务日历

我们在前面知道了如何设置项目日历，但是可能会存在我们需要为某个任务的负责团队单独设置日历，那么这时候就要求我们设置任务日历

- 如果是单任务，那么我们可以点击任务，打开【任务信息】窗口，切换到【高级】中进行设置

  ![image-20230530001735386](../../../../AppData/Roaming/Typora/typora-user-images/image-20230530001735386.png)

- 如果是多任务，那我们则需要选中多个任务，然后点击【任务】=>【信息】中，找到【高级】，然后设置日历即可
- 除此之外，我们还可以使用一种更为简便的方法，那就是插入【任务日历】一列，我们就可以单独为每一个任务进行设置



## 2.4 设置里程碑任务

设置里程碑任务一共有三种方式：

1. 双击任务，打开【任务信息】窗口，找到高级，将其标记为里程碑

   ![image-20230530004958495](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230530004958495.png)

2. 插入【里程碑】列，然后进行设置

3. 当工期为0时，那么该任务默认就是一个里程碑任务



## 2.5 创建周期性任务

对于有些任务，我们是每隔一段时间就需要展开一次，例如进行代码审查等，设置方式如下：

在==任务==菜单栏中有任务选项，设置==任务周期==

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230530084840535.png" alt="image-20230530084840535" style="zoom:33%;" />

如果我们设置的工期数是以小时为单位，我们想要日期显示具体到小时，那么我们需要到【选项】 => 【project 视图】 => 【日期格式】 中去设置

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230530090516036.png" alt="image-20230530090516036" style="zoom:50%;" />



## 2.6 修改列名称、插入新列

选中列，右键 => 【域设定】 => 打开【字段窗格】 进行设置即可

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230530091713102.png" alt="image-20230530091713102" style="zoom:50%;" />

新建自定义列的方式：右键列 => 选择插入【插入列】 => 之后键入自定义列的名称即可

> 注意：此时我们设置的自定义列会自动占用一个文本字段，我们可以到【自定义字段】中进行查看，在里面进行编辑



## 2.7 制作下拉菜单

![image-20230604105607197](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230604105607197.png)

<strong style="color:red">注意：只有自定义列才能使用查阅或设置公式，而对于预置列（例如：工期、开始时间、完成时间）都无法设置下拉菜单或者公式</strong>



## 2.8 标记列

标记列拥有友好的提示功能，如下：

![image-20230604160141130](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230604160141130.png)



## 2.9 查找前置任务和后续任务

如果我们想要查看某一个任务的前置任务，那么我们可以直接在任务表中查看，另外还有一种更加直观的方式，就是在 【视图】 => 【详细信息】 里面打开【任务窗格】，然后打开【前置和后续任务】即可形成复合视图

![image-20230604165255172](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230604165255172.png)

如果我们想要查看多个前置任务，可以在【甘特图格式】中找到【任务路径】，选择==前置任务== 或者 ==后置任务==

区分前置任务、前置驱动任务和后置任务、后置驱动任务

![image-20230604192913544](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230604192913544.png)

- 前置驱动任务其实是前置任务的一种，为直接前置
- 后置驱动任务同时是后置任务的一种，为直接后置



## 2.10 不同 Project 版本之间的保存、加密可行性

![image-20230604203520599](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230604203520599.png)

> 从上图我们可以知道：高版本 Project 向下兼容低版本，也就是高版本可以打开低版本的 Project 的 mpp 文件，但是反过来是不行的

所以，我们在日常工作中，为了避免兼容性问题，经常需要将 mpp 文件保存为低版本的，设置方法如下：

在【选项】的【保存】中更改保存的文件格式即可，如下：

![image-20230604204947486](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230604204947486.png)

设置的 project 加密方式如下：

![image-20230604205221647](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230604205221647.png)



## 2.11 使用 project 生成和设置单代号网络图

目前 project 仅支持单代号网络图，在 project 中也就是==网络图==，如下：

![image-20230604210346861](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230604210346861.png)

默认情况下：

- 关键任务：红色矩形
- 非关键任务：蓝色边框，无底色矩形
- 关键里程碑：红色六边形
- 非关键里程碑：蓝色边框，无底色六边形
- 关键摘要任务：红色平行四边形
- 非关键摘要任务：蓝色平行四边形



## 2.12 创建资源基本信息

项目直接成本主要包括：<u>工时类资源、材料类资源、成本类资源</u>

![image-20230604220342051](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230604220342051.png)

在项目的一开始是没有资源工作表的，需要我们自己手动创建

**总结**

![image-20230604223313818](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230604223313818.png)



## 2.13 任务类型

资源分配中，主要有三种任务类型，分为：固定单位、固定工期和固定工时

![image-20230604231833413](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230604231833413.png)

![image-20230604231845320](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230604231845320.png)



**任务工期与（工时类）资源数量之间的关系**

我们可以使用林格曼效应来解释

![image-20230604232624391](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230604232624391.png)
