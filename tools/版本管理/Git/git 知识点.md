[toc]

# 1 概述

## 1.1 版本控制器的方式

a. 集中式版本控制工具

集中式版本控制工具，版本库是集中存放在中央服务器的，团队里每个人工具时从中央服务器下载代码，是必须要联网才能工作的，局域网或互联网。个人修改后提交到中央版本库

举例：SVN 和 CVS



b. 分布式版本控制工具

分布式版本控制系统中没有"中央服务器"，每个人的电脑上都是一个完整的版本库，这样工作的时候，无需要联网，因为版本库就是在你自己的电脑上。多人协作只需要将各自修改的对方，就能互相看到对方的修改。

举例：Git



## 1.2 SVN

![](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/%E5%88%86%E5%B8%83%E5%BC%8F%E7%89%88%E6%9C%AC%E6%8E%A7%E5%88%B6.png)



## 1.3 Git

Git 是一个开源的<font color="red">分布式版本控制系统(Distributed Version Control System，简称 DVCS)</font>，可以有效、高速地处理从小到大的项目版本管理。Git 是 [Linus Torvalds](https://baike.baidu.com/item/%E6%9E%97%E7%BA%B3%E6%96%AF%C2%B7%E6%9C%AC%E7%BA%B3%E7%AC%AC%E5%85%8B%E7%89%B9%C2%B7%E6%89%98%E7%93%A6%E5%85%B9/1034429?fromtitle=linus&fromid=400810&fr=aladdin) 为了帮助管理 Linux 内核开发而开发的一个版本控制软件。

![分布式管理流程](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/%E5%88%86%E5%B8%83%E5%BC%8F%E7%AE%A1%E7%90%86%E6%B5%81%E7%A8%8B.jpg)

Linus 对开发自己的版本系统做出的要求：

- 速度
- 简单的设计
- 对线性开发模式的强力支持（允许成千上万个并行开发的分支）
- 安全分布式
- 有能力高效管理类似 Linux 内核一样的超大规模项目（速度和数据量）



## 1.4 Git 的学习资料

Git 所有指令大全：https://gitee.com/all-about-git

Git 互动学习网址：https://oschina.gitee.io/learn-git-branching/







# 2 Git 的安装与常用命令

## 2.1 Git 的下载安装

直接进入 Git 官网下载即可：https://git-scm.com/downloads

![image-20220817110735464](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20220817110735464.png)

选择好自己电脑对应的OS

![](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20220817110908509.png)

在这里选择电脑的版本（64 bit or 32 bit）和 Git版本（便携版 or 安装版），一般选择 64 位的安装版本

由于 Git 在国内下载速度非常慢，这里推荐一个镜像网站：https://registry.npmmirror.com/binary.html?path=git-for-windows/

我们在安装完成之后，右键可以可以看到快捷方式中多出了两个选项：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20220817112950401.png" alt="image-20220817112950401" style="zoom: 33%;" />

Git GUI：Git 提供的图形界面工具

Git Bash：Git 提供的命令行工具



## 2.2 Git 的基本配置

### 2.2.1 配置用户信息

我们之所以需要设置用户名和邮件，是因为每次的 Git 提交都需要用到该信息，相关配置如下：

1.打开 Git Bash

2.设置用户信息

```bash
# 设置用户名
git config --global user.name xxx

# 配置用户邮箱（不一定要真实存在）
git config --global user.email xxx
```



3.查看配置信息

```bash
# 查看用户名
git config --global user.name

# 查看配置信息
git config --global user.email
```

> 我们还可以使用 `git config -l` 查看所有的配置信息



当我们配置好用户名和邮箱后，会自动生成 `.gitconfig` 文件，里面包含 git 的所有配置信息，默认位置：`C:\Users\Administrator\.gitconfig`



:herb:**扩展：git 查看配置信息指令**

1. 查看所有配置：`git config -l`
2. 查看系统配置：`git config --system --list`
3. 查看当前用户配置：`git config --global --list`

> - `--list` 也可以简写为 `-l`
>
> - system 配置文件默认位置：`‪C:\Program Files\Git\etc\gitconfig`
> - global 全局配置文件默认位置：`:\Users\Administrator\.gitconfig`





### 2.2.2 为常用指令设置别名

有些常用的指令参数非常多，为了避免输入麻烦，我们可以使用别名

1.打开用户目录，创建<font color="red"> .bashrc </font>文件

注意：部分 windows 系统不允许用户创建点号开头的文件，可以打开 git bash 执行 `touch ~/.bashrc`

2.在 <font color="red">.bashrc</font> 文件中输入如下内容：

```bash
# 用于输出 git 提交日志
alias git-log='git log --pretty=oneline --all --graph --abbrev-commit'

# 用于输出当前目录所有文件及基本信息
alias ll='ls -al'
```

3.打开 Git Bash ，执行 `source ~/.bashrc`

**![image-20220817121454841](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20220817121454841.png)**



### 2.2.3 解决 Git Bash 乱码问题

1. 打开Git Bash 执行下面命令

   ```bash
   git config --global core.quotepath false
   ```

2. `${git_home}/etc/bash.bashrc` 文件最后加入下面两行

   ```bash
   export LANG="zh_CN.UTF-8" 
   export LC_ALL="zh_CN.UTF-8"
   ```

   

## 2.3 获取本地仓库

要使用 Git 对我们的代码进行版本控制，首先需要获得本地仓库

1. 在我们的电脑的任意位置创建一个空目录（例如 test）作为我们的本地 Git 仓库

   ```bash
   mkdir ~/Desktop/test
   ```

2) 进入这个目录，右键打开 Git Bash 窗口
3) 执行命令`git init`
4) 创建成功后可以在文件夹下看到隐藏的 ==.git== 目录

