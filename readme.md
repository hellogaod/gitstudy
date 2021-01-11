#  git 仓库学习总结  #

**学习内容全部来自：[廖雪峰git教程](https://www.liaoxuefeng.com/wiki/896043488029600)，这里更新了新的内容并且做了个人总结**

 版本库定义：

	什么是版本库呢？版本库又名仓库，英文名repository，你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。
 
## 一 常用命令：（切记不要用中文路径） ##

  **1.创建空目录：**

   ①选一个合适的目录鼠标右键选择"git bash here"

   ②命令： 

    //创建gitRepository目录
    $ mkdir gitRepository
    
    //定位到gitRepository目录
    $ cd gitRepository
    
    //pwd用于显示当前目录
    $ pwd
  
  **2.（仓库初始化）将创建的目录编程Git可管理的仓库命令：**

    //会在gitRepository目录下生产.git目录，表示创建管理仓库成功（该文件夹下的文件不可动）
    $ git init
   
  **3.添加文件到仓库（即使文件存在，也需要这两个步骤）：**

   ①命令：
    
	//文件添加到仓库
    $ git add readme.txt
    
    //文件提交到仓库 -m 后的内容是本次提交的版本说明
    $git commit -m "文件新加了对git常用命令的整理"
    
   ②解释：

    为什么Git添加文件需要add，commit一共两步呢？因为commit可以一次提交很多文件，所以你可以多次add不同的文件 
  
  **4.查看git结果命令（文件提交情况）：**

    //git status命令可以让我们时刻掌握仓库当前的状态
    $ git status
   
  **5.查看git中被修改的文件，修改了什么具体内容命令**

    $ git diff readme.txt
   
  **6.查看历史提交版本命令**

    $ git log
  
   ②如果嫌输出信息太多，看得眼花缭乱的，可以试试加上--pretty=oneline参数

    $ git log --pretty=oneline
    
  **7.版本回退命令**

	 //HEAD指向的版本就是当前版本，上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100
	 $ git reset --hard HEAD^
	 
  **8.还可以去某个版本命令**

	//版本号没必要写全，前几位就可以了，Git会自动去找。当然也不能只写前一两位，因为Git可能会找到多个版本号，就无法确定是哪一个了。
	$ git reset --hard 8ca283d4ad0c7897f4eb9e486fd4be284c25bd84
	
  **9.回退到某个版本，但是版本commit id忘了，咋办？**

	//查看版本号commit id对应的提交说明（所以必须有准确的提交说明）
	$ git reflog
	
  **10.撤销修改命令**(在工作区的和暂缓区的区别)

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

   **11.删除文件**

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

  **12.本地git仓库推送到github远程仓库**(还有相关配置如下有介绍)

  (1)要关联一个远程库，使用命令

	$ git remote add origin git@github.com:hellogaod/gitstudy.git

  (2)关联后，使用命令`git push -u origin master`第一次推送master分支的所有内容；

	$ git push -u origin master

  (3)此后，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改；

	$ git push origin master

  **13.远程克隆仓库**

	//以gitstudy项目为例，输入如下命令
	$ git clone git@github.com:hellogaod/gitstudy.git

要克隆一个仓库，首先必须知道仓库的地址，然后使用git clone命令克隆。

Git支持多种协议，包括https，但ssh协议速度最快。


  **14.1分支创建，切换，合并**

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

  **14.2 分支冲突**

  准备新的feature1分支，继续我们的新分支开发：

	$ git switch -c feature1
	Switched to a new branch 'feature1'

  修改readme.md最后一行

  在feature1分支上提交（add，commit），然后切换到master分支

	$ git switch master
	Switched to branch 'master'

  在master分支上把readme.md文件的最后一行并提交。
  
  在当前master分支下合并feature1分支

	$ git merge feature1

  果然冲突了！Git告诉我们，readme.md文件存在冲突，必须手动解决冲突后再提交。`git status`也可以告诉我们冲突的文件

  我们可以直接查看readme.md的内容：

	<<<<<<< HEAD
	Creating a new branch is quick & simple.
	=======
	Creating a new branch is quick AND simple.
	>>>>>>> feature1

  Git用<<<<<<<，=======，>>>>>>>标记出不同分支的内容，我们修改后保存，然后在提交即可。

  用带参数的git log也可以看到分支的合并情况：

	$ git log --graph --pretty=oneline --abbrev-commit

  并且删除feature1分支

	$ git branch -d feature1

**14.3分支策略管理**

 **（1）分支管理**

  通常，合并分支时，如果可能，Git会用Fast forward模式，但这种模式下，删除分支后，会丢掉分支信息。

  如果要强制禁用`Fast forward`模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。

  实战如下：

  ①首先，仍然创建并切换dev分支：

	$ git switch -c dev
	Switched to a new branch 'dev'

  ②修改readme.md文件，并提交一个新的commit：

	$ git add readme.md 
	$ git commit -m "add merge"
	[dev f52c633] add merge
	 1 file changed, 1 insertion(+)
  
  ③现在，我们切换回master：

	$ git switch master
	Switched to branch 'master'

  ④准备合并`dev`分支，请注意--no-ff参数，表示禁用`Fast forward`：

	$ git merge --no-ff -m "merge with no-ff" dev
	Merge made by the 'recursive' strategy.
	 readme.md | 1 +
	 1 file changed, 1 insertion(+)

  ⑤因为本次合并要创建一个新的commit，所以加上-m参数，把commit描述写进去。

  ⑥合并后，我们用git log看看分支历史：

	$ git log --graph --pretty=oneline --abbrev-commit
	*   e1e9c68 (HEAD -> master) merge with no-ff
	|\  
	| * f52c633 (dev) add merge
	|/  
	*   cf810e4 conflict fixed
	...

  不使用`Fast forward`模式，merge后就像这样：

![](https://www.liaoxuefeng.com/files/attachments/919023225142304/0)

  比较一下使用 `Fast forward`模式：

![](https://www.liaoxuefeng.com/files/attachments/919023000423040/0)


  **合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。**


  **（2）分支策略**

  在实际开发中，我们应该按照几个基本原则进行分支管理：
 
  ①首先，`master`分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；

  ②干活都在`dev`分支上，也就是说，`dev`分支是不稳定的，到某个时候，比如1.0版本发布时，再把`dev`分支合并到`master`上，在`master`分支发布1.0版本；

  ③你和你的小伙伴们每个人都在dev分支上干活，每个人都有自己的分支，时不时地往dev分支上合并就可以了。

  所以，团队合作的分支看起来就像这样：

![](https://www.liaoxuefeng.com/files/attachments/919023260793600/0)

  **（3）Bug分支**

  软件开发中，bug就像家常便饭一样。有了bug就需要修复，在Git中，由于分支是如此的强大，所以，每个bug都可以通过一个新的临时分支来修复，修复后，合并分支，然后将临时分支删除。

  场景：当前正在`dev`分支上工作尚未完成（未提交），但是需要改一个`bug`（创建一个`issue-101代号`-bug分支）

  ①master分支上修改bug
  	
	//查看提交状态，未提交
	$ git status
	On branch dev
	Changes to be committed:
	  (use "git reset HEAD <file>..." to unstage)
	
		new file:   hello.py
	
	Changes not staged for commit:
	  (use "git add <file>..." to update what will be committed)
	  (use "git checkout -- <file>..." to discard changes in working directory)
	
		modified:   readme.md


	//Git还提供了一个stash功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作
	$ git stash
	Saved working directory and index state WIP on dev: f52c633 add merge


  现在，用git status查看工作区，就是干净的（除非有没有被Git管理的文件），因此可以放心地创建分支来修复bug。

  首先确定要在哪个分支上修复bug，假定需要在master分支上修复，就从master创建临时分支：

	//切换回maseter分支
	$ git checkout master
	Switched to branch 'master'
	Your branch is ahead of 'origin/master' by 6 commits.
	  (use "git push" to publish your local commits)
	
	//创建issue-101临时分支
	$ git checkout -b issue-101
	Switched to a new branch 'issue-101'

  修复完，add然后commit。

  完成后，切换到master分支，并完成合并，最后删除issue-101分支：

	$ git switch master
	Switched to branch 'master'
	Your branch is ahead of 'origin/master' by 6 commits.
	  (use "git push" to publish your local commits)
	
	//不要采用Fast forward模式
	$ git merge --no-ff -m "merged bug fix 101" issue-101
	Merge made by the 'recursive' strategy.
	 readme.txt | 2 +-
	 1 file changed, 1 insertion(+), 1 deletion(-)

  然后回到`dev`分支继续干活
  
	$ git switch dev
	Switched to branch 'dev'
	
	$ git status
	On branch dev
	nothing to commit, working tree clean

  工作区是干净的，刚才的工作现场存到哪去了？用`git stash list`命令看看：

	$ git stash list
	stash@{0}: WIP on dev: f52c633 add merge

  工作现场还在，Git把stash内容存在某个地方了，但是需要恢复一下，有两个办法：

  一是用`git stash apply`恢复，但是恢复后，stash内容并不删除，你需要用`git stash drop`来删除；

  另一种方式是用`git stash pop`，恢复的同时把stash内容也删了：

	$ git stash pop
	On branch dev
	Changes to be committed:
	  (use "git reset HEAD <file>..." to unstage)
	
		new file:   hello.py
	
	Changes not staged for commit:
	  (use "git add <file>..." to update what will be committed)
	  (use "git checkout -- <file>..." to discard changes in working directory)
	
		modified:   readme.md
	
	Dropped refs/stash@{0} (5d677e2ee266f39ea296182fb2354265b91b3b2a)


  再用`git stash list`查看，就看不到任何stash内容了：

	$ git stash list

  你可以多次stash，恢复的时候，先用`git stash list`查看，然后恢复指定的stash，用命令：

	$ git stash apply stash@{0}

  ②master分支上修复的bug，想要合并到当前dev分支
  
  我们只需要把4c805e2 fix bug 101这个提交所做的修改“复制”到dev分支。注意：我们只想复制4c805e2 fix bug 101这个提交所做的修改，并不是把整个master分支merge过来。

  为了方便操作，Git专门提供了一个cherry-pick命令，让我们能复制一个特定的提交到当前分支：

	$ git branch
	* dev
	  master
	//将4c805e2任务赋值到当前dev分支上
	$ git cherry-pick 4c805e2
	[master 1d4b803] fix bug 101
	 1 file changed, 1 insertion(+), 1 deletion(-)

  Git自动给dev分支做了一次提交，注意这次提交的commit是1d4b803，它并不同于master的4c805e2，因为这两个commit只是改动相同，但确实是两个不同的commit。用git cherry-pick，我们就不需要在dev分支上手动再把修bug的过程重复一遍。

  **（4）Feature分支**

  添加一个新功能时，你肯定不希望因为一些实验性质的代码，把主分支搞乱了，所以，每添加一个新功能，最好新建一个feature分支，在上面开发，完成后，合并，最后，删除该feature分支。

  例：现在，你终于接到了一个新任务：开发代号为Vulcan的新功能，该功能计划用于下一代星际飞船。并且让人蛋碎的是开发完了该功能取消了！！！

  于是准备开发：

	$ git switch -c feature-vulcan
	Switched to a new branch 'feature-vulcan'

  5分钟后，开发完毕：

	$ git add vulcan.py
	
	$ git status
	On branch feature-vulcan
	Changes to be committed:
	  (use "git reset HEAD <file>..." to unstage)
	
		new file:   vulcan.py
	
	$ git commit -m "add feature vulcan"
	[feature-vulcan 287773e] add feature vulcan
	 1 file changed, 2 insertions(+)
	 create mode 100644 vulcan.py		

  切回`dev`，准备合并：

	$ git switch dev

  一切顺利的话，feature分支和bug分支是类似的，合并，然后删除。

  但是！就在此时，接到上级命令，因经费不足，新功能必须取消！

  虽然白干了，但是这个包含机密资料的分支还是必须就地销毁：

	$ git branch -d feature-vulcan
	error: The branch 'feature-vulcan' is not fully merged.
	If you are sure you want to delete it, run 'git branch -D feature-vulcan'.

  销毁失败。Git友情提醒，`feature-vulcan`分支还没有被合并，如果删除，将丢失掉修改，如果要强行删除，需要使用大写的-D参数。

  现在我们强行删除：

	$ git branch -D feature-vulcan
	Deleted branch feature-vulcan (was 287773e).

  **15.多人协作**

  当你从远程仓库克隆时，实际上Git自动把本地的master分支和远程的master分支对应起来了，并且，远程仓库的默认名称是origin。

  要查看远程库的信息，用`git remote`：

	$ git remote
	origin

  或者，用`git remote -v`显示更详细的信息：

	$ git remote -v
	origin  git@github.com:hellogaod/gitstudy.git (fetch)
	origin  git@github.com:hellogaod/gitstudy.git (push)

  上面显示了可以抓取和推送的`origin`的地址。如果没有推送权限，就看不到push的地址。

  **（1）推送分支**

  推送分支，就是把该分支上的所有本地提交推送到远程库。推送时，要指定本地分支，这样，Git就会把该分支推送到远程库对应的远程分支上：

	$ git push origin master

  如果要推送其他分支，比如`dev`，就改成：

	$ git push origin dev

  但是，并不是一定要把本地分支往远程推送，那么，哪些分支需要推送，哪些不需要呢？

 - master分支是主分支，因此要时刻与远程同步；

 - dev分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；

 - bug分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；

 - feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。

  总之，就是在Git中，分支完全可以在本地自己藏着玩，是否推送，视你的心情而定！

  **(2)抓取分支**

  多人协作时，大家都会往master和dev分支上推送各自的修改。

  现在，模拟一个你的小伙伴，可以在另一台电脑（注意要把SSH Key添加到GitHub）或者同一台电脑的另一个目录下克隆：

	$ git clone git@github.com:hellogaod/gitstudy.git
	Cloning into 'learngit'...
	remote: Counting objects: 40, done.
	remote: Compressing objects: 100% (21/21), done.
	remote: Total 40 (delta 14), reused 40 (delta 14), pack-reused 0
	Receiving objects: 100% (40/40), done.
	Resolving deltas: 100% (14/14), done.

  当你的小伙伴从远程库clone时，默认情况下，你的小伙伴只能看到本地的master分支。不信可以用git branch命令看看：

	$ git branch
	* master

  现在，你的小伙伴要在dev分支上开发，就必须创建远程origin的dev分支到本地，于是他用这个命令创建本地dev分支：

	$ git checkout -b dev origin/dev
 
  现在，他就可以在dev上继续修改，然后，时不时地把dev分支push到远程：

	$ git add env.txt
	
	$ git commit -m "add env"
	[dev 7a5e5dd] add env
	 1 file changed, 1 insertion(+)
	 create mode 100644 env.txt
	
	//推送到github
	$ git push origin dev
	Counting objects: 3, done.
	Delta compression using up to 4 threads.
	Compressing objects: 100% (2/2), done.
	Writing objects: 100% (3/3), 308 bytes | 308.00 KiB/s, done.
	Total 3 (delta 0), reused 0 (delta 0)
	To github.com:michaelliao/learngit.git
	   f52c633..7a5e5dd  dev -> dev

  你的小伙伴已经向`origin/dev`分支推送了他的提交，而碰巧你也对同样的文件作了修改，并试图推送：

	$ cat env.txt
	env
	
	$ git add env.txt
	
	$ git commit -m "add new env"
	[dev 7bd91f1] add new env
	 1 file changed, 1 insertion(+)
	 create mode 100644 env.txt
	
	$ git push origin dev
	To github.com:michaelliao/learngit.git
	 ! [rejected]        dev -> dev (non-fast-forward)
	error: failed to push some refs to 'git@github.com:michaelliao/learngit.git'
	hint: Updates were rejected because the tip of your current branch is behind
	hint: its remote counterpart. Integrate the remote changes (e.g.
	hint: 'git pull ...') before pushing again.
	hint: See the 'Note about fast-forwards' in 'git push --help' for details.

  推送失败，因为你的小伙伴的最新提交和你试图推送的提交有冲突，解决办法也很简单：**Git已经提示我们，（pull之前先将文件commit到本地仓库）先用`git pull`把最新的提交从`origin/dev`抓下来，然后，在本地合并，解决冲突，再推送：**（当前本地分支应该是在dev中）

	$ git pull
	There is no tracking information for the current branch.
	Please specify which branch you want to merge with.
	See git-pull(1) for details.
	
	    git pull <remote> <branch>
	
	If you wish to set tracking information for this branch you can do so with:
	
	    git branch --set-upstream-to=origin/<branch> dev


 ` git pull`也失败了，原因是没有指定本地dev分支与远程`origin/dev`分支的链接，根据提示，设置dev和origin/dev的链接：

	$ git branch --set-upstream-to=origin/dev dev
	Branch 'dev' set up to track remote branch 'dev' from 'origin'.

  再pull：

	$ git pull
	Auto-merging env.txt
	CONFLICT (add/add): Merge conflict in env.txt
	Automatic merge failed; fix conflicts and then commit the result.

  这回git pull成功，但是合并有冲突，需要手动解决，解决的方法和分支管理中的解决冲突完全一样。解决后，提交，再push：

	$ git commit -m "fix env conflict"
	[dev 57c53ab] fix env conflict
	
	$ git push origin dev
	Counting objects: 6, done.
	Delta compression using up to 4 threads.
	Compressing objects: 100% (4/4), done.
	Writing objects: 100% (6/6), 621 bytes | 621.00 KiB/s, done.
	Total 6 (delta 0), reused 0 (delta 0)
	To github.com:michaelliao/learngit.git
	   7a5e5dd..57c53ab  dev -> dev

  **（3）总结**

  多人协作的工作模式通常是这样：

 - 首先，可以试图用git push origin <branch-name>推送自己的修改；

 - 如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；

 - 如果合并有冲突，则解决冲突，并在本地提交；

 - 没有冲突或者解决掉冲突后，再用git push origin <branch-name>推送就能成功！

 - 如果git pull提示no tracking information，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream-to <branch-name> origin/<branch-name>。

  这就是多人协作的工作模式，一旦熟悉了，就非常简单。

  **16.Rebase**（因人而异，我就不喜欢这个）

  多人在同一个分支上协作时，很容易出现冲突。即使没有冲突，后push的童鞋不得不先pull，在本地合并，然后才能push成功。

  每次合并再push后，分支变成了这样：

	$ git log --graph --pretty=oneline --abbrev-commit
	* d1be385 (HEAD -> master, origin/master) init hello
	*   e5e69f1 Merge branch 'dev'
	|\  
	| *   57c53ab (origin/dev, dev) fix env conflict
	| |\  
	| | * 7a5e5dd add env
	| * | 7bd91f1 add new env
	| |/  
	* |   12a631b merged bug fix 101
	|\ \  
	| * | 4c805e2 fix bug 101
	|/ /  
	* |   e1e9c68 merge with no-ff
	|\ \  
	| |/  
	| * f52c633 add merge
	|/  
	*   cf810e4 conflict fixed

  总之看上去很乱，有强迫症的童鞋会问：为什么Git的提交历史不能是一条干净的直线？

  其实是可以做到的！

  Git有一种称为rebase的操作，有人把它翻译成“变基”。

  我们还是从实际问题出发，看看怎么把分叉的提交变成直线。

  在和远程分支同步后，我们对hello.py这个文件做了两次提交。用git log命令看看：

	$ git log --graph --pretty=oneline --abbrev-commit
	* 582d922 (HEAD -> master) add author
	* 8875536 add comment
	* d1be385 (origin/master) init hello
	*   e5e69f1 Merge branch 'dev'
	|\  
	| *   57c53ab (origin/dev, dev) fix env conflict
	| |\  
	| | * 7a5e5dd add env
	| * | 7bd91f1 add new env
	...

  注意到Git用(HEAD -> master)和(origin/master)标识出当前分支的HEAD和远程origin的位置分别是582d922 add author和d1be385 init hello，本地分支比远程分支快两个提交。

  现在我们尝试推送本地分支：

	$ git push origin master
	To github.com:michaelliao/learngit.git
	 ! [rejected]        master -> master (fetch first)
	error: failed to push some refs to 'git@github.com:michaelliao/learngit.git'
	hint: Updates were rejected because the remote contains work that you do
	hint: not have locally. This is usually caused by another repository pushing
	hint: to the same ref. You may want to first integrate the remote changes
	hint: (e.g., 'git pull ...') before pushing again.
	hint: See the 'Note about fast-forwards' in 'git push --help' for details.

  很不幸，失败了，这说明有人先于我们推送了远程分支。按照经验，先pull一下：

	$ git pull
	remote: Counting objects: 3, done.
	remote: Compressing objects: 100% (1/1), done.
	remote: Total 3 (delta 1), reused 3 (delta 1), pack-reused 0
	Unpacking objects: 100% (3/3), done.
	From github.com:michaelliao/learngit
	   d1be385..f005ed4  master     -> origin/master
	 * [new tag]         v1.0       -> v1.0
	Auto-merging hello.py
	Merge made by the 'recursive' strategy.
	 hello.py | 1 +
	 1 file changed, 1 insertion(+)

  再用git status看看状态：

	$ git status
	On branch master
	Your branch is ahead of 'origin/master' by 3 commits.
	  (use "git push" to publish your local commits)
	
	nothing to commit, working tree clean

  加上刚才合并的提交，现在我们本地分支比远程分支超前3个提交。

  用git log看看：

	$ git log --graph --pretty=oneline --abbrev-commit
	*   e0ea545 (HEAD -> master) Merge branch 'master' of github.com:michaelliao/learngit
	|\  
	| * f005ed4 (origin/master) set exit=1
	* | 582d922 add author
	* | 8875536 add comment
	|/  
	* d1be385 init hello

  对强迫症童鞋来说，现在事情有点不对头，提交历史分叉了。如果现在把本地分支push到远程，有没有问题？

  有！

  什么问题？

  不好看！

  有没有解决方法？

  有！

  这个时候，rebase就派上了用场。我们输入命令git rebase试试：

	$git rebase
	First, rewinding head to replay your work on top of it...
	Applying: add comment
	Using index info to reconstruct a base tree...
	M	hello.py
	Falling back to patching base and 3-way merge...
	Auto-merging hello.py
	Applying: add author
	Using index info to reconstruct a base tree...
	M	hello.py
	Falling back to patching base and 3-way merge...
	Auto-merging hello.py

  输出了一大堆操作，到底是啥效果？再用git log看看：

	$ git log --graph --pretty=oneline --abbrev-commit
	* 7e61ed4 (HEAD -> master) add author
	* 3611cfe add comment
	* f005ed4 (origin/master) set exit=1
	* d1be385 init hello

  **这就是rebase操作的特点：把分叉的提交历史“整理”成一条直线，看上去更直观。
  缺点是本地的分叉提交已经被修改过了。**

  最后，通过push操作把本地分支推送到远程：

	$ git push origin master
	Counting objects: 6, done.
	Delta compression using up to 4 threads.
	Compressing objects: 100% (5/5), done.
	Writing objects: 100% (6/6), 576 bytes | 576.00 KiB/s, done.
	Total 6 (delta 2), reused 0 (delta 0)
	remote: Resolving deltas: 100% (2/2), completed with 1 local object.
	To github.com:michaelliao/learngit.git
	   f005ed4..7e61ed4  master -> master

  再用git log看看效果：

	$ git log --graph --pretty=oneline --abbrev-commit
	* 7e61ed4 (HEAD -> master, origin/master) add author
	* 3611cfe add comment
	* f005ed4 set exit=1
	* d1be385 init hello

  远程分支的提交历史也是一条直线。

  **17.标签管理**

  发布一个版本时，我们通常先在版本库中打一个标签（tag），这样，就唯一确定了打标签时刻的版本。将来无论什么时候，取某个标签的版本，就是把那个打标签的时刻的历史版本取出来。所以，标签也是版本库的一个快照。

  Git的标签虽然是版本库的快照，但其实它就是指向某个commit的指针（跟分支很像对不对？但是分支可以移动，标签不能移动），所以，创建和删除标签都是瞬间完成的。


  Git有commit，为什么还要引入tag？

  “请把上周一的那个版本打包发布，commit号是6a5819e...”

  “一串乱七八糟的数字不好找！”

  如果换一个办法：

  “请把上周一的那个版本打包发布，版本号是v1.2”

  “好的，按照tag v1.2查找commit就行！”

  所以，tag就是一个让人容易记住的有意义的名字，它跟某个commit绑在一起。

  **（1）创建标签**
  
  在Git中打标签非常简单，首先，切换到需要打标签的分支上：

	$ git branch
	* dev
	  master
	$ git checkout master
	Switched to branch 'master'

  然后，敲命令`git tag <name>`就可以打一个新标签：

	$ git tag v1.0

  可以用命令git tag查看所有标签：

	$ git tag
	v1.0 

  默认标签是打在最新提交的commit上的。有时候，如果忘了打标签，比如，现在已经是周五了，但应该在周一打的标签没有打，怎么办？

  方法是找到历史提交的commit id，然后打上就可以了：

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

  比方说要对`add merge`这次提交打标签，它对应的commit id是f52c633，敲入命令：

	$ git tag v0.9 f52c633

  再用命令git tag查看标签：

	$ git tag
	v0.9
	v1.0

  注意，标签不是按时间顺序列出，而是按字母排序的。可以用`git show <tagname>`查看标签信息：

	$ git show v0.9
	commit f52c63349bc3c1593499807e5c8e972b82c8f286 (tag: v0.9)
	Author: Michael Liao <askxuefeng@gmail.com>
	Date:   Fri May 18 21:56:54 2018 +0800
	
	    add merge
	
	diff --git a/readme.txt b/readme.md

 
  可以看到，v0.9确实打在add merge这次提交上。

  还可以创建带有说明的标签，用-a指定标签名，-m指定说明文字

	$ git tag -a v0.1 -m "version 0.1 released" 1094adb

  用命令git show <tagname>可以看到说明文字：

	$ git show v0.1
	tag v0.1
	Tagger: Michael Liao <askxuefeng@gmail.com>
	Date:   Fri May 18 22:48:43 2018 +0800
	
	version 0.1 released
	
	commit 1094adb7b9b3807259d8cb349e7df1d4d6477073 (tag: v0.1)
	Author: Michael Liao <askxuefeng@gmail.com>
	Date:   Fri May 18 21:06:15 2018 +0800
	
	    append GPL
	
	diff --git a/readme.txt b/readme.txt
	...

 **注意：标签总是和某个commit挂钩。如果这个commit既出现在master分支，又出现在dev分支，那么在这两个分支上都可以看到这个标签。**

  **(2)操作标签**

  如果标签打错了，也可以删除：

	$ git tag -d v0.1
	Deleted tag 'v0.1' (was f15b0dd)

  因为创建的标签都只存储在本地，不会自动推送到远程。所以，打错的标签可以在本地安全删除。

  如果要推送某个标签到远程，使用命令`git push origin <tagname>`：

	$ git push origin v1.0
	Total 0 (delta 0), reused 0 (delta 0)
	To github.com:michaelliao/learngit.git
	 * [new tag]         v1.0 -> v1.0

  或者，一次性推送全部尚未推送到远程的本地标签：

	$ git push origin --tags
	Total 0 (delta 0), reused 0 (delta 0)
	To github.com:michaelliao/learngit.git
	 * [new tag]         v0.9 -> v0.9

  如果标签已经推送到远程，要删除远程标签就麻烦一点，先从本地删除：

	$ git tag -d v0.9
	Deleted tag 'v0.9' (was f52c633)

  然后，从远程删除。删除命令也是push，但是格式如下：

	$ git push origin :refs/tags/v0.9
	To github.com:michaelliao/learngit.git
	 - [deleted]         v0.9

  要看看是否真的从远程库删除了标签，可以登陆GitHub查看。

  小结

   命令git push origin <tagname>可以推送一个本地标签；

   命令git push origin --tags可以推送全部未推送过的本地标签；

   命令git tag -d <tagname>可以删除一个本地标签；

   命令git push origin :refs/tags/<tagname>可以删除一个远程标签。

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


## 四 自定义git ##

### 1.忽略特殊文件 ###
  有些时候，你必须把某些文件放到Git工作目录中，但又不能提交它们，比如保存了数据库密码的配置文件啦，等等，每次git status都会显示Untracked files ...，有强迫症的童鞋心里肯定不爽。

  好在Git考虑到了大家的感受，这个问题解决起来也很简单，在Git工作区的根目录下创建一个特殊的.gitignore文件，然后把要忽略的文件名填进去，Git就会自动忽略这些文件。

  不需要从头写.gitignore文件，GitHub已经为我们准备了各种配置文件，只需要组合一下就可以使用了。所有配置文件可以直接在线浏览：[https://github.com/github/gitignore](https://github.com/github/gitignore)

  忽略文件的原则是：

  - 忽略操作系统自动生成的文件，比如缩略图等；
  
  - 忽略编译生成的中间文件、可执行文件等，也就是如果一个文件是通过另一个文件自动生成的，那自动生成的文件就没必要放进版本库，比如Java编译产生的.class文件；

  - 忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件。

  举个例子：

  假设你在Windows下进行Python开发，Windows会自动在有图片的目录下生成隐藏的缩略图文件，如果有自定义目录，目录下就会有Desktop.ini文件，因此你需要忽略Windows自动生成的垃圾文件：

	# Windows:
	Thumbs.db
	ehthumbs.db
	Desktop.ini

  然后，继续忽略Python编译产生的.pyc、.pyo、dist等文件或目录：

	# Python:
	*.py[cod]
	*.so
	*.egg
	*.egg-info
	dist
	build

  加上你自己定义的文件，最终得到一个完整的.gitignore文件，内容如下：

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

  最后一步就是把`.gitignore`也提交到Git，就完成了！当然检验`.gitignore`的标准是`git status`命令是不是说`working directory clean`。

  使用Windows的童鞋注意了，如果你在资源管理器里新建一个`.gitignore`文件，它会非常弱智地提示你必须输入文件名，但是在文本编辑器里“保存”或者“另存为”就可以把文件保存为`.gitignore`了。

  有些时候，你想添加一个文件到Git，但发现添加不了，原因是这个文件被`.gitignore`忽略了：

	$ git add App.class
	The following paths are ignored by one of your .gitignore files:
	App.class
	Use -f if you really want to add them.

  如果你确实想添加该文件，可以用`-f`强制添加到Git：

	$ git add -f App.class



  **或者你发现，可能是`.gitignore`写得有问题，需要找出来到底哪个规则写错了，可以用`git check-ignore`命令检查：**

	$ git check-ignore -v App.class
	.gitignore:3:*.class	App.class

  Git会告诉我们，.gitignore的第3行规则忽略了该文件，于是我们就可以知道应该修订哪个规则。

  还有些时候，当我们编写了规则排除了部分文件时：

	# 排除所有.开头的隐藏文件:
	.*
	# 排除所有.class文件:
	*.class

  但是我们发现.*这个规则把.gitignore也排除了，并且App.class需要被添加到版本库，但是被*.class规则排除了。

	# 排除所有.开头的隐藏文件:
	.*
	# 排除所有.class文件:
	*.class
	
	# 不排除.gitignore和App.class:
	!.gitignore
	!App.class

  **把指定文件排除在.gitignore规则外的写法就是!+文件名，所以，只需把例外文件添加进去即可。**

### 2.配置别名 ###（可用可不用，个人感觉用了就没了灵魂）

  如果敲git st就表示git status那就简单多了，当然这种偷懒的办法我们是极力赞成的。

  我们只需要敲一行命令，告诉Git，以后`st`就表示`status`：

	$ git config --global alias.st status

  当然还有别的命令可以简写，很多人都用co表示checkout，ci表示commit，br表示branch：

	$ git config --global alias.co checkout
	$ git config --global alias.ci commit
	$ git config --global alias.br branch

  以后提交就可以简写成：

	$ git ci -m "bala bala bala..."

### 3.搭建Git服务器 ###
