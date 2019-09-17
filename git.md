### 1.	什么是git
##### 定义
> 用C语言开发的分布式版本控制系统

##### 特点
> 跟踪并管理的是修改操作，而非文件

##### 使用场景
> 我们在写论文的时候，写完了提交给指导老师去审核，老师说你哪哪哪写的不好要改改，于是乎，你就屁颠屁颠的去改。一般的做法是：不直接在原论文上修改，而是复制一份，在新复制的那一份上做改动。因为如果改了论文，被老师评价说还不如上一次，让你重该，你还可以找到上一次的论文，但是如果不复制，直接在原来的论文上改动

### 2.	Git仓库工作分区
分区包括：工作区+暂存区+分支
 

### 3.	创建一个空的git库
- s1：创建一个空目录

```
  mkdir 目录名
```
  
- s2：进入目录	

```
  cd 目录名
```

- s3：初始化该目录为一个空git库，也可以初始化一个非空的目录

```
  git init
```


### 4.	将文件添加到git仓库
s1： 添加到暂存区（没有消息就是好消息，可以多个git add 然后一个git commit）

```
   git add 文件名
```

s2:  提交到分支 
```
   git commit -m "-m一定要写，这里写注释" 
```

s3:  更新到远程库

```
   git push
```



### 5.	显示文件修改日志 -> 将当前版本回退
##### (1)	显示

```
  git status（查看本地库状态）
  git log（查看提交历史，以便确定要回退到哪个版本）
  git log --pretty=oneline（简约）
  git reflog （查看命令历史，以便确定要回到未来的哪个版本）
```

##### （2）回退
> commit操作都有一个commit id（版本号），它是一个SHA1计算出来的一个非常大的数字，用十六进制表示，一次commit代表一个版本 


```
  git reset --hard HEAD^ （--不是注释，是语法～）
  git reset --hard 1094a （commit id 的随便前几位，唯一性）

  HEAD ：表示当前版本
  HEAD^ ：表示上一个版本
  HEAD^^：上上一个版本
  HEAD~100：往上100个版本
```


### 6.	查看工作区和版本库里面最新版本的区别

```
  git diff HEAD -- 文件名 （--不是注释）
```


### 7.	撤销修改
- ##### 场景1
###### 当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。
- ##### 场景2
###### 当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD <file>，就回到了场景1，第二步按场景1操作。
- ##### 场景3
###### 已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。

### 8.	删除文件
S1：

```
  rm 文件名
```

S2：删除版本库中

```
  git rm 文件名
  git commit -m "remove 文件～～"
```
误删：因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：

```
git checkout -- 误删的文件名
```

工作库：从来没有被添加到版本库就被删除的文件，是无法恢复的

### 9.	与远程库的一些操作

```
  git clone （从远程库中克隆）
  git pull （更新成和远程一致）
  git push （add, commit后推到远程库）
  git remote	（查看远程库的信息）
  git remote -v	（查看远程库的信息，更详细）
```


### 10.	分支
##### (1)	创建分支

```
  git branch dev
```

##### (2)	切换分支

```
  git checkout dev
```

##### 创建+切换


```
  git checkout -b dev
```


##### (3)	查看当前分支

```
  git branch
```

##### (4)	分支合并
当前分支合并另外一个分支

```
	git merge dev（master分支下合并dev分支，这种模式下，删除分支后，会丢掉分支信息）
	git merge --no-ff -m "merge with no-ff" dev（禁用Fast forward模式，
	Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。）
```

##### (5)	删除某个分支（删除dev分支）

```
git branch -d dev
```

##### (6)	看到分支合并图

```
git log --graph
```

##### (7)	bug分支
一般修复bug需要停止当前工作，可能会用到以下命令

```
git stash       把当前工作现场“储藏”起来，等以后恢复现场后继续工作
git stash list  查看被储藏的工作区，存放在stash中
git stash apply 恢复被储藏的工作区，但是stash内容并不删除，需要用git stash drop来删除
git stash pop   恢复的同时把stash内容也删
```

##### (8)	feature分支
一般添加新功能的时候会创建一个新的分支
丢弃一个没有被合并过的分支

```
git branch -D <name>（强行删除）
```

##### (9)	多人协作的工作模式通常是这样
- 	首先，可以试图用git push origin <branch-name>推送自己的修改；
- 	如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；
- 	如果合并有冲突，则解决冲突，并在本地提交；
- 	没有冲突或者解决掉冲突后，再用git push origin <branch-name>推送就能成功！
- 	如果git pull提示no tracking information，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream-to <branch-name> origin/<branch-name>。
##### (10)	 给分支创建标签
- S1.  首先，切换到需要打标签的分支
- S2.  打一个新标签

```
 git tag <name>：打一个新标签
（git tag v1.0）
（git tag v0.9 f52c633 。 给当前分支对应的某个commit ID 打标签）
```
 

- 查看所有标签，标签不是按时间顺序列出，而是按字母排序的

```
git tag
```
- 查看标签信息

```
git show tagname
```
- 删除某一标签（因为创建的标签都只存储在本地，不会自动推送到远程）

```
git tag -d v0.1 
```
- 推送某个标签到远程（origin表示远程库）

```
git push origin <tagname>
```
- 一次性推送全部尚未推送到远程的本地标签

```
git push origin –tags
```

- 删除推送到远程的标签，先从本地删除git tag -d v0.9；然后，从远程删除git push origin :refs/tags/v0.9