![image-20220817124921099](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20220817124921099.png)



## 2.4 git 工作区及相互关系

git 工作区一共有四个，其中 git 本地一共有三个工作区域：

- Working Directory：工作区，就是平时存放项目代码的地方
- Stage / Index：暂存区，用于临时存放做出的改动，事实上它只是一个文件，保存即将提交的文件列表信息
- Repository / Git Directory：仓库区，安全存放数据的位置，这里有你所提交的所有版本的数据，其中 HEAD 指向的是最新放入仓库的版本

远程还有一个 git 仓库：

- Remote Directory：远程仓库，托管代码的服务器（Github / Gitee）

![image-20230409102744370](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230409102744370.png)



本地工作区的具体转换如下：

![git -仓库01](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/git%20-%E4%BB%93%E5%BA%9301.png)

状态转换的命令：

- git add：工作区 --> 暂存区
- git commit：暂存区 --> 工作区

要查看文件处于什么状态，我们可以使用`git status`命令



### 2.4.1 *查看修改的状态

作用：查看修改的状态（暂存区、工作区）

命令：`git status`

### 2.4.2 *添加工作区到暂存区

作用：添加工作区一个或多个文件的修改到暂存区

命令：`git add 单个文件名 | 通配符`

> 将所有修改加入到暂存区：`git add .`

### 2.4.3 *提交暂存区到本地仓库

作用：提交暂存区内容到本地仓库的当前分支

命令：`git commit -m '注释内容'`

### 2.4.4 *查看提交日志

由于之前，我们设置了 <font color="red">.bashrc</font> 文件，里面配置了相关参数，所以我们可以直接使用 `git-log` 来查看提交的日志

作用：查看提交记录

命令：`git log [option]`

option：

- --al：显示所有分支
- --pretty=oneline：将提交信息显示为一行
- --abbrev-commit：使得输出的 commitID 更加简短
- --graph：以图的形式显示

### 2.4.5 *版本回退

作用：版本切换

命令：`git reset commitID --hard`

> commitID 可以使用 `git-log` 或者 `git log`指令查看

如何查看已经删除的记录（主要查看其 commitID）?

使用命令 `git reflog` 可以查看之前删除的提交记录，但需要注意的是如果我们将 git  托管的文件夹删除，那么就回天乏术了

小技巧:sparkles:

快速复制粘贴 commitID：选中 + 鼠标滚轮（即鼠标中键）

### 2.4.5 添加文件至忽略列表

