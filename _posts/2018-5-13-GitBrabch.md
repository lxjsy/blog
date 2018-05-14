---
layout: post
title: "Git分支与多人协作"
date: 2018-5-13 9:10:12 
description: "Git分支管理"
tag: Git
---

本章记录在[廖雪峰老师的Git教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)的指导下Windows中Git的使用！

### 分支

#### Git分支
在Git中，可以快速的创建、删除和切换分支，这也是Git版本控制器的优势。
通过分支，我们可以开展独立的工作，不影响其他分支的工作。
当完成开发之后，可以一次性把分支合并到原来的分支上，确保项目的安全。

### Git分支结构
Git会将每一次提交串成一条时间线，这条时间线就是一个分支。

在初始化Git仓库的时候，默认创建一个名为`master`的主分支，`master`指向分支的最新提交。当每一次提交，`master`分支都会向前移动一步，这样，随着你的不断提交，`master`分支的线也越来与越长。其他的分支也是类似的工作。
`HEAD`指向的时当前工作分支中的当前版本库，结合[Git版本库创建和版本修—版本回退](http://luxijia.top/2018/05/GitLearning)综合理解。

通过命令`git branch`查看Git中的所有分支。

### 创建分支
* 创建分支，使用命令`git branch <name>`
* 创建并切换分支，使用命令`git checkout -b <name>`

当创建新的分支时，Git创建一个与新分支同名的指针，指向`master`相同的提交，再把`HEAD`指向新分支的指针，表示在新分支上。可以看到除了新增加一个指针，和修改`HEAD`指针指向，工作区的文件没有发生任何变化。

### 分支修改与冲突解决
* 切换分支，使用命令`git checkout <name>`
* 合并某分支到当前分支，使用命令`git merge <name> `
* 删除分支，使用命令`git branch -d <name> `

在切换分支进行工作后进行融合时，会有出现两种情况：
> - 分支修改某文件后进行提交，切回到主分支直接进行分支合并，然后删除分支
> - 分支修改某文件后进行提交，切换到祖主分支后对同一文件进行修改后提交，再与分支进行合并，然后删除分支

第一种情况是`Fast-forward`类型，即快进模式，分支合并会非常顺利，Git直接把`master`指针指向新分支的指针，工作区内容不变，但这种方式并不好，一般使用[分支管理策略](#分支管理策略)。

![Fastforward](/images/git/fastforward.png)

第二种情况会出现合并冲突，Git无法进行快速合并，因此需要对冲突进行解决，再提交和合并分支。

#### 冲突解决
当出现上述第二种情况，在使用分支合并命令进行分支合并的时候，Git会提示我们冲突的地方，也可以使用`git status`可以看到冲突的文件。
查看冲突的文件，Git通过`<<<<<<<<`,`=======`,`>>>>>>>>`标记出不同分支的内容。我们将冲突的文件修改后，再添加、提交。通过带参数的`git long`查看分支合并情况。
```
$ git log --graph --pretty=oneline --abbrev-commit 
```

![collision](/images/git/collision.png)

### 分支管理策略
在进行分支合并市，Git在可能的情况下使用`Fast forward`模式，但是这种模式会导致在删除分支后，分支信息会丢失。

使用命令`git merge --merge --no-ff`来强制禁用`Fast forward`模式，Git就会在meger时生成一个新的commit，因此需要参数`-m`添加描述。这时可以通过命令`git log`查看分支历史。

![no-ff](/images/git/no-ff.png)

#### 原则
* `master`分支应该非常稳定仅用来发布新版本，平时不能在上面工作
* 在其他分支上进行工作，当开发到一定阶段，将分支与主分支进行合并，将阶段性版本进行发布

团队合作的工作分支

![teamwork](/images/git/teamwork.png)

#### Bug分支
在修复Bug时，通常会通过创建新的bug分支进行修复，然后合并，最后删除。
>>
当工作未完成时，可以先将工作现场储存起来，当继续工作时，在恢复工作现场。
* 存储当前工作现场，使用命令`git stash`
* 查看存储的工作现场，使用命令`git stash list`
* 恢复现场，使用命令`git stash apply`或者`git stash pop`

在恢复工作现场时，可以使用两类指令。前者恢复工作现场，但不删除`stash`中的内容，需要通过命令`git stash drop`来删除，后者在恢复工作现场的同时把`stash`内容也删除了。

#### Featrue分支
软件开发中，总有新的功能不断被添加，因此开发一个新的featrue，新建一个分支。

如果要丢弃一个没有被合并过的分支，可以通过命令`git branch -D <name>`强行删除。

#### 多人协作
下面时多人协作中的信息查看和仓库配置
* 查看远程库信息，使用命令`git remote -v`
* 克隆远程仓库，使用命令`git clone <ssh/https url>`进行克隆
* 从远程仓库进行克隆时，默认只有`master`分支，需要使用命令`git checkout -b <branch-name> origin/<branch-name>`在本地创建和远程分支对应的分支。

多人协作的工作模式
> 1. 工作提交，通过命令`git push origin <branch-name>`推送自己的修改
> 2. 如果推送失败，则因为远程分支比本地库更新，需要用`git pull`试图合并
> 3. 如果合并有冲突，则解决冲突，并在本地提交
> 4. 没有冲突或者解决冲突，再执行步骤1

如果`git pull`提示“no tracking information”，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream branch-name origin/branch-name`。


