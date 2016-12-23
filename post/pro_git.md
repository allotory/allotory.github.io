Title: Git基础笔记
Date: 2016-02-29
Tags: Git
Category: Git
Slug: pro_git
Author: Ellery

1. 版本控制是一种记录若干文件内容变化，以便将来查阅特定版本修订情况的系统。

2. 集中化的版本控制系统（Centralized Version Control Systems，简称CVCS ），诸如 CVS，Subversion 以及 Perforce 等，最显而易见的缺点是中央服务器的单点故障。

3. 分布式版本控制系统（ Distributed Version Control System，简称DVCS ），诸如 Git，Mercurial，Bazaar 还有 Darcs 等。

4. 对于任何一个文件，在 Git 内都只有三种状态：已提交（committed），已修改（modified）和已暂存（staged）。

	已提交表示该文件已经被安全地保存在本地数据库中了；—— git commit
	
	已修改表示修改了某个文件，但还没有提交保存；
	
	已暂存表示把已修改的文件放在下次提交时要保存的清单中。—— git add

5. Git 管理项目时，文件流转的三个工作区域：Git 的本地数据目录，工作目录以及暂存区域。

6. 每个项目都有一个 git 目录，它是 Git 用来保存元数据和对象数据库的地方。该目录非常重要，每次克隆镜像仓库的时候，实际拷贝的就是这个目录里面的数据。

7. 基本的 Git 工作流程

	![screenshot](/images/11_local_operations.png)

	* 在工作目录中修改某些文件。

	* 对这些修改了的文件作快照，并保存到暂存区域。

	* 提交更新，将保存在暂存区域的文件快照转储到git 目录中。
	
8. git config（译注：实际是 git-config 命令，只不过可以通过 git 加一个名字来呼叫此命令。），专门用来配置或读取相应的工作环境变量。而正是由这些环境变量，决定了 Git 在各个环节的具体工作方式和行为。这些变量可以存放在以下三个不同的地方：

	* /etc/gitconfig 文件：系统中对所有用户都普遍适用的配置。若使用 git config 时用 --system 选项，读写的就是这个文件。
	
	* ~/.gitconfig 文件：用户目录下的配置文件只适用于该用户。若使用 git config 时用--global 选项，读写的就是这个文件。

	* 当前项目的 git 目录中的配置文件（也就是工作目录中的 .git/config 文件）：这里的配置仅仅针对当前项目有效。每一个级别的配置都会覆盖上层的相同配置，所以 .git/config 里的配置会覆盖 /etc/gitconfig 中的同名变量。

9. 配置个人的用户名称和电子邮件地址：

		$ git config --global user.name "Ellery Zhang"
		$ git config --global user.email al**ry@msn.com

10. 默认使用的文本编辑器，Git 需要你输入一些额外消息的时候，会自动调用一个外部文本编辑器给你用。默认会使用操作系统指定的默认编辑器，一般可能会是 Vi 或者 Vim。

		$ git config --global core.editor emacs

11. 检查已有的配置信息，可以使用 git config --list 命令

		$ git config --list
		core.symlinks=false
		core.autocrlf=true
		color.diff=auto
		color.status=auto
		color.branch=auto
		color.interactive=true
		pack.packsizelimit=2g
		help.format=html
		http.sslcainfo=/bin/curl-ca-bundle.crt
		sendemail.smtpserver=/bin/msmtp.exe
		diff.astextplain.textconv=astextplain
		rebase.autosquash=true
		gui.encoding=utf-8
		i18n.commitencoding=GB2312
		svn.pathnameencoding=GB2312
		user.name=Ellery  Zhang
		user.email=al***ry@msn.com
		core.autocrlf=true
		core.excludesfile=D:\Documents\gitignore_global.txt
		i18n.commitencoding=utf-8

	可以直接查阅某个环境变量的设定，只要把特定的名字跟在后面即可。

		$ git config user.name
		Ellery  Zhang