一般我们总会有些文件无需纳入Git 的管理，也不希望它们总出现在未跟踪文件列表。 通常都是些自动 生成的文件，比如日志文件，或者编译过程中创建的临时文件等。 在这种情况下，我们可以在工作目录 中创建一个名为 <font color="red">.gitignore</font> 的文件（文件名称固定），列出要忽略的文件模式。下面是一个示例：

```bash
# no .a files 
*.a
# but do track lib.a, even though you're ignoring .a files above 
!lib.a
# only ignore the TODO file in the current directory, not subdir/TODO 
/TODO
# ignore all files in the build/ directory 
build/
# ignore doc/notes.txt, but not doc/server/arch.txt 
doc/*.txt
# ignore all .pdf files in the doc/ directory 
doc/**/*.pdf
```



**练习：基础操作**

```bash
#####################仓库初始化###################### 
# 创建目录（git_test01）并在目录下打开gitbash
略
# 初始化git仓库 
git init
#####################创建文件并提交##################### 
# 目录下创建文件    file01.txt
略
# 将修改加入暂存区 
git add .
# 将修改提交到本地仓库，提交记录内容为：commit 001 
git commit -m 'commit 001'
# 查看日志 
git log
####################修改文件并提交###################### 
# 修改file01的内容为：count=1
略
# 将修改加入暂存区 
git add .
# # 将修改提交到本地仓库，提交记录内容为：update file01 
git commit --m 'update file01'
# 查看日志 
git log
# 以精简的方式显示提交记录
git-log
####################将最后一次修改还原################## 
# 查看提交记录
git-log
# 找到倒数第2次提交的commitID
略
# 版本回退
git reset commitID --hard
```



## 2.5 Git 分支常用指令

几乎所有的版本控制系统都以某种形式支持分支。  使用分支意味着你可以把你的工作从开发主线上分离开来进行重大的Bug修改、开发新的功能，以免影响开发主线。

我们可以通过 `git-log` ，看以看到分支明细和各自操作，其中 `HEAD -->`指向的是当前分支，如下图：

![image-20220817161830763](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20220817161830763.png)

### 2.5.1 查看本地分支

```bash
git branch
```

### 2.5.2 创建本地分支

```bash
git branch 分支名
```

### 2.5.3 *切换分支

```bash
git checkout 分支名
```

我们还可以直接切换到一个不存在的分支（创建并切换）

```bash
git checkout -b 分支名
```

### 2.5.4 *合并分支

一个分支可以合并到另一个一个分支

```bash
git merge 分支名称
```

### 2.5.5 删除分支

```bash
# 删除分支时，需做各种检查
git branch -d 分支名称

# 强制删除分支
git branch -D 分支名称
```

扩展：强制删除分支的场景

首先要明确一点：不能删除当前分支，如果我们同时创建了一个分支同时提交代码到仓库时，我们此时切换到另一个分支，准备删除它时，git 会认为你是误删，所以会询问，如果要强制删除，请使用选项 -D

### 2.5.6 解决冲突

当我们在两个或多个分支上对文件进行修改可能会存在冲突，例如同时修改了同一文件的同一行后对其进行合并，那么此时 Git 不知道你要修改成分支1还是分支2，我们就需要解决冲突，解决冲突的步骤如下：

1. 处理文件中冲突的地方
2. 将解决完冲突的地方加入到暂存区（add）
3. 提交到仓库（commit）

示例：

```bash
# 在test文件中创建文件 file01.txt
touch file01.txt
# 创建分支 dev01
git branch dev01
# 在 master 分支中修改文件,里面插入 count = 1 后保存退出
vim file01.txt
# 上传提交
git add .
git commit -m 'change file01 count = 1'
# 回到 dev01 分支，同样修改文件 fil01 里面 count = 2
git checkout dev01
git add .
git commit -m 'change file01 count = 2'
# 合并分支
git checkout master
git merge dev01
```

准备工作完成之后，查看冲突内容

![image-20220817174839554](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20220817174839554.png)

我们可以看到合并错误

这时候的文件会将两份修改都放在一起，我们只需要按以上方法步骤解决即可：

