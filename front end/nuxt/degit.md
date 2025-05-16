degit 是一种替代 `git clone` 的工具。

传统 `git clone` 的应用场景

01 直接 clone 整个项目，会将默认主分支（master / main）和其 commit 记录以及所有的远程分支全部拉取下来，速度较慢

```bash
git clone https://github.com/vitest-dev/vitest.git
# or GitHub 登陆情况下
git clone git@github.com:vitest-dev/vitest.git
```



02 获取固定分支

```bash
git clone -b branchname
```

```bash
# 获取固定分支，这种方式仍然会将默认的主分支带上，且由于多一个指定分支，所以拉取速度更慢
git clone -b release https://github.com/vitest-dev/vitest.git
```

```bash
# 只拉取指定分支，不拉取默认分支，添加 --single-branch 指令
git clone -b release --single-branch https://github.com/vitest-dev/vitest.git
```



03 拉取指定分支最新的代码，不要 commit，添加 `--depth=1` 指令

```bash
git clone -b release --depth=1 --single-branch https://github.com/vitest-dev/vitest.git
```



虽然方法 3 已经已经非常追求极致了，但是还是存在一个问题，就是会携带 `.git` 文件夹，如果我们不想要该文件夹，那么就可以使用我们今天的主角 —— degit

degit 是一个 npm package，需要全局安装才能使用

```bash
npm install -g degit
```

拉取指定远程分支到本地中

```bash
degit vitest-dev/vitest#main my_project_name
```

 