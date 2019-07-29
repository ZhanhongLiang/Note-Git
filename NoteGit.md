- Git前言

- Git使用技巧

- Git原理理解

- 自定义Git库（用树莓派来作为服务器进行Git管理）

- 作业

- 问题

- 参照

- 文献资料文档

  --------

  

  # 1.Git前言

  首先我是刚刚接触Git的，之前没有了解过有关SVN和CVS，但是知道它是集中式的管理库，后来linus自己用了两周写出了Git，emmmmm，大佬果然是大佬，之前那段时间可能懒得写，所以才不写，我也懒得说linux和SVN之间的爱恨情仇了，只要记住，Git是分布式的文件管理系统，每台电脑都可以当服务器进行管理，只要有别人的公钥就行了，废话多说，下面来介绍Git的使用技巧（**仅是入门水平，希望指出错误**）！！！！

#   2.Git使用技巧

（**1）1.Git的使用思路与步骤**

STEP1- 创建库（Created）

STEP2- 提交文档（Commit)

（STEP2.1-退回上次修改/重回未来）

（STEP2.2-删除）

**（STEP2.3-创建分支（master分支、dev分支、feature分支）和修改冲突）**

STEP3-推送关联Github或者Gitee（remote)

（STEP3.1-拉取Github的项目文档）(pull)

STEP4-推送文档(push)

（STEP4.1-推送tags版本号，与commit挂钩）(tag)



**STEP1-创建库**

打开了Gitbash，顾名思义，分布式管理库也就是人手一台电脑就是库，所以我们要创建一个空库，方便我们管理自己的文件那么这个时候我们就要创建一个目录了，`mkdir <file>`这个命令（emmm，Git本来就是linus开发的，别问我为什么和Linux系统的一样）

第一步当然就是用`git init`

```
$ mkdir learngit
$ cd learngit
$ pwd
/Users/michael/learngit
```

然后就将当前的目录作为库的目录

```
$ git init
Initialized empty Git repository in /Users/michael/learngit/.git/
```



**STEP2- 提交文档（Commit)**

然后用Vi或者nano编写代码或者文档，或者自己在外面编写，**注意一定要放在当前目录下！！！**

然后添加和提交(-m是提交说明)

```
$ git add <file>
$ $ git commit -m "message"
[master (root-commit) eaadf4e] wrote a readme file
 1 file changed, 2 insertions(+)
 create mode 100644 readme.txt
```



