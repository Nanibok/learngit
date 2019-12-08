# 廖雪峰Git教程学习笔记

## 一.Git简介

Git是目前世界上最先进的分布式版本控制系统

版本控制系统：自动记录跟踪每次文件的改动，作一个文件快照，形成一个版本。

分布式版本控制系统：每个人电脑里都有完整的版本库，不必联网，中央服务器主要用于交换团队的修改，如：Mercurial、Bazaar、BitKeeper、Git

集中式的版本控制系统：版本库是集中存放在中央服务器，必须联网才能工作，如：CVS、SVN

历史：linux之父为了更好的管理他自己的开源项目linux系统而自己用C语言开发的分布式版本控制系统Git

## 二.	Git安装

### 2.1	Linux上安装

1.判断是否已安装Git

```
$ git
```

2否则

Debian或Ubuntu Linux中

```
sudo apt-get install git
或
sudo apt-get install git-core（旧）
```

其他
从Git官网下载源码，解压，依次执行

```
./config
make
sudo make install
```

### 2.2	Windows上安装

1.从Git官网下载安装程序,直接安装

下载地址：https://gitee.com/git-installer/git-for-windows

2.配置

```
$ git config --global user.name "Your Name"				//名字
$ git config --global user.email "email@example.com"	 //邮箱
```

## 三.创建版本库(repository/仓库/目录)

1.选择一个合适的地方，创建一个空目录

```
$ mkdir learngit	//创建一个空文件夹（如果你使用Windows系统，为了避免遇到各种莫名其妙的问题，请确保目录名（包括父目录）不包含中文。）
$ cd learngit	//移动到新文件夹
$ pwd	//显示当前位置的路径
```

2.初始化目录，将其变成Git可以管理的仓库

```
$ git init	//初始化
$ ls -ah 	//罗列目录下的文件
```

成功之后自动创建.git文件夹，以便通过Git跟踪管理版本库

注意：

所有的版本控制系统只能跟踪文本文件的改动，比如TXT文件，网页，所有的程序代码等等；
无法跟踪图片、视频、Word等二进制文件的变化；
Windows自带记事本的文本文件开头有个0xefbbbf，可能会编译错误，导致无法跟踪；

## 四.	把文件添加到版本库

### 4.1	把文件添加到仓库

```
$ git add (file_name)	//反复多次使用，添加多个文件，把文件修改添加到暂存区
```

### 4.2	把文件提交到仓库

```
$ git commit -m "(description)"	//一次提交，把暂存区的所有内容提交到当前分支
```

## 五.	穿梭功能

### 1.基本概念

工作区（Working Directory）：存在文件的目录
版本库（Repository）：.git文件夹
暂存区（stage或index）：
分支（branch）：
master分支：Git自动创建的唯一一个主分支
commit id：版本号
HEAD：表示当前版本 HEAD^：上一个版本 HEAD^^：上上一个版本 ……  HEAD~100：上100个版本
$ git status	//查看仓库当前的状态
$ git diff file	//查看已修改未提交内容与最新版本的不同

### 2.版本回退

$ git log或$ git log --pretty=oneline	//显示从最近到最远的提交日志
$ git reset --hard HEAD^	//回退到上一个版本
$ git reset --hard 1094a	//回到1094adb...版本
$ git reflog	//查看命令日志
原理：Git在内部有个指向当前版本的HEAD指针，当你回退版本的时候，Git仅仅是把HEAD从指向该版本号并更新工作区的文件

### 3.管理修改

每次修改，如果不用git add到暂存区，那就不会加入到commit中。
git diff HEAD -- file	//查看工作区和版本库里面最新版本的区别.

### 4.撤销修改

人工修改
$ git checkout -- file	//丢弃工作区的修改
$ git reset HEAD <file>	//撤销暂存区的修改
场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。

场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD <file>，就回到了场景1，第二步按场景1操作。

