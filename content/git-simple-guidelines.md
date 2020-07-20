---
title: "Git简易指南"
date: 2020-07-20T23:04:02+08:00
draft: false
---
# Git安装

## MacOS

`MacOS`自带git命令，前提是需要安装`Xcode Command Line Tools`

```bash
git --version
```

## Linux

根据发行版的不同安装方式有差异，这里给出`debian`系的安装方式：
```bash
sudo apt install git-all
```

更多的安装方式请参考[这里!!](https://git-scm.com/download/linux)

## Windows

打开[Git - Downloading Package](https://git-scm.com/download/win) 该网址，下载适合自己系统位数的版本进行安装。


<!--more-->

# HTTP 和 SSH 方式连接远程GIT

## HTTP
该方式相交于SSH连接，安全性有所降低，但使用起来都是没有什么差距的

不方便的点是在于每次本地git和远端git进行数据来往时，都会询问要账号密码，相对于SSH验证的话会比较麻烦，但是有些第三方的git工具会帮你进行该操作，可以帮你保存账号和密码。

## SSH
初次使用会比较麻烦，需要配置openssh密钥对

### 首先创建一个自己密钥
```bash
ssh-keygen -t rsa -C "你的email地址" #高级用法自己看帮助
```
### 添加SSH KEY到服务器

以gitlab为例，`User Settings` --> `SSH Keys` -->  将`id_rsa.pub`里面的内容拷贝到key的输入框里面，title自己随便写一个(建议写自己的电脑名称，方便多台电脑记录key的使用),点击`Add key`进行添加。

然后就可以愉快的进行Git操作了。

*至于ssh config的高级用法，请自行查找资料(便于多远端git地址验证和使用)*


# Clone 克隆

第一件要做的事情就是克隆代码仓库到本地，使用的命令是
```bash
git clone http地址或者ssh地址
```

# Push 推送

如果是远端仓库为空的仓库，本地有了git仓库，要本地推送到远端，使用push指令

```bash
git push
```

>push之前需要绑定一次远端和本地的关联
>使用`git remote add`进行添加
例如：
```bash
git remote add origin xxx.git
```
其中origin可以任意取名称作为识别区分，作为一个本地仓库多个远端仓库的情况使用，默认情况夏还是建议使用`origin`

# Fetch 获取

用于获取远端分支的更新
```bash
git fetch
```
可以得到远端的分支的一些变化，以便确保知道哪些分支有改动

# Pull 拉取

该指令主要是针对当前分支 进行拉取远端最新的数据，如果远端没有更新，那么会提示`Already up to date.`
```bash
git pull
```
# Add 暂存文件

该指令是将指定文件添加到暂存区
```bash
git add 文件名
git add . #增加新增文件和更新的文件，不包括删除的文件
git add -u #只增加本身已经被add的文件(tracked),也就是被更新的文件
git add -A #增加所有变更
```

# Commit 提交

将暂存区的内容进行保存为一次提交，有点类似于保存文件并打上记号
```bash
git commit -m '提交信息' #
git commit —amend # 会把当前暂存的文件直接添加到上一次的commit中，不会增加多的commit，用于补充修改上次commit的少量内容使用，可以让commit保持干净
```

*题外话:Commit规范*

```bash
npm install commitizen -g
git cz
```

# Merge 合并
合并分支的目的是为了团队协作之间，将不同的代码分支进行合并
```bash
git merge 目标分支名 #可以是远端分支 例如 origin/dev
```
首先在当前分支下，使用该命令，会将目标分支合并到当前分支，如果有冲突的情况下，会有冲突文件的特殊标识。
git并会说明冲突部分的代码块是当前分支(current)还是输入(incomming)的内容，需要解决冲突，也就是在冲突的部分，二者选其中一个。对文件进行修改后保存，进行commit提交,并推送远端分支，完成合并

*注：如果没有冲突的情况下，直接合并会将当前的head指到两个分支里面最新的commit,并且所有的commit将会合并到一个分支，可以通过`git log`进行查看*


# Status 状态
用于查看当前工作区的状体，查看文件新增 更新 删除，该指令会将文件的变化列在命令输出的结果里面
```bash
git status
```

# Branch 分支
```bash
git branch 分支名 #会基于当前分支创建一个指定名字的分支
git branch #列出所有分支
git branch -d 分支名 #删除分支
git branch -D 分支名 #强制删除分支
```

# Checkout 签出
用于切换当前分支，以及创建分支
>还有恢复文件的功能
>如果是删除文件已经commit了，需要通过`git checkout #commitID 文件名`,这时候`git status`可以看到会将该文件列为新增文件，你需要重新创建一个commit来将他保存到git中
>如果是还没有进行提交的文件`git checkout HEAD -- 文件名` 即可。

```bash
git checkout 分支名
git checkout -b dev origin/dev # 直接基于远程dev分支 创建一个本地dev分支，并且切换到dev分支
```

*新版本git已经将checkout拆分为`switch`和`restore`,具体用法可以自行了解*
```bash
git switch 分支名 #切换分支
git switch -c 分支名 #创建一个分支并切换过去
```

# Stash 草稿
用于在进行修改后，但是却不需要进行commit，这个时候需要一个干净的工作区的情况下，例如在不提交的的时候进行合并代码等操作

注意stash的文件必须是被add到git工作区了的文件才可以(该文件要被git所控制)
```bash
git stash [save message] #message代表的是自己写的注释，以便还原该stash 建议使用工具
git stash list #查看草稿列表
git stash clear #删除所有草稿
```

# Tag 标签
版本号控制
```bash
git tag v1.0.0
git tag list #列出所有tag
git tag -d v1.0.0 #删除指定tag
git push origin --tags #推送所有tag
git push origin v1.0.0 #推送指定tag
```


# Rm 删除
```bash
git rm 文件名 #同时从工作区和索引中删除文件。即本地的文件也被删除了
git rm --cached 文件名 #从索引中删除文件。但是本地文件还存在， 只是不希望这个文件被版本控制。
git rm -r 文件夹名 # 删除文件夹
```

# Log 日志

用于查看commit提交的记录
```bash
git log
```


# Init 初始化
```bash
git init
```
该指令可以在一个空文件夹下创建一个git环境，会再当前目录下创建一个`.git`文件夹作为git的工作目录
