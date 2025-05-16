# 1 npm ERR! code 128

**问题描述**

```
npm ERR! code 128
npm ERR! An unknown git error occurred
npm ERR! command git --no-replace-objects ls-remote ssh://git@github.com/nhn/raphael.git
npm ERR! ssh: connect to host github.com port 22: Connection refused
npm ERR! fatal: Could not read from remote repository.
npm ERR!
npm ERR! Please make sure you have the correct access rights
npm ERR! and the repository exists.
```

![image-20221206142308184](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221206142308184.png)



**出现错误原因**

我们需要在 github 上设置 ssh 秘钥，否则对于某些文件是没有权限获取



**解决方法**

1. 首先需要在 git 中设置一下邮箱，接着输入以下命令来生成 ssh 秘钥

   ```bash
   ssh-keygen -t rsa -C "xxx@xx.com"
   ```

2. 查看生成的秘钥

   ```bash
   cat ~/.ssh/id_rsa.pub
   ```

3. 添加秘钥到 github 中

   ![image-20221206142755999](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20221206142755999.png)



如果还是不行，那么可能就自己网络的问题，可以使用下面的工具来加速访问：[DecSideCar 传送门](https://github.com/docmirror/dev-sidecar/releases)





# 2 vscode 使用内置 commit 速度慢

vscode 中 git commit ，**需要我们在左上角的输入框中输入 git 对应的版本号**（使用 git -v 查看版本），例如输入 2.40.0 后再进行 commit，就能很快提交



# 3 TLS certificate verification has been disabled!

该错误其实无足轻重，翻译过来大致就是 "TLS 安全验证已被禁用"，但这并不影响我们 git 的使用，如果去除该警告，我们手动将其开启即可

```bash
git config --global http.sslVerify true
```

:warning:**注意**

当我们开启安全验证后，很有可能出现拉别人的代码的拉不下来的情况，所以，安全验证最好还是关闭：

```bash
git config --global http.sslVerify false
```



# 4 vscode 中配置 git

在 IDEA 中，我们不需要自己手动配置 git，因为 IDEA 工具会自动帮助我们检索，但是如果是在 vscode 中，我们需要自己手动进行配置 git，步骤也很简单，直接在选项中搜索 git.path ，然后配置 setting.json 即可

![image-20230409215953920](https://theblogimage.oss-cn-fuzhou.aliyuncs.com/imagefortypora/image-20230409215953920.png)

> 注意：git.exe 是 cmd 目录下的 git.exe

