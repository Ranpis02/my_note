> git version 2.45.1.windows.1

# 1 git status 

`git status` 命令可以用来查看文件的提交状态

例如：

| prompt                        | graphic sign | desc                       |
| ----------------------------- | ------------ | -------------------------- |
| Untracked files               | `U`          | 未追踪文件                 |
| Changes to be committed       | `A`          | 暂存区文件                 |
| Changes not staged for commit | `M`          | 暂存区或资源库被修改的文件 |



`git status` 有存在额外的信息，我们可以使用 `git status -s` 简化信息 

```shell
$ git status -s
 M 2.txt
AM 3.txt
?? 4.txt
```

- `M`：已经提交（commit）后经过修改的文件
- `AM`：暂存区的文件被修改
- `??`：未追踪的文件



# 2 git add

`git add` 是个多功能命令，主要作用有如下两个：

1. 将未追踪文件加入到追踪文件队列
2. 将新添加的文件添加到暂存区



# 3 undo operation



1. 对提交文件的注释不满意或者提交完后想起将其他文件合并提交：`git commit --amend`

   - 修改已提交文件的注释

      ```shell
      git commit password.txt -m 'manage password'
      git commit --amend
      ```

      进入文本编辑界面，按 `i` 进入编辑模式，修改完成后 `:wq` 保存退出

2. 将已添加到暂存区的文件取消暂存

   ```shell
   git add README.md
   git reset HEAD README.md
   ```

   > [!important]
   >
   > 1. 其中 `HEAD` 为指向当前分区的指针
   >
   > 2. `git reset` 是一个很危险的命令，特别是加了参数 `-hard`，但是在这里我们可以这么操作

3. 撤销对文件的修改

   ```shell
   git checkout -- README.md
   ```




🤗 总结：撤销暂存文件和修改文件的命令其实不需要单独去记，因为在 git 图形化工具中一般都有



# 4 git fetch & git pull

- git fetch: 仅从远程库中拉取代码，不自动合并到本地分支
- git pull: 从远程库中拉取代码的同时合并到本地分支

> [!tip]
>
> 参考视频：https://www.bilibili.com/video/BV1Ba4y1s7uU/?spm_id_from=333.1007.top_right_bar_window_history.content.click&vd_source=1196492d1b62ce51a99239bb8005ee49



# 5 git rebase & git merge

> [!important]
>
> 先说结论：在平时工作中，我们尽可能不要使用 `git rebase`，合并代码我们最好使用 `git merge`