12. 想了解 Git 的各式工具该怎么用，可以阅读它们的使用帮助，有三种方法：

		$ git help <verb>
		$ git <verb> --help
		$ man git-<verb>

	例如：学习 config 用法，会在浏览器中打开帮助页面。

		$ git help config
		Launching default browser to display HTML ...

13. 要对现有的某个项目开始用 Git 管理，只需到此项目所在的目录，执行：

		$ git init

14. 如果想对某个开源项目出一份力，可以先把该项目的 Git 仓库复制一份出来，这就需要用到 git clone 命令。克隆仓库的命令格式为 git clone [url] 。

		$ git clone git@github.com:allotory/mellisuga.git

	如果希望在克隆的时候，自己定义要新建的项目目录名称，可以在上面的命令最后指定：

		$ git clone git@github.com:allotory/mellisuga.git newname

15. Git 支持许多数据传输协议。之前的例子使用的是 git:// 协议，不过你也可以用 http(s):// 或者 user@server:/path.git 表示的 SSH 传输协议。

16. 工作目录下面的所有文件都不外乎这两种状态：已跟踪或未跟踪。
	
	已跟踪的文件是指本来就被纳入版本控制管理的文件，在上次快照中有它们的记录，工作一段时间后，它们的状态可能是未更新（未修改），已修改或者已放入暂存区。
	
	所有其他文件都属于未跟踪文件。它们既没有上次更新时的快照，也不在当前的暂存区域。

	![screenshot](/images/11_life_cycle.png)

17. 要确定哪些文件当前处于什么状态，可以用 git status 命令。

		$ git status

	新建一个 readme文件，再次查看状态

		$ git status
		On branch master
		Initial commit
		Untracked files:
		  (use "git add <file>..." to include in what will be committed)
		        readme.txt
		nothing added to commit but untracked files present (use "git add" to track)

18. 使用命令 git add 开始跟踪一个新文件。所以，要跟踪 README 文件，运行：

		$ git add readme.txt

	再次查看状态	

		$ git status
		On branch master
		Initial commit
		Changes to be committed:
		  (use "git rm --cached <file>..." to unstage)
		        new file:   readme.txt

19. 提交更新时请一定要确认还有什么修改过的或新建的文件还没有git add 过，否则提交的时候不会记录这些还没暂存起来的变化。

	所以，每次提交前，先用git status 看下，是不是都已暂存，然后再运行提交命令git commit：

		$ git commit -m 'add readme'
		[master (root-commit) a40e540] add readme
		 1 file changed, 1 insertion(+)
		 create mode 100644 readme.txt
	
	-m 参数后跟提交说明。如果不使用 -m 参数，则会启动文本编辑器以便输入本次提交的说明。
	
	可以看到，提交后它会告诉你，当前是在（master）分支提交的，本次提交的完整 SHA-1 校验和是（a40e540），以及在本次提交中，有（1）个文件修订过，（1）行添改和删改过。

20. 暂存已修改文件，修改readme文件后查看状态

		$ git status
		On branch master
		Changes not staged for commit:
		  (use "git add <file>..." to update what will be committed)
		  (use "git checkout -- <file>..." to discard changes in working directory)
		        modified:   readme.txt
		no changes added to commit (use "git add" and/or "git commit -a")

	文件 readme.txt 出现在 Changes not staged for commit 这行下面，说明已跟踪文件的内容发生了变化，但还没有放到暂存区。 

	要暂存这次更新，需要运行 git add 命令。

		$ git add readme.txt
		$ git status
		On branch master
		Changes to be committed:
		  (use "git reset HEAD <file>..." to unstage)
		        modified:   readme.txt

	此时文件已暂存，下次提交时就会一并记录到仓库。 

21. git status 命令的输出十分详细，但其用语有些繁琐。 如果你使用 git status -s 命令或 git status --short 命令，你将得到更为紧凑的格式输出。

22. 我们可以创建一个名为 .gitignore 的文件，列出要忽略的文件模式。

23. 文件 .gitignore 的格式规范如下：

	* 所有空行或者以 ＃ 开头的行都会被 Git 忽略。

	* 可以使用标准的 glob 模式匹配。

	* 匹配模式可以以（ / ）开头防止递归。

	* 匹配模式可以以（ / ）结尾指定目录。

	* 要忽略指定模式以外的文件或目录，可以在模式前加上惊叹号（ ! ）取反。

