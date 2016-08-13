#Git笔记

###简介

Git是目前世界上最先进的**分布式版本控制系统**  
SVN是一个集中式版本控制系统，使用必须联网，中央服务器挂了全都干不了活

###下载安装
window下载安装 <https://git-for-windows.github.io>速度慢  
360云盘 <https://yunpan.cn/c6IxpWzcBMqNC>  访问密码 5c14
安装完成后 找到`Git->Git Bash` 
运行   
   `$ git config --global user.name "Your Name"`
   `$ git config --global user.email "email@example.com"` 

 因为Git是分布式版本控制系统，所以，每个机器都必须自报家门：你的名字和Email地址  
--global参数 表示你这台机器上所有的Git仓库都会使用这个配置


###创建版本库
    $ mkdir learngit  
    $ cd learngit/  
    $ git init   
 通过git init命令把这个目录变成Git可以管理的仓库,会在当前目录下生成一个.git隐藏文件，不可轻易改动

###把文件加入版本库
	#把文件 加入版本库 可以添加多个文件，然后提交一次
    $ git add readme.txt 
	#把文件提交到版本库 -m 提交的说明
	$ git commit -m "wrote a readme file"

  	
###仓库状态和对比文件
    $ git status 查看当前仓库的状态，哪些文件被修改
    $ git diff 查看文件修改了哪些内容  

###版本回退
    $ git log sj 查看提交的历史记录
    $ git log --pretty=oneline 每条记录只输出一条信息
  一长串的数字加字母是commit id 版本号

在git中用HEAD表示当前版本，上一个版本就是HEAD^，上上一个版本就是HEAD^^，往上100个版本HEAD~100

#######回退到上一个版本

	 `git reset --hard HEAD^`

#######如果再回退回来(后面是版本号，可以只写前面一段，git可以找到)

	 `git reset --hard 23jrrj32jr0fjds`
#######提交的命令历史    (带有版本号)  

	 `git reflog`

HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令git reset --hard commit_id 

###工作区和暂存区
#######工作区（Working Directory）
就是你在电脑里能看到的目录
#######版本库（Repository）
工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库。

Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。
#######添加文件分两步：
1. 第一步是用git add把文件添加进去，实际上就是把文件修改添加到暂存区；
2. 第二步是用git commit提交更改，实际上就是把暂存区的所有内容提交到当前分支。
　　　　　　　

你可以简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。  
如果不提交到暂存区，既修改了本地文件，然后commit 也不会把本地文件的变化提交上去， git跟踪的是修改，不是文件

###撤销修改
`$ git checkout -- file`  

撤销本地修改，使文件回到最近一次add 或者commit的状态  ，注意命令中的--如果没有--就意味是切换到另一个分支

#######这里有两种情况：  
	1. 一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；  
	2. 一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。  
	
#######如果错误的修改已经添加到暂存区了呢（add了）
	$ git reset HEAD readme.txt
既可以回退版本，也可以把暂存区的修改回退到工作区。当我们用HEAD时，表示最新的版本
分两部：第一步：git reset HEAD readme.txt 把暂存区的版本回退到最新，第二步：git checkout -- readme.txt撤销本地修改



###删除文件
    $ git rm readme.txt
    $ git commit -m "delete readme.txt"
如果手动删除了本地文件，还需要运行上面两行命令，如果是误删除，那么可以用`git checkout -- readme.txt`   

`git checkout`其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。

###远程仓库
#######创建github ssh-key  
打开git-bash  

     `$ ssh-keygen -t rsa -C "youremail@example.com" ` 
 
一路回车,如果一切顺利的话，可以在用户主目录里找到**.ssh目录**，里面有**id_rsa和id_rsa.pub**两个文件，这两个就是SSH Key的秘钥对，id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥，可以放心地告诉任何人。    
然后登陆GitHub，打开“Account settings”，“SSH Keys”页面：点“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub

1. 创建一个github 仓库 （注意要拿ssh连接的地址）
	git@github.com:lilixin/learngit.git
2. 关联本地仓库到github远程仓库
    $ git remote add origin git@github.com:lilixin/learngit.git  
远程库的名字就是origin，这是Git默认的叫法，也可以改成别的，但是origin这个名字一看就知道是远程库。
3. 把本地库的所有内容推送到远程库上  
	$ git push -u origin master   
