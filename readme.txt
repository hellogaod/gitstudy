gitRepository目录是一个版本库：
	版本库定义：什么是版本库呢？版本库又名仓库，英文名repository，你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。
	
常用命令：（切记不要用中文路径）
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
				