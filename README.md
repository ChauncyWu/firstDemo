# Git教程笔记
[Git教程 - 廖雪峰的官方网站](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)</br>
![git图标](https://www.liaoxuefeng.com/files/attachments/0013848605496402772ffdb6ab448deb7eef7baa124171b000/0)

## Git简介

Git是目前世界上最先进的分布式版本控制系统
 
解决类似如下尴尬的问题：想删除一个段落，又怕将来想恢复找不回来怎么办？有办法，先把当前文件“另存为……”一个新的Word文件，再接着改，改到一定程度，再“另存为……”一个新文件，这样一直改下去，最后你的Word文档变成了这样：

![word图片](https://www.liaoxuefeng.com/files/attachments/0013848606651673ff1c83932d249118bf8fd5c58c15ca2000/0)

## 安装Git

[Windows上安装Git](https://git-for-windows.github.io/)

安装完成打开“Git Bush”，在命令行输入：

> $ git config --global user.name "Your Name"
> $ git config --global user.email "email@example.com"

**--global** 参数表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址。

## 创建版本库

1. 创建空目录（空文件夹）

	> $ mkdir learngit  
	> $ cd learngit  
	> $ pwd  
	> /Users/michael/learngit

	**pwd** 命令用于显示当前目录。
	如果你使用Windows系统，为了避免遇到各种莫名其妙的问题，请确保目录名（包括父目录）不包含中文。
	
2. 通过 **git init** 命令把这个目录变成Git可以管理的仓库：

	> $ git init</br>
	> Initialized empty Git repository in /Users/michael/learngit/.git/

3. 把文件添加到版本库

	第一步，用命令 **git add** 告诉Git，把文件添加到仓库，可反复多次使用，添加多个文件：
		
	> $ git add readme.txt 

	第二步，用命令 **git commit** 告诉Git，把文件提交到仓库：
		
	> $ git commit -m "说明文字"

## 时光机穿梭

* 要随时掌握工作区的状态，使用 **git status** 命令；

* 如果 **git status** 告诉你有文件被修改过，用 **git diff (文件名)** 可以查看修改内容；

* **git log**可以查看提交历史，以便确定要回退到哪个版本。如果嫌输出信息太多，看得眼花缭乱的，可以试试加上 **--pretty=oneline** 参数；

* 要把当前版本回退到之前版本，使用 **git reset** 命令;

* 在Git中，用HEAD表示当前版本，也就是最新的提交，上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100。

	> $ git reset --hard HEAD^
	> HEAD is now at ea34578 add distributed

* HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令 **git reset --hard commit_id**

* **git reflog** 用来记录你的每一次命令，可找到id用于回到未来版本；

* 撤销修改
	* 场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令 **git checkout -- file** 。

	* 场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令 **git reset HEAD file** ，就回到了场景1，第二步按场景1操作。

	* 场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。
* 命令 **git rm** 用于删除一个文件。如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，你会丢失最近一次提交后你修改的内容。

## 远程仓库

#### 免费获得Git远程仓库

* 注册GitHub账号。由于你的本地Git仓库和GitHub仓库之间的传输是通过SSH加密的，所以，需要一点设置：
	* 第1步：创建SSH Key。在用户主目录（C:/用户/ChauncyWu/）下，看看有没有.ssh目录，如果有，再看看这个目录下有没有id_rsa和id_rsa.pub这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：
	
		> $ ssh-keygen -t rsa -C "youremail@example.com"
	
		你需要把邮件地址换成你自己的邮件地址，然后一路回车，使用默认值即可,一切顺利的话，可以在用户主目录里找到.ssh目录，里面有id_rsa和id_rsa.pub两个文件，这两个就是SSH Key的秘钥对，id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥。

	* 第2步：登陆GitHub，打开“Account settings”，“SSH Keys”页面，然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容：
	
	![添加Key](https://www.liaoxuefeng.com/files/attachments/001384908342205cc1234dfe1b541ff88b90b44b30360da000/0)

#### 添加远程库

* 要关联一个远程库，使用命令 **git remote add origin git@server-name:path/repo-name.git** ；（git remote add origin git@github.com:ChauncyWu/repo-name.git）

* 关联后，使用命令 **git push -u origin master** 第一次推送master分支的所有内容，第一次使用Git的clone或者push命令连接GitHub时，会得到一个警告，这是因为Git使用SSH连接，而SSH连接在第一次验证GitHub服务器的Key时，需要你确认GitHub的Key的指纹信息是否真的来自GitHub的服务器，输入 *yes* 回车即可。

* 此后，每次本地提交后，只要有必要，就可以使用命令 **git push origin master** 推送最新修改；

#### 从远程库克隆

* 要克隆一个仓库，首先必须知道仓库的地址，然后使用git clone命令克隆。

	> $ git clone git@github.com:michaelliao/gitskills.git
	> Cloning into 'gitskills'...
	> remote: Counting objects: 3, done.
	> remote: Total 3 (delta 0), reused 0 (delta 0)
	> Receiving objects: 100% (3/3), done.

## 分支管理

#### 创建与合并分支

* 查看分支：git branch

* 创建分支：git branch 《name》

* 切换分支：git checkout 《name》

* 创建+切换分支：git checkout -b 《name》

* 合并某分支到当前分支：git merge 《name》

* 删除分支：git branch -d 《name》

#### 解决冲突

* 查看冲突：

	> $ git merge feature1
	> Auto-merging readme.txt
	> CONFLICT (content): Merge conflict in readme.txt
	> Automatic merge failed; fix conflicts and then commit the result.

	Git告诉我们，readme.txt文件存在冲突，必须手动解决冲突后再提交。git status也可以告诉我们冲突的文件。

* 查看readme.txt的内容：Git用<<<<<<<，=======，>>>>>>>标记出不同分支的内容，修改后再提交。
* 用 **git log --graph** 命令可以看到分支合并图。

#### 分支管理策略

* 合并分支时，如果可能，Git会用Fast forward模式，但这种模式下，删除分支后，会丢掉分支信息。如果要强制禁用Fast forward模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。

	> $ git merge --no-ff -m "merge with no-ff" dev
	> Merge made by the 'recursive' strategy.
	>  readme.txt |    1 +
	>  1 file changed, 1 insertion(+)

* 分支策略

	在实际开发中，我们应该按照几个基本原则进行分支管理：

	首先，master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；

	那在哪干活呢？干活都在dev分支上，也就是说，dev分支是不稳定的，到某个时候，比如1.0版本发布时，再把dev分支合并到master上，在master分支发布1.0版本；

	你和你的小伙伴们每个人都在dev分支上干活，每个人都有自己的分支，时不时地往dev分支上合并就可以了。

	所以，团队合作的分支看起来就像这样：

	![分支策略](https://www.liaoxuefeng.com/files/attachments/001384909239390d355eb07d9d64305b6322aaf4edac1e3000/0)

#### Bug分支

* Git还提供了一个stash功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作：

	> $ git stash
	> Saved working directory and index state WIP on dev: 6224937 add merge
	> HEAD is now at 6224937 add merge

* 修复bug
	* 首先确定要在哪个分支上修复bug，假定需要在master分支上修复，就从master创建临时分支：
	
	> $ git checkout master
	> Switched to branch 'master'
	> Your branch is ahead of 'origin/master' by 6 commits.
	> $ git checkout -b issue-101
	> Switched to a new branch 'issue-101'

* 现在修复bug，然后提交。
* 切换到master分支，并完成合并，最后删除issue-101分支。
* 接着回到dev分支干活了:

	> $ git checkout dev
	> Switched to branch 'dev'
	> $ git status
	> \# On branch dev
	> nothing to commit (working directory clean)
	
	工作区是干净的，刚才的工作现场存到哪去了？用git stash list命令看看：

	> $ git stash list
	> stash@{0}: WIP on dev: 6224937 add merge
	
	需要恢复一下，有两个办法：

	一是用 **git stash apply** 恢复，但是恢复后，stash内容并不删除，你需要用 **git stash drop** 来删除。

	另一种方式是用 **git stash pop** ，恢复的同时把stash内容也删了。

#### Feature分支

开发一个新feature，最好新建一个分支；

如果要丢弃一个没有被合并过的分支，可以通过 **git branch -D 《name》** 强行删除。

#### 多人协作
* 要查看远程库的信息，用 **git remote** ：

> $ git remote
> origin

或者，用 **git remote -v** 显示更详细的信息：

> $ git remote -v
> origin  git@github.com:michaelliao/learngit.git (fetch)
> origin  git@github.com:michaelliao/learngit.git (push)

上面显示了可以抓取和推送的origin的地址。如果没有推送权限，就看不到push的地址。

* 从本地推送分支，使用 **git push origin branch-name** ，如果推送失败，先用git pull抓取远程的新提交；

* 在本地创建和远程分支对应的分支，使用 **git checkout -b branch-name origin/branch-name** ，本地和远程分支的名称最好一致；

* 建立本地分支和远程分支的关联，使用 **git branch --set-upstream branch-name origin/branch-name** ；

* 从远程抓取分支，使用 **git pull** ，如果有冲突，要先处理冲突。

## 标签管理

#### 创建标签

* **git tag 《name》 （id）** 用于新建一个标签，默认为HEAD，也可以指定一个commit id；

* **git tag -a 《tagname》 -m "blablabla..."** 可以指定标签信息；

* **git tag -s 《tagname》 -m "blablabla..."**可以用PGP签名标签；

* 命令git tag可以查看所有标签；

* 用命令 **git show 《tagname》** 可以看到说明文字。

#### 操作标签

* 命令 **git push origin 《tagname》** 可以推送一个本地标签；

* 命令 **git push origin --tags** 可以推送全部未推送过的本地标签；

* 命令 **git tag -d 《tagname》** 可以删除一个本地标签；

* 命令 **git push origin :refs/tags/《tagname》** 可以删除一个远程标签，需先从本地删除；

## 使用GitHub

* 如何参与一个开源项目呢？比如人气极高的bootstrap项目，这是一个非常强大的CSS框架，你可以访问它的项目主页https://github.com/twbs/bootstrap，点“Fork”就在自己的账号下克隆了一个bootstrap仓库，然后，从自己的账号下clone：

	> git clone git@github.com:michaelliao/bootstrap.git

* 可以推送pull request给官方仓库来贡献代码。

## 使用码云

## 自定义Git

* 让Git显示颜色，会让命令输出看起来更醒目：

	> $ git config --global color.ui true

* 忽略特殊文件

	在Git工作区的根目录下创建一个特殊的.gitignore文件，然后把要忽略的文件名填进去，Git就会自动忽略这些文件。

	[GitHub准备的各种配置文件](https://github.com/github/gitignore)

	[ignore 自动生成网址](https://www.gitignore.io/)

* 配置别名

	* 如用 **git st** 表示 **git status**
	
		> $ git config --global alias.st status
		
		……

		> $ git config --global alias.co checkout
		> $ git config --global alias.ci commit
		> $ git config --global alias.br branch
	
	* 用 **git unstage file** 表示 **git reset HEAD file**
	
	* 配置一个 **git last** ，让其显示最后一次提交信息：

		> $ git config --global alias.last 'log -1'
	
	* 每个仓库的Git配置文件都放在.git/config文件中,别名就在[alias]后面，要删除别名，直接把对应的行删掉即可。

## 搭建Git服务器

[搭建Git服务器](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/00137583770360579bc4b458f044ce7afed3df579123eca000)
	