第一次加-u参数，以后使用下面推送修改
	`$ git push  origin master` 

###从远程库克隆
	$ git clone git@github.com:lilixin/hello.git

###分支管理
#######创建并切换一个分支(相当于两条命令)
	$ git checkout -b dev
#######创建分支
	$ git branch dev
#######切换分支
	$ git checkout dev
#######合并分支(合并指定分支到当前分支)
	$ git merge dev
#######删除分支
	$ git branch -d dev
#######查看分支
	$ git branch  

###解决冲突
`git merge newbranch` 提示冲突
 修改文件重新push 然后 merge
#######查看合并冲突日志 --graph
	$ git log --graph --pretty=oneline --abbrev-commit
###分支策略
实际开发中master分支不会轻易在上面干活，每个人都有自己的分支，大家都往dev分支合并，等需要上线时，合并dev分支到master分支  
#######默认情况下Git会用Fast forward模式，但这种模式下，删除分支后，会丢掉分支信息。
所以合并分支时禁用Fast forward模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息  

	$ git merge --no-ff -m "merge with no-ff" dev

###bug分支
当你接到一个修复一个代号101的bug的任务时，很自然地，你想创建一个分支issue-101来修复它，但是，等等，当前正在dev上进行的工作还没有提交，并且当前工作还没完成，不能提交，而bug又需要修复
	$ git stash  //使用此命令把当前工作状态保存
#######此命令会把当前状态保存
然后就可以在master分支上创建一个bug分支去修复bug，合并分支
然后就可以切回到之前的dev分支
	$ git stash list  
查看之前保存的内容
#######如何恢复
	$ git stash pop  //会把内容恢复并从stash list 中删除

	$ git stash apply //只是恢复
	$ git stash drop  //把保存的内容从stash list 中删除
###强制删除未合并的分支
	$ git branch -D newbranch 

###多人协作
查看远程库信息，使用`git remote -v`；

本地新建的分支如果不推送到远程，对其他人就是不可见的；

从本地推送分支，使用`git push origin branch-name`，如果推送失败，先用`git pull`抓取远程的新提交；

在本地创建和远程分支对应的分支，使用`git checkout -b branch-name origin/branch-name`，本地和远程分支的名称最好一致；

建立本地分支和远程分支的关联，使用`git branch --set-upstream branch-name origin/branch-name`；

从远程抓取分支，使用`git pull`，如果有冲突，要先处理冲突。

###标签管理
#######创建标签
	$ git tag v1.0  //首先切换到需要打标签的分支
默认是打到最新的提交上，也就是HEAD

	$ git tag v0.9 6224937 //也可以按commit id 打标签  
	//可以给标签打上说明 -a指定标签名 -m 说明信息  
	$ git tag -a v0.1 -m "version 0.1 released" 3628164  
#######查看标签
	$ git tag 
	$ git show <tagname> //显示详细信息
###标签管理
	git push origin <tagname>可以推送一个本地标签；

	git push origin --tags可以推送全部未推送过的本地标签；

	git tag -d <tagname>可以删除一个本地标签；

	git push origin :refs/tags/<tagname>可以删除一个远程标签。

###忽略文件
忽略某些文件时，需要编写.gitignore	  
.gitignore文件本身要放到版本库里，并且可以对.gitignore做版本管理！ 如：  
		
    # Windows:
    Thumbs.db
    ehthumbs.db
    Desktop.ini
    
    # Python:
    *.py[cod]
    *.so
    *.egg
    *.egg-info
    dist
    build
    
    # My configurations:
    db.ini
    deploy_key_rsa

#######强制添加
	$ git add -f App.class//某个文件被.gitignore忽略，强制加
#######检查.gitignore哪里出了错
	$ git check-ignore

###配置别名
	$ git config --global alias.co checkout
	$ git config --global alias.ci commit
	$ git config --global alias.br branch
	$ git config --global alias.last 'log -1'//显示最近一次
 
	$ git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"

--global是对当前用户起作用，上面这样配置只会对当前仓库起作用   
每个git仓库的配置文件都在`.git/config`下面

###搭建Git服务器
[搭建Git服务器文章地址](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/00137583770360579bc4b458f044ce7afed3df579123eca000)