![image-20220817183129600](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20220817183129600.png)

### 2.5.7 开发中分支使用原则和流程

几乎所有的版本控制系统都以某种形式支持分支。使用分支意味着你可以把你的工作从开发主线上分离开来进行重大的Bug修改、开发新的功能，以免影响开发主线。

在开发过程中，一般有如下分支使用原则和流程：

- master（生产）分支

  线上分支、主分支，中小规模项目作为线上运行的应用对应的分支

- develop（开发）分支

  是从 master 创建的分支，一般作为开发部门事务主要开发分支，如果没有其他并行开发不同期上线要求，都可以在此版本进行开发，阶段完成后，需要合并到 master 分支，准备上线

- feature/xxxx分支

  从 develop 创建的分支，一般是同期并行开发，但不同期上线时创建的分支，分支上的研发任务完成后合并到 develop 分支

- hotfix / xxxx 分支

  从 master 派生的分支，一般作为线上 bug 修复使用，修复完成后需要合并到 master 、test、develop 分支

- 还有一些其他的分支，在此不在详述，例如 test 分支 （用于代码测试）、pre 分支（预上线分支）等等

![Git-branch](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/Git-branch.jpg)

### 2.5.8 补充：快进模式（Fast-forward)

快进模式是指在使用git 进行分支合并的过程中，直接跳过合并，而使要合并的分支直接跳到另一分支的状态。举例如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20220817212501692.png" alt="image-20220817212501692" style="zoom:50%;" />

这时候如果我们将分支 dev01 和 master 合并会发现，master会直接跑到 dev01 的状态，而不会形成一个折线拱桥，说明 git 在处理这段合并的时候是==直接将 mater 的状态切换到 dev01 的状态==，这边是所谓的快进模式

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20220817212633258.png" alt="image-20220817212633258" style="zoom:50%;" />



## 2.6 Git 的工作流程

1. 在本地工作目录中添加、修改文件
2. 将需要进行版本管理的文件放入到暂存区
3. 将暂存区中的文件提交到 git 本地仓库

基本流程图如下：

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230409104427051.png" alt="image-20230409104427051" style="zoom:50%;" />



Git 中，文件被统一管理， Git 中的文件大致有四种状态：

- Untracked
- Unmodified
- Modified
- Staged

**查看文件状态**

- 查找指定文件状态：`git status [filename]`
- 查看所有文件状态：`git status`



## 2.7 .gitignore 忽略文件

对于有些文件，例如：数据库文件、临时文件、设计文件、外部库文件（例如外部 jar 包和 node_modules 下的依赖等），我们对于这些文件，往往是不需要纳入版本控制的，所以需要建立 .gitignore 文件用来忽略某些文件，忽略文件的规则我们可以搭配通配符使用，最常用的规则如下：

1. 以 "#" 开头的规则表示这是一个注释
2. 通配符 `*` 用于匹配多个字符，`?` 用于匹配单个字符,`[]` 用于匹配列表，即匹配任何一个在方括号中的字符
3. 以 `!` 开头的表示 "例外"，即不被忽略的



## 2.8 git 终端命令的操作

- ctrl+u 删除命令行开始至光标处
- ctrl+k 删除光标处至命令行结尾
- ctrl+a 光标移动到最前面
- ctrl+e 光标移动到最后面

 





# 3 Git 远程仓库

前面我们已经知道了Git中存在两种类型的仓库，即本地仓库和远程仓库。那么我们如何搭建 Git 远程仓库呢？

我们可以借助互联网上提供的一些代码托管服务来实现，其中比较常用的有GitHub、码云、GitLab等。 

