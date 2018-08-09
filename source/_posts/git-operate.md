---
title: git常用操作指南
disqusId: jiayongli
date: 2018-08-09 15:15:52
tags:
- git
categories: technology
---

### **基本git命令**

- git staus 查看改动的文件列表
- git add .
- git commit -am "分支号，task主题"  放到暂存区
- git pull 防止破坏别人更新的代码
- git push 推送到远端

### 在本地创建分支，推到远程

- git branch adaptTarget
- git push origin adaptTarget
- git checkout adaptTarget
- git branch --set-upstream-to=origin/adaptTarget adaptTarget

### 创建远程分支

git push origin adaptTarget


<!--more-->

### 删除远程分支

1.git branch -r -d origin/testgit

2.git push origin :testgit

### **创建本地分支追踪远程分支，会在本地创建一个分支serverfix**

1.如果当前处于一个分支，那么你可以直接 git checkout –track origin/branchName

可以直接创建一个branchName并切换到该分支或者git checkout -b branchName origin/branchName也可以达到同样的效果。但是推荐第一种，省事。

2.如果你创建了一个空的分支，并切换到了该分支，那么你可以执行git branch –set-upstream-to=origin/branchName ，将该分支关联到远程分支

### **实用的git命令--暂存本此修改**

> git stash
>
> git stash pop

### **在工作中如果你遇见下面的问题**

1.首先你修改了一个文件，然后使用了 git add 

2.然后你想把这个文件还原为原来的状态，使用git checkout -- ，发现并不好使

这种情况，使用：

1.git reset HEAD 该文件 （回退所有内容到上一个版本，这里指这个文件）

2.然后再使用 git checkout -- 

### **git reset，revert，checkout区别**

> git reset HEAD~2 将head指针往前移动两个版本
>
> git revert HEAD~2 将head指针往前移动两个版本，并将这个版本作为一次新的提交
>
> git checkout 分支号 将HEAD指针移动到另一个分支上，也就是切换分支

### 回归代码

> git log

> git reset --hard 回归到的版本的hash值

> git push -f 强制push

### 清除cache

git rm -r –cached .

### 回到上一个提交的版本

如果git commit了，发现这次提交有问题，那么想回到上一个版本
那么首先git log,找到上一个版本的commit id，然后git reset --hard <commit id>即可

这样操作只能让代码会到当前版本，而不能把代码的头指针指向这个版本。

如果想这样做，使用

> git pull -f

> git reset --hard HEAD



### 删除远程分支和tag

```
git push origin --delete <branchName>
git push origin --delete tag <tagname>
```

### 删除本地分支

> git branch -d <branchName>

### There was a problem with the editor 'vi'

提交代码如果遇到上面的问题，可以采用下面的方式来解决

```
git config --global core.editor /usr/bin/vim

```

git 内部有两个编辑器，nano和Vim，一般默认的都是nano，给git设置一个默认的编辑器就可以了





### 新建了一个项目，突然想把这个项目发到github上面如何操作？

前提是你已经在本地和github关联好了。

首先登录到你的github创建一个项目，然后在你的本地里面执行

```
git init
git add .
git commit -m "first commit"
git remote add origin https://github.com/yonglijia/myBlog1.git
git push -u origin master

```

### 如何使新建的.gitignore生效

```
git rm -r --cached .
```

上面这个命令将会移除所有的缓存索引，然后运行再运行以下命令添加所有的文件：

```
git add .
```

### Unable to change file mode on /.git/: Operation not permitted

这种问题

```
sudo chmod 777 /.git
```

搞定

------

### remote: Permission to isolumon/ZLKit.git denied to solumon.fatal: unable to access 'https://github.com/isolumon/ZLKit.git/': The requested URL returned error: 403

解决方式：修改.git目录下的config文件 里面的url

由上面错误信息提示出的：
https://github.com/isolumon/ZLKit.git/

更改为下面的
https://isolumon@github.com/isolumon/ZLKit.git

然后输入这个github的密码就可以了

### remote origin already exists.

```
解决办法如下：
1、先输入$ git remote rm origin
2、再输入$ git remote add origin git@github.com:djqiang/gitdemo.git 就不会报错了！
```

### 每次提交都提示你输入密码

在命令行输入命令:

`git config --global credential.helper store`

☞ 这一步会在用户目录下的.gitconfig文件最后添加:

```
[credential]
     helper = store
```

现在push你的代码 (git push), 这时会让你输入用户名密码, 这一步输入的用户名密码会被记住, 下次再push代码时就不用输入用户名密码啦!

☞这一步会在用户目录下生成文件.git-credential记录用户名密码的信息.

☞ `git config --global` 命令实际上在操作用户目录下的.gitconfig文件, 我们cat一下此文件(cat .gitconfig), 其内容如下:

```
[user]
 name = alice
 email = alice@aol.com
[push]
 default = simple
[credential]
 helper = store
```

`git config --global user.email "alice@aol.com"` 操作的就是上面的email

`git config --global push.default matching` 操作的就是上面的push段中的default字段

`git config --global credential.helper store` 操作的就是上面最后一行的值

### 本地修改了许多文件，其中有些是新增的，因为开发需要这些都不要了，想要丢弃掉，可以使用如下命令：

> git checkout . #本地所有修改的。没有的提交的，都返回到原来的状态

> git stash #把所有没有提交的修改暂存到stash里面。可用git stash pop回复。

> git reset --hard HASH #返回到某个节点，不保留修改。

> git reset --soft HASH #返回到某个节点。保留修改

> git clean -df #返回到某个节点

> git clean 参数
>     -n 显示 将要 删除的 文件 和  目录
>     -f 删除 文件
>     -df 删除 文件 和 目录
> 也可以使用：

`git checkout . && git clean -xdf`

### 代码回退

默认参数 -soft,所有commit的修改都会退回到git缓冲区
参数--hard，所有commit的修改直接丢弃

```
$ git reset --hard HEAD^
```

 回退到上个版本

```
$ git reset --hard commit_id    退到/进到 指定commit_id
```

推送到远程

```
$ git push origin HEAD --force
```

### modified content, untracked content

1. 删除该目录下的.git目录，一般是隐藏状态
2. `git rm -r --cached dirname`
3. `git add dirname`
4. 重新提交就ok了
