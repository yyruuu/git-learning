#Git
###Git简介？
1. Git是目前世界上最先进的分布式版本控制系统.
2. 集中式 VS 分布式
	- 版本库集中存放在中央服务器，干活时，先从服务器取得最新版本，结束之后，再推送给中央服务器。
		- 缺点：需要联网，传输速度慢。
	- 分布式系统则没有中央服务器，每个人的电脑上都有一个完整的版本库。且不需要联网。
		- 优点：安全性，就算出问题也容易恢复。
3. 创建版本库：

> $ mkdir learngit<br>
$ cd learngit<br>
$ pwd<br>
/Users/michael/learngit<br>

- 将目录变为Git可管理的仓库：初始化Git仓库

> $ git init<br>
Initialized empty Git repository in /Users/michael/learngit/.git/

- 把文件放到Git仓库：（要放在之前创建的git－learning目录下）
	- 用命令git.add告诉Git，把文件添加到仓库。

	> $ git add readme.txt

	- 用git commit告诉Git，把文件提交到仓库
		- -m后面输入的是本次提交的说明。
		- git commit运行后，会返回被改动的文件数和改动的内容 
	
	> $ git commit -m "wrote a readme file"<br>
[master (root-commit) eaadf4e] wrote a readme file
 1 file changed, 2 insertions(+)
 create mode 100644 readme.txt 
 
 	- commit 可以一次提交很多文件，可以多次add不同的文件后再commit

###Git的一些操作
1. git status命令可以掌握仓库当前的状态，只能查看xx文件被修改了。随时掌握工作区的状态，提交前后都可以查看。
2. git diff可以具体查看修改了什么内容。当作的修改没什么问题的时候，就可以提交了，提交修改也分两步。
	- git add fileName
	- git commit -m "修改的说明" 

3. git log命令查看历史修改纪录。HEAD表示当前版本，上一个版本是HEAD^,上上版本是HEAD^^
4. 回退到上一版本。使用git reset命令

	> $ git reset --hard HEAD^
	
5. 回退后悔后，想回到之前的版本，在终端中往回翻，找到那个版本的commit id号

	> git reset --hard 版本号（前几位即可）
	
6. 当终端中找不到commit id后，用git reflog命令，它可以记录每一次的命令，找到commit id，再用5的方法即可。

###工作区和暂存区
1. 工作区就是电脑本地的目录，比如之前创建的git-learning
2. 工作区有一个隐藏目录.git，是Git的版本库。git commit往master分支上提交更改。
	- 版本库里有一个成为stage（也叫index）的暂存区
	- Git自动创建的分支master
	- 指向master的指针HEAD

![](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384907702917346729e9afbf4127b6dfbae9207af016000/0)

###管理和修改

1. git add把要提交的所有修改放在暂存区。git commit将暂存区的所有文件提交到master分支。提交后暂存区就没有任何内容了。如果不用git add到暂存区，那就不会commit到分支。

![](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384907720458e56751df1c474485b697575073c40ae9000/0)

2. 撤销修改：git checkout -- 文件名 回到最后一次git commit或git add状态。
	- 上述命令可以将文件在工作区的修改全部撤销。
		- 一种情况：文件修改后还没有被放在暂存区，撤销修改后会回到和版本库一模一样的状态。
		- 另一种情况：文件已经提交到暂存区中，又作了修改，撤销修改后会回到添加到暂存区后的状态。
			- 当改乱了工作区的某个文件的内容，还添加到了暂存区。丢弃修改分两步走：
				- 第一步：git reset HEAD 文件名，编程第一种情况
				- 第二步：按照第一种情况操作git checkout -- filrName

3. 删除文件
	- 在文件管理器中删除文件，或者用rm 文件名 的命令。
	- 确定要从版本库中删除文件，用git rm 文件名 命令
	- 然后用git commit -m “xxx”

4. 误删文件：
	- 在版本库中恢复 git checkout -- test.txt

###远程仓库
1. Github账号于本地库关联：详见https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/0013752340242354807e192f02a44359908df8a5643103a000
	
	> git remote add origin git@server-name:path/repo-name.git

2. 本地提交后，提交到远程仓库：

	> git push origin master
	
3. 克隆Github库到本地
	
	> $ git clone git@github.com:自己的github用户名/仓库名.git
	
###分支管理
1. master分支又称为主分支，每提交一次，master分支会向前移动一步。
![](https://cdn.liaoxuefeng.com/cdn/files/attachments/0013849087937492135fbf4bbd24dfcbc18349a8a59d36d000/0)

2. 当创建了一个分支，和master指向相同的提交，再把HEAD指向dev，表面当前分支在dev上
![](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384908811773187a597e2d844eefb11f5cf5d56135ca000/0)

> git checkout -b dev
> git branch (查看分支)

3. 分支做出提交后，dev指针向前移动一步，而master还在原来的位置。把dev合并到master上，直接把master指向dev的当前提交即可。工作完成后也可以删除分支，只剩master分支。
![](https://cdn.liaoxuefeng.com/cdn/files/attachments/00138490883510324231a837e5d4aee844d3e4692ba50f5000/0)
	
4. git分支操作：

> 查看分支：git branch<br>
> 创建分支：git branch <name><br>
> 切换分支：git checkout <name><br>
> 创建+切换分支：git checkout -b <name><br>
> 合并某分支到当前分支：git merge <name><br>
> 删除分支：git branch -d <name><br>

5. bug分支－－git stash把工作现场隐藏，等bug修复后再继续工作(不太懂)
https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/00137602359178794d966923e5c4134bc8bf98dfb03aea3000
6. 开发一个新feature，最好新建一个分支；如果要丢弃一个没有被合并过的分支，可以通过git branch -D <name>强行删除。
7. 推送分支，比如dev：但不是所有的分支都需要往远程推送

> $ git push origin dev


###多人工作模式：
1. 创建远程origin的dev分支到本地：

	> $ git checkout -b dev origin/dev
	

	- 首先，可以试图用git push origin <branch-name>推送自己的修改；

	- 如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；如果合并有冲突，则解决冲突，并在本地提交；

	- 没有冲突或者解决掉冲突后，再用git push origin <branch-name>推送就能成功！

	- 如果git pull提示no tracking information，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream-to <branch-name> origin/<branch-name>。

2. git rebase 把分叉的提交历史变为一条直线，看上去更直观。rebase的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。

###标签管理
1. 创建标签
	- 先切换到要打标签的分支上git checkout -b 名称
	- git tag v1.0
	- git tag查看所有标签
	- $ git tag v0.9 f52c633给其他commit版本打标签

2. 删除标签：git tag -d v1.0(版本号)
3. 推送某个标签到远程：git push origin <tagname>
4. 推送所有未推送到远程的本地标签：git push origin --tags
5. 删除远程标签：
	- 先删除本地：git tag -d v1.0
	- 删除远程：git push origin :refs/tags/v1.0 
 

 