场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，进行版本回退，不过前提是没有推送到远程库。

### 5.删除文件

$ rm file	//删除文件管理器的文件
$ git rm file	//从版本库中删除文件
$ git checkout -- file。//恢复错删文件

## 六.远程仓库

Github-->Git远程仓库

### 6.1	SSH加密：

1.创建SSH Key

```
$ ssh-keygen -t rsa -C "youremail@example.com"
```

2.登陆GitHub，打开“Account settings”，“SSH Keys”页面，
点“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容

### 6.2	添加远程库：

1.Github上创建repository

2.本地仓库关联github的远程库

```
$ git remote add origin git@github.com:用户名/远程仓库名.git
```

3.把本地仓库的内容推送到GitHub仓库

```
$ git push -u origin master
```

4.推送最新修改

```
$ git push origin master
```

### 6.3	克隆远程库

1.确定Github上repository的地址
2.克隆远程库到本地

```
$ git clone git@github.com:用户名/仓库名.git
```

### 七.分支管理

每次提交，Git都把它们串成一条时间线，这条时间线就是一个分支。
一开始，只有一条时间线，在Git里，这个分支叫主分支，即master分支。
HEAD严格来说不是指向提交，而是指向master，master才是指向提交的，所以，HEAD指向的就是当前分支
一开始的时候，master分支是一条线，Git用master指向最新的提交，再用HEAD指向master，就能确定当前分支，以及当前分支的提交点：
每次提交，master分支都会向前移动一步，这样，随着你不断提交，master分支的线也越来越长。
当我们创建新的分支，例如dev时，Git新建了一个指针叫dev，指向master相同的提交，再把HEAD指向dev，就表示当前分支在dev上：
不过，从现在开始，对工作区的修改和提交就是针对dev分支了，比如新提交一次后，dev指针往前移动一步，而master指针不变
假如我们在dev上的工作完成了，就可以把dev合并到master上。Git怎么合并呢？最简单的方法，就是直接把master指向dev的当前提交，就完成了合并

### 创建与合并分支

1.创建分支
$ git checkout -b dev
或
$ git branch dev	//创建分支
$ git checkout dev	//切换分支

$ git branch	//查看分支
2.修改文件后切换回master分支
$ git checkout master
3.把dev分支的工作成果合并到master分支上
$ git merge dev
4.删除dev分支
$ git branch -d dev

### 解决分支冲突

分支冲突：要合并的分支在同一提交点上有不同的修改
解决方法：把Git合并失败的文件手动编辑为我们希望的内容，再提交。
$ git status	//可查看冲突文件
$ git log --graph --pretty=oneline --abbrev-commit //查看分支合并图

### 分支管理策略

$ git merge --no-ff -m "merge with no-ff" dev 	
//强制禁用Fast forward模式，，Git就会在merge时生成一个新的commit，从分支历史上就可以看出分支信息

### 分支管理策略

通常，合并分支时，如果可能，Git会用Fast forward模式，但这种模式下，删除分支后，会丢掉分支信息。

如果要强制禁用Fast forward模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。

下面我们实战一下--no-ff方式的git merge：

首先，仍然创建并切换dev分支：

$ git checkout -b dev
Switched to a new branch 'dev'
修改readme.txt文件，并提交一个新的commit：

$ git add readme.txt 
$ git commit -m "add merge"
[dev f52c633] add merge
 1 file changed, 1 insertion(+)
现在，我们切换回master：

$ git checkout master
Switched to branch 'master'
准备合并dev分支，请注意--no-ff参数，表示禁用Fast forward：

$ git merge --no-ff -m "merge with no-ff" dev
Merge made by the 'recursive' strategy.
 readme.txt | 1 +
 1 file changed, 1 insertion(+)
因为本次合并要创建一个新的commit，所以加上-m参数，把commit描述写进去。

合并后，我们用git log看看分支历史：

