# gitRepository目录是一个版本库 #：

 版本库定义：

	什么是版本库呢？版本库又名仓库，英文名repository，你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。
 
## 一 常用命令：（切记不要用中文路径） ##

  1.创建空目录：

   ①选一个合适的目录鼠标右键选择"git bash here"

   ②命令： 

    //创建gitRepository目录
    $ mkdir gitRepository
    
    //定位到gitRepository目录
    $ cd gitRepository
    
    //pwd用于显示当前目录
    $ pwd
  
  2.（仓库初始化）将创建的目录编程Git可管理的仓库命令：

    //会在gitRepository目录下生产.git目录，表示创建管理仓库成功（该文件夹下的文件不可动）
    $ git init
    
  3.添加文件到仓库（即使文件存在，也需要这两个步骤）：

   ①命令：
    
	//文件添加到仓库
    $ git add readme.txt
    
    //文件提交到仓库 -m 后的内容是本次提交的版本说明
    $git commit -m "文件新加了对git常用命令的整理"
    
   ②解释：

    为什么Git添加文件需要add，commit一共两步呢？因为commit可以一次提交很多文件，所以你可以多次add不同的文件 
  
  4.查看git结果命令（文件提交情况）：

    //git status命令可以让我们时刻掌握仓库当前的状态
    $ git status
   
  5.查看git中被修改的文件，修改了什么具体内容命令

    $ git diff readme.txt
   
  6.查看历史提交版本命令

    $ git log
  
   ②如果嫌输出信息太多，看得眼花缭乱的，可以试试加上--pretty=oneline参数

    $ git log --pretty=oneline
    
  7.版本回退命令

	 //HEAD指向的版本就是当前版本，上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100
	 $ git reset --hard HEAD^
	 
  8.还可以去某个版本命令

	//版本号没必要写全，前几位就可以了，Git会自动去找。当然也不能只写前一两位，因为Git可能会找到多个版本号，就无法确定是哪一个了。
	$ git reset --hard 8ca283d4ad0c7897f4eb9e486fd4be284c25bd84
	
  9.回退到某个版本，但是版本commit id忘了，咋办？

	//查看版本号commit id对应的提交说明（所以必须有准确的提交说明）
	$ git reflog
	
  10.

## 二 概念 ##

Git和其他版本控制系统如SVN的一个不同之处就是有暂存区的概念。


工作区（Working Directory）

      就是你在电脑里能看到的目录，比如我的\gitRepository文件夹就是一个工作区：

![](https://img-blog.csdnimg.cn/20201228152650660.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2Zvc2hlbmd0YW5n,size_16,color_FFFFFF,t_70)

版本库（Repository）

    工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库。

    Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。

![](https://img-blog.csdnimg.cn/20201228152931169.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2Zvc2hlbmd0YW5n,size_16,color_FFFFFF,t_70)

分支和`HEAD`的概念我们以后再讲。

前面讲了我们把文件往Git版本库里添加的时候，是分两步执行的：

第一步是用`git add`把文件添加进去，实际上就是把文件修改添加到暂存区；

第二步是用`git commit`提交更改，实际上就是把暂存区的所有内容提交到当前分支。

因为我们创建Git版本库时，Git自动为我们创建了唯一一个`master`分支，所以，现在，`git commit`就是往`master`分支上提交更改。