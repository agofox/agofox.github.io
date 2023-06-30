---
title: Git配置及使用
date: 2023-04-26 01:04:03
type: "Git"
description: Git的安装及使用命令 

---
- [Windows 平台上安装Git](#windows-平台上安装git)
- [用户信息](#用户信息)
- [查看配置信息](#查看配置信息)
- [Git 基本操作](#git-基本操作)
  - [提交与修改](#提交与修改)
  - [提交日志](#提交日志)
  - [远程操作](#远程操作)
- [git clone](#git-clone)
- [Git 分支管理](#git-分支管理)
- [添加远程库](#添加远程库)



## Windows 平台上安装Git
安装包下载地址：https://git-scm.com/


## 用户信息
配置个人的用户名称和电子邮件地址：

```bash
$ git config --global user.name "runoob"
$ git config --global user.email test@runoob.com
```

如果用了 --global 选项，那么更改的配置文件就是位于你用户主目录下的那个，以后你所有的项目都会默认使用这里配置的用户信息。

如果要在某个特定的项目中使用其他名字或者电邮，只要去掉 --global 选项重新配置即可，新的设定保存在当前项目的 .git/config 文件里。

## 查看配置信息
要检查已有的配置信息，可以使用 git config --list 命令：
```bash
$ git config --list
```


## Git 基本操作


```bash
$ git init
$ git add .
$ git commit -m '初始化项目版本'
```
git init - 初始化仓库。

git add . - 添加文件到暂存区。

git commit - 将暂存区内容添加到仓库中。

### 提交与修改

Git 的工作就是创建和保存你的项目的快照及与之后的快照进行对比。

下表列出了有关创建与提交你的项目的快照的命令：
```
命令	说明

git add	添加文件到暂存区

git status	查看仓库当前的状态，显示有变更的文件。

git diff	比较文件的不同，即暂存区和工作区的差异。

git commit	提交暂存区到本地仓库。

git reset	回退版本。

git rm	将文件从暂存区和工作区中删除。

git mv	移动或重命名工作区文件。
```
### 提交日志

```
命令	说明
git log	查看历史提交记录
git blame <file>	以列表形式查看指定文件的历史修改记录
```

### 远程操作

```
命令	说明
git remote	远程仓库操作
git fetch	从远程获取代码库
git pull	下载远程代码并合并
git push	上传远程代码并合并
```

## git clone
我们使用 git clone 从现有 Git 仓库中拷贝项目（类似 svn checkout）。
克隆仓库的命令格式为：
```bash
git clone <repo>
```
如果我们需要克隆到指定的目录，可以使用以下命令格式：

```bash
git clone <repo> <directory>
```
参数说明：

repo:Git 仓库。

directory:本地目录。

比如，要克隆 Ruby 语言的 Git 代码仓库 Grit，可以用下面的命令：

```bash
$ git clone git://github.com/schacon/grit.git
```
## Git 分支管理
创建分支命令：

```bash
$ git branch (branchname)
```

切换分支命令:

```bash
$ git checkout (branchname)
```

当你切换分支的时候，Git 会用该分支的最后提交的快照替换你的工作目录的内容， 所以多个分支不需要多个目录。

合并分支命令:
```bash
$ git merge 
```
删除分支命令：
```bash
$ git branch -d (branchname)
```
## 添加远程库
要添加一个新的远程仓库，可以指定一个简单的名字，以便将来引用,命令格式如下：

```bash
$ git remote add [shortname] [url]
```

本例以 Github 为例作为远程仓库，如果你没有 Github 可以在官网 https://github.com/注册。

由于你的本地 Git 仓库和 GitHub 仓库之间的传输是通过SSH加密的，所以我们需要配置验证信息：

使用以下命令生成 SSH Key：

```bash
$ ssh-keygen -t rsa -C "youremail@example.com"
```

后面的 your_email@youremail.com 改为你在 Github 上注册的邮箱，之后会要求确认路径和输入密码，我们这使用默认的一路回车就行。

成功的话会在 ~/ 下生成 .ssh 文件夹，进去，打开 id_rsa.pub，复制里面的 key。
回到 github 上，进入 Account => Settings（账户配置）。

![图片显示需要能访问Github](/img-folder/git/1.jpg)

左边选择 SSH and GPG keys，然后点击 New SSH key 按钮,title 设置标题，可以随便填，粘贴在你电脑上生成的 key。

![图片显示需要能访问Github](/img-folder/git/2.jpg)

添加成功后界面如下所示

![图片显示需要能访问Github](/img-folder/git/3.jpg)

为了验证是否成功，输入以下命令：

```bash
$ ssh -T git@github.com
The authenticity of host 'github.com (52.74.223.119)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes                   # 输入 yes
Warning: Permanently added 'github.com,52.74.223.119' (RSA) to the list of known hosts.
Hi tianqixin! You've successfully authenticated, but GitHub does not provide shell access. # 成功信息
```