24. 要查看尚未暂存的文件更新了哪些部分，不加参数直接输入

		$ git diff

	此命令比较的是工作目录中当前文件和暂存区域快照之间的差异， 也就是修改之后还没有暂存起来的变化内容。
	
	git diff 本身只显示尚未暂存的改动，而不是自上次提交以来所做的所有改动。

25. 若要查看已暂存的将要添加到下次提交的内容，可以用

		$git diff --cached
	
	或（推荐使用）

		$git diff --staged

26. Git 中移除某个文件，就必须要从已跟踪文件清单中移除（确切地说，是从暂存区域移除），然后提交。

		$git rm

	如果删除之前修改过并且已经放到暂存区域的话，则必须要用强制删除选项 -f

		$git rm -f

	如果想把文件从 Git 仓库中删除（亦即从暂存区域移除），但仍然希望保留该文件在当前操作目录中。

		$git rm  --cached

27. Git 中对文件重命名

		$ git mv file_from file_to

	其实，运行 git mv 就相当于运行了下面三条命令：

		$ mv README.md README
		$ git rm README.md
		$ git add README

28. 查看提交历史

		$ git log

	默认时此命令会按提交时间倒序列出所有的更新，会列出每个提交的 SHA-1 校验和、作者的名字和电子邮件地址、提交时间以及提交说明。

		commit a40e54088aa857b2b46c0c9fe0a0f2455bea51a5
		Author: Ellery <allotory@msn.com>
		Date:   Mon Jan 4 16:40:47 2016 +0800
		    add readme

	git log 有很多参数，参数是 -p ，用来显示每次提交的内容差异。 -2 来表示仅显示最近两次提交

		$ git log -p -2

	想看到每次提交的简略的统计信息，你可以使用 --stat 参数

		$ git log --stat

	--stat 参数在每次提交的下面列出额所有被修改过的文件、有多少文件被修改了以及被修改过的文件的哪些行被移除或是添加了。

	--shortstat 只显示 --stat 中最后的参数修改添加移除统计。
	
	另一个常用的参数是 --pretty 。 这个参数可以指定使用不同于默认格式的方式展示提交历史。

	比如 oneline 将每个提交放在一行显示，查看的提交数很大时很有用，此时仅会显示sha-1和提交说明。

		$ git log --pretty=oneline
		b413d95e070aa3dc4ad425644a4086f00c983ba7 add test
		559eb5339da498e847eadb3d579bc267d0684627 insert readme
		a40e54088aa857b2b46c0c9fe0a0f2455bea51a5 add readme

	另外还有 short 格式:

		$ git log --pretty=short
		commit b413d95e070aa3dc4ad425644a4086f00c983ba7
		Author: Ellery <allotory@msn.com>
		    add test

	full格式 :

		$ git log --pretty=full
		commit b413d95e070aa3dc4ad425644a4086f00c983ba7
		Author: Ellery <allotory@msn.com>
		Commit: Ellery <allotory@msn.com>
		    add test

	fuller格式 

		$ git log --pretty=fuller
		commit b413d95e070aa3dc4ad425644a4086f00c983ba7
		Author:     Ellery <allotory@msn.com>
		AuthorDate: Mon Jan 4 16:55:11 2016 +0800
		Commit:     Ellery <allotory@msn.com>
		CommitDate: Mon Jan 4 16:55:11 2016 +0800
		    add test

	format参数可以定制要显示的记录格式。 这样的输出对后期提取分析格外有用。

		$ git log --pretty=format:"%h - %an, %ar : %s"
		b413d95 - Ellery, 6 days ago : add test
		559eb53 - Ellery, 6 days ago : insert readme
		a40e540 - Ellery, 6 days ago : add readme

	当 oneline 或 format 与另一个 log 选项 --graph 结合使用时尤其有用。 这个选项添加了一些     ASCII字符串来形象地展示你的分支、合并历史

		$ git log --pretty=format:"%h %s" --graph

