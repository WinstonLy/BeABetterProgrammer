# 安装

首先在电脑上安装Git，
然后生成ssh key，使用命令

 `ssh-keygen -t rsa -C "your_email@youremail.com"`，your_email是你的email，默认在用户的家目录下.ssh/id_rsa.pub文件里面
最后在GitHub的Account Settings，左边选择SSH Keys，Add SSH Key,title随便填，粘贴key。

# 介绍
Git是一个开源的分布式版本控制系统，可以有效、高速地处理从很小到非常大的项目版本管理。Git 是 Linus Torvalds 为了帮助管理 Linux 内核开发而开发的一个开放源码的版本控制软件。
## Git 初始化
```
git config --global user.name "name"
git config --global user.email "email address"
```

- *Git 基本操作命令*

  > creat repository（创建版本库）：`mkdir file catalogue`  //创建一个空目录
  > `cd ./file catalogue`
  > `pwd`                   //显示当前目录
  > `git init`              //初始化，建立一个git 版本库
  
  
  
  > Git 分为工作区，暂存区，版本库，add命令将文件存到暂存区，commit命令将文件提交到版本库中
  > command（基本命令）：      
  
  > `git add file `       
  
  > `git commit -m "message of operation" ` //添加文件到仓库中
  
  > `git status`            //查看工作区状态
  
  > `git diff file`         //查看file修改内容
  
  > `git log`               //显示从远到近的提交历史记录日志，
  
  > --pretty=oneline --abbrev-commit可以查看简略信息
  
  > `git reset --hard HEAD^` //回退到上一个版本，^表示前一个版本
  
  > `cat file`              //查看file内容
  
  > `git teset --hard commit id` //恢复以common id的版本
  
  > `git reflog`            //查看历史操作操作命令
  
  > `git diff HEAD -- file` //查看当前工作区和最新版本库中的区别
  
  > `git checkout -- file`  //丢弃工作区的修改
  
  > `git reset HEAD file`   //将提交到暂存区的修改回退到工作区中
  
  > `git rm；git commit -m “message”` //从版本库中删除文件
  
  > `git pull origin master`   //从本地仓库或本地分支获取并集成(整合）
  >
  > - 如果过程中出现`please enter a commit message…`,首先按下esc退出键然后输   > 入 ：wq即可
  > - 如果出现fatal: refusing to merge unrelated histories，用一下命令解决yes
  > - `git pull origin master --allow-unrelated-histories`


  >
  > 创建一个远程仓库，将本地版本库关联到GitHub上
  >
  > - 在github上创建一个空的远程仓库
  >
  > - `git remote add origin git@github.com:WinstonLy/learngit.git`
  >
  >    //将本地的版本库与Github关联起来，origin是远程库的名字，WinstonLy是GitHub账户名，  learngit是在GitHub上创建的repo
  >    `git remote rm origin`   //删除和本地关联的GitHub远程库
  >    关联多个远程库，需要将远程库的名称改一下
  >
  > - `git push -u origin master`     //将本地的内容推送到远程，git push 命令将当前分支master推送
  >     到远程，第一次推送时，加上-u不仅执行推送还执行关联操作
  >
  > - `git push origin master`        //第一次推送成功后，只需要执行此命令就能将最新修改推送到GitHub

- *从GitHub上克隆一个版本库到本地*
  1. 在GitHub上创建一个远程仓库   //勾选Initialize this repository with a README
  2. git clone git@github.com:WinstonLy/gitskills.git  //将Github上的gitskills仓库克隆到本地
  3. 在本地查看版本库

