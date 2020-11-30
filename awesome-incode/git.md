---
description: git初始化操作以及基本命令
---

# git入门

## 初次配置本地仓库

### 1、安装git\(macOS自带不用安装\)

[打开链接安装跟操作系统匹配的git版本](https://git-scm.com/)

其中包含

`git bash`\(常用，也是下面内容中用到的工具\)

`git cmd`

`git GUI`\(macos的哪个版本听说挺好用的，但没有实践的机会-。-\)

### 2、注册GitHub账号

略、其他网站账号也可以\(如：gitee，但下文都是默认GitHub的操作\)

### 3、初始化本地配置

```bash
# 设置提交代码时的用户信息
#有--global是全局配置设置，禁止在公共编译机上使用
$ git config [--global] user.name "[name]"
$ git config [--global] user.email "[email address]"
#示例
$ git config user.name "zoulam" #非全局
$ git config --global user.name "zoulam"#全局
```

**其他命令**

```bash
# 其他命令
# 显示当前的Git配置
$ git config --list

# 编辑Git配置文件
$ git config -e [--global]
```

**简单初始化**\(推荐使用`git clone`命令\)

```bash
# 在当前目录新建一个Git代码库,会生成.git文件
$ git init

# 新建一个目录，将其初始化为Git代码库
$ git init [project-name]

# 下载一个项目和它的整个代码历史
$ git clone [url]

# 拉取最新版本的代码,即对本地代码进行更新
$ git pull
```

#### clone 分支

`git clone -b [branchName] url.git`

### 4、工作区**=&gt;**暂存区

```bash
# 添加指定文件到暂存区
$ git add [file1] [file2] ...

# 添加当前目录的所有文件到暂存区
$ git add .

# 添加commit message
$ git commit -m "[commit message]"
#示例
$ git commit -m "[本次提交初始化了Vue项目]"

#查看状态
$ git status

#撤回操作
$ git checkout [filename]#旧版git
$ git restore [filename]#新版git

#查看文件内容变化
$ git diff
```

**其他命令**

```bash
#清空控制台
$ clear

# 添加指定目录到暂存区，包括子目录
$ git add [dir]

# 添加每个变化前，都会要求确认
# 对于同一个文件的多处变化，可以实现分次提交
$ git add -p

# 删除工作区文件，并且将这次删除放入暂存区
$ git rm [file1] [file2] ...

# 停止追踪指定文件，但该文件会保留在工作区
$ git rm --cached [file]

# 改名文件，并且将这个改名放入暂存区
$ git mv [file-original] [file-renamed]
```

### 中立区

> 不少程序员是十分讨厌人使用`git commit -m "commit message"`提交commit 信息的，还有一种比较常见的方式是
>
> `git commit`进入`vim`填写多行的commit信息

### 5、太多的commit message

> 解决方案，使用rebase合并
>
> 注：提交commit message需要根据公司规范提交，也可以借鉴Angular的提交规范
>
> 策略：我的实践是master合并feature branch时用merge，但如果我在feature branch上要解决和master的冲突，会rebase origin/master 然后再push 提PR

本次示例中的git log只有**5**条，所以合并的条数最大是**\(5-1\)**条即**4**条

![](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200717181219239.png)

```bash
# 合并最近四次次commit信息,进入vim编辑器
$ git rebase -i HEAD~4
# 进入vim编辑器,编辑commit id 内的所有子commit信息(不包含他本身)
$ git rebase -i [commit id]

# 回到rebase文件编辑
$ git rebase --edit-todo

# 保存rebase文件
$ git rebase --continue
```

#### 操作前

![](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200717180606630.png)

#### 内部命令翻译

```bash
# 命令:
# p, pick <提交> = 使用提交
# r, reword <提交> = 使用提交，但修改提交说明
# e, edit <提交> = 使用提交，进入 shell 以便进行提交修补
# s, squash <提交> = 使用提交，但融合到前一个提交
# f, fixup <提交> = 类似于 "squash"，但丢弃提交说明日志
# x, exec <命令> = 使用 shell 运行命令（此行剩余部分）
# b, break = 在此处停止（使用 'git rebase --continue' 继续变基）
# d, drop <提交> = 删除提交
# l, label <label> = 为当前 HEAD 打上标记
# t, reset <label> = 重置 HEAD 到该标记
# m, merge [-C <commit> | -c <commit>] <label> [# <oneline>]
```

#### vim常用操作

```bash
#    esc进入命令模式
#    i或者insert进入编辑模式
#    :wq 或 x保存文件并退出
#    :w 保存
#    :q 退出不保存 会弹出警告但可以
#    :q! 加上感叹号就可以强制退出了

#可视块操作模式
#     ctrl+v 进入可视块操作模式
#     d 删除
#     i 进入编辑，要esc退出编辑才能看到多行操作的效果
#     u 撤回操作
#     ctrl+r 取消撤回操作

# 复制粘贴操作
#    v进入可视模式
#     y复制且退出可视模式
#    p粘贴
```

#### 操作后

**将除了最上面的的第一条的开头的pick改成s或者squash即可合并**

```bash
pick d682955 第二次提交
s b446b9f 第三次提交
s 6e2a21f 第四次提交
s 12f62e9 第五次提交
```

![](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200717181458152.png)

![](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200717181556757.png)

[更详细的rebase命令](https://git-scm.com/docs/git-rebase)

### 6、回退commit版本

**commit的版本号使用`git log`打印如下图**

![](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200717165149846.png)

```bash
#查看日志
$ git log

#恢复到指定版本
$ git reset --hard [版本号]

#不常用
#恢复到上一个版本
$ git reset --hard HEAD^

# 恢复到上上一个版本
$ git reset --hard HEAD^^
```

### 7、暂存区**=&gt;**远程仓库

#### ①创建GitHubRepository**（下面的命令在2020/10的时候发生了变化）**

>  主分支`master`变成`main`

![](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200717162134088.png)

#### ②生成SSH

```bash
#生成密钥
$ ssh-keygen -t rsa -C "[你绑定的GitHub邮箱]"
#示例
$ ssh-keygen -t rsa -C "zoulam19981205@163.com"
```

![image-20200717170642041](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200717170642041.png)

密钥文件名：`id_rsa.pub`

#### ③在github配置生成到本地的SSH并重命名

**本步骤是让GitHub仓库对用户电脑授权**

![](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200717171046493.png)

![](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200717171248711.png)

#### ④进行初始化连接

```bash
#建立连接
$ git remote add origin [仓库地址，见下图]

# 将暂存区内容提交到master分支
$ git push -u origin master
# 首次提交需要输入账号密码

# 自此之后的提交
$ git push
```

![](https://zoulam-pic-repo.oss-cn-beijing.aliyuncs.com/img/image-20200717172005282.png)

### 8、提交信息

```bash
git log --name-status #每次修改的文件列表, 显示状态
git log --name-only #每次修改的文件列表
git log --stat # 每次修改的文件列表, 及文件修改的统计
git whatchanged #每次修改的文件列表
git whatchanged --stat  #每次修改的文件列表, 及文件修改的统计
git show # 显示最后一次的文件改变的具体内容
```

## 基本概念

### 1、文件\(`.git`、`.gitignore`\)

`.git`：存储当前仓库的版本信息，使用`git init`命令生成

`.gitignore`：存放不会被同步到GitHub的文件信息，如`node_modules`、vscode配置文件`.vscode`、macos生成的`.DS_store`等，以提高文件的可读性和实际大小。

#### 配置全局`.gitginore`方式

在当前项目的`.git\info\exclude`

```bash
$ git config --global core.excludesfile ~/.gitignore
```

#### 忽略的优先级问题

在 .gitingore 文件中，每一行指定一个忽略规则，Git检查忽略规则的时候有多个来源，它的优先级如下（由高到低）： 1）从命令行中读取可用的忽略规则 

2）当前目录定义的规则 

3）父级目录定义的规则，依次递推 

4）$GIT\_DIR/info/exclude 文件中定义的规则

 5）core.excludesfile中定义的全局规则

#### `.gitignore`语法

```bash
#               表示此为注释,将被Git忽略
*.a             表示忽略所有 .a 结尾的文件
!lib.a          表示但lib.a除外
/TODO           表示仅仅忽略项目根目录下的 TODO 文件，不包括 subdir/TODO
build/          表示忽略 build/目录下的所有文件，过滤整个build文件夹；
doc/*.txt       表示会忽略doc/notes.txt但不包括 doc/server/arch.txt

bin/:           表示忽略当前路径下的bin文件夹，该文件夹下的所有内容都会被忽略，不忽略 bin 文件
/bin:           表示忽略根目录下的bin文件
/*.c:           表示忽略cat.c，不忽略 build/cat.c
debug/*.obj:    表示忽略debug/io.obj，不忽略 debug/common/io.obj和tools/debug/io.obj
**/foo:         表示忽略/foo,a/foo,a/b/foo等
a/**/b:         表示忽略a/b, a/x/b,a/x/y/b等
!/bin/run.sh    表示不忽略bin目录下的run.sh文件
*.log:          表示忽略所有 .log 文件
config.php:     表示忽略当前路径的 config.php 文件

/mtk/           表示过滤整个文件夹
*.zip           表示过滤所有.zip文件
/mtk/do.c       表示过滤某个具体文件

被过滤掉的文件就不会出现在git仓库中（gitlab或github）了，当然本地库中还有，只是push的时候不会上传。

需要注意的是，gitignore还可以指定要将哪些文件添加到版本管理中，如下：
!*.zip
!/mtk/one.txt

唯一的区别就是规则开头多了一个感叹号，Git会将满足这类规则的文件添加到版本管理中。为什么要有两种规则呢？
想象一个场景：假如我们只需要管理/mtk/目录中的one.txt文件，这个目录中的其他文件都不需要管理，那么.gitignore规则应写为：：
/mtk/*
!/mtk/one.txt

假设我们只有过滤规则，而没有添加规则，那么我们就需要把/mtk/目录下除了one.txt以外的所有文件都写出来！
注意上面的/mtk/*不能写为/mtk/，否则父目录被前面的规则排除掉了，one.txt文件虽然加了!过滤规则，也不会生效！

----------------------------------------------------------------------------------
还有一些规则如下：
fd1/*
说明：忽略目录 fd1 下的全部内容；注意，不管是根目录下的 /fd1/ 目录，还是某个子目录 /child/fd1/ 目录，都会被忽略；

/fd1/*
说明：忽略根目录下的 /fd1/ 目录的全部内容；

/*
!.gitignore
!/fw/ 
/fw/*
!/fw/bin/
!/fw/sf/
说明：忽略全部内容，但是不忽略 .gitignore 文件、根目录下的 /fw/bin/ 和 /fw/sf/ 目录；注意要先对bin/的父目录使用!规则，使其不被排除。
```

#### 关于已经被上传GitHub但需要过滤的文件

```bash
#删除所有的node_modules文件
$ git rm -r -f --cached **/node_modules/
```

`readme.md`：会被GitHub默认解析成页面内容，对项目或者当前文件下进行介绍。

### 2、工作区、暂存区、远程仓库

工作区：本地磁盘

暂存区：虚拟仓库\(提交代码是很频繁的操作，此处是暂时存储这些commit的信息再一次性上传到远程仓库\)

远程\(remote\)仓库：github上的仓库

### Status

使用`git status`获取

1. 未修改\(Origin\)
2. 已修改\(Modified\)&未追踪\(Untracked\)

   修改：删除、增加、修改代码（只要是变动）

   未追踪：文件层面上的修改

3. 已暂存\(Staged\)
4. 已提交\(Committed\)
5. 已推送\(Pushed\)

### Branch、Tag

> 两个都是重大更新之后的一个定义方式。
>
> **以Jquery为例**
>
> `Branch`放置稳定版本（大版本）
>
> `Tag`放置历史所有版本(所有版本)
>
> **协同开发**
>
> 每人拉取一个`branch`、定期merge

#### 分支操作

```bash
$ git checkout -b demo1 #在本地创建名字为demo1的分支并进入
$ git push origin demo1#在远程仓库创建demo1的分支
$ git branch --set-upstream-to=origin/demo1#让当前分支与远程分支demo1建立联系
$ git git branch -a  #查看远程和当前分支
```

#### branch命令

```bash
# 列出所有本地分支
$ git branch

# 列出所有远程分支
$ git branch -r

# 列出所有本地分支和远程分支
$ git branch -a

# 新建一个分支，但依然停留在当前分支
$ git branch [branch-name]

# 新建一个分支，并切换到该分支
$ git checkout -b [branch]

# 新建一个分支，指向指定commit
$ git branch [branch] [commit]

# 新建一个分支，与指定的远程分支建立追踪关系
$ git branch --track [branch] [remote-branch]

# 切换到指定分支，并更新工作区
$ git checkout [branch-name]

# 切换到上一个分支
$ git checkout -

# 建立追踪关系，在现有分支与指定的远程分支之间
$ git branch --set-upstream [branch] [remote-branch]

# 合并指定分支到当前分支
$ git merge [branch]

# 选择一个commit，合并进当前分支
$ git cherry-pick [commit]

# 删除分支
$ git branch -d [branch-name]

# 删除远程分支
$ git push origin --delete [branch-name]
$ git branch -dr [remote/branch]
```

#### tag命令

```bash
# 列出所有tag
$ git tag

# 新建一个tag在当前commit
$ git tag [tag]

# 新建一个tag在指定commit
$ git tag [tag] [commit]

# 删除本地tag
$ git tag -d [tag]

# 删除远程tag
$ git push origin :refs/tags/[tagName]

# 查看tag信息
$ git show [tag]

# 提交指定tag
$ git push [remote] [tag]

# 提交所有tag
$ git push [remote] --tags

# 新建一个分支，指向某个tag
$ git checkout -b [branch] [tag]
```

## 3、更新git

#### 1、使用官网安装包进行覆盖安装（略）

#### 2、window命令更新

```bash
$ git --version 查看版本
$ git update-git-for-windows
```

## 常见问题

### 1）需要每一次的`git push`都需要输入账号密码

```bash
#远程仓库提交方式版本
$ git remote -v 

 #移除原始提交方式
$ git remote rm origin

#改用ssh
$ git remote add origin [ 仓库地址]

#设置跟随master，并提交到远程仓库
$ git push -u origin master
```

### 2）`release`进程未中断

```bash
#中断rebase进程
$ git rebase --abort
```

### 3）`git push`出现rejected错误

> 该错误是拒绝本地提交到远程仓库，原因是远程仓库的内容和本地仓库的内容冲突，据说是**非法篡改**

**解决方式①**

```bash
# 确定本地的代码没有问题，强制push
$ git push -f
```

**解决方式②**

```bash
# 确定远程仓库的代码没问题，拉取最新的内容再重写本地代码后git push
# 这两个命令合起来等效git pull
$ git fetch

$ git merge
```

```bash
# 获取远程仓库的更新
$ git pull
```

### 4）关于中文乱码问题

```bash
$ git config --global core.quotepath false          # 显示 status 编码
$ git config --global gui.encoding utf-8            # 图形界面编码
$ git config --global i18n.commit.encoding utf-8    # 提交信息编码
$ git config --global i18n.logoutputencoding utf-8    # 输出 log 编码
$ export LESSCHARSET=utf-8
# 最后一条命令是因为 git log 默认使用 less 分页，所以需要 bash 对 less 命令进行 utf-8 编码
```

### 5）`ls`打印中文目录是乱码

进入`Git\mingw64\share\git\completion`目录下打开`git-completion.bash`文件在末尾添上这么一行信息

```bash
alias ls="ls --show-control-chars --color"
```