29. 撤消操作

		$ git commit --amend

	这个命令会将暂存区中的文件提交。 如果自上次提交以来你还未做任何修改（例如，在上次提交后马上执行了此命令），那么快照会保持不变，所修改的只是提交信息。

30. 取消已暂存文件，将以暂存的文件从暂存区移除

		$ git reset HEAD test.md

31. 撤消对文件的修改

		$ git checkout -- test.md

32. 远程仓库是指托管在因特网或其他网络中的项目的版本库。

33. 查看已经配置的远程仓库服务器，可以使用 git remote 命令，如果你已经克隆了远程的仓库，那么至少应该能看到 origin - 这是 Git 给你克隆的仓库服务器的默认名字

		$ git remote
		origin

	指定选项 -v ，会显示需要读写远程仓库使用的 Git 保存的简写与其对应的 URL。

		$ git remote -v
		origin  git@github.com:allotory/mellisuga.git (fetch)
		origin  git@github.com:allotory/mellisuga.git (push)

34. 添加远程仓库，运行 git remote add <shortname> <url> 添加一个新的远程 Git 仓库

		$git remote add me git@github.com:allotory/mellisuga.git

	在命令行中使用字符串 me 来代替整个 URL。

	如果你想拉取远程仓库中有但你没有的信息，可以运行

		 $git fetch me

	等价于

		 $git fetch git@github.com:allotory/mellisuga.git

35. 从远程仓库中抓取与拉取

		$ git fetch [remote-name]

	例如

		$ git fetch origin

	因为 clone 命令克隆了某个仓库，命令会自动将其添加为远程仓库并默认以 'origin' 为简写。所以上述命令将会拥有那个远程仓库中所有分支的引用。

		$ git fetch origin master

	抓取回origin主机的master分支到本地。

	git fetch 命令会将数据拉取到你的本地仓库 - 它并不会自动合并或修改你当前的工作。

36. 如果有一个分支设置为跟踪一个远程分支， git pull 命令来自动的抓取然后合并远程分支到当前分支。

	默认情况下， git clone 命令会自动设置本地 master 分支跟踪克隆的远程仓库的 master 分支（或不管是什么名字的默认分支）。 运行 git pull 通常会从最初克隆的服务器上抓取数据并自动尝试合并到当前所在的分支。

37. 推送到远程仓库

		git push [remotename] [branch-name]

	要将 master 分支推送到 origin 服务器时

		$ git push origin master

38. 查看某个远程仓库的更多信息，可以使用 git remote show [remote-name] 命令。会列出远程仓库的 URL 与跟踪分支的信息

		$ git remote show origin
		* remote origin
		  Fetch URL: git@github.com:allotory/mellisuga.git
		  Push  URL: git@github.com:allotory/mellisuga.git
		  HEAD branch: master
		  Remote branch:
		    master tracked
		  Local branch configured for 'git pull':
		    master merges with remote master
		  Local ref configured for 'git push':
		    master pushes to master (up to date)

39. 要重命名引用的名字可以运行 git remote rename 去修改远程仓库的简写名

		$ git remote rename me mellisuga

	值得注意的是这同样也会修改你的远程分支名字。 那些过去引用 me/master 的现在会引用 mellisuga/master

40. 要移除远程仓库

		$ git remote rm mellisuga

41. Git 可以给历史中的某个提交打上标签，以示重要。 比较有代表性的是使用这个功能来标记发布结点（v1.0 等等）。

42. 列出标签

		$ git tag

43. Git 使用两种主要类型的标签：轻量标签（lightweight）与附注标签（annotated）。

