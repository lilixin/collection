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

######回退到上一个版本

	 `git reset --hard HEAD^`

######如果再回退回来(后面是版本号，可以只写前面一段，git可以找到)

	 `git reset --hard 23jrrj32jr0fjds`
######提交的命令历史    (带有版本号)  

	 `git reflog`

HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令git reset --hard commit_id 

###工作区和暂存区
######工作区（Working Directory）
就是你在电脑里能看到的目录
######版本库（Repository）
工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库。

Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。
######添加文件分两步：
1. 第一步是用git add把文件添加进去，实际上就是把文件修改添加到暂存区；
2. 第二步是用git commit提交更改，实际上就是把暂存区的所有内容提交到当前分支。
　　　　　　　

你可以简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。