- [GitHub](https://github.com/)是一个面向开源及私有软件项目的托管平台，因为只支持Git 作为唯一的版本库格式进行托管，故名 GitHub
- [码云](https://gitee.com/ )是国内的一个代码托管平台，由于==服务器在国内==，所以相比于 GitHub，码云速度会更快
- [GitLab]( https://about.gitlab.com/) 是一个用于仓库管理系统的开源项目，使用Git作为代码管理工具，并在此基础上搭建起来的web服务,一般用于在==企业、学校等==内部网络搭建git私服

## 3.1 远程仓库托管设置

### 3.1.1 注册码云

<img src="https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20220817214342773.png" alt="image-20220817214342773" style="zoom:33%;" />

### 3.1.2 创建仓库

![image-20220817215222390](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20220817215222390.png)

### 3.1.3 配置 SSH 秘钥

第一步：生成 SSH 公钥

```bash
ssh-keygen -t rsa
```

之后不断回车即可

> 我们可以看到所使用的加密算法为RSA ，属于不对称加密
>
> 如果公钥已经存在，则会自动覆盖，生成的 SSH 公钥会存放在用户文件夹下的 .ssh 目录下，该目录下应该有两个文件：id_rsa（私钥文件） 和 id_rsa.pub（公钥文件）

第二步：Gitee 设置账户公钥

获取公钥

```bash
cat ~/.ssh/id_rsa.pub
```

将获得的内容全部复制粘贴到 Gitte 设置里的 SSH公钥设置：

![image-20220817223800431](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20220817223800431.png)

第三步：验证配置是否成功

```bash
ssh -T git@gitee.com
```

注意：如果出现以下错误提示：

```
Warning: Permanently added 'gitee.com' (ED25519) to the list of known hosts.
Connection closed by 212.64.63.215 port 22
```

那么可以通过重新生成秘钥后重复上述操作





## 3.2 操作远程仓库

### 3.2.1 添加远程仓库

此操作是先初始化本地仓库，然后与已创建的远程库进行对接。

```bash
git remote add <远端名称><仓库路径>
```

- 远端名称，<font color="red">默认是 origin</font> ,取决于远端服务器设置

- 仓库路径，从远端服务器获取此URL，获取方式如下：

![image-20220817225935553](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20220817225935553.png)

- 例如：我的设置是：`git remote add origin https://gitee.com/Jiuling01/my_storage_git.git`

### 3.2.2 查看远程仓库

```bash
git remote 
```

示例：

![image-20220817234407831](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20220817234407831.png)

### 3.2.3 推送到远程仓库

```bash
git push [-f] [--set-upstream] [远端名称 [本地分支名][:远端分支名]]
```

- 如果远端分支名和本地分支名相同，则可以只写本地分支，例如：

  ```bash
  git push origin master
  ```

- -f 代表强制覆盖，当我们同时对远程仓库的分支和本地的同一个对应分支做出修改时，可使用此选项来强制以本地做出的修改为准
- `--set--upstream`推送到远端的同时并且建立起和远端分支的关联分支，如果当前分支已经和远端分支关联，则可以省略分支名和远端名

当我们推送成功后，可以看到我们的远程仓库中已经有了内容

![image-20220817234729869](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20220817234729869.png)

![image-20220817234758248](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20220817234758248.png)

:heavy_exclamation_mark:<font color="red">注意：当我们建立本地分支和远程分支的关联关系时，一定要确保本地仓库一定要有内容</font>

### 3.2.4 本地分支和远程分支的关联关系

查看关联关系，我们可以使用`git branch -vv`命令

![image-20220817235155372](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20220817235155372.png)

我们可以看到，此时的状态代表没有关联关系

当我们输入：`git push --set-upstream origin master:master` 

![image-20220817235707820](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20220817235707820.png)

后，会发现本地的 master 分支已经和远程仓库的 origin 下的 master 分支已经绑定了，这时候当<font color="red">我们处于 master 分支</font>时可以直接输入`git push`来完成更新远程仓库。但是但我们改变当前分支，则会提示我们没有绑定的远程分支，如下：

![image-20220818000112840](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20220818000112840.png)



### 3.2.5 克隆远程仓库

如果已经有一个远端仓库，我们直接直接 clone 到本地

```bash
git clone <仓库路径> [本地目录]
```

- 本地目录可以直接省略，会自动生成一个目录

![image-20220818001001466](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20220818001001466.png)

![image-20220818001100787](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20220818001100787.png)

![image-20220818001040298](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20220818001040298.png)

注意：由于我是在 test 文件夹中打开的 Git Bash ，所以会直接将远程仓库克隆到 test 文件夹中

### 3.2.6 从远程仓库抓取和拉取

远程分支和本地分支一样，我们可以进行 merge 操作，只是需要先把远端仓库里面的更新都下载到本地，再进行操作。

> git clone 不可能频繁使用，不然每次都要 clone 一个一模一样的远程仓库将十分的麻烦且占本地存储空间

1. 抓取

   ```bash
   git fetch [remote name] [branch nanme]
   ```

   - <font color="red">抓取指令就是将仓库的更新都抓取到本地，不会进行合并</font>

   - 如果不指定远端名和分支名，则抓取所有分支

2. 拉取

   ```
   git pull [remote name] [branch name]
   ```

   - <font color="red">拉取命令就是将远端仓库的修改拉倒本地自动进行合并，等同于 fetch + merge</font>

   - 如果不指定远端名称和分支名，则抓取所有并更新当前分支

测试：我们首先在 test 文件夹创建一个新文件 file03.txt，之后再将其上传到暂存区后提交到本地仓库再推送到远程仓库

![image-20220818065100654](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20220818065100654.png)

当我们使用 `git  fetch` 之后，发现 file03.txt 并没有出现在 clone 的文件夹中，我们将其合并，输入`git merge origin/master`之后才发现出现了 file03.txt 文件，而 `git pull` 则是直接将上面两个步骤合并到一起

### 3.2.7 解决合并冲突

如下图，当我们同时对本地文件夹和克隆文件夹的同一份文件的同一行做出不同修改时，由于此时远程仓库已经发生变化，当我们抓取并合并到克隆的仓库时会发生冲突

![git-冲突](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/git-%E5%86%B2%E7%AA%81.jpg)



背景：在一段时间，A、B用户修改了同一个文件，且修改了同一行位置的代码，此时会发生合并冲突。 
B用户在本地修改代码后优先推送到远程仓库，此时A用户在本地修订代码，提交到本地仓库后，也需要推送到远程仓库，此时A用户晚于B用户，故需要先拉取远程仓库的提交，经过合并后才能推送到远端分支

在B用户拉取代码时，因为A、B用户同一段时间修改了同一个文件的相同位置代码，故会发生合并冲突。**远程分支也是分支，所以合并时冲突的解决方案和解决本地分支冲突相同**



# 4 git 代理

**设置代理**

```bash
git config --global http.proxy 'http://127.0.0.1:7890' 
git config --global https.proxy 'http://127.0.0.1:7890'
```



**查看代理**

```bash
git config --global --get http.proxy
git config --global --get https.proxy
```



**取消代理**

```bash
git config --global --unset http.proxy
git config --global --unset https.proxy
```



# 5 git 分支详细探讨

git 分支指令一览：https://gitee.com/all-about-git

```bash
# 列出所有本地分支
git branch

# 列出所有远程分支
git branch-r

# 列出所有本地分支和远程分支
git branch -a

# 新建一个分支，但依然停留在当前分支
git branch [branch-name]

# 切换指定分支，并更新工作区
git checkout [brach-name]

# 切换到上一个分支
git checkout -

# 新建一个分支，并切换到该分支
git checkout -b [branch]
```



```bash
# 在本地合并指定分支到当前分支
git merge [branch]

# 将本地分支版本上传到远程分支版本合并
git push <远程主机名> <本地分支名>:<远程分支名>
# 注意：远程主机名一般为 origin,如果本地分支名和远程分支名一致，则可以简写为 git push <远程主机名> <本地分支名>

# 如果本地版本与远程版本有差异，但又要强制推送可以使用 --force 参数(不推荐)
git push --force origin master
```



```bash
# 删除本地分支
git push -d  [branch-name]

# 删除远程分支
git push origin --delete [brach-name]
git branch -dr remote/branch # 例如git branch -dr origin/dev01
```



```bash
# 从远程库中获取某个分支，再与本地指定分支合并
git pull <远程主机名> <远程分支名>:<本地分支名>
```



# 6 git 撤销操作

```bash
# 重置暂存区的指定文件
git reset [file]

# 软着陆
git reset --soft 版本号

# 硬着陆
git reset --hard 版本号
```