44. 附注标签

		$ git tag -a v0.1 -m 'my first version 0.1'

	-m 选项指定了一条将会存储在标签中的信息。
	
	使用 git show 命令可以看到标签信息与对应的提交信息

		$ git show v0.1
		tag v0.1
		Tagger: Ellery <allotory@msn.com>
		Date:   Sat Jan 23 21:35:49 2016 +0800
		my first version v0.1
		…

	输出显示了打标签者的信息、打标签的日期时间、附注信息，然后显示具体的提交信息。

	轻量标签本质上是将提交校验和存储到一个文件中 - 没有保存任何其他信息。 创建轻量标签，不需要使用 -a 、 -s 或 -m 选项，只需要提供标签名字：

		$ git tag v0.1

	如果在标签上运行 git show ，你不会看到额外的标签信息。 命令只会显示出提交信息。

	可以对过去的提交打标签。要在那个提交上打标签，你需要在命令的末尾指定提交的校验和（或部分校验和）:

		$ git tag -a v0.01 5593b

	如不使用 -m 选项时，会默认调用 vim 编辑器编辑标签信息。

45. 默认情况下， git push 命令并不会传送标签到远程仓库服务器上。 在创建完标签后你必须显式地推送标签到共享服务器上。- 你可以运行 git push origin [tagname]

		$ git push origin v0.1

	如果想要一次性推送很多标签，也可以使用带有 --tags 选项的 git push 命令

		$ git push origin --tags

46. 创建新分支

	创建新分支只是为你创建了一个个可以移动的新的指针。如，创建一个 iss53 分支， 你需要使用 git branch 命令

	![screenshot](/images/11_basic_branching_1.png)

		$ git branch iss53

	这会在当前所在的提交对象上创建一个指针 iss53

	![screenshot](/images/11_basic_branching_2.png)

	Git 区分当前实在哪个分支上时，使用的是一个名为 HEAD 的特殊指针。指向当前所在的本地分支（可以将 HEAD 想象为当前分支的别名）。
	
	此时HEAD指针仍然在 master 分支上。 因为 git branch 命令仅仅创建一个新分支，并不会自动切换到新分支中去。

47. 切换到一个已存在的分支，你需要使用 git checkout 命令

		$ git checkout iss53

	这样 HEAD 就指向 iss53 分支了。

48. 可以简单地使用 git log 命令查看分叉历史。它会输出你的提交历史、各个分支的指向以及项目的分支分叉情况。

		git log --oneline --decorate --graph --all

49. 想要新建一个分支并同时切换到那个分支上，你可以运行一个带有 -b 参数的 git checkout 命令

		$ git checkout -b iss53

	它是下面两条命令的简写

		$ git branch iss53
		$ git checkout iss53

50. 分支合并，当 iss53 分支进行修改并提交后，可以将其与 master 分支合并，需要先切换到 master 分支，然后调用 git merge命令合并分支。

		$ git checkout master
		$ git merge iss53

51. 当你试图合并两个分支时，如果顺着这个分分支走下去能够到达另一个分支，那么 Git 在合并两者的时候，只会简单的将指针向前推进（指针右移），因为这种情况下的合并操作没有需要解决的分歧——这就叫做 “快进（fast-forward）”。

52. 删除分支

		$ git branch -d iss53

53. 你在两个不同的分支中，对同一个文件的同一个部分进行了不同的修改，Git 就没法干净的合并它们。在合并它们的时候就会产生合并冲突，此时 Git 做了合并，但是没有主动地创建一个新的合并提交。

	任何因包含合并冲突而有待解决的文件，都会以未合并状态（unmerged）标识出来

54. 分支管理

	查看分支列表

		$ git branch

	如果需要查看每个分支的最后一次提交，可以运行

		$ git branch -v

	--merged 与 --no-merged 这两个选项可以过滤这个列表中已经合并或尚未合并到当前分支的分支。
	
	如果分支包含了还未合并的工作，尝试使用 git branch -d 命令删除它时会失败，如果需要可以使用 -D参数强制删除。

55. 远程引用（origin）是对远程仓库的引用（指针），包括分支、标签等，来显式地获得远程引用的完整列表：

		git ls-remote (remote-name)

	或者

		git remote show (remote-name)

56. 远程跟踪分支是远程分支状态的引用。 它们是你不能移动的本地引用，当你做任何网络通信操作时，它们会自动移动。 远程跟踪分支像是你上次连接到远程仓库时，那些分支所处状态的书签。它们以 (remote)/(branch) 形式命名。