- *分支的创建与合并*
  **基本知识**

  >1. 在版本回退里，你已经知道，每次提交，Git都把它们串成一条时间线，这条时间线就是一个分支。
  >     截止到目前，只有一条时间线，在Git里，这个分支叫主分支，即master分支。HEAD严格来说不是
  >     指向提交，而是指向master，master才是指向提交的，所以，HEAD指向的就是当前分支。
  >2. 一开始的时候，master分支是一条线，Git用master指向最新的提交，再用HEAD指向master，就能
  >     确定当前分支，以及当前分支的提交点：
  >3. 每次提交，master分支都会向前移动一步，这样，随着你不断提交，master分支的线也越来越长。
  >4. 当我们创建新的分支，例如dev时，Git新建了一个指针叫dev，指向master相同的提交，再把HEAD
  >     	指向dev，就表示当前分支在dev上：								 
  >5. 不过，从现在开始，对工作区的修改和提交就是针对dev分支了，比如新提交一次后，dev指针往
  >     前移动一步，而master指针不变

  **操作**

  >1. `git checkout -b dev`	 //创建dev分支，-b表示创建并切换
  >     =`git branch dev`        //创建一个新的dev分支
  >      `git checkout dev`      //切换到dev分支
  >2. `git branch `             //查看当前分支，会列出所有分支，当前分支前面会标一个*号
  >3. 然后可在dev分支上进行一系列添加提交等操作
  >4. `git checkout master`    //dev工作完成后，切换回master分支	
  >5. `git merge dev`		     //将dev分支的工作成果合并到master分支上
  >6. `git branch -d dev`      //删除dev分支，删除后可用git branch查看分支，只剩下master分支

  **解决冲突**

  >1. 当同时在dev分支和master分支上做修改时，在合并时就会产生冲突，无法快速合并，需要手动修改
  >
  >2. 利用git status查看冲突文件，查看文件内容，Git用<<<<<<<，=======，>>>>>>>标记出不同分支的内容
  >
  >3. 手动修改冲突的文件内容后在进行提交，用git log --graph可以看到分支的合并情况
  >
  >4. 删除其中的侧分支（dev）
  >
  >

  **分支管理**

  >1. 通常，合并分支时，如果可能，Git会用Fast forward模式，但这种模式下，删除分支后，会丢掉分支信息。
  >
  >2. 如果要强制禁用Fast forward模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就
  >     可以看出分支信息。
  >
  >3. `git merge --no-ff -m "merge with no-ff" dev `    //禁用Fast forward

  在实际开发中，我们应该按照几个基本原则进行分支管理：
  首先，master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；
  干活都在dev分支上，也就是说，dev分支是不稳定的，到某个时候，比如1.0版本
  发布时，再把dev分支合并到master上，在master分支发布1.0版本；
  你和你的小伙伴们每个人都在dev分支上干活，每个人都有自己的分支，时不时地往dev分支上合并就可以了
  **BUG分支**
  遇到bug的时候创建另一个临时分支来处理bug，处理完之后将两个分支合并。

  >1. `git stash`     //把当前工作现场储藏起来，git status可查看工作区
  >
  >2. 找到需要从哪个分支上修复bug，在这个分支上创建临时分支
  >     `git checkout master，git checkout -b issue-101`
  >
  >3. 进行bug修复
  >
  >4. 修复完成之后回到master分支进行合并，删除issue-101分支
  >
  >5. 恢复工作现场
  >     `git stash list`  //查看工作现场存放位置
  >     `git stash apply` //恢复工作现场   git stash drop来删除stash内容
  >     =`git stash pop`  //回复的同时也将stash删除了

开发一个新的feature，建立一个新的分支，丢弃一个没有合并过的分支，
用`git branch -D name`强行删除

# 多人协作

`git remote`              //查看远程库的信息
`git remote -v `          //显示更详细的信息
`git push origin master`  //推送相应分支，master表示要推送的分支名，
                         不同分支推送情况不同，master，dev等分支要时刻更新

## 工作模式

- 从远程库克隆，并在本地创建和远程相对应的分支，使用`git checkout -b branch-name origin/branch-name`

- 进行相关修改，并将分支时不时push到远程，使用`git push origin branch-name`推送自己的修改

- 如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并

- 如果合并有冲突，则解决冲突，并在本地提交

- 没有冲突或者解决掉冲突后，再用`git push origin branch-name`推送就能成功
  注意：如果`git pull`提示`no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream-to branch-name origin/branch-name`

`git rebase`操作可以把本地未push的分叉提交历史整理成直线
`git rebase`的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比

## 标签

tag（标签）本质上和commit id骑着同样的作用，只是便于观看和查询
**打标签**

1. 切换到要打标签的分支上
             git branch，git checkout master

2. git tag v1.0      //给该分支打上标签

3. git tag           //查看所有标签
      git tag v0.9 commit id   //给历史提交的某个commit id打上标签

4. git show tagname         //查看标签信息  

5. git tag -a tagname -m "message" commit id   //给某个id打标签并添加说明

6. git tag -d tagname       //删除某个标签
      git push origin ：refs/tags/tagname   //与上面的合起来一起删除远程库的标签

7. git push origin tagname  //将某个标签推送到远程
      git push origin -tags    //一次性全部推送尚未推送到远程的本地标签

忽略某些文件时，需要编写.gitignore；
.gitignore文件本身要放到版本库里，并且可以对.gitignore做版本管理
`git check-ignore -v file`     //查看忽略该文件的规则

配置别名：`git config--global alias。st status`     //st=status
          配置Git的时候，加上--global是针对当前用户起作用的，如果不加，那只针对当前的仓库起作用
		  每个仓库的Git配置文件都放在.git/config文件中
		  每个仓库的Git配置文件都放在.git/config文件中，配置别名可以直接改这个文件
		  