**STEP2.1-退回上次修改**/**重回未来**

首先设想之前修改了几次代码，或者在几个地方修改了几次代码，但是发现要找回之前的代码，我们应该怎么做？？？

好了，按下面操作。。。。

```
$ git log
commit 1094adb7b9b3807259d8cb349e7df1d4d6477073 (HEAD -> master)
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:06:15 2018 +0800

    append GPL

commit e475afc93c209a690c39c13a46716e8fa000c366
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:03:36 2018 +0800

    add distributed

commit eaadf4e385e865d25c48e7ca9c8395c3f7dfaef0
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 20:59:18 2018 +0800

    wrote a readme file
```

看到了一堆文字，这个是关于你之前的提交的东西的记录。。其中最上面的是最近一次提交的，HEAD是当前的版本所在。。。

```
$ git reset --hard HEAD^
HEAD is now at e475afc add distributed
```

好了我们穿越到了之前的一个版本，现在我们要找跟早之前的版本，应该怎么做呢？？？？？？

```
$ git log

```

找到相应的commit id用

```
$ git reset --hard <commit id>
HEAD is now at 83b0afe append GPL
```

凡事都有后悔，你第二天想后悔了，那么就用

```
$ git reflog
```

好了重返未来了，你看到了之前的版本，那么就可以找到相应id，像上面一样重回未来。。。。。



**STEP2.2-删除**

你有两个选择，一是确实要从版本库中删除该文件，那就用命令`git rm`删掉，并且`git commit`：

```
$ git rm test.txt
rm 'test.txt'

$ git commit -m "remove test.txt"
[master d46f35e] remove test.txt
 1 file changed, 1 deletion(-)
 delete mode 100644 test.txt
```

另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：

```
$ git checkout -- test.txt
```

`git checkout`其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。

**注意：从来没有被添加到版本库就被删除的文件，是无法恢复的！**



**STEP2.3-创建分支（master分支、dev分支、feature分支）和修改冲突**

首先，我们创建`dev`分支，然后切换到`dev`分支：

```
$ git checkout -b dev
Switched to a new branch 'dev'
```

然后，用`git branch`命令查看当前分支：

```
$ git branch
* dev
  master
```

然后我们修改了dev分支里面的某个文件，添加并提交！

```
$ git add readme.txt 
$ git commit -m "branch test"
[dev b17d20e] branch test
 1 file changed, 1 insertion(+)
```

现在，`dev`分支的工作完成，我们就可以切换回`master`分支：

```
$ git checkout master
Switched to branch 'master'
```

**合并分支！！！！**

```
$ git merge --no-ff dev
Updating d46f35e..b17d20e
Fast-forward
 readme.txt | 1 +
 1 file changed, 1 insertion(+)
```

删除分支！！！

```
$ git branch -d dev
Deleted branch dev (was b17d20e).
```

再次查看分支，你会发现只剩下master分支了！！

```
$ git branch
* master
```

在这里我们也许会有疑问，为什么不能在master上面修改并且提交呢？？**答案是不能**，因为这会产生冲突，这个时候我们要手动修改。。。。。。

```
$ git status
On branch master
Your branch is ahead of 'origin/master' by 2 commits.
  (use "git push" to publish your local commits)

You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)

	both modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

产生了冲突！！！我们要手动将master的修改了，才能进行合并。。。



![](https://static.liaoxuefeng.com/files/attachments/919023260793600/0)

这个时候我们看图里，这个是现代公司一般都采用的模式，master是主要分支，dev是提交分支,其余的都是我们自己的工作分支，平时都是提交到dev分支里面，最总才汇聚成master分支（最总版本）

还有一个就是bug修复的时候，我们应该怎么做呢？？这个时候不能提交当前的工作，但是急需去修复bug，那么只能扔下手头工作去修复bug。按下面操作！！！！





Git还提供了一个`stash`功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作：

```
$ git stash
Saved working directory and index state WIP on dev: f52c633 add merge
```

首先确定要在哪个分支上修复bug，假定需要在`master`分支上修复，就从`master`创建临时分支：

```
$ git checkout master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 6 commits.
  (use "git push" to publish your local commits)

$ git checkout -b issue-101
Switched to a new branch 'issue-101'
```

然后按照之前的步骤！！！！

```
$ git checkout dev
Switched to branch 'dev'

$ git status
On branch dev
nothing to commit, working tree clean
```

工作区是干净的，刚才的工作现场存到哪去了？用`git stash list`命令看看：

```
$ git stash list
stash@{0}: WIP on dev: f52c633 add merge
```

用`git stash pop`，恢复的同时把stash内容也删了：

```
$ git stash pop
On branch dev
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	new file:   hello.py

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt

Dropped refs/stash@{0} (5d677e2ee266f39ea296182fb2354265b91b3b2a)
```

假如我们要强行删除没有合并的分支！！

现在我们强行删除：

```
$ git branch -D feature-vulcan
Deleted branch feature-vulcan (was 287773e).
```



**STEP3-推送关联Github或者Gitee（remote)**

远程库的默认名字是origin，但是通常就是github和gitee这两个，我们可以这样关联，

```
git remote add github git@github.com:ZhanhongLinag/Opencv.git
git remote add gitee git@github.com:chinwongleung/Opencv.git
```

- 查看远程库信息，使用`git remote -v`；
- 本地新建的分支如果不推送到远程，对其他人就是不可见的；
- 从本地推送分支，使用`git push origin branch-name`，如果推送失败，先用`git pull`抓取远程的新提交；
- 在本地创建和远程分支对应的分支，使用`git checkout -b branch-name origin/branch-name`，本地和远程分支的名称最好一致；
- 建立本地分支和远程分支的关联，使用`git branch --set-upstream branch-name origin/branch-name`；
- 从远程抓取分支，使用`git pull`，如果有冲突，要先处理冲突。

**STEP4-推送文档(push)**

推送分支，就是把该分支上的所有本地提交推送到远程库。推送时，要指定本地分支，这样，Git就会把该分支推送到远程库对应的远程分支上：

```
$ git push origin master
```

如果要推送其他分支，比如`dev`，就改成：

```
$ git push origin dev
```

但是，并不是一定要把本地分支往远程推送，那么，哪些分支需要推送，哪些不需要呢？

- `master`分支是主分支，因此要时刻与远程同步；
- `dev`分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；
- bug分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；
- feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。

总之，就是在Git中，分支完全可以在本地自己藏着玩，是否推送，视你的心情而定

**STEP4.1-推送tags版本号，与commit挂钩**

找到历史提交的commit id，然后打上就可以了：

```
$ git log --pretty=oneline --abbrev-commit
12a631b (HEAD -> master, tag: v1.0, origin/master) merged bug fix 101
4c805e2 fix bug 101
e1e9c68 merge with no-ff
f52c633 add merge
cf810e4 conflict fixed
5dc6824 & simple
14096d0 AND simple
b17d20e branch test
d46f35e remove test.txt
b84166e add test.txt
519219b git tracks changes
e43a48b understand how stage works
1094adb append GPL
e475afc add distributed
eaadf4e wrote a readme file
```

比方说要对`add merge`这次提交打标签，它对应的commit id是`f52c633`，敲入命令：

```
$ git tag -a v0.9 -m 'message' <commit id>
```

再用命令`git tag`查看标签：

```
$ git tag
v0.9
v1.0
```

**注意：标签总是和某个commit挂钩。如果这个commit既出现在master分支，又出现在dev分支，那么在这两个分支上都可以看到这个标签。**

好了到现在已经入门了Git了，我们可以进行相应的项目推送了！！！！

# 3.Git原理理解

**3.1-工作区和版本区**

就是你在电脑里能看到的目录，比如我的`learngit`文件夹就是一个工作区：

工作区：

![](https://raw.githubusercontent.com/ZhanhongLiang/Note-Git/master/1.png)



工作区有一个隐藏目录`.git`，这个不算工作区，而是Git的版本库。

Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD`。

版本区：

![](https://static.liaoxuefeng.com/files/attachments/919020037470528/0)

前面讲了我们把文件往Git版本库里添加的时候，是分两步执行的：

第一步是用`git add`把文件添加进去，实际上就是把文件修改添加到暂存区；

第二步是用`git commit`提交更改，实际上就是把暂存区的所有内容提交到当前分支。

因为我们创建Git版本库时，Git自动为我们创建了唯一一个`master`分支，所以，现在，`git commit`就是往`master`分支上提交更改。

你可以简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。



# 4.自定义Git库（用树莓派来作为服务器进行Git管理）

待续更新！！！！！



# 5.作业

**作业：自己创立一个github和gitee账号，并且尝试推送练习，最后用截图提交上来。**

**提交方式：自己上传我的github里面，github账号：https://github.com/ZhanhongLiang/Homework**

# 6.问题

Windows环境：

打开

> C:\Windows\System32\drivers\etc\hosts     
>



> 添加一行：13.229.188.59　　github.com
> 测试

这里写图片描述

![](https://raw.githubusercontent.com/ZhanhongLiang/Note-Git/master/2.png)

可以看见它又变了，连接到另一个服务器上了。
但是不慌，将这个IP也加上去就行了。

# 7.参照

廖雪峰的Git文档。

https://blog.csdn.net/lvsehaiyang1993/article/details/80881433



# 8.文献资料文档

https://github.com/ZhanhongLiang/Note-Git/blob/master/git-cheatsheet.pdf