57. 远程跟踪分支，假设你的网络里有一个在 git.ourcompany.com 的 Git 服务器。 如果你从这里克隆，Git 的 clone 命令会为你自动将其命名为 origin ，拉取它的所有数据，创建一个指向它的 master 分支的指针，并且在本地将其命名为 origin/master 。 Git 也会给你一个与 origin 的 master 分支在指向同一个地方的本地 master 分支，这样你就有工作的基础。

					服务器（被命名） 本地
		默认仓库名	origin			-
		默认分支名	origin/master	master

58. 本地分支 dev 推送到远程仓库

		$ git push origin dev

	如果在远程仓库希望改名字，可以使用

		$ git push origin dev: newname

59. 其他协作者从服务器上抓取数据时，他们会在本地生成一个远程分支 origin/dev ，指向服务器的 dev 分支的引用。
	
	特别注意的一点是当抓取到新的远程跟踪分支时，本地不会自动生成一份可编辑的副本（拷贝）。即，这种情况下，不会有一个新的 dev 分支 - 只有一个不可以修改的 origin/dev 指针。
	
	可以运行 git merge origin/dev 将这些工作合并到当前所在的分支。
	
	如果想要在自己的本地 dev 分支上工作，可以将其建立在远程跟踪分支之上。

		$ git checkout -b dev origin/dev

60. 从一个远程跟踪分支检出的一个本地分支会自动创建一个叫做跟踪分支，跟踪分支是与远程分支有直接关系的本地分支。
	
	当克隆一个仓库时，它通常会自动地创建一个跟踪 origin/master 的 master 分支。
	
	可以设置其他的跟踪分支 - 其他远程仓库上的跟踪分支，或者不跟踪 master 分支。

		$ git checkout --track origin/serverfix

	将本地分支与远程分支设置为不同名字，本地分支 sf 会自动从 origin/serverfix 拉取。

		$ git checkout -b sf origin/serverfix

	如果想要查看设置的所有跟踪分支，可以使用 git branch 的 -vv 选项。

		$ git branch -vv

61. 删除远程分支

		$ git push origin --delete serverfix

62. 在 Git 中整合来自不同分支的修改主要有两种方法： merge 以及 rebase 。
	
	开发任务分叉到两个不同分支，又各自提交了更新。

	![screenshot](/images/12_basic_rebase_1.png)

	整合分支最容易的方法是 merge 命令。 它会把两个分支的最新快照（C3 和 C4）以及二者最近的共同祖先（C2）进行三方合并，合并的结果是生成一个新的快照（并提交）。

	![screenshot](/images/12_basic_rebase_2.png)

	还有一种方法：你可以提取在 C4 中引入的补丁和修改，然后在 C3 的基础上再应用一次。 在 Git 中，这种操作就叫做 变基。 你可以使用 rebase 命令将提交到某一分支上的所有修改都移至另一分支上，就好像“重新播放”一样。

		$ git checkout experiment
		$ git rebase master
		First, rewinding head to replay your work on top of it...
		Applying: added staged command

	它的原理是首先找到这两个分支（即当前分支 experiment、变基操作的目标基底分支 master）的最近共同祖先 C2，然后对比当前分支相对于该祖先的历次提交，提取相应的修改并存为临时文件，然后将当前分支指向目标基底 C3, 最后以此将之前另存为临时文件的修改依序应用。（译注：写明了 commit id，以便理解，下同）

	![screenshot](/images/12_basic_rebase_3.png)

	现在回到 master 分支，进行一次快进合并。

		$ git checkout master
		$ git merge experiment

	![screenshot](/images/12_basic_rebase_4.png)

	两种整合方法的最终结果没有任何区别，但是变基使得提交历史更加整洁。
	
	使用 git rebase [basebranch] [topicbranch] 命令可以直接将特性分支（即本例中的  experiment ）变基到目标分支（即 master）上。这样做能省去你先切换到 experiment 分支，再对其执行变基命令的多个步骤。

63. 变基也并非完美无缺，要用它得遵守一条准则：**不要对在你的仓库外有副本的分支执行变基**。