$ git log --graph --pretty=oneline --abbrev-commit
*   e1e9c68 (HEAD -> master) merge with no-ff
|\  
| * f52c633 (dev) add merge
|/  
*   cf810e4 conflict fixed
...
可以看到，不使用Fast forward模式，merge后就像这样：

git-no-ff-mode

### 分支策略

master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；
干活都在dev分支上，也就是说，dev分支是不稳定的。
到某个时候，比如1.0版本发布时，再把dev分支合并到master上，在master分支发布1.0版本
团队每个人都在dev分支上干活，每个人都有自己的分支，时不时地往dev分支上合并

### BUG分支

每个bug都可以通过一个新的临时分支来修复，修复后，合并分支，然后将临时分支删除
$ git stash		//把当前工作现场“储藏”起来，等以后恢复现场后继续工作
或
$ git stash apply stash@{0}		//恢复指定的stash
$ git stash list	//查看已存放的工作现场

$ git stash apply	//恢复工作现场
$ git stash drop	//删除stash内容
或
$ git stash pop		//恢复并删除stash内容

### Feature分支

开发一个新feature，最好新建一个分支；
如果要丢弃一个没有被合并过的分支，可以通过git branch -D <name>强行删除

### 多人协作

$ git remote	//查看远程库的信息
$ git remote -v		//示更远程库详细的信息

### 推送分支

把某分支上的所有本地提交推送到远程库。
推送时，要指定本地分支，这样，Git就会把该分支推送到远程库对应的远程分支上
$ git push origin master
推送原则：
master分支是主分支，因此要时刻与远程同步；

dev分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；

bug分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；

feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。

### 抓取分支

$ git clone git@github.com:michaelliao/learngit.git	//克隆，只有master分支
$ git checkout -b dev origin/dev	//创建与远程origin的相应的分支
当其他人已推送的的最新提交和你试图推送的提交有冲突，往往推送失败
解决方法：先用git pull把最新的提交从origin/dev抓下来，然后，在本地合并，解决冲突，再推送
$ git branch --set-upstream-to=origin/dev dev	//指定本地dev分支与远程origin/dev分支的链接
$ git pull	//抓取分支

### 多人协作流程：

查看远程库信息，使用git remote -v；

本地新建的分支如果不推送到远程，对其他人就是不可见的；

从本地推送分支，使用git push origin branch-name，如果推送失败，先用git pull抓取远程的新提交；

在本地创建和远程分支对应的分支，使用git checkout -b branch-name origin/branch-name，本地和远程分支的名称最好一致；

建立本地分支和远程分支的关联，使用git branch --set-upstream branch-name origin/branch-name；

从远程抓取分支，使用git pull，如果有冲突，要先处理冲突。

### Rebase

把分叉的提交历史“整理”成一条直线，看上去更直观，但本地的分叉提交已经被修改过了
$ git rebase	//把本地未push的分叉提交历史整理成直线

## 八.标签管理

发布一个版本时，我们通常先在版本库中打一个标签（tag），
这样，就唯一确定了打标签时刻的版本。
将来无论什么时候，取某个标签的版本，就是把那个打标签的时刻的历史版本取出来。
所以，标签也是版本库的一个快照，实质上也是指向某个commit的指针，但不能移动

### 创建标签

切换到需要打标签的分支，敲命令git tag <name>就可以打一个新标签
$ git tag	//查看所有标签，按字母排序的
$ git tag v0.9 f52c633	//给指定commit id打标签
$ git tag -a v0.1 -m "version 0.1 released" 1094adb		//创建带有说明的标签
$ git show <tagname>	//查看标签信息

### 操作标签

$ git tag -d v0.1	//删除本地标签
git push origin <tagname>	//推送某个标签到远程
$ git push origin --tags	//一次性推送全部尚未推送到远程的本地标签

$ git tag -d v0.9
$ git push origin :refs/tags/v0.9	//删除远程标签