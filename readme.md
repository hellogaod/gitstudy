#  git 仓库学习总结  #

**学习内容全部来自：[廖雪峰git教程](https://www.liaoxuefeng.com/wiki/896043488029600)，这里更新了新的内容并且做了个人总结**

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
	
  10.撤销修改命令(在工作区的和暂缓区的区别)

	//这里有两种情况：

	//一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；

	//一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。
	
	$ git checkout -- readme.txt
	
	//用命令git reset HEAD <file>可以把暂存区的修改撤销掉（unstage），重新放回工作区
	$ git reset HEAD readme.txt
	Unstaged changes after reset:
	M	readme.txt

 ` git checkout -- file`命令中的--很重要，没有--，就变成了“切换到另一个分支”的命令，我们在后面的分支管理中会再次遇到`git checkout`命令。

 ` git reset`命令既可以回退版本，也可以把暂存区的修改回退到工作区。当我们用`HEAD`时，表示最新的版本。


  场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。

  场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD <file>，就回到了场景1，第二步按场景1操作。

  场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。

 11.删除文件

  一般情况下，你通常直接在文件管理器中把没用的文件删了，或者用rm命令删了：

	$ rm readme.txt

  现在你有两个选择，一是确实要从版本库中删除该文件，那就用命令git rm删掉，并且git commit：

	$ git rm readme.txt
	rm 'readme.txt'

	$ git commit -m "remove readme.txt"
	[master d46f35e] remove readme.txt
	 1 file changed, 1 deletion(-)
	 delete mode 100644 readme.txt

  现在，文件就从版本库中被删除了。

  另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：

	$ git checkout -- readme.txt

  `git checkout`其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。

12.本地git仓库推送到github远程仓库(还有相关配置如下有介绍)

  (1)要关联一个远程库，使用命令

	$ git remote add origin git@github.com:hellogaod/gitstudy.git

  (2)关联后，使用命令`git push -u origin master`第一次推送master分支的所有内容；

	$ git push -u origin master

  (3)此后，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改；

	$ git push origin master

13.远程克隆仓库

	//以gitstudy项目为例，输入如下命令
	$ git clone git@github.com:hellogaod/gitstudy.git

要克隆一个仓库，首先必须知道仓库的地址，然后使用git clone命令克隆。

Git支持多种协议，包括https，但ssh协议速度最快。


14.1分支创建，切换，合并

 (1)git checkout

  创建并切换到分支

	$ git checkout -b dev
	Switched to a new branch 'dev'

  git checkout命令加上-b参数表示创建并切换，相当于以下两条命令

	$ git branch dev
	$ git checkout dev
	Switched to branch 'dev'

  然后，用git branch命令查看当前分支

	$ git branch
	* dev
	  master

  git branch命令会列出所有分支，当前分支前面会标一个*号。

  然后，我们就可以在dev分支上正常提交，比如对readme.md做个修改，加上XXX提交

  现在，dev分支的工作完成，我们就可以切换回master分支：

	$ git checkout master
	Switched to branch 'master'

  切换回master分支后，再查看一个readme.md文件，刚才添加的内容不见了！因为那个提交是在dev分支上，而master分支此刻的提交点并没有变：

  现在，我们把dev分支的工作成果合并到master分支上：

	$ git merge dev
	Updating d46f35e..b17d20e
	Fast-forward
	 readme.txt | 1 +
	 1 file changed, 1 insertion(+)

  git merge命令用于合并指定分支到当前分支。合并后，再查看readme.md的内容，就可以看到，和dev分支的最新提交是完全一样的。

  注意到上面的`Fast-forward`信息，Git告诉我们，这次合并是“快进模式”，也就是直接把master指向dev的当前提交，所以合并速度非常快。

	//合并完成后，就可以放心地删除dev分支了
	$ git branch -d dev
	Deleted branch dev (was 00d9ea4).

  删除后，查看branch，就只剩下master分支了：

	$ git branch
	* master
	
  (2)switch

  我们注意到切换分支使用git checkout <branch>，而前面讲过的撤销修改则是git checkout -- <file>，同一个命令，有两种作用，确实有点令人迷惑。

  实际上，切换分支这个动作，用switch更科学。因此，最新版本的Git提供了新的git switch命令来切换分支：

  创建并切换到新的dev分支，可以使用：

	$ git switch -c dev

  直接切换到已有的master分支，可以使用：
	
	$ git switch master

  使用新的git switch命令，比git checkout要更容易理解。

  (3)小结

  Git鼓励大量使用分支：

  查看分支：`git branch`

  创建分支：`git branch <name>`

  切换分支：`git checkout <name>或者git switch <name>`
 
  创建+切换分支：`git checkout -b <name>或者git switch -c <name>`

  合并某分支到当前分支：`git merge <name>`

  删除分支：`git branch -d <name>`

 **因为创建、合并和删除分支非常快，所以Git鼓励你使用分支完成某个任务，合并后再删掉分支，这和直接在master分支上工作效果是一样的，但过程更安全。**

14.2 分支冲突


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

一旦提交后，如果你又没有对工作区做任何修改，那么工作区就是“干净”的：

	$ git status
	On branch master
	nothing to commit, working tree clean

现在版本库变成了这样，暂存区就没有任何内容了：

![](https://www.liaoxuefeng.com/files/attachments/919020100829536/0)


**Git是如何跟踪修改的，每次修改，如果不用git add到暂存区，那就不会加入到commit中**

##  三 本地关联github远程仓库  ##

### 1.本地关联到gitHub上 ###

第1步：创建SSH Key。在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有id_rsa和id_rsa.pub这两个文件，如果已经有了，可直接跳到下一步。

![](https://img-blog.csdnimg.cn/20201231134115699.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2Zvc2hlbmd0YW5n,size_16,color_FFFFFF,t_70)

如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：

	$ ssh-keygen -t rsa -C "youremail@example.com"

你需要把邮件地址换成你自己的邮件地址，然后一路回车，使用默认值即可，由于这个Key也不是用于军事目的，所以也无需设置密码。

如果一切顺利的话，可以在用户主目录里找到.ssh目录，里面有id_rsa和id_rsa.pub两个文件，这两个就是SSH Key的秘钥对，id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥，可以放心地告诉任何人。

第2步：登陆GitHub，打开“Settings”，“SSH Keys”页面：

![](https://img-blog.csdnimg.cn/20201231134304426.png)

![](https://img-blog.csdnimg.cn/20201231134423605.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2Zvc2hlbmd0YW5n,size_16,color_FFFFFF,t_70)

然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容：

![](https://img-blog.csdnimg.cn/20201231134621479.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2Zvc2hlbmd0YW5n,size_16,color_FFFFFF,t_70)

为什么GitHub需要SSH Key呢？因为GitHub需要识别出你推送的提交确实是你推送的，而不是别人冒充的，而Git支持SSH协议，所以，GitHub只要知道了你的公钥，就可以确认只有你自己才能推送。

当然，GitHub允许你添加多个Key。假定你有若干电脑，你一会儿在公司提交，一会儿在家里提交，只要把每台电脑的Key都添加到GitHub，就可以在每台电脑上往GitHub推送了。

最后友情提示，在GitHub上免费托管的Git仓库，任何人都可以看到喔（但只有你自己才能改）。所以，不要把敏感信息放进去。

### 2.本地git仓库push到gitHub仓库 ###

github创建仓库如下图（从2019年开始，github创建私有仓库免费，如果你不想让别人看到你的仓库，可以设置私有，实在不放心，后面还会介绍手动创建一个git仓库）

![](https://img-blog.csdnimg.cn/20201231135356897.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2Zvc2hlbmd0YW5n,size_16,color_FFFFFF,t_70)

在Repository name填入`gitstudy`，其他保持默认设置，点击“Create repository”按钮，就成功地创建了一个新的Git仓库

目前，在GitHub上的这个`gitstudy`仓库还是空的，GitHub告诉我们，可以从这个仓库克隆出新的仓库，也可以把一个已有的本地仓库与之关联，然后，把本地仓库的内容推送到GitHub仓库。


现在，我们根据GitHub的提示，在本地的`gitstudy`仓库下运行命令：

	$ git remote add origin git@github.com:hellogaod/gitstudy.git

远程库的名字就是origin，这是Git默认的叫法，也可以改成别的，但是origin这个名字一看就知道是远程库。

下一步，就可以把本地库的所有内容推送到远程库上：

	$ git push -u origin master
	The authenticity of host 'github.com (13.250.177.223)' can't be established.
	RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
	Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
	Warning: Permanently added 'github.com,13.250.177.223' (RSA) to the list of known hosts.
	Enumerating objects: 29, done.
	Counting objects: 100% (29/29), done.
	Delta compression using up to 4 threads
	Compressing objects: 100% (24/24), done.
	Writing objects: 100% (29/29), 7.82 KiB | 1001.00 KiB/s, done.
	Total 29 (delta 7), reused 0 (delta 0), pack-reused 0
	remote: Resolving deltas: 100% (7/7), done.
	To github.com:hellogaod/gitstudy.git
	 * [new branch]      master -> master
	Branch 'master' set up to track remote branch 'master' from 'origin'.


把本地库的内容推送到远程，用git push命令，实际上是把当前分支master推送到远程。

由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。

推送成功后，可以立刻在GitHub页面中看到远程库的内容已经和本地一模一样：

![](https://img-blog.csdnimg.cn/20201231140633639.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2Zvc2hlbmd0YW5n,size_16,color_FFFFFF,t_70)

从现在起，只要本地作了提交，就可以通过命令：

	$ git push origin master
