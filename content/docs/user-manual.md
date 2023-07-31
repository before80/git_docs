+++
title = "用户手册"
weight = 30
type = "docs"
date = 2023-07-26T09:47:23+08:00
description = ""
isCJKLanguage = true
draft = false

+++



# User Manual - 用户手册



https://git-scm.com/docs/user-manual

## 简介

​	Git 是一个快速的分布式版本控制系统。

​	本手册旨在适合具有基本UNIX命令行技能但没有Git先前知识的人阅读。

​	[Repositories and Branches](https://git-scm.com/docs/user-manual#repositories-and-branches)和[Exploring Git history](https://git-scm.com/docs/user-manual#exploring-git-history) 解释了如何使用git获取和研究项目。阅读这些章节可以了解如何构建和测试软件项目的特定版本，搜索回归等等。

​	需要进行实际开发的人还应阅读[Developing with Git](https://git-scm.com/docs/user-manual#Developing-With-git)和[Sharing development with others](https://git-scm.com/docs/user-manual#sharing-development)。

​	更多章节涵盖更多专业主题。

​	完整的参考文档可通过man页或[git-help[1]](https://chat.openai.com/1/git-help)命令获得。例如，对于命令 `git clone <repo>`，您可以选择使用：

``` bash
$ man git-clone
```

或者：

``` bash
$ git help clone
```

​	使用后者，您可以使用您选择的手册查看器；有关更多信息，请参见[git-help[1]](https://chat.openai.com/1/git-help)。

​	还请查看[Git快速参考](https://git-scm.com/docs/user-manual#git-quick-start)，了解Git命令的简要概述，不含任何解释。

​	最后，请参阅[此手册的注释和待办事项清单](https://git-scm.com/docs/user-manual#todo)，以了解如何帮助完善本手册。

## 仓库和分支

### 获取Git仓库的方法

​	在阅读本手册时，有一个Git仓库供您实验会很有用。

​	最佳方法是使用[git-clone[1]](https://chat.openai.com/1/git-clone)命令下载现有仓库的副本。如果您还没有项目的选择，请参考以下一些有趣的示例：

```
	# Git itself (approx. 40MB download):
$ git clone git://git.kernel.org/pub/scm/git/git.git
	# the Linux kernel (approx. 640MB download):
$ git clone git://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git
```

​	对于大型项目，初始克隆可能需要较长时间，但您只需要克隆一次。

​	克隆命令会创建一个以项目名称（上述示例中为`git`或`linux`）命名的新目录。进入此目录后，您将看到它包含了一个项目文件的副本，称为[工作树](https://git-scm.com/docs/user-manual#def_working_tree)，以及一个特殊的顶级目录`.git`，其中包含有关项目历史的所有信息。

### 如何检出项目的不同版本

​	Git最适合用作存储一组文件历史的工具。它将历史存储为项目内容的一组压缩的相互关联的快照。在Git中，每个这样的版本称为[commit](https://git-scm.com/docs/user-manual#def_commit)。

​	这些快照并不一定全部按照从最旧到最新的单一线排列；相反，工作可以同时沿着并行的开发线进行，称为[分支](https://git-scm.com/docs/user-manual#def_branch)，这些分支可以合并和分叉。

​	单个Git仓库可以跟踪多个分支上的开发。它通过保持一个引用每个分支上最新提交的[头部](https://git-scm.com/docs/user-manual#def_head)列表来实现；[git-branch[1]](https://chat.openai.com/1/git-branch)命令显示分支头列表：

``` bash
$ git branch
* master
```

​	新克隆的仓库默认包含一个分支头，名为“master”，并且工作目录初始化为该分支头指向的项目状态。

​	大多数项目还使用[标签](https://git-scm.com/docs/user-manual#def_tag)。标签与分支一样，是对项目历史的引用，可以使用[git-tag[1]](https://chat.openai.com/1/git-tag)命令列出标签：

``` bash
$ git tag -l
v2.6.11
v2.6.11-tree
v2.6.12
v2.6.12-rc2
v2.6.12-rc3
v2.6.12-rc4
v2.6.12-rc5
v2.6.12-rc6
v2.6.13
...
```

​	标签始终指向项目的相同版本，而分支则预期随着开发的进行而不断推进。

​	创建一个新的分支头，指向这些版本之一，并使用[git-switch[1]](https://chat.openai.com/1/git-switch)检出它：

``` bash
$ git switch -c new v2.6.13
```

​	工作目录将反映出项目在标记v2.6.13时的内容，并且[git-branch[1]](https://chat.openai.com/1/git-branch)显示两个分支，其中星号标记当前检出的分支：

``` bash
$ git branch
  master
* new
```

​	如果您决定更换到2.6.17版本，可以使用以下命令将当前分支指向v2.6.17：

``` bash
$ git reset --hard v2.6.17
```

请注意，如果当前分支头是对特定历史点的唯一引用，那么重置该分支可能会导致您无法找到它曾指向的历史；因此请谨慎使用此命令。

### 理解历史记录：Commits

​	项目历史中的每个更改都由一个commit表示。[git-show[1]](https://chat.openai.com/1/git-show)命令显示当前分支上最近的提交：

``` bash
$ git show
commit 17cf781661e6d38f737f15f53ab552f1e95960d7
Author: Linus Torvalds <torvalds@ppc970.osdl.org.(none)>
Date:   Tue Apr 19 14:11:06 2005 -0700

    Remove duplicate getenv(DB_ENVIRONMENT) call

    Noted by Tony Luck.

diff --git a/init-db.c b/init-db.c
index 65898fa..b002dc6 100644
--- a/init-db.c
+++ b/init-db.c
@@ -7,7 +7,7 @@

 int main(int argc, char **argv)
 {
-	char *sha1_dir = getenv(DB_ENVIRONMENT), *path;
+	char *sha1_dir, *path;
 	int len, i;

 	if (mkdir(".git", 0755) < 0) {
```

​	如您所见，commit显示了谁进行了最新更改，他们做了什么以及为什么。

​	每个commit都有一个40位的十六进制id，有时称为"object name"或"SHA-1 id"，显示在`git show`输出的第一行上。通常您可以用较短的名字引用一个commit，例如标签或分支名称，但是这个较长的名字也可能很有用。最重要的是，它是该commit的全局唯一名称：因此，如果您将对象名称告诉其他人（例如通过电子邮件），那么您可以确保该名称在他们的仓库中引用的是与您的仓库中相同的commit（假设他们的仓库中确实有该commit）。由于对象名称是通过对commit内容进行哈希计算得到的，所以可以确保commit不会在其名称不变的情况下发生更改。

​	实际上，在[Git概念](https://git-scm.com/docs/user-manual#git-concepts)中我们将看到Git历史中存储的所有内容，包括文件数据和目录内容，都存储在具有其内容哈希的对象中。

#### 理解历史：commits、parents和reachability 

​	每个commit（除了项目中的第一个commit）还有一个父commit，显示了此commit之前发生了什么。跟随父commit的链条最终会将您带回到项目的开始。

​	但是，commits不会形成简单的列表；Git允许开发线同时分叉和合并，两条开发线在哪里再次汇合被称为“合并点”。表示合并的commit因此可能具有多个父commit，其中每个父commit表示通往该点的一条开发线上最近的commit。

​	最好的方法是使用[gitk[1]](https://chat.openai.com/1/gitk)命令来查看它是如何工作的；现在在Git仓库上运行gitk，并查找合并commit将有助于理解Git如何组织历史。

​	在下面，我们说如果commit X“可达”commit Y，则commit X是commit Y的祖先。同样地，您可以说Y是X的后代，或者说有一条从commit Y到commit X的父链。

#### 理解历史：历史图表

​	我们有时使用如下的图表来表示Git历史。Commits显示为"o"，它们之间的链接用绘制线表示：- /和\。时间从左到右：

```
         o--o--o <-- Branch A
        /
 o--o--o <-- master
        \
         o--o--o <-- Branch B
```

​	如果我们需要讨论特定的commit，字符"o"可能会替换为其他字母或数字。

#### 理解历史：什么是分支？

​	当我们需要精确时，我们将使用"branch"一词表示开发线，"branch head"（或"head"）表示对分支上最新commit的引用。在上面的示例中，名为"A"的分支头是对一个特定commit的指针，但我们将这个指向该点的三个commit的行称为"branch A"的一部分。

​	然而，当不会导致混淆时，我们通常只使用"branch"这个术语来表示分支和分支头。

### 操作分支

​	创建、删除和修改分支都很快捷简单；以下是这些命令的摘要：

- `git branch`

  列出所有分支。

- `git branch <branch>`

  创建一个名为`<branch>`的新分支，它引用与当前分支相同的历史点。

- `git branch <branch> <start-point>`

  创建一个名为`<branch>`的新分支，它引用`<start-point>`，可以以任何您喜欢的方式指定它，包括使用分支名称或标签名称。

- `git branch -d <branch>`

  删除分支`<branch>`；如果分支未完全合并到其上游分支或包含在当前分支中，则此命令将带有警告的失败。

- `git branch -D <branch>`

  无视分支合并状态删除分支`<branch>`。

- `git switch <branch>`

  将当前分支切换为`<branch>`，更新工作目录以反映`<branch>`引用的版本。

- `git switch -c <new> <start-point>`

  创建一个名为`<new>`的新分支，引用`<start-point>`，然后检出它。

​	特殊符号"HEAD"始终可以用来指代当前分支。实际上，Git使用`.git`目录中的`HEAD`文件来记住当前的分支：

``` bash
$ cat .git/HEAD
ref: refs/heads/master
```

### 不创建新分支查看旧版本

​	`git switch`命令通常期望一个分支头，但是当使用`--detach`参数调用时，它也会接受任意提交，例如，您可以检出由标签引用的提交：

``` bash
$ git switch --detach v2.6.17
Note: checking out 'v2.6.17'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by performing another switch.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command again. Example:

  git switch -c new_branch_name

HEAD is now at 427abfa Linux v2.6.17
```

​	然后，HEAD将指向提交的SHA-1，而不是分支，`git branch`显示您不再在分支上：

``` bash
$ cat .git/HEAD
427abfa28afedffadfca9dd8b067eb6d36bac53f
$ git branch
* (detached from v2.6.17)
  master
```

​	在这种情况下，我们称HEAD为"detached"。

​	这是一种无需为新分支编写名称就可以检查特定版本的简便方法。如果决定，稍后仍然可以为该版本创建新的分支（或标签）。

### 查看远程仓库的分支

​	在克隆时创建的"master"分支是克隆源仓库中HEAD的副本。然而，该仓库可能还有其他分支，而您的本地仓库将保留每个远程分支的跟踪分支，称为远程跟踪分支，您可以使用`-r`选项查看它们，例如[git-branch[1]](https://chat.openai.com/1/git-branch)：

``` bash
$ git branch -r
  origin/HEAD
  origin/html
  origin/maint
  origin/man
  origin/master
  origin/next
  origin/seen
  origin/todo
```

​	在这个例子中，"origin"被称为远程仓库，或简称为"remote"。从我们的角度来看，该仓库的分支称为"remote branches"。上面列出的远程跟踪分支是基于克隆时的远程分支创建的，并且将通过`git fetch`（因此`git pull`）和`git push`进行更新。详情请参见[使用git fetch更新仓库](https://git-scm.com/docs/user-manual#Updating-a-repository-With-git-fetch)。

​	您可能希望在自己的分支上构建其中一个远程跟踪分支，就像构建标签一样：

``` bash
$ git switch -c my-todo-copy origin/todo
```

​	您还可以直接检出`origin/todo`来查看它或编写一次性的补丁。详情请参见[分离头指针](https://git-scm.com/docs/user-manual#detached-head)。

​	请注意，"origin"只是Git默认用于引用克隆源仓库的名称。

### 命名分支、标签和其他引用

​	分支、远程跟踪分支和标签都是对commits的引用。所有引用都以以`refs`开头的斜杠分隔路径名命名；我们目前使用的名称实际上是缩写：

- 分支`test`的缩写为`refs/heads/test`。
- 标签`v2.6.18`的缩写为`refs/tags/v2.6.18`。
- `origin/master`的缩写为`refs/remotes/origin/master`。

​	完整名称有时在有多个具有相同简写名称的引用时很有用。

（新创建的引用实际上存储在`.git/refs`目录中，路径是其名称给出的路径。但是，出于效率考虑，它们也可能被打包在一个单独的文件中；详情请参见[git-pack-refs[1]](https://chat.openai.com/1/git-pack-refs)）。

​	作为另一个有用的快捷方式，一个仓库的"HEAD"可以使用该仓库的名称来表示。例如，"origin"通常是指向仓库"origin"的"HEAD"分支的快捷方式。

​	有关Git检查引用的完整路径列表，以及在有多个具有相同简写名称的引用时决定选择哪个的顺序，请参阅[gitrevisions[7]](https://chat.openai.com/7/gitrevisions)的"SPECIFYING REVISIONS"部分。



### 使用git fetch更新仓库

​	在克隆一个仓库并提交了一些自己的更改后，您可能希望检查原始仓库是否有更新。

​	没有参数的`git-fetch`命令将更新所有远程跟踪分支到原始仓库中找到的最新版本。它不会影响您自己的任何分支，甚至不会影响在克隆时为您创建的"master"分支。

### 从其他仓库获取分支

​	您还可以使用[git-remote[1]](https://chat.openai.com/1/git-remote)从其他仓库跟踪分支，而不仅限于克隆的仓库：

``` bash
$ git remote add staging git://git.kernel.org/.../gregkh/staging.git
$ git fetch staging
...
From git://git.kernel.org/pub/scm/linux/kernel/git/gregkh/staging
 * [new branch]      master     -> staging/master
 * [new branch]      staging-linus -> staging/staging-linus
 * [new branch]      staging-next -> staging/staging-next
```

​	新的远程跟踪分支将存储在您给出的`git remote add`的简写名称下，例如在本例中为`staging`：

``` bash
$ git branch -r
  origin/HEAD -> origin/master
  origin/master
  staging/master
  staging/staging-linus
  staging/staging-next
```

​	如果以后运行`git fetch <remote>`，将更新指定`<remote>`的远程跟踪分支。

​	如果检查`.git/config`文件，您会看到Git已添加一个新的部分：

``` bash
$ cat .git/config
...
[remote "staging"]
	url = git://git.kernel.org/pub/scm/linux/kernel/git/gregkh/staging.git
	fetch = +refs/heads/*:refs/remotes/staging/*
...
```

​	这会导致Git跟踪远程仓库的分支；您可以通过使用文本编辑器编辑`.git/config`来修改或删除这些配置选项（有关详细信息，请参见[git-config[1]](https://chat.openai.com/1/git-config)中的"CONFIGURATION FILE"部分）。

## 探索Git历史

​	Git最好被视为一个存储文件集合历史的工具。它通过存储文件层次结构的压缩快照以及显示这些快照之间关系的"提交"来实现这一点。

​	Git提供了非常灵活和快速的工具来探索项目的历史。

​	我们先介绍一个特殊的工具，用于查找引入项目中错误的提交。

### 如何使用二分查找来查找回归 How to use bisect to find a regression

​	假设您的项目的2.6.18版本工作正常，但"master"分支的版本崩溃。有时找到这种回归的原因的最佳方法是通过对项目的历史进行蛮力搜索，以找到导致问题的特定提交。[git-bisect[1]](https://chat.openai.com/1/git-bisect)命令可以帮助您做到这一点：

``` bash
$ git bisect start
$ git bisect good v2.6.18
$ git bisect bad master
Bisecting: 3537 revisions left to test after this
[65934a9a028b88e83e2b0f8b36618fe503349f8e] BLOCK: Make USB storage depend on SCSI rather than selecting it [try #6]
```

​	如果此时运行`git branch`，您会看到Git暂时将您切换到了"(no branch)"。HEAD现在处于未关联任何分支的状态，并直接指向一个提交（提交id为65934），它可从"master"到达，但无法从v2.6.18到达。编译并测试它，看看是否崩溃。假设它崩溃了。然后执行：

``` bash
$ git bisect bad
Bisecting: 1769 revisions left to test after this
[7eff82c8b1511017ae605f0c99ac275a7e21b867] i2c-core: Drop useless bitmaskings
```

这会检出一个较早的版本。以此类推，在每个阶段告诉Git版本它给您的是好的还是坏的，并注意每次剩余待测试版本数将减半左右。

​	经过大约13次测试（在本例中），它将输出有问题提交的提交id。然后，您可以使用[git-show[1]](https://chat.openai.com/1/git-show)查看提交，找出是谁编写的，并将带有提交id的错误报告发送给他们。最后，运行

``` bash
$ git bisect reset
```

返回到之前所在的分支。

​	请注意，`git bisect`在每个点为您检出的版本只是一个建议，如果您认为换一个版本是个好主意，您可以尝试不同的版本。例如，有时候您可能会遇到导致了与问题无关的提交；运行

``` bash
$ git bisect visualize
```

这会运行gitk并在它选择的提交上标记一个带有"bisect"标记的标记。选择一个看起来安全的附近提交，记录其提交id，并使用以下命令检出：

``` bash
$ git reset --hard fb47ddb2db
```

然后测试，运行`bisect good`或`bisect bad`，并继续。

与其运行`git bisect visualize`然后`git reset --hard fb47ddb2db`，您可能只想告诉Git要跳过当前的提交：

``` bash
$ git bisect skip
```

​	不过，在这种情况下，Git最终可能无法确定在一些已跳过的提交和稍后的有问题的提交之间的第一个有问题的提交。

​	如果您有一个测试脚本可以判断哪些提交是好的，哪些是有问题的，还可以自动化二分查找过程。有关此功能和其他`git bisect`功能的更多信息，请参阅[git-bisect[1]](https://chat.openai.com/1/git-bisect)。

### 命名提交

​	我们已经看到了几种提交命名的方法： 

- 40位十六进制对象名称
- 分支名称：指向给定分支头部的提交
- 标签名称：指向给定标签所指向的提交（我们已经看到分支和标签是[引用](https://git-scm.com/docs/user-manual#how-git-stores-references)的特殊情况）。
- HEAD：指向当前分支的头部

​	还有许多其他命名方式，请参阅[gitrevisions[7]](https://chat.openai.com/7/gitrevisions)手册中的"SPECIFYING REVISIONS"部分，以获取完整的命名修订版本的方式列表。以下是一些示例：

``` bash
$ git show fb47ddb2 # the first few characters of the object name
		    # are usually enough to specify it uniquely
$ git show HEAD^    # the parent of the HEAD commit
$ git show HEAD^^   # the grandparent
$ git show HEAD~4   # the great-great-grandparent
```

​	请注意，合并提交可能有多个父提交；默认情况下，`^`和`~`将跟随提交中列出的第一个父提交，但您也可以选择：

``` bash
$ git show HEAD^1   # show the first parent of HEAD
$ git show HEAD^2   # show the second parent of HEAD
```

​	除了HEAD，还有几个其他的特殊提交名称：

​	合并提交（稍后会讨论），以及诸如`git reset`这样会更改当前检出提交的操作，通常会在当前操作之前将ORIG_HEAD设置为HEAD的值。

​	`git fetch`操作总是将最后获取的分支的头部存储在FETCH_HEAD中。例如，如果您运行`git fetch`而不指定本地分支作为操作的目标：

``` bash
$ git fetch git://example.com/proj.git theirbranch
```

获取的提交仍将从FETCH_HEAD可用。

​	在讨论合并时，我们还将看到特殊名称MERGE_HEAD，它指的是要合并到当前分支的另一个分支。

​	[git-rev-parse[1]](../1/git-rev-parse)命令是一个低级命令，偶尔用于将某个提交的名称转换为该提交的对象名称：

``` bash
$ git rev-parse origin
e05db0fd4f31dde7005f075a84f96b360d05984b
```

### 创建标签

我们还可以创建一个标签来指向特定的提交；运行以下命令后：

``` bash
$ git tag stable-1 1b2e1d63ff
```

​	您可以使用`stable-1`来引用提交1b2e1d63ff。

​	这将创建一个"轻量级"标签。如果您还希望在标签中包含评论，并可能对其进行加密签名，则应该创建一个标签对象；有关详细信息，请参见[git-tag[1]](https://chat.openai.com/1/git-tag)手册。

### 浏览修订版本 Browsing revisions

​	[git-log[1]](https://chat.openai.com/1/git-log)命令可以显示提交列表。默认情况下，它显示从父提交可达的所有提交；但您也可以提出更具体的要求：

``` bash
$ git log v2.5..	# commits since (not reachable from) v2.5
$ git log test..master	# commits reachable from master but not test
$ git log master..test	# ...reachable from test but not master
$ git log master...test	# ...reachable from either test or master,
			#    but not both
$ git log --since="2 weeks ago" # commits from the last 2 weeks
$ git log Makefile      # commits which modify Makefile
$ git log fs/		# ... which modify any file under fs/
$ git log -S'foo()'	# commits which add or remove any file data
			# matching the string 'foo()'
```

​	当然，您可以将所有这些组合起来；以下命令查找从v2.5开始的提交，这些提交触及了`Makefile`或`fs`目录下的任何文件：

``` bash
$ git log v2.5.. Makefile fs/
```

​	您还可以要求git log显示补丁：

``` bash
$ git log -p
```

​	有关更多显示选项，请参阅[git-log[1]](https://chat.openai.com/1/git-log)手册中的`--pretty`选项。

​	请注意，git log从最近的提交开始，并通过父提交向后工作；然而，由于Git历史可能包含多个独立的开发线，提交的特定列表顺序可能有些随意。

### 生成差异

​	您可以使用[git-diff[1]](https://chat.openai.com/1/git-diff)在任何两个版本之间生成差异：

``` bash
$ git diff master..test
```

​	这将产生两个分支末端之间的差异。如果您希望找到从它们的共同祖先到test的差异，可以使用三个点而不是两个点：

``` bash
$ git diff master...test
```

​	有时您需要的是一组补丁；为此，您可以使用[git-format-patch[1]](https://chat.openai.com/1/git-format-patch)：

``` bash
$ git format-patch master..test
```

将生成一个文件，其中包含从test可达但从master不可达的每个提交的补丁。

### 查看旧文件版本

​	您可以通过先检出正确的修订版本来查看文件的旧版本。但有时，仅查看单个文件的旧版本而不检出任何内容会更加方便；这个命令可以实现：

``` bash
$ git show v2.5:fs/locks.c
```

​	冒号前面可以是任何命名了某个提交的内容，冒号后面可以是Git跟踪的文件的任何路径。

### 示例

#### 计算分支上的提交数量

​	假设您想知道自从`mybranch`与`origin`分叉以来，在`mybranch`上做了多少提交：

``` bash
$ git log --pretty=oneline origin..mybranch | wc -l
```

​	或者，您可能经常使用更低级的`git-rev-list[1]`命令来完成这种操作，该命令仅列出所有给定提交的SHA-1：

``` bash
$ git rev-list origin..mybranch | wc -l
```

#### 检查两个分支是否指向相同的历史点

​	假设您想检查两个分支是否指向历史中的相同点。

``` bash
$ git diff origin..master
```

将告诉您两个分支的项目内容是否相同；不过，理论上可能会通过两个不同的历史路径到达相同的项目内容。您可以比较对象名称：

``` bash
$ git rev-list origin
e05db0fd4f31dde7005f075a84f96b360d05984b
$ git rev-list master
e05db0fd4f31dde7005f075a84f96b360d05984b
```

​	或者您可以回忆起`...`运算符选择可从任一引用或另一引用到达的所有提交，但不同时可达；因此

``` bash
$ git log origin...master
```

当两个分支相同时，将不返回提交。

#### 查找包含给定修复的第一个已打标签版本 Find first tagged version including a given fix

​	假设您知道提交e05db0fd修复了某个问题。您想找到包含该修复的最早已打标签版本。

​	当然，可能有不止一个答案 —— 如果在提交e05db0fd之后分支历史分叉，那么可能会有多个“最早”的已打标签版本。

​	您可以直接目视检查从e05db0fd开始的提交：

``` bash
$ gitk e05db0fd..
```

或者您可以使用[git-name-rev[1]](https://chat.openai.com/1/git-name-rev)，它将根据找到的任何标签为提交赋予名称，该标签指向提交的后代之一：

``` bash
$ git name-rev --tags e05db0fd
e05db0fd tags/v1.5.0-rc1^0~23
```

而[git-describe[1]](https://chat.openai.com/1/git-describe)命令则相反，使用提交所基于的标签来命名修订版本：

``` bash
$ git describe e05db0fd
v1.5.0-rc0-260-ge05db0f
```

但是这可能有助于您猜测给定提交之后可能出现的标签。

​	如果您只想验证给定的已打标签版本是否包含给定的提交，可以使用[git-merge-base[1]](https://chat.openai.com/1/git-merge-base)：

``` bash
$ git merge-base e05db0fd v1.5.0-rc1
e05db0fd4f31dde7005f075a84f96b360d05984b
```

​	merge-base命令查找给定提交的共同祖先，并在一个提交是另一个提交的后代的情况下始终返回其中一个；因此，上面的输出显示e05db0fd实际上是v1.5.0-rc1的一个祖先。

​	或者，注意到：

``` bash
$ git log v1.5.0-rc1..e05db0fd
```

将只在v1.5.0-rc1包含e05db0fd时输出为空，因为它只输出不可从v1.5.0-rc1到达的提交。

​	作为另一种选择，[git-show-branch[1]](https://chat.openai.com/1/git-show-branch)命令会列出可从其参数到达的提交，并在左侧显示显示指示该提交可从哪个参数到达的内容。因此，如果您运行类似于：

``` bash
$ git show-branch e05db0fd v1.5.0-rc0 v1.5.0-rc1 v1.5.0-rc2
! [e05db0fd] Fix warnings in sha1_file.c - use C99 printf format if
available
 ! [v1.5.0-rc0] GIT v1.5.0 preview
  ! [v1.5.0-rc1] GIT v1.5.0-rc1
   ! [v1.5.0-rc2] GIT v1.5.0-rc2
...
```

然后像下面这样的行：

```
+ ++ [e05db0fd] Fix warnings in sha1_file.c - use C99 printf format if
available
```

显示e05db0fd可从自身、v1.5.0-rc1和v1.5.0-rc2到达，但不可从v1.5.0-rc0到达。

#### 显示特定分支中独有的提交

​	假设您希望查看从名为`master`的分支头部可达，但从仓库中的任何其他分支头部不可达的所有提交。

​	我们可以使用[git-show-ref[1]](https://chat.openai.com/1/git-show-ref)列出此仓库中的所有分支头部：

``` bash
$ git show-ref --heads
bf62196b5e363d73353a9dcf094c59595f3153b7 refs/heads/core-tutorial
db768d5504c1bb46f63ee9d6e1772bd047e05bf9 refs/heads/maint
a07157ac624b2524a059a3414e99f6f44bebc1e7 refs/heads/master
24dbc180ea14dc1aebe09f14c8ecf32010690627 refs/heads/tutorial-2
1e87486ae06626c2f31eaa63d26fc0fd646c8af2 refs/heads/tutorial-fixes
```

​	我们可以仅获取分支头名称，并去掉`master`，使用标准实用程序cut和grep的帮助

``` bash
$ git show-ref --heads | cut -d' ' -f2 | grep -v '^refs/heads/master'
refs/heads/core-tutorial
refs/heads/maint
refs/heads/tutorial-2
refs/heads/tutorial-fixes
```

​	然后，我们可以要求查看所有从master可达，但从这些其他头部不可达的提交

``` bash
$ gitk master --not $( git show-ref --heads | cut -d' ' -f2 |
				grep -v '^refs/heads/master' )
```

​	显然，有无数种变化方式；例如，要查看所有从某个分支可达，但从仓库中的任何标签不可达的提交：

``` bash
$ gitk $( git show-ref --heads ) --not  $( git show-ref --tags )
```

（有关诸如`--not`等提交选择语法的解释，请参阅[gitrevisions[7]](https://chat.openai.com/7/gitrevisions)。）

#### 为软件发布创建更改日志和tarball  - Creating a changelog and tarball for a software release

​	[git-archive[1]](https://chat.openai.com/1/git-archive)命令可以从项目的任何版本创建tar或zip存档；例如：

``` bash
$ git archive -o latest.tar.gz --prefix=project/ HEAD
```

将使用HEAD创建一个gzipped的tar存档，其中每个文件名前面都有`project/`。如果可能，输出文件格式将根据输出文件扩展名进行推断，请参阅[git-archive[1]](https://chat.openai.com/1/git-archive)了解详情。

​	Git版本1.7.7之前的版本不知道`tar.gz`格式，您需要显式使用gzip：

``` bash
$ git archive --format=tar --prefix=project/ HEAD | gzip >latest.tar.gz
```

​	如果您正在发布软件项目的新版本，可能希望同时制作更改日志并将其包含在发布公告中。

​	例如，Linus Torvalds会通过打标签来发布新的内核版本，然后运行：

``` bash
$ release-script 2.6.12 2.6.13-rc6 2.6.13-rc7
```

其中`release-script`是一个外观如下的shell脚本：

```
#!/bin/sh
stable="$1"
last="$2"
new="$3"
echo "# git tag v$new"
echo "git archive --prefix=linux-$new/ v$new | gzip -9 > ../linux-$new.tar.gz"
echo "git diff v$stable v$new | gzip -9 > ../patch-$new.gz"
echo "git log --no-merges v$new ^v$last > ../ChangeLog-$new"
echo "git shortlog --no-merges v$new ^v$last > ../ShortLog"
echo "git diff --stat --summary -M v$last v$new > ../diffstat-$new"
```

然后，只需将输出命令剪切并粘贴到验证它们是否正确的地方。

#### 查找引用包含给定内容的文件的提交 Finding commits referencing a file with given content

​	有人给您一个文件的副本，并询问哪些提交修改了文件，以便在提交之前或之后包含了给定的内容。您可以使用以下命令找出：

``` bash
$  git log --raw --abbrev=40 --pretty=oneline |
	grep -B 1 `git hash-object filename`
```

​	为什么这能工作的原因留给（高级）学生作为练习。[git-log[1]](https://chat.openai.com/1/git-log)、[git-diff-tree[1]](https://chat.openai.com/1/git-diff-tree)和[git-hash-object[1]](https://chat.openai.com/1/git-hash-object)的手册可能会对您有所帮助。

## 使用Git进行开发

### 告诉Git您的name

​	在创建任何提交之前，您应该向Git介绍自己。最简单的方法是使用[git-config[1]](https://chat.openai.com/1/git-config)：

``` bash
$ git config --global user.name 'Your Name Comes Here'
$ git config --global user.email 'you@yourdomain.example.com'
```

​	这将在您的主目录中的一个名为`.gitconfig`的文件中添加以下内容：

```
[user]
	name = Your Name Comes Here
	email = you@yourdomain.example.com
```

​	有关配置文件的详细信息，请参阅[git-config[1]](https://chat.openai.com/1/git-config)中的“CONFIGURATION FILE”部分。该文件是纯文本，因此您也可以使用您喜欢的编辑器进行编辑。

### 创建一个新的仓库

​	从头开始创建一个新的仓库非常简单：

``` bash
$ mkdir project
$ cd project
$ git init
```

​	如果有一些初始内容（例如，一个tarball）：

``` bash
$ tar xzvf project.tar.gz
$ cd project
$ git init
$ git add . # include everything below ./ in the first commit:
$ git commit
```

### 如何创建提交

​	创建新的提交需要三个步骤：

1. 使用您喜欢的编辑器对工作目录进行一些更改。
5. 告诉Git您的更改。
6. 使用第2步中告诉Git的内容创建提交。

​	实际上，您可以交错和重复步骤1和2多次：为了跟踪您在步骤3要提交的内容，Git在特殊的暂存区域中维护树内容的快照，该区域称为“索引”。

​	开始时，索引的内容将与HEAD的内容相同。因此，`git diff --cached`命令，在HEAD和索引之间显示的差异，此时应该不会产生输出。

​	修改索引很简单：

​	要使用新文件或修改后的文件内容更新索引，请使用：

``` bash
$ git add path/to/file
```

​	要从索引和工作树中删除文件，请使用：

``` bash
$ git rm path/to/file
```

​	在每个步骤之后，您可以通过验证以下命令：

``` bash
$ git diff --cached
```

始终显示HEAD和索引文件之间的差异，这是您现在将要创建的提交内容，以及

``` bash
$ git diff
```

显示工作树与索引文件之间的差异。

​	请注意，`git add`命令始终只会将文件的当前内容添加到索引；除非再次对文件运行`git add`命令，否则对同一文件的进一步更改将被忽略。

​	当您准备好时，只需运行：

``` bash
$ git commit
```

Git将提示您输入提交消息，然后创建新的提交。您可以使用以下命令验证它是否符合您的期望：

``` bash
$ git show
```

​	作为一个特殊的快捷方式，

``` bash
$ git commit -a
```

将更新索引中的任何已修改或删除的文件并创建一个提交，这一切都在一个步骤中完成。

​	有许多命令可用于跟踪即将提交的内容：

``` bash
$ git diff --cached # difference between HEAD and the index; what
		    # would be committed if you ran "commit" now.
$ git diff	    # difference between the index file and your
		    # working directory; changes that would not
		    # be included if you ran "commit" now.
$ git diff HEAD	    # difference between HEAD and working tree; what
		    # would be committed if you ran "commit -a" now.
$ git status	    # a brief per-file summary of the above.
```

​	您还可以使用[git-gui[1]](https://chat.openai.com/1/git-gui)来创建提交，查看索引和工作目录文件的更改，并在索引中单独选择要包含在提交中的差异块（右键单击差异块并选择“Stage Hunk For Commit”）。

### 创建良好的提交消息

​	虽然不是必需的，但最好的做法是在提交消息的开头用一行简短（少于50个字符）的摘要总结更改，然后是一个空行，然后是更详细的描述。提交消息中第一个空行之前的文本被视为提交标题，并且该标题在整个Git中都被使用。例如，[git-format-patch[1]](https://chat.openai.com/1/git-format-patch)将提交转换为电子邮件，并在主题行中使用标题，在正文中使用提交的其余部分。

### 忽略文件

​	项目通常会生成一些您不希望使用Git跟踪的文件。这通常包括构建过程生成的文件或编辑器生成的临时备份文件。当然，如果您不在这些文件上调用`git add`，它们将*不会*被Git跟踪。但是，保持这些未跟踪的文件存在很快会变得很烦人；例如，它们会使`git add .`几乎无用，并且它们会不断显示在`git status`的输出中。

​	您可以通过在工作目录的顶层创建一个名为`.gitignore`的文件来告诉Git忽略某些文件，其内容如下：

```
# Lines starting with '#' are considered comments.
# Ignore any file named foo.txt.
foo.txt
# Ignore (generated) html files,
*.html
# except foo.html which is maintained by hand.
!foo.html
# Ignore objects and archives.
*.[oa]
```

​	有关语法的详细解释，请参阅[gitignore[5]](https://chat.openai.com/5/gitignore)。您还可以将`.gitignore`文件放置在工作树的其他目录中，它们将适用于这些目录及其子目录。可以像添加其他文件一样将`.gitignore`文件添加到仓库中（只需运行`git add .gitignore`和`git commit`，和通常一样），这对于排除模式（例如与构建输出文件匹配的模式）对于克隆您的仓库的其他用户也是有意义的。

​	如果希望排除模式仅影响某些仓库（而不是给定项目的每个仓库），则可以将它们放入仓库中的文件`.git/info/exclude`，或者放入`core.excludesFile`配置变量指定的任何文件中。一些Git命令还可以直接在命令行上接受排除模式。有关详细信息，请参阅[gitignore[5]](https://chat.openai.com/5/gitignore)。

### 如何合并

​	您可以使用[git-merge[1]](https://chat.openai.com/1/git-merge)将两个分支的开发合并在一起：

``` bash
$ git merge branchname
```

将分支`branchname`中的开发合并到当前分支。

​	合并是通过将`branchname`中的更改与从它们的历史分叉以来在当前分支中的最新提交之前进行的更改进行组合而完成的。当此组合干净地完成时，工作树将被组合结果覆盖，或者当此组合导致冲突时，工作树将被部分合并的结果覆盖。因此，如果您对与合并受影响的文件相同的文件有未提交的更改，Git将拒绝继续执行。大多数情况下，您将在合并之前提交您的更改；如果您不这样做，那么[git-stash[1]](https://chat.openai.com/1/git-stash)可以在您进行合并时拿走这些更改，并在合并后重新应用它们。

​	如果更改足够独立，Git将自动完成合并并提交结果（或在[快进](https://git-scm.com/docs/user-manual#fast-forwards)的情况下重用现有提交，参见下文）。另一方面，如果存在冲突，例如，如果在远程分支和本地分支的相同文件以两种不同的方式进行了修改，那么您将会收到警告；输出可能如下所示：

``` bash
$ git merge next
 100% (4/4) done
Auto-merged file.txt
CONFLICT (content): Merge conflict in file.txt
Automatic merge failed; fix conflicts and then commit the result.
```

​	在有问题的文件中保留冲突标记，然后在手动解决冲突后，您可以使用以下命令更新索引的内容并运行Git提交，就像创建新文件时那样。

​	如果使用gitk检查结果提交，您将看到它有两个父提交，一个指向当前分支的顶部，另一个指向另一个分支的顶部。

### 解决合并冲突

​	当合并不会自动解决时，Git会将索引和工作树保留在一种特殊状态中，以提供您解决合并所需的所有信息。

​	具有冲突的文件在索引中被特别标记，因此在您解决问题并更新索引之前，[git-commit[1]](https://chat.openai.com/1/git-commit)将失败：

``` bash
$ git commit
file.txt: needs merge
```

​	此外，[git-status[1]](https://chat.openai.com/1/git-status)将把这些文件列为“unmerged”，并且具有冲突的文件将添加冲突标记，例如：

```
<<<<<<< HEAD:file.txt
Hello world
=======
Goodbye
>>>>>>> 77976da35a11db4580b80ae27e8d65caf5208086:file.txt
```

​	您需要做的就是编辑这些文件以解决冲突，然后：

``` bash
$ git add file.txt
$ git commit
```

​	请注意，提交消息已经填充了一些关于合并的信息。通常，您可以使用此默认消息，但如果需要，您可以添加自己的附加评论。

​	上述内容是解决简单合并所需了解的全部。但是，Git还提供了更多信息来帮助解决冲突：

#### 在合并过程中获取冲突解决帮助 Getting conflict-resolution help during a merge

​	所有Git能够自动合并的更改已经添加到索引文件中，因此[git-diff[1]](https://chat.openai.com/1/git-diff)仅显示冲突。它使用一种不寻常的语法：

``` bash
$ git diff
diff --cc file.txt
index 802992c,2b60207..0000000
--- a/file.txt
+++ b/file.txt
@@@ -1,1 -1,1 +1,5 @@@
++<<<<<<< HEAD:file.txt
 +Hello world
++=======
+ Goodbye
++>>>>>>> 77976da35a11db4580b80ae27e8d65caf5208086:file.txt
```

​	请注意，解决此冲突后将提交的提交将具有两个父提交，而不是通常的一个：一个父提交将是HEAD，即当前分支的最新提交；另一个父提交将是另一个分支的最新提交，它暂时存储在MERGE_HEAD中。

​	在合并过程中，索引保存每个文件的三个版本。这三个“文件阶段”分别表示文件的不同版本：

``` bash
$ git show :1:file.txt	# the file in a common ancestor of both branches
$ git show :2:file.txt	# the version from HEAD.
$ git show :3:file.txt	# the version from MERGE_HEAD.
```

​	当您要求[git-diff[1]](https://chat.openai.com/1/git-diff)显示冲突时，它会在工作树中运行三方差异，使用第2个和第3个阶段来显示仅来自双方的内容的块（换句话说，当块的合并结果仅来自第2个阶段时，该部分不会产生冲突，也不会显示。与第3个阶段相同）。

​	上面的差异显示了file.txt的工作树版本与第2个和第3个阶段版本之间的差异。因此，不是在每一行前面用单个`+`或`-`表示，它现在使用两列：第一列用于第一个父提交和工作目录副本之间的差异，第二列用于第二个父提交和工作目录副本之间的差异。有关格式的详细信息，请参阅[git-diff-files[1]](https://chat.openai.com/1/git-diff-files)中的“COMBINED DIFF FORMAT”部分。

​	在明显的方式下解决冲突后（但在更新索引之前），差异将如下所示：

``` bash
$ git diff
diff --cc file.txt
index 802992c,2b60207..0000000
--- a/file.txt
+++ b/file.txt
@@@ -1,1 -1,1 +1,1 @@@
- Hello world
 -Goodbye
++Goodbye world
```

​	这显示我们解决的版本从第一个父提交中删除了“Hello world”，从第二个父提交中删除了“Goodbye”，并添加了“Goodbye world”，它之前在两个版本中都不存在。

​	一些特殊的diff选项允许将工作目录与这些阶段中的任何一个进行比较：

``` bash
$ git diff -1 file.txt		# diff against stage 1
$ git diff --base file.txt	# same as the above
$ git diff -2 file.txt		# diff against stage 2
$ git diff --ours file.txt	# same as the above
$ git diff -3 file.txt		# diff against stage 3
$ git diff --theirs file.txt	# same as the above.
```

​	[git-log[1]](https://chat.openai.com/1/git-log)和[gitk[1]](https://chat.openai.com/1/gitk)命令还为合并提供特殊帮助：

``` bash
$ git log --merge
$ gitk --merge
```

​	这将显示仅存在于HEAD或MERGE_HEAD上并涉及未合并文件的所有提交。

​	您还可以使用[git-mergetool[1]](https://chat.openai.com/1/git-mergetool)，它允许您使用外部工具（如Emacs或kdiff3）合并未合并的文件。

​	每次您解决文件中的冲突并更新索引后：

``` bash
$ git add file.txt
```

该文件的不同阶段将被“折叠”，之后`git diff`（默认情况下）将不再显示该文件的差异。

### 撤销合并

​	如果您陷入困境并决定放弃整个合并，您始终可以返回到合并之前的状态：

``` bash
$ git merge --abort
```

​	或者，如果您已经提交了要丢弃的合并：

``` bash
$ git reset --hard ORIG_HEAD
```

​	但是，在某些情况下，这最后一个命令可能会很危险，如果您已经提交了该提交并且该提交本身可能已经合并到另一个分支中，请不要执行此操作，因为这可能会导致进一步的合并混淆。

### 快速前进合并 Fast-forward merges

​	上面没有提到的一种特殊情况会受到不同的对待。通常，合并会生成一个合并提交，其中包含两个父提交，一个指向合并的两个开发线中的每个提交。

​	但是，如果当前分支是另一个分支的祖先——也就是说，当前分支中的每个提交已经包含在另一个分支中——那么Git只会执行“快速前进”；当前分支的头指针被移动到合并分支的头指针，而不会创建任何新的提交。

### 修复错误

​	如果您搞乱了工作树，但尚未提交错误，您可以使用以下命令将整个工作树恢复到上次提交的状态：

``` bash
$ git restore --staged --worktree :/
```

​	如果您做了一个后来希望没有做的提交，有两种基本不同的方式来解决这个问题：

1. 您可以创建一个新的提交，撤销旧提交所做的任何更改。如果您的错误已经公开发表，这是正确的做法。
3. 您可以返回并修改旧提交。如果您已经公开发布历史记录，请永远不要这样做；Git通常不希望项目的“历史记录”发生变化，并且不能正确地从已更改其历史记录的分支执行重复合并。

#### 使用新提交修复错误 Fixing a mistake with a new commit

​	创建一个新提交以撤消先前的更改非常简单；只需将[git-revert[1]](https://chat.openai.com/1/git-revert)命令传递给不良提交的引用。例如，要撤消最近的提交：

``` bash
$ git revert HEAD
```

​	这将创建一个新的提交，该提交将撤消HEAD中的更改。您将有机会编辑新提交的提交消息。

​	您还可以撤消早期的更改，例如，倒数第二个提交：

``` bash
$ git revert HEAD^
```

​	在这种情况下，Git将尝试撤消旧的更改，同时保留自那时以来所做的任何更改。如果较新的更改与要撤消的更改重叠，则将要求您手动解决冲突，就像[解决合并冲突](https://git-scm.com/docs/user-manual#resolving-a-merge)的情况一样。

#### 通过重写历史记录来修复错误 Fixing a mistake by rewriting history

​	如果有问题的提交是最近的提交，并且您尚未公开该提交，那么您可以使用`git reset`命令[摧毁该提交](https://git-scm.com/docs/user-manual#undoing-a-merge)。

​	或者，您可以编辑工作目录并更新索引来修复错误，就像您要[创建一个新提交](https://git-scm.com/docs/user-manual#how-to-make-a-commit)一样，然后运行以下命令：

``` bash
$ git commit --amend
```

这将通过包含您的更改替换旧提交，您将有机会首先编辑旧提交消息。

​	再次强调，在可能已经合并到另一个分支中的提交上永远不要执行此操作；在这种情况下，请改用[git-revert[1]](https://chat.openai.com/1/git-revert)。

​	还可以替换历史记录中更早的提交，但这是一个需要留给[另一章节](https://git-scm.com/docs/user-manual#cleaning-up-history)处理的高级主题。

#### 检出文件的旧版本 Checking out an old version of a file

​	在撤消之前的错误更改的过程中，您可能会发现使用[git-restore[1]](https://chat.openai.com/1/git-restore)来检出特定文件的旧版本很有用。该命令如下：

``` bash
$ git restore --source=HEAD^ path/to/file
```

这将用HEAD^提交中的内容替换path/to/file，并更新索引以匹配。它不会改变分支。

​	如果您只是想查看文件的旧版本，而不修改工作目录，可以使用[git-show[1]](https://chat.openai.com/1/git-show)实现：

``` bash
$ git show HEAD^:path/to/file
```

它将显示给定版本的文件内容。

#### 暂时搁置进行中的工作 Temporarily setting aside work in progress

​	当您正在处理一些复杂的工作时，您可能会发现一个无关但显而易见和琐碎的错误。您希望在继续工作之前先修复它。您可以使用[git-stash[1]](https://chat.openai.com/1/git-stash)来保存当前工作状态，在修复错误后（或者在不同的分支上进行修复，然后再返回），恢复进行中的工作更改。

``` bash
$ git stash push -m "work in progress for foo feature"
```

​	这个命令将把您的更改保存到`stash`，并将您的工作树和索引重置为与当前分支的最新提交匹配。然后您可以像往常一样进行修复。

```
... edit and test ...
$ git commit -a -m "blorpl: typofix"
```

​	之后，您可以使用`git stash pop`回到您之前进行的工作：

``` bash
$ git stash pop
```

### 确保良好的性能

​	在大型仓库中，Git依赖压缩来确保历史信息在磁盘或内存中不占用太多空间。一些Git命令可能会自动运行[git-gc[1]](https://chat.openai.com/1/git-gc)，因此您不必手动运行它。但是，压缩大型仓库可能需要一些时间，因此您可能希望显式调用`gc`，以避免在不方便的时候自动触发压缩。

### 确保可靠性

#### 检查仓库是否损坏

​	[git-fsck[1]](https://chat.openai.com/1/git-fsck)命令对仓库运行多个自一致性检查，并报告任何问题。这可能需要一些时间。

``` bash
$ git fsck
dangling commit 7281251ddd2a61e38657c827739c57015671a6b3
dangling commit 2706a059f258c6b245f298dc4ff2ccd30ec21a63
dangling commit 13472b7c4b80851a1bc551779171dcb03655e9b5
dangling blob 218761f9d90712d37a9c5e36f406f92202db07eb
dangling commit bf093535a34a4d35731aa2bd90fe6b176302f14f
dangling commit 8e4bec7f2ddaa268bef999853c25755452100f8e
dangling tree d50bb86186bf27b681d25af89d3b5b68382e4085
dangling tree b24c2473f1fd3d91352a624795be026d64c8841f
...
```

​	您将看到关于悬空对象的信息。它们是仍然存在于仓库中但不再由您的任何分支引用的对象，它们可能会在一段时间后通过`gc`被移除。您可以运行`git fsck --no-dangling`来抑制这些消息，并查看实际的错误。

#### 恢复丢失的更改

##### Reflogs

​	假设您使用[`git reset --hard`](https://git-scm.com/docs/user-manual#fixing-mistakes)修改了一个分支，然后意识到该分支是您在历史中唯一的引用。

​	幸运的是，Git还保存了一个称为"reflog"的日志，记录了每个分支的所有先前值。因此，在这种情况下，您仍然可以使用以下方式找到旧的历史记录：

``` bash
$ git log master@{1}
```

​	这会列出可从`master`分支上一个版本的提交中访问的提交。此语法可用于接受提交的任何Git命令，不仅限于`git log`。其他一些示例：

``` bash
$ git show master@{2}		# See where the branch pointed 2,
$ git show master@{3}		# 3, ... changes ago.
$ gitk master@{yesterday}	# See where it pointed yesterday,
$ gitk master@{"1 week ago"}	# ... or last week
$ git log --walk-reflogs master	# show reflog entries for master
```

​	HEAD也保留了单独的reflog，因此

``` bash
$ git show HEAD@{"1 week ago"}
```

将显示HEAD一周前指向的地方，而不是当前分支一周前指向的地方。这允许您查看您检出的内容的历史。

​	默认情况下，reflogs保留30天，之后它们可能会被修剪。查看[git-reflog[1]](https://chat.openai.com/1/git-reflog)和[git-gc[1]](https://chat.openai.com/1/git-gc)以了解如何控制此修剪，以及在[gitrevisions[7]](https://chat.openai.com/7/gitrevisions)的“指定修订”部分中了解详情。

​	请注意，reflog历史与普通的Git历史非常不同。尽管普通历史由在同一项目上工作的每个仓库共享，但reflog历史不是共享的：它只告诉您有关本地仓库中分支的更改方式。

##### 检查悬空对象 Examining dangling objects

​	在某些情况下，reflog可能无法拯救您。例如，假设您删除了一个分支，然后意识到您需要其中包含的历史。reflog也被删除；然而，如果您尚未对仓库进行修剪，那么您仍然可以在`git fsck`报告的悬空对象中找到丢失的提交。有关详细信息，请参阅[Dangling objects](https://git-scm.com/docs/user-manual#dangling-objects)。

``` bash
$ git fsck
dangling commit 7281251ddd2a61e38657c827739c57015671a6b3
dangling commit 2706a059f258c6b245f298dc4ff2ccd30ec21a63
dangling commit 13472b7c4b80851a1bc551779171dcb03655e9b5
...
```

​	您可以使用以下方式检查其中一个悬空提交：

``` bash
$ gitk 7281251ddd --not --all
```

这听起来就是它的作用：它表示您希望查看由悬空提交描述的提交历史，但不包括所有现有分支和标签所描述的历史。因此，您得到的是可以访问丢失的提交的历史。(请注意，可能不止一个提交：我们仅报告“线的尖端”是悬空的，但可能存在一个完整的深度和复杂的提交历史被丢弃。)

​	如果您决定要还原历史记录，您可以随时创建一个指向它的新引用，例如，新的分支：

``` bash
$ git branch recovered-branch 7281251ddd
```

​	其他类型的悬空对象（blobs和trees）也是可能的，并且悬空对象可能在其他情况下出现。

## 与他人共享开发

### 使用`git pull`获取更新

​	在克隆了一个仓库并提交了一些您自己的更改后，您可能希望检查原始仓库是否有更新，并将其合并到您自己的工作中。

​	我们已经了解了[如何保持远程跟踪分支更新](https://git-scm.com/docs/user-manual#Updating-a-repository-With-git-fetch)（使用[git-fetch[1]](https://chat.openai.com/1/git-fetch)）以及如何合并两个分支。因此，您可以将来自原始仓库的更改合并到您的仓库的主分支中：

``` bash
$ git fetch
$ git merge origin/master
```

​	然而，[git-pull[1]](https://chat.openai.com/1/git-pull)命令提供了一种一步完成此操作的方法：

``` bash
$ git pull origin master
```

​	实际上，如果您已经检出了`master`分支，那么这个分支已经由`git clone`配置为从原始仓库的HEAD分支获取更改。因此，通常您可以只需使用一个简单的命令完成上面的操作：

``` bash
$ git pull
```

​	这个命令将获取远程分支的更改并合并到您的远程跟踪分支`origin/*`，然后将默认分支合并到当前分支。

​	更一般地，从远程跟踪分支创建的分支将默认从该分支进行拉取。请参阅[git-config[1]](https://chat.openai.com/1/git-config)中`branch.<name>.remote`和`branch.<name>.merge`选项的描述，以及[git-checkout[1]](https://chat.openai.com/1/git-checkout)中关于`--track`选项的讨论，以了解如何控制这些默认设置。

​	除了节省您的击键外，`git pull`还通过生成默认的提交消息来帮助您记录您从哪个分支和仓库拉取的更改。

（但请注意，在[快进合并](https://git-scm.com/docs/user-manual#fast-forwards)的情况下，将不会创建此类提交；相反，您的分支将只是更新为指向上游分支的最新提交。）

​	`git pull`命令还可以将`.`作为“远程”仓库，这样它只会合并当前仓库中的一个分支；因此，以下命令是大致等效的：

``` bash
$ git pull . branch
$ git merge branch
```

### 提交补丁给项目

​	如果您只有少量更改，最简单的提交方式可能是通过电子邮件发送它们作为补丁：

​	首先，使用[git-format-patch[1]](https://chat.openai.com/1/git-format-patch)命令；例如：

``` bash
$ git format-patch origin
```

将在当前目录中生成一系列编号的文件，每个文件对应当前分支中的一个补丁，但不包含在`origin/HEAD`中的补丁。

​	`git format-patch`可以包括一个初始的“封面信件”。您可以在提交消息后三个破折号（`---`）之后但在补丁之前插入对每个补丁的评论。如果使用`git notes`跟踪您的封面信件内容，`git format-patch --notes`将以类似的方式包括提交的备注。

​	然后，您可以将这些补丁导入到您的邮件客户端中，并手动发送它们。然而，如果您需要一次发送大量补丁，您可能更喜欢使用[git-send-email[1]](https://chat.openai.com/1/git-send-email)脚本来自动化此过程。首先请咨询您的项目邮件列表，以确定他们提交补丁的要求。

### 导入补丁到项目

​	Git还提供了一个名为[git-am[1]](https://chat.openai.com/1/git-am)（am代表"apply mailbox"）的工具，用于导入这样一系列通过电子邮件发送的补丁。只需将所有包含补丁的消息按顺序保存到一个单独的邮箱文件中，例如`patches.mbox`，然后运行：

``` bash
$ git am -3 patches.mbox
```

​	Git将按顺序应用每个补丁；如果发现任何冲突，它将停止，并且您可以按照"[解决合并冲突](https://git-scm.com/docs/user-manual#resolving-a-merge)"中描述的方式解决冲突。（`-3`选项告诉Git执行合并；如果您希望它只是中止并保持您的树和索引不变，您可以省略该选项。）

​	一旦索引更新了冲突解决的结果，而不是创建一个新的提交，只需运行：

``` bash
$ git am --continue
```

Git将为您创建提交，并继续从邮箱中应用剩余的补丁。

​	最终的结果将是一系列提交，每个提交对应于原始邮箱中的一个补丁，其中作者和提交日志消息分别来自包含每个补丁的消息。

### 公共Git仓库

​	另一种向项目提交更改的方法是告诉该项目的维护者从您的仓库中拉取更改，使用[git-pull[1]](https://chat.openai.com/1/git-pull)。在"[使用`git pull`获取更新](https://git-scm.com/docs/user-manual#getting-updates-With-git-pull)"一节中，我们将其描述为从“主”仓库获取更新的方法，但在另一个方向上同样有效。

​	如果您和维护者在同一台机器上都有账号，那么您可以直接从彼此的仓库中拉取更改；接受仓库URL作为参数的命令也将接受一个本地目录名称：

``` bash
$ git clone /path/to/repository
$ git pull /path/to/other/repository
```

或者使用ssh URL：

``` bash
$ git clone ssh://yourhost/~you/repository
```

​	对于开发者较少的项目，或用于同步几个私有仓库，这可能已经足够。

​	然而，更常见的方法是为他人提供一个单独的公共仓库（通常在不同的主机上），供他们从中拉取更改。这通常更方便，可以使您将私有的工作进度与公开可见的工作清晰地分开。

​	您将继续在您的个人仓库中进行日常工作，但会定期将从个人仓库推送的更改转移到公共仓库中，让其他开发者从那个仓库拉取。因此，在只有一个其他开发者拥有公共仓库的情况下，更改的流程如下：

```
		      you push
your personal repo ------------------> your public repo
      ^                                     |
      |                                     |
      | you pull                            | they pull
      |                                     |
      |                                     |
      |               they push             V
their public repo <------------------- their repo
```

​	我们将在接下来的章节中解释如何做到这一点。

### 设置公共仓库 Setting up a public repository

​	假设您的个人仓库位于目录`~/proj`。我们首先创建仓库的一个新克隆，并告知`git daemon`它是公共仓库：

``` bash
$ git clone --bare ~/proj proj.git
$ touch proj.git/git-daemon-export-ok
```

​	生成的`proj.git`目录包含一个“裸”（bare）的git仓库——它只是`.git`目录的内容，没有围绕它的任何文件被检出。

​	接下来，将`proj.git`复制到您计划托管公共仓库的服务器上。您可以使用scp、rsync或任何最方便的方法。

### 通过Git协议导出Git仓库 Exporting a Git repository via the Git protocol

​	这是首选的方法。

​	如果其他人管理服务器，他们应该告诉您在哪个目录下放置仓库，以及仓库在`git://`URL下的位置。然后，您可以跳到下面的章节"[向公共仓库推送更改](https://git-scm.com/docs/user-manual#pushing-changes-to-a-public-repository)"。

​	否则，您只需要启动[git-daemon[1]](https://chat.openai.com/1/git-daemon)；它将监听9418端口。默认情况下，它允许访问任何看起来像Git仓库并包含魔法文件`git-daemon-export-ok`的目录。通过将一些目录路径作为`git daemon`的参数，可以进一步限制导出到这些路径。

​	您也可以将`git daemon`作为inetd服务运行；请参阅[git-daemon[1]](https://chat.openai.com/1/git-daemon)手册中的详细信息。（尤其是例子部分。）

### 通过HTTP导出Git仓库 

​	Git协议提供更好的性能和可靠性，但在配置了Web服务器的主机上，使用HTTP导出可能更简单。

​	您只需将新创建的裸Git仓库放置在由Web服务器导出的目录中，并进行一些调整以向Web客户端提供一些额外的所需信息：

``` bash
$ mv proj.git /home/you/public_html/proj.git
$ cd proj.git
$ git --bare update-server-info
$ mv hooks/post-update.sample hooks/post-update
```

（有关最后两行的解释，请参阅[git-update-server-info[1]](https://chat.openai.com/1/git-update-server-info)和[githooks[5]](https://chat.openai.com/5/githooks)。）

​	公布`proj.git`的URL。其他任何人随后都能使用命令行克隆或拉取：

``` bash
$ git clone http://yourserver.com/~you/proj.git
```

（对于稍微复杂一些的使用WebDAV的设置，也允许通过HTTP推送，请参阅[setup-git-server-over-http](https://git-scm.com/docs/howto/setup-git-server-over-http)。）

### 向公共仓库推送更改

​	请注意，上述两种方法（通过[http](https://git-scm.com/docs/user-manual#exporting-via-http)或[git](https://git-scm.com/docs/user-manual#exporting-via-git)导出）允许其他维护者获取您的最新更改，但不允许写入访问权限，而您需要这样的权限来将私有仓库中创建的最新更改更新到公共仓库。

​	最简单的方法是使用[git-push[1]](https://chat.openai.com/1/git-push)和SSH来实现；要更新远程分支`master`为您的`master`分支的最新状态，请运行：

``` bash
$ git push ssh://yourserver.com/~you/proj.git master:master
```

或者只需执行：

``` bash
$ git push ssh://yourserver.com/~you/proj.git master
```

​	与`git fetch`类似，如果这不会导致[fast-forward合并](https://git-scm.com/docs/user-manual#fast-forwards)，`git push`会报错；有关处理此情况的详细信息，请参见下一节。

​	请注意，`push`的目标通常是[裸仓库](https://git-scm.com/docs/user-manual#def_bare_repository)。您也可以推送到一个已检出工作树的仓库，但默认情况下，不允许推送更新当前已检出分支，以防止混淆。有关详细信息，请参阅[git-config[1]](https://chat.openai.com/1/git-config)中`receive.denyCurrentBranch`选项的描述。

​	与`git fetch`一样，您还可以设置配置选项以节省输入；例如：

``` bash
$ git remote add public-repo ssh://yourserver.com/~you/proj.git
```

将以下内容添加到`.git/config`：

```
[remote "public-repo"]
	url = yourserver.com:proj.git
	fetch = +refs/heads/*:refs/remotes/example/*
```

这使您可以仅使用以下命令进行推送：

``` bash
$ git push public-repo master
```

​	有关详细信息，请参见[git-config[1]](https://chat.openai.com/1/git-config)中`remote.<name>.url`、`branch.<name>.remote`和`remote.<name>.push`选项的解释。

### 当推送失败时怎么办

​	如果推送不会导致[fast-forward合并](https://git-scm.com/docs/user-manual#fast-forwards)远程分支，则会失败，并显示错误消息，例如：

```
 ! [rejected]        master -> master (non-fast-forward)
error: failed to push some refs to '...'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

​	这可能发生在以下情况下：

- 使用`git reset --hard`删除已发布的提交，或者
- 使用`git commit --amend`替换已发布的提交（如[通过重写历史来修正错误](https://git-scm.com/docs/user-manual#fixing-a-mistake-by-rewriting-history)所示），或者
- 使用`git rebase`重新基于任何已发布的提交（如[使用git rebase保持补丁系列最新](https://git-scm.com/docs/user-manual#using-git-rebase)所示）。

​	您可以通过在分支名称前加上加号来强制`git push`进行更新：

``` bash
$ git push ssh://yourserver.com/~you/proj.git +master
```

​	请注意加号的添加。或者，您可以使用`-f`标志来强制远程更新，如下所示：

``` bash
$ git push -f ssh://yourserver.com/~you/proj.git master
```

​	通常，当公共仓库中的分支头被修改时，它将被修改为指向之前所指向的提交的后代。在这种情况下强制推送，将打破这一约定。（参见[重写历史可能带来的问题](https://git-scm.com/docs/user-manual#problems-With-rewriting-history)。）

​	然而，对于需要简单地发布正在进行的补丁系列的人来说，这是一种常见的做法，只要您警告其他开发者，这是您打算管理分支的方式，这是可以接受的妥协。

​	当多个人有权推送到同一仓库时，推送可能以这种方式失败。在这种情况下，正确的解决方法是在尝试推送之前先更新您的工作：通过拉取或通过获取和变基。有关更多信息，请参见[下一节](https://git-scm.com/docs/user-manual#setting-up-a-shared-repository)和[gitcvs-migration[7]](https://chat.openai.com/7/gitcvs-migration)。

### 设置共享仓库 Setting up a shared repository

​	另一种合作的方式是使用与CVS中常用的模型类似的模型，其中拥有特殊权限的几个开发者都推送和拉取共享的单一仓库。有关如何设置这样的模型，请参阅[gitcvs-migration[7]](https://chat.openai.com/7/gitcvs-migration)。

​	然而，虽然Git对共享仓库提供了支持，但一般不建议采用这种操作方式，因为Git支持的协作方式——通过交换补丁和从公共仓库拉取——相对于中央共享仓库具有诸多优势：

- Git能够快速导入和合并补丁，使单个维护者可以处理非常高速率下的传入更改。当工作量变得过大时，`git pull`提供了一种简单的方式，使维护者可以将这项工作委派给其他维护者，同时仍允许对传入更改进行可选的审查。
- 由于每个开发者的仓库都拥有项目历史的完整副本，没有仓库是特殊的，其他开发者很容易通过相互协议或者在维护者不响应或难以合作的情况下接管项目的维护。
- 缺少中央“提交者”组意味着对于谁“在内”和谁“在外”没有太多正式的决定需求。

### 允许web浏览仓库

​	gitweb CGI脚本为用户提供了一种无需安装Git即可浏览项目修订、文件内容和日志的简便方式。可选择启用RSS/Atom源和责备/注释详细信息等功能。

​	使用[git-instaweb[1]](https://chat.openai.com/1/git-instaweb)命令可以简单地启动使用gitweb浏览仓库。使用instaweb时，默认服务器是lighttpd。

​	有关在Git源代码树中设置永久安装与CGI或Perl可用服务器的详细说明，请参阅gitweb/INSTALL文件和[gitweb[1]](https://chat.openai.com/1/gitweb)。

### 如何获取一个最小历史记录的Git仓库 

​	截断历史记录的[shallow clone](https://git-scm.com/docs/user-manual#def_shallow_clone)在只对项目的最近历史记录感兴趣且从上游获取完整历史记录昂贵时非常有用。

​	通过指定[git-clone[1]](https://chat.openai.com/1/git-clone)的`--depth`开关来创建[shallow clone](https://git-scm.com/docs/user-manual#def_shallow_clone)。深度可以在后续使用[git-fetch[1]](https://chat.openai.com/1/git-fetch)的`--depth`开关进行更改，或者通过`--unshallow`恢复完整历史。

​	在[shallow clone](https://git-scm.com/docs/user-manual#def_shallow_clone)中进行合并，只要合并基在最近的历史中就可以。否则，它将类似于合并不相关的历史，可能导致巨大的冲突。这个限制可能使得这样的仓库在基于合并的工作流中不适合使用。

### 示例

#### 维护Linux子系统的主题分支

​	这描述了Tony Luck在Linux内核的IA64架构维护者角色中如何使用Git。

​	他使用两个公共分支：

- “测试”树，最初将补丁放置其中，以便在与其他正在进行的开发集成时可以得到一些曝光。这个树对于Andrew来说是可以随时从-mm拉取的。
- “发布”树，其中放置经过测试的补丁，用于进行最终的健全性检查，并且作为将它们发送给Linus的工具（通过向他发送一个“请拉取”请求）。

​	他还使用一组临时分支（“主题分支”），每个分支包含一组逻辑上相关的补丁。

​	要设置这个，首先通过克隆Linus的公共树来创建您的工作树：

```bash
$ git clone git://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git work
$ cd work
```

​	Linus的树将存储在名为origin/master的远程跟踪分支中，并且可以使用[git-fetch[1]](https://chat.openai.com/1/git-fetch)进行更新；您可以使用[git-remote[1]](https://chat.openai.com/1/git-remote)跟踪其他公共树来设置一个“remote”，并且使用[git-fetch[1]](https://chat.openai.com/1/git-fetch)来保持它们最新。有关详细信息，请参阅[仓库和分支](https://git-scm.com/docs/user-manual#repositories-and-branches)。

​	现在创建要使用的分支；这些分支从origin/master分支的当前尖端开始，并且应该设置为默认合并Linus的更改（使用[git-branch[1]](https://chat.openai.com/1/git-branch)的`--track`选项）。

``` bash
$ git branch --track test origin/master
$ git branch --track release origin/master
```

​	这些可以通过[git-pull[1]](https://chat.openai.com/1/git-pull)轻松保持更新。

``` bash
$ git switch test && git pull
$ git switch release && git pull
```

​	重要提示！如果这些分支中有任何本地更改，那么合并将在历史记录中创建一个提交对象（如果没有本地更改，Git将执行“快进”合并）。许多人不喜欢这在Linux历史中创建的“噪音”，因此您应该避免在`release`分支中随意执行此操作，因为这些嘈杂的提交将成为向Linus请求从发布分支拉取时永久历史记录的一部分。

​	一些配置变量（请参阅[git-config[1]](https://chat.openai.com/1/git-config)）可以使将这两个分支都推送到您的公共树变得容易。（请参阅[设置公共仓库](https://git-scm.com/docs/user-manual#setting-up-a-public-repository)。）

``` bash
$ cat >> .git/config <<EOF
[remote "mytree"]
	url =  master.kernel.org:/pub/scm/linux/kernel/git/aegl/linux.git
	push = release
	push = test
EOF
```

​	然后，您可以使用[git-push[1]](https://chat.openai.com/1/git-push)推送测试和发布树：

``` bash
$ git push mytree
```

​	或者只推送其中一个测试或发布分支：

``` bash
$ git push mytree test
```

或者

``` bash
$ git push mytree release
```

​	现在，要应用来自社区的一些补丁，请为保存这个补丁（或相关的一组补丁）的分支命名，并从Linus的分支的最近稳定标签创建一个新的分支。选择一个稳定的基准作为您的分支将有助于：1）帮助您：避免包含不相关的、可能经过轻度测试的更改；2）帮助未来的错误猎人使用`git bisect`来查找问题。

``` bash
$ git switch -c speed-up-spinlocks v2.6.35
```

​	然后应用补丁（或补丁系列），运行一些测试，并提交更改。如果补丁是一个多部分系列，则应将每个部分作为单独的提交应用到此分支。

``` bash
$ ... patch ... test  ... commit [ ... patch ... test ... commit ]*
```

​	当您对此更改的状态感到满意时，可以合并到“测试”分支，以准备将其公开：

``` bash
$ git switch test && git merge speed-up-spinlocks
```

​	在这里，您很可能不会遇到任何冲突……但是，如果您在此步骤上花费了一些时间，并且还拉取了来自上游的新版本，那么可能会有冲突。

​	稍后，当足够的时间过去并完成了测试，您可以将同一个分支拉入“发布”树，准备好发送给上游。这是您看到在每个补丁（或补丁系列）都保持在自己的分支中的价值所在。这意味着补丁可以以任何顺序移动到“发布”树中。

``` bash
$ git switch release && git merge speed-up-spinlocks
```

​	过了一段时间后，您将拥有多个分支，并且尽管您为每个分支选择了精心挑选的名称，您可能会忘记它们的用途或状态。为了获得对特定分支所做更改的提醒，请使用：

``` bash
$ git log linux..branchname | git shortlog
```

​	要查看它是否已经合并到测试或发布分支中，请使用：

``` bash
$ git log test..branchname
```

或者

``` bash
$ git log release..branchname
```

（如果该分支尚未合并，您将看到一些日志条目。如果已经合并，那么将不会有输出。）

​	一旦一个补丁完成了整个循环（从测试到发布，然后由Linus拉取，最后回到您的本地origin/master分支），则不再需要该更改的分支。当您从以下输出检测到这一点时：

``` bash
$ git log origin..branchname
```

为空时，表示可以删除该分支：

``` bash
$ git branch -d branchname
```

​	有些更改非常微小，不需要创建单独的分支，然后将其合并到测试和发布分支中。对于这些更改，直接应用到“发布”分支，然后将其合并到“测试”分支中。

​	在将您的工作推送到“mytree”之后，您可以使用[git-request-pull[1]](https://chat.openai.com/1/git-request-pull)准备一个“请拉取”请求消息，然后发送给Linus：

``` bash
$ git push mytree
$ git request-pull origin mytree release
```

​	这里还有一些简化所有这些过程的脚本。

```
==== update script ====
# Update a branch in my Git tree.  If the branch to be updated
# is origin, then pull from kernel.org.  Otherwise merge
# origin/master branch into test|release branch

case "$1" in
test|release)
	git checkout $1 && git pull . origin
	;;
origin)
	before=$(git rev-parse refs/remotes/origin/master)
	git fetch origin
	after=$(git rev-parse refs/remotes/origin/master)
	if [ $before != $after ]
	then
		git log $before..$after | git shortlog
	fi
	;;
*)
	echo "usage: $0 origin|test|release" 1>&2
	exit 1
	;;
esac
==== merge script ====
# Merge a branch into either the test or release branch

pname=$0

usage()
{
	echo "usage: $pname branch test|release" 1>&2
	exit 1
}

git show-ref -q --verify -- refs/heads/"$1" || {
	echo "Can't see branch <$1>" 1>&2
	usage
}

case "$2" in
test|release)
	if [ $(git log $2..$1 | wc -c) -eq 0 ]
	then
		echo $1 already merged into $2 1>&2
		exit 1
	fi
	git checkout $2 && git pull . $1
	;;
*)
	usage
	;;
esac
==== status script ====
# report on status of my ia64 Git tree

gb=$(tput setab 2)
rb=$(tput setab 1)
restore=$(tput setab 9)

if [ `git rev-list test..release | wc -c` -gt 0 ]
then
	echo $rb Warning: commits in release that are not in test $restore
	git log test..release
fi

for branch in `git show-ref --heads | sed 's|^.*/||'`
do
	if [ $branch = test -o $branch = release ]
	then
		continue
	fi

	echo -n $gb ======= $branch ====== $restore " "
	status=
	for ref in test release origin/master
	do
		if [ `git rev-list $ref..$branch | wc -c` -gt 0 ]
		then
			status=$status${ref:0:1}
		fi
	done
	case $status in
	trl)
		echo $rb Need to pull into test $restore
		;;
	rl)
		echo "In test"
		;;
	l)
		echo "Waiting for linus"
		;;
	"")
		echo $rb All done $restore
		;;
	*)
		echo $rb "<$status>" $restore
		;;
	esac
	git log origin/master..$branch | git shortlog
done
```

## 重写历史记录和维护补丁系列

​	通常情况下，对于一个项目来说，提交的修改只会增加，不会删除或替换。Git是根据这个假设设计的，违反这个假设会导致Git的合并机制（例如）出现问题。

​	然而，在某些情况下，违反这个假设可能是有用的。

### 创建完美的补丁系列

​	假设您是一个大型项目的贡献者，并且想要添加一个复杂的功能，并以一种使其他开发者能够轻松阅读您的更改、验证其正确性并理解您为何进行每个更改的方式呈现给他们。

​	如果您将所有更改都呈现为单个补丁（或提交），他们可能会发现一次性消化太多内容。

​	如果您向他们展示整个工作历史，包括错误、修正和死胡同，他们可能会不知所措。

​	因此，通常的做法是生成一系列补丁，满足以下条件：

1. 每个补丁可以按顺序应用。
3. 每个补丁包含一个单一的逻辑更改，并附带一个解释此更改的消息。
5. 没有补丁引入回归问题：在应用了系列的任何初始部分后，生成的项目仍然可以编译和运行，并且没有之前没有的错误。
7. 完整的补丁系列产生的结果与您自己（可能是混乱得多的）开发过程产生的结果相同。

​	我们将介绍一些工具，这些工具可以帮助您做到这一点，并解释如何使用它们，然后解释因为您正在重写历史而可能出现的一些问题。

### 使用git rebase保持补丁系列最新

​	假设您在一个远程跟踪分支`origin`上创建了一个分支`mywork`，并在其上创建了一些提交：

``` bash
$ git switch -c mywork origin
$ vi file.txt
$ git commit
$ vi otherfile.txt
$ git commit
...
```

​	您还没有将任何提交合并到mywork中，因此它只是在`origin`之上的一个简单线性序列的补丁：

```
 o--o--O <-- origin
        \
	 a--b--c <-- mywork
```

​	在上游项目中进行了一些更有趣的工作，`origin`已经前进了：

```
 o--o--O--o--o--o <-- origin
        \
         a--b--c <-- mywork
```

​	此时，您可以使用`pull`来合并您的更改；结果将创建一个新的合并提交，如下所示：

```
 o--o--O--o--o--o <-- origin
        \        \
         a--b--c--m <-- mywork
```

​	但是，如果您更喜欢在mywork中保持历史记录为一系列简单的提交而没有合并，您可以选择使用[git-rebase[1]](https://chat.openai.com/1/git-rebase)：

``` bash
$ git switch mywork
$ git rebase origin
```

​	这将从mywork中移除每个提交，并临时将它们保存为补丁（保存在名为`.git/rebase-apply`的目录中），然后更新mywork以指向origin的最新版本，然后将每个保存的补丁应用到新的mywork中。结果将如下所示：

```
 o--o--O--o--o--o <-- origin
		 \
		  a'--b'--c' <-- mywork
```

​	在此过程中，可能会发现冲突。在这种情况下，它会停止，并允许您解决冲突；解决冲突后，使用`git add`将索引更新为这些内容，然后，而不是运行`git commit`，只需运行：

``` bash
$ git rebase --continue
```

Git将继续应用其余的补丁。

​	您可以随时使用`--abort`选项中止此过程，并将mywork恢复到在开始rebase之前的状态：

``` bash
$ git rebase --abort
```

​	如果需要重新排序或编辑分支中的多个提交，可以使用`git rebase -i`更容易地实现，它允许您重新排序和压缩提交，并在rebase期间将其标记为个别编辑。有关详细信息，请参阅[Using interactive rebases](https://git-scm.com/docs/user-manual#interactive-rebase)以及[Reordering or selecting from a patch series](https://git-scm.com/docs/user-manual#reordering-patch-series)。

### 重写单个提交

​	我们在[修正错误通过重写历史记录](https://git-scm.com/docs/user-manual#fixing-a-mistake-by-rewriting-history)中看到，您可以使用以下命令替换最近的提交：

``` bash
$ git commit --amend
```

这将使用包含您的更改的新提交替换旧的提交，首先为旧的提交消息提供编辑的机会。这对于修复最后一次提交中的拼写错误或调整错误暂存提交的补丁内容很有用。

​	如果您需要修改较早的提交，可以使用[交互式rebase的`edit`指令](https://git-scm.com/docs/user-manual#interactive-rebase)。

### 重新排序或选择补丁系列

​	有时候您想编辑更早的提交。一种方法是使用`git format-patch`创建一系列补丁，然后将状态重置为补丁之前：

``` bash
$ git format-patch origin
$ git reset --hard origin
```

​	然后根据需要修改、重新排序或删除补丁，再使用[git-am[1]](https://chat.openai.com/1/git-am)将其再次应用：

``` bash
$ git am *.patch
```

### 使用交互式rebase

​	您还可以使用交互式rebase编辑补丁系列。这与[使用`format-patch`重新排序补丁系列](https://git-scm.com/docs/user-manual#reordering-patch-series)相同，因此请使用您最喜欢的界面。

​	将当前HEAD重新base到您想要保留不变的最后一个提交。例如，如果您想要重新排序最后5个提交，使用：

``` bash
$ git rebase -i HEAD~5
```

​	这将使用一个列表打开编辑器，其中列出了执行重新base所需的步骤。

```
pick deadbee The oneline of this commit
pick fa1afe1 The oneline of the next commit
...

# Rebase c0ffeee..deadbee onto c0ffeee
#
# Commands:
#  p, pick = use commit
#  r, reword = use commit, but edit the commit message
#  e, edit = use commit, but stop for amending
#  s, squash = use commit, but meld into previous commit
#  f, fixup = like "squash", but discard this commit's log message
#  x, exec = run command (the rest of the line) using shell
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
# Note that empty commits are commented out
```

​	如注释中所解释的，您可以通过编辑列表来重新排序提交、将它们压缩在一起、编辑提交消息等。一旦满意，保存列表并关闭编辑器，重新base就会开始。

​	当将`pick`替换为`edit`，或者列表中的步骤无法机械地解决冲突并需要您的帮助时，重新base将停止。编辑和/或解决冲突后，您可以继续进行`git rebase --continue`。如果觉得情况变得太复杂，您随时可以通过`git rebase --abort`退出。即使在rebase完成后，您仍然可以通过使用[reflog](https://git-scm.com/docs/user-manual#reflogs)来恢复原始分支。

​	有关该过程和其他提示的更详细讨论，请参阅[git-rebase[1]](https://chat.openai.com/1/git-rebase)的“INTERACTIVE MODE”部分。

### 其他工具

​	还有许多其他工具，比如StGit，用于维护补丁系列。这些超出了本手册的范围。

### 重写历史的问题

​	重写分支的历史的主要问题与合并有关。假设某人拉取了您的分支，并将其合并到他们的分支中，结果可能如下所示：

```
 o--o--O--o--o--o <-- origin
        \        \
         t--t--t--m <-- their branch:
```

​	然后，假设您修改了最后三个提交：

```
	 o--o--o <-- new head of origin
	/
 o--o--O--o--o--o <-- old head of origin
```

​	如果我们在一个仓库中检查所有这些历史，它将看起来像：

```
	 o--o--o <-- new head of origin
	/
 o--o--O--o--o--o <-- old head of origin
        \        \
         t--t--t--m <-- their branch:
```

​	Git无法知道新HEAD是旧HEAD的更新版本；它会将此情况与两个开发者在旧HEAD和新HEAD上独立进行工作时完全相同处理。在这一点上，如果有人试图将新HEAD合并到他们的分支中，Git将尝试合并两条（旧和新的）开发线，而不是尝试用新的替换旧的。结果可能出人意料。

​	您仍然可以选择发布其历史被重写的分支，并且其他人可以将这些分支fetch下来以便查看或测试，但是他们不应该尝试将这些分支pull到他们自己的工作中。

​	对于真正支持正确合并的分布式开发，发布的分支不应该被重写。

### 为什么bisect合并提交可能比bisect线性历史更困难

​	[git-bisect[1]](https://chat.openai.com/1/git-bisect)命令可以正确处理包含合并提交的历史。然而，当它找到的提交是一个合并提交时，用户可能需要比通常更努力地弄清楚为什么该提交引入了问题。

​	假设有以下历史：

```
      ---Z---o---X---...---o---A---C---D
          \                       /
           o---o---Y---...---o---B
```

​	假设在开发的上行中，在提交X处更改了Z处存在的一个函数的含义。从Z到A的提交更改了函数的实现以及存在于Z处的所有调用站点，以及它们添加的新调用站点，以使它们保持一致。在A处没有错误。

​	同时假设在下行的开发中，在提交Y处有人为该函数添加了一个新的调用站点。从Z到B的提交都假定该函数和调用者的旧语义是一致的。在B处也没有错误。

​	进一步假设两个开发线在C处干净地合并，因此不需要冲突解决。

​	然而，C处的代码是有问题的，因为在下行开发的调用者尚未转换为上行开发引入的新语义。所以如果您只知道D是有问题的，Z是正常的，并且[git-bisect[1]](https://chat.openai.com/1/git-bisect)标识C为罪魁祸首，您将如何找出问题是由这种语义变化引起的呢？

​	当`git bisect`的结果是非合并提交时，您通常可以通过仅检查该提交来发现问题。开发者可以通过将其更改分解为小的自包含提交来使这个过程变得简单。然而，在上面的情况下，这对于发现问题是没有帮助的，因为任何单个提交的检查都无法发现问题；相反，需要全局视角来发现问题。更糟糕的是，上行开发中有关问题函数的语义变化可能只是上行开发中的一小部分更改。

​	另一方面，如果在C处没有合并，而是在Z到B之间的历史rebase到A的顶部，您将得到这种线性历史：

```
    ---Z---o---X--...---o---A---o---o---Y*--...---o---B*--D*
```

​	在Z和`D*`之间进行二分查找将找到一个罪魁祸首提交`Y*`，并且弄清楚为什么`Y*`是有问题的可能更容易。

​	部分原因是基于此，许多有经验的Git用户即使在工作中存在合并较多的项目时，在发布之前也会将历史保持线性，通过rebase到最新的上游版本。

## 高级分支管理

### 获取单个分支

​	除了使用[git-remote[1]](https://chat.openai.com/1/git-remote)，您还可以选择一次只更新一个分支，并将其存储在本地以任意名称命名：

``` bash
$ git fetch origin todo:my-todo-work
```

​	第一个参数`origin`表示告诉Git从最初克隆的仓库中获取。第二个参数告诉Git从远程仓库获取名为`todo`的分支，并将其存储在本地名称为`refs/heads/my-todo-work`的位置。

​	您还可以从其他仓库获取分支；因此：

``` bash
$ git fetch git://example.com/proj.git master:example-master
```

​	将创建一个名为`example-master`的新分支，并将其存储在从给定URL的仓库中获取的`master`分支中。如果您已经有一个名为example-master的分支，它将尝试将其[快进（fast-forward）](https://git-scm.com/docs/user-manual#fast-forwards)到example.com的master分支给出的提交。更详细地说：

### git fetch和 fast-forwards

​	在前面的示例中，当更新现有分支时，`git fetch`会检查确保远程分支上的最新提交是您的分支副本上最新提交的后代，然后将您的分支副本更新到指向新的提交。Git将此过程称为[快进（fast-forward）](https://git-scm.com/docs/user-manual#fast-forwards)。

​	fast-forward看起来像这样：

```
 o--o--o--o <-- old head of the branch
           \
            o--o--o <-- new head of the branch
```

​	在某些情况下，新HEAD实际上**不**是旧HEAD的后代。例如，开发者可能意识到存在严重错误，并决定回退，导致情况如下：

```
 o--o--o--o--a--b <-- old head of the branch
           \
            o--o--o <-- new head of the branch
```

​	在这种情况下，`git fetch`会失败并打印出警告。

​	在这种情况下，您仍然可以强制Git更新到新HEAD，如下一节所述。但是，请注意，在上面的情况下，这可能意味着丢失标记为`a`和`b`的提交，除非您已经创建了自己的引用指向它们。

### 强制git fetch进行non-fast-forward更新 

​	如果git fetch失败，因为分支的新HEAD不是旧HEAD的后代，您可以使用以下命令强制更新：

``` bash
$ git fetch git://example.com/proj.git +master:refs/remotes/example/master
```

​	注意添加了`+`号。或者，您可以使用`-f`标志强制更新所有获取的分支，如下所示：

``` bash
$ git fetch -f origin
```

​	请注意，旧版本的example/master指向的提交可能会丢失，正如我们在前面的部分中所看到的。

### 配置远程跟踪分支

​	我们之前看到`origin`只是一个指向最初克隆的仓库的快捷方式。这些信息存储在Git配置变量中，您可以使用[git-config[1]](https://chat.openai.com/1/git-config)查看：

``` bash
$ git config -l
core.repositoryformatversion=0
core.filemode=true
core.logallrefupdates=true
remote.origin.url=git://git.kernel.org/pub/scm/git/git.git
remote.origin.fetch=+refs/heads/*:refs/remotes/origin/*
branch.master.remote=origin
branch.master.merge=refs/heads/master
```

​	如果还有其他您经常使用的仓库，您可以创建类似的配置选项以节省输入；例如：

``` bash
$ git remote add example git://example.com/proj.git
```

将以下内容添加到`.git/config`：

```
[remote "example"]
	url = git://example.com/proj.git
	fetch = +refs/heads/*:refs/remotes/example/*
```

​	另请注意，上述配置也可以通过直接编辑文件`.git/config`来完成，而不是使用[git-remote[1]](https://chat.openai.com/1/git-remote)。

​	在配置远程仓库后，以下三个命令将执行相同的操作：

``` bash
$ git fetch git://example.com/proj.git +refs/heads/*:refs/remotes/example/*
$ git fetch example +refs/heads/*:refs/remotes/example/*
$ git fetch example
```

​	有关上述配置选项的更多详细信息，请参阅[git-config[1]](https://chat.openai.com/1/git-config)，有关refspec语法的更多详细信息，请参阅[git-fetch[1]](https://chat.openai.com/1/git-fetch)。

## Git 概念

​	Git建立在少数简单而强大的概念之上。虽然您可以不了解它们就完成工作，但如果您了解它们，将会发现Git更加直观。

​	我们从最重要的[对象数据库](https://git-scm.com/docs/user-manual#def_object_database)和[index](https://git-scm.com/docs/user-manual#def_index)开始。

### 对象数据库

​	我们在[理解历史：提交](https://git-scm.com/docs/user-manual#understanding-commits)中已经看到所有提交都存储在一个40位数字的“对象名称”下。实际上，存储项目历史所需的所有信息都存储在具有这些名称的对象中。在每种情况下，名称是通过计算对象内容的SHA-1哈希值得到的。SHA-1哈希是一种加密哈希函数。对于我们来说，这意味着不可能找到两个具有相同名称的不同对象。这有许多优点；其中之一是：

- 通过比较名称，Git可以快速确定两个对象是否相同。
- 由于对象名称在每个仓库中的计算方式相同，因此存储在两个仓库中的相同内容始终将存储在相同的名称下。
- 通过检查对象的名称是否仍然是其内容的SHA-1哈希，Git可以在读取对象时检测错误。

（有关对象格式和SHA-1计算的详细信息，请参阅[对象存储格式](https://git-scm.com/docs/user-manual#object-details)。）

​	有四种不同类型的对象：“blob”、“tree”、“commit”和“tag”。

- ["blob"对象](https://git-scm.com/docs/user-manual#def_blob_object)用于存储文件数据。
- ["tree"对象](https://git-scm.com/docs/user-manual#def_tree_object)将一个或多个“blob”对象绑定到目录结构中。此外，树对象可以引用其他树对象，从而创建目录层次结构。
- ["commit"对象](https://git-scm.com/docs/user-manual#def_commit_object)将这些目录层次结构链接在一起，形成一个[有向无环图](https://git-scm.com/docs/user-manual#def_DAG)的修订版本——每个提交包含恰好一个树的对象名称，该树指定了提交时的目录层次结构。此外，提交还引用“父”提交对象，描述了如何到达该目录层次结构的历史。
- ["tag"对象](https://git-scm.com/docs/user-manual#def_tag_object)符号标识并可用于签署其他对象。它包含另一个对象的对象名称和类型，一个符号名称（当然！）以及可选的签名。

​	更详细地介绍一下对象类型：

#### Commit 对象

​	"commit" 对象将一个树的物理状态与我们是如何到达该状态以及原因的描述相结合。使用`--pretty=raw`选项查看您喜欢的提交，例如[git-show[1]](https://chat.openai.com/1/git-show)或[git-log[1]](https://chat.openai.com/1/git-log)：

```bash
$ git show -s --pretty=raw 2be7fcb476
commit 2be7fcb4764f2dbcee52635b91fedb1b3dcf7ab4
tree fb3a8bdd0ceddd019615af4d57a53f43d8cee2bf
parent 257a84d9d02e90447b149af58b271c19405edb6a
author Dave Watson <dwatson@mimvista.com> 1187576872 -0400
committer Junio C Hamano <gitster@pobox.com> 1187591163 -0700

    Fix misspelling of 'suppress' in docs

    Signed-off-by: Junio C Hamano <gitster@pobox.com>
```

​	如您所见，提交由以下内容定义：

- 一个树：一个树对象的SHA-1名称（如下所定义），表示特定时间点上目录的内容。
- 父项：表示项目历史中上一步的一个或多个提交的SHA-1名称。上面的示例有一个父项；合并提交可能有多个父项。没有父项的提交称为“根”提交，并表示项目的初始版本。每个项目必须至少有一个根。项目还可以有多个根，尽管这不常见（也不一定是一个好主意）。
- 作者：对该更改负责的人的姓名，以及其日期。
- 提交者：实际创建提交的人的姓名，以及提交日期。这可能与作者不同，例如，如果作者是为提交撰写了补丁并通过电子邮件发送给实际创建提交的人。
- 描述此提交的注释。

​	请注意，提交本身不包含有关实际更改的任何信息；所有更改是通过比较由此提交引用的树的内容与其父项关联的树来计算的。特别地，Git不尝试显式记录文件重命名，尽管它可以识别存在相同文件数据在不同路径上发生更改的情况，这暗示了重命名（请参阅[git-diff[1]](https://chat.openai.com/1/git-diff)中的`-M`选项）。提交通常由[git-commit[1]](https://chat.openai.com/1/git-commit)创建，它创建一个提交，其父项通常是当前的HEAD，其树来自当前存储在索引中的内容。

​	一般情况下，使用[git-commit[1]](https://chat.openai.com/1/git-commit)创建提交。该命令创建一个提交，其父提交通常是当前的HEAD，而树是从当前索引中的内容中获取的。

#### Tree 对象

​	多功能的[git-show[1]](https://chat.openai.com/1/git-show)命令也可用于查看树对象，但[git-ls-tree[1]](https://chat.openai.com/1/git-ls-tree)将提供更多详细信息：

``` bash
$ git ls-tree fb3a8bdd0ce
100644 blob 63c918c667fa005ff12ad89437f2fdc80926e21c    .gitignore
100644 blob 5529b198e8d14decbe4ad99db3f7fb632de0439d    .mailmap
100644 blob 6ff87c4664981e4397625791c8ea3bbb5f2279a3    COPYING
040000 tree 2fb783e477100ce076f6bf57e4a6f026013dc745    Documentation
100755 blob 3c0032cec592a765692234f1cba47dfdcc3a9200    GIT-VERSION-GEN
100644 blob 289b046a443c0647624607d471289b2c7dcd470b    INSTALL
100644 blob 4eb463797adc693dc168b926b6932ff53f17d0b1    Makefile
100644 blob 548142c327a6790ff8821d67c2ee1eff7a656b52    README
...
```

​	正如您所见，树对象包含一个条目列表，每个条目都有一个模式、对象类型、SHA-1名称和名称，并按名称排序。它表示单个目录树的内容。

​	对象类型可以是blob，表示文件的内容，也可以是另一个树，表示子目录的内容。由于树和blob，像所有其他对象一样，是通过它们的内容的SHA-1哈希命名的，因此当且仅当它们的内容（包括递归地包含所有子目录的内容）相同时，两个树具有相同的SHA-1名称。这使得Git可以快速确定两个相关树对象之间的差异，因为它可以忽略具有相同对象名称的任何条目。

（注意：在存在子模块的情况下，树也可以具有提交作为条目。请参阅[子模块](https://git-scm.com/docs/user-manual#submodules)的文档。）

​	请注意，所有文件的模式都是644或755：Git实际上只关注可执行位。

#### Blob 对象

​	您可以使用[git-show[1]](https://chat.openai.com/1/git-show)查看blob的内容；例如，从上面的树中`COPYING`的条目中获取的blob如下：

``` bash
$ git show 6ff87c4664

 Note that the only valid version of the GPL as far as this project
 is concerned is _this_ particular version of the license (ie v2, not
 v2.2 or v3.x or whatever), unless explicitly otherwise stated.
...
```

​	"blob"对象只是一种二进制数据块。它不引用其他任何内容，也没有任何属性。

​	由于blob完全由其数据定义，如果目录树中的两个文件（或仓库的多个不同版本中的文件）具有相同的内容，则它们将共享相同的blob对象。对象完全独立于其在目录树中的位置，重命名文件不会更改与该文件关联的对象。

​	请注意，可以使用[git-show[1]](https://chat.openai.com/1/git-show)命令的`<revision>:<path>`语法来查看任何树或blob对象。这有时对于浏览当前未检出的树的内容很有用。

#### Trust

​	如果您从一个来源接收到blob的SHA-1名称，从另一个（可能不受信任的）来源接收到其内容，只要SHA-1名称一致，您仍然可以相信这些内容是正确的。这是因为SHA-1被设计成不可能找到产生相同哈希的不同内容。

​	同样，您只需要相信顶级树对象的SHA-1名称，就可以信任它引用的整个目录的内容。如果您从可信任的来源接收到提交的SHA-1名称，那么您可以轻松验证通过该提交的父项可达的所有提交的整个历史记录，以及这些提交引用的所有树的内容。

​	因此，要在系统中引入一些真正的信任，您只需要对*一个*特殊备注进行数字签名，其中包含一个顶级提交的名称。您的数字签名向他人表明您信任该提交，而提交历史记录的不可变性告诉他人他们可以信任整个历史记录。

​	换句话说，您可以通过只发送一封电子邮件来轻松验证整个存档，告诉人们顶级提交的名称（SHA-1哈希），并使用类似GPG/PGP的方法对该电子邮件进行数字签名。

​	为了在这方面提供帮助，Git还提供了标签对象...

#### Tag 对象

​	标签对象包含一个对象、对象类型、标签名称、创建标签的人（“tagger”）的姓名和一条消息，该消息可能包含签名，如[git-cat-file[1]](https://chat.openai.com/1/git-cat-file)所示：

``` bash
$ git cat-file tag v1.5.0
object 437b1b20df4b356c9342dac8d38849f24ef44f27
type commit
tag v1.5.0
tagger Junio C Hamano <junkio@cox.net> 1171411200 +0000

GIT 1.5.0
-----BEGIN PGP SIGNATURE-----
Version: GnuPG v1.4.6 (GNU/Linux)

iD8DBQBF0lGqwMbZpPMRm5oRAuRiAJ9ohBLd7s2kqjkKlq1qqC57SbnmzQCdG4ui
nLE/L9aUXdWeTFPron96DLA=
=2E+0
-----END PGP SIGNATURE-----
```

​	有关如何创建和验证标签对象的详细信息，请参阅[git-tag[1]](https://chat.openai.com/1/git-tag)命令。（请注意，[git-tag[1]](https://chat.openai.com/1/git-tag)还可用于创建“轻量级标签”，这根本不是标签对象，而只是以`refs/tags/`开头的简单引用。）

#### Git如何高效地存储对象：pack文件

​	新创建的对象最初存储在一个以对象的SHA-1哈希命名的文件中（存储在`.git/objects`中）。

​	不幸的是，一旦项目拥有大量对象，该系统变得效率低下。尝试在一个旧项目上执行以下操作：

``` bash
$ git count-objects
6930 objects, 47620 kilobytes
```

​	第一个数字是保留在单独文件中的对象数量。第二个数字是这些“松散”对象占用的空间。

​	通过将这些松散对象移入“pack文件”可以节省空间并使Git更快。pack文件以高效压缩的格式存储一组对象；有关pack文件格式的详细信息，请参阅[gitformat-pack[5]](https://chat.openai.com/5/gitformat-pack)。

​	要将松散对象放入pack文件中，只需运行git repack：

``` bash
$ git repack
Counting objects: 6020, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (6020/6020), done.
Writing objects: 100% (6020/6020), done.
Total 6020 (delta 4070), reused 0 (delta 0)
```

​	这将在`.git/objects/pack/`中创建一个单独的“pack文件”，其中包含所有当前未打包的对象。然后，您可以运行

``` bash
$ git prune
```

来删除任何现在包含在pack文件中的“松散”对象。这也将删除任何未引用的对象（例如，当您使用`git reset`删除提交时可能会创建未引用的对象）。您可以通过查看`.git/objects`目录或运行

``` bash
$ git count-objects
0 objects, 0 kilobytes
```

来验证松散对象是否已删除。

​	尽管对象文件已经不存在，但是引用这些对象的任何命令将与之前完全相同。

​	[git-gc[1]](https://chat.openai.com/1/git-gc)命令可以为您执行打包、修剪等操作，因此通常是您唯一需要的高级命令。

#### 悬空对象 Dangling objects

​	[git-fsck[1]](https://chat.openai.com/1/git-fsck)命令有时会报告悬空对象的问题。但这并不是问题。

​	悬空对象最常见的原因是你已经对一个分支进行了变基，或者你从别人那里拉取了一个已经进行了变基的分支，参见[重写历史和维护补丁系列](https://git-scm.com/docs/user-manual#cleaning-up-history)。在这种情况下，原始分支的旧HEAD仍然存在，以及它所指向的一切。只是分支指针本身不再存在，因为你用另一个替代它了。

​	还有其他导致悬空对象的情况。例如，"悬空blob"可能是因为你在执行`git add`命令添加文件后，然后在实际提交并将其作为大版本的一部分之前，又对该文件进行了其他更改并提交了**更新后**的版本——最初添加的旧状态不再被任何提交或树引用，所以它现在是一个悬空的blob对象。

​	类似地，当运行"ort"合并策略并发现存在交叉合并，从而有多个合并基（这相当不寻常，但确实会发生），它会生成一个临时的中间树（或者甚至更多，如果您有许多交叉合并和多个合并基）作为临时内部合并基，同样，它们是真实的对象，但最终结果将不会指向它们，所以它们最终"悬空"在你的仓库中。

​	通常情况下，悬空对象无需担心。它们甚至可能非常有用：如果出现问题，悬空对象可能是恢复旧树的方式（例如，你进行了变基，并意识到你实际上并不想变基——你可以查看你拥有的悬空对象，并决定将HEAD重置为某个旧的悬空状态）。

​	对于提交，您可以使用：

``` bash
$ gitk <dangling-commit-sha-goes-here> --not --all
```

​	这将要求显示从给定提交可达的所有历史记录，但不包括任何分支、标签或其他引用。如果您决定需要该提交，您始终可以创建一个新的引用，例如：

``` bash
$ git branch recovered-branch <dangling-commit-sha-goes-here>
```

​	对于blob和树，您不能执行相同的操作，但您仍然可以查看它们。您可以使用以下命令：

``` bash
$ git show <dangling-blob/tree-sha-goes-here>
```

to show what the contents of the blob were (or, for a tree, basically what the `ls` for that directory was), and that may give you some idea of what the operation was that left that dangling object.

来显示blob的内容（或者对于树来说，基本上是该目录的`ls`内容），这可能会让您对导致该悬空对象出现的操作有一些了解。

​	通常情况下，悬空blob和树并不是非常有趣。它们几乎总是由于半路合并基（如果有冲突的合并，blob中通常会包含冲突标记，如果您手动解决了冲突的合并），或者只是因为您中断了`git fetch`，使用^C或类似方法，导致对象数据库中有*一些*新对象，但它们只是悬空的，没有用。

​	无论如何，一旦确信您对任何悬空状态不感兴趣，您可以通过修剪所有不可达的对象来删除它们：

``` bash
$ git prune
```

它们将被清除。（在静态的仓库上运行`git prune`，就像进行文件系统的fsck恢复：您不希望在文件系统挂载时执行该操作。`git prune`的设计是不会在这些并发访问仓库的情况下造成任何损害，但您可能会收到混淆或令人恐慌的消息。）

#### 从仓库损坏中恢复

​	根据设计，Git对其信任的数据持谨慎态度。然而，即使在Git本身没有错误的情况下，硬件或操作系统错误仍可能导致数据损坏。

​	防止此类问题的首要措施是备份。您可以使用克隆（clone）进行Git目录的备份，或者只使用cp、tar或任何其他备份机制。

​	作为最后的手段，您可以搜索损坏的对象，并尝试手动替换它们。在尝试之前，请备份您的仓库，以防在过程中进一步破坏。

​	我们假设问题是单个丢失或损坏的blob，这有时是一个可以解决的问题（恢复丢失的树，尤其是提交，**要困难得多**）。

​	在开始之前，使用[git-fsck[1]](https://chat.openai.com/1/git-fsck)验证是否存在损坏，并确定损坏的位置；这可能会耗费一些时间。

​	假设输出看起来像这样：

``` bash
$ git fsck --full --no-dangling
broken link from    tree 2d9263c6d23595e7cb2a21e5ebbb53655278dff8
              to    blob 4b9458b3786228369c63936db65827de3cc06200
missing blob 4b9458b3786228369c63936db65827de3cc06200
```

​	现在您知道blob 4b9458b3丢失了，并且树2d9263c6指向它。如果您能在其他仓库中找到该丢失的blob对象的一个副本，您可以将它移动到`.git/objects/4b/9458b3...`并完成。假设您找不到。您仍然可以使用[git-ls-tree[1]](https://chat.openai.com/1/git-ls-tree)检查指向它的树，它可能输出类似于：

``` bash
$ git ls-tree 2d9263c6d23595e7cb2a21e5ebbb53655278dff8
100644 blob 8d14531846b95bfa3564b58ccfb7913a034323b8	.gitignore
100644 blob ebf9bf84da0aab5ed944264a5db2a65fe3a3e883	.mailmap
100644 blob ca442d313d86dc67e0a2e5d584b465bd382cbf5c	COPYING
...
100644 blob 4b9458b3786228369c63936db65827de3cc06200	myfile
...
```

​	现在您知道缺失的blob是名为`myfile`的文件的数据。很有可能您也可以识别出目录——假设它在`somedirectory`中。如果您很幸运，丢失的副本可能与您的工作树中的`somedirectory/myfile`处检出的副本相同；您可以使用[git-hash-object[1]](https://chat.openai.com/1/git-hash-object)来测试是否正确：

``` bash
$ git hash-object -w somedirectory/myfile
```

这将创建并存储一个带有`somedirectory/myfile`内容的blob对象，并输出该对象的SHA-1。如果非常幸运，它可能是4b9458b3786228369c63936db65827de3cc06200，这意味着您猜对了，损坏已经修复！

​	否则，您需要更多信息。您如何确定已丢失的文件版本？

​	最简单的方法是使用：

``` bash
$ git log --raw --all --full-history -- somedirectory/myfile
```

​	因为您请求了原始输出，您现在将得到类似于：

```
commit abc
Author:
Date:
...
:100644 100644 4b9458b newsha M somedirectory/myfile


commit xyz
Author:
Date:

...
:100644 100644 oldsha 4b9458b M somedirectory/myfile
```

​	这告诉您，随后的版本是"newsha"，之前的版本是"oldsha"。您还知道与从oldsha到4b9458b的更改以及从4b9458b到newsha的更改有关的提交消息。

​	如果您一直在提交足够小的更改，现在您可能有很大机会重建中间状态4b9458b的内容。

​	如果您可以做到这一点，现在可以使用以下命令重新创建丢失的对象：

``` bash
$ git hash-object -w <recreated-file>
```

您的仓库现在恢复正常！

（顺便说一下，您可以忽略`fsck`，直接执行：

``` bash
$ git log --raw --all
```

然后在整个输出中查找丢失对象（4b9458b）的sha。这取决于您——Git确实**有**大量信息，只是缺少一个特定的blob版本。）

### 索引

​	索引是一个二进制文件（通常保存在`.git/index`），其中包含一个经过排序的路径名列表，每个路径名都带有权限和对应的blob对象的SHA-1值；[git-ls-files[1]](https://chat.openai.com/1/git-ls-files)可以显示索引的内容：

``` bash
$ git ls-files --stage
100644 63c918c667fa005ff12ad89437f2fdc80926e21c 0	.gitignore
100644 5529b198e8d14decbe4ad99db3f7fb632de0439d 0	.mailmap
100644 6ff87c4664981e4397625791c8ea3bbb5f2279a3 0	COPYING
100644 a37b2152bd26be2c2289e1f57a292534a51a93c7 0	Documentation/.gitignore
100644 fbefe9a45b00a54b58d94d06eca48b03d40a50e0 0	Documentation/Makefile
...
100644 2511aef8d89ab52be5ec6a5e46236b4b6bcd07ea 0	xdiff/xtypes.h
100644 2ade97b2574a9f77e7ae4002a4e07a6a38e46d07 0	xdiff/xutils.c
100644 d5de8292e05e7c36c4b68857c1cf9855e3d2f70a 0	xdiff/xutils.h
```

​	请注意，在旧的文档中，您可能会看到索引被称为"当前目录缓存"或者只是"缓存"。它有三个重要的属性：

1. 索引包含生成单个（唯一确定的）树对象所需的所有信息。

   例如，运行[git-commit[1]](https://chat.openai.com/1/git-commit)会根据索引生成这个树对象，将其存储在对象数据库中，并将其用作与新提交相关联的树对象。

2. 索引允许快速比较它定义的树对象和工作树之间的差异。

   它通过为每个条目存储一些附加数据（例如上次修改时间）来实现。这些数据不在上面的输出中显示，并且不会存储在创建的树对象中，但可以用于快速确定工作目录中的哪些文件与索引中存储的内容不同，从而使Git无需读取这些文件的所有数据来查找更改。

3. 它可以高效地表示不同树对象之间的合并冲突信息，使得每个路径名都与涉及的树有足够的信息关联，以便您可以在它们之间创建三方合并。

   我们在[在合并过程中获取冲突解决帮助](https://git-scm.com/docs/user-manual#conflict-resolution)中看到，在合并过程中，索引可以存储单个文件的多个版本（称为"stage"）。上面[git-ls-files[1]](https://chat.openai.com/1/git-ls-files)输出中的第三列是stage编号，对于存在合并冲突的文件，其值将为0以外的值。


​	因此，索引是一种临时的暂存区，其中填充着您正在处理的树。

​	如果完全删除索引，通常不会丢失任何信息，只要您知道它所描述的树的名称。

## 子模块（Submodules）

​	大型项目通常由较小、独立的模块组成。例如，嵌入式Linux发行版的源代码树可能包含发行版中的每个软件部件以及一些本地修改；一个视频播放器可能需要构建并针对特定的、已经测试过的解压库版本进行编译；几个独立的程序可能共享相同的构建脚本。

​	在集中式版本控制系统中，通常通过将每个模块包含在一个单一的仓库中来实现这一点。开发者可以检出所有模块，或者只检出需要处理的模块。他们甚至可以在单个提交中跨多个模块修改文件，同时移动文件或更新API和翻译。

​	然而，Git不允许部分检出，所以在Git中复制这种方法会迫使开发者保留他们不打算操作的模块的本地副本。大型检出中的提交速度会比预期慢，因为Git需要扫描每个目录以查找更改。如果模块具有大量的本地历史，克隆操作将需要很长时间。

​	幸运的是，分布式版本控制系统可以更好地与外部资源集成。在集中式模型中，外部项目的一个任意快照从其自己的版本控制中导出，然后导入到供应商分支的本地版本控制中。所有的历史都被隐藏了。使用分布式版本控制，您可以克隆整个外部历史，并更容易跟踪开发和重新合并本地更改。

​	Git的子模块支持允许一个仓库包含外部项目的检出，作为子目录。子模块保持它们自己的身份；子模块支持只是存储子模块仓库位置和提交ID，因此克隆包含子模块的父项目（"superproject"）的其他开发者可以轻松地克隆所有子模块，并保持相同的提交。父项目的部分检出也是可能的：您可以告诉Git克隆一些或所有的子模块。

​	[git-submodule[1]](https://chat.openai.com/1/git-submodule)命令自Git 1.5.3版本起可用。使用Git 1.5.2版本的用户可以在仓库中查找子模块提交并手动检出它们；更早版本不会识别子模块。

​	为了查看子模块支持的工作原理，请创建四个示例仓库，稍后将用作子模块：

``` bash
$ mkdir ~/git
$ cd ~/git
$ for i in a b c d
do
	mkdir $i
	cd $i
	git init
	echo "module $i" > $i.txt
	git add $i.txt
	git commit -m "Initial commit, submodule $i"
	cd ..
done
```

​	现在创建父项目并添加所有子模块：

``` bash
$ mkdir super
$ cd super
$ git init
$ for i in a b c d
do
	git submodule add ~/git/$i $i
done
```

| Note | Do not use local URLs here if you plan to publish your superproject! |
| ---- | ------------------------------------------------------------ |
| 注意 | 如果计划发布您的父项目，请不要在此处使用本地URL！            |

​	查看`git submodule`创建的文件：

``` bash
$ ls -a
.  ..  .git  .gitmodules  a  b  c  d
```

​	`git submodule add <repo> <path>`命令执行了一些操作：

- 它从`<repo>`克隆子模块到当前目录下给定的`<path>`，默认情况下会检出主分支。
- 它将子模块的克隆路径添加到[gitmodules[5]](https://chat.openai.com/5/gitmodules)文件，并将此文件添加到索引中，准备提交。
- 它将子模块的当前提交ID添加到索引中，准备提交。

​	提交父项目：

``` bash
$ git commit -m "Add submodules a, b, c and d."
```

​	现在克隆父项目：

``` bash
$ cd ..
$ git clone super cloned
$ cd cloned
```

​	子模块目录已经存在，但是它们是空的：

``` bash
$ ls -a a
.  ..
$ git submodule status
-d266b9873ad50488163457f025db7cdd9683d88b a
-e81d457da15309b4fef4249aba9b50187999670d b
-c1536a972b9affea0f16e0680ba87332dc059146 c
-d96249ff5d57de5de093e6baff9e0aafa5276a74 d
```

| Note | The commit object names shown above would be different for you, but they should match the HEAD commit object names of your repositories. You can check it by running `git ls-remote ../a`. |
| ---- | ------------------------------------------------------------ |
| 注意 | 上面显示的提交对象名称对您来说可能会有所不同，但它们应该与您的仓库的HEAD提交对象名称匹配。您可以通过运行`git ls-remote ../a`来检查。 |

​	拉取子模块是一个两步的过程。首先运行 `git submodule init` 将子模块的仓库URL添加到 `.git/config` 文件中：

``` bash
$ git submodule init
```

​	然后使用 `git submodule update` 克隆仓库并检出超级项目中指定的提交：

``` bash
$ git submodule update
$ cd a
$ ls -a
.  ..  .git  a.txt
```

​	`git submodule update` 和 `git submodule add` 的一个主要区别是，`git submodule update` 检出一个特定的提交，而不是分支的最新提交。这类似于检出一个标签：HEAD处于分离状态，因此你不在分支上工作。

``` bash
$ git branch
* (detached from d266b98)
  master
```

​	如果您想在子模块中进行更改，并且HEAD处于分离状态，则应该创建或检出一个分支，进行更改，将更改发布到子模块中，然后更新超级项目以引用新的提交：

``` bash
$ git switch master
```

或者

``` bash
$ git switch -c fix-up
```

然后

``` bash
$ echo "adding a line again" >> a.txt
$ git commit -a -m "Updated the submodule from within the superproject."
$ git push
$ cd ..
$ git diff
diff --git a/a b/a
index d266b98..261dfac 160000
--- a/a
+++ b/a
@@ -1 +1 @@
-Subproject commit d266b9873ad50488163457f025db7cdd9683d88b
+Subproject commit 261dfac35cb99d380eb966e102c1197139f7fa24
$ git add a
$ git commit -m "Updated submodule a."
$ git push
```

​	如果您想要更新子模块，那么在执行 `git pull` 之后，必须运行 `git submodule update`。

### 子模块的陷阱 Pitfalls with submodules

​	在发布引用子模块的更改之前，请始终发布子模块本身的更改。如果您忘记发布子模块的更改，其他人将无法克隆该仓库：

``` bash
$ cd ~/git/super/a
$ echo i added another line to this file >> a.txt
$ git commit -a -m "doing it wrong this time"
$ cd ..
$ git add a
$ git commit -m "Updated submodule a again."
$ git push
$ cd ~/git/cloned
$ git pull
$ git submodule update
error: pathspec '261dfac35cb99d380eb966e102c1197139f7fa24' did not match any file(s) known to git.
Did you forget to 'git add'?
Unable to checkout '261dfac35cb99d380eb966e102c1197139f7fa24' in submodule path 'a'
```

​	在旧的Git版本中，很容易忘记在子模块中提交新的或修改的文件，这会导致类似于不推送子模块更改的问题。从Git 1.7.0开始，当子模块包含新文件或修改的文件时，超级项目中的`git status`和`git diff`将显示子模块为已修改状态，以防止意外提交此状态。当生成补丁输出或使用`--submodule`选项时，`git diff`还会在工作树侧添加`-dirty`。

``` bash
$ git diff
diff --git a/sub b/sub
--- a/sub
+++ b/sub
@@ -1 +1 @@
-Subproject commit 3f356705649b5d566d97ff843cf193359229a453
+Subproject commit 3f356705649b5d566d97ff843cf193359229a453-dirty
$ git diff --submodule
Submodule sub 3f35670..3f35670-dirty:
```

​	您还不应将子模块倒回超级项目中的任何早期提交之前的分支。

​	如果在子模块中进行了未提交的更改并且未首先检出一个分支，则不安全地运行`git submodule update`，这些更改将被静默地覆盖：

``` bash
$ cat a.txt
module a
$ echo line added from private2 >> a.txt
$ git commit -a -m "line added inside private2"
$ cd ..
$ git submodule update
Submodule path 'a': checked out 'd266b9873ad50488163457f025db7cdd9683d88b'
$ cd a
$ cat a.txt
module a
```

| Note | The changes are still visible in the submodule’s reflog. |
| ---- | -------------------------------------------------------- |
| 注意 | 在子模块的记录日志中仍然可以看到更改。                   |

​	如果在子模块工作树中有未提交的更改，`git submodule update` 将不会覆盖它们。而是会显示有关无法从脏分支切换的常规警告。

## Git的低级操作

​	许多高级命令最初是使用较小的一组低级Git命令实现的Shell脚本。当需要对Git进行非常规操作，或者只是想了解其内部工作原理时，这些低级命令仍然非常有用。

### 对象访问和操作

​	[git-cat-file[1]](https://chat.openai.com/1/git-cat-file) 命令可以显示任何对象的内容，尽管高级命令 [git-show[1]](https://chat.openai.com/1/git-show) 通常更有用。

​	[git-commit-tree[1]](https://chat.openai.com/1/git-commit-tree) 命令允许构建具有任意父对象和树对象的提交。

​	可以使用 [git-write-tree[1]](https://chat.openai.com/1/git-write-tree) 创建树对象，并且可以通过 [git-ls-tree[1]](https://chat.openai.com/1/git-ls-tree) 访问其数据。两个树对象可以使用 [git-diff-tree[1]](https://chat.openai.com/1/git-diff-tree) 进行比较。

​	可以使用 [git-mktag[1]](https://chat.openai.com/1/git-mktag) 创建标签，并且签名可以使用 [git-verify-tag[1]](https://chat.openai.com/1/git-verify-tag) 进行验证，尽管通常使用 [git-tag[1]](https://chat.openai.com/1/git-tag) 更简单。

### 工作流程

​	高级操作，如 [git-commit[1]](https://chat.openai.com/1/git-commit) 和 [git-restore[1]](https://chat.openai.com/1/git-restore) 通过在工作树、索引和对象数据库之间移动数据来工作。Git提供了执行这些步骤的低级操作。

​	通常，所有的Git操作都是在索引文件上进行的。一些操作仅在索引文件上工作（显示索引的当前状态），但大多数操作在索引文件与数据库或工作目录之间移动数据。因此，有四种主要的组合：

#### 工作目录 → 索引

​	[git-update-index[1]](https://chat.openai.com/1/git-update-index) 命令将工作目录中的信息更新到索引中。您通常通过指定要更新的文件名来更新索引信息，例如：

``` bash
$ git update-index filename
```

但是为了避免文件名通配符等常见错误，该命令通常不会完全添加全新的条目或删除旧的条目，即通常只会更新现有的缓存条目。

​	要告诉Git是的，您确实意识到某些文件不再存在，或者应该添加新文件，请分别使用 `--remove` 和 `--add` 标志。

​	注意！`--remove` 标志并不意味着随后的文件名一定会被删除：如果文件在目录结构中仍然存在，则索引将根据其新状态更新，而不会删除它们。`--remove` 的唯一含义是 `update-index` 将考虑已删除的文件为有效的内容，并且如果该文件实际上不再存在，则会相应地更新索引。

​	作为特例，您还可以执行 `git update-index --refresh`，它将刷新每个索引的"stat"信息以与当前stat信息匹配。它不会更新对象状态本身，并且只会更新用于快速测试对象是否仍与其旧后备存储对象匹配的字段。

​	之前介绍的 [git-add[1]](https://chat.openai.com/1/git-add) 只是 [git-update-index[1]](https://chat.openai.com/1/git-update-index) 的一个包装器。

#### 索引 → 对象数据库

​	您可以使用以下程序将当前的索引文件写入“tree”对象：

``` bash
$ git write-tree
```

该程序不带任何选项，它会将当前的索引写入描述该状态的一组树对象，并返回生成的顶层树的名称。您可以使用该树在任何时候重新生成索引，通过执行以下操作：

#### 对象数据库 → 索引

​	您可以从对象数据库中读取“tree”文件，并使用它来填充（并覆盖 - 如果您的索引包含任何未保存状态，您可能希望稍后恢复！）当前的索引。正常操作是：

``` bash
$ git read-tree <SHA-1 of tree>
```

现在，您的索引文件将等同于之前保存的树。然而，这只是您的索引文件：您的工作目录内容尚未修改。

#### 索引 → 工作目录

​	您可以通过"checking out"文件来从索引更新您的工作目录。这不是一个很常见的操作，因为通常您只需要保持您的文件更新，并且而不是写入您的工作目录，您会告诉索引文件关于工作目录中的更改（即 `git update-index`）。

​	但是，如果您决定切换到新版本，或者检出其他人的版本，或者恢复以前的树，您将用 `git read-tree` 填充索引文件，然后需要使用以下命令检出结果：

``` bash
$ git checkout-index filename
```

或者，如果要检出所有索引，使用 `-a`。

​	注意！`git checkout-index` 通常会拒绝覆盖旧文件，因此如果您已经检出了旧版本的树，则需要在 `-a` 标志或文件名之前使用 `-f` 标志（在其之前）来*强制*检出。

​	最后，还有一些不纯粹从一种表示形式移动到另一种表示形式的操作：

#### 将所有内容联系在一起 Tying it all together

​	要提交使用 `git write-tree` 实例化的树，您将创建一个引用该树及其背后的历史的 "commit" 对象。主要是前面在历史中紧随其后的 "parent" 提交。

​	通常，一个 "commit" 有一个父提交：某种更改之前的树的前一个状态。然而，有时候一个 "commit" 可能有两个或更多个父提交，在这种情况下，我们称之为"合并"，因为此类提交将其他提交表示的两个或多个先前状态聚集（"合并"）在一起。

​	换句话说，"tree" 表示工作目录的特定目录状态，而 "commit" 表示那个时间点的状态，并解释了我们是如何到达那里的。

​	您通过给它提交时的状态描述的树以及父提交列表来创建 "commit" 对象：

``` bash
$ git commit-tree <tree> -p <parent> [(-p <parent2>)...]
```

然后在标准输入上给出提交原因（通过从管道或文件重定向，或者直接在 tty 上键入）。

​	`git commit-tree` 将返回表示该提交的对象的名称，您应该保存它以供以后使用。通常，您会提交一个新的 `HEAD` 状态，而 Git 不关心您将该状态的注释保存在何处，但实际上我们倾向于将结果写入 `.git/HEAD` 指向的文件，以便始终可以看到最后一个提交状态是什么。

​	以下是说明各种组件如何相互配合的示意图：

```
                     commit-tree
                      commit obj
                       +----+
                       |    |
                       |    |
                       V    V
                    +-----------+
                    | Object DB |
                    |  Backing  |
                    |   Store   |
                    +-----------+
                       ^
           write-tree  |     |
             tree obj  |     |
                       |     |  read-tree
                       |     |  tree obj
                             V
                    +-----------+
                    |   Index   |
                    |  "cache"  |
                    +-----------+
         update-index  ^
             blob obj  |     |
                       |     |
    checkout-index -u  |     |  checkout-index
             stat      |     |  blob obj
                             V
                    +-----------+
                    |  Working  |
                    | Directory |
                    +-----------+
```

### 检查数据

​	您可以使用各种辅助工具检查对象数据库和索引中表示的数据。对于每个对象，您可以使用 [git-cat-file[1]](https://chat.openai.com/1/git-cat-file) 查看有关对象的详细信息：

``` bash
$ git cat-file -t <objectname>
```

显示对象的类型，一旦您知道了类型（通常隐含在找到对象的位置中），您可以使用：

``` bash
$ git cat-file blob|tree|commit|tag <objectname>
```

来显示其内容。注意！树具有二进制内容，因此有一个特殊的辅助工具用于显示内容，称为 `git ls-tree`，它将二进制内容转换为更易读的形式。

​	查看“commit”对象特别有教育意义，因为这些对象往往很小且相当容易理解。特别是，如果您遵循将顶部提交名称保存在 `.git/HEAD` 中的约定，则可以执行以下操作：

``` bash
$ git cat-file commit HEAD
```

以查看顶部提交的内容。

### 合并多个树

​	Git 可以帮助您执行三方合并，通过多次重复合并过程，可以使用它执行多路合并。通常情况下，您只执行一次三方合并（调和两个历史线），然后提交结果，但如果愿意，您可以一次合并多个分支。

​	要执行三方合并，您首先获取要合并的两个提交，并找到它们的最近公共父提交（第三个提交），然后比较对应于这三个提交的树。

​	要获取合并的"基"，请查找两个提交的公共父提交：

``` bash
$ git merge-base <commit1> <commit2>
```

​	这将打印它们都基于的提交名称。现在，您应该查找这些提交的树对象，可以使用以下命令轻松地执行：

``` bash
$ git cat-file commit <commitname> | head -1
```

因为树对象信息总是出现在提交对象的第一行。

​	一旦知道您将要合并的三个树（即 "原始" 树，也称为共同树，以及两个 "结果" 树，即要合并的分支），您可以在索引中执行 "merge" 读取。如果它必须丢弃旧的索引内容，它会发出警告，因此您应该确保已提交这些内容 - 实际上，您通常会对最后一个提交执行合并（因此应与当前索引的内容匹配）。

​	要执行合并，使用以下命令：

``` bash
$ git read-tree -m -u <origtree> <yourtree> <targettree>
```

这将在索引文件中直接为您执行所有简单的合并操作，然后您可以使用 `git write-tree` 将结果写入。

### 继续合并多个树

​	不幸的是，许多合并并不是简单的。如果有文件已添加、移动或删除，或者两个分支都修改了相同的文件，您将得到一个包含“合并条目”的索引树。这样的索引树*无法*写入到树对象中，您必须使用其他工具解决任何此类合并冲突，然后才能写出结果。

​	您可以使用 `git ls-files --unmerged` 命令检查此类索引状态。以下是一个例子：

``` bash
$ git read-tree -m $orig HEAD $target
$ git ls-files --unmerged
100644 263414f423d0e4d70dae8fe53fa34614ff3e2860 1	hello.c
100644 06fa6a24256dc7e560efa5687fa84b51f0263c3a 2	hello.c
100644 cc44c73eb783565da5831b4d820c962954019b69 3	hello.c
```

​	`git ls-files --unmerged` 输出的每一行以blob模式位、blob SHA-1、*stage number*和文件名开头。*stage number* 是Git用来表示它来自哪个树的方式：stage 1 对应 `$orig` 树，stage 2 对应 `HEAD` 树，stage 3 对应 `$target` 树。

​	前面我们说过，简单的合并是在 `git read-tree -m` 中完成的。例如，如果文件从 `$orig` 到 `HEAD` 或者 `$orig` 到 `$target` 都没有改变，或者文件从 `$orig` 到 `HEAD` 和 `$orig` 到 `$target` 改变的方式相同，那么最终的结果显然是 `HEAD` 中的内容。上面的示例显示了文件 `hello.c` 在从 `$orig` 到 `HEAD` 和从 `$orig` 到 `$target` 之间以不同的方式进行了更改。您可以通过运行您喜欢的3路合并程序（如`diff3`、`merge`或Git自己的merge-file）来解决这个问题，操作blob对象中来自这三个stage的数据，例如：

``` bash
$ git cat-file blob 263414f >hello.c~1
$ git cat-file blob 06fa6a2 >hello.c~2
$ git cat-file blob cc44c73 >hello.c~3
$ git merge-file hello.c~2 hello.c~1 hello.c~3
```

​	这将在文件 `hello.c~2` 中留下合并结果，如果有冲突，则会有冲突标记。在验证合并结果正确后，您可以通过以下命令告诉Git该文件的最终合并结果：

``` bash
$ mv -f hello.c~2 hello.c
$ git update-index hello.c
```

​	当一个路径处于“unmerged”状态时，对该路径运行 `git update-index` 告诉Git将该路径标记为已解决。

​	以上是Git在最低层面上的合并描述，以帮助您理解底层的概念。实际上，即使是Git本身，也不会为此运行 `git cat-file` 三次。有一个名为 `git merge-index` 的程序，它将stage提取到临时文件中，并在其上调用一个“merge”脚本：

``` bash
$ git merge-index git-merge-one-file hello.c
```

高级别的 `git merge -s resolve` 就是使用这种方法实现的。

## Hacking Git

​	本章涵盖了 Git 实现的内部细节，这些细节可能只有 Git 开发人员才需要了解。

### 对象存储格式

​	所有对象都有一个静态确定的“类型”，用于标识对象的格式（即它是如何使用的，以及它如何引用其他对象）。目前有四种不同的对象类型：“blob”、“tree”、“commit”和“tag”。

​	无论对象类型如何，所有对象共享以下特征：它们都使用zlib进行了压缩，并且具有标头，该标头不仅指定了它们的类型，还提供有关对象中数据的大小信息。值得注意的是，用于命名对象的SHA-1哈希是原始数据加上此标头的哈希，因此 `sha1sum file` 的*文件*与*文件*的对象名称不匹配。

​	因此，无论对象的内容或类型如何，总是可以独立测试对象的一般一致性：可以通过验证（a）它们的哈希与文件内容匹配，并且（b）对象成功解压缩为一系列`<ascii type without space> + <space> + <ascii decimal size> + <byte\0> + <binary object data>`的字节流来验证它们的内部一致性。

​	结构化对象可以进一步验证其结构和与其他对象的连接。这通常通过 `git fsck` 程序执行，该程序生成所有对象的完整依赖图，并验证它们的内部一致性（除了仅验证它们的浅表一致性通过哈希验证之外）。

### Git 源代码的鸟瞰

​	对于新开发人员来说，找到Git的源代码并不总是容易。本节将为您提供一些指导，以显示从哪里开始。

​	一个很好的起点是查看最初提交的内容：

``` bash
$ git switch --detach e83c5163
```

​	初始版本为Git今天拥有的几乎所有功能奠定了基础，但大小足够小，可以在一次阅读中理解。

​	请注意，术语自初始提交以来已经发生了变化。例如，初始提交中的README使用“changeset”一词来描述我们现在称为[commit](https://git-scm.com/docs/user-manual#def_commit_object)的内容。

​	此外，我们不再称之为“cache”，而是称为“index”；然而，该文件仍然称为`cache.h`。请注意：现在没有太多理由再更改它，特别是因为没有一个好的单一名称，因为它基本上是所有Git的C源文件都包含的*the*头文件。

​	如果您掌握了初始提交中的思想，可以检出一个更新的版本，并浏览`cache.h`、`object.h`和`commit.h`。

​	在早期，Git（遵循UNIX的传统）是一堆极其简单的程序，您可以在脚本中使用这些程序，将一个程序的输出传递给另一个程序。这事实上有利于初步开发，因为测试新功能更容易。但是，最近，许多这些部分已经成为内建命令，一些核心功能已经被“库化”，即放入libgit.a中，以提高性能、可移植性和避免代码重复。

​	现在，您了解了索引是什么（并在`cache.h`中找到了相应的数据结构），以及只有几种对象类型（blob、tree、commit和tag），它们继承自`struct object`，它是它们的第一个成员（因此，您可以将例如`(struct object *)commit`转换为`&commit->object`以达到相同的效果，即获取对象名称和标志）。

​	现在是一个休息的好时机，以便让这些信息沉淀下来。

​	下一步：熟悉对象命名。阅读 [命名commits](https://git-scm.com/docs/user-manual#naming-commits)。有很多方法来为一个对象命名（不仅限于revisions！）。所有这些都在`sha1_name.c`中处理。只需快速查看函数`get_sha1()`。许多特殊处理由函数如`get_sha1_basic()`等执行。

​	这只是为了让您了解Git最大程度的库化部分：revision walker。

​	基本上，`git log` 的初始版本是一个shell脚本：

``` bash
$ git-rev-list --pretty $(git-rev-parse --default HEAD "$@") | \
	LESS=-S ${PAGER:-less}
```

​	这意味着什么？

​	`git rev-list` 是revision walker的原始版本，它始终将一系列revisions打印到stdout。它仍然功能齐全，并且需要这样做，因为大多数新的Git命令都是使用`git rev-list`的脚本开始的。

​	`git rev-parse` 已经不再那么重要了；它仅用于过滤对不同plumbing命令有关的选项，这些命令由脚本调用。

​	`git rev-list` 做的大部分工作都包含在 `revision.c` 和 `revision.h` 中。它将选项封装在名为 `rev_info` 的结构体中，该结构体控制着如何以及哪些修订版本被遍历等。

​	`git rev-parse` 最初的任务现在由函数 `setup_revisions()` 完成，它解析修订版本和修订版本遍历器的常用命令行选项。此信息存储在结构体 `rev_info` 中以供以后使用。在调用 `setup_revisions()` 后，您可以进行自己的命令行选项解析。之后，您必须调用 `prepare_revision_walk()` 进行初始化，然后可以使用函数 `get_revision()` 逐个获取提交。

​	如果您对修订版本遍历过程的更多细节感兴趣，可以查看 `cmd_log()` 的第一个实现；执行 `git show v1.3.0~155^2~4` 并滚动到该函数（请注意，您不再需要直接调用 `setup_pager()`）。

​	现在，`git log` 是一个内建命令，这意味着它*包含在*命令 `git` 中。内建命令的源代码包含：

- 一个名为 `cmd_<bla>` 的函数，通常定义在 `builtin/<bla.c>` 中（请注意，旧版本的Git将其定义在 `builtin-<bla>.c` 中），并在 `builtin.h` 中声明。
- 在 `git.c` 中的 `commands[]` 数组中的条目，
- 在 `Makefile` 中的 `BUILTIN_OBJECTS` 中的条目。

​	有时，一个源文件中包含多个内建命令。例如，`cmd_whatchanged()` 和 `cmd_log()` 都位于 `builtin/log.c` 中，因为它们共享相当多的代码。在这种情况下，不像所在的 `.c` 文件命名的命令必须在 `Makefile` 的 `BUILT_INS` 中列出。

​	`git log` 在C中看起来比原始脚本更复杂，但这样可以实现更大的灵活性和性能。

​	现在是一个适合休息的好时机。

​	第三课是：研究代码。确实，这是了解Git组织结构的最佳方式（在您了解了基本概念之后）。

​	因此，考虑一些您感兴趣的内容，比如“如果只知道blob的对象名称，我怎么访问它？”。第一步是找到可以使用它的Git命令。在这个例子中，要么是 `git show`，要么是 `git cat-file`。

​	为了明确起见，让我们使用 `git cat-file`，因为它： 

- 是plumbing，
- 即使在最初的提交中也存在（在 `cat-file.c` 中仅经历了大约20个修订版本，当它成为内建命令时，将其重命名为 `builtin/cat-file.c`，然后只进行了不到10个版本的修改）。

​	所以，看看 `builtin/cat-file.c`，搜索 `cmd_cat_file()` 并查看它做了什么。

```
        git_config(git_default_config);
        if (argc != 3)
		usage("git cat-file [-t|-s|-e|-p|<type>] <sha1>");
        if (get_sha1(argv[2], sha1))
                die("Not a valid object name %s", argv[2]);
```

​	让我们略过显而易见的细节；这里唯一真正有趣的部分是对 `get_sha1()` 的调用。它试图将 `argv[2]` 解释为对象名称，如果它引用当前仓库中存在的对象，则将结果SHA-1写入变量 `sha1`。

​	以下两点很有趣：

- `get_sha1()` 成功时返回0。这可能会让一些新的Git开发者感到惊讶，但在UNIX中，有一个长期的传统，在不同错误的情况下返回不同的负数值，并在成功的情况下返回0。
- `get_sha1()` 函数签名中的变量 `sha1` 是 `unsigned char *`，但实际上预期是指向 `unsigned char[20]` 的指针。这个变量将包含给定commit的160位SHA-1值。请注意，每当SHA-1作为 `unsigned char *` 传递时，它是二进制表示，与以十六进制字符表示的ASCII表示形式（传递为 `char *`）相对应。

​	您将在代码中看到这两点。

​	接下来是主要部分：

```
        case 0:
                buf = read_object_with_reference(sha1, argv[1], &size, NULL);
```

​	这是如何读取一个blob（实际上不仅限于blob，还包括任何类型的对象）的方法。要了解 `read_object_with_reference()` 函数实际上是如何工作的，请找到它的源代码（在Git仓库中执行类似于 `git grep read_object_with | grep ":[a-z]"` 的命令），并阅读源代码。

​	要了解结果如何使用，只需继续阅读 `cmd_cat_file()`：

```
        write_or_die(1, buf, size);
```

​	有时，您不知道在哪里查找某个功能。在许多这种情况下，通过 `git log` 的输出进行搜索，然后使用 `git show` 显示对应的提交。

​	例如：如果您知道有一些与 `git bundle` 相关的测试用例，但不记得在哪里（是的，您*可以* `git grep bundle t/`，但这并没有说明问题！）：

``` bash
$ git log --no-merges t/
```

​	在分页器（`less`）中，只需搜索“bundle”，然后向前翻动几行，看到它在提交 18449ab0 中。现在只需复制此对象名称，并将其粘贴到命令行中：

``` bash
$ git show 18449ab0
```

Voila。

​	另一个例子：找出使某个脚本成为内建命令所需的步骤：

``` bash
$ git log --no-merges --diff-filter=A builtin/*.c
```

​	您看，Git实际上是了解有关Git源代码的最佳工具！

## Git 术语 - Git Glossary

### Git 解释

- alternate object database - 替代对象数据库

  通过替代机制，一个[仓库](https://git-scm.com/docs/user-manual#def_repository)可以从另一个对象数据库继承部分对象数据库，这个对象数据库被称为 "替代"。

- bare repository - 裸仓库

  裸仓库通常是一个适当命名的[目录](https://git-scm.com/docs/user-manual#def_directory)，具有 `.git` 后缀，它没有本地检出的任何受版本控制的文件副本。也就是说，所有的 Git 管理和控制文件，通常存在于隐藏的 `.git` 子目录中，直接存在于 `repository.git` 目录中，且没有其他文件存在和被检出。通常，公共仓库的发布者提供裸仓库。

- blob object - blob 对象

  无类型的[对象](https://git-scm.com/docs/user-manual#def_object)，例如文件的内容。

- branch - 分支

  "分支" 是一个开发线。分支上的最新[提交](https://git-scm.com/docs/user-manual#def_commit)被称为该分支的顶部。分支的顶部由分支[头引用](https://git-scm.com/docs/user-manual#def_head)所引用，该头引用随着分支上的额外开发而向前移动。单个 Git [仓库](https://git-scm.com/docs/user-manual#def_repository) 可以跟踪任意数量的分支，但是您的[工作树](https://git-scm.com/docs/user-manual#def_working_tree)只与其中之一关联（"当前"或"检出"分支），并且 [HEAD](https://git-scm.com/docs/user-manual#def_HEAD) 指向该分支。

- cache - 缓存

  对于：[索引](https://git-scm.com/docs/user-manual#def_index)。

- chain - 链

  一组对象，列表中的每个对象包含对其后继的引用（例如，[提交](https://git-scm.com/docs/user-manual#def_commit) 的后继可能是它的[父提交](https://git-scm.com/docs/user-manual#def_parent)之一）。

- changeset - 变更集

  BitKeeper/cvsps 中的术语，表示"[提交](https://git-scm.com/docs/user-manual#def_commit)"。由于 Git 不存储变更，而是状态，因此在 Git 中使用术语 "变更集" 实际上是不合适的。

- checkout - 检出

  使用来自[对象数据库](https://git-scm.com/docs/user-manual#def_object_database)中的[树对象](https://git-scm.com/docs/user-manual#def_tree_object)或[blob](https://git-scm.com/docs/user-manual#def_blob_object)更新[工作树](https://git-scm.com/docs/user-manual#def_working_tree)的操作，并更新[索引](https://git-scm.com/docs/user-manual#def_index)和[HEAD](https://git-scm.com/docs/user-manual#def_HEAD)，如果整个工作树已指向新的[分支](https://git-scm.com/docs/user-manual#def_branch)。

- cherry-picking

  在[SCM](https://git-scm.com/docs/user-manual#def_SCM)行话中，"樱桃拣选" 意味着从一系列变更（通常是提交）中选择一部分变更，并将它们记录为基于不同仓库的新一系列变更。在 Git 中，这通过 "git cherry-pick" 命令来完成，该命令提取现有[提交](https://git-scm.com/docs/user-manual#def_commit)引入的更改，并将其作为新的提交基于当前[分支](https://git-scm.com/docs/user-manual#def_branch)的顶部记录。

- clean

  如果[工作树](https://git-scm.com/docs/user-manual#def_working_tree)与当前[头引用](https://git-scm.com/docs/user-manual#def_head)引用的[修订版本](https://git-scm.com/docs/user-manual#def_revision)对应，则[工作树](https://git-scm.com/docs/user-manual#def_working_tree)是"clean"的。另请参阅 "[dirty](https://git-scm.com/docs/user-manual#def_dirty)"。

- commit - 提交

  作为名词：Git 历史中的一个单一点；整个项目的历史被表示为一组相关的提交。"提交" 这个词在 Git 中经常用于与其他版本控制系统中使用的 "修订版本" 或 "版本" 这两个词相同的地方。也用作 [提交对象](https://git-scm.com/docs/user-manual#def_commit_object) 的简称。作为动词：将项目的新状态以新提交的形式存储在 Git 历史中，通过创建一个新的提交来表示当前[索引](https://git-scm.com/docs/user-manual#def_index)的状态，并将[HEAD](https://git-scm.com/docs/user-manual#def_HEAD)前进到指向新的提交。

- commit graph concept, representations and usage - 提交图概念、表示和用法

  表示提交对象数据库中提交所形成的 [DAG](https://git-scm.com/docs/user-manual#def_DAG) 结构的同义词，由分支顶部引用引用，使用它们的链接提交的[链](https://git-scm.com/docs/user-manual#def_chain)。该结构是定义性的提交图。图可以以其他方式表示，例如["提交图" 文件](https://git-scm.com/docs/user-manual#def_commit_graph_file)。

- commit-graph file

  "提交图"（通常连字符连接）文件是提交图的附加表示，加速提交图遍历。"提交图" 文件存储在 `.git/objects/info` 目录中或替代对象数据库的 info 目录中。

- commit object

  包含有关特定[修订版本](https://git-scm.com/docs/user-manual#def_revision)的信息的[对象](https://git-scm.com/docs/user-manual#def_object)，例如[父提交](https://git-scm.com/docs/user-manual#def_parent)、提交者、作者、日期和对应于存储的修订版本顶部[目录](https://git-scm.com/docs/user-manual#def_directory)的[树对象](https://git-scm.com/docs/user-manual#def_tree_object)。

- commit-ish (also committish) - 提交短词（也称为 committish）

  可以递归引用为提交对象的[提交对象](https://git-scm.com/docs/user-manual#def_commit_object)或[对象](https://git-scm.com/docs/user-manual#def_object)。以下都是提交短词：提交对象、指向提交对象的[标签对象](https://git-scm.com/docs/user-manual#def_tag_object)、指向指向提交对象的标签对象的标签对象、等等。

- core Git - 核心 Git

  Git 的基本数据结构和实用工具。仅提供有限的源代码管理工具。

- DAG - 有向无环图

  有向无环图。[提交对象](https://git-scm.com/docs/user-manual#def_commit_object)形成有向无环图，因为它们有父提交（有向），并且提交对象的图是无环的（没有以同一[对象](https://git-scm.com/docs/user-manual#def_object)开始并结束的[链](https://git-scm.com/docs/user-manual#def_chain)）。

- dangling object - 悬挂对象

  一个[无法访问的对象](https://git-scm.com/docs/user-manual#def_unreachable_object)，即使从其他无法访问的对象，也无法访问；悬挂对象在仓库中没有任何引用或从其他引用或[对象](https://git-scm.com/docs/user-manual#def_object)中。

- detached HEAD - 游离 HEAD

  通常，[HEAD](https://git-scm.com/docs/user-manual#def_HEAD) 存储一个[分支](https://git-scm.com/docs/user-manual#def_branch)的名称，对 HEAD 所代表的树进行操作的命令是操作导致 HEAD 指向的分支顶部的历史。但是，Git 也允许您[检出](https://git-scm.com/docs/user-manual#def_checkout)不一定是任何特定分支顶部的任意[提交](https://git-scm.com/docs/user-manual#def_commit)。此状态下的 HEAD 被称为 "脱离 HEAD"。请注意，在 HEAD 被脱离的状态下，对当前分支历史的操作（例如 `git commit` 用于构建新历史）仍然有效。它们将更新 HEAD 指向更新后的历史的顶部，而不会影响任何分支。更新或查询关于当前分支的信息（例如 `git branch --set-upstream-to` 设置当前分支与哪个远程跟踪分支进行整合）的命令在这种状态下显然不起作用，因为在这种状态下没有（真正的）当前分支可以查询。

- directory - 目录

  使用 "ls" 命令获得的列表 :-)

- dirty

  如果[工作树](https://git-scm.com/docs/user-manual#def_working_tree)包含尚未[提交](https://git-scm.com/docs/user-manual#def_commit)到当前[分支](https://git-scm.com/docs/user-manual#def_branch)的更改，则[工作树](https://git-scm.com/docs/user-manual#def_working_tree)被称为 "dirty"。

- evil merge - 恶意合并

  恶意合并是引入未出现在任何[父提交](https://git-scm.com/docs/user-manual#def_parent)中的更改的合并。

- fast-forward

  快速向前是一种特殊类型的[合并](https://git-scm.com/docs/user-manual#def_merge)，其中你有一个[修订版本](https://git-scm.com/docs/user-manual#def_revision)，并且你正在将另一个[分支](https://git-scm.com/docs/user-manual#def_branch)的更改（恰好是你所拥有的修订版本的后代）合并到当前分支。在这种情况下，您不会创建一个新的[合并](https://git-scm.com/docs/user-manual#def_merge)[提交](https://git-scm.com/docs/user-manual#def_commit)，而是将您的分支直接更新为指向要合并的分支的相同修订版本。这在[远程跟踪分支](https://git-scm.com/docs/user-manual#def_remote_tracking_branch)的情况下经常发生，是从远程[仓库](https://git-scm.com/docs/user-manual#def_repository)拉取的分支。

- fetch - 拉取

  拉取一个[分支](https://git-scm.com/docs/user-manual#def_branch)意味着从远程[仓库](https://git-scm.com/docs/user-manual#def_repository)获取该分支的[头引用](https://git-scm.com/docs/user-manual#def_head_ref)，找出本地[对象数据库](https://git-scm.com/docs/user-manual#def_object_database)中缺少的对象，并获取它们。另请参阅 [git-fetch[1]](https://chat.openai.com/1/git-fetch)。

- file system - 文件系统

  Linus Torvalds 最初设计 Git 是作为用户空间文件系统，即容纳文件和目录的基础设施。这确保了 Git 的效率和速度。

- Git archive - Git 存档

  [仓库](https://git-scm.com/docs/user-manual#def_repository) 的同义词（供 arch 用户使用）。

- gitfile - git 文件

  工作树根目录下的一个普通文件 `.git`，指向真正的仓库的目录。

- grafts

  grafts允许将两个本来不同的开发线连接起来，通过为提交记录虚假的祖先信息。这样，您可以让 Git 假装 [提交](https://git-scm.com/docs/user-manual#def_commit) 的[父提交](https://git-scm.com/docs/user-manual#def_parent)与创建提交时记录的不同。通过 `.git/info/grafts` 文件进行配置。请注意，种植机制已过时，并可能导致在仓库之间传输对象时出现问题；参阅 [git-replace[1]](https://chat.openai.com/1/git-replace) 使用更灵活、更可靠的系统完成相同的工作。

- hash

  在 Git 的上下文中，是 [对象名](https://git-scm.com/docs/user-manual#def_object_name) 的同义词。

- head

  [分支](https://git-scm.com/docs/user-manual#def_branch) 顶部的[命名引用](https://git-scm.com/docs/user-manual#def_ref)。HEAD 存储在 `$GIT_DIR/refs/heads/` 目录中的文件中，除非使用了 packed refs（参阅 [git-pack-refs[1]](https://chat.openai.com/1/git-pack-refs)）。

- HEAD

  当前的[分支](https://git-scm.com/docs/user-manual#def_branch)。更详细地说：通常情况下，您的[工作树](https://git-scm.com/docs/user-manual#def_working_tree)是从 HEAD 引用的树的状态派生的。HEAD 是您仓库中某个[分支](https://git-scm.com/docs/user-manual#def_branch)的引用，但在使用 [游离 HEAD](https://git-scm.com/docs/user-manual#def_detached_HEAD) 的情况下，它直接引用任意提交。

- head ref

  [head](https://git-scm.com/docs/user-manual#def_head) 的同义词。

- hook

  在正常执行几个 Git 命令时，会调用可选的脚本，允许开发者添加功能或检查。通常，钩子允许对命令进行预验证并可能中止，以及在操作完成后进行后通知。钩子脚本位于 `$GIT_DIR/hooks/` 目录中，只需从文件名中删除 `.sample` 后缀即可启用它们。在早期的 Git 版本中，您必须使它们可执行。

- index - 索引

  一组文件的统计信息，其内容存储为对象。索引是您[工作树](https://git-scm.com/docs/user-manual#def_working_tree)的存储版本。实际上，它还可以包含第二个甚至第三个[工作树](https://git-scm.com/docs/user-manual#def_working_tree)版本，这些版本在[合并](https://git-scm.com/docs/user-manual#def_merge)时使用。

- index entry - 索引条目

  有关特定文件的信息，存储在[索引](https://git-scm.com/docs/user-manual#def_index)中。如果已经开始[合并](https://git-scm.com/docs/user-manual#def_merge)但尚未完成（即索引中包含该文件的多个版本），则索引条目可能是未合并的。

- master - 主分支

  默认的开发[分支](https://git-scm.com/docs/user-manual#def_branch)。每当创建一个 Git [仓库](https://git-scm.com/docs/user-manual#def_repository) 时，都会创建一个名为 "master" 的分支，并成为活动分支。在大多数情况下，这包含本地开发，尽管这仅仅是惯例，并不是必需的。

- merge - 合并

  作为动词：将另一个[分支](https://git-scm.com/docs/user-manual#def_branch)（可能来自外部[仓库](https://git-scm.com/docs/user-manual#def_repository)）的内容合并到当前分支。如果要合并的分支来自不同的仓库，首先通过[获取](https://git-scm.com/docs/user-manual#def_fetch)远程分支，然后将结果合并到当前分支。这种获取和合并操作的组合被称为 [pull](https://git-scm.com/docs/user-manual#def_pull)。合并是由自动过程执行的，该过程识别自分叉以来所做的更改，然后将所有这些更改一起应用。在冲突的情况下，可能需要手动干预以完成合并。作为名词：除非是 [快速向前](https://git-scm.com/docs/user-manual#def_fast_forward)，否则成功的合并将创建一个新的[提交](https://git-scm.com/docs/user-manual#def_commit)作为当前[分支](https://git-scm.com/docs/user-manual#def_branch)的顶部，该提交将两个合并的分支的内容合并到一个新的提交中。

- object

  Git中存储的存储单元。它通过其内容的[SHA-1](https://git-scm.com/docs/user-manual#def_SHA1)唯一标识。因此，对象是不可更改的。

- object database

  存储一组"对象"，每个[对象](https://git-scm.com/docs/user-manual#def_object)由其[对象名称](https://git-scm.com/docs/user-manual#def_object_name)唯一标识。这些对象通常存储在`$GIT_DIR/objects/`目录中。

- object identifier (oid)

  [对象名称](https://git-scm.com/docs/user-manual#def_object_name)的同义词。

- object name

  [对象](https://git-scm.com/docs/user-manual#def_object)的唯一标识符。对象名称通常由40个字符的十六进制字符串表示。也常称为[SHA-1](https://git-scm.com/docs/user-manual#def_SHA1)。

- object type

  [对象](https://git-scm.com/docs/user-manual#def_object)的标识符之一，包括"[commit](https://git-scm.com/docs/user-manual#def_commit_object)"、"[tree](https://git-scm.com/docs/user-manual#def_tree_object)"、"[tag](https://git-scm.com/docs/user-manual#def_tag_object)"或"[blob](https://git-scm.com/docs/user-manual#def_blob_object)"。

- octopus

  合并多个[分支](https://git-scm.com/docs/user-manual#def_branch)的过程。

- origin

  默认的上游[仓库](https://git-scm.com/docs/user-manual#def_repository)。大多数项目至少有一个上游项目进行跟踪。默认情况下，*origin* 被用于此目的。新的上游更新将被获取到名为 origin/name-of-upstream-branch 的[远程跟踪分支](https://git-scm.com/docs/user-manual#def_remote_tracking_branch)，您可以使用 `git branch -r` 查看这些分支。

- overlay

  仅更新和添加工作目录中的文件，但不删除它们，类似于如何使用 *cp -R* 更新目标目录中的内容。这是在[checkout](https://git-scm.com/docs/user-manual#def_checkout)时默认的模式，用于从[index](https://git-scm.com/docs/user-manual#def_index)或[树状对象](https://git-scm.com/docs/user-manual#def_tree-ish)检出文件。相比之下，no-overlay模式也会删除源中不存在的已跟踪文件，类似于 *rsync --delete*。

- pack

  一组已被压缩成一个文件的对象（以节省空间或高效传输）。

- pack index

  位于[pack](https://git-scm.com/docs/user-manual#def_pack)中的对象的标识符和其他信息，以便有效地访问pack的内容。

- pathspec

  路径模式，用于限制 Git 命令中的路径。

  在 "git ls-files"、"git ls-tree"、"git add"、"git grep"、"git diff"、"git checkout" 和许多其他命令的命令行上使用路径模式，以将操作范围限制为树或工作树的某个子集。请参阅每个命令的文档，了解路径是否相对于当前目录或顶层目录。路径模式的语法如下：

  - any path matches itself
- 任何路径与其本身匹配。
  - 直到最后一个斜杠的路径模式表示一个目录前缀。该路径模式的范围限制为该子树。
- 路径模式的其余部分是剩余路径名的模式。相对于目录前缀的路径将使用 fnmatch(3) 与该模式匹配；特别地，*** 和 *?* *可以*匹配目录分隔符。
  
  例如，`Documentation/*.jpg` 将匹配 Documentation 子树中的所有 `.jpg` 文件，包括 `Documentation/chapter_1/figure_1.jpg`。
  
  以冒号 `:` 开头的路径模式具有特殊含义。在短形式中，冒号 `:` 后跟零个或多个 "magic signature" 字母（可选择由另一个冒号 `:` 终止），其余部分是要与路径匹配的模式。"magic signature" 由既不是字母数字字符、glob、正则表达式特殊字符、也不是冒号的 ASCII 符号组成。如果模式以不属于 "magic signature" 符号集且不是冒号的字符开头，则可以省略终止 "magic signature" 的可选冒号。
  
  在长形式中，冒号 `:` 后跟一个开括号 `(`，一个由逗号分隔的零个或多个 "magic words" 列表，以及一个闭括号 `)`，其余部分是要与路径匹配的模式。

  只有冒号的路径模式表示 "没有路径模式"。不应将此形式与其他路径模式组合使用。

  - top

    魔术词 `top`（魔术签名：`/`）使模式从工作树的根部匹配，即使你在子目录中运行命令。

  - literal

    模式中的通配符，如 `*` 或 `?`，被视为普通字符。

  - icase

    不区分大小写匹配。

  - glob

    Git将模式视为适用于 fnmatch(3) 的shell glob，并带有FNM_PATHNAME标志：模式中的通配符将不匹配路径名中的斜杠。例如，"Documentation/*.html" 匹配 "Documentation/git.html"，但不匹配 "Documentation/ppc/ppc.html" 或 "tools/perf/Documentation/perf.html"。在完整路径名上匹配的连续两个星号（"`**`"）可能具有特殊含义：以斜杠开头的 "`**`" 表示匹配所有目录。例如，"`**/foo`" 匹配任何位置的文件或目录 "`foo`"，与模式 "`foo`" 相同。"`**/foo/bar`" 匹配任何直接位于目录 "`foo`" 下的位置的文件或目录 "`bar`"。尽管有其他连续的星号，但它们被视为无效。Glob魔术与Literal魔术不兼容。

  - attr

    在 `attr:` 后面是一个用空格分隔的 "属性要求" 列表，所有这些要求都必须满足，以便将路径视为匹配；这是除了通常的非魔术路径规范模式匹配之外的附加要求。请参阅 [gitattributes[5]](https://git-scm.com/docs/gitattributes)。对于路径的每个属性要求，可以采用以下形式："`ATTR`" 要求设置属性 `ATTR`。"`-ATTR`" 要求未设置属性 `ATTR`。"`ATTR=VALUE`" 要求属性 `ATTR` 设置为字符串 `VALUE`。"`!ATTR`" 要求属性 `ATTR` 未指定。请注意，当匹配树对象时，属性仍从工作树获取，而不是从给定的树对象获取。

  - exclude

    在路径匹配任何非排除路径规范后，它将通过所有排除路径规范（魔术签名：`!` 或其同义词 `^`）。如果匹配，则路径将被忽略。当没有非排除路径规范时，排除将应用于结果集，就好像没有使用任何路径规范。

- parent

  [提交对象](https://git-scm.com/docs/user-manual#def_commit_object) 包含（可能为空的）逻辑前任（即其父项）列表。

- pickaxe

  术语 "pickaxe" 是 diffcore 程序的一个选项，它帮助选择添加或删除给定文本字符串的更改。通过 `--pickaxe-all` 选项，它可以用于查看引入或删除特定文本行的完整 [changeset](https://git-scm.com/docs/user-manual#def_changeset)。参见 [git-diff[1]](https://chat.openai.com/1/git-diff)。

- plumbing

  "plumbing" 是 [核心 Git](https://git-scm.com/docs/user-manual#def_core_git) 的可爱名称。

- porcelain

  "porcelain" 是依赖于 [核心 Git](https://git-scm.com/docs/user-manual#def_core_git) 的程序和程序套件的可爱名称，它提供对核心 Git 的高级访问。Porcelain 暴露比 [plumbing](https://git-scm.com/docs/user-manual#def_plumbing) 更多的 [SCM](https://git-scm.com/docs/user-manual#def_SCM) 接口。

- per-worktree ref

  "per-worktree ref" 是指每个 [工作树](https://git-scm.com/docs/user-manual#def_worktree) 的引用，而不是全局引用。目前只有 [HEAD](https://git-scm.com/docs/user-manual#def_HEAD) 和以 `refs/bisect/` 开头的任何引用属于 per-worktree ref，但以后可能包括其他异常引用。

- pseudoref

  伪引用是 `$GIT_DIR` 下的文件类，对于 rev-parse 来说它们就像引用一样，但是在 Git 中它们被特殊处理。伪引用的名称全是大写字母，并且总是以包含 [SHA-1](https://git-scm.com/docs/user-manual#def_SHA1) 的一行开始，后面跟着空格。因此，HEAD 不是伪引用，因为它有时是符号引用。它们可能还包含一些附加数据。`MERGE_HEAD` 和 `CHERRY_PICK_HEAD` 就是例子。与 [per-worktree 引用](https://git-scm.com/docs/user-manual#def_per_worktree_ref) 不同，这些文件不能是符号引用，并且没有引用日志。它们也不能通过正常的引用更新机制更新。相反，它们通过直接写入文件来更新。然而，它们可以被读取，就像它们是引用一样，所以 `git rev-parse MERGE_HEAD` 将正常工作。

- pull

  "pull" 一个 [分支](https://git-scm.com/docs/user-manual#def_branch) 意味着将其 [fetch](https://git-scm.com/docs/user-manual#def_fetch) 并 [merge](https://git-scm.com/docs/user-manual#def_merge)。参见 [git-pull[1]](https://chat.openai.com/1/git-pull)。

- push

  "push" 一个 [分支](https://git-scm.com/docs/user-manual#def_branch) 意味着从远程 [仓库](https://git-scm.com/docs/user-manual#def_repository) 获取该分支的 [head ref](https://git-scm.com/docs/user-manual#def_head_ref)，查看它是否是分支的本地 head ref 的祖先，如果是，则将所有从本地 head ref 可达的且远程仓库中缺失的对象放入远程 [对象数据库](https://git-scm.com/docs/user-manual#def_object_database)，并更新远程 head ref。如果远程 [head](https://git-scm.com/docs/user-manual#def_head) 不是本地 head 的祖先，则推送失败。

- reachable

  给定 [提交](https://git-scm.com/docs/user-manual#def_commit) 的所有祖先都被称为 "可达" 于该提交。更一般地，一个 [对象](https://git-scm.com/docs/user-manual#def_object) 从另一个对象是 "可达" 的，如果我们可以通过 [链](https://git-scm.com/docs/user-manual#def_chain) 到达一个对象从而到达另一个对象，该链遵循 [tags](https://git-scm.com/docs/user-manual#def_tag) 到它们标记的任何内容，[commits](https://git-scm.com/docs/user-manual#def_commit_object) 到它们的父项或树，以及 [trees](https://git-scm.com/docs/user-manual#def_tree_object) 到它们包含的树或 [blobs](https://git-scm.com/docs/user-manual#def_blob_object)。

- reachability bitmaps

  可达性位图存储有关包文件或多包索引（MIDX）中所选一组提交的可达性信息，以加速对象搜索。位图存储在 ".bitmap" 文件中。一个仓库最多只能有一个位图文件。位图文件可以属于一个包，也可以属于仓库的多包索引（如果存在的话）。

- rebase

  重新应用一系列更改，从一个 [分支](https://git-scm.com/docs/user-manual#def_branch) 到另一个基准，并将该分支的 [head](https://git-scm.com/docs/user-manual#def_head) 重置为结果。

- ref

  以 `refs/` 开头的名称（例如 `refs/heads/master`），指向对象名称或另一个引用（后者称为 [symbolic ref](https://git-scm.com/docs/user-manual#def_symref)）。为了方便起见，在 Git 命令的参数中使用引用时，有时可以缩写；有关详情，请参阅 [gitrevisions[7]](https://chat.openai.com/7/gitrevisions)。引用存储在 [仓库](https://git-scm.com/docs/user-manual#def_repository) 中。引用命名空间是分层的。不同的子层次结构用于不同的目的（例如，`refs/heads/` 层次结构用于表示本地分支）。有一些特殊用途的引用不以 `refs/` 开头。最显著的例子是 `HEAD`。

- reflog

  引用日志显示引用的本地历史。换句话说，它可以告诉您在 *此* 仓库中的倒数第三个修订是什么，以及昨天下午9:14的当前状态是什么。有关详情，请参阅 [git-reflog[1]](https://chat.openai.com/1/git-reflog)。

- refspec

  "refspec" 用于 [fetch](https://git-scm.com/docs/user-manual#def_fetch) 和 [push](https://git-scm.com/docs/user-manual#def_push) 来描述远程引用和本地引用之间的映射。

- remote repository

  一个 [仓库](https://git-scm.com/docs/user-manual#def_repository)，用于跟踪相同的项目，但位于其他位置。要与远程进行通信，请参阅 [fetch](https://git-scm.com/docs/user-manual#def_fetch) 或 [push](https://git-scm.com/docs/user-manual#def_push)。

- remote-tracking branch

  用于跟踪另一个 [仓库](https://git-scm.com/docs/user-manual#def_repository) 中的更改的 [引用](https://git-scm.com/docs/user-manual#def_ref)。它通常看起来像 *refs/remotes/foo/bar*（表示它跟踪名为 *bar* 的远程名为 *foo* 的分支），并与配置的 fetch [refspec](https://git-scm.com/docs/user-manual#def_refspec) 的右侧匹配。远程跟踪分支不应包含直接的修改，也不应该有针对它的本地提交。

- repository

  包含所有 [引用](https://git-scm.com/docs/user-manual#def_ref) 以及可从引用访问的所有对象的[对象数据库](https://git-scm.com/docs/user-manual#def_object_database)的集合，可能还包含来自一个或多个 [porcelain](https://git-scm.com/docs/user-manual#def_porcelain) 的元数据。一个仓库可以通过 [alternates 机制](https://git-scm.com/docs/user-manual#def_alternate_object_database)与其他仓库共享一个对象数据库。

- resolve

  手动修复失败的自动 [合并](https://git-scm.com/docs/user-manual#def_merge) 留下的问题。

- revision

  [提交](https://git-scm.com/docs/user-manual#def_commit)（名词）的同义词。

- rewind

  抛弃开发的一部分，即将 [head](https://git-scm.com/docs/user-manual#def_head) 分配给一个较早的 [revision](https://git-scm.com/docs/user-manual#def_revision)。

- SCM

  源代码管理（工具）。

- SHA-1

  "Secure Hash Algorithm 1"，一种加密哈希函数。在 Git 的上下文中，它被用作 [对象名称](https://git-scm.com/docs/user-manual#def_object_name) 的同义词。

- shallow clone

  大多数情况下，是 [shallow repository](https://git-scm.com/docs/user-manual#def_shallow_repository) 的同义词，但这个短语更明确地说明它是通过运行 `git clone --depth=...` 命令创建的。

- shallow repository

  浅 [仓库](https://git-scm.com/docs/user-manual#def_repository) 具有不完整的历史，其中部分 [提交](https://git-scm.com/docs/user-manual#def_commit) 的 [parents](https://git-scm.com/docs/user-manual#def_parent) 被切断了（换句话说，Git 被告知这些提交没有父提交，即使它们在 [commit object](https://git-scm.com/docs/user-manual#def_commit_object) 中记录）。这在您仅对项目的最近历史感兴趣时很有用，即使上游记录的真实历史要大得多。通过给 [git-clone[1]](https://chat.openai.com/1/git-clone) 提供 `--depth` 选项可以创建浅仓库，其历史可以在之后使用 [git-fetch[1]](https://chat.openai.com/1/git-fetch) 扩展。

- stash entry

  用于临时存储脏的工作目录和索引内容以供将来重用的 [object](https://git-scm.com/docs/user-manual#def_object)。

- submodule

  在另一个仓库（后者称为 [superproject](https://git-scm.com/docs/user-manual#def_superproject)）内部持有单独项目的历史的 [仓库](https://git-scm.com/docs/user-manual#def_repository)。

- superproject

  引用其工作树中的其他项目的仓库（称为 [submodules](https://git-scm.com/docs/user-manual#def_submodule)）。超级项目了解所包含子模块的 [commit](https://git-scm.com/docs/user-manual#def_commit_object) 名称（但不持有副本）。

- symref

  符号引用：它不包含 [SHA-1](https://git-scm.com/docs/user-manual#def_SHA1) id 本身，而是具有 *ref: refs/some/thing* 格式，并且在引用时递归地解引用到该引用。*[HEAD](https://git-scm.com/docs/user-manual#def_HEAD)* 就是符号引用的一个典型例子。符号引用可以使用 [git-symbolic-ref[1]](https://chat.openai.com/1/git-symbolic-ref) 命令来操作。

- tag

  `refs/tags/` 命名空间下的 [ref](https://git-scm.com/docs/user-manual#def_ref)，指向任意类型的对象（通常标签指向 [tag](https://git-scm.com/docs/user-manual#def_tag_object) 或 [commit object](https://git-scm.com/docs/user-manual#def_commit_object)）。与 [head](https://git-scm.com/docs/user-manual#def_head) 不同，标签不会由 `commit` 命令更新。Git 标签与 Lisp 标签（在 Git 上下文中称为 [object type](https://git-scm.com/docs/user-manual#def_object_type)）无关。标签最常用于标记提交历史 [链](https://git-scm.com/docs/user-manual#def_chain) 中的特定点。

- tag object

  一个包含指向另一个对象的 [ref](https://git-scm.com/docs/user-manual#def_ref) 的[对象](https://git-scm.com/docs/user-manual#def_object)，它可以像[提交对象](https://git-scm.com/docs/user-manual#def_commit_object)一样包含消息。如果还包含 (PGP) 签名，则称为 "signed tag object"（签名标签对象）。

- topic branch

  开发人员用来标识一个概念性开发线的常规 Git [分支](https://git-scm.com/docs/user-manual#def_branch)。由于分支非常容易和廉价，通常希望有几个小分支，每个分支都包含非常明确定义的概念或相关的小增量更改。

- tree

  一个[工作树](https://git-scm.com/docs/user-manual#def_working_tree)，或者包含依赖的[对象](https://git-scm.com/docs/user-manual#def_object)和树对象的[tree object](https://git-scm.com/docs/user-manual#def_tree_object)（即工作树的存储表示）。

- tree object

  一个包含文件名和模式列表以及与关联的 blob 和/或树对象的[ref](https://git-scm.com/docs/user-manual#def_ref)的[对象](https://git-scm.com/docs/user-manual#def_object)。树等同于[目录](https://git-scm.com/docs/user-manual#def_directory)。

- tree-ish (also treeish)

  可以递归地解引用为树对象的[tree object](https://git-scm.com/docs/user-manual#def_tree_object)或[对象](https://git-scm.com/docs/user-manual#def_object)。解引用[提交对象](https://git-scm.com/docs/user-manual#def_commit_object)会产生与[revision](https://git-scm.com/docs/user-manual#def_revision)的顶级[目录](https://git-scm.com/docs/user-manual#def_directory)对应的树对象。以下都是 tree-ish：[commit-ish](https://git-scm.com/docs/user-manual#def_commit-ish)、树对象、指向树对象的[标签对象](https://git-scm.com/docs/user-manual#def_tag_object)、指向指向树对象的标签对象的标签对象、以此类推。

- unmerged index

  包含未合并[索引条目](https://git-scm.com/docs/user-manual#def_index_entry)的[索引](https://git-scm.com/docs/user-manual#def_index)。

- unreachable object

  一个从[分支](https://git-scm.com/docs/user-manual#def_branch)、[标签](https://git-scm.com/docs/user-manual#def_tag)或任何其他引用无法[访问](https://git-scm.com/docs/user-manual#def_reachable)的[对象](https://git-scm.com/docs/user-manual#def_object)。

- upstream branch

  默认合并到所讨论的分支的[分支](https://git-scm.com/docs/user-manual#def_branch)（或所讨论的分支被变基到的分支）。通过 branch.<name>.remote 和 branch.<name>.merge 进行配置。如果 *A* 的上游分支是 *origin/B*，有时我们说 "*A* is tracking *origin/B*"。

- working tree

  实际签出文件的树。工作树通常包含[HEAD](https://git-scm.com/docs/user-manual#def_HEAD)提交的树内容，以及您进行的但尚未提交的本地更改。

- worktree

  一个仓库可以没有工作树（即裸仓库），也可以有一个或多个附加的工作树。一个 "工作树" 由 "工作树" 和仓库元数据组成，其中大部分元数据在单个仓库的其他工作树之间共享，而一些元数据在每个工作树之间单独维护（例如索引、HEAD 和伪引用，如 MERGE_HEAD、每个工作树的引用和每个工作树的配置文件）。

## 附录 A：Git 快速参考

​	这是主要命令的快速摘要；前面的章节更详细地解释了这些命令的工作原理。

### 创建一个新的仓库

​	从一个 tarball 文件创建：

``` bash
$ tar xzf project.tar.gz
$ cd project
$ git init
Initialized empty Git repository in .git/
$ git add .
$ git commit
```

​	从一个远程仓库克隆：

``` bash
$ git clone git://example.com/pub/project.git
$ cd project
```

### 管理分支

``` bash
$ git branch			# 列出该仓库中的所有本地分支
$ git switch test	    # "test" 切换到分支 "test"
$ git branch new		# 创建从当前 HEAD 开始的分支 "new"HEAD
$ git branch -d new		# 删除分支 "new"
```

Instead of basing a new branch on current HEAD (the default), use:

​	不基于当前 HEAD（默认情况下）创建新分支，使用：

``` bash
$ git branch new test    # 以 "test" 为基础创建新分支
$ git branch new v2.6.15 # 以标签 v2.6.15 为基础创建新分支
$ git branch new HEAD^   # 以最近提交的前一次提交为基础
$ git branch new HEAD^^  # 以前一次提交的前一次提交为基础
$ git branch new test~10 # 以分支 "test" 最新提交之前的 10 次提交为基础
```

​	同时创建并切换到新分支：

``` bash
$ git switch -c new v2.6.15
```

​	从克隆的仓库更新和查看分支：

``` bash
$ git fetch		# update 更新
$ git branch -r		# list 列出
  origin/master
  origin/next
  ...
$ git switch -c masterwork origin/master
```

​	从不同的仓库获取一个分支，并在你的仓库中赋予它一个新名称：

``` bash
$ git fetch git://example.com/project.git theirbranch:mybranch
$ git fetch git://example.com/project.git v2.6.15:mybranch
```

​	保持对你经常使用的仓库的列表：

``` bash
$ git remote add example git://example.com/project.git
$ git remote			# 列出远程仓库
example
origin
$ git remote show example	# 获取详细信息
* remote example
  URL: git://example.com/project.git
  Tracked remote branches
    master
    next
    ...
$ git fetch example		# 从 example 更新分支
$ git branch -r			# 列出所有远程分支
```

### 探索历史

``` bash
$ gitk			    # 可视化浏览历史记录
$ git log		    # 列出所有提交记录
$ git log src/		    # ...列出修改 src/ 的提交
$ git log v2.6.15..v2.6.16  # ...列出在 v2.6.16 中，不在 v2.6.15 中的提交
$ git log master..test	    # ...列出在分支 test 中，不在分支 master 中的提交
$ git log test..master	    # ...列出在分支 master 中，不在分支 test 中的提交
$ git log test...master	    # ...列出在一个分支中，不在两个分支中的提交
$ git log -S'foo()'	    # ...差异包含 "foo()" 的提交
$ git log --since="2 weeks ago"
$ git log -p		    # 显示补丁信息
$ git show		    # 最近的提交
$ git diff v2.6.15..v2.6.16 # 两个标签版本之间的差异
$ git diff v2.6.15..HEAD    # 与当前 HEAD 的差异
$ git grep "foo()"	    # 在工作目录中搜索 "foo()"
$ git grep v2.6.15 "foo()"  # 在旧版本树中搜索 "foo()"
$ git show v2.6.15:a.txt    # 查看 a.txt 的旧版本
```

Search for regressions:

​	寻找回归问题：

``` bash
$ git bisect start
$ git bisect bad		# 当前版本是有问题的
$ git bisect good v2.6.13-rc2	# 最近已知的好的版本
Bisecting: 675 revisions left to test after this
				# 在这里进行测试，然后：
$ git bisect good		# 如果该版本是好的，或者
$ git bisect bad		# 如果该版本是有问题的。
				# 重复上述步骤直到完成。
```

### 进行更改

​	确保 Git 知道责任者是谁：

``` bash
$ cat >>~/.gitconfig <<\EOF
[user]
	name = Your Name Comes Here
	email = you@yourdomain.example.com
EOF
```

​	选择要包含在下一次提交中的文件内容，然后进行提交：

``` bash
$ git add a.txt    # updated file 更新的文件
$ git add b.txt    # new file 新文件
$ git rm c.txt     # old file 旧文件
$ git commit
```

​	或者，在一步中准备并创建提交：

``` bash
$ git commit d.txt # 仅使用 d.txt 的最新内容
$ git commit -a	   # 使用所有已跟踪文件的最新内容
```

### 合并

``` bash
$ git merge test   # 将分支 "test" 合并到当前分支
$ git pull git://example.com/project.git master
		   # 从远程分支获取并合并
$ git pull . test  # 等同于 git merge test
```

### 共享你的更改

Importing or atches:

​	导入或导出补丁：

``` bash
$ git format-patch origin..HEAD # format a patch for each commit 为 HEAD 中的每个提交格式化一个补丁
				# in HEAD but not in origin 但不在 origin 中的提交
$ git am mbox # import patches from the mailbox "mbox" 从邮箱 "mbox" 导入补丁
```

​	从不同的 Git 仓库获取一个分支，然后合并到当前分支：

``` bash
$ git pull git://example.com/project.git theirbranch
```

​	将获取的分支存储到本地分支，然后合并到当前分支：

``` bash
$ git pull git://example.com/project.git theirbranch:mybranch
```

​	在创建提交的本地分支后，用你的提交更新远程分支：

``` bash
$ git push ssh://example.com/project.git mybranch:theirbranch
```

​	当远程分支和本地分支都命名为 "test" 时：

``` bash
$ git push ssh://example.com/project.git test
```

​	常用远程仓库的快捷版本：

``` bash
$ git remote add example ssh://example.com/project.git
$ git push example test
```

### 仓库维护

​	检查是否存在损坏：

``` bash
$ git fsck
```

​	重新压缩，移除未使用的垃圾：

``` bash
$ git gc
```

## 附录 B：本手册的注释和 todo list

### Todo list

​	本手册还在不断完善中。

​	基本要求：

- 必须能够被具有基本UNIX命令行知识的有智慧的人按顺序从头到尾阅读，而无需对Git有任何特殊了解。必要时，其他前提条件应在涉及的地方明确说明。
- 在可能的情况下，章节标题应明确描述它们所解释的任务，使用尽可能简单的语言，只需要必要的知识，例如，“将补丁导入项目”而不是“`git am`命令”。

​	考虑如何创建一个清晰的章节依赖图，使人们能够在不必阅读中间所有内容的情况下获得重要的主题。

​	扫描 `Documentation/` 以查找其他遗漏的内容；特别是：

- howto’s
- some of `technical/`?
- hooks
- list of commands in [git[1]](../1/git)

​	扫描电子邮件存档以查找其他git遗漏的内容。

​	查看手册页面，看看是否有任何假设比本手册提供的背景知识更多。

​	添加更多良好的示例。可以单独创建仅包含示例的章节，也许将“高级示例”作为标准的章节放在每个章节的末尾？

​	在适当的地方添加对词汇表的交叉引用。

​	添加关于与其他版本控制系统（包括CVS、Subversion以及一系列发布tarball导入）的合作的章节。

​	撰写关于使用plumbing和编写脚本的章节。

​	涉及到Alternates、clone -reference等内容。

进一步介绍从仓库损坏中恢复。参见：

- [https://lore.kernel.org/git/Pine.LNX.4.64.0702272039540.12485@woody.linux-foundation.org/](https://lore.kernel.org/git/Pine.LNX.4.64.0702272039540.12485@woody.linux-foundation.org/ ) 

- [https://lore.kernel.org/git/Pine.LNX.4.64.0702141033400.3604@woody.linux-foundation.org/](https://lore.kernel.org/git/Pine.LNX.4.64.0702141033400.3604@woody.linux-foundation.org/)

