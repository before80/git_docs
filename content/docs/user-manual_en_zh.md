+++
title = "用户手册——中英对照版"
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

Git is a fast distributed revision control system.

​	Git 是一个快速的分布式版本控制系统。

This manual is designed to be readable by someone with basic UNIX command-line skills, but no previous knowledge of Git.

​	本手册旨在适合具有基本UNIX命令行技能但没有Git先前知识的人阅读。

[Repositories and Branches](https://git-scm.com/docs/user-manual#repositories-and-branches) and [Exploring Git history](https://git-scm.com/docs/user-manual#exploring-git-history) explain how to fetch and study a project using git—read these chapters to learn how to build and test a particular version of a software project, search for regressions, and so on.

​	[Repositories and Branches](https://git-scm.com/docs/user-manual#repositories-and-branches)和[Exploring Git history](https://git-scm.com/docs/user-manual#exploring-git-history) 解释了如何使用git获取和研究项目。阅读这些章节可以了解如何构建和测试软件项目的特定版本，搜索回归等等。

People needing to do actual development will also want to read [Developing with Git](https://git-scm.com/docs/user-manual#Developing-With-git) and [Sharing development with others](https://git-scm.com/docs/user-manual#sharing-development).

​	需要进行实际开发的人还应阅读[Developing with Git](https://git-scm.com/docs/user-manual#Developing-With-git)和[Sharing development with others](https://git-scm.com/docs/user-manual#sharing-development)。

Further chapters cover more specialized topics.

​	更多章节涵盖更多专业主题。

Comprehensive reference documentation is available through the man pages, or [git-help[1]](../1/git-help) command. For example, for the command `git clone <repo>`, you can either use:

​	完整的参考文档可通过man页或[git-help[1]](../1/git-help)命令获得。例如，对于命令 `git clone <repo>`，您可以选择使用：

``` bash
$ man git-clone
```

或者：

``` bash
$ git help clone
```

With the latter, you can use the manual viewer of your choice; see [git-help[1]](../1/git-help) for more information.

​	使用后者，您可以使用您选择的手册查看器；有关更多信息，请参见[git-help[1]](../1/git-help)。

See also [Git Quick Reference](https://git-scm.com/docs/user-manual#git-quick-start) for a brief overview of Git commands, without any explanation.

​	还请查看[Git快速参考](https://git-scm.com/docs/user-manual#git-quick-start)，了解Git命令的简要概述，不含任何解释。

Finally, see [Notes and todo list for this manual](https://git-scm.com/docs/user-manual#todo) for ways that you can help make this manual more complete.

​	最后，请参阅[此手册的注释和待办事项清单](https://git-scm.com/docs/user-manual#todo)，以了解如何帮助完善本手册。

## 仓库和分支

### 获取Git仓库的方法

It will be useful to have a Git repository to experiment with as you read this manual.

​	在阅读本手册时，有一个Git仓库供您实验会很有用。

The best way to get one is by using the [git-clone[1]](../1/git-clone) command to download a copy of an existing repository. If you don’t already have a project in mind, here are some interesting examples:

​	最佳方法是使用[git-clone[1]](../1/git-clone)命令下载现有仓库的副本。如果您还没有项目的选择，请参考以下一些有趣的示例：

```
	# Git itself (approx. 40MB download):
$ git clone git://git.kernel.org/pub/scm/git/git.git
	# the Linux kernel (approx. 640MB download):
$ git clone git://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git
```

The initial clone may be time-consuming for a large project, but you will only need to clone once.

​	对于大型项目，初始克隆可能需要较长时间，但您只需要克隆一次。

The clone command creates a new directory named after the project (`git` or `linux` in the examples above). After you cd into this directory, you will see that it contains a copy of the project files, called the [working tree](https://git-scm.com/docs/user-manual#def_working_tree), together with a special top-level directory named `.git`, which contains all the information about the history of the project.

​	克隆命令会创建一个以项目名称（上述示例中为`git`或`linux`）命名的新目录。进入此目录后，您将看到它包含了一个项目文件的副本，称为[工作树](https://git-scm.com/docs/user-manual#def_working_tree)，以及一个特殊的顶级目录`.git`，其中包含有关项目历史的所有信息。

### 如何检出项目的不同版本

Git is best thought of as a tool for storing the history of a collection of files. It stores the history as a compressed collection of interrelated snapshots of the project’s contents. In Git each such version is called a [commit](https://git-scm.com/docs/user-manual#def_commit).

​	Git最适合用作存储一组文件历史的工具。它将历史存储为项目内容的一组压缩的相互关联的快照。在Git中，每个这样的版本称为[commit](https://git-scm.com/docs/user-manual#def_commit)。

Those snapshots aren’t necessarily all arranged in a single line from oldest to newest; instead, work may simultaneously proceed along parallel lines of development, called [branches](https://git-scm.com/docs/user-manual#def_branch), which may merge and diverge.

​	这些快照并不一定全部按照从最旧到最新的单一线排列；相反，工作可以同时沿着并行的开发线进行，称为[分支](https://git-scm.com/docs/user-manual#def_branch)，这些分支可以合并和分叉。

A single Git repository can track development on multiple branches. It does this by keeping a list of [heads](https://git-scm.com/docs/user-manual#def_head) which reference the latest commit on each branch; the [git-branch[1]](../1/git-branch) command shows you the list of branch heads:

​	单个Git仓库可以跟踪多个分支上的开发。它通过保持一个引用每个分支上最新提交的[头部](https://git-scm.com/docs/user-manual#def_head)列表来实现；[git-branch[1]](../1/git-branch)命令显示分支头列表：

``` bash
$ git branch
* master
```

A freshly cloned repository contains a single branch head, by default named "master", with the working directory initialized to the state of the project referred to by that branch head.

​	新克隆的仓库默认包含一个分支头，名为“master”，并且工作目录初始化为该分支头指向的项目状态。

Most projects also use [tags](https://git-scm.com/docs/user-manual#def_tag). Tags, like heads, are references into the project’s history, and can be listed using the [git-tag[1]](../1/git-tag) command:

​	大多数项目还使用[标签](https://git-scm.com/docs/user-manual#def_tag)。标签与分支一样，是对项目历史的引用，可以使用[git-tag[1]](../1/git-tag)命令列出标签：

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

Tags are expected to always point at the same version of a project, while heads are expected to advance as development progresses.

​	标签始终指向项目的相同版本，而分支则预期随着开发的进行而不断推进。

Create a new branch head pointing to one of these versions and check it out using [git-switch[1]](../1/git-switch):

​	创建一个新的分支头，指向这些版本之一，并使用[git-switch[1]](../1/git-switch)检出它：

``` bash
$ git switch -c new v2.6.13
```

The working directory then reflects the contents that the project had when it was tagged v2.6.13, and [git-branch[1]](../1/git-branch) shows two branches, with an asterisk marking the currently checked-out branch:

​	工作目录将反映出项目在标记v2.6.13时的内容，并且[git-branch[1]](../1/git-branch)显示两个分支，其中星号标记当前检出的分支：

``` bash
$ git branch
  master
* new
```

If you decide that you’d rather see version 2.6.17, you can modify the current branch to point at v2.6.17 instead, with

​	如果您决定更换到2.6.17版本，可以使用以下命令将当前分支指向v2.6.17：

``` bash
$ git reset --hard v2.6.17
```

Note that if the current branch head was your only reference to a particular point in history, then resetting that branch may leave you with no way to find the history it used to point to; so use this command carefully.

请注意，如果当前分支头是对特定历史点的唯一引用，那么重置该分支可能会导致您无法找到它曾指向的历史；因此请谨慎使用此命令。

### 理解历史记录：Commits

Every change in the history of a project is represented by a commit. The [git-show[1]](../1/git-show) command shows the most recent commit on the current branch:

​	项目历史中的每个更改都由一个commit表示。[git-show[1]](../1/git-show)命令显示当前分支上最近的提交：

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

As you can see, a commit shows who made the latest change, what they did, and why.

​	如您所见，commit显示了谁进行了最新更改，他们做了什么以及为什么。

Every commit has a 40-hexdigit id, sometimes called the "object name" or the "SHA-1 id", shown on the first line of the `git show` output. You can usually refer to a commit by a shorter name, such as a tag or a branch name, but this longer name can also be useful. Most importantly, it is a globally unique name for this commit: so if you tell somebody else the object name (for example in email), then you are guaranteed that name will refer to the same commit in their repository that it does in yours (assuming their repository has that commit at all). Since the object name is computed as a hash over the contents of the commit, you are guaranteed that the commit can never change without its name also changing.

​	每个commit都有一个40位的十六进制id，有时称为"object name"或"SHA-1 id"，显示在`git show`输出的第一行上。通常您可以用较短的名字引用一个commit，例如标签或分支名称，但是这个较长的名字也可能很有用。最重要的是，它是该commit的全局唯一名称：因此，如果您将对象名称告诉其他人（例如通过电子邮件），那么您可以确保该名称在他们的仓库中引用的是与您的仓库中相同的commit（假设他们的仓库中确实有该commit）。由于对象名称是通过对commit内容进行哈希计算得到的，所以可以确保commit不会在其名称不变的情况下发生更改。

In fact, in [Git concepts](https://git-scm.com/docs/user-manual#git-concepts) we shall see that everything stored in Git history, including file data and directory contents, is stored in an object with a name that is a hash of its contents.

​	实际上，在[Git概念](https://git-scm.com/docs/user-manual#git-concepts)中我们将看到Git历史中存储的所有内容，包括文件数据和目录内容，都存储在具有其内容哈希的对象中。

#### 理解历史：commits、parents和reachability 

Every commit (except the very first commit in a project) also has a parent commit which shows what happened before this commit. Following the chain of parents will eventually take you back to the beginning of the project.

​	每个commit（除了项目中的第一个commit）还有一个父commit，显示了此commit之前发生了什么。跟随父commit的链条最终会将您带回到项目的开始。

However, the commits do not form a simple list; Git allows lines of development to diverge and then reconverge, and the point where two lines of development reconverge is called a "merge". The commit representing a merge can therefore have more than one parent, with each parent representing the most recent commit on one of the lines of development leading to that point.

​	但是，commits不会形成简单的列表；Git允许开发线同时分叉和合并，两条开发线在哪里再次汇合被称为“合并点”。表示合并的commit因此可能具有多个父commit，其中每个父commit表示通往该点的一条开发线上最近的commit。

The best way to see how this works is using the [gitk[1]](../1/gitk) command; running gitk now on a Git repository and looking for merge commits will help understand how Git organizes history.

​	最好的方法是使用[gitk[1]](../1/gitk)命令来查看它是如何工作的；现在在Git仓库上运行gitk，并查找合并commit将有助于理解Git如何组织历史。

In the following, we say that commit X is "reachable" from commit Y if commit X is an ancestor of commit Y. Equivalently, you could say that Y is a descendant of X, or that there is a chain of parents leading from commit Y to commit X.

​	在下面，我们说如果commit X“可达”commit Y，则commit X是commit Y的祖先。同样地，您可以说Y是X的后代，或者说有一条从commit Y到commit X的父链。

#### 理解历史：历史图表

We will sometimes represent Git history using diagrams like the one below. Commits are shown as "o", and the links between them with lines drawn with - / and \. Time goes left to right:

​	我们有时使用如下的图表来表示Git历史。Commits显示为"o"，它们之间的链接用绘制线表示：- /和\。时间从左到右：

```
         o--o--o <-- Branch A
        /
 o--o--o <-- master
        \
         o--o--o <-- Branch B
```

If we need to talk about a particular commit, the character "o" may be replaced with another letter or number.

​	如果我们需要讨论特定的commit，字符"o"可能会替换为其他字母或数字。

#### 理解历史：什么是分支？

When we need to be precise, we will use the word "branch" to mean a line of development, and "branch head" (or just "head") to mean a reference to the most recent commit on a branch. In the example above, the branch head named "A" is a pointer to one particular commit, but we refer to the line of three commits leading up to that point as all being part of "branch A".

​	当我们需要精确时，我们将使用"branch"一词表示开发线，"branch head"（或"head"）表示对分支上最新commit的引用。在上面的示例中，名为"A"的分支头是对一个特定commit的指针，但我们将这个指向该点的三个commit的行称为"branch A"的一部分。

However, when no confusion will result, we often just use the term "branch" both for branches and for branch heads.

​	然而，当不会导致混淆时，我们通常只使用"branch"这个术语来表示分支和分支头。

### 操作分支

Creating, deleting, and modifying branches is quick and easy; here’s a summary of the commands:

​	创建、删除和修改分支都很快捷简单；以下是这些命令的摘要：

- `git branch`

  list all branches.

  列出所有分支。

- `git branch <branch>`

  create a new branch named `<branch>`, referencing the same point in history as the current branch.

  创建一个名为`<branch>`的新分支，它引用与当前分支相同的历史点。

- `git branch <branch> <start-point>`

  create a new branch named `<branch>`, referencing `<start-point>`, which may be specified any way you like, including using a branch name or a tag name.

  创建一个名为`<branch>`的新分支，它引用`<start-point>`，可以以任何您喜欢的方式指定它，包括使用分支名称或标签名称。

- `git branch -d <branch>`

  delete the branch `<branch>`; if the branch is not fully merged in its upstream branch or contained in the current branch, this command will fail with a warning.

  删除分支`<branch>`；如果分支未完全合并到其上游分支或包含在当前分支中，则此命令将带有警告的失败。

- `git branch -D <branch>`

  delete the branch `<branch>` irrespective of its merged status.

  无视分支合并状态删除分支`<branch>`。

- `git switch <branch>`

  make the current branch `<branch>`, updating the working directory to reflect the version referenced by `<branch>`.

  将当前分支切换为`<branch>`，更新工作目录以反映`<branch>`引用的版本。

- `git switch -c <new> <start-point>`

  create a new branch `<new>` referencing `<start-point>`, and check it out.
  
  创建一个名为`<new>`的新分支，引用`<start-point>`，然后检出它。

The special symbol "HEAD" can always be used to refer to the current branch. In fact, Git uses a file named `HEAD` in the `.git` directory to remember which branch is current:

​	特殊符号"HEAD"始终可以用来指代当前分支。实际上，Git使用`.git`目录中的`HEAD`文件来记住当前的分支：

``` bash
$ cat .git/HEAD
ref: refs/heads/master
```

### 不创建新分支查看旧版本

The `git switch` command normally expects a branch head, but will also accept an arbitrary commit when invoked with --detach; for example, you can check out the commit referenced by a tag:

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

The HEAD then refers to the SHA-1 of the commit instead of to a branch, and git branch shows that you are no longer on a branch:

​	然后，HEAD将指向提交的SHA-1，而不是分支，`git branch`显示您不再在分支上：

``` bash
$ cat .git/HEAD
427abfa28afedffadfca9dd8b067eb6d36bac53f
$ git branch
* (detached from v2.6.17)
  master
```

In this case we say that the HEAD is "detached".

​	在这种情况下，我们称HEAD为"detached"。

This is an easy way to check out a particular version without having to make up a name for the new branch. You can still create a new branch (or tag) for this version later if you decide to.

​	这是一种无需为新分支编写名称就可以检查特定版本的简便方法。如果决定，稍后仍然可以为该版本创建新的分支（或标签）。

### 查看远程仓库的分支

The "master" branch that was created at the time you cloned is a copy of the HEAD in the repository that you cloned from. That repository may also have had other branches, though, and your local repository keeps branches which track each of those remote branches, called remote-tracking branches, which you can view using the `-r` option to [git-branch[1]](../1/git-branch):

​	在克隆时创建的"master"分支是克隆源仓库中HEAD的副本。然而，该仓库可能还有其他分支，而您的本地仓库将保留每个远程分支的跟踪分支，称为远程跟踪分支，您可以使用`-r`选项查看它们，例如[git-branch[1]](../1/git-branch)：

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

In this example, "origin" is called a remote repository, or "remote" for short. The branches of this repository are called "remote branches" from our point of view. The remote-tracking branches listed above were created based on the remote branches at clone time and will be updated by `git fetch` (hence `git pull`) and `git push`. See [Updating a repository with git fetch](https://git-scm.com/docs/user-manual#Updating-a-repository-With-git-fetch) for details.

​	在这个例子中，"origin"被称为远程仓库，或简称为"remote"。从我们的角度来看，该仓库的分支称为"remote branches"。上面列出的远程跟踪分支是基于克隆时的远程分支创建的，并且将通过`git fetch`（因此`git pull`）和`git push`进行更新。详情请参见[使用git fetch更新仓库](https://git-scm.com/docs/user-manual#Updating-a-repository-With-git-fetch)。

You might want to build on one of these remote-tracking branches on a branch of your own, just as you would for a tag:

​	您可能希望在自己的分支上构建其中一个远程跟踪分支，就像构建标签一样：

``` bash
$ git switch -c my-todo-copy origin/todo
```

You can also check out `origin/todo` directly to examine it or write a one-off patch. See [detached head](https://git-scm.com/docs/user-manual#detached-head).

​	您还可以直接检出`origin/todo`来查看它或编写一次性的补丁。详情请参见[分离头指针](https://git-scm.com/docs/user-manual#detached-head)。

Note that the name "origin" is just the name that Git uses by default to refer to the repository that you cloned from.

​	请注意，"origin"只是Git默认用于引用克隆源仓库的名称。

### 命名分支、标签和其他引用

Branches, remote-tracking branches, and tags are all references to commits. All references are named with a slash-separated path name starting with `refs`; the names we’ve been using so far are actually shorthand:

​	分支、远程跟踪分支和标签都是对commits的引用。所有引用都以以`refs`开头的斜杠分隔路径名命名；我们目前使用的名称实际上是缩写：

- The branch `test` is short for `refs/heads/test`.
- The tag `v2.6.18` is short for `refs/tags/v2.6.18`.
- `origin/master` is short for `refs/remotes/origin/master`.
- 分支`test`的缩写为`refs/heads/test`。
- 标签`v2.6.18`的缩写为`refs/tags/v2.6.18`。
- `origin/master`的缩写为`refs/remotes/origin/master`。

The full name is occasionally useful if, for example, there ever exists a tag and a branch with the same name.

​	完整名称有时在有多个具有相同简写名称的引用时很有用。

(Newly created refs are actually stored in the `.git/refs` directory, under the path given by their name. However, for efficiency reasons they may also be packed together in a single file; see [git-pack-refs[1]](../1/git-pack-refs)).

（新创建的引用实际上存储在`.git/refs`目录中，路径是其名称给出的路径。但是，出于效率考虑，它们也可能被打包在一个单独的文件中；详情请参见[git-pack-refs[1]](../1/git-pack-refs)）。

As another useful shortcut, the "HEAD" of a repository can be referred to just using the name of that repository. So, for example, "origin" is usually a shortcut for the HEAD branch in the repository "origin".

​	作为另一个有用的快捷方式，一个仓库的"HEAD"可以使用该仓库的名称来表示。例如，"origin"通常是指向仓库"origin"的"HEAD"分支的快捷方式。

For the complete list of paths which Git checks for references, and the order it uses to decide which to choose when there are multiple references with the same shorthand name, see the "SPECIFYING REVISIONS" section of [gitrevisions[7]](../7/gitrevisions).

​	有关Git检查引用的完整路径列表，以及在有多个具有相同简写名称的引用时决定选择哪个的顺序，请参阅[gitrevisions[7]](../7/gitrevisions)的"SPECIFYING REVISIONS"部分。



### 使用git fetch更新仓库

After you clone a repository and commit a few changes of your own, you may wish to check the original repository for updates.

​	在克隆一个仓库并提交了一些自己的更改后，您可能希望检查原始仓库是否有更新。

The `git-fetch` command, with no arguments, will update all of the remote-tracking branches to the latest version found in the original repository. It will not touch any of your own branches—not even the "master" branch that was created for you on clone.

​	没有参数的`git-fetch`命令将更新所有远程跟踪分支到原始仓库中找到的最新版本。它不会影响您自己的任何分支，甚至不会影响在克隆时为您创建的"master"分支。

### 从其他仓库获取分支

You can also track branches from repositories other than the one you cloned from, using [git-remote[1]](../1/git-remote):

​	您还可以使用[git-remote[1]](../1/git-remote)从其他仓库跟踪分支，而不仅限于克隆的仓库：

``` bash
$ git remote add staging git://git.kernel.org/.../gregkh/staging.git
$ git fetch staging
...
From git://git.kernel.org/pub/scm/linux/kernel/git/gregkh/staging
 * [new branch]      master     -> staging/master
 * [new branch]      staging-linus -> staging/staging-linus
 * [new branch]      staging-next -> staging/staging-next
```

New remote-tracking branches will be stored under the shorthand name that you gave `git remote add`, in this case `staging`:

​	新的远程跟踪分支将存储在您给出的`git remote add`的简写名称下，例如在本例中为`staging`：

``` bash
$ git branch -r
  origin/HEAD -> origin/master
  origin/master
  staging/master
  staging/staging-linus
  staging/staging-next
```

If you run `git fetch <remote>` later, the remote-tracking branches for the named `<remote>` will be updated.

​	如果以后运行`git fetch <remote>`，将更新指定`<remote>`的远程跟踪分支。

If you examine the file `.git/config`, you will see that Git has added a new stanza:

​	如果检查`.git/config`文件，您会看到Git已添加一个新的部分：

``` bash
$ cat .git/config
...
[remote "staging"]
	url = git://git.kernel.org/pub/scm/linux/kernel/git/gregkh/staging.git
	fetch = +refs/heads/*:refs/remotes/staging/*
...
```

This is what causes Git to track the remote’s branches; you may modify or delete these configuration options by editing `.git/config` with a text editor. (See the "CONFIGURATION FILE" section of [git-config[1]](../1/git-config) for details.)

​	这会导致Git跟踪远程仓库的分支；您可以通过使用文本编辑器编辑`.git/config`来修改或删除这些配置选项（有关详细信息，请参见[git-config[1]](../1/git-config)中的"CONFIGURATION FILE"部分）。

## 探索Git历史

Git is best thought of as a tool for storing the history of a collection of files. It does this by storing compressed snapshots of the contents of a file hierarchy, together with "commits" which show the relationships between these snapshots.

​	Git最好被视为一个存储文件集合历史的工具。它通过存储文件层次结构的压缩快照以及显示这些快照之间关系的"提交"来实现这一点。

Git provides extremely flexible and fast tools for exploring the history of a project.

​	Git提供了非常灵活和快速的工具来探索项目的历史。

We start with one specialized tool that is useful for finding the commit that introduced a bug into a project.

​	我们先介绍一个特殊的工具，用于查找引入项目中错误的提交。

### 如何使用二分查找来查找回归 How to use bisect to find a regression

Suppose version 2.6.18 of your project worked, but the version at "master" crashes. Sometimes the best way to find the cause of such a regression is to perform a brute-force search through the project’s history to find the particular commit that caused the problem. The [git-bisect[1]](../1/git-bisect) command can help you do this:

​	假设您的项目的2.6.18版本工作正常，但"master"分支的版本崩溃。有时找到这种回归的原因的最佳方法是通过对项目的历史进行蛮力搜索，以找到导致问题的特定提交。[git-bisect[1]](../1/git-bisect)命令可以帮助您做到这一点：

``` bash
$ git bisect start
$ git bisect good v2.6.18
$ git bisect bad master
Bisecting: 3537 revisions left to test after this
[65934a9a028b88e83e2b0f8b36618fe503349f8e] BLOCK: Make USB storage depend on SCSI rather than selecting it [try #6]
```

If you run `git branch` at this point, you’ll see that Git has temporarily moved you in "(no branch)". HEAD is now detached from any branch and points directly to a commit (with commit id 65934) that is reachable from "master" but not from v2.6.18. Compile and test it, and see whether it crashes. Assume it does crash. Then:

​	如果此时运行`git branch`，您会看到Git暂时将您切换到了"(no branch)"。HEAD现在处于未关联任何分支的状态，并直接指向一个提交（提交id为65934），它可从"master"到达，但无法从v2.6.18到达。编译并测试它，看看是否崩溃。假设它崩溃了。然后执行：

``` bash
$ git bisect bad
Bisecting: 1769 revisions left to test after this
[7eff82c8b1511017ae605f0c99ac275a7e21b867] i2c-core: Drop useless bitmaskings
```

checks out an older version. Continue like this, telling Git at each stage whether the version it gives you is good or bad, and notice that the number of revisions left to test is cut approximately in half each time.

这会检出一个较早的版本。以此类推，在每个阶段告诉Git版本它给您的是好的还是坏的，并注意每次剩余待测试版本数将减半左右。

After about 13 tests (in this case), it will output the commit id of the guilty commit. You can then examine the commit with [git-show[1]](../1/git-show), find out who wrote it, and mail them your bug report with the commit id. Finally, run

​	经过大约13次测试（在本例中），它将输出有问题提交的提交id。然后，您可以使用[git-show[1]](../1/git-show)查看提交，找出是谁编写的，并将带有提交id的错误报告发送给他们。最后，运行

``` bash
$ git bisect reset
```

to return you to the branch you were on before.

返回到之前所在的分支。

Note that the version which `git bisect` checks out for you at each point is just a suggestion, and you’re free to try a different version if you think it would be a good idea. For example, occasionally you may land on a commit that broke something unrelated; run

​	请注意，`git bisect`在每个点为您检出的版本只是一个建议，如果您认为换一个版本是个好主意，您可以尝试不同的版本。例如，有时候您可能会遇到导致了与问题无关的提交；运行

``` bash
$ git bisect visualize
```

which will run gitk and label the commit it chose with a marker that says "bisect". Choose a safe-looking commit nearby, note its commit id, and check it out with:

这会运行gitk并在它选择的提交上标记一个带有"bisect"标记的标记。选择一个看起来安全的附近提交，记录其提交id，并使用以下命令检出：

``` bash
$ git reset --hard fb47ddb2db
```

then test, run `bisect good` or `bisect bad` as appropriate, and continue.

然后测试，运行`bisect good`或`bisect bad`，并继续。

Instead of `git bisect visualize` and then `git reset --hard fb47ddb2db`, you might just want to tell Git that you want to skip the current commit:

与其运行`git bisect visualize`然后`git reset --hard fb47ddb2db`，您可能只想告诉Git要跳过当前的提交：

``` bash
$ git bisect skip
```

In this case, though, Git may not eventually be able to tell the first bad one between some first skipped commits and a later bad commit.

​	不过，在这种情况下，Git最终可能无法确定在一些已跳过的提交和稍后的有问题的提交之间的第一个有问题的提交。

There are also ways to automate the bisecting process if you have a test script that can tell a good from a bad commit. See [git-bisect[1]](../1/git-bisect) for more information about this and other `git bisect` features.

​	如果您有一个测试脚本可以判断哪些提交是好的，哪些是有问题的，还可以自动化二分查找过程。有关此功能和其他`git bisect`功能的更多信息，请参阅[git-bisect[1]](../1/git-bisect)。

### 命名提交

We have seen several ways of naming commits already:

​	我们已经看到了几种提交命名的方法： 

- 40-hexdigit object name
- branch name: refers to the commit at the head of the given branch
- tag name: refers to the commit pointed to by the given tag (we’ve seen branches and tags are special cases of [references](https://git-scm.com/docs/user-manual#how-git-stores-references)).
- HEAD: refers to the head of the current branch
- 40位十六进制对象名称
- 分支名称：指向给定分支头部的提交
- 标签名称：指向给定标签所指向的提交（我们已经看到分支和标签是[引用](https://git-scm.com/docs/user-manual#how-git-stores-references)的特殊情况）。
- HEAD：指向当前分支的头部

There are many more; see the "SPECIFYING REVISIONS" section of the [gitrevisions[7]](../7/gitrevisions) man page for the complete list of ways to name revisions. Some examples:

​	还有许多其他命名方式，请参阅[gitrevisions[7]](../7/gitrevisions)手册中的"SPECIFYING REVISIONS"部分，以获取完整的命名修订版本的方式列表。以下是一些示例：

``` bash
$ git show fb47ddb2 # the first few characters of the object name
		    # are usually enough to specify it uniquely
$ git show HEAD^    # the parent of the HEAD commit
$ git show HEAD^^   # the grandparent
$ git show HEAD~4   # the great-great-grandparent
```

Recall that merge commits may have more than one parent; by default, `^` and `~` follow the first parent listed in the commit, but you can also choose:

​	请注意，合并提交可能有多个父提交；默认情况下，`^`和`~`将跟随提交中列出的第一个父提交，但您也可以选择：

``` bash
$ git show HEAD^1   # show the first parent of HEAD
$ git show HEAD^2   # show the second parent of HEAD
```

In addition to HEAD, there are several other special names for commits:

​	除了HEAD，还有几个其他的特殊提交名称：

Merges (to be discussed later), as well as operations such as `git reset`, which change the currently checked-out commit, generally set ORIG_HEAD to the value HEAD had before the current operation.

​	合并提交（稍后会讨论），以及诸如`git reset`这样会更改当前检出提交的操作，通常会在当前操作之前将ORIG_HEAD设置为HEAD的值。

The `git fetch` operation always stores the head of the last fetched branch in FETCH_HEAD. For example, if you run `git fetch` without specifying a local branch as the target of the operation

​	`git fetch`操作总是将最后获取的分支的头部存储在FETCH_HEAD中。例如，如果您运行`git fetch`而不指定本地分支作为操作的目标：

``` bash
$ git fetch git://example.com/proj.git theirbranch
```

the fetched commits will still be available from FETCH_HEAD.

获取的提交仍将从FETCH_HEAD可用。

When we discuss merges we’ll also see the special name MERGE_HEAD, which refers to the other branch that we’re merging in to the current branch.

​	在讨论合并时，我们还将看到特殊名称MERGE_HEAD，它指的是要合并到当前分支的另一个分支。

The [git-rev-parse[1]](../1/git-rev-parse) command is a low-level command that is occasionally useful for translating some name for a commit to the object name for that commit:

​	[git-rev-parse[1]](../1/git-rev-parse)命令是一个低级命令，偶尔用于将某个提交的名称转换为该提交的对象名称：

``` bash
$ git rev-parse origin
e05db0fd4f31dde7005f075a84f96b360d05984b
```

### 创建标签

We can also create a tag to refer to a particular commit; after running

我们还可以创建一个标签来指向特定的提交；运行以下命令后：

``` bash
$ git tag stable-1 1b2e1d63ff
```

You can use `stable-1` to refer to the commit 1b2e1d63ff.

​	您可以使用`stable-1`来引用提交1b2e1d63ff。

This creates a "lightweight" tag. If you would also like to include a comment with the tag, and possibly sign it cryptographically, then you should create a tag object instead; see the [git-tag[1]](../1/git-tag) man page for details.

​	这将创建一个"轻量级"标签。如果您还希望在标签中包含评论，并可能对其进行加密签名，则应该创建一个标签对象；有关详细信息，请参见[git-tag[1]](../1/git-tag)手册。

### 浏览修订版本 Browsing revisions

The [git-log[1]](../1/git-log) command can show lists of commits. On its own, it shows all commits reachable from the parent commit; but you can also make more specific requests:

​	[git-log[1]](../1/git-log)命令可以显示提交列表。默认情况下，它显示从父提交可达的所有提交；但您也可以提出更具体的要求：

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

And of course you can combine all of these; the following finds commits since v2.5 which touch the `Makefile` or any file under `fs`:

​	当然，您可以将所有这些组合起来；以下命令查找从v2.5开始的提交，这些提交触及了`Makefile`或`fs`目录下的任何文件：

``` bash
$ git log v2.5.. Makefile fs/
```

You can also ask git log to show patches:

​	您还可以要求git log显示补丁：

``` bash
$ git log -p
```

See the `--pretty` option in the [git-log[1]](../1/git-log) man page for more display options.

​	有关更多显示选项，请参阅[git-log[1]](../1/git-log)手册中的`--pretty`选项。

Note that git log starts with the most recent commit and works backwards through the parents; however, since Git history can contain multiple independent lines of development, the particular order that commits are listed in may be somewhat arbitrary.

​	请注意，git log从最近的提交开始，并通过父提交向后工作；然而，由于Git历史可能包含多个独立的开发线，提交的特定列表顺序可能有些随意。

### 生成差异

You can generate diffs between any two versions using [git-diff[1]](../1/git-diff):

​	您可以使用[git-diff[1]](../1/git-diff)在任何两个版本之间生成差异：

``` bash
$ git diff master..test
```

That will produce the diff between the tips of the two branches. If you’d prefer to find the diff from their common ancestor to test, you can use three dots instead of two:

​	这将产生两个分支末端之间的差异。如果您希望找到从它们的共同祖先到test的差异，可以使用三个点而不是两个点：

``` bash
$ git diff master...test
```

Sometimes what you want instead is a set of patches; for this you can use [git-format-patch[1]](../1/git-format-patch):

​	有时您需要的是一组补丁；为此，您可以使用[git-format-patch[1]](../1/git-format-patch)：

``` bash
$ git format-patch master..test
```

will generate a file with a patch for each commit reachable from test but not from master.

将生成一个文件，其中包含从test可达但从master不可达的每个提交的补丁。

### 查看旧文件版本

You can always view an old version of a file by just checking out the correct revision first. But sometimes it is more convenient to be able to view an old version of a single file without checking anything out; this command does that:

​	您可以通过先检出正确的修订版本来查看文件的旧版本。但有时，仅查看单个文件的旧版本而不检出任何内容会更加方便；这个命令可以实现：

``` bash
$ git show v2.5:fs/locks.c
```

Before the colon may be anything that names a commit, and after it may be any path to a file tracked by Git.

​	冒号前面可以是任何命名了某个提交的内容，冒号后面可以是Git跟踪的文件的任何路径。

### 示例

#### 计算分支上的提交数量

Suppose you want to know how many commits you’ve made on `mybranch` since it diverged from `origin`:

​	假设您想知道自从`mybranch`与`origin`分叉以来，在`mybranch`上做了多少提交：

``` bash
$ git log --pretty=oneline origin..mybranch | wc -l
```

Alternatively, you may often see this sort of thing done with the lower-level command [git-rev-list[1]](../1/git-rev-list), which just lists the SHA-1’s of all the given commits:

​	或者，您可能经常使用更低级的`git-rev-list[1]`命令来完成这种操作，该命令仅列出所有给定提交的SHA-1：

``` bash
$ git rev-list origin..mybranch | wc -l
```

#### 检查两个分支是否指向相同的历史点

Suppose you want to check whether two branches point at the same point in history.

​	假设您想检查两个分支是否指向历史中的相同点。

``` bash
$ git diff origin..master
```

will tell you whether the contents of the project are the same at the two branches; in theory, however, it’s possible that the same project contents could have been arrived at by two different historical routes. You could compare the object names:

将告诉您两个分支的项目内容是否相同；不过，理论上可能会通过两个不同的历史路径到达相同的项目内容。您可以比较对象名称：

``` bash
$ git rev-list origin
e05db0fd4f31dde7005f075a84f96b360d05984b
$ git rev-list master
e05db0fd4f31dde7005f075a84f96b360d05984b
```

Or you could recall that the `...` operator selects all commits reachable from either one reference or the other but not both; so

​	或者您可以回忆起`...`运算符选择可从任一引用或另一引用到达的所有提交，但不同时可达；因此

``` bash
$ git log origin...master
```

will return no commits when the two branches are equal.

当两个分支相同时，将不返回提交。

#### 查找包含给定修复的第一个已打标签版本 Find first tagged version including a given fix

Suppose you know that the commit e05db0fd fixed a certain problem. You’d like to find the earliest tagged release that contains that fix.

​	假设您知道提交e05db0fd修复了某个问题。您想找到包含该修复的最早已打标签版本。

Of course, there may be more than one answer—if the history branched after commit e05db0fd, then there could be multiple "earliest" tagged releases.

​	当然，可能有不止一个答案 —— 如果在提交e05db0fd之后分支历史分叉，那么可能会有多个“最早”的已打标签版本。

You could just visually inspect the commits since e05db0fd:

​	您可以直接目视检查从e05db0fd开始的提交：

``` bash
$ gitk e05db0fd..
```

or you can use [git-name-rev[1]](../1/git-name-rev), which will give the commit a name based on any tag it finds pointing to one of the commit’s descendants:

或者您可以使用[git-name-rev[1]](../1/git-name-rev)，它将根据找到的任何标签为提交赋予名称，该标签指向提交的后代之一：

``` bash
$ git name-rev --tags e05db0fd
e05db0fd tags/v1.5.0-rc1^0~23
```

The [git-describe[1]](../1/git-describe) command does the opposite, naming the revision using a tag on which the given commit is based:

而[git-describe[1]](../1/git-describe)命令则相反，使用提交所基于的标签来命名修订版本：

``` bash
$ git describe e05db0fd
v1.5.0-rc0-260-ge05db0f
```

but that may sometimes help you guess which tags might come after the given commit.

但是这可能有助于您猜测给定提交之后可能出现的标签。

If you just want to verify whether a given tagged version contains a given commit, you could use [git-merge-base[1]](../1/git-merge-base):

​	如果您只想验证给定的已打标签版本是否包含给定的提交，可以使用[git-merge-base[1]](../1/git-merge-base)：

``` bash
$ git merge-base e05db0fd v1.5.0-rc1
e05db0fd4f31dde7005f075a84f96b360d05984b
```

The merge-base command finds a common ancestor of the given commits, and always returns one or the other in the case where one is a descendant of the other; so the above output shows that e05db0fd actually is an ancestor of v1.5.0-rc1.

​	merge-base命令查找给定提交的共同祖先，并在一个提交是另一个提交的后代的情况下始终返回其中一个；因此，上面的输出显示e05db0fd实际上是v1.5.0-rc1的一个祖先。

Alternatively, note that

​	或者，注意到：

``` bash
$ git log v1.5.0-rc1..e05db0fd
```

will produce empty output if and only if v1.5.0-rc1 includes e05db0fd, because it outputs only commits that are not reachable from v1.5.0-rc1.

将只在v1.5.0-rc1包含e05db0fd时输出为空，因为它只输出不可从v1.5.0-rc1到达的提交。

As yet another alternative, the [git-show-branch[1]](../1/git-show-branch) command lists the commits reachable from its arguments with a display on the left-hand side that indicates which arguments that commit is reachable from. So, if you run something like

​	作为另一种选择，[git-show-branch[1]](../1/git-show-branch)命令会列出可从其参数到达的提交，并在左侧显示显示指示该提交可从哪个参数到达的内容。因此，如果您运行类似于：

``` bash
$ git show-branch e05db0fd v1.5.0-rc0 v1.5.0-rc1 v1.5.0-rc2
! [e05db0fd] Fix warnings in sha1_file.c - use C99 printf format if
available
 ! [v1.5.0-rc0] GIT v1.5.0 preview
  ! [v1.5.0-rc1] GIT v1.5.0-rc1
   ! [v1.5.0-rc2] GIT v1.5.0-rc2
...
```

then a line like

然后像下面这样的行：

```
+ ++ [e05db0fd] Fix warnings in sha1_file.c - use C99 printf format if
available
```

shows that e05db0fd is reachable from itself, from v1.5.0-rc1, and from v1.5.0-rc2, and not from v1.5.0-rc0.

显示e05db0fd可从自身、v1.5.0-rc1和v1.5.0-rc2到达，但不可从v1.5.0-rc0到达。

#### 显示特定分支中独有的提交

Suppose you would like to see all the commits reachable from the branch head named `master` but not from any other head in your repository.

​	假设您希望查看从名为`master`的分支头部可达，但从仓库中的任何其他分支头部不可达的所有提交。

We can list all the heads in this repository with [git-show-ref[1]](../1/git-show-ref):

​	我们可以使用[git-show-ref[1]](../1/git-show-ref)列出此仓库中的所有分支头部：

``` bash
$ git show-ref --heads
bf62196b5e363d73353a9dcf094c59595f3153b7 refs/heads/core-tutorial
db768d5504c1bb46f63ee9d6e1772bd047e05bf9 refs/heads/maint
a07157ac624b2524a059a3414e99f6f44bebc1e7 refs/heads/master
24dbc180ea14dc1aebe09f14c8ecf32010690627 refs/heads/tutorial-2
1e87486ae06626c2f31eaa63d26fc0fd646c8af2 refs/heads/tutorial-fixes
```

We can get just the branch-head names, and remove `master`, with the help of the standard utilities cut and grep:

​	我们可以仅获取分支头名称，并去掉`master`，使用标准实用程序cut和grep的帮助

``` bash
$ git show-ref --heads | cut -d' ' -f2 | grep -v '^refs/heads/master'
refs/heads/core-tutorial
refs/heads/maint
refs/heads/tutorial-2
refs/heads/tutorial-fixes
```

And then we can ask to see all the commits reachable from master but not from these other heads:

​	然后，我们可以要求查看所有从master可达，但从这些其他头部不可达的提交

``` bash
$ gitk master --not $( git show-ref --heads | cut -d' ' -f2 |
				grep -v '^refs/heads/master' )
```

Obviously, endless variations are possible; for example, to see all commits reachable from some head but not from any tag in the repository:

​	显然，有无数种变化方式；例如，要查看所有从某个分支可达，但从仓库中的任何标签不可达的提交：

``` bash
$ gitk $( git show-ref --heads ) --not  $( git show-ref --tags )
```

(See [gitrevisions[7]](../7/gitrevisions) for explanations of commit-selecting syntax such as `--not`.)

（有关诸如`--not`等提交选择语法的解释，请参阅[gitrevisions[7]](../7/gitrevisions)。）

#### 为软件发布创建更改日志和tarball  - Creating a changelog and tarball for a software release

The [git-archive[1]](../1/git-archive) command can create a tar or zip archive from any version of a project; for example:

​	[git-archive[1]](../1/git-archive)命令可以从项目的任何版本创建tar或zip存档；例如：

``` bash
$ git archive -o latest.tar.gz --prefix=project/ HEAD
```

will use HEAD to produce a gzipped tar archive in which each filename is preceded by `project/`. The output file format is inferred from the output file extension if possible, see [git-archive[1]](../1/git-archive) for details.

将使用HEAD创建一个gzipped的tar存档，其中每个文件名前面都有`project/`。如果可能，输出文件格式将根据输出文件扩展名进行推断，请参阅[git-archive[1]](../1/git-archive)了解详情。

Versions of Git older than 1.7.7 don’t know about the `tar.gz` format, you’ll need to use gzip explicitly:

​	Git版本1.7.7之前的版本不知道`tar.gz`格式，您需要显式使用gzip：

``` bash
$ git archive --format=tar --prefix=project/ HEAD | gzip >latest.tar.gz
```

If you’re releasing a new version of a software project, you may want to simultaneously make a changelog to include in the release announcement.

​	如果您正在发布软件项目的新版本，可能希望同时制作更改日志并将其包含在发布公告中。

Linus Torvalds, for example, makes new kernel releases by tagging them, then running:

​	例如，Linus Torvalds会通过打标签来发布新的内核版本，然后运行：

``` bash
$ release-script 2.6.12 2.6.13-rc6 2.6.13-rc7
```

where release-script is a shell script that looks like:

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

and then he just cut-and-pastes the output commands after verifying that they look OK.

然后，只需将输出命令剪切并粘贴到验证它们是否正确的地方。

#### 查找引用包含给定内容的文件的提交 Finding commits referencing a file with given content

Somebody hands you a copy of a file, and asks which commits modified a file such that it contained the given content either before or after the commit. You can find out with this:

​	有人给您一个文件的副本，并询问哪些提交修改了文件，以便在提交之前或之后包含了给定的内容。您可以使用以下命令找出：

``` bash
$  git log --raw --abbrev=40 --pretty=oneline |
	grep -B 1 `git hash-object filename`
```

Figuring out why this works is left as an exercise to the (advanced) student. The [git-log[1]](../1/git-log), [git-diff-tree[1]](../1/git-diff-tree), and [git-hash-object[1]](../1/git-hash-object) man pages may prove helpful.

​	为什么这能工作的原因留给（高级）学生作为练习。[git-log[1]](../1/git-log)、[git-diff-tree[1]](../1/git-diff-tree)和[git-hash-object[1]](../1/git-hash-object)的手册可能会对您有所帮助。

## 使用Git进行开发

### 告诉Git您的name

Before creating any commits, you should introduce yourself to Git. The easiest way to do so is to use [git-config[1]](../1/git-config):

​	在创建任何提交之前，您应该向Git介绍自己。最简单的方法是使用[git-config[1]](../1/git-config)：

``` bash
$ git config --global user.name 'Your Name Comes Here'
$ git config --global user.email 'you@yourdomain.example.com'
```

Which will add the following to a file named `.gitconfig` in your home directory:

​	这将在您的主目录中的一个名为`.gitconfig`的文件中添加以下内容：

```
[user]
	name = Your Name Comes Here
	email = you@yourdomain.example.com
```

See the "CONFIGURATION FILE" section of [git-config[1]](../1/git-config) for details on the configuration file. The file is plain text, so you can also edit it with your favorite editor.

​	有关配置文件的详细信息，请参阅[git-config[1]](../1/git-config)中的“CONFIGURATION FILE”部分。该文件是纯文本，因此您也可以使用您喜欢的编辑器进行编辑。

### 创建一个新的仓库

Creating a new repository from scratch is very easy:

​	从头开始创建一个新的仓库非常简单：

``` bash
$ mkdir project
$ cd project
$ git init
```

If you have some initial content (say, a tarball):

​	如果有一些初始内容（例如，一个tarball）：

``` bash
$ tar xzvf project.tar.gz
$ cd project
$ git init
$ git add . # include everything below ./ in the first commit:
$ git commit
```

### 如何创建提交

Creating a new commit takes three steps:

​	创建新的提交需要三个步骤：

1. Making some changes to the working directory using your favorite editor.
2. Telling Git about your changes.
3. Creating the commit using the content you told Git about in step 2.
4. 使用您喜欢的编辑器对工作目录进行一些更改。
5. 告诉Git您的更改。
6. 使用第2步中告诉Git的内容创建提交。

In practice, you can interleave and repeat steps 1 and 2 as many times as you want: in order to keep track of what you want committed at step 3, Git maintains a snapshot of the tree’s contents in a special staging area called "the index."

​	实际上，您可以交错和重复步骤1和2多次：为了跟踪您在步骤3要提交的内容，Git在特殊的暂存区域中维护树内容的快照，该区域称为“索引”。

At the beginning, the content of the index will be identical to that of the HEAD. The command `git diff --cached`, which shows the difference between the HEAD and the index, should therefore produce no output at that point.

​	开始时，索引的内容将与HEAD的内容相同。因此，`git diff --cached`命令，在HEAD和索引之间显示的差异，此时应该不会产生输出。

Modifying the index is easy:

​	修改索引很简单：

To update the index with the contents of a new or modified file, use

​	要使用新文件或修改后的文件内容更新索引，请使用：

``` bash
$ git add path/to/file
```

To remove a file from the index and from the working tree, use

​	要从索引和工作树中删除文件，请使用：

``` bash
$ git rm path/to/file
```

After each step you can verify that

​	在每个步骤之后，您可以通过验证以下命令：

``` bash
$ git diff --cached
```

always shows the difference between the HEAD and the index file—this is what you’d commit if you created the commit now—and that

始终显示HEAD和索引文件之间的差异，这是您现在将要创建的提交内容，以及

``` bash
$ git diff
```

shows the difference between the working tree and the index file.

显示工作树与索引文件之间的差异。

Note that `git add` always adds just the current contents of a file to the index; further changes to the same file will be ignored unless you run `git add` on the file again.

​	请注意，`git add`命令始终只会将文件的当前内容添加到索引；除非再次对文件运行`git add`命令，否则对同一文件的进一步更改将被忽略。

When you’re ready, just run

​	当您准备好时，只需运行：

``` bash
$ git commit
```

and Git will prompt you for a commit message and then create the new commit. Check to make sure it looks like what you expected with

Git将提示您输入提交消息，然后创建新的提交。您可以使用以下命令验证它是否符合您的期望：

``` bash
$ git show
```

As a special shortcut,

​	作为一个特殊的快捷方式，

``` bash
$ git commit -a
```

will update the index with any files that you’ve modified or removed and create a commit, all in one step.

将更新索引中的任何已修改或删除的文件并创建一个提交，这一切都在一个步骤中完成。

A number of commands are useful for keeping track of what you’re about to commit:

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

You can also use [git-gui[1]](../1/git-gui) to create commits, view changes in the index and the working tree files, and individually select diff hunks for inclusion in the index (by right-clicking on the diff hunk and choosing "Stage Hunk For Commit").

​	您还可以使用[git-gui[1]](../1/git-gui)来创建提交，查看索引和工作目录文件的更改，并在索引中单独选择要包含在提交中的差异块（右键单击差异块并选择“Stage Hunk For Commit”）。

### 创建良好的提交消息

Though not required, it’s a good idea to begin the commit message with a single short (less than 50 character) line summarizing the change, followed by a blank line and then a more thorough description. The text up to the first blank line in a commit message is treated as the commit title, and that title is used throughout Git. For example, [git-format-patch[1]](../1/git-format-patch) turns a commit into email, and it uses the title on the Subject line and the rest of the commit in the body.

​	虽然不是必需的，但最好的做法是在提交消息的开头用一行简短（少于50个字符）的摘要总结更改，然后是一个空行，然后是更详细的描述。提交消息中第一个空行之前的文本被视为提交标题，并且该标题在整个Git中都被使用。例如，[git-format-patch[1]](../1/git-format-patch)将提交转换为电子邮件，并在主题行中使用标题，在正文中使用提交的其余部分。

### 忽略文件

A project will often generate files that you do *not* want to track with Git. This typically includes files generated by a build process or temporary backup files made by your editor. Of course, *not* tracking files with Git is just a matter of *not* calling `git add` on them. But it quickly becomes annoying to have these untracked files lying around; e.g. they make `git add .` practically useless, and they keep showing up in the output of `git status`.

​	项目通常会生成一些您不希望使用Git跟踪的文件。这通常包括构建过程生成的文件或编辑器生成的临时备份文件。当然，如果您不在这些文件上调用`git add`，它们将*不会*被Git跟踪。但是，保持这些未跟踪的文件存在很快会变得很烦人；例如，它们会使`git add .`几乎无用，并且它们会不断显示在`git status`的输出中。

You can tell Git to ignore certain files by creating a file called `.gitignore` in the top level of your working directory, with contents such as:

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

See [gitignore[5]](../5/gitignore) for a detailed explanation of the syntax. You can also place .gitignore files in other directories in your working tree, and they will apply to those directories and their subdirectories. The `.gitignore` files can be added to your repository like any other files (just run `git add .gitignore` and `git commit`, as usual), which is convenient when the exclude patterns (such as patterns matching build output files) would also make sense for other users who clone your repository.

​	有关语法的详细解释，请参阅[gitignore[5]](../5/gitignore)。您还可以将`.gitignore`文件放置在工作树的其他目录中，它们将适用于这些目录及其子目录。可以像添加其他文件一样将`.gitignore`文件添加到仓库中（只需运行`git add .gitignore`和`git commit`，和通常一样），这对于排除模式（例如与构建输出文件匹配的模式）对于克隆您的仓库的其他用户也是有意义的。

If you wish the exclude patterns to affect only certain repositories (instead of every repository for a given project), you may instead put them in a file in your repository named `.git/info/exclude`, or in any file specified by the `core.excludesFile` configuration variable. Some Git commands can also take exclude patterns directly on the command line. See [gitignore[5]](../5/gitignore) for the details.

​	如果希望排除模式仅影响某些仓库（而不是给定项目的每个仓库），则可以将它们放入仓库中的文件`.git/info/exclude`，或者放入`core.excludesFile`配置变量指定的任何文件中。一些Git命令还可以直接在命令行上接受排除模式。有关详细信息，请参阅[gitignore[5]](../5/gitignore)。

### 如何合并

You can rejoin two diverging branches of development using [git-merge[1]](../1/git-merge):

​	您可以使用[git-merge[1]](../1/git-merge)将两个分支的开发合并在一起：

``` bash
$ git merge branchname
```

merges the development in the branch `branchname` into the current branch.

将分支`branchname`中的开发合并到当前分支。

A merge is made by combining the changes made in `branchname` and the changes made up to the latest commit in your current branch since their histories forked. The work tree is overwritten by the result of the merge when this combining is done cleanly, or overwritten by a half-merged results when this combining results in conflicts. Therefore, if you have uncommitted changes touching the same files as the ones impacted by the merge, Git will refuse to proceed. Most of the time, you will want to commit your changes before you can merge, and if you don’t, then [git-stash[1]](../1/git-stash) can take these changes away while you’re doing the merge, and reapply them afterwards.

​	合并是通过将`branchname`中的更改与从它们的历史分叉以来在当前分支中的最新提交之前进行的更改进行组合而完成的。当此组合干净地完成时，工作树将被组合结果覆盖，或者当此组合导致冲突时，工作树将被部分合并的结果覆盖。因此，如果您对与合并受影响的文件相同的文件有未提交的更改，Git将拒绝继续执行。大多数情况下，您将在合并之前提交您的更改；如果您不这样做，那么[git-stash[1]](../1/git-stash)可以在您进行合并时拿走这些更改，并在合并后重新应用它们。

If the changes are independent enough, Git will automatically complete the merge and commit the result (or reuse an existing commit in case of [fast-forward](https://git-scm.com/docs/user-manual#fast-forwards), see below). On the other hand, if there are conflicts—for example, if the same file is modified in two different ways in the remote branch and the local branch—then you are warned; the output may look something like this:

​	如果更改足够独立，Git将自动完成合并并提交结果（或在[快进](https://git-scm.com/docs/user-manual#fast-forwards)的情况下重用现有提交，参见下文）。另一方面，如果存在冲突，例如，如果在远程分支和本地分支的相同文件以两种不同的方式进行了修改，那么您将会收到警告；输出可能如下所示：

``` bash
$ git merge next
 100% (4/4) done
Auto-merged file.txt
CONFLICT (content): Merge conflict in file.txt
Automatic merge failed; fix conflicts and then commit the result.
```

Conflict markers are left in the problematic files, and after you resolve the conflicts manually, you can update the index with the contents and run Git commit, as you normally would when creating a new file.

​	在有问题的文件中保留冲突标记，然后在手动解决冲突后，您可以使用以下命令更新索引的内容并运行Git提交，就像创建新文件时那样。

If you examine the resulting commit using gitk, you will see that it has two parents, one pointing to the top of the current branch, and one to the top of the other branch.

​	如果使用gitk检查结果提交，您将看到它有两个父提交，一个指向当前分支的顶部，另一个指向另一个分支的顶部。

### 解决合并冲突

When a merge isn’t resolved automatically, Git leaves the index and the working tree in a special state that gives you all the information you need to help resolve the merge.

​	当合并不会自动解决时，Git会将索引和工作树保留在一种特殊状态中，以提供您解决合并所需的所有信息。

Files with conflicts are marked specially in the index, so until you resolve the problem and update the index, [git-commit[1]](../1/git-commit) will fail:

​	具有冲突的文件在索引中被特别标记，因此在您解决问题并更新索引之前，[git-commit[1]](../1/git-commit)将失败：

``` bash
$ git commit
file.txt: needs merge
```

Also, [git-status[1]](../1/git-status) will list those files as "unmerged", and the files with conflicts will have conflict markers added, like this:

​	此外，[git-status[1]](../1/git-status)将把这些文件列为“unmerged”，并且具有冲突的文件将添加冲突标记，例如：

```
<<<<<<< HEAD:file.txt
Hello world
=======
Goodbye
>>>>>>> 77976da35a11db4580b80ae27e8d65caf5208086:file.txt
```

All you need to do is edit the files to resolve the conflicts, and then

​	您需要做的就是编辑这些文件以解决冲突，然后：

``` bash
$ git add file.txt
$ git commit
```

Note that the commit message will already be filled in for you with some information about the merge. Normally you can just use this default message unchanged, but you may add additional commentary of your own if desired.

​	请注意，提交消息已经填充了一些关于合并的信息。通常，您可以使用此默认消息，但如果需要，您可以添加自己的附加评论。

The above is all you need to know to resolve a simple merge. But Git also provides more information to help resolve conflicts:

​	上述内容是解决简单合并所需了解的全部。但是，Git还提供了更多信息来帮助解决冲突：

#### 在合并过程中获取冲突解决帮助 Getting conflict-resolution help during a merge

All of the changes that Git was able to merge automatically are already added to the index file, so [git-diff[1]](../1/git-diff) shows only the conflicts. It uses an unusual syntax:

​	所有Git能够自动合并的更改已经添加到索引文件中，因此[git-diff[1]](../1/git-diff)仅显示冲突。它使用一种不寻常的语法：

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

Recall that the commit which will be committed after we resolve this conflict will have two parents instead of the usual one: one parent will be HEAD, the tip of the current branch; the other will be the tip of the other branch, which is stored temporarily in MERGE_HEAD.

​	请注意，解决此冲突后将提交的提交将具有两个父提交，而不是通常的一个：一个父提交将是HEAD，即当前分支的最新提交；另一个父提交将是另一个分支的最新提交，它暂时存储在MERGE_HEAD中。

During the merge, the index holds three versions of each file. Each of these three "file stages" represents a different version of the file:

​	在合并过程中，索引保存每个文件的三个版本。这三个“文件阶段”分别表示文件的不同版本：

``` bash
$ git show :1:file.txt	# the file in a common ancestor of both branches
$ git show :2:file.txt	# the version from HEAD.
$ git show :3:file.txt	# the version from MERGE_HEAD.
```

When you ask [git-diff[1]](../1/git-diff) to show the conflicts, it runs a three-way diff between the conflicted merge results in the work tree with stages 2 and 3 to show only hunks whose contents come from both sides, mixed (in other words, when a hunk’s merge results come only from stage 2, that part is not conflicting and is not shown. Same for stage 3).

​	当您要求[git-diff[1]](../1/git-diff)显示冲突时，它会在工作树中运行三方差异，使用第2个和第3个阶段来显示仅来自双方的内容的块（换句话说，当块的合并结果仅来自第2个阶段时，该部分不会产生冲突，也不会显示。与第3个阶段相同）。

The diff above shows the differences between the working-tree version of file.txt and the stage 2 and stage 3 versions. So instead of preceding each line by a single `+` or `-`, it now uses two columns: the first column is used for differences between the first parent and the working directory copy, and the second for differences between the second parent and the working directory copy. (See the "COMBINED DIFF FORMAT" section of [git-diff-files[1]](../1/git-diff-files) for a details of the format.)

​	上面的差异显示了file.txt的工作树版本与第2个和第3个阶段版本之间的差异。因此，不是在每一行前面用单个`+`或`-`表示，它现在使用两列：第一列用于第一个父提交和工作目录副本之间的差异，第二列用于第二个父提交和工作目录副本之间的差异。有关格式的详细信息，请参阅[git-diff-files[1]](../1/git-diff-files)中的“COMBINED DIFF FORMAT”部分。

After resolving the conflict in the obvious way (but before updating the index), the diff will look like:

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

This shows that our resolved version deleted "Hello world" from the first parent, deleted "Goodbye" from the second parent, and added "Goodbye world", which was previously absent from both.

​	这显示我们解决的版本从第一个父提交中删除了“Hello world”，从第二个父提交中删除了“Goodbye”，并添加了“Goodbye world”，它之前在两个版本中都不存在。

Some special diff options allow diffing the working directory against any of these stages:

​	一些特殊的diff选项允许将工作目录与这些阶段中的任何一个进行比较：

``` bash
$ git diff -1 file.txt		# diff against stage 1
$ git diff --base file.txt	# same as the above
$ git diff -2 file.txt		# diff against stage 2
$ git diff --ours file.txt	# same as the above
$ git diff -3 file.txt		# diff against stage 3
$ git diff --theirs file.txt	# same as the above.
```

The [git-log[1]](../1/git-log) and [gitk[1]](../1/gitk) commands also provide special help for merges:

​	[git-log[1]](../1/git-log)和[gitk[1]](../1/gitk)命令还为合并提供特殊帮助：

``` bash
$ git log --merge
$ gitk --merge
```

These will display all commits which exist only on HEAD or on MERGE_HEAD, and which touch an unmerged file.

​	这将显示仅存在于HEAD或MERGE_HEAD上并涉及未合并文件的所有提交。

You may also use [git-mergetool[1]](../1/git-mergetool), which lets you merge the unmerged files using external tools such as Emacs or kdiff3.

​	您还可以使用[git-mergetool[1]](../1/git-mergetool)，它允许您使用外部工具（如Emacs或kdiff3）合并未合并的文件。

Each time you resolve the conflicts in a file and update the index:

​	每次您解决文件中的冲突并更新索引后：

``` bash
$ git add file.txt
```

the different stages of that file will be "collapsed", after which `git diff` will (by default) no longer show diffs for that file.

该文件的不同阶段将被“折叠”，之后`git diff`（默认情况下）将不再显示该文件的差异。

### 撤销合并

If you get stuck and decide to just give up and throw the whole mess away, you can always return to the pre-merge state with

​	如果您陷入困境并决定放弃整个合并，您始终可以返回到合并之前的状态：

``` bash
$ git merge --abort
```

Or, if you’ve already committed the merge that you want to throw away,

​	或者，如果您已经提交了要丢弃的合并：

``` bash
$ git reset --hard ORIG_HEAD
```

However, this last command can be dangerous in some cases—never throw away a commit you have already committed if that commit may itself have been merged into another branch, as doing so may confuse further merges.

​	但是，在某些情况下，这最后一个命令可能会很危险，如果您已经提交了该提交并且该提交本身可能已经合并到另一个分支中，请不要执行此操作，因为这可能会导致进一步的合并混淆。

### 快速前进合并 Fast-forward merges

There is one special case not mentioned above, which is treated differently. Normally, a merge results in a merge commit, with two parents, one pointing at each of the two lines of development that were merged.

​	上面没有提到的一种特殊情况会受到不同的对待。通常，合并会生成一个合并提交，其中包含两个父提交，一个指向合并的两个开发线中的每个提交。

However, if the current branch is an ancestor of the other—so every commit present in the current branch is already contained in the other branch—then Git just performs a "fast-forward"; the head of the current branch is moved forward to point at the head of the merged-in branch, without any new commits being created.

​	但是，如果当前分支是另一个分支的祖先——也就是说，当前分支中的每个提交已经包含在另一个分支中——那么Git只会执行“快速前进”；当前分支的头指针被移动到合并分支的头指针，而不会创建任何新的提交。

### 修复错误

If you’ve messed up the working tree, but haven’t yet committed your mistake, you can return the entire working tree to the last committed state with

​	如果您搞乱了工作树，但尚未提交错误，您可以使用以下命令将整个工作树恢复到上次提交的状态：

``` bash
$ git restore --staged --worktree :/
```

If you make a commit that you later wish you hadn’t, there are two fundamentally different ways to fix the problem:

​	如果您做了一个后来希望没有做的提交，有两种基本不同的方式来解决这个问题：

1. You can create a new commit that undoes whatever was done by the old commit. This is the correct thing if your mistake has already been made public.
2. 您可以创建一个新的提交，撤销旧提交所做的任何更改。如果您的错误已经公开发表，这是正确的做法。
3. You can go back and modify the old commit. You should never do this if you have already made the history public; Git does not normally expect the "history" of a project to change, and cannot correctly perform repeated merges from a branch that has had its history changed.
4. 您可以返回并修改旧提交。如果您已经公开发布历史记录，请永远不要这样做；Git通常不希望项目的“历史记录”发生变化，并且不能正确地从已更改其历史记录的分支执行重复合并。

#### 使用新提交修复错误 Fixing a mistake with a new commit

Creating a new commit that reverts an earlier change is very easy; just pass the [git-revert[1]](../1/git-revert) command a reference to the bad commit; for example, to revert the most recent commit:

​	创建一个新提交以撤消先前的更改非常简单；只需将[git-revert[1]](../1/git-revert)命令传递给不良提交的引用。例如，要撤消最近的提交：

``` bash
$ git revert HEAD
```

This will create a new commit which undoes the change in HEAD. You will be given a chance to edit the commit message for the new commit.

​	这将创建一个新的提交，该提交将撤消HEAD中的更改。您将有机会编辑新提交的提交消息。

You can also revert an earlier change, for example, the next-to-last:

​	您还可以撤消早期的更改，例如，倒数第二个提交：

``` bash
$ git revert HEAD^
```

In this case Git will attempt to undo the old change while leaving intact any changes made since then. If more recent changes overlap with the changes to be reverted, then you will be asked to fix conflicts manually, just as in the case of [resolving a merge](https://git-scm.com/docs/user-manual#resolving-a-merge).

​	在这种情况下，Git将尝试撤消旧的更改，同时保留自那时以来所做的任何更改。如果较新的更改与要撤消的更改重叠，则将要求您手动解决冲突，就像[解决合并冲突](https://git-scm.com/docs/user-manual#resolving-a-merge)的情况一样。

#### 通过重写历史记录来修复错误 Fixing a mistake by rewriting history

If the problematic commit is the most recent commit, and you have not yet made that commit public, then you may just [destroy it using `git reset`](https://git-scm.com/docs/user-manual#undoing-a-merge).

​	如果有问题的提交是最近的提交，并且您尚未公开该提交，那么您可以使用`git reset`命令[摧毁该提交](https://git-scm.com/docs/user-manual#undoing-a-merge)。

Alternatively, you can edit the working directory and update the index to fix your mistake, just as if you were going to [create a new commit](https://git-scm.com/docs/user-manual#how-to-make-a-commit), then run

​	或者，您可以编辑工作目录并更新索引来修复错误，就像您要[创建一个新提交](https://git-scm.com/docs/user-manual#how-to-make-a-commit)一样，然后运行以下命令：

``` bash
$ git commit --amend
```

which will replace the old commit by a new commit incorporating your changes, giving you a chance to edit the old commit message first.

这将通过包含您的更改替换旧提交，您将有机会首先编辑旧提交消息。

Again, you should never do this to a commit that may already have been merged into another branch; use [git-revert[1]](../1/git-revert) instead in that case.

​	再次强调，在可能已经合并到另一个分支中的提交上永远不要执行此操作；在这种情况下，请改用[git-revert[1]](../1/git-revert)。

It is also possible to replace commits further back in the history, but this is an advanced topic to be left for [another chapter](https://git-scm.com/docs/user-manual#cleaning-up-history).

​	还可以替换历史记录中更早的提交，但这是一个需要留给[另一章节](https://git-scm.com/docs/user-manual#cleaning-up-history)处理的高级主题。

#### 检出文件的旧版本 Checking out an old version of a file

In the process of undoing a previous bad change, you may find it useful to check out an older version of a particular file using [git-restore[1]](../1/git-restore). The command

​	在撤消之前的错误更改的过程中，您可能会发现使用[git-restore[1]](../1/git-restore)来检出特定文件的旧版本很有用。该命令如下：

``` bash
$ git restore --source=HEAD^ path/to/file
```

replaces path/to/file by the contents it had in the commit HEAD^, and also updates the index to match. It does not change branches.

这将用HEAD^提交中的内容替换path/to/file，并更新索引以匹配。它不会改变分支。

If you just want to look at an old version of the file, without modifying the working directory, you can do that with [git-show[1]](../1/git-show):

​	如果您只是想查看文件的旧版本，而不修改工作目录，可以使用[git-show[1]](../1/git-show)实现：

``` bash
$ git show HEAD^:path/to/file
```

which will display the given version of the file.

它将显示给定版本的文件内容。

#### 暂时搁置进行中的工作 Temporarily setting aside work in progress

While you are in the middle of working on something complicated, you find an unrelated but obvious and trivial bug. You would like to fix it before continuing. You can use [git-stash[1]](../1/git-stash) to save the current state of your work, and after fixing the bug (or, optionally after doing so on a different branch and then coming back), unstash the work-in-progress changes.

​	当您正在处理一些复杂的工作时，您可能会发现一个无关但显而易见和琐碎的错误。您希望在继续工作之前先修复它。您可以使用[git-stash[1]](../1/git-stash)来保存当前工作状态，在修复错误后（或者在不同的分支上进行修复，然后再返回），恢复进行中的工作更改。

``` bash
$ git stash push -m "work in progress for foo feature"
```

This command will save your changes away to the `stash`, and reset your working tree and the index to match the tip of your current branch. Then you can make your fix as usual.

​	这个命令将把您的更改保存到`stash`，并将您的工作树和索引重置为与当前分支的最新提交匹配。然后您可以像往常一样进行修复。

```
... edit and test ...
$ git commit -a -m "blorpl: typofix"
```

After that, you can go back to what you were working on with `git stash pop`:

​	之后，您可以使用`git stash pop`回到您之前进行的工作：

``` bash
$ git stash pop
```

### 确保良好的性能

On large repositories, Git depends on compression to keep the history information from taking up too much space on disk or in memory. Some Git commands may automatically run [git-gc[1]](../1/git-gc), so you don’t have to worry about running it manually. However, compressing a large repository may take a while, so you may want to call `gc` explicitly to avoid automatic compression kicking in when it is not convenient.

​	在大型仓库中，Git依赖压缩来确保历史信息在磁盘或内存中不占用太多空间。一些Git命令可能会自动运行[git-gc[1]](../1/git-gc)，因此您不必手动运行它。但是，压缩大型仓库可能需要一些时间，因此您可能希望显式调用`gc`，以避免在不方便的时候自动触发压缩。

### 确保可靠性

#### 检查仓库是否损坏

The [git-fsck[1]](../1/git-fsck) command runs a number of self-consistency checks on the repository, and reports on any problems. This may take some time.

​	[git-fsck[1]](../1/git-fsck)命令对仓库运行多个自一致性检查，并报告任何问题。这可能需要一些时间。

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

You will see informational messages on dangling objects. They are objects that still exist in the repository but are no longer referenced by any of your branches, and can (and will) be removed after a while with `gc`. You can run `git fsck --no-dangling` to suppress these messages, and still view real errors.

​	您将看到关于悬空对象的信息。它们是仍然存在于仓库中但不再由您的任何分支引用的对象，它们可能会在一段时间后通过`gc`被移除。您可以运行`git fsck --no-dangling`来抑制这些消息，并查看实际的错误。

#### 恢复丢失的更改

##### Reflogs

Say you modify a branch with [`git reset --hard`](https://git-scm.com/docs/user-manual#fixing-mistakes), and then realize that the branch was the only reference you had to that point in history.

​	假设您使用[`git reset --hard`](https://git-scm.com/docs/user-manual#fixing-mistakes)修改了一个分支，然后意识到该分支是您在历史中唯一的引用。

Fortunately, Git also keeps a log, called a "reflog", of all the previous values of each branch. So in this case you can still find the old history using, for example,

​	幸运的是，Git还保存了一个称为"reflog"的日志，记录了每个分支的所有先前值。因此，在这种情况下，您仍然可以使用以下方式找到旧的历史记录：

``` bash
$ git log master@{1}
```

This lists the commits reachable from the previous version of the `master` branch head. This syntax can be used with any Git command that accepts a commit, not just with `git log`. Some other examples:

​	这会列出可从`master`分支上一个版本的提交中访问的提交。此语法可用于接受提交的任何Git命令，不仅限于`git log`。其他一些示例：

``` bash
$ git show master@{2}		# See where the branch pointed 2,
$ git show master@{3}		# 3, ... changes ago.
$ gitk master@{yesterday}	# See where it pointed yesterday,
$ gitk master@{"1 week ago"}	# ... or last week
$ git log --walk-reflogs master	# show reflog entries for master
```

A separate reflog is kept for the HEAD, so

​	HEAD也保留了单独的reflog，因此

``` bash
$ git show HEAD@{"1 week ago"}
```

will show what HEAD pointed to one week ago, not what the current branch pointed to one week ago. This allows you to see the history of what you’ve checked out.

将显示HEAD一周前指向的地方，而不是当前分支一周前指向的地方。这允许您查看您检出的内容的历史。

The reflogs are kept by default for 30 days, after which they may be pruned. See [git-reflog[1]](../1/git-reflog) and [git-gc[1]](../1/git-gc) to learn how to control this pruning, and see the "SPECIFYING REVISIONS" section of [gitrevisions[7]](../7/gitrevisions) for details.

​	默认情况下，reflogs保留30天，之后它们可能会被修剪。查看[git-reflog[1]](../1/git-reflog)和[git-gc[1]](../1/git-gc)以了解如何控制此修剪，以及在[gitrevisions[7]](../7/gitrevisions)的“指定修订”部分中了解详情。

Note that the reflog history is very different from normal Git history. While normal history is shared by every repository that works on the same project, the reflog history is not shared: it tells you only about how the branches in your local repository have changed over time.

​	请注意，reflog历史与普通的Git历史非常不同。尽管普通历史由在同一项目上工作的每个仓库共享，但reflog历史不是共享的：它只告诉您有关本地仓库中分支的更改方式。

##### 检查悬空对象 Examining dangling objects

In some situations the reflog may not be able to save you. For example, suppose you delete a branch, then realize you need the history it contained. The reflog is also deleted; however, if you have not yet pruned the repository, then you may still be able to find the lost commits in the dangling objects that `git fsck` reports. See [Dangling objects](https://git-scm.com/docs/user-manual#dangling-objects) for the details.

​	在某些情况下，reflog可能无法拯救您。例如，假设您删除了一个分支，然后意识到您需要其中包含的历史。reflog也被删除；然而，如果您尚未对仓库进行修剪，那么您仍然可以在`git fsck`报告的悬空对象中找到丢失的提交。有关详细信息，请参阅[Dangling objects](https://git-scm.com/docs/user-manual#dangling-objects)。

``` bash
$ git fsck
dangling commit 7281251ddd2a61e38657c827739c57015671a6b3
dangling commit 2706a059f258c6b245f298dc4ff2ccd30ec21a63
dangling commit 13472b7c4b80851a1bc551779171dcb03655e9b5
...
```

You can examine one of those dangling commits with, for example,

​	您可以使用以下方式检查其中一个悬空提交：

``` bash
$ gitk 7281251ddd --not --all
```

which does what it sounds like: it says that you want to see the commit history that is described by the dangling commit(s), but not the history that is described by all your existing branches and tags. Thus you get exactly the history reachable from that commit that is lost. (And notice that it might not be just one commit: we only report the "tip of the line" as being dangling, but there might be a whole deep and complex commit history that was dropped.)

这听起来就是它的作用：它表示您希望查看由悬空提交描述的提交历史，但不包括所有现有分支和标签所描述的历史。因此，您得到的是可以访问丢失的提交的历史。(请注意，可能不止一个提交：我们仅报告“线的尖端”是悬空的，但可能存在一个完整的深度和复杂的提交历史被丢弃。)

If you decide you want the history back, you can always create a new reference pointing to it, for example, a new branch:

​	如果您决定要还原历史记录，您可以随时创建一个指向它的新引用，例如，新的分支：

``` bash
$ git branch recovered-branch 7281251ddd
```

Other types of dangling objects (blobs and trees) are also possible, and dangling objects can arise in other situations.

​	其他类型的悬空对象（blobs和trees）也是可能的，并且悬空对象可能在其他情况下出现。

## 与他人共享开发

### 使用`git pull`获取更新

After you clone a repository and commit a few changes of your own, you may wish to check the original repository for updates and merge them into your own work.

​	在克隆了一个仓库并提交了一些您自己的更改后，您可能希望检查原始仓库是否有更新，并将其合并到您自己的工作中。

We have already seen [how to keep remote-tracking branches up to date](https://git-scm.com/docs/user-manual#Updating-a-repository-With-git-fetch) with [git-fetch[1]](../1/git-fetch), and how to merge two branches. So you can merge in changes from the original repository’s master branch with:

​	我们已经了解了[如何保持远程跟踪分支更新](https://git-scm.com/docs/user-manual#Updating-a-repository-With-git-fetch)（使用[git-fetch[1]](../1/git-fetch)）以及如何合并两个分支。因此，您可以将来自原始仓库的更改合并到您的仓库的主分支中：

``` bash
$ git fetch
$ git merge origin/master
```

However, the [git-pull[1]](../1/git-pull) command provides a way to do this in one step:

​	然而，[git-pull[1]](../1/git-pull)命令提供了一种一步完成此操作的方法：

``` bash
$ git pull origin master
```

In fact, if you have `master` checked out, then this branch has been configured by `git clone` to get changes from the HEAD branch of the origin repository. So often you can accomplish the above with just a simple

​	实际上，如果您已经检出了`master`分支，那么这个分支已经由`git clone`配置为从原始仓库的HEAD分支获取更改。因此，通常您可以只需使用一个简单的命令完成上面的操作：

``` bash
$ git pull
```

This command will fetch changes from the remote branches to your remote-tracking branches `origin/*`, and merge the default branch into the current branch.

​	这个命令将获取远程分支的更改并合并到您的远程跟踪分支`origin/*`，然后将默认分支合并到当前分支。

More generally, a branch that is created from a remote-tracking branch will pull by default from that branch. See the descriptions of the `branch.<name>.remote` and `branch.<name>.merge` options in [git-config[1]](../1/git-config), and the discussion of the `--track` option in [git-checkout[1]](../1/git-checkout), to learn how to control these defaults.

​	更一般地，从远程跟踪分支创建的分支将默认从该分支进行拉取。请参阅[git-config[1]](../1/git-config)中`branch.<name>.remote`和`branch.<name>.merge`选项的描述，以及[git-checkout[1]](../1/git-checkout)中关于`--track`选项的讨论，以了解如何控制这些默认设置。

In addition to saving you keystrokes, `git pull` also helps you by producing a default commit message documenting the branch and repository that you pulled from.

​	除了节省您的击键外，`git pull`还通过生成默认的提交消息来帮助您记录您从哪个分支和仓库拉取的更改。

(But note that no such commit will be created in the case of a [fast-forward](https://git-scm.com/docs/user-manual#fast-forwards); instead, your branch will just be updated to point to the latest commit from the upstream branch.)

（但请注意，在[快进合并](https://git-scm.com/docs/user-manual#fast-forwards)的情况下，将不会创建此类提交；相反，您的分支将只是更新为指向上游分支的最新提交。）

The `git pull` command can also be given `.` as the "remote" repository, in which case it just merges in a branch from the current repository; so the commands

​	`git pull`命令还可以将`.`作为“远程”仓库，这样它只会合并当前仓库中的一个分支；因此，以下命令是大致等效的：

``` bash
$ git pull . branch
$ git merge branch
```

are roughly equivalent.

### 提交补丁给项目

If you just have a few changes, the simplest way to submit them may just be to send them as patches in email:

​	如果您只有少量更改，最简单的提交方式可能是通过电子邮件发送它们作为补丁：

First, use [git-format-patch[1]](../1/git-format-patch); for example:

​	首先，使用[git-format-patch[1]](../1/git-format-patch)命令；例如：

``` bash
$ git format-patch origin
```

will produce a numbered series of files in the current directory, one for each patch in the current branch but not in `origin/HEAD`.

将在当前目录中生成一系列编号的文件，每个文件对应当前分支中的一个补丁，但不包含在`origin/HEAD`中的补丁。

`git format-patch` can include an initial "cover letter". You can insert commentary on individual patches after the three dash line which `format-patch` places after the commit message but before the patch itself. If you use `git notes` to track your cover letter material, `git format-patch --notes` will include the commit’s notes in a similar manner.	

​	`git format-patch`可以包括一个初始的“封面信件”。您可以在提交消息后三个破折号（`---`）之后但在补丁之前插入对每个补丁的评论。如果使用`git notes`跟踪您的封面信件内容，`git format-patch --notes`将以类似的方式包括提交的备注。

You can then import these into your mail client and send them by hand. However, if you have a lot to send at once, you may prefer to use the [git-send-email[1]](../1/git-send-email) script to automate the process. Consult the mailing list for your project first to determine their requirements for submitting patches.

​	然后，您可以将这些补丁导入到您的邮件客户端中，并手动发送它们。然而，如果您需要一次发送大量补丁，您可能更喜欢使用[git-send-email[1]](../1/git-send-email)脚本来自动化此过程。首先请咨询您的项目邮件列表，以确定他们提交补丁的要求。

### 导入补丁到项目

Git also provides a tool called [git-am[1]](../1/git-am) (am stands for "apply mailbox"), for importing such an emailed series of patches. Just save all of the patch-containing messages, in order, into a single mailbox file, say `patches.mbox`, then run

​	Git还提供了一个名为[git-am[1]](../1/git-am)（am代表"apply mailbox"）的工具，用于导入这样一系列通过电子邮件发送的补丁。只需将所有包含补丁的消息按顺序保存到一个单独的邮箱文件中，例如`patches.mbox`，然后运行：

``` bash
$ git am -3 patches.mbox
```

Git will apply each patch in order; if any conflicts are found, it will stop, and you can fix the conflicts as described in "[Resolving a merge](https://git-scm.com/docs/user-manual#resolving-a-merge)". (The `-3` option tells Git to perform a merge; if you would prefer it just to abort and leave your tree and index untouched, you may omit that option.)

​	Git将按顺序应用每个补丁；如果发现任何冲突，它将停止，并且您可以按照"[解决合并冲突](https://git-scm.com/docs/user-manual#resolving-a-merge)"中描述的方式解决冲突。（`-3`选项告诉Git执行合并；如果您希望它只是中止并保持您的树和索引不变，您可以省略该选项。）

Once the index is updated with the results of the conflict resolution, instead of creating a new commit, just run

​	一旦索引更新了冲突解决的结果，而不是创建一个新的提交，只需运行：

``` bash
$ git am --continue
```

and Git will create the commit for you and continue applying the remaining patches from the mailbox.

Git将为您创建提交，并继续从邮箱中应用剩余的补丁。

The final result will be a series of commits, one for each patch in the original mailbox, with authorship and commit log message each taken from the message containing each patch.

​	最终的结果将是一系列提交，每个提交对应于原始邮箱中的一个补丁，其中作者和提交日志消息分别来自包含每个补丁的消息。

### 公共Git仓库

Another way to submit changes to a project is to tell the maintainer of that project to pull the changes from your repository using [git-pull[1]](../1/git-pull). In the section "[Getting updates with `git pull`](https://git-scm.com/docs/user-manual#getting-updates-With-git-pull)" we described this as a way to get updates from the "main" repository, but it works just as well in the other direction.

​	另一种向项目提交更改的方法是告诉该项目的维护者从您的仓库中拉取更改，使用[git-pull[1]](../1/git-pull)。在"[使用`git pull`获取更新](https://git-scm.com/docs/user-manual#getting-updates-With-git-pull)"一节中，我们将其描述为从“主”仓库获取更新的方法，但在另一个方向上同样有效。

If you and the maintainer both have accounts on the same machine, then you can just pull changes from each other’s repositories directly; commands that accept repository URLs as arguments will also accept a local directory name:

​	如果您和维护者在同一台机器上都有账号，那么您可以直接从彼此的仓库中拉取更改；接受仓库URL作为参数的命令也将接受一个本地目录名称：

``` bash
$ git clone /path/to/repository
$ git pull /path/to/other/repository
```

or an ssh URL:

或者使用ssh URL：

``` bash
$ git clone ssh://yourhost/~you/repository
```

For projects with few developers, or for synchronizing a few private repositories, this may be all you need.

​	对于开发者较少的项目，或用于同步几个私有仓库，这可能已经足够。

However, the more common way to do this is to maintain a separate public repository (usually on a different host) for others to pull changes from. This is usually more convenient, and allows you to cleanly separate private work in progress from publicly visible work.

​	然而，更常见的方法是为他人提供一个单独的公共仓库（通常在不同的主机上），供他们从中拉取更改。这通常更方便，可以使您将私有的工作进度与公开可见的工作清晰地分开。

You will continue to do your day-to-day work in your personal repository, but periodically "push" changes from your personal repository into your public repository, allowing other developers to pull from that repository. So the flow of changes, in a situation where there is one other developer with a public repository, looks like this:

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

We explain how to do this in the following sections.

​	我们将在接下来的章节中解释如何做到这一点。

### 设置公共仓库 Setting up a public repository

Assume your personal repository is in the directory `~/proj`. We first create a new clone of the repository and tell `git daemon` that it is meant to be public:

​	假设您的个人仓库位于目录`~/proj`。我们首先创建仓库的一个新克隆，并告知`git daemon`它是公共仓库：

``` bash
$ git clone --bare ~/proj proj.git
$ touch proj.git/git-daemon-export-ok
```

The resulting directory proj.git contains a "bare" git repository—it is just the contents of the `.git` directory, without any files checked out around it.

​	生成的`proj.git`目录包含一个“裸”（bare）的git仓库——它只是`.git`目录的内容，没有围绕它的任何文件被检出。

Next, copy `proj.git` to the server where you plan to host the public repository. You can use scp, rsync, or whatever is most convenient.

​	接下来，将`proj.git`复制到您计划托管公共仓库的服务器上。您可以使用scp、rsync或任何最方便的方法。

### 通过Git协议导出Git仓库 Exporting a Git repository via the Git protocol

This is the preferred method.

​	这是首选的方法。

If someone else administers the server, they should tell you what directory to put the repository in, and what `git://` URL it will appear at. You can then skip to the section "[Pushing changes to a public repository](https://git-scm.com/docs/user-manual#pushing-changes-to-a-public-repository)", below.

​	如果其他人管理服务器，他们应该告诉您在哪个目录下放置仓库，以及仓库在`git://`URL下的位置。然后，您可以跳到下面的章节"[向公共仓库推送更改](https://git-scm.com/docs/user-manual#pushing-changes-to-a-public-repository)"。

Otherwise, all you need to do is start [git-daemon[1]](../1/git-daemon); it will listen on port 9418. By default, it will allow access to any directory that looks like a Git directory and contains the magic file git-daemon-export-ok. Passing some directory paths as `git daemon` arguments will further restrict the exports to those paths.

​	否则，您只需要启动[git-daemon[1]](../1/git-daemon)；它将监听9418端口。默认情况下，它允许访问任何看起来像Git仓库并包含魔法文件`git-daemon-export-ok`的目录。通过将一些目录路径作为`git daemon`的参数，可以进一步限制导出到这些路径。

You can also run `git daemon` as an inetd service; see the [git-daemon[1]](../1/git-daemon) man page for details. (See especially the examples section.)

​	您也可以将`git daemon`作为inetd服务运行；请参阅[git-daemon[1]](../1/git-daemon)手册中的详细信息。（尤其是例子部分。）

### 通过HTTP导出Git仓库 

The Git protocol gives better performance and reliability, but on a host with a web server set up, HTTP exports may be simpler to set up.

​	Git协议提供更好的性能和可靠性，但在配置了Web服务器的主机上，使用HTTP导出可能更简单。

All you need to do is place the newly created bare Git repository in a directory that is exported by the web server, and make some adjustments to give web clients some extra information they need:

​	您只需将新创建的裸Git仓库放置在由Web服务器导出的目录中，并进行一些调整以向Web客户端提供一些额外的所需信息：

``` bash
$ mv proj.git /home/you/public_html/proj.git
$ cd proj.git
$ git --bare update-server-info
$ mv hooks/post-update.sample hooks/post-update
```

(For an explanation of the last two lines, see [git-update-server-info[1]](../1/git-update-server-info) and [githooks[5]](../5/githooks).)

（有关最后两行的解释，请参阅[git-update-server-info[1]](../1/git-update-server-info)和[githooks[5]](../5/githooks)。）

Advertise the URL of `proj.git`. Anybody else should then be able to clone or pull from that URL, for example with a command line like:

​	公布`proj.git`的URL。其他任何人随后都能使用命令行克隆或拉取：

``` bash
$ git clone http://yourserver.com/~you/proj.git
```

(See also [setup-git-server-over-http](https://git-scm.com/docs/howto/setup-git-server-over-http) for a slightly more sophisticated setup using WebDAV which also allows pushing over HTTP.)

（对于稍微复杂一些的使用WebDAV的设置，也允许通过HTTP推送，请参阅[setup-git-server-over-http](https://git-scm.com/docs/howto/setup-git-server-over-http)。）

### 向公共仓库推送更改

Note that the two techniques outlined above (exporting via [http](https://git-scm.com/docs/user-manual#exporting-via-http) or [git](https://git-scm.com/docs/user-manual#exporting-via-git)) allow other maintainers to fetch your latest changes, but they do not allow write access, which you will need to update the public repository with the latest changes created in your private repository.

​	请注意，上述两种方法（通过[http](https://git-scm.com/docs/user-manual#exporting-via-http)或[git](https://git-scm.com/docs/user-manual#exporting-via-git)导出）允许其他维护者获取您的最新更改，但不允许写入访问权限，而您需要这样的权限来将私有仓库中创建的最新更改更新到公共仓库。

The simplest way to do this is using [git-push[1]](../1/git-push) and ssh; to update the remote branch named `master` with the latest state of your branch named `master`, run

​	最简单的方法是使用[git-push[1]](../1/git-push)和SSH来实现；要更新远程分支`master`为您的`master`分支的最新状态，请运行：

``` bash
$ git push ssh://yourserver.com/~you/proj.git master:master
```

或者只需执行：

``` bash
$ git push ssh://yourserver.com/~you/proj.git master
```

As with `git fetch`, `git push` will complain if this does not result in a [fast-forward](https://git-scm.com/docs/user-manual#fast-forwards); see the following section for details on handling this case.

​	与`git fetch`类似，如果这不会导致[fast-forward合并](https://git-scm.com/docs/user-manual#fast-forwards)，`git push`会报错；有关处理此情况的详细信息，请参见下一节。

Note that the target of a `push` is normally a [bare](https://git-scm.com/docs/user-manual#def_bare_repository) repository. You can also push to a repository that has a checked-out working tree, but a push to update the currently checked-out branch is denied by default to prevent confusion. See the description of the receive.denyCurrentBranch option in [git-config[1]](../1/git-config) for details.

​	请注意，`push`的目标通常是[裸仓库](https://git-scm.com/docs/user-manual#def_bare_repository)。您也可以推送到一个已检出工作树的仓库，但默认情况下，不允许推送更新当前已检出分支，以防止混淆。有关详细信息，请参阅[git-config[1]](../1/git-config)中`receive.denyCurrentBranch`选项的描述。

As with `git fetch`, you may also set up configuration options to save typing; so, for example:

​	与`git fetch`一样，您还可以设置配置选项以节省输入；例如：

``` bash
$ git remote add public-repo ssh://yourserver.com/~you/proj.git
```

adds the following to `.git/config`:

将以下内容添加到`.git/config`：

```
[remote "public-repo"]
	url = yourserver.com:proj.git
	fetch = +refs/heads/*:refs/remotes/example/*
```

which lets you do the same push with just

这使您可以仅使用以下命令进行推送：

``` bash
$ git push public-repo master
```

See the explanations of the `remote.<name>.url`, `branch.<name>.remote`, and `remote.<name>.push` options in [git-config[1]](../1/git-config) for details.

​	有关详细信息，请参见[git-config[1]](../1/git-config)中`remote.<name>.url`、`branch.<name>.remote`和`remote.<name>.push`选项的解释。

### 当推送失败时怎么办

If a push would not result in a [fast-forward](https://git-scm.com/docs/user-manual#fast-forwards) of the remote branch, then it will fail with an error like:

​	如果推送不会导致[fast-forward合并](https://git-scm.com/docs/user-manual#fast-forwards)远程分支，则会失败，并显示错误消息，例如：

```
 ! [rejected]        master -> master (non-fast-forward)
error: failed to push some refs to '...'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

This can happen, for example, if you:

​	这可能发生在以下情况下：

- use `git reset --hard` to remove already-published commits, or
- use `git commit --amend` to replace already-published commits (as in [Fixing a mistake by rewriting history](https://git-scm.com/docs/user-manual#fixing-a-mistake-by-rewriting-history)), or
- use `git rebase` to rebase any already-published commits (as in [Keeping a patch series up to date using git rebase](https://git-scm.com/docs/user-manual#using-git-rebase)).
- 使用`git reset --hard`删除已发布的提交，或者
- 使用`git commit --amend`替换已发布的提交（如[通过重写历史来修正错误](https://git-scm.com/docs/user-manual#fixing-a-mistake-by-rewriting-history)所示），或者
- 使用`git rebase`重新基于任何已发布的提交（如[使用git rebase保持补丁系列最新](https://git-scm.com/docs/user-manual#using-git-rebase)所示）。

You may force `git push` to perform the update anyway by preceding the branch name with a plus sign:

​	您可以通过在分支名称前加上加号来强制`git push`进行更新：

``` bash
$ git push ssh://yourserver.com/~you/proj.git +master
```

Note the addition of the `+` sign. Alternatively, you can use the `-f` flag to force the remote update, as in:

​	请注意加号的添加。或者，您可以使用`-f`标志来强制远程更新，如下所示：

``` bash
$ git push -f ssh://yourserver.com/~you/proj.git master
```

Normally whenever a branch head in a public repository is modified, it is modified to point to a descendant of the commit that it pointed to before. By forcing a push in this situation, you break that convention. (See [Problems with rewriting history](https://git-scm.com/docs/user-manual#problems-With-rewriting-history).)

​	通常，当公共仓库中的分支头被修改时，它将被修改为指向之前所指向的提交的后代。在这种情况下强制推送，将打破这一约定。（参见[重写历史可能带来的问题](https://git-scm.com/docs/user-manual#problems-With-rewriting-history)。）

Nevertheless, this is a common practice for people that need a simple way to publish a work-in-progress patch series, and it is an acceptable compromise as long as you warn other developers that this is how you intend to manage the branch.

​	然而，对于需要简单地发布正在进行的补丁系列的人来说，这是一种常见的做法，只要您警告其他开发者，这是您打算管理分支的方式，这是可以接受的妥协。

It’s also possible for a push to fail in this way when other people have the right to push to the same repository. In that case, the correct solution is to retry the push after first updating your work: either by a pull, or by a fetch followed by a rebase; see the [next section](https://git-scm.com/docs/user-manual#setting-up-a-shared-repository) and [gitcvs-migration[7]](../7/gitcvs-migration) for more.

​	当多个人有权推送到同一仓库时，推送可能以这种方式失败。在这种情况下，正确的解决方法是在尝试推送之前先更新您的工作：通过拉取或通过获取和变基。有关更多信息，请参见[下一节](https://git-scm.com/docs/user-manual#setting-up-a-shared-repository)和[gitcvs-migration[7]](../7/gitcvs-migration)。

### 设置共享仓库 Setting up a shared repository

Another way to collaborate is by using a model similar to that commonly used in CVS, where several developers with special rights all push to and pull from a single shared repository. See [gitcvs-migration[7]](../7/gitcvs-migration) for instructions on how to set this up.

​	另一种合作的方式是使用与CVS中常用的模型类似的模型，其中拥有特殊权限的几个开发者都推送和拉取共享的单一仓库。有关如何设置这样的模型，请参阅[gitcvs-migration[7]](../7/gitcvs-migration)。

However, while there is nothing wrong with Git’s support for shared repositories, this mode of operation is not generally recommended, simply because the mode of collaboration that Git supports—by exchanging patches and pulling from public repositories—has so many advantages over the central shared repository:

​	然而，虽然Git对共享仓库提供了支持，但一般不建议采用这种操作方式，因为Git支持的协作方式——通过交换补丁和从公共仓库拉取——相对于中央共享仓库具有诸多优势：

- Git’s ability to quickly import and merge patches allows a single maintainer to process incoming changes even at very high rates. And when that becomes too much, `git pull` provides an easy way for that maintainer to delegate this job to other maintainers while still allowing optional review of incoming changes.
- Git能够快速导入和合并补丁，使单个维护者可以处理非常高速率下的传入更改。当工作量变得过大时，`git pull`提供了一种简单的方式，使维护者可以将这项工作委派给其他维护者，同时仍允许对传入更改进行可选的审查。
- Since every developer’s repository has the same complete copy of the project history, no repository is special, and it is trivial for another developer to take over maintenance of a project, either by mutual agreement, or because a maintainer becomes unresponsive or difficult to work with.
- 由于每个开发者的仓库都拥有项目历史的完整副本，没有仓库是特殊的，其他开发者很容易通过相互协议或者在维护者不响应或难以合作的情况下接管项目的维护。
- The lack of a central group of "committers" means there is less need for formal decisions about who is "in" and who is "out".
- 缺少中央“提交者”组意味着对于谁“在内”和谁“在外”没有太多正式的决定需求。

### 允许web浏览仓库

The gitweb cgi script provides users an easy way to browse your project’s revisions, file contents and logs without having to install Git. Features like RSS/Atom feeds and blame/annotation details may optionally be enabled.

​	gitweb CGI脚本为用户提供了一种无需安装Git即可浏览项目修订、文件内容和日志的简便方式。可选择启用RSS/Atom源和责备/注释详细信息等功能。

The [git-instaweb[1]](../1/git-instaweb) command provides a simple way to start browsing the repository using gitweb. The default server when using instaweb is lighttpd.

​	使用[git-instaweb[1]](../1/git-instaweb)命令可以简单地启动使用gitweb浏览仓库。使用instaweb时，默认服务器是lighttpd。

See the file gitweb/INSTALL in the Git source tree and [gitweb[1]](../1/gitweb) for instructions on details setting up a permanent installation with a CGI or Perl capable server.

​	有关在Git源代码树中设置永久安装与CGI或Perl可用服务器的详细说明，请参阅gitweb/INSTALL文件和[gitweb[1]](../1/gitweb)。

### 如何获取一个最小历史记录的Git仓库 

A [shallow clone](https://git-scm.com/docs/user-manual#def_shallow_clone), with its truncated history, is useful when one is interested only in recent history of a project and getting full history from the upstream is expensive.

​	截断历史记录的[shallow clone](https://git-scm.com/docs/user-manual#def_shallow_clone)在只对项目的最近历史记录感兴趣且从上游获取完整历史记录昂贵时非常有用。

A [shallow clone](https://git-scm.com/docs/user-manual#def_shallow_clone) is created by specifying the [git-clone[1]](../1/git-clone) `--depth` switch. The depth can later be changed with the [git-fetch[1]](../1/git-fetch) `--depth` switch, or full history restored with `--unshallow`.

​	通过指定[git-clone[1]](../1/git-clone)的`--depth`开关来创建[shallow clone](https://git-scm.com/docs/user-manual#def_shallow_clone)。深度可以在后续使用[git-fetch[1]](../1/git-fetch)的`--depth`开关进行更改，或者通过`--unshallow`恢复完整历史。

Merging inside a [shallow clone](https://git-scm.com/docs/user-manual#def_shallow_clone) will work as long as a merge base is in the recent history. Otherwise, it will be like merging unrelated histories and may have to result in huge conflicts. This limitation may make such a repository unsuitable to be used in merge based workflows.

​	在[shallow clone](https://git-scm.com/docs/user-manual#def_shallow_clone)中进行合并，只要合并基在最近的历史中就可以。否则，它将类似于合并不相关的历史，可能导致巨大的冲突。这个限制可能使得这样的仓库在基于合并的工作流中不适合使用。

### 示例

#### 维护Linux子系统的主题分支

This describes how Tony Luck uses Git in his role as maintainer of the IA64 architecture for the Linux kernel.

​	这描述了Tony Luck在Linux内核的IA64架构维护者角色中如何使用Git。

He uses two public branches:

​	他使用两个公共分支：

- A "test" tree into which patches are initially placed so that they can get some exposure when integrated with other ongoing development. This tree is available to Andrew for pulling into -mm whenever he wants.
- “测试”树，最初将补丁放置其中，以便在与其他正在进行的开发集成时可以得到一些曝光。这个树对于Andrew来说是可以随时从-mm拉取的。
- A "release" tree into which tested patches are moved for final sanity checking, and as a vehicle to send them upstream to Linus (by sending him a "please pull" request.)
- “发布”树，其中放置经过测试的补丁，用于进行最终的健全性检查，并且作为将它们发送给Linus的工具（通过向他发送一个“请拉取”请求）。

He also uses a set of temporary branches ("topic branches"), each containing a logical grouping of patches.

​	他还使用一组临时分支（“主题分支”），每个分支包含一组逻辑上相关的补丁。

To set this up, first create your work tree by cloning Linus’s public tree:

​	要设置这个，首先通过克隆Linus的公共树来创建您的工作树：

```bash
$ git clone git://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git work
$ cd work
```

Linus’s tree will be stored in the remote-tracking branch named origin/master, and can be updated using [git-fetch[1]](../1/git-fetch); you can track other public trees using [git-remote[1]](../1/git-remote) to set up a "remote" and [git-fetch[1]](../1/git-fetch) to keep them up to date; see [Repositories and Branches](https://git-scm.com/docs/user-manual#repositories-and-branches).

​	Linus的树将存储在名为origin/master的远程跟踪分支中，并且可以使用[git-fetch[1]](../1/git-fetch)进行更新；您可以使用[git-remote[1]](../1/git-remote)跟踪其他公共树来设置一个“remote”，并且使用[git-fetch[1]](../1/git-fetch)来保持它们最新。有关详细信息，请参阅[仓库和分支](https://git-scm.com/docs/user-manual#repositories-and-branches)。

Now create the branches in which you are going to work; these start out at the current tip of origin/master branch, and should be set up (using the `--track` option to [git-branch[1]](../1/git-branch)) to merge changes in from Linus by default.

​	现在创建要使用的分支；这些分支从origin/master分支的当前尖端开始，并且应该设置为默认合并Linus的更改（使用[git-branch[1]](../1/git-branch)的`--track`选项）。

``` bash
$ git branch --track test origin/master
$ git branch --track release origin/master
```

These can be easily kept up to date using [git-pull[1]](../1/git-pull).

​	这些可以通过[git-pull[1]](../1/git-pull)轻松保持更新。

``` bash
$ git switch test && git pull
$ git switch release && git pull
```

Important note! If you have any local changes in these branches, then this merge will create a commit object in the history (with no local changes Git will simply do a "fast-forward" merge). Many people dislike the "noise" that this creates in the Linux history, so you should avoid doing this capriciously in the `release` branch, as these noisy commits will become part of the permanent history when you ask Linus to pull from the release branch.

​	重要提示！如果这些分支中有任何本地更改，那么合并将在历史记录中创建一个提交对象（如果没有本地更改，Git将执行“快进”合并）。许多人不喜欢这在Linux历史中创建的“噪音”，因此您应该避免在`release`分支中随意执行此操作，因为这些嘈杂的提交将成为向Linus请求从发布分支拉取时永久历史记录的一部分。

A few configuration variables (see [git-config[1]](../1/git-config)) can make it easy to push both branches to your public tree. (See [Setting up a public repository](https://git-scm.com/docs/user-manual#setting-up-a-public-repository).)

​	一些配置变量（请参阅[git-config[1]](../1/git-config)）可以使将这两个分支都推送到您的公共树变得容易。（请参阅[设置公共仓库](https://git-scm.com/docs/user-manual#setting-up-a-public-repository)。）

``` bash
$ cat >> .git/config <<EOF
[remote "mytree"]
	url =  master.kernel.org:/pub/scm/linux/kernel/git/aegl/linux.git
	push = release
	push = test
EOF
```

Then you can push both the test and release trees using [git-push[1]](../1/git-push):

​	然后，您可以使用[git-push[1]](../1/git-push)推送测试和发布树：

``` bash
$ git push mytree
```

or push just one of the test and release branches using:

​	或者只推送其中一个测试或发布分支：

``` bash
$ git push mytree test
```

or

或者

``` bash
$ git push mytree release
```

Now to apply some patches from the community. Think of a short snappy name for a branch to hold this patch (or related group of patches), and create a new branch from a recent stable tag of Linus’s branch. Picking a stable base for your branch will: 1) help you: by avoiding inclusion of unrelated and perhaps lightly tested changes 2) help future bug hunters that use `git bisect` to find problems

​	现在，要应用来自社区的一些补丁，请为保存这个补丁（或相关的一组补丁）的分支命名，并从Linus的分支的最近稳定标签创建一个新的分支。选择一个稳定的基准作为您的分支将有助于：1）帮助您：避免包含不相关的、可能经过轻度测试的更改；2）帮助未来的错误猎人使用`git bisect`来查找问题。

``` bash
$ git switch -c speed-up-spinlocks v2.6.35
```

Now you apply the patch(es), run some tests, and commit the change(s). If the patch is a multi-part series, then you should apply each as a separate commit to this branch.

​	然后应用补丁（或补丁系列），运行一些测试，并提交更改。如果补丁是一个多部分系列，则应将每个部分作为单独的提交应用到此分支。

``` bash
$ ... patch ... test  ... commit [ ... patch ... test ... commit ]*
```

When you are happy with the state of this change, you can merge it into the "test" branch in preparation to make it public:

​	当您对此更改的状态感到满意时，可以合并到“测试”分支，以准备将其公开：

``` bash
$ git switch test && git merge speed-up-spinlocks
```

It is unlikely that you would have any conflicts here … but you might if you spent a while on this step and had also pulled new versions from upstream.

​	在这里，您很可能不会遇到任何冲突……但是，如果您在此步骤上花费了一些时间，并且还拉取了来自上游的新版本，那么可能会有冲突。

Sometime later when enough time has passed and testing done, you can pull the same branch into the `release` tree ready to go upstream. This is where you see the value of keeping each patch (or patch series) in its own branch. It means that the patches can be moved into the `release` tree in any order.

​	稍后，当足够的时间过去并完成了测试，您可以将同一个分支拉入“发布”树，准备好发送给上游。这是您看到在每个补丁（或补丁系列）都保持在自己的分支中的价值所在。这意味着补丁可以以任何顺序移动到“发布”树中。

``` bash
$ git switch release && git merge speed-up-spinlocks
```

After a while, you will have a number of branches, and despite the well chosen names you picked for each of them, you may forget what they are for, or what status they are in. To get a reminder of what changes are in a specific branch, use:

​	过了一段时间后，您将拥有多个分支，并且尽管您为每个分支选择了精心挑选的名称，您可能会忘记它们的用途或状态。为了获得对特定分支所做更改的提醒，请使用：

``` bash
$ git log linux..branchname | git shortlog
```

To see whether it has already been merged into the test or release branches, use:

​	要查看它是否已经合并到测试或发布分支中，请使用：

``` bash
$ git log test..branchname
```

or

或者

``` bash
$ git log release..branchname
```

(If this branch has not yet been merged, you will see some log entries. If it has been merged, then there will be no output.)

（如果该分支尚未合并，您将看到一些日志条目。如果已经合并，那么将不会有输出。）

Once a patch completes the great cycle (moving from test to release, then pulled by Linus, and finally coming back into your local `origin/master` branch), the branch for this change is no longer needed. You detect this when the output from:

​	一旦一个补丁完成了整个循环（从测试到发布，然后由Linus拉取，最后回到您的本地origin/master分支），则不再需要该更改的分支。当您从以下输出检测到这一点时：

``` bash
$ git log origin..branchname
```

is empty. At this point the branch can be deleted:

为空时，表示可以删除该分支：

``` bash
$ git branch -d branchname
```

Some changes are so trivial that it is not necessary to create a separate branch and then merge into each of the test and release branches. For these changes, just apply directly to the `release` branch, and then merge that into the `test` branch.

​	有些更改非常微小，不需要创建单独的分支，然后将其合并到测试和发布分支中。对于这些更改，直接应用到“发布”分支，然后将其合并到“测试”分支中。

After pushing your work to `mytree`, you can use [git-request-pull[1]](../1/git-request-pull) to prepare a "please pull" request message to send to Linus:

​	在将您的工作推送到“mytree”之后，您可以使用[git-request-pull[1]](../1/git-request-pull)准备一个“请拉取”请求消息，然后发送给Linus：

``` bash
$ git push mytree
$ git request-pull origin mytree release
```

Here are some of the scripts that simplify all this even further.

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

Normally commits are only added to a project, never taken away or replaced. Git is designed with this assumption, and violating it will cause Git’s merge machinery (for example) to do the wrong thing.

​	通常情况下，对于一个项目来说，提交的修改只会增加，不会删除或替换。Git是根据这个假设设计的，违反这个假设会导致Git的合并机制（例如）出现问题。

However, there is a situation in which it can be useful to violate this assumption.

​	然而，在某些情况下，违反这个假设可能是有用的。

### 创建完美的补丁系列

Suppose you are a contributor to a large project, and you want to add a complicated feature, and to present it to the other developers in a way that makes it easy for them to read your changes, verify that they are correct, and understand why you made each change.

​	假设您是一个大型项目的贡献者，并且想要添加一个复杂的功能，并以一种使其他开发者能够轻松阅读您的更改、验证其正确性并理解您为何进行每个更改的方式呈现给他们。

If you present all of your changes as a single patch (or commit), they may find that it is too much to digest all at once.

​	如果您将所有更改都呈现为单个补丁（或提交），他们可能会发现一次性消化太多内容。

If you present them with the entire history of your work, complete with mistakes, corrections, and dead ends, they may be overwhelmed.

​	如果您向他们展示整个工作历史，包括错误、修正和死胡同，他们可能会不知所措。

So the ideal is usually to produce a series of patches such that:

​	因此，通常的做法是生成一系列补丁，满足以下条件：

1. Each patch can be applied in order.
2. 每个补丁可以按顺序应用。
3. Each patch includes a single logical change, together with a message explaining the change.
4. 每个补丁包含一个单一的逻辑更改，并附带一个解释此更改的消息。
5. No patch introduces a regression: after applying any initial part of the series, the resulting project still compiles and works, and has no bugs that it didn’t have before.
6. 没有补丁引入回归问题：在应用了系列的任何初始部分后，生成的项目仍然可以编译和运行，并且没有之前没有的错误。
7. The complete series produces the same end result as your own (probably much messier!) development process did.
8. 完整的补丁系列产生的结果与您自己（可能是混乱得多的）开发过程产生的结果相同。

We will introduce some tools that can help you do this, explain how to use them, and then explain some of the problems that can arise because you are rewriting history.

​	我们将介绍一些工具，这些工具可以帮助您做到这一点，并解释如何使用它们，然后解释因为您正在重写历史而可能出现的一些问题。

### 使用git rebase保持补丁系列最新

Suppose that you create a branch `mywork` on a remote-tracking branch `origin`, and create some commits on top of it:

​	假设您在一个远程跟踪分支`origin`上创建了一个分支`mywork`，并在其上创建了一些提交：

``` bash
$ git switch -c mywork origin
$ vi file.txt
$ git commit
$ vi otherfile.txt
$ git commit
...
```

You have performed no merges into mywork, so it is just a simple linear sequence of patches on top of `origin`:

​	您还没有将任何提交合并到mywork中，因此它只是在`origin`之上的一个简单线性序列的补丁：

```
 o--o--O <-- origin
        \
	 a--b--c <-- mywork
```

Some more interesting work has been done in the upstream project, and `origin` has advanced:

​	在上游项目中进行了一些更有趣的工作，`origin`已经前进了：

```
 o--o--O--o--o--o <-- origin
        \
         a--b--c <-- mywork
```

At this point, you could use `pull` to merge your changes back in; the result would create a new merge commit, like this:

​	此时，您可以使用`pull`来合并您的更改；结果将创建一个新的合并提交，如下所示：

```
 o--o--O--o--o--o <-- origin
        \        \
         a--b--c--m <-- mywork
```

However, if you prefer to keep the history in mywork a simple series of commits without any merges, you may instead choose to use [git-rebase[1]](../1/git-rebase):

​	但是，如果您更喜欢在mywork中保持历史记录为一系列简单的提交而没有合并，您可以选择使用[git-rebase[1]](../1/git-rebase)：

``` bash
$ git switch mywork
$ git rebase origin
```

This will remove each of your commits from mywork, temporarily saving them as patches (in a directory named `.git/rebase-apply`), update mywork to point at the latest version of origin, then apply each of the saved patches to the new mywork. The result will look like:

​	这将从mywork中移除每个提交，并临时将它们保存为补丁（保存在名为`.git/rebase-apply`的目录中），然后更新mywork以指向origin的最新版本，然后将每个保存的补丁应用到新的mywork中。结果将如下所示：

```
 o--o--O--o--o--o <-- origin
		 \
		  a'--b'--c' <-- mywork
```

In the process, it may discover conflicts. In that case it will stop and allow you to fix the conflicts; after fixing conflicts, use `git add` to update the index with those contents, and then, instead of running `git commit`, just run

​	在此过程中，可能会发现冲突。在这种情况下，它会停止，并允许您解决冲突；解决冲突后，使用`git add`将索引更新为这些内容，然后，而不是运行`git commit`，只需运行：

``` bash
$ git rebase --continue
```

and Git will continue applying the rest of the patches.

Git将继续应用其余的补丁。

At any point you may use the `--abort` option to abort this process and return mywork to the state it had before you started the rebase:

​	您可以随时使用`--abort`选项中止此过程，并将mywork恢复到在开始rebase之前的状态：

``` bash
$ git rebase --abort
```

If you need to reorder or edit a number of commits in a branch, it may be easier to use `git rebase -i`, which allows you to reorder and squash commits, as well as marking them for individual editing during the rebase. See [Using interactive rebases](https://git-scm.com/docs/user-manual#interactive-rebase) for details, and [Reordering or selecting from a patch series](https://git-scm.com/docs/user-manual#reordering-patch-series) for alternatives.

​	如果需要重新排序或编辑分支中的多个提交，可以使用`git rebase -i`更容易地实现，它允许您重新排序和压缩提交，并在rebase期间将其标记为个别编辑。有关详细信息，请参阅[Using interactive rebases](https://git-scm.com/docs/user-manual#interactive-rebase)以及[Reordering or selecting from a patch series](https://git-scm.com/docs/user-manual#reordering-patch-series)。

### 重写单个提交

We saw in [Fixing a mistake by rewriting history](https://git-scm.com/docs/user-manual#fixing-a-mistake-by-rewriting-history) that you can replace the most recent commit using

​	我们在[修正错误通过重写历史记录](https://git-scm.com/docs/user-manual#fixing-a-mistake-by-rewriting-history)中看到，您可以使用以下命令替换最近的提交：

``` bash
$ git commit --amend
```

which will replace the old commit by a new commit incorporating your changes, giving you a chance to edit the old commit message first. This is useful for fixing typos in your last commit, or for adjusting the patch contents of a poorly staged commit.

这将使用包含您的更改的新提交替换旧的提交，首先为旧的提交消息提供编辑的机会。这对于修复最后一次提交中的拼写错误或调整错误暂存提交的补丁内容很有用。

If you need to amend commits from deeper in your history, you can use [interactive rebase’s `edit` instruction](https://git-scm.com/docs/user-manual#interactive-rebase).

​	如果您需要修改较早的提交，可以使用[交互式rebase的`edit`指令](https://git-scm.com/docs/user-manual#interactive-rebase)。

### 重新排序或选择补丁系列

Sometimes you want to edit a commit deeper in your history. One approach is to use `git format-patch` to create a series of patches and then reset the state to before the patches:

​	有时候您想编辑更早的提交。一种方法是使用`git format-patch`创建一系列补丁，然后将状态重置为补丁之前：

``` bash
$ git format-patch origin
$ git reset --hard origin
```

Then modify, reorder, or eliminate patches as needed before applying them again with [git-am[1]](../1/git-am):

​	然后根据需要修改、重新排序或删除补丁，再使用[git-am[1]](../1/git-am)将其再次应用：

``` bash
$ git am *.patch
```

### 使用交互式rebase

You can also edit a patch series with an interactive rebase. This is the same as [reordering a patch series using `format-patch`](https://git-scm.com/docs/user-manual#reordering-patch-series), so use whichever interface you like best.

​	您还可以使用交互式rebase编辑补丁系列。这与[使用`format-patch`重新排序补丁系列](https://git-scm.com/docs/user-manual#reordering-patch-series)相同，因此请使用您最喜欢的界面。

Rebase your current HEAD on the last commit you want to retain as-is. For example, if you want to reorder the last 5 commits, use:

​	将当前HEAD重新base到您想要保留不变的最后一个提交。例如，如果您想要重新排序最后5个提交，使用：

``` bash
$ git rebase -i HEAD~5
```

This will open your editor with a list of steps to be taken to perform your rebase.

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

As explained in the comments, you can reorder commits, squash them together, edit commit messages, etc. by editing the list. Once you are satisfied, save the list and close your editor, and the rebase will begin.

​	如注释中所解释的，您可以通过编辑列表来重新排序提交、将它们压缩在一起、编辑提交消息等。一旦满意，保存列表并关闭编辑器，重新base就会开始。

The rebase will stop where `pick` has been replaced with `edit` or when a step in the list fails to mechanically resolve conflicts and needs your help. When you are done editing and/or resolving conflicts you can continue with `git rebase --continue`. If you decide that things are getting too hairy, you can always bail out with `git rebase --abort`. Even after the rebase is complete, you can still recover the original branch by using the [reflog](https://git-scm.com/docs/user-manual#reflogs).

​	当将`pick`替换为`edit`，或者列表中的步骤无法机械地解决冲突并需要您的帮助时，重新base将停止。编辑和/或解决冲突后，您可以继续进行`git rebase --continue`。如果觉得情况变得太复杂，您随时可以通过`git rebase --abort`退出。即使在rebase完成后，您仍然可以通过使用[reflog](https://git-scm.com/docs/user-manual#reflogs)来恢复原始分支。

For a more detailed discussion of the procedure and additional tips, see the "INTERACTIVE MODE" section of [git-rebase[1]](../1/git-rebase).

​	有关该过程和其他提示的更详细讨论，请参阅[git-rebase[1]](../1/git-rebase)的“INTERACTIVE MODE”部分。

### 其他工具

There are numerous other tools, such as StGit, which exist for the purpose of maintaining a patch series. These are outside of the scope of this manual.

​	还有许多其他工具，比如StGit，用于维护补丁系列。这些超出了本手册的范围。

### 重写历史的问题

The primary problem with rewriting the history of a branch has to do with merging. Suppose somebody fetches your branch and merges it into their branch, with a result something like this:

​	重写分支的历史的主要问题与合并有关。假设某人拉取了您的分支，并将其合并到他们的分支中，结果可能如下所示：

```
 o--o--O--o--o--o <-- origin
        \        \
         t--t--t--m <-- their branch:
```

Then suppose you modify the last three commits:

​	然后，假设您修改了最后三个提交：

```
	 o--o--o <-- new head of origin
	/
 o--o--O--o--o--o <-- old head of origin
```

If we examined all this history together in one repository, it will look like:

​	如果我们在一个仓库中检查所有这些历史，它将看起来像：

```
	 o--o--o <-- new head of origin
	/
 o--o--O--o--o--o <-- old head of origin
        \        \
         t--t--t--m <-- their branch:
```

Git has no way of knowing that the new head is an updated version of the old head; it treats this situation exactly the same as it would if two developers had independently done the work on the old and new heads in parallel. At this point, if someone attempts to merge the new head in to their branch, Git will attempt to merge together the two (old and new) lines of development, instead of trying to replace the old by the new. The results are likely to be unexpected.

​	Git无法知道新HEAD是旧HEAD的更新版本；它会将此情况与两个开发者在旧HEAD和新HEAD上独立进行工作时完全相同处理。在这一点上，如果有人试图将新HEAD合并到他们的分支中，Git将尝试合并两条（旧和新的）开发线，而不是尝试用新的替换旧的。结果可能出人意料。

You may still choose to publish branches whose history is rewritten, and it may be useful for others to be able to fetch those branches in order to examine or test them, but they should not attempt to pull such branches into their own work.

​	您仍然可以选择发布其历史被重写的分支，并且其他人可以将这些分支fetch下来以便查看或测试，但是他们不应该尝试将这些分支pull到他们自己的工作中。

For true distributed development that supports proper merging, published branches should never be rewritten.

​	对于真正支持正确合并的分布式开发，发布的分支不应该被重写。

### 为什么bisect合并提交可能比bisect线性历史更困难

The [git-bisect[1]](../1/git-bisect) command correctly handles history that includes merge commits. However, when the commit that it finds is a merge commit, the user may need to work harder than usual to figure out why that commit introduced a problem.

​	[git-bisect[1]](../1/git-bisect)命令可以正确处理包含合并提交的历史。然而，当它找到的提交是一个合并提交时，用户可能需要比通常更努力地弄清楚为什么该提交引入了问题。

Imagine this history:

​	假设有以下历史：

```
      ---Z---o---X---...---o---A---C---D
          \                       /
           o---o---Y---...---o---B
```

Suppose that on the upper line of development, the meaning of one of the functions that exists at Z is changed at commit X. The commits from Z leading to A change both the function’s implementation and all calling sites that exist at Z, as well as new calling sites they add, to be consistent. There is no bug at A.

​	假设在开发的上行中，在提交X处更改了Z处存在的一个函数的含义。从Z到A的提交更改了函数的实现以及存在于Z处的所有调用站点，以及它们添加的新调用站点，以使它们保持一致。在A处没有错误。

Suppose that in the meantime on the lower line of development somebody adds a new calling site for that function at commit Y. The commits from Z leading to B all assume the old semantics of that function and the callers and the callee are consistent with each other. There is no bug at B, either.

​	同时假设在下行的开发中，在提交Y处有人为该函数添加了一个新的调用站点。从Z到B的提交都假定该函数和调用者的旧语义是一致的。在B处也没有错误。

Suppose further that the two development lines merge cleanly at C, so no conflict resolution is required.

​	进一步假设两个开发线在C处干净地合并，因此不需要冲突解决。

Nevertheless, the code at C is broken, because the callers added on the lower line of development have not been converted to the new semantics introduced on the upper line of development. So if all you know is that D is bad, that Z is good, and that [git-bisect[1]](../1/git-bisect) identifies C as the culprit, how will you figure out that the problem is due to this change in semantics?

​	然而，C处的代码是有问题的，因为在下行开发的调用者尚未转换为上行开发引入的新语义。所以如果您只知道D是有问题的，Z是正常的，并且[git-bisect[1]](../1/git-bisect)标识C为罪魁祸首，您将如何找出问题是由这种语义变化引起的呢？

When the result of a `git bisect` is a non-merge commit, you should normally be able to discover the problem by examining just that commit. Developers can make this easy by breaking their changes into small self-contained commits. That won’t help in the case above, however, because the problem isn’t obvious from examination of any single commit; instead, a global view of the development is required. To make matters worse, the change in semantics in the problematic function may be just one small part of the changes in the upper line of development.

​	当`git bisect`的结果是非合并提交时，您通常可以通过仅检查该提交来发现问题。开发者可以通过将其更改分解为小的自包含提交来使这个过程变得简单。然而，在上面的情况下，这对于发现问题是没有帮助的，因为任何单个提交的检查都无法发现问题；相反，需要全局视角来发现问题。更糟糕的是，上行开发中有关问题函数的语义变化可能只是上行开发中的一小部分更改。

On the other hand, if instead of merging at C you had rebased the history between Z to B on top of A, you would have gotten this linear history:

​	另一方面，如果在C处没有合并，而是在Z到B之间的历史rebase到A的顶部，您将得到这种线性历史：

```
    ---Z---o---X--...---o---A---o---o---Y*--...---o---B*--D*
```

Bisecting between Z and `D*` would hit a single culprit commit `Y*`, and understanding why `Y*` was broken would probably be easier.

​	在Z和`D*`之间进行二分查找将找到一个罪魁祸首提交`Y*`，并且弄清楚为什么`Y*`是有问题的可能更容易。

Partly for this reason, many experienced Git users, even when working on an otherwise merge-heavy project, keep the history linear by rebasing against the latest upstream version before publishing.

​	部分原因是基于此，许多有经验的Git用户即使在工作中存在合并较多的项目时，在发布之前也会将历史保持线性，通过rebase到最新的上游版本。

## 高级分支管理

### 获取单个分支

Instead of using [git-remote[1]](../1/git-remote), you can also choose just to update one branch at a time, and to store it locally under an arbitrary name:

​	除了使用[git-remote[1]](../1/git-remote)，您还可以选择一次只更新一个分支，并将其存储在本地以任意名称命名：

``` bash
$ git fetch origin todo:my-todo-work
```

The first argument, `origin`, just tells Git to fetch from the repository you originally cloned from. The second argument tells Git to fetch the branch named `todo` from the remote repository, and to store it locally under the name `refs/heads/my-todo-work`.

​	第一个参数`origin`表示告诉Git从最初克隆的仓库中获取。第二个参数告诉Git从远程仓库获取名为`todo`的分支，并将其存储在本地名称为`refs/heads/my-todo-work`的位置。

You can also fetch branches from other repositories; so

​	您还可以从其他仓库获取分支；因此：

``` bash
$ git fetch git://example.com/proj.git master:example-master
```

will create a new branch named `example-master` and store in it the branch named `master` from the repository at the given URL. If you already have a branch named example-master, it will attempt to [fast-forward](https://git-scm.com/docs/user-manual#fast-forwards) to the commit given by example.com’s master branch. In more detail:

​	将创建一个名为`example-master`的新分支，并将其存储在从给定URL的仓库中获取的`master`分支中。如果您已经有一个名为example-master的分支，它将尝试将其[快进（fast-forward）](https://git-scm.com/docs/user-manual#fast-forwards)到example.com的master分支给出的提交。更详细地说：

### git fetch和 fast-forwards

In the previous example, when updating an existing branch, `git fetch` checks to make sure that the most recent commit on the remote branch is a descendant of the most recent commit on your copy of the branch before updating your copy of the branch to point at the new commit. Git calls this process a [fast-forward](https://git-scm.com/docs/user-manual#fast-forwards).

​	在前面的示例中，当更新现有分支时，`git fetch`会检查确保远程分支上的最新提交是您的分支副本上最新提交的后代，然后将您的分支副本更新到指向新的提交。Git将此过程称为[快进（fast-forward）](https://git-scm.com/docs/user-manual#fast-forwards)。

A fast-forward looks something like this:

​	fast-forward看起来像这样：

```
 o--o--o--o <-- old head of the branch
           \
            o--o--o <-- new head of the branch
```

In some cases it is possible that the new head will **not** actually be a descendant of the old head. For example, the developer may have realized a serious mistake was made and decided to backtrack, resulting in a situation like:

​	在某些情况下，新HEAD实际上**不**是旧HEAD的后代。例如，开发者可能意识到存在严重错误，并决定回退，导致情况如下：

```
 o--o--o--o--a--b <-- old head of the branch
           \
            o--o--o <-- new head of the branch
```

In this case, `git fetch` will fail, and print out a warning.

​	在这种情况下，`git fetch`会失败并打印出警告。

In that case, you can still force Git to update to the new head, as described in the following section. However, note that in the situation above this may mean losing the commits labeled `a` and `b`, unless you’ve already created a reference of your own pointing to them.

​	在这种情况下，您仍然可以强制Git更新到新HEAD，如下一节所述。但是，请注意，在上面的情况下，这可能意味着丢失标记为`a`和`b`的提交，除非您已经创建了自己的引用指向它们。

### 强制git fetch进行non-fast-forward更新 

If git fetch fails because the new head of a branch is not a descendant of the old head, you may force the update with:

​	如果git fetch失败，因为分支的新HEAD不是旧HEAD的后代，您可以使用以下命令强制更新：

``` bash
$ git fetch git://example.com/proj.git +master:refs/remotes/example/master
```

Note the addition of the `+` sign. Alternatively, you can use the `-f` flag to force updates of all the fetched branches, as in:

​	注意添加了`+`号。或者，您可以使用`-f`标志强制更新所有获取的分支，如下所示：

``` bash
$ git fetch -f origin
```

Be aware that commits that the old version of example/master pointed at may be lost, as we saw in the previous section.

​	请注意，旧版本的example/master指向的提交可能会丢失，正如我们在前面的部分中所看到的。

### 配置远程跟踪分支

We saw above that `origin` is just a shortcut to refer to the repository that you originally cloned from. This information is stored in Git configuration variables, which you can see using [git-config[1]](../1/git-config):

​	我们之前看到`origin`只是一个指向最初克隆的仓库的快捷方式。这些信息存储在Git配置变量中，您可以使用[git-config[1]](../1/git-config)查看：

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

If there are other repositories that you also use frequently, you can create similar configuration options to save typing; for example,

​	如果还有其他您经常使用的仓库，您可以创建类似的配置选项以节省输入；例如：

``` bash
$ git remote add example git://example.com/proj.git
```

adds the following to `.git/config`:

将以下内容添加到`.git/config`：

```
[remote "example"]
	url = git://example.com/proj.git
	fetch = +refs/heads/*:refs/remotes/example/*
```

Also note that the above configuration can be performed by directly editing the file `.git/config` instead of using [git-remote[1]](../1/git-remote).

​	另请注意，上述配置也可以通过直接编辑文件`.git/config`来完成，而不是使用[git-remote[1]](../1/git-remote)。

After configuring the remote, the following three commands will do the same thing:

​	在配置远程仓库后，以下三个命令将执行相同的操作：

``` bash
$ git fetch git://example.com/proj.git +refs/heads/*:refs/remotes/example/*
$ git fetch example +refs/heads/*:refs/remotes/example/*
$ git fetch example
```

See [git-config[1]](../1/git-config) for more details on the configuration options mentioned above and [git-fetch[1]](../1/git-fetch) for more details on the refspec syntax.

​	有关上述配置选项的更多详细信息，请参阅[git-config[1]](../1/git-config)，有关refspec语法的更多详细信息，请参阅[git-fetch[1]](../1/git-fetch)。

## Git 概念

Git is built on a small number of simple but powerful ideas. While it is possible to get things done without understanding them, you will find Git much more intuitive if you do.

​	Git建立在少数简单而强大的概念之上。虽然您可以不了解它们就完成工作，但如果您了解它们，将会发现Git更加直观。

We start with the most important, the [object database](https://git-scm.com/docs/user-manual#def_object_database) and the [index](https://git-scm.com/docs/user-manual#def_index).

​	我们从最重要的[对象数据库](https://git-scm.com/docs/user-manual#def_object_database)和[index](https://git-scm.com/docs/user-manual#def_index)开始。

### 对象数据库

We already saw in [Understanding History: Commits](https://git-scm.com/docs/user-manual#understanding-commits) that all commits are stored under a 40-digit "object name". In fact, all the information needed to represent the history of a project is stored in objects with such names. In each case the name is calculated by taking the SHA-1 hash of the contents of the object. The SHA-1 hash is a cryptographic hash function. What that means to us is that it is impossible to find two different objects with the same name. This has a number of advantages; among others:

​	我们在[理解历史：提交](https://git-scm.com/docs/user-manual#understanding-commits)中已经看到所有提交都存储在一个40位数字的“对象名称”下。实际上，存储项目历史所需的所有信息都存储在具有这些名称的对象中。在每种情况下，名称是通过计算对象内容的SHA-1哈希值得到的。SHA-1哈希是一种加密哈希函数。对于我们来说，这意味着不可能找到两个具有相同名称的不同对象。这有许多优点；其中之一是：

- Git can quickly determine whether two objects are identical or not, just by comparing names.
- Since object names are computed the same way in every repository, the same content stored in two repositories will always be stored under the same name.
- Git can detect errors when it reads an object, by checking that the object’s name is still the SHA-1 hash of its contents.
- 通过比较名称，Git可以快速确定两个对象是否相同。
- 由于对象名称在每个仓库中的计算方式相同，因此存储在两个仓库中的相同内容始终将存储在相同的名称下。
- 通过检查对象的名称是否仍然是其内容的SHA-1哈希，Git可以在读取对象时检测错误。

(See [Object storage format](https://git-scm.com/docs/user-manual#object-details) for the details of the object formatting and SHA-1 calculation.)

（有关对象格式和SHA-1计算的详细信息，请参阅[对象存储格式](https://git-scm.com/docs/user-manual#object-details)。）

There are four different types of objects: "blob", "tree", "commit", and "tag".

​	有四种不同类型的对象：“blob”、“tree”、“commit”和“tag”。

- A ["blob" object](https://git-scm.com/docs/user-manual#def_blob_object) is used to store file data.
- ["blob"对象](https://git-scm.com/docs/user-manual#def_blob_object)用于存储文件数据。
- A ["tree" object](https://git-scm.com/docs/user-manual#def_tree_object) ties one or more "blob" objects into a directory structure. In addition, a tree object can refer to other tree objects, thus creating a directory hierarchy.
- ["tree"对象](https://git-scm.com/docs/user-manual#def_tree_object)将一个或多个“blob”对象绑定到目录结构中。此外，树对象可以引用其他树对象，从而创建目录层次结构。
- A ["commit" object](https://git-scm.com/docs/user-manual#def_commit_object) ties such directory hierarchies together into a [directed acyclic graph](https://git-scm.com/docs/user-manual#def_DAG) of revisions—each commit contains the object name of exactly one tree designating the directory hierarchy at the time of the commit. In addition, a commit refers to "parent" commit objects that describe the history of how we arrived at that directory hierarchy.
- ["commit"对象](https://git-scm.com/docs/user-manual#def_commit_object)将这些目录层次结构链接在一起，形成一个[有向无环图](https://git-scm.com/docs/user-manual#def_DAG)的修订版本——每个提交包含恰好一个树的对象名称，该树指定了提交时的目录层次结构。此外，提交还引用“父”提交对象，描述了如何到达该目录层次结构的历史。
- A ["tag" object](https://git-scm.com/docs/user-manual#def_tag_object) symbolically identifies and can be used to sign other objects. It contains the object name and type of another object, a symbolic name (of course!) and, optionally, a signature.
- ["tag"对象](https://git-scm.com/docs/user-manual#def_tag_object)符号标识并可用于签署其他对象。它包含另一个对象的对象名称和类型，一个符号名称（当然！）以及可选的签名。

The object types in some more detail:

​	更详细地介绍一下对象类型：

#### Commit 对象

The "commit" object links a physical state of a tree with a description of how we got there and why. Use the `--pretty=raw` option to [git-show[1]](../1/git-show) or [git-log[1]](../1/git-log) to examine your favorite commit:

​	"commit" 对象将一个树的物理状态与我们是如何到达该状态以及原因的描述相结合。使用`--pretty=raw`选项查看您喜欢的提交，例如[git-show[1]](../1/git-show)或[git-log[1]](../1/git-log)：

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

As you can see, a commit is defined by:

​	如您所见，提交由以下内容定义：

- a tree: The SHA-1 name of a tree object (as defined below), representing the contents of a directory at a certain point in time.
- 一个树：一个树对象的SHA-1名称（如下所定义），表示特定时间点上目录的内容。
- parent(s): The SHA-1 name(s) of some number of commits which represent the immediately previous step(s) in the history of the project. The example above has one parent; merge commits may have more than one. A commit with no parents is called a "root" commit, and represents the initial revision of a project. Each project must have at least one root. A project can also have multiple roots, though that isn’t common (or necessarily a good idea).
- 父项：表示项目历史中上一步的一个或多个提交的SHA-1名称。上面的示例有一个父项；合并提交可能有多个父项。没有父项的提交称为“根”提交，并表示项目的初始版本。每个项目必须至少有一个根。项目还可以有多个根，尽管这不常见（也不一定是一个好主意）。
- an author: The name of the person responsible for this change, together with its date.
- 作者：对该更改负责的人的姓名，以及其日期。
- a committer: The name of the person who actually created the commit, with the date it was done. This may be different from the author, for example, if the author was someone who wrote a patch and emailed it to the person who used it to create the commit.
- 提交者：实际创建提交的人的姓名，以及提交日期。这可能与作者不同，例如，如果作者是为提交撰写了补丁并通过电子邮件发送给实际创建提交的人。
- a comment describing this commit.
- 描述此提交的注释。

Note that a commit does not itself contain any information about what actually changed; all changes are calculated by comparing the contents of the tree referred to by this commit with the trees associated with its parents. In particular, Git does not attempt to record file renames explicitly, though it can identify cases where the existence of the same file data at changing paths suggests a rename. (See, for example, the `-M` option to [git-diff[1]](../1/git-diff)).

​	请注意，提交本身不包含有关实际更改的任何信息；所有更改是通过比较由此提交引用的树的内容与其父项关联的树来计算的。特别地，Git不尝试显式记录文件重命名，尽管它可以识别存在相同文件数据在不同路径上发生更改的情况，这暗示了重命名（请参阅[git-diff[1]](../1/git-diff)中的`-M`选项）。提交通常由[git-commit[1]](../1/git-commit)创建，它创建一个提交，其父项通常是当前的HEAD，其树来自当前存储在索引中的内容。

A commit is usually created by [git-commit[1]](../1/git-commit), which creates a commit whose parent is normally the current HEAD, and whose tree is taken from the content currently stored in the index.

​	一般情况下，使用[git-commit[1]](../1/git-commit)创建提交。该命令创建一个提交，其父提交通常是当前的HEAD，而树是从当前索引中的内容中获取的。

#### Tree 对象

The ever-versatile [git-show[1]](../1/git-show) command can also be used to examine tree objects, but [git-ls-tree[1]](../1/git-ls-tree) will give you more details:

​	多功能的[git-show[1]](../1/git-show)命令也可用于查看树对象，但[git-ls-tree[1]](../1/git-ls-tree)将提供更多详细信息：

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

As you can see, a tree object contains a list of entries, each with a mode, object type, SHA-1 name, and name, sorted by name. It represents the contents of a single directory tree.

​	正如您所见，树对象包含一个条目列表，每个条目都有一个模式、对象类型、SHA-1名称和名称，并按名称排序。它表示单个目录树的内容。

The object type may be a blob, representing the contents of a file, or another tree, representing the contents of a subdirectory. Since trees and blobs, like all other objects, are named by the SHA-1 hash of their contents, two trees have the same SHA-1 name if and only if their contents (including, recursively, the contents of all subdirectories) are identical. This allows Git to quickly determine the differences between two related tree objects, since it can ignore any entries with identical object names.

​	对象类型可以是blob，表示文件的内容，也可以是另一个树，表示子目录的内容。由于树和blob，像所有其他对象一样，是通过它们的内容的SHA-1哈希命名的，因此当且仅当它们的内容（包括递归地包含所有子目录的内容）相同时，两个树具有相同的SHA-1名称。这使得Git可以快速确定两个相关树对象之间的差异，因为它可以忽略具有相同对象名称的任何条目。

(Note: in the presence of submodules, trees may also have commits as entries. See [Submodules](https://git-scm.com/docs/user-manual#submodules) for documentation.)

（注意：在存在子模块的情况下，树也可以具有提交作为条目。请参阅[子模块](https://git-scm.com/docs/user-manual#submodules)的文档。）

Note that the files all have mode 644 or 755: Git actually only pays attention to the executable bit.

​	请注意，所有文件的模式都是644或755：Git实际上只关注可执行位。

#### Blob 对象

You can use [git-show[1]](../1/git-show) to examine the contents of a blob; take, for example, the blob in the entry for `COPYING` from the tree above:

​	您可以使用[git-show[1]](../1/git-show)查看blob的内容；例如，从上面的树中`COPYING`的条目中获取的blob如下：

``` bash
$ git show 6ff87c4664

 Note that the only valid version of the GPL as far as this project
 is concerned is _this_ particular version of the license (ie v2, not
 v2.2 or v3.x or whatever), unless explicitly otherwise stated.
...
```

A "blob" object is nothing but a binary blob of data. It doesn’t refer to anything else or have attributes of any kind.

​	"blob"对象只是一种二进制数据块。它不引用其他任何内容，也没有任何属性。

Since the blob is entirely defined by its data, if two files in a directory tree (or in multiple different versions of the repository) have the same contents, they will share the same blob object. The object is totally independent of its location in the directory tree, and renaming a file does not change the object that file is associated with.

​	由于blob完全由其数据定义，如果目录树中的两个文件（或仓库的多个不同版本中的文件）具有相同的内容，则它们将共享相同的blob对象。对象完全独立于其在目录树中的位置，重命名文件不会更改与该文件关联的对象。

Note that any tree or blob object can be examined using [git-show[1]](../1/git-show) with the `<revision>:<path>` syntax. This can sometimes be useful for browsing the contents of a tree that is not currently checked out.

​	请注意，可以使用[git-show[1]](../1/git-show)命令的`<revision>:<path>`语法来查看任何树或blob对象。这有时对于浏览当前未检出的树的内容很有用。

#### Trust

If you receive the SHA-1 name of a blob from one source, and its contents from another (possibly untrusted) source, you can still trust that those contents are correct as long as the SHA-1 name agrees. This is because the SHA-1 is designed so that it is infeasible to find different contents that produce the same hash.

​	如果您从一个来源接收到blob的SHA-1名称，从另一个（可能不受信任的）来源接收到其内容，只要SHA-1名称一致，您仍然可以相信这些内容是正确的。这是因为SHA-1被设计成不可能找到产生相同哈希的不同内容。

Similarly, you need only trust the SHA-1 name of a top-level tree object to trust the contents of the entire directory that it refers to, and if you receive the SHA-1 name of a commit from a trusted source, then you can easily verify the entire history of commits reachable through parents of that commit, and all of those contents of the trees referred to by those commits.

​	同样，您只需要相信顶级树对象的SHA-1名称，就可以信任它引用的整个目录的内容。如果您从可信任的来源接收到提交的SHA-1名称，那么您可以轻松验证通过该提交的父项可达的所有提交的整个历史记录，以及这些提交引用的所有树的内容。

So to introduce some real trust in the system, the only thing you need to do is to digitally sign just *one* special note, which includes the name of a top-level commit. Your digital signature shows others that you trust that commit, and the immutability of the history of commits tells others that they can trust the whole history.

​	因此，要在系统中引入一些真正的信任，您只需要对*一个*特殊备注进行数字签名，其中包含一个顶级提交的名称。您的数字签名向他人表明您信任该提交，而提交历史记录的不可变性告诉他人他们可以信任整个历史记录。

In other words, you can easily validate a whole archive by just sending out a single email that tells the people the name (SHA-1 hash) of the top commit, and digitally sign that email using something like GPG/PGP.

​	换句话说，您可以通过只发送一封电子邮件来轻松验证整个存档，告诉人们顶级提交的名称（SHA-1哈希），并使用类似GPG/PGP的方法对该电子邮件进行数字签名。

To assist in this, Git also provides the tag object…

​	为了在这方面提供帮助，Git还提供了标签对象...

#### Tag 对象

A tag object contains an object, object type, tag name, the name of the person ("tagger") who created the tag, and a message, which may contain a signature, as can be seen using [git-cat-file[1]](../1/git-cat-file):

​	标签对象包含一个对象、对象类型、标签名称、创建标签的人（“tagger”）的姓名和一条消息，该消息可能包含签名，如[git-cat-file[1]](../1/git-cat-file)所示：

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

See the [git-tag[1]](../1/git-tag) command to learn how to create and verify tag objects. (Note that [git-tag[1]](../1/git-tag) can also be used to create "lightweight tags", which are not tag objects at all, but just simple references whose names begin with `refs/tags/`).

​	有关如何创建和验证标签对象的详细信息，请参阅[git-tag[1]](../1/git-tag)命令。（请注意，[git-tag[1]](../1/git-tag)还可用于创建“轻量级标签”，这根本不是标签对象，而只是以`refs/tags/`开头的简单引用。）

#### Git如何高效地存储对象：pack文件

Newly created objects are initially created in a file named after the object’s SHA-1 hash (stored in `.git/objects`).

​	新创建的对象最初存储在一个以对象的SHA-1哈希命名的文件中（存储在`.git/objects`中）。

Unfortunately this system becomes inefficient once a project has a lot of objects. Try this on an old project:

​	不幸的是，一旦项目拥有大量对象，该系统变得效率低下。尝试在一个旧项目上执行以下操作：

``` bash
$ git count-objects
6930 objects, 47620 kilobytes
```

The first number is the number of objects which are kept in individual files. The second is the amount of space taken up by those "loose" objects.

​	第一个数字是保留在单独文件中的对象数量。第二个数字是这些“松散”对象占用的空间。

You can save space and make Git faster by moving these loose objects in to a "pack file", which stores a group of objects in an efficient compressed format; the details of how pack files are formatted can be found in [gitformat-pack[5]](../5/gitformat-pack).

​	通过将这些松散对象移入“pack文件”可以节省空间并使Git更快。pack文件以高效压缩的格式存储一组对象；有关pack文件格式的详细信息，请参阅[gitformat-pack[5]](../5/gitformat-pack)。

To put the loose objects into a pack, just run git repack:

​	要将松散对象放入pack文件中，只需运行git repack：

``` bash
$ git repack
Counting objects: 6020, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (6020/6020), done.
Writing objects: 100% (6020/6020), done.
Total 6020 (delta 4070), reused 0 (delta 0)
```

This creates a single "pack file" in .git/objects/pack/ containing all currently unpacked objects. You can then run

​	这将在`.git/objects/pack/`中创建一个单独的“pack文件”，其中包含所有当前未打包的对象。然后，您可以运行

``` bash
$ git prune
```

to remove any of the "loose" objects that are now contained in the pack. This will also remove any unreferenced objects (which may be created when, for example, you use `git reset` to remove a commit). You can verify that the loose objects are gone by looking at the `.git/objects` directory or by running

来删除任何现在包含在pack文件中的“松散”对象。这也将删除任何未引用的对象（例如，当您使用`git reset`删除提交时可能会创建未引用的对象）。您可以通过查看`.git/objects`目录或运行

``` bash
$ git count-objects
0 objects, 0 kilobytes
```

来验证松散对象是否已删除。

Although the object files are gone, any commands that refer to those objects will work exactly as they did before.

​	尽管对象文件已经不存在，但是引用这些对象的任何命令将与之前完全相同。

The [git-gc[1]](../1/git-gc) command performs packing, pruning, and more for you, so is normally the only high-level command you need.

​	[git-gc[1]](../1/git-gc)命令可以为您执行打包、修剪等操作，因此通常是您唯一需要的高级命令。

#### 悬空对象 Dangling objects

The [git-fsck[1]](../1/git-fsck) command will sometimes complain about dangling objects. They are not a problem.

​	[git-fsck[1]](../1/git-fsck)命令有时会报告悬空对象的问题。但这并不是问题。

The most common cause of dangling objects is that you’ve rebased a branch, or you have pulled from somebody else who rebased a branch—see [Rewriting history and maintaining patch series](https://git-scm.com/docs/user-manual#cleaning-up-history). In that case, the old head of the original branch still exists, as does everything it pointed to. The branch pointer itself just doesn’t, since you replaced it with another one.

​	悬空对象最常见的原因是你已经对一个分支进行了变基，或者你从别人那里拉取了一个已经进行了变基的分支，参见[重写历史和维护补丁系列](https://git-scm.com/docs/user-manual#cleaning-up-history)。在这种情况下，原始分支的旧HEAD仍然存在，以及它所指向的一切。只是分支指针本身不再存在，因为你用另一个替代它了。

There are also other situations that cause dangling objects. For example, a "dangling blob" may arise because you did a `git add` of a file, but then, before you actually committed it and made it part of the bigger picture, you changed something else in that file and committed that **updated** thing—the old state that you added originally ends up not being pointed to by any commit or tree, so it’s now a dangling blob object.

​	还有其他导致悬空对象的情况。例如，"悬空blob"可能是因为你在执行`git add`命令添加文件后，然后在实际提交并将其作为大版本的一部分之前，又对该文件进行了其他更改并提交了**更新后**的版本——最初添加的旧状态不再被任何提交或树引用，所以它现在是一个悬空的blob对象。

Similarly, when the "ort" merge strategy runs, and finds that there are criss-cross merges and thus more than one merge base (which is fairly unusual, but it does happen), it will generate one temporary midway tree (or possibly even more, if you had lots of criss-crossing merges and more than two merge bases) as a temporary internal merge base, and again, those are real objects, but the end result will not end up pointing to them, so they end up "dangling" in your repository.

​	类似地，当运行"ort"合并策略并发现存在交叉合并，从而有多个合并基（这相当不寻常，但确实会发生），它会生成一个临时的中间树（或者甚至更多，如果您有许多交叉合并和多个合并基）作为临时内部合并基，同样，它们是真实的对象，但最终结果将不会指向它们，所以它们最终"悬空"在你的仓库中。

Generally, dangling objects aren’t anything to worry about. They can even be very useful: if you screw something up, the dangling objects can be how you recover your old tree (say, you did a rebase, and realized that you really didn’t want to—you can look at what dangling objects you have, and decide to reset your head to some old dangling state).

​	通常情况下，悬空对象无需担心。它们甚至可能非常有用：如果出现问题，悬空对象可能是恢复旧树的方式（例如，你进行了变基，并意识到你实际上并不想变基——你可以查看你拥有的悬空对象，并决定将HEAD重置为某个旧的悬空状态）。

For commits, you can just use:

​	对于提交，您可以使用：

``` bash
$ gitk <dangling-commit-sha-goes-here> --not --all
```

This asks for all the history reachable from the given commit but not from any branch, tag, or other reference. If you decide it’s something you want, you can always create a new reference to it, e.g.,

​	这将要求显示从给定提交可达的所有历史记录，但不包括任何分支、标签或其他引用。如果您决定需要该提交，您始终可以创建一个新的引用，例如：

``` bash
$ git branch recovered-branch <dangling-commit-sha-goes-here>
```

For blobs and trees, you can’t do the same, but you can still examine them. You can just do

​	对于blob和树，您不能执行相同的操作，但您仍然可以查看它们。您可以使用以下命令：

``` bash
$ git show <dangling-blob/tree-sha-goes-here>
```

to show what the contents of the blob were (or, for a tree, basically what the `ls` for that directory was), and that may give you some idea of what the operation was that left that dangling object.

来显示blob的内容（或者对于树来说，基本上是该目录的`ls`内容），这可能会让您对导致该悬空对象出现的操作有一些了解。

Usually, dangling blobs and trees aren’t very interesting. They’re almost always the result of either being a half-way mergebase (the blob will often even have the conflict markers from a merge in it, if you have had conflicting merges that you fixed up by hand), or simply because you interrupted a `git fetch` with ^C or something like that, leaving *some* of the new objects in the object database, but just dangling and useless.

​	通常情况下，悬空blob和树并不是非常有趣。它们几乎总是由于半路合并基（如果有冲突的合并，blob中通常会包含冲突标记，如果您手动解决了冲突的合并），或者只是因为您中断了`git fetch`，使用^C或类似方法，导致对象数据库中有*一些*新对象，但它们只是悬空的，没有用。

Anyway, once you are sure that you’re not interested in any dangling state, you can just prune all unreachable objects:

​	无论如何，一旦确信您对任何悬空状态不感兴趣，您可以通过修剪所有不可达的对象来删除它们：

``` bash
$ git prune
```

and they’ll be gone. (You should only run `git prune` on a quiescent repository—it’s kind of like doing a filesystem fsck recovery: you don’t want to do that while the filesystem is mounted. `git prune` is designed not to cause any harm in such cases of concurrent accesses to a repository but you might receive confusing or scary messages.)

它们将被清除。（在静态的仓库上运行`git prune`，就像进行文件系统的fsck恢复：您不希望在文件系统挂载时执行该操作。`git prune`的设计是不会在这些并发访问仓库的情况下造成任何损害，但您可能会收到混淆或令人恐慌的消息。）

#### 从仓库损坏中恢复

By design, Git treats data trusted to it with caution. However, even in the absence of bugs in Git itself, it is still possible that hardware or operating system errors could corrupt data.

​	根据设计，Git对其信任的数据持谨慎态度。然而，即使在Git本身没有错误的情况下，硬件或操作系统错误仍可能导致数据损坏。

The first defense against such problems is backups. You can back up a Git directory using clone, or just using cp, tar, or any other backup mechanism.

​	防止此类问题的首要措施是备份。您可以使用克隆（clone）进行Git目录的备份，或者只使用cp、tar或任何其他备份机制。

As a last resort, you can search for the corrupted objects and attempt to replace them by hand. Back up your repository before attempting this in case you corrupt things even more in the process.

​	作为最后的手段，您可以搜索损坏的对象，并尝试手动替换它们。在尝试之前，请备份您的仓库，以防在过程中进一步破坏。

We’ll assume that the problem is a single missing or corrupted blob, which is sometimes a solvable problem. (Recovering missing trees and especially commits is **much** harder).

​	我们假设问题是单个丢失或损坏的blob，这有时是一个可以解决的问题（恢复丢失的树，尤其是提交，**要困难得多**）。

Before starting, verify that there is corruption, and figure out where it is with [git-fsck[1]](../1/git-fsck); this may be time-consuming.

​	在开始之前，使用[git-fsck[1]](../1/git-fsck)验证是否存在损坏，并确定损坏的位置；这可能会耗费一些时间。

Assume the output looks like this:

​	假设输出看起来像这样：

``` bash
$ git fsck --full --no-dangling
broken link from    tree 2d9263c6d23595e7cb2a21e5ebbb53655278dff8
              to    blob 4b9458b3786228369c63936db65827de3cc06200
missing blob 4b9458b3786228369c63936db65827de3cc06200
```

Now you know that blob 4b9458b3 is missing, and that the tree 2d9263c6 points to it. If you could find just one copy of that missing blob object, possibly in some other repository, you could move it into `.git/objects/4b/9458b3...` and be done. Suppose you can’t. You can still examine the tree that pointed to it with [git-ls-tree[1]](../1/git-ls-tree), which might output something like:

​	现在您知道blob 4b9458b3丢失了，并且树2d9263c6指向它。如果您能在其他仓库中找到该丢失的blob对象的一个副本，您可以将它移动到`.git/objects/4b/9458b3...`并完成。假设您找不到。您仍然可以使用[git-ls-tree[1]](../1/git-ls-tree)检查指向它的树，它可能输出类似于：

``` bash
$ git ls-tree 2d9263c6d23595e7cb2a21e5ebbb53655278dff8
100644 blob 8d14531846b95bfa3564b58ccfb7913a034323b8	.gitignore
100644 blob ebf9bf84da0aab5ed944264a5db2a65fe3a3e883	.mailmap
100644 blob ca442d313d86dc67e0a2e5d584b465bd382cbf5c	COPYING
...
100644 blob 4b9458b3786228369c63936db65827de3cc06200	myfile
...
```

So now you know that the missing blob was the data for a file named `myfile`. And chances are you can also identify the directory—let’s say it’s in `somedirectory`. If you’re lucky the missing copy might be the same as the copy you have checked out in your working tree at `somedirectory/myfile`; you can test whether that’s right with [git-hash-object[1]](../1/git-hash-object):

​	现在您知道缺失的blob是名为`myfile`的文件的数据。很有可能您也可以识别出目录——假设它在`somedirectory`中。如果您很幸运，丢失的副本可能与您的工作树中的`somedirectory/myfile`处检出的副本相同；您可以使用[git-hash-object[1]](../1/git-hash-object)来测试是否正确：

``` bash
$ git hash-object -w somedirectory/myfile
```

which will create and store a blob object with the contents of somedirectory/myfile, and output the SHA-1 of that object. if you’re extremely lucky it might be 4b9458b3786228369c63936db65827de3cc06200, in which case you’ve guessed right, and the corruption is fixed!

这将创建并存储一个带有`somedirectory/myfile`内容的blob对象，并输出该对象的SHA-1。如果非常幸运，它可能是4b9458b3786228369c63936db65827de3cc06200，这意味着您猜对了，损坏已经修复！

Otherwise, you need more information. How do you tell which version of the file has been lost?

​	否则，您需要更多信息。您如何确定已丢失的文件版本？

The easiest way to do this is with:

​	最简单的方法是使用：

``` bash
$ git log --raw --all --full-history -- somedirectory/myfile
```

Because you’re asking for raw output, you’ll now get something like

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

This tells you that the immediately following version of the file was "newsha", and that the immediately preceding version was "oldsha". You also know the commit messages that went with the change from oldsha to 4b9458b and with the change from 4b9458b to newsha.

​	这告诉您，随后的版本是"newsha"，之前的版本是"oldsha"。您还知道与从oldsha到4b9458b的更改以及从4b9458b到newsha的更改有关的提交消息。

If you’ve been committing small enough changes, you may now have a good shot at reconstructing the contents of the in-between state 4b9458b.

​	如果您一直在提交足够小的更改，现在您可能有很大机会重建中间状态4b9458b的内容。

If you can do that, you can now recreate the missing object with

​	如果您可以做到这一点，现在可以使用以下命令重新创建丢失的对象：

``` bash
$ git hash-object -w <recreated-file>
```

and your repository is good again!

您的仓库现在恢复正常！

(Btw, you could have ignored the `fsck`, and started with doing a

（顺便说一下，您可以忽略`fsck`，直接执行：

``` bash
$ git log --raw --all
```

and just looked for the sha of the missing object (4b9458b) in that whole thing. It’s up to you—Git does **have** a lot of information, it is just missing one particular blob version.

然后在整个输出中查找丢失对象（4b9458b）的sha。这取决于您——Git确实**有**大量信息，只是缺少一个特定的blob版本。）

### 索引

The index is a binary file (generally kept in `.git/index`) containing a sorted list of path names, each with permissions and the SHA-1 of a blob object; [git-ls-files[1]](../1/git-ls-files) can show you the contents of the index:

​	索引是一个二进制文件（通常保存在`.git/index`），其中包含一个经过排序的路径名列表，每个路径名都带有权限和对应的blob对象的SHA-1值；[git-ls-files[1]](../1/git-ls-files)可以显示索引的内容：

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

Note that in older documentation you may see the index called the "current directory cache" or just the "cache". It has three important properties:

​	请注意，在旧的文档中，您可能会看到索引被称为"当前目录缓存"或者只是"缓存"。它有三个重要的属性：

1. The index contains all the information necessary to generate a single (uniquely determined) tree object.

2. 索引包含生成单个（唯一确定的）树对象所需的所有信息。

   For example, running [git-commit[1]](../1/git-commit) generates this tree object from the index, stores it in the object database, and uses it as the tree object associated with the new commit.

   例如，运行[git-commit[1]](../1/git-commit)会根据索引生成这个树对象，将其存储在对象数据库中，并将其用作与新提交相关联的树对象。

3. The index enables fast comparisons between the tree object it defines and the working tree.

4. 索引允许快速比较它定义的树对象和工作树之间的差异。

   It does this by storing some additional data for each entry (such as the last modified time). This data is not displayed above, and is not stored in the created tree object, but it can be used to determine quickly which files in the working directory differ from what was stored in the index, and thus save Git from having to read all of the data from such files to look for changes.

   它通过为每个条目存储一些附加数据（例如上次修改时间）来实现。这些数据不在上面的输出中显示，并且不会存储在创建的树对象中，但可以用于快速确定工作目录中的哪些文件与索引中存储的内容不同，从而使Git无需读取这些文件的所有数据来查找更改。

5. It can efficiently represent information about merge conflicts between different tree objects, allowing each pathname to be associated with sufficient information about the trees involved that you can create a three-way merge between them.

6. 它可以高效地表示不同树对象之间的合并冲突信息，使得每个路径名都与涉及的树有足够的信息关联，以便您可以在它们之间创建三方合并。

   We saw in [Getting conflict-resolution help during a merge](https://git-scm.com/docs/user-manual#conflict-resolution) that during a merge the index can store multiple versions of a single file (called "stages"). The third column in the [git-ls-files[1]](../1/git-ls-files) output above is the stage number, and will take on values other than 0 for files with merge conflicts.

   我们在[在合并过程中获取冲突解决帮助](https://git-scm.com/docs/user-manual#conflict-resolution)中看到，在合并过程中，索引可以存储单个文件的多个版本（称为"stage"）。上面[git-ls-files[1]](../1/git-ls-files)输出中的第三列是stage编号，对于存在合并冲突的文件，其值将为0以外的值。

The index is thus a sort of temporary staging area, which is filled with a tree which you are in the process of working on.

​	因此，索引是一种临时的暂存区，其中填充着您正在处理的树。

If you blow the index away entirely, you generally haven’t lost any information as long as you have the name of the tree that it described.

​	如果完全删除索引，通常不会丢失任何信息，只要您知道它所描述的树的名称。

## 子模块（Submodules）

Large projects are often composed of smaller, self-contained modules. For example, an embedded Linux distribution’s source tree would include every piece of software in the distribution with some local modifications; a movie player might need to build against a specific, known-working version of a decompression library; several independent programs might all share the same build scripts.

​	大型项目通常由较小、独立的模块组成。例如，嵌入式Linux发行版的源代码树可能包含发行版中的每个软件部件以及一些本地修改；一个视频播放器可能需要构建并针对特定的、已经测试过的解压库版本进行编译；几个独立的程序可能共享相同的构建脚本。

With centralized revision control systems this is often accomplished by including every module in one single repository. Developers can check out all modules or only the modules they need to work with. They can even modify files across several modules in a single commit while moving things around or updating APIs and translations.

​	在集中式版本控制系统中，通常通过将每个模块包含在一个单一的仓库中来实现这一点。开发者可以检出所有模块，或者只检出需要处理的模块。他们甚至可以在单个提交中跨多个模块修改文件，同时移动文件或更新API和翻译。

Git does not allow partial checkouts, so duplicating this approach in Git would force developers to keep a local copy of modules they are not interested in touching. Commits in an enormous checkout would be slower than you’d expect as Git would have to scan every directory for changes. If modules have a lot of local history, clones would take forever.

​	然而，Git不允许部分检出，所以在Git中复制这种方法会迫使开发者保留他们不打算操作的模块的本地副本。大型检出中的提交速度会比预期慢，因为Git需要扫描每个目录以查找更改。如果模块具有大量的本地历史，克隆操作将需要很长时间。

On the plus side, distributed revision control systems can much better integrate with external sources. In a centralized model, a single arbitrary snapshot of the external project is exported from its own revision control and then imported into the local revision control on a vendor branch. All the history is hidden. With distributed revision control you can clone the entire external history and much more easily follow development and re-merge local changes.

​	幸运的是，分布式版本控制系统可以更好地与外部资源集成。在集中式模型中，外部项目的一个任意快照从其自己的版本控制中导出，然后导入到供应商分支的本地版本控制中。所有的历史都被隐藏了。使用分布式版本控制，您可以克隆整个外部历史，并更容易跟踪开发和重新合并本地更改。

Git’s submodule support allows a repository to contain, as a subdirectory, a checkout of an external project. Submodules maintain their own identity; the submodule support just stores the submodule repository location and commit ID, so other developers who clone the containing project ("superproject") can easily clone all the submodules at the same revision. Partial checkouts of the superproject are possible: you can tell Git to clone none, some or all of the submodules.

​	Git的子模块支持允许一个仓库包含外部项目的检出，作为子目录。子模块保持它们自己的身份；子模块支持只是存储子模块仓库位置和提交ID，因此克隆包含子模块的父项目（"superproject"）的其他开发者可以轻松地克隆所有子模块，并保持相同的提交。父项目的部分检出也是可能的：您可以告诉Git克隆一些或所有的子模块。

The [git-submodule[1]](../1/git-submodule) command is available since Git 1.5.3. Users with Git 1.5.2 can look up the submodule commits in the repository and manually check them out; earlier versions won’t recognize the submodules at all.

​	[git-submodule[1]](../1/git-submodule)命令自Git 1.5.3版本起可用。使用Git 1.5.2版本的用户可以在仓库中查找子模块提交并手动检出它们；更早版本不会识别子模块。

To see how submodule support works, create four example repositories that can be used later as a submodule:

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

Now create the superproject and add all the submodules:

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

See what files `git submodule` created:

​	查看`git submodule`创建的文件：

``` bash
$ ls -a
.  ..  .git  .gitmodules  a  b  c  d
```

The `git submodule add <repo> <path>` command does a couple of things:

​	`git submodule add <repo> <path>`命令执行了一些操作：

- It clones the submodule from `<repo>` to the given `<path>` under the current directory and by default checks out the master branch.
- 它从`<repo>`克隆子模块到当前目录下给定的`<path>`，默认情况下会检出主分支。
- It adds the submodule’s clone path to the [gitmodules[5]](../5/gitmodules) file and adds this file to the index, ready to be committed.
- 它将子模块的克隆路径添加到[gitmodules[5]](../5/gitmodules)文件，并将此文件添加到索引中，准备提交。
- It adds the submodule’s current commit ID to the index, ready to be committed.
- 它将子模块的当前提交ID添加到索引中，准备提交。

Commit the superproject:

​	提交父项目：

``` bash
$ git commit -m "Add submodules a, b, c and d."
```

Now clone the superproject:

​	现在克隆父项目：

``` bash
$ cd ..
$ git clone super cloned
$ cd cloned
```

The submodule directories are there, but they’re empty:

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

Pulling down the submodules is a two-step process. First run `git submodule init` to add the submodule repository URLs to `.git/config`:

​	拉取子模块是一个两步的过程。首先运行 `git submodule init` 将子模块的仓库URL添加到 `.git/config` 文件中：

``` bash
$ git submodule init
```

Now use `git submodule update` to clone the repositories and check out the commits specified in the superproject:

​	然后使用 `git submodule update` 克隆仓库并检出超级项目中指定的提交：

``` bash
$ git submodule update
$ cd a
$ ls -a
.  ..  .git  a.txt
```

One major difference between `git submodule update` and `git submodule add` is that `git submodule update` checks out a specific commit, rather than the tip of a branch. It’s like checking out a tag: the head is detached, so you’re not working on a branch.

​	`git submodule update` 和 `git submodule add` 的一个主要区别是，`git submodule update` 检出一个特定的提交，而不是分支的最新提交。这类似于检出一个标签：HEAD处于分离状态，因此你不在分支上工作。

``` bash
$ git branch
* (detached from d266b98)
  master
```

If you want to make a change within a submodule and you have a detached head, then you should create or checkout a branch, make your changes, publish the change within the submodule, and then update the superproject to reference the new commit:

​	如果您想在子模块中进行更改，并且HEAD处于分离状态，则应该创建或检出一个分支，进行更改，将更改发布到子模块中，然后更新超级项目以引用新的提交：

``` bash
$ git switch master
```

or

或者

``` bash
$ git switch -c fix-up
```

then

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

You have to run `git submodule update` after `git pull` if you want to update submodules, too.

​	如果您想要更新子模块，那么在执行 `git pull` 之后，必须运行 `git submodule update`。

### 子模块的陷阱 Pitfalls with submodules

Always publish the submodule change before publishing the change to the superproject that references it. If you forget to publish the submodule change, others won’t be able to clone the repository:

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

In older Git versions it could be easily forgotten to commit new or modified files in a submodule, which silently leads to similar problems as not pushing the submodule changes. Starting with Git 1.7.0 both `git status` and `git diff` in the superproject show submodules as modified when they contain new or modified files to protect against accidentally committing such a state. `git diff` will also add a `-dirty` to the work tree side when generating patch output or used with the `--submodule` option:

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

You also should not rewind branches in a submodule beyond commits that were ever recorded in any superproject.

​	您还不应将子模块倒回超级项目中的任何早期提交之前的分支。

It’s not safe to run `git submodule update` if you’ve made and committed changes within a submodule without checking out a branch first. They will be silently overwritten:

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

If you have uncommitted changes in your submodule working tree, `git submodule update` will not overwrite them. Instead, you get the usual warning about not being able switch from a dirty branch.

​	如果在子模块工作树中有未提交的更改，`git submodule update` 将不会覆盖它们。而是会显示有关无法从脏分支切换的常规警告。

## Git的低级操作

Many of the higher-level commands were originally implemented as shell scripts using a smaller core of low-level Git commands. These can still be useful when doing unusual things with Git, or just as a way to understand its inner workings.

​	许多高级命令最初是使用较小的一组低级Git命令实现的Shell脚本。当需要对Git进行非常规操作，或者只是想了解其内部工作原理时，这些低级命令仍然非常有用。

### 对象访问和操作

The [git-cat-file[1]](../1/git-cat-file) command can show the contents of any object, though the higher-level [git-show[1]](../1/git-show) is usually more useful.

​	[git-cat-file[1]](../1/git-cat-file) 命令可以显示任何对象的内容，尽管高级命令 [git-show[1]](../1/git-show) 通常更有用。

The [git-commit-tree[1]](../1/git-commit-tree) command allows constructing commits with arbitrary parents and trees.

​	[git-commit-tree[1]](../1/git-commit-tree) 命令允许构建具有任意父对象和树对象的提交。

A tree can be created with [git-write-tree[1]](../1/git-write-tree) and its data can be accessed by [git-ls-tree[1]](../1/git-ls-tree). Two trees can be compared with [git-diff-tree[1]](../1/git-diff-tree).

​	可以使用 [git-write-tree[1]](../1/git-write-tree) 创建树对象，并且可以通过 [git-ls-tree[1]](../1/git-ls-tree) 访问其数据。两个树对象可以使用 [git-diff-tree[1]](../1/git-diff-tree) 进行比较。

A tag is created with [git-mktag[1]](../1/git-mktag), and the signature can be verified by [git-verify-tag[1]](../1/git-verify-tag), though it is normally simpler to use [git-tag[1]](../1/git-tag) for both.

​	可以使用 [git-mktag[1]](../1/git-mktag) 创建标签，并且签名可以使用 [git-verify-tag[1]](../1/git-verify-tag) 进行验证，尽管通常使用 [git-tag[1]](../1/git-tag) 更简单。

### 工作流程

High-level operations such as [git-commit[1]](../1/git-commit) and [git-restore[1]](../1/git-restore) work by moving data between the working tree, the index, and the object database. Git provides low-level operations which perform each of these steps individually.

​	高级操作，如 [git-commit[1]](../1/git-commit) 和 [git-restore[1]](../1/git-restore) 通过在工作树、索引和对象数据库之间移动数据来工作。Git提供了执行这些步骤的低级操作。

Generally, all Git operations work on the index file. Some operations work **purely** on the index file (showing the current state of the index), but most operations move data between the index file and either the database or the working directory. Thus there are four main combinations:

​	通常，所有的Git操作都是在索引文件上进行的。一些操作仅在索引文件上工作（显示索引的当前状态），但大多数操作在索引文件与数据库或工作目录之间移动数据。因此，有四种主要的组合：

#### 工作目录 → 索引

The [git-update-index[1]](../1/git-update-index) command updates the index with information from the working directory. You generally update the index information by just specifying the filename you want to update, like so:

​	[git-update-index[1]](../1/git-update-index) 命令将工作目录中的信息更新到索引中。您通常通过指定要更新的文件名来更新索引信息，例如：

``` bash
$ git update-index filename
```

but to avoid common mistakes with filename globbing etc., the command will not normally add totally new entries or remove old entries, i.e. it will normally just update existing cache entries.

但是为了避免文件名通配符等常见错误，该命令通常不会完全添加全新的条目或删除旧的条目，即通常只会更新现有的缓存条目。

To tell Git that yes, you really do realize that certain files no longer exist, or that new files should be added, you should use the `--remove` and `--add` flags respectively.

​	要告诉Git是的，您确实意识到某些文件不再存在，或者应该添加新文件，请分别使用 `--remove` 和 `--add` 标志。

NOTE! A `--remove` flag does *not* mean that subsequent filenames will necessarily be removed: if the files still exist in your directory structure, the index will be updated with their new status, not removed. The only thing `--remove` means is that update-index will be considering a removed file to be a valid thing, and if the file really does not exist any more, it will update the index accordingly.

​	注意！`--remove` 标志并不意味着随后的文件名一定会被删除：如果文件在目录结构中仍然存在，则索引将根据其新状态更新，而不会删除它们。`--remove` 的唯一含义是 `update-index` 将考虑已删除的文件为有效的内容，并且如果该文件实际上不再存在，则会相应地更新索引。

As a special case, you can also do `git update-index --refresh`, which will refresh the "stat" information of each index to match the current stat information. It will *not* update the object status itself, and it will only update the fields that are used to quickly test whether an object still matches its old backing store object.

​	作为特例，您还可以执行 `git update-index --refresh`，它将刷新每个索引的"stat"信息以与当前stat信息匹配。它不会更新对象状态本身，并且只会更新用于快速测试对象是否仍与其旧后备存储对象匹配的字段。

The previously introduced [git-add[1]](../1/git-add) is just a wrapper for [git-update-index[1]](../1/git-update-index).

​	之前介绍的 [git-add[1]](../1/git-add) 只是 [git-update-index[1]](../1/git-update-index) 的一个包装器。

#### 索引 → 对象数据库

You write your current index file to a "tree" object with the program

​	您可以使用以下程序将当前的索引文件写入“tree”对象：

``` bash
$ git write-tree
```

that doesn’t come with any options—it will just write out the current index into the set of tree objects that describe that state, and it will return the name of the resulting top-level tree. You can use that tree to re-generate the index at any time by going in the other direction:

该程序不带任何选项，它会将当前的索引写入描述该状态的一组树对象，并返回生成的顶层树的名称。您可以使用该树在任何时候重新生成索引，通过执行以下操作：

#### 对象数据库 → 索引

You read a "tree" file from the object database, and use that to populate (and overwrite—don’t do this if your index contains any unsaved state that you might want to restore later!) your current index. Normal operation is just

​	您可以从对象数据库中读取“tree”文件，并使用它来填充（并覆盖 - 如果您的索引包含任何未保存状态，您可能希望稍后恢复！）当前的索引。正常操作是：

``` bash
$ git read-tree <SHA-1 of tree>
```

and your index file will now be equivalent to the tree that you saved earlier. However, that is only your *index* file: your working directory contents have not been modified.

现在，您的索引文件将等同于之前保存的树。然而，这只是您的索引文件：您的工作目录内容尚未修改。

#### 索引 → 工作目录

You update your working directory from the index by "checking out" files. This is not a very common operation, since normally you’d just keep your files updated, and rather than write to your working directory, you’d tell the index files about the changes in your working directory (i.e. `git update-index`).

​	您可以通过"checking out"文件来从索引更新您的工作目录。这不是一个很常见的操作，因为通常您只需要保持您的文件更新，并且而不是写入您的工作目录，您会告诉索引文件关于工作目录中的更改（即 `git update-index`）。

However, if you decide to jump to a new version, or check out somebody else’s version, or just restore a previous tree, you’d populate your index file with read-tree, and then you need to check out the result with

​	但是，如果您决定切换到新版本，或者检出其他人的版本，或者恢复以前的树，您将用 `git read-tree` 填充索引文件，然后需要使用以下命令检出结果：

``` bash
$ git checkout-index filename
```

or, if you want to check out all of the index, use `-a`.

或者，如果要检出所有索引，使用 `-a`。

NOTE! `git checkout-index` normally refuses to overwrite old files, so if you have an old version of the tree already checked out, you will need to use the `-f` flag (*before* the `-a` flag or the filename) to *force* the checkout.

​	注意！`git checkout-index` 通常会拒绝覆盖旧文件，因此如果您已经检出了旧版本的树，则需要在 `-a` 标志或文件名之前使用 `-f` 标志（在其之前）来*强制*检出。

Finally, there are a few odds and ends which are not purely moving from one representation to the other:

​	最后，还有一些不纯粹从一种表示形式移动到另一种表示形式的操作：

#### 将所有内容联系在一起 Tying it all together

To commit a tree you have instantiated with `git write-tree`, you’d create a "commit" object that refers to that tree and the history behind it—most notably the "parent" commits that preceded it in history.

​	要提交使用 `git write-tree` 实例化的树，您将创建一个引用该树及其背后的历史的 "commit" 对象。主要是前面在历史中紧随其后的 "parent" 提交。

Normally a "commit" has one parent: the previous state of the tree before a certain change was made. However, sometimes it can have two or more parent commits, in which case we call it a "merge", due to the fact that such a commit brings together ("merges") two or more previous states represented by other commits.

​	通常，一个 "commit" 有一个父提交：某种更改之前的树的前一个状态。然而，有时候一个 "commit" 可能有两个或更多个父提交，在这种情况下，我们称之为"合并"，因为此类提交将其他提交表示的两个或多个先前状态聚集（"合并"）在一起。

In other words, while a "tree" represents a particular directory state of a working directory, a "commit" represents that state in time, and explains how we got there.

​	换句话说，"tree" 表示工作目录的特定目录状态，而 "commit" 表示那个时间点的状态，并解释了我们是如何到达那里的。

You create a commit object by giving it the tree that describes the state at the time of the commit, and a list of parents:

​	您通过给它提交时的状态描述的树以及父提交列表来创建 "commit" 对象：

``` bash
$ git commit-tree <tree> -p <parent> [(-p <parent2>)...]
```

and then giving the reason for the commit on stdin (either through redirection from a pipe or file, or by just typing it at the tty).

然后在标准输入上给出提交原因（通过从管道或文件重定向，或者直接在 tty 上键入）。

`git commit-tree` will return the name of the object that represents that commit, and you should save it away for later use. Normally, you’d commit a new `HEAD` state, and while Git doesn’t care where you save the note about that state, in practice we tend to just write the result to the file pointed at by `.git/HEAD`, so that we can always see what the last committed state was.

​	`git commit-tree` 将返回表示该提交的对象的名称，您应该保存它以供以后使用。通常，您会提交一个新的 `HEAD` 状态，而 Git 不关心您将该状态的注释保存在何处，但实际上我们倾向于将结果写入 `.git/HEAD` 指向的文件，以便始终可以看到最后一个提交状态是什么。

Here is a picture that illustrates how various pieces fit together:

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

You can examine the data represented in the object database and the index with various helper tools. For every object, you can use [git-cat-file[1]](../1/git-cat-file) to examine details about the object:

​	您可以使用各种辅助工具检查对象数据库和索引中表示的数据。对于每个对象，您可以使用 [git-cat-file[1]](../1/git-cat-file) 查看有关对象的详细信息：

``` bash
$ git cat-file -t <objectname>
```

shows the type of the object, and once you have the type (which is usually implicit in where you find the object), you can use

显示对象的类型，一旦您知道了类型（通常隐含在找到对象的位置中），您可以使用：

``` bash
$ git cat-file blob|tree|commit|tag <objectname>
```

to show its contents. NOTE! Trees have binary content, and as a result there is a special helper for showing that content, called `git ls-tree`, which turns the binary content into a more easily readable form.

来显示其内容。注意！树具有二进制内容，因此有一个特殊的辅助工具用于显示内容，称为 `git ls-tree`，它将二进制内容转换为更易读的形式。

It’s especially instructive to look at "commit" objects, since those tend to be small and fairly self-explanatory. In particular, if you follow the convention of having the top commit name in `.git/HEAD`, you can do

​	查看“commit”对象特别有教育意义，因为这些对象往往很小且相当容易理解。特别是，如果您遵循将顶部提交名称保存在 `.git/HEAD` 中的约定，则可以执行以下操作：

``` bash
$ git cat-file commit HEAD
```

to see what the top commit was.

以查看顶部提交的内容。

### 合并多个树

Git can help you perform a three-way merge, which can in turn be used for a many-way merge by repeating the merge procedure several times. The usual situation is that you only do one three-way merge (reconciling two lines of history) and commit the result, but if you like to, you can merge several branches in one go.

​	Git 可以帮助您执行三方合并，通过多次重复合并过程，可以使用它执行多路合并。通常情况下，您只执行一次三方合并（调和两个历史线），然后提交结果，但如果愿意，您可以一次合并多个分支。

To perform a three-way merge, you start with the two commits you want to merge, find their closest common parent (a third commit), and compare the trees corresponding to these three commits.

​	要执行三方合并，您首先获取要合并的两个提交，并找到它们的最近公共父提交（第三个提交），然后比较对应于这三个提交的树。

To get the "base" for the merge, look up the common parent of two commits:

​	要获取合并的"基"，请查找两个提交的公共父提交：

``` bash
$ git merge-base <commit1> <commit2>
```

This prints the name of a commit they are both based on. You should now look up the tree objects of those commits, which you can easily do with

​	这将打印它们都基于的提交名称。现在，您应该查找这些提交的树对象，可以使用以下命令轻松地执行：

``` bash
$ git cat-file commit <commitname> | head -1
```

since the tree object information is always the first line in a commit object.

因为树对象信息总是出现在提交对象的第一行。

Once you know the three trees you are going to merge (the one "original" tree, aka the common tree, and the two "result" trees, aka the branches you want to merge), you do a "merge" read into the index. This will complain if it has to throw away your old index contents, so you should make sure that you’ve committed those—in fact you would normally always do a merge against your last commit (which should thus match what you have in your current index anyway).

​	一旦知道您将要合并的三个树（即 "原始" 树，也称为共同树，以及两个 "结果" 树，即要合并的分支），您可以在索引中执行 "merge" 读取。如果它必须丢弃旧的索引内容，它会发出警告，因此您应该确保已提交这些内容 - 实际上，您通常会对最后一个提交执行合并（因此应与当前索引的内容匹配）。

To do the merge, do

​	要执行合并，使用以下命令：

``` bash
$ git read-tree -m -u <origtree> <yourtree> <targettree>
```

which will do all trivial merge operations for you directly in the index file, and you can just write the result out with `git write-tree`.

这将在索引文件中直接为您执行所有简单的合并操作，然后您可以使用 `git write-tree` 将结果写入。

### 继续合并多个树

Sadly, many merges aren’t trivial. If there are files that have been added, moved or removed, or if both branches have modified the same file, you will be left with an index tree that contains "merge entries" in it. Such an index tree can *NOT* be written out to a tree object, and you will have to resolve any such merge clashes using other tools before you can write out the result.

​	不幸的是，许多合并并不是简单的。如果有文件已添加、移动或删除，或者两个分支都修改了相同的文件，您将得到一个包含“合并条目”的索引树。这样的索引树*无法*写入到树对象中，您必须使用其他工具解决任何此类合并冲突，然后才能写出结果。

You can examine such index state with `git ls-files --unmerged` command. An example:

​	您可以使用 `git ls-files --unmerged` 命令检查此类索引状态。以下是一个例子：

``` bash
$ git read-tree -m $orig HEAD $target
$ git ls-files --unmerged
100644 263414f423d0e4d70dae8fe53fa34614ff3e2860 1	hello.c
100644 06fa6a24256dc7e560efa5687fa84b51f0263c3a 2	hello.c
100644 cc44c73eb783565da5831b4d820c962954019b69 3	hello.c
```

Each line of the `git ls-files --unmerged` output begins with the blob mode bits, blob SHA-1, *stage number*, and the filename. The *stage number* is Git’s way to say which tree it came from: stage 1 corresponds to the `$orig` tree, stage 2 to the `HEAD` tree, and stage 3 to the `$target` tree.

​	`git ls-files --unmerged` 输出的每一行以blob模式位、blob SHA-1、*stage number*和文件名开头。*stage number* 是Git用来表示它来自哪个树的方式：stage 1 对应 `$orig` 树，stage 2 对应 `HEAD` 树，stage 3 对应 `$target` 树。

Earlier we said that trivial merges are done inside `git read-tree -m`. For example, if the file did not change from `$orig` to `HEAD` or `$target`, or if the file changed from `$orig` to `HEAD` and `$orig` to `$target` the same way, obviously the final outcome is what is in `HEAD`. What the above example shows is that file `hello.c` was changed from `$orig` to `HEAD` and `$orig` to `$target` in a different way. You could resolve this by running your favorite 3-way merge program, e.g. `diff3`, `merge`, or Git’s own merge-file, on the blob objects from these three stages yourself, like this:

​	前面我们说过，简单的合并是在 `git read-tree -m` 中完成的。例如，如果文件从 `$orig` 到 `HEAD` 或者 `$orig` 到 `$target` 都没有改变，或者文件从 `$orig` 到 `HEAD` 和 `$orig` 到 `$target` 改变的方式相同，那么最终的结果显然是 `HEAD` 中的内容。上面的示例显示了文件 `hello.c` 在从 `$orig` 到 `HEAD` 和从 `$orig` 到 `$target` 之间以不同的方式进行了更改。您可以通过运行您喜欢的3路合并程序（如`diff3`、`merge`或Git自己的merge-file）来解决这个问题，操作blob对象中来自这三个stage的数据，例如：

``` bash
$ git cat-file blob 263414f >hello.c~1
$ git cat-file blob 06fa6a2 >hello.c~2
$ git cat-file blob cc44c73 >hello.c~3
$ git merge-file hello.c~2 hello.c~1 hello.c~3
```

This would leave the merge result in `hello.c~2` file, along with conflict markers if there are conflicts. After verifying the merge result makes sense, you can tell Git what the final merge result for this file is by:

​	这将在文件 `hello.c~2` 中留下合并结果，如果有冲突，则会有冲突标记。在验证合并结果正确后，您可以通过以下命令告诉Git该文件的最终合并结果：

``` bash
$ mv -f hello.c~2 hello.c
$ git update-index hello.c
```

When a path is in the "unmerged" state, running `git update-index` for that path tells Git to mark the path resolved.

​	当一个路径处于“unmerged”状态时，对该路径运行 `git update-index` 告诉Git将该路径标记为已解决。

The above is the description of a Git merge at the lowest level, to help you understand what conceptually happens under the hood. In practice, nobody, not even Git itself, runs `git cat-file` three times for this. There is a `git merge-index` program that extracts the stages to temporary files and calls a "merge" script on it:

​	以上是Git在最低层面上的合并描述，以帮助您理解底层的概念。实际上，即使是Git本身，也不会为此运行 `git cat-file` 三次。有一个名为 `git merge-index` 的程序，它将stage提取到临时文件中，并在其上调用一个“merge”脚本：

``` bash
$ git merge-index git-merge-one-file hello.c
```

and that is what higher level `git merge -s resolve` is implemented with.

高级别的 `git merge -s resolve` 就是使用这种方法实现的。

## Hacking Git

This chapter covers internal details of the Git implementation which probably only Git developers need to understand.

​	本章涵盖了 Git 实现的内部细节，这些细节可能只有 Git 开发人员才需要了解。

### 对象存储格式

All objects have a statically determined "type" which identifies the format of the object (i.e. how it is used, and how it can refer to other objects). There are currently four different object types: "blob", "tree", "commit", and "tag".

​	所有对象都有一个静态确定的“类型”，用于标识对象的格式（即它是如何使用的，以及它如何引用其他对象）。目前有四种不同的对象类型：“blob”、“tree”、“commit”和“tag”。

Regardless of object type, all objects share the following characteristics: they are all deflated with zlib, and have a header that not only specifies their type, but also provides size information about the data in the object. It’s worth noting that the SHA-1 hash that is used to name the object is the hash of the original data plus this header, so `sha1sum` *file* does not match the object name for *file*.

​	无论对象类型如何，所有对象共享以下特征：它们都使用zlib进行了压缩，并且具有标头，该标头不仅指定了它们的类型，还提供有关对象中数据的大小信息。值得注意的是，用于命名对象的SHA-1哈希是原始数据加上此标头的哈希，因此 `sha1sum file` 的*文件*与*文件*的对象名称不匹配。

As a result, the general consistency of an object can always be tested independently of the contents or the type of the object: all objects can be validated by verifying that (a) their hashes match the content of the file and (b) the object successfully inflates to a stream of bytes that forms a sequence of `<ascii type without space> + <space> + <ascii decimal size> + <byte\0> + <binary object data>`.

​	因此，无论对象的内容或类型如何，总是可以独立测试对象的一般一致性：可以通过验证（a）它们的哈希与文件内容匹配，并且（b）对象成功解压缩为一系列`<ascii type without space> + <space> + <ascii decimal size> + <byte\0> + <binary object data>`的字节流来验证它们的内部一致性。

The structured objects can further have their structure and connectivity to other objects verified. This is generally done with the `git fsck` program, which generates a full dependency graph of all objects, and verifies their internal consistency (in addition to just verifying their superficial consistency through the hash).

​	结构化对象可以进一步验证其结构和与其他对象的连接。这通常通过 `git fsck` 程序执行，该程序生成所有对象的完整依赖图，并验证它们的内部一致性（除了仅验证它们的浅表一致性通过哈希验证之外）。

### Git 源代码的鸟瞰

It is not always easy for new developers to find their way through Git’s source code. This section gives you a little guidance to show where to start.

​	对于新开发人员来说，找到Git的源代码并不总是容易。本节将为您提供一些指导，以显示从哪里开始。

A good place to start is with the contents of the initial commit, with:

​	一个很好的起点是查看最初提交的内容：

``` bash
$ git switch --detach e83c5163
```

The initial revision lays the foundation for almost everything Git has today, but is small enough to read in one sitting.

​	初始版本为Git今天拥有的几乎所有功能奠定了基础，但大小足够小，可以在一次阅读中理解。

Note that terminology has changed since that revision. For example, the README in that revision uses the word "changeset" to describe what we now call a [commit](https://git-scm.com/docs/user-manual#def_commit_object).

​	请注意，术语自初始提交以来已经发生了变化。例如，初始提交中的README使用“changeset”一词来描述我们现在称为[commit](https://git-scm.com/docs/user-manual#def_commit_object)的内容。

Also, we do not call it "cache" any more, but rather "index"; however, the file is still called `cache.h`. Remark: Not much reason to change it now, especially since there is no good single name for it anyway, because it is basically *the* header file which is included by *all* of Git’s C sources.

​	此外，我们不再称之为“cache”，而是称为“index”；然而，该文件仍然称为`cache.h`。请注意：现在没有太多理由再更改它，特别是因为没有一个好的单一名称，因为它基本上是所有Git的C源文件都包含的*the*头文件。

If you grasp the ideas in that initial commit, you should check out a more recent version and skim `cache.h`, `object.h` and `commit.h`.

​	如果您掌握了初始提交中的思想，可以检出一个更新的版本，并浏览`cache.h`、`object.h`和`commit.h`。

In the early days, Git (in the tradition of UNIX) was a bunch of programs which were extremely simple, and which you used in scripts, piping the output of one into another. This turned out to be good for initial development, since it was easier to test new things. However, recently many of these parts have become builtins, and some of the core has been "libified", i.e. put into libgit.a for performance, portability reasons, and to avoid code duplication.

​	在早期，Git（遵循UNIX的传统）是一堆极其简单的程序，您可以在脚本中使用这些程序，将一个程序的输出传递给另一个程序。这事实上有利于初步开发，因为测试新功能更容易。但是，最近，许多这些部分已经成为内建命令，一些核心功能已经被“库化”，即放入libgit.a中，以提高性能、可移植性和避免代码重复。

By now, you know what the index is (and find the corresponding data structures in `cache.h`), and that there are just a couple of object types (blobs, trees, commits and tags) which inherit their common structure from `struct object`, which is their first member (and thus, you can cast e.g. `(struct object *)commit` to achieve the *same* as `&commit->object`, i.e. get at the object name and flags).

​	现在，您了解了索引是什么（并在`cache.h`中找到了相应的数据结构），以及只有几种对象类型（blob、tree、commit和tag），它们继承自`struct object`，它是它们的第一个成员（因此，您可以将例如`(struct object *)commit`转换为`&commit->object`以达到相同的效果，即获取对象名称和标志）。

Now is a good point to take a break to let this information sink in.

​	现在是一个休息的好时机，以便让这些信息沉淀下来。

Next step: get familiar with the object naming. Read [Naming commits](https://git-scm.com/docs/user-manual#naming-commits). There are quite a few ways to name an object (and not only revisions!). All of these are handled in `sha1_name.c`. Just have a quick look at the function `get_sha1()`. A lot of the special handling is done by functions like `get_sha1_basic()` or the likes.

​	下一步：熟悉对象命名。阅读 [命名commits](https://git-scm.com/docs/user-manual#naming-commits)。有很多方法来为一个对象命名（不仅限于revisions！）。所有这些都在`sha1_name.c`中处理。只需快速查看函数`get_sha1()`。许多特殊处理由函数如`get_sha1_basic()`等执行。

This is just to get you into the groove for the most libified part of Git: the revision walker.

​	这只是为了让您了解Git最大程度的库化部分：revision walker。

Basically, the initial version of `git log` was a shell script:

​	基本上，`git log` 的初始版本是一个shell脚本：

``` bash
$ git-rev-list --pretty $(git-rev-parse --default HEAD "$@") | \
	LESS=-S ${PAGER:-less}
```

What does this mean?

​	这意味着什么？

`git rev-list` is the original version of the revision walker, which *always* printed a list of revisions to stdout. It is still functional, and needs to, since most new Git commands start out as scripts using `git rev-list`.

​	`git rev-list` 是revision walker的原始版本，它始终将一系列revisions打印到stdout。它仍然功能齐全，并且需要这样做，因为大多数新的Git命令都是使用`git rev-list`的脚本开始的。

`git rev-parse` is not as important any more; it was only used to filter out options that were relevant for the different plumbing commands that were called by the script.

​	`git rev-parse` 已经不再那么重要了；它仅用于过滤对不同plumbing命令有关的选项，这些命令由脚本调用。

Most of what `git rev-list` did is contained in `revision.c` and `revision.h`. It wraps the options in a struct named `rev_info`, which controls how and what revisions are walked, and more.

​	`git rev-list` 做的大部分工作都包含在 `revision.c` 和 `revision.h` 中。它将选项封装在名为 `rev_info` 的结构体中，该结构体控制着如何以及哪些修订版本被遍历等。

The original job of `git rev-parse` is now taken by the function `setup_revisions()`, which parses the revisions and the common command-line options for the revision walker. This information is stored in the struct `rev_info` for later consumption. You can do your own command-line option parsing after calling `setup_revisions()`. After that, you have to call `prepare_revision_walk()` for initialization, and then you can get the commits one by one with the function `get_revision()`.

​	`git rev-parse` 最初的任务现在由函数 `setup_revisions()` 完成，它解析修订版本和修订版本遍历器的常用命令行选项。此信息存储在结构体 `rev_info` 中以供以后使用。在调用 `setup_revisions()` 后，您可以进行自己的命令行选项解析。之后，您必须调用 `prepare_revision_walk()` 进行初始化，然后可以使用函数 `get_revision()` 逐个获取提交。

If you are interested in more details of the revision walking process, just have a look at the first implementation of `cmd_log()`; call `git show v1.3.0~155^2~4` and scroll down to that function (note that you no longer need to call `setup_pager()` directly).

​	如果您对修订版本遍历过程的更多细节感兴趣，可以查看 `cmd_log()` 的第一个实现；执行 `git show v1.3.0~155^2~4` 并滚动到该函数（请注意，您不再需要直接调用 `setup_pager()`）。

Nowadays, `git log` is a builtin, which means that it is *contained* in the command `git`. The source side of a builtin is

​	现在，`git log` 是一个内建命令，这意味着它*包含在*命令 `git` 中。内建命令的源代码包含：

- a function called `cmd_<bla>`, typically defined in `builtin/<bla.c>` (note that older versions of Git used to have it in `builtin-<bla>.c` instead), and declared in `builtin.h`.
- 一个名为 `cmd_<bla>` 的函数，通常定义在 `builtin/<bla.c>` 中（请注意，旧版本的Git将其定义在 `builtin-<bla>.c` 中），并在 `builtin.h` 中声明。
- an entry in the `commands[]` array in `git.c`, and
- 在 `git.c` 中的 `commands[]` 数组中的条目，
- an entry in `BUILTIN_OBJECTS` in the `Makefile`.
- 在 `Makefile` 中的 `BUILTIN_OBJECTS` 中的条目。

Sometimes, more than one builtin is contained in one source file. For example, `cmd_whatchanged()` and `cmd_log()` both reside in `builtin/log.c`, since they share quite a bit of code. In that case, the commands which are *not* named like the `.c` file in which they live have to be listed in `BUILT_INS` in the `Makefile`.

​	有时，一个源文件中包含多个内建命令。例如，`cmd_whatchanged()` 和 `cmd_log()` 都位于 `builtin/log.c` 中，因为它们共享相当多的代码。在这种情况下，不像所在的 `.c` 文件命名的命令必须在 `Makefile` 的 `BUILT_INS` 中列出。

`git log` looks more complicated in C than it does in the original script, but that allows for a much greater flexibility and performance.

​	`git log` 在C中看起来比原始脚本更复杂，但这样可以实现更大的灵活性和性能。

Here again it is a good point to take a pause.

​	现在是一个适合休息的好时机。

Lesson three is: study the code. Really, it is the best way to learn about the organization of Git (after you know the basic concepts).

​	第三课是：研究代码。确实，这是了解Git组织结构的最佳方式（在您了解了基本概念之后）。

So, think about something which you are interested in, say, "how can I access a blob just knowing the object name of it?". The first step is to find a Git command with which you can do it. In this example, it is either `git show` or `git cat-file`.

​	因此，考虑一些您感兴趣的内容，比如“如果只知道blob的对象名称，我怎么访问它？”。第一步是找到可以使用它的Git命令。在这个例子中，要么是 `git show`，要么是 `git cat-file`。

For the sake of clarity, let’s stay with `git cat-file`, because it

​	为了明确起见，让我们使用 `git cat-file`，因为它： 

- is plumbing, and
- 是plumbing，
- was around even in the initial commit (it literally went only through some 20 revisions as `cat-file.c`, was renamed to `builtin/cat-file.c` when made a builtin, and then saw less than 10 versions).
- 即使在最初的提交中也存在（在 `cat-file.c` 中仅经历了大约20个修订版本，当它成为内建命令时，将其重命名为 `builtin/cat-file.c`，然后只进行了不到10个版本的修改）。

So, look into `builtin/cat-file.c`, search for `cmd_cat_file()` and look what it does.

​	所以，看看 `builtin/cat-file.c`，搜索 `cmd_cat_file()` 并查看它做了什么。

```
        git_config(git_default_config);
        if (argc != 3)
		usage("git cat-file [-t|-s|-e|-p|<type>] <sha1>");
        if (get_sha1(argv[2], sha1))
                die("Not a valid object name %s", argv[2]);
```

Let’s skip over the obvious details; the only really interesting part here is the call to `get_sha1()`. It tries to interpret `argv[2]` as an object name, and if it refers to an object which is present in the current repository, it writes the resulting SHA-1 into the variable `sha1`.

​	让我们略过显而易见的细节；这里唯一真正有趣的部分是对 `get_sha1()` 的调用。它试图将 `argv[2]` 解释为对象名称，如果它引用当前仓库中存在的对象，则将结果SHA-1写入变量 `sha1`。

Two things are interesting here:

​	以下两点很有趣：

- `get_sha1()` returns 0 on *success*. This might surprise some new Git hackers, but there is a long tradition in UNIX to return different negative numbers in case of different errors—and 0 on success.
- `get_sha1()` 成功时返回0。这可能会让一些新的Git开发者感到惊讶，但在UNIX中，有一个长期的传统，在不同错误的情况下返回不同的负数值，并在成功的情况下返回0。
- the variable `sha1` in the function signature of `get_sha1()` is `unsigned char *`, but is actually expected to be a pointer to `unsigned char[20]`. This variable will contain the 160-bit SHA-1 of the given commit. Note that whenever a SHA-1 is passed as `unsigned char *`, it is the binary representation, as opposed to the ASCII representation in hex characters, which is passed as `char *`.
- `get_sha1()` 函数签名中的变量 `sha1` 是 `unsigned char *`，但实际上预期是指向 `unsigned char[20]` 的指针。这个变量将包含给定commit的160位SHA-1值。请注意，每当SHA-1作为 `unsigned char *` 传递时，它是二进制表示，与以十六进制字符表示的ASCII表示形式（传递为 `char *`）相对应。

You will see both of these things throughout the code.

​	您将在代码中看到这两点。

Now, for the meat:

​	接下来是主要部分：

```
        case 0:
                buf = read_object_with_reference(sha1, argv[1], &size, NULL);
```

This is how you read a blob (actually, not only a blob, but any type of object). To know how the function `read_object_with_reference()` actually works, find the source code for it (something like `git grep read_object_with | grep ":[a-z]"` in the Git repository), and read the source.

​	这是如何读取一个blob（实际上不仅限于blob，还包括任何类型的对象）的方法。要了解 `read_object_with_reference()` 函数实际上是如何工作的，请找到它的源代码（在Git仓库中执行类似于 `git grep read_object_with | grep ":[a-z]"` 的命令），并阅读源代码。

To find out how the result can be used, just read on in `cmd_cat_file()`:

​	要了解结果如何使用，只需继续阅读 `cmd_cat_file()`：

```
        write_or_die(1, buf, size);
```

Sometimes, you do not know where to look for a feature. In many such cases, it helps to search through the output of `git log`, and then `git show` the corresponding commit.

​	有时，您不知道在哪里查找某个功能。在许多这种情况下，通过 `git log` 的输出进行搜索，然后使用 `git show` 显示对应的提交。

Example: If you know that there was some test case for `git bundle`, but do not remember where it was (yes, you *could* `git grep bundle t/`, but that does not illustrate the point!):

​	例如：如果您知道有一些与 `git bundle` 相关的测试用例，但不记得在哪里（是的，您*可以* `git grep bundle t/`，但这并没有说明问题！）：

``` bash
$ git log --no-merges t/
```

In the pager (`less`), just search for "bundle", go a few lines back, and see that it is in commit 18449ab0. Now just copy this object name, and paste it into the command line

​	在分页器（`less`）中，只需搜索“bundle”，然后向前翻动几行，看到它在提交 18449ab0 中。现在只需复制此对象名称，并将其粘贴到命令行中：

``` bash
$ git show 18449ab0
```

Voila.

Voila。

Another example: Find out what to do in order to make some script a builtin:

​	另一个例子：找出使某个脚本成为内建命令所需的步骤：

``` bash
$ git log --no-merges --diff-filter=A builtin/*.c
```

You see, Git is actually the best tool to find out about the source of Git itself!

​	您看，Git实际上是了解有关Git源代码的最佳工具！

## Git 术语 - Git Glossary

### Git 解释

- alternate object database - 替代对象数据库

  Via the alternates mechanism, a [repository](https://git-scm.com/docs/user-manual#def_repository) can inherit part of its [object database](https://git-scm.com/docs/user-manual#def_object_database) from another object database, which is called an "alternate".

  通过替代机制，一个[仓库](https://git-scm.com/docs/user-manual#def_repository)可以从另一个对象数据库继承部分对象数据库，这个对象数据库被称为 "替代"。

- bare repository - 裸仓库

  A bare repository is normally an appropriately named [directory](https://git-scm.com/docs/user-manual#def_directory) with a `.git` suffix that does not have a locally checked-out copy of any of the files under revision control. That is, all of the Git administrative and control files that would normally be present in the hidden `.git` sub-directory are directly present in the `repository.git` directory instead, and no other files are present and checked out. Usually publishers of public repositories make bare repositories available.

  裸仓库通常是一个适当命名的[目录](https://git-scm.com/docs/user-manual#def_directory)，具有 `.git` 后缀，它没有本地检出的任何受版本控制的文件副本。也就是说，所有的 Git 管理和控制文件，通常存在于隐藏的 `.git` 子目录中，直接存在于 `repository.git` 目录中，且没有其他文件存在和被检出。通常，公共仓库的发布者提供裸仓库。

- blob object - blob 对象

  Untyped [object](https://git-scm.com/docs/user-manual#def_object), e.g. the contents of a file.

  无类型的[对象](https://git-scm.com/docs/user-manual#def_object)，例如文件的内容。

- branch - 分支

  A "branch" is a line of development. The most recent [commit](https://git-scm.com/docs/user-manual#def_commit) on a branch is referred to as the tip of that branch. The tip of the branch is [referenced](https://git-scm.com/docs/user-manual#def_ref) by a branch [head](https://git-scm.com/docs/user-manual#def_head), which moves forward as additional development is done on the branch. A single Git [repository](https://git-scm.com/docs/user-manual#def_repository) can track an arbitrary number of branches, but your [working tree](https://git-scm.com/docs/user-manual#def_working_tree) is associated with just one of them (the "current" or "checked out" branch), and [HEAD](https://git-scm.com/docs/user-manual#def_HEAD) points to that branch.

  "分支" 是一个开发线。分支上的最新[提交](https://git-scm.com/docs/user-manual#def_commit)被称为该分支的顶部。分支的顶部由分支[头引用](https://git-scm.com/docs/user-manual#def_head)所引用，该头引用随着分支上的额外开发而向前移动。单个 Git [仓库](https://git-scm.com/docs/user-manual#def_repository) 可以跟踪任意数量的分支，但是您的[工作树](https://git-scm.com/docs/user-manual#def_working_tree)只与其中之一关联（"当前"或"检出"分支），并且 [HEAD](https://git-scm.com/docs/user-manual#def_HEAD) 指向该分支。

- cache - 缓存

  Obsolete for: [index](https://git-scm.com/docs/user-manual#def_index).

  对于：[索引](https://git-scm.com/docs/user-manual#def_index)。

- chain - 链

  A list of objects, where each [object](https://git-scm.com/docs/user-manual#def_object) in the list contains a reference to its successor (for example, the successor of a [commit](https://git-scm.com/docs/user-manual#def_commit) could be one of its [parents](https://git-scm.com/docs/user-manual#def_parent)).

  一组对象，列表中的每个对象包含对其后继的引用（例如，[提交](https://git-scm.com/docs/user-manual#def_commit) 的后继可能是它的[父提交](https://git-scm.com/docs/user-manual#def_parent)之一）。

- changeset - 变更集

  BitKeeper/cvsps speak for "[commit](https://git-scm.com/docs/user-manual#def_commit)". Since Git does not store changes, but states, it really does not make sense to use the term "changesets" with Git.

  BitKeeper/cvsps 中的术语，表示"[提交](https://git-scm.com/docs/user-manual#def_commit)"。由于 Git 不存储变更，而是状态，因此在 Git 中使用术语 "变更集" 实际上是不合适的。

- checkout - 检出

  The action of updating all or part of the [working tree](https://git-scm.com/docs/user-manual#def_working_tree) with a [tree object](https://git-scm.com/docs/user-manual#def_tree_object) or [blob](https://git-scm.com/docs/user-manual#def_blob_object) from the [object database](https://git-scm.com/docs/user-manual#def_object_database), and updating the [index](https://git-scm.com/docs/user-manual#def_index) and [HEAD](https://git-scm.com/docs/user-manual#def_HEAD) if the whole working tree has been pointed at a new [branch](https://git-scm.com/docs/user-manual#def_branch).

  使用来自[对象数据库](https://git-scm.com/docs/user-manual#def_object_database)中的[树对象](https://git-scm.com/docs/user-manual#def_tree_object)或[blob](https://git-scm.com/docs/user-manual#def_blob_object)更新[工作树](https://git-scm.com/docs/user-manual#def_working_tree)的操作，并更新[索引](https://git-scm.com/docs/user-manual#def_index)和[HEAD](https://git-scm.com/docs/user-manual#def_HEAD)，如果整个工作树已指向新的[分支](https://git-scm.com/docs/user-manual#def_branch)。

- cherry-picking

  In [SCM](https://git-scm.com/docs/user-manual#def_SCM) jargon, "cherry pick" means to choose a subset of changes out of a series of changes (typically commits) and record them as a new series of changes on top of a different codebase. In Git, this is performed by the "git cherry-pick" command to extract the change introduced by an existing [commit](https://git-scm.com/docs/user-manual#def_commit) and to record it based on the tip of the current [branch](https://git-scm.com/docs/user-manual#def_branch) as a new commit.

  在[SCM](https://git-scm.com/docs/user-manual#def_SCM)行话中，"樱桃拣选" 意味着从一系列变更（通常是提交）中选择一部分变更，并将它们记录为基于不同仓库的新一系列变更。在 Git 中，这通过 "git cherry-pick" 命令来完成，该命令提取现有[提交](https://git-scm.com/docs/user-manual#def_commit)引入的更改，并将其作为新的提交基于当前[分支](https://git-scm.com/docs/user-manual#def_branch)的顶部记录。

- clean

  A [working tree](https://git-scm.com/docs/user-manual#def_working_tree) is clean, if it corresponds to the [revision](https://git-scm.com/docs/user-manual#def_revision) referenced by the current [head](https://git-scm.com/docs/user-manual#def_head). Also see "[dirty](https://git-scm.com/docs/user-manual#def_dirty)".

  如果[工作树](https://git-scm.com/docs/user-manual#def_working_tree)与当前[头引用](https://git-scm.com/docs/user-manual#def_head)引用的[修订版本](https://git-scm.com/docs/user-manual#def_revision)对应，则[工作树](https://git-scm.com/docs/user-manual#def_working_tree)是"clean"的。另请参阅 "[dirty](https://git-scm.com/docs/user-manual#def_dirty)"。

- commit - 提交

  As a noun: A single point in the Git history; the entire history of a project is represented as a set of interrelated commits. The word "commit" is often used by Git in the same places other revision control systems use the words "revision" or "version". Also used as a short hand for [commit object](https://git-scm.com/docs/user-manual#def_commit_object).As a verb: The action of storing a new snapshot of the project’s state in the Git history, by creating a new commit representing the current state of the [index](https://git-scm.com/docs/user-manual#def_index) and advancing [HEAD](https://git-scm.com/docs/user-manual#def_HEAD) to point at the new commit.

  作为名词：Git 历史中的一个单一点；整个项目的历史被表示为一组相关的提交。"提交" 这个词在 Git 中经常用于与其他版本控制系统中使用的 "修订版本" 或 "版本" 这两个词相同的地方。也用作 [提交对象](https://git-scm.com/docs/user-manual#def_commit_object) 的简称。作为动词：将项目的新状态以新提交的形式存储在 Git 历史中，通过创建一个新的提交来表示当前[索引](https://git-scm.com/docs/user-manual#def_index)的状态，并将[HEAD](https://git-scm.com/docs/user-manual#def_HEAD)前进到指向新的提交。

- commit graph concept, representations and usage - 提交图概念、表示和用法

  A synonym for the [DAG](https://git-scm.com/docs/user-manual#def_DAG) structure formed by the commits in the object database, [referenced](https://git-scm.com/docs/user-manual#def_ref) by branch tips, using their [chain](https://git-scm.com/docs/user-manual#def_chain) of linked commits. This structure is the definitive commit graph. The graph can be represented in other ways, e.g. the ["commit-graph" file](https://git-scm.com/docs/user-manual#def_commit_graph_file).

  表示提交对象数据库中提交所形成的 [DAG](https://git-scm.com/docs/user-manual#def_DAG) 结构的同义词，由分支顶部引用引用，使用它们的链接提交的[链](https://git-scm.com/docs/user-manual#def_chain)。该结构是定义性的提交图。图可以以其他方式表示，例如["提交图" 文件](https://git-scm.com/docs/user-manual#def_commit_graph_file)。

- commit-graph file

  The "commit-graph" (normally hyphenated) file is a supplemental representation of the [commit graph](https://git-scm.com/docs/user-manual#def_commit_graph_general) which accelerates commit graph walks. The "commit-graph" file is stored either in the .git/objects/info directory or in the info directory of an alternate object database.

  "提交图"（通常连字符连接）文件是提交图的附加表示，加速提交图遍历。"提交图" 文件存储在 `.git/objects/info` 目录中或替代对象数据库的 info 目录中。

- commit object

  An [object](https://git-scm.com/docs/user-manual#def_object) which contains the information about a particular [revision](https://git-scm.com/docs/user-manual#def_revision), such as [parents](https://git-scm.com/docs/user-manual#def_parent), committer, author, date and the [tree object](https://git-scm.com/docs/user-manual#def_tree_object) which corresponds to the top [directory](https://git-scm.com/docs/user-manual#def_directory) of the stored revision.

  包含有关特定[修订版本](https://git-scm.com/docs/user-manual#def_revision)的信息的[对象](https://git-scm.com/docs/user-manual#def_object)，例如[父提交](https://git-scm.com/docs/user-manual#def_parent)、提交者、作者、日期和对应于存储的修订版本顶部[目录](https://git-scm.com/docs/user-manual#def_directory)的[树对象](https://git-scm.com/docs/user-manual#def_tree_object)。

- commit-ish (also committish) - 提交短词（也称为 committish）

  A [commit object](https://git-scm.com/docs/user-manual#def_commit_object) or an [object](https://git-scm.com/docs/user-manual#def_object) that can be recursively dereferenced to a commit object. The following are all commit-ishes: a commit object, a [tag object](https://git-scm.com/docs/user-manual#def_tag_object) that points to a commit object, a tag object that points to a tag object that points to a commit object, etc.

  可以递归引用为提交对象的[提交对象](https://git-scm.com/docs/user-manual#def_commit_object)或[对象](https://git-scm.com/docs/user-manual#def_object)。以下都是提交短词：提交对象、指向提交对象的[标签对象](https://git-scm.com/docs/user-manual#def_tag_object)、指向指向提交对象的标签对象的标签对象、等等。

- core Git - 核心 Git

  Fundamental data structures and utilities of Git. Exposes only limited source code management tools

  Git 的基本数据结构和实用工具。仅提供有限的源代码管理工具。

- DAG - 有向无环图

  Directed acyclic graph. The [commit objects](https://git-scm.com/docs/user-manual#def_commit_object) form a directed acyclic graph, because they have parents (directed), and the graph of commit objects is acyclic (there is no [chain](https://git-scm.com/docs/user-manual#def_chain) which begins and ends with the same [object](https://git-scm.com/docs/user-manual#def_object)).

  有向无环图。[提交对象](https://git-scm.com/docs/user-manual#def_commit_object)形成有向无环图，因为它们有父提交（有向），并且提交对象的图是无环的（没有以同一[对象](https://git-scm.com/docs/user-manual#def_object)开始并结束的[链](https://git-scm.com/docs/user-manual#def_chain)）。

- dangling object - 悬挂对象

  An [unreachable object](https://git-scm.com/docs/user-manual#def_unreachable_object) which is not [reachable](https://git-scm.com/docs/user-manual#def_reachable) even from other unreachable objects; a dangling object has no references to it from any reference or [object](https://git-scm.com/docs/user-manual#def_object) in the [repository](https://git-scm.com/docs/user-manual#def_repository).

  一个[无法访问的对象](https://git-scm.com/docs/user-manual#def_unreachable_object)，即使从其他无法访问的对象，也无法访问；悬挂对象在仓库中没有任何引用或从其他引用或[对象](https://git-scm.com/docs/user-manual#def_object)中。

- detached HEAD - 游离 HEAD

  Normally the [HEAD](https://git-scm.com/docs/user-manual#def_HEAD) stores the name of a [branch](https://git-scm.com/docs/user-manual#def_branch), and commands that operate on the history HEAD represents operate on the history leading to the tip of the branch the HEAD points at. However, Git also allows you to [check out](https://git-scm.com/docs/user-manual#def_checkout) an arbitrary [commit](https://git-scm.com/docs/user-manual#def_commit) that isn’t necessarily the tip of any particular branch. The HEAD in such a state is called "detached".Note that commands that operate on the history of the current branch (e.g. `git commit` to build a new history on top of it) still work while the HEAD is detached. They update the HEAD to point at the tip of the updated history without affecting any branch. Commands that update or inquire information *about* the current branch (e.g. `git branch --set-upstream-to` that sets what remote-tracking branch the current branch integrates with) obviously do not work, as there is no (real) current branch to ask about in this state.

  通常，[HEAD](https://git-scm.com/docs/user-manual#def_HEAD) 存储一个[分支](https://git-scm.com/docs/user-manual#def_branch)的名称，对 HEAD 所代表的树进行操作的命令是操作导致 HEAD 指向的分支顶部的历史。但是，Git 也允许您[检出](https://git-scm.com/docs/user-manual#def_checkout)不一定是任何特定分支顶部的任意[提交](https://git-scm.com/docs/user-manual#def_commit)。此状态下的 HEAD 被称为 "脱离 HEAD"。请注意，在 HEAD 被脱离的状态下，对当前分支历史的操作（例如 `git commit` 用于构建新历史）仍然有效。它们将更新 HEAD 指向更新后的历史的顶部，而不会影响任何分支。更新或查询关于当前分支的信息（例如 `git branch --set-upstream-to` 设置当前分支与哪个远程跟踪分支进行整合）的命令在这种状态下显然不起作用，因为在这种状态下没有（真正的）当前分支可以查询。

- directory - 目录

  The list you get with "ls" :-)

  使用 "ls" 命令获得的列表 :-)

- dirty

  A [working tree](https://git-scm.com/docs/user-manual#def_working_tree) is said to be "dirty" if it contains modifications which have not been [committed](https://git-scm.com/docs/user-manual#def_commit) to the current [branch](https://git-scm.com/docs/user-manual#def_branch).

  如果[工作树](https://git-scm.com/docs/user-manual#def_working_tree)包含尚未[提交](https://git-scm.com/docs/user-manual#def_commit)到当前[分支](https://git-scm.com/docs/user-manual#def_branch)的更改，则[工作树](https://git-scm.com/docs/user-manual#def_working_tree)被称为 "dirty"。

- evil merge - 恶意合并

  An evil merge is a [merge](https://git-scm.com/docs/user-manual#def_merge) that introduces changes that do not appear in any [parent](https://git-scm.com/docs/user-manual#def_parent).

  恶意合并是引入未出现在任何[父提交](https://git-scm.com/docs/user-manual#def_parent)中的更改的合并。

- fast-forward

  A fast-forward is a special type of [merge](https://git-scm.com/docs/user-manual#def_merge) where you have a [revision](https://git-scm.com/docs/user-manual#def_revision) and you are "merging" another [branch](https://git-scm.com/docs/user-manual#def_branch)'s changes that happen to be a descendant of what you have. In such a case, you do not make a new [merge](https://git-scm.com/docs/user-manual#def_merge) [commit](https://git-scm.com/docs/user-manual#def_commit) but instead just update your branch to point at the same revision as the branch you are merging. This will happen frequently on a [remote-tracking branch](https://git-scm.com/docs/user-manual#def_remote_tracking_branch) of a remote [repository](https://git-scm.com/docs/user-manual#def_repository).

  快速向前是一种特殊类型的[合并](https://git-scm.com/docs/user-manual#def_merge)，其中你有一个[修订版本](https://git-scm.com/docs/user-manual#def_revision)，并且你正在将另一个[分支](https://git-scm.com/docs/user-manual#def_branch)的更改（恰好是你所拥有的修订版本的后代）合并到当前分支。在这种情况下，您不会创建一个新的[合并](https://git-scm.com/docs/user-manual#def_merge)[提交](https://git-scm.com/docs/user-manual#def_commit)，而是将您的分支直接更新为指向要合并的分支的相同修订版本。这在[远程跟踪分支](https://git-scm.com/docs/user-manual#def_remote_tracking_branch)的情况下经常发生，是从远程[仓库](https://git-scm.com/docs/user-manual#def_repository)拉取的分支。

- fetch - 拉取

  Fetching a [branch](https://git-scm.com/docs/user-manual#def_branch) means to get the branch’s [head ref](https://git-scm.com/docs/user-manual#def_head_ref) from a remote [repository](https://git-scm.com/docs/user-manual#def_repository), to find out which objects are missing from the local [object database](https://git-scm.com/docs/user-manual#def_object_database), and to get them, too. See also [git-fetch[1]](../1/git-fetch).

  拉取一个[分支](https://git-scm.com/docs/user-manual#def_branch)意味着从远程[仓库](https://git-scm.com/docs/user-manual#def_repository)获取该分支的[头引用](https://git-scm.com/docs/user-manual#def_head_ref)，找出本地[对象数据库](https://git-scm.com/docs/user-manual#def_object_database)中缺少的对象，并获取它们。另请参阅 [git-fetch[1]](../1/git-fetch)。

- file system - 文件系统

  Linus Torvalds originally designed Git to be a user space file system, i.e. the infrastructure to hold files and directories. That ensured the efficiency and speed of Git.

  Linus Torvalds 最初设计 Git 是作为用户空间文件系统，即容纳文件和目录的基础设施。这确保了 Git 的效率和速度。

- Git archive - Git 存档

  Synonym for [repository](https://git-scm.com/docs/user-manual#def_repository) (for arch people).

  [仓库](https://git-scm.com/docs/user-manual#def_repository) 的同义词（供 arch 用户使用）。

- gitfile - git 文件

  A plain file `.git` at the root of a working tree that points at the directory that is the real repository.

  工作树根目录下的一个普通文件 `.git`，指向真正的仓库的目录。

- grafts

  Grafts enables two otherwise different lines of development to be joined together by recording fake ancestry information for commits. This way you can make Git pretend the set of [parents](https://git-scm.com/docs/user-manual#def_parent) a [commit](https://git-scm.com/docs/user-manual#def_commit) has is different from what was recorded when the commit was created. Configured via the `.git/info/grafts` file.Note that the grafts mechanism is outdated and can lead to problems transferring objects between repositories; see [git-replace[1]](../1/git-replace) for a more flexible and robust system to do the same thing.

  grafts允许将两个本来不同的开发线连接起来，通过为提交记录虚假的祖先信息。这样，您可以让 Git 假装 [提交](https://git-scm.com/docs/user-manual#def_commit) 的[父提交](https://git-scm.com/docs/user-manual#def_parent)与创建提交时记录的不同。通过 `.git/info/grafts` 文件进行配置。请注意，种植机制已过时，并可能导致在仓库之间传输对象时出现问题；参阅 [git-replace[1]](../1/git-replace) 使用更灵活、更可靠的系统完成相同的工作。

- hash

  In Git’s context, synonym for [object name](https://git-scm.com/docs/user-manual#def_object_name).

  在 Git 的上下文中，是 [对象名](https://git-scm.com/docs/user-manual#def_object_name) 的同义词。

- head

  A [named reference](https://git-scm.com/docs/user-manual#def_ref) to the [commit](https://git-scm.com/docs/user-manual#def_commit) at the tip of a [branch](https://git-scm.com/docs/user-manual#def_branch). Heads are stored in a file in `$GIT_DIR/refs/heads/` directory, except when using packed refs. (See [git-pack-refs[1]](../1/git-pack-refs).)

  [分支](https://git-scm.com/docs/user-manual#def_branch) 顶部的[命名引用](https://git-scm.com/docs/user-manual#def_ref)。HEAD 存储在 `$GIT_DIR/refs/heads/` 目录中的文件中，除非使用了 packed refs（参阅 [git-pack-refs[1]](../1/git-pack-refs)）。

- HEAD

  The current [branch](https://git-scm.com/docs/user-manual#def_branch). In more detail: Your [working tree](https://git-scm.com/docs/user-manual#def_working_tree) is normally derived from the state of the tree referred to by HEAD. HEAD is a reference to one of the [heads](https://git-scm.com/docs/user-manual#def_head) in your repository, except when using a [detached HEAD](https://git-scm.com/docs/user-manual#def_detached_HEAD), in which case it directly references an arbitrary commit.

  当前的[分支](https://git-scm.com/docs/user-manual#def_branch)。更详细地说：通常情况下，您的[工作树](https://git-scm.com/docs/user-manual#def_working_tree)是从 HEAD 引用的树的状态派生的。HEAD 是您仓库中某个[分支](https://git-scm.com/docs/user-manual#def_branch)的引用，但在使用 [游离 HEAD](https://git-scm.com/docs/user-manual#def_detached_HEAD) 的情况下，它直接引用任意提交。

- head ref

  A synonym for [head](https://git-scm.com/docs/user-manual#def_head).

  [head](https://git-scm.com/docs/user-manual#def_head) 的同义词。

- hook

  During the normal execution of several Git commands, call-outs are made to optional scripts that allow a developer to add functionality or checking. Typically, the hooks allow for a command to be pre-verified and potentially aborted, and allow for a post-notification after the operation is done. The hook scripts are found in the `$GIT_DIR/hooks/` directory, and are enabled by simply removing the `.sample` suffix from the filename. In earlier versions of Git you had to make them executable.

  在正常执行几个 Git 命令时，会调用可选的脚本，允许开发者添加功能或检查。通常，钩子允许对命令进行预验证并可能中止，以及在操作完成后进行后通知。钩子脚本位于 `$GIT_DIR/hooks/` 目录中，只需从文件名中删除 `.sample` 后缀即可启用它们。在早期的 Git 版本中，您必须使它们可执行。

- index - 索引

  A collection of files with stat information, whose contents are stored as objects. The index is a stored version of your [working tree](https://git-scm.com/docs/user-manual#def_working_tree). Truth be told, it can also contain a second, and even a third version of a working tree, which are used when [merging](https://git-scm.com/docs/user-manual#def_merge).

  一组文件的统计信息，其内容存储为对象。索引是您[工作树](https://git-scm.com/docs/user-manual#def_working_tree)的存储版本。实际上，它还可以包含第二个甚至第三个[工作树](https://git-scm.com/docs/user-manual#def_working_tree)版本，这些版本在[合并](https://git-scm.com/docs/user-manual#def_merge)时使用。

- index entry - 索引条目

  The information regarding a particular file, stored in the [index](https://git-scm.com/docs/user-manual#def_index). An index entry can be unmerged, if a [merge](https://git-scm.com/docs/user-manual#def_merge) was started, but not yet finished (i.e. if the index contains multiple versions of that file).

  有关特定文件的信息，存储在[索引](https://git-scm.com/docs/user-manual#def_index)中。如果已经开始[合并](https://git-scm.com/docs/user-manual#def_merge)但尚未完成（即索引中包含该文件的多个版本），则索引条目可能是未合并的。

- master - 主分支

  The default development [branch](https://git-scm.com/docs/user-manual#def_branch). Whenever you create a Git [repository](https://git-scm.com/docs/user-manual#def_repository), a branch named "master" is created, and becomes the active branch. In most cases, this contains the local development, though that is purely by convention and is not required.

  默认的开发[分支](https://git-scm.com/docs/user-manual#def_branch)。每当创建一个 Git [仓库](https://git-scm.com/docs/user-manual#def_repository) 时，都会创建一个名为 "master" 的分支，并成为活动分支。在大多数情况下，这包含本地开发，尽管这仅仅是惯例，并不是必需的。

- merge - 合并

  As a verb: To bring the contents of another [branch](https://git-scm.com/docs/user-manual#def_branch) (possibly from an external [repository](https://git-scm.com/docs/user-manual#def_repository)) into the current branch. In the case where the merged-in branch is from a different repository, this is done by first [fetching](https://git-scm.com/docs/user-manual#def_fetch) the remote branch and then merging the result into the current branch. This combination of fetch and merge operations is called a [pull](https://git-scm.com/docs/user-manual#def_pull). Merging is performed by an automatic process that identifies changes made since the branches diverged, and then applies all those changes together. In cases where changes conflict, manual intervention may be required to complete the merge.As a noun: unless it is a [fast-forward](https://git-scm.com/docs/user-manual#def_fast_forward), a successful merge results in the creation of a new [commit](https://git-scm.com/docs/user-manual#def_commit) representing the result of the merge, and having as [parents](https://git-scm.com/docs/user-manual#def_parent) the tips of the merged [branches](https://git-scm.com/docs/user-manual#def_branch). This commit is referred to as a "merge commit", or sometimes just a "merge".

  作为动词：将另一个[分支](https://git-scm.com/docs/user-manual#def_branch)（可能来自外部[仓库](https://git-scm.com/docs/user-manual#def_repository)）的内容合并到当前分支。如果要合并的分支来自不同的仓库，首先通过[获取](https://git-scm.com/docs/user-manual#def_fetch)远程分支，然后将结果合并到当前分支。这种获取和合并操作的组合被称为 [pull](https://git-scm.com/docs/user-manual#def_pull)。合并是由自动过程执行的，该过程识别自分叉以来所做的更改，然后将所有这些更改一起应用。在冲突的情况下，可能需要手动干预以完成合并。作为名词：除非是 [快速向前](https://git-scm.com/docs/user-manual#def_fast_forward)，否则成功的合并将创建一个新的[提交](https://git-scm.com/docs/user-manual#def_commit)作为当前[分支](https://git-scm.com/docs/user-manual#def_branch)的顶部，该提交将两个合并的分支的内容合并到一个新的提交中。

- object

  The unit of storage in Git. It is uniquely identified by the [SHA-1](https://git-scm.com/docs/user-manual#def_SHA1) of its contents. Consequently, an object cannot be changed.

  Git中存储的存储单元。它通过其内容的[SHA-1](https://git-scm.com/docs/user-manual#def_SHA1)唯一标识。因此，对象是不可更改的。

- object database

  Stores a set of "objects", and an individual [object](https://git-scm.com/docs/user-manual#def_object) is identified by its [object name](https://git-scm.com/docs/user-manual#def_object_name). The objects usually live in `$GIT_DIR/objects/`.

  存储一组"对象"，每个[对象](https://git-scm.com/docs/user-manual#def_object)由其[对象名称](https://git-scm.com/docs/user-manual#def_object_name)唯一标识。这些对象通常存储在`$GIT_DIR/objects/`目录中。

- object identifier (oid)

  Synonym for [object name](https://git-scm.com/docs/user-manual#def_object_name).

  [对象名称](https://git-scm.com/docs/user-manual#def_object_name)的同义词。

- object name

  The unique identifier of an [object](https://git-scm.com/docs/user-manual#def_object). The object name is usually represented by a 40 character hexadecimal string. Also colloquially called [SHA-1](https://git-scm.com/docs/user-manual#def_SHA1).

  [对象](https://git-scm.com/docs/user-manual#def_object)的唯一标识符。对象名称通常由40个字符的十六进制字符串表示。也常称为[SHA-1](https://git-scm.com/docs/user-manual#def_SHA1)。

- object type

  One of the identifiers "[commit](https://git-scm.com/docs/user-manual#def_commit_object)", "[tree](https://git-scm.com/docs/user-manual#def_tree_object)", "[tag](https://git-scm.com/docs/user-manual#def_tag_object)" or "[blob](https://git-scm.com/docs/user-manual#def_blob_object)" describing the type of an [object](https://git-scm.com/docs/user-manual#def_object).

  [对象](https://git-scm.com/docs/user-manual#def_object)的标识符之一，包括"[commit](https://git-scm.com/docs/user-manual#def_commit_object)"、"[tree](https://git-scm.com/docs/user-manual#def_tree_object)"、"[tag](https://git-scm.com/docs/user-manual#def_tag_object)"或"[blob](https://git-scm.com/docs/user-manual#def_blob_object)"。

- octopus

  To [merge](https://git-scm.com/docs/user-manual#def_merge) more than two [branches](https://git-scm.com/docs/user-manual#def_branch).

  合并多个[分支](https://git-scm.com/docs/user-manual#def_branch)的过程。

- origin

  The default upstream [repository](https://git-scm.com/docs/user-manual#def_repository). Most projects have at least one upstream project which they track. By default *origin* is used for that purpose. New upstream updates will be fetched into [remote-tracking branches](https://git-scm.com/docs/user-manual#def_remote_tracking_branch) named origin/name-of-upstream-branch, which you can see using `git branch -r`.

  默认的上游[仓库](https://git-scm.com/docs/user-manual#def_repository)。大多数项目至少有一个上游项目进行跟踪。默认情况下，*origin* 被用于此目的。新的上游更新将被获取到名为 origin/name-of-upstream-branch 的[远程跟踪分支](https://git-scm.com/docs/user-manual#def_remote_tracking_branch)，您可以使用 `git branch -r` 查看这些分支。

- overlay

  Only update and add files to the working directory, but don’t delete them, similar to how *cp -R* would update the contents in the destination directory. This is the default mode in a [checkout](https://git-scm.com/docs/user-manual#def_checkout) when checking out files from the [index](https://git-scm.com/docs/user-manual#def_index) or a [tree-ish](https://git-scm.com/docs/user-manual#def_tree-ish). In contrast, no-overlay mode also deletes tracked files not present in the source, similar to *rsync --delete*.

  仅更新和添加工作目录中的文件，但不删除它们，类似于如何使用 *cp -R* 更新目标目录中的内容。这是在[checkout](https://git-scm.com/docs/user-manual#def_checkout)时默认的模式，用于从[index](https://git-scm.com/docs/user-manual#def_index)或[树状对象](https://git-scm.com/docs/user-manual#def_tree-ish)检出文件。相比之下，no-overlay模式也会删除源中不存在的已跟踪文件，类似于 *rsync --delete*。

- pack

  A set of objects which have been compressed into one file (to save space or to transmit them efficiently).

  一组已被压缩成一个文件的对象（以节省空间或高效传输）。

- pack index

  The list of identifiers, and other information, of the objects in a [pack](https://git-scm.com/docs/user-manual#def_pack), to assist in efficiently accessing the contents of a pack.

  位于[pack](https://git-scm.com/docs/user-manual#def_pack)中的对象的标识符和其他信息，以便有效地访问pack的内容。

- pathspec

  Pattern used to limit paths in Git commands.

  路径模式，用于限制 Git 命令中的路径。

  Pathspecs are used on the command line of "git ls-files", "git ls-tree", "git add", "git grep", "git diff", "git checkout", and many other commands to limit the scope of operations to some subset of the tree or working tree. See the documentation of each command for whether paths are relative to the current directory or toplevel. The pathspec syntax is as follows:

  在 "git ls-files"、"git ls-tree"、"git add"、"git grep"、"git diff"、"git checkout" 和许多其他命令的命令行上使用路径模式，以将操作范围限制为树或工作树的某个子集。请参阅每个命令的文档，了解路径是否相对于当前目录或顶层目录。路径模式的语法如下：

  - any path matches itself
  - 任何路径与其本身匹配。
  - the pathspec up to the last slash represents a directory prefix. The scope of that pathspec is limited to that subtree.
  - 直到最后一个斜杠的路径模式表示一个目录前缀。该路径模式的范围限制为该子树。
  - the rest of the pathspec is a pattern for the remainder of the pathname. Paths relative to the directory prefix will be matched against that pattern using fnmatch(3); in particular, *** and *?* *can* match directory separators.
  - 路径模式的其余部分是剩余路径名的模式。相对于目录前缀的路径将使用 fnmatch(3) 与该模式匹配；特别地，*** 和 *?* *可以*匹配目录分隔符。

  For example, Documentation/*.jpg will match all .jpg files in the Documentation subtree, including Documentation/chapter_1/figure_1.jpg.

  例如，`Documentation/*.jpg` 将匹配 Documentation 子树中的所有 `.jpg` 文件，包括 `Documentation/chapter_1/figure_1.jpg`。

  A pathspec that begins with a colon `:` has special meaning. In the short form, the leading colon `:` is followed by zero or more "magic signature" letters (which optionally is terminated by another colon `:`), and the remainder is the pattern to match against the path. The "magic signature" consists of ASCII symbols that are neither alphanumeric, glob, regex special characters nor colon. The optional colon that terminates the "magic signature" can be omitted if the pattern begins with a character that does not belong to "magic signature" symbol set and is not a colon.

  以冒号 `:` 开头的路径模式具有特殊含义。在短形式中，冒号 `:` 后跟零个或多个 "magic signature" 字母（可选择由另一个冒号 `:` 终止），其余部分是要与路径匹配的模式。"magic signature" 由既不是字母数字字符、glob、正则表达式特殊字符、也不是冒号的 ASCII 符号组成。如果模式以不属于 "magic signature" 符号集且不是冒号的字符开头，则可以省略终止 "magic signature" 的可选冒号。

  In the long form, the leading colon `:` is followed by an open parenthesis `(`, a comma-separated list of zero or more "magic words", and a close parentheses `)`, and the remainder is the pattern to match against the path.

  在长形式中，冒号 `:` 后跟一个开括号 `(`，一个由逗号分隔的零个或多个 "magic words" 列表，以及一个闭括号 `)`，其余部分是要与路径匹配的模式。

  A pathspec with only a colon means "there is no pathspec". This form should not be combined with other pathspec.

  只有冒号的路径模式表示 "没有路径模式"。不应将此形式与其他路径模式组合使用。

  - top

    The magic word `top` (magic signature: `/`) makes the pattern match from the root of the working tree, even when you are running the command from inside a subdirectory.

    魔术词 `top`（魔术签名：`/`）使模式从工作树的根部匹配，即使你在子目录中运行命令。

  - literal

    Wildcards in the pattern such as `*` or `?` are treated as literal characters.

    模式中的通配符，如 `*` 或 `?`，被视为普通字符。

  - icase

    Case insensitive match.

    不区分大小写匹配。

  - glob

    Git treats the pattern as a shell glob suitable for consumption by fnmatch(3) with the FNM_PATHNAME flag: wildcards in the pattern will not match a / in the pathname. For example, "Documentation/*.html" matches "Documentation/git.html" but not "Documentation/ppc/ppc.html" or "tools/perf/Documentation/perf.html".Two consecutive asterisks ("`**`") in patterns matched against full pathname may have special meaning:A leading "`**`" followed by a slash means match in all directories. For example, "`**/foo`" matches file or directory "`foo`" anywhere, the same as pattern "`foo`". "`**/foo/bar`" matches file or directory "`bar`" anywhere that is directly under directory "`foo`".A trailing "`/**`" matches everything inside. For example, "`abc/**`" matches all files inside directory "abc", relative to the location of the `.gitignore` file, with infinite depth.A slash followed by two consecutive asterisks then a slash matches zero or more directories. For example, "`a/**/b`" matches "`a/b`", "`a/x/b`", "`a/x/y/b`" and so on.Other consecutive asterisks are considered invalid.Glob magic is incompatible with literal magic.

    Git将模式视为适用于 fnmatch(3) 的shell glob，并带有FNM_PATHNAME标志：模式中的通配符将不匹配路径名中的斜杠。例如，"Documentation/*.html" 匹配 "Documentation/git.html"，但不匹配 "Documentation/ppc/ppc.html" 或 "tools/perf/Documentation/perf.html"。在完整路径名上匹配的连续两个星号（"`**`"）可能具有特殊含义：以斜杠开头的 "`**`" 表示匹配所有目录。例如，"`**/foo`" 匹配任何位置的文件或目录 "`foo`"，与模式 "`foo`" 相同。"`**/foo/bar`" 匹配任何直接位于目录 "`foo`" 下的位置的文件或目录 "`bar`"。尽管有其他连续的星号，但它们被视为无效。Glob魔术与Literal魔术不兼容。

  - attr

    After `attr:` comes a space separated list of "attribute requirements", all of which must be met in order for the path to be considered a match; this is in addition to the usual non-magic pathspec pattern matching. See [gitattributes[5]](https://git-scm.com/docs/gitattributes).Each of the attribute requirements for the path takes one of these forms:"`ATTR`" requires that the attribute `ATTR` be set."`-ATTR`" requires that the attribute `ATTR` be unset."`ATTR=VALUE`" requires that the attribute `ATTR` be set to the string `VALUE`."`!ATTR`" requires that the attribute `ATTR` be unspecified.Note that when matching against a tree object, attributes are still obtained from working tree, not from the given tree object.

    在 `attr:` 后面是一个用空格分隔的 "属性要求" 列表，所有这些要求都必须满足，以便将路径视为匹配；这是除了通常的非魔术路径规范模式匹配之外的附加要求。请参阅 [gitattributes[5]](https://git-scm.com/docs/gitattributes)。对于路径的每个属性要求，可以采用以下形式："`ATTR`" 要求设置属性 `ATTR`。"`-ATTR`" 要求未设置属性 `ATTR`。"`ATTR=VALUE`" 要求属性 `ATTR` 设置为字符串 `VALUE`。"`!ATTR`" 要求属性 `ATTR` 未指定。请注意，当匹配树对象时，属性仍从工作树获取，而不是从给定的树对象获取。

  - exclude

    After a path matches any non-exclude pathspec, it will be run through all exclude pathspecs (magic signature: `!` or its synonym `^`). If it matches, the path is ignored. When there is no non-exclude pathspec, the exclusion is applied to the result set as if invoked without any pathspec.

    在路径匹配任何非排除路径规范后，它将通过所有排除路径规范（魔术签名：`!` 或其同义词 `^`）。如果匹配，则路径将被忽略。当没有非排除路径规范时，排除将应用于结果集，就好像没有使用任何路径规范。

- parent

  A [commit object](https://git-scm.com/docs/user-manual#def_commit_object) contains a (possibly empty) list of the logical predecessor(s) in the line of development, i.e. its parents.

  [提交对象](https://git-scm.com/docs/user-manual#def_commit_object) 包含（可能为空的）逻辑前任（即其父项）列表。

- pickaxe

  The term [pickaxe](https://git-scm.com/docs/user-manual#def_pickaxe) refers to an option to the diffcore routines that help select changes that add or delete a given text string. With the `--pickaxe-all` option, it can be used to view the full [changeset](https://git-scm.com/docs/user-manual#def_changeset) that introduced or removed, say, a particular line of text. See [git-diff[1]](../1/git-diff).

  术语 "pickaxe" 是 diffcore 程序的一个选项，它帮助选择添加或删除给定文本字符串的更改。通过 `--pickaxe-all` 选项，它可以用于查看引入或删除特定文本行的完整 [changeset](https://git-scm.com/docs/user-manual#def_changeset)。参见 [git-diff[1]](../1/git-diff)。

- plumbing

  Cute name for [core Git](https://git-scm.com/docs/user-manual#def_core_git).

  "plumbing" 是 [核心 Git](https://git-scm.com/docs/user-manual#def_core_git) 的可爱名称。

- porcelain

  Cute name for programs and program suites depending on [core Git](https://git-scm.com/docs/user-manual#def_core_git), presenting a high level access to core Git. Porcelains expose more of a [SCM](https://git-scm.com/docs/user-manual#def_SCM) interface than the [plumbing](https://git-scm.com/docs/user-manual#def_plumbing).

  "porcelain" 是依赖于 [核心 Git](https://git-scm.com/docs/user-manual#def_core_git) 的程序和程序套件的可爱名称，它提供对核心 Git 的高级访问。Porcelain 暴露比 [plumbing](https://git-scm.com/docs/user-manual#def_plumbing) 更多的 [SCM](https://git-scm.com/docs/user-manual#def_SCM) 接口。

- per-worktree ref

  Refs that are per-[worktree](https://git-scm.com/docs/user-manual#def_worktree), rather than global. This is presently only [HEAD](https://git-scm.com/docs/user-manual#def_HEAD) and any refs that start with `refs/bisect/`, but might later include other unusual refs.

  "per-worktree ref" 是指每个 [工作树](https://git-scm.com/docs/user-manual#def_worktree) 的引用，而不是全局引用。目前只有 [HEAD](https://git-scm.com/docs/user-manual#def_HEAD) 和以 `refs/bisect/` 开头的任何引用属于 per-worktree ref，但以后可能包括其他异常引用。

- pseudoref

  Pseudorefs are a class of files under `$GIT_DIR` which behave like refs for the purposes of rev-parse, but which are treated specially by git. Pseudorefs both have names that are all-caps, and always start with a line consisting of a [SHA-1](https://git-scm.com/docs/user-manual#def_SHA1) followed by whitespace. So, HEAD is not a pseudoref, because it is sometimes a symbolic ref. They might optionally contain some additional data. `MERGE_HEAD` and `CHERRY_PICK_HEAD` are examples. Unlike [per-worktree refs](https://git-scm.com/docs/user-manual#def_per_worktree_ref), these files cannot be symbolic refs, and never have reflogs. They also cannot be updated through the normal ref update machinery. Instead, they are updated by directly writing to the files. However, they can be read as if they were refs, so `git rev-parse MERGE_HEAD` will work.

  伪引用是 `$GIT_DIR` 下的文件类，对于 rev-parse 来说它们就像引用一样，但是在 Git 中它们被特殊处理。伪引用的名称全是大写字母，并且总是以包含 [SHA-1](https://git-scm.com/docs/user-manual#def_SHA1) 的一行开始，后面跟着空格。因此，HEAD 不是伪引用，因为它有时是符号引用。它们可能还包含一些附加数据。`MERGE_HEAD` 和 `CHERRY_PICK_HEAD` 就是例子。与 [per-worktree 引用](https://git-scm.com/docs/user-manual#def_per_worktree_ref) 不同，这些文件不能是符号引用，并且没有引用日志。它们也不能通过正常的引用更新机制更新。相反，它们通过直接写入文件来更新。然而，它们可以被读取，就像它们是引用一样，所以 `git rev-parse MERGE_HEAD` 将正常工作。

- pull

  Pulling a [branch](https://git-scm.com/docs/user-manual#def_branch) means to [fetch](https://git-scm.com/docs/user-manual#def_fetch) it and [merge](https://git-scm.com/docs/user-manual#def_merge) it. See also [git-pull[1]](../1/git-pull).

  "pull" 一个 [分支](https://git-scm.com/docs/user-manual#def_branch) 意味着将其 [fetch](https://git-scm.com/docs/user-manual#def_fetch) 并 [merge](https://git-scm.com/docs/user-manual#def_merge)。参见 [git-pull[1]](../1/git-pull)。

- push

  Pushing a [branch](https://git-scm.com/docs/user-manual#def_branch) means to get the branch’s [head ref](https://git-scm.com/docs/user-manual#def_head_ref) from a remote [repository](https://git-scm.com/docs/user-manual#def_repository), find out if it is an ancestor to the branch’s local head ref, and in that case, putting all objects, which are [reachable](https://git-scm.com/docs/user-manual#def_reachable) from the local head ref, and which are missing from the remote repository, into the remote [object database](https://git-scm.com/docs/user-manual#def_object_database), and updating the remote head ref. If the remote [head](https://git-scm.com/docs/user-manual#def_head) is not an ancestor to the local head, the push fails.

  "push" 一个 [分支](https://git-scm.com/docs/user-manual#def_branch) 意味着从远程 [仓库](https://git-scm.com/docs/user-manual#def_repository) 获取该分支的 [head ref](https://git-scm.com/docs/user-manual#def_head_ref)，查看它是否是分支的本地 head ref 的祖先，如果是，则将所有从本地 head ref 可达的且远程仓库中缺失的对象放入远程 [对象数据库](https://git-scm.com/docs/user-manual#def_object_database)，并更新远程 head ref。如果远程 [head](https://git-scm.com/docs/user-manual#def_head) 不是本地 head 的祖先，则推送失败。

- reachable

  All of the ancestors of a given [commit](https://git-scm.com/docs/user-manual#def_commit) are said to be "reachable" from that commit. More generally, one [object](https://git-scm.com/docs/user-manual#def_object) is reachable from another if we can reach the one from the other by a [chain](https://git-scm.com/docs/user-manual#def_chain) that follows [tags](https://git-scm.com/docs/user-manual#def_tag) to whatever they tag, [commits](https://git-scm.com/docs/user-manual#def_commit_object) to their parents or trees, and [trees](https://git-scm.com/docs/user-manual#def_tree_object) to the trees or [blobs](https://git-scm.com/docs/user-manual#def_blob_object) that they contain.

  给定 [提交](https://git-scm.com/docs/user-manual#def_commit) 的所有祖先都被称为 "可达" 于该提交。更一般地，一个 [对象](https://git-scm.com/docs/user-manual#def_object) 从另一个对象是 "可达" 的，如果我们可以通过 [链](https://git-scm.com/docs/user-manual#def_chain) 到达一个对象从而到达另一个对象，该链遵循 [tags](https://git-scm.com/docs/user-manual#def_tag) 到它们标记的任何内容，[commits](https://git-scm.com/docs/user-manual#def_commit_object) 到它们的父项或树，以及 [trees](https://git-scm.com/docs/user-manual#def_tree_object) 到它们包含的树或 [blobs](https://git-scm.com/docs/user-manual#def_blob_object)。

- reachability bitmaps

  Reachability bitmaps store information about the [reachability](https://git-scm.com/docs/user-manual#def_reachable) of a selected set of commits in a packfile, or a multi-pack index (MIDX), to speed up object search. The bitmaps are stored in a ".bitmap" file. A repository may have at most one bitmap file in use. The bitmap file may belong to either one pack, or the repository’s multi-pack index (if it exists).

  可达性位图存储有关包文件或多包索引（MIDX）中所选一组提交的可达性信息，以加速对象搜索。位图存储在 ".bitmap" 文件中。一个仓库最多只能有一个位图文件。位图文件可以属于一个包，也可以属于仓库的多包索引（如果存在的话）。

- rebase

  To reapply a series of changes from a [branch](https://git-scm.com/docs/user-manual#def_branch) to a different base, and reset the [head](https://git-scm.com/docs/user-manual#def_head) of that branch to the result.

  重新应用一系列更改，从一个 [分支](https://git-scm.com/docs/user-manual#def_branch) 到另一个基准，并将该分支的 [head](https://git-scm.com/docs/user-manual#def_head) 重置为结果。

- ref

  A name that begins with `refs/` (e.g. `refs/heads/master`) that points to an [object name](https://git-scm.com/docs/user-manual#def_object_name) or another ref (the latter is called a [symbolic ref](https://git-scm.com/docs/user-manual#def_symref)). For convenience, a ref can sometimes be abbreviated when used as an argument to a Git command; see [gitrevisions[7]](../7/gitrevisions) for details. Refs are stored in the [repository](https://git-scm.com/docs/user-manual#def_repository).The ref namespace is hierarchical. Different subhierarchies are used for different purposes (e.g. the `refs/heads/` hierarchy is used to represent local branches).There are a few special-purpose refs that do not begin with `refs/`. The most notable example is `HEAD`.

  以 `refs/` 开头的名称（例如 `refs/heads/master`），指向对象名称或另一个引用（后者称为 [symbolic ref](https://git-scm.com/docs/user-manual#def_symref)）。为了方便起见，在 Git 命令的参数中使用引用时，有时可以缩写；有关详情，请参阅 [gitrevisions[7]](../7/gitrevisions)。引用存储在 [仓库](https://git-scm.com/docs/user-manual#def_repository) 中。引用命名空间是分层的。不同的子层次结构用于不同的目的（例如，`refs/heads/` 层次结构用于表示本地分支）。有一些特殊用途的引用不以 `refs/` 开头。最显著的例子是 `HEAD`。

- reflog

  A reflog shows the local "history" of a ref. In other words, it can tell you what the 3rd last revision in *this* repository was, and what was the current state in *this* repository, yesterday 9:14pm. See [git-reflog[1]](../1/git-reflog) for details.

  引用日志显示引用的本地历史。换句话说，它可以告诉您在 *此* 仓库中的倒数第三个修订是什么，以及昨天下午9:14的当前状态是什么。有关详情，请参阅 [git-reflog[1]](../1/git-reflog)。

- refspec

  A "refspec" is used by [fetch](https://git-scm.com/docs/user-manual#def_fetch) and [push](https://git-scm.com/docs/user-manual#def_push) to describe the mapping between remote [ref](https://git-scm.com/docs/user-manual#def_ref) and local ref.

  "refspec" 用于 [fetch](https://git-scm.com/docs/user-manual#def_fetch) 和 [push](https://git-scm.com/docs/user-manual#def_push) 来描述远程引用和本地引用之间的映射。

- remote repository

  A [repository](https://git-scm.com/docs/user-manual#def_repository) which is used to track the same project but resides somewhere else. To communicate with remotes, see [fetch](https://git-scm.com/docs/user-manual#def_fetch) or [push](https://git-scm.com/docs/user-manual#def_push).

  一个 [仓库](https://git-scm.com/docs/user-manual#def_repository)，用于跟踪相同的项目，但位于其他位置。要与远程进行通信，请参阅 [fetch](https://git-scm.com/docs/user-manual#def_fetch) 或 [push](https://git-scm.com/docs/user-manual#def_push)。

- remote-tracking branch

  A [ref](https://git-scm.com/docs/user-manual#def_ref) that is used to follow changes from another [repository](https://git-scm.com/docs/user-manual#def_repository). It typically looks like *refs/remotes/foo/bar* (indicating that it tracks a branch named *bar* in a remote named *foo*), and matches the right-hand-side of a configured fetch [refspec](https://git-scm.com/docs/user-manual#def_refspec). A remote-tracking branch should not contain direct modifications or have local commits made to it.

  用于跟踪另一个 [仓库](https://git-scm.com/docs/user-manual#def_repository) 中的更改的 [引用](https://git-scm.com/docs/user-manual#def_ref)。它通常看起来像 *refs/remotes/foo/bar*（表示它跟踪名为 *bar* 的远程名为 *foo* 的分支），并与配置的 fetch [refspec](https://git-scm.com/docs/user-manual#def_refspec) 的右侧匹配。远程跟踪分支不应包含直接的修改，也不应该有针对它的本地提交。

- repository

  A collection of [refs](https://git-scm.com/docs/user-manual#def_ref) together with an [object database](https://git-scm.com/docs/user-manual#def_object_database) containing all objects which are [reachable](https://git-scm.com/docs/user-manual#def_reachable) from the refs, possibly accompanied by meta data from one or more [porcelains](https://git-scm.com/docs/user-manual#def_porcelain). A repository can share an object database with other repositories via [alternates mechanism](https://git-scm.com/docs/user-manual#def_alternate_object_database).

  包含所有 [引用](https://git-scm.com/docs/user-manual#def_ref) 以及可从引用访问的所有对象的[对象数据库](https://git-scm.com/docs/user-manual#def_object_database)的集合，可能还包含来自一个或多个 [porcelain](https://git-scm.com/docs/user-manual#def_porcelain) 的元数据。一个仓库可以通过 [alternates 机制](https://git-scm.com/docs/user-manual#def_alternate_object_database)与其他仓库共享一个对象数据库。

- resolve

  The action of fixing up manually what a failed automatic [merge](https://git-scm.com/docs/user-manual#def_merge) left behind.

  手动修复失败的自动 [合并](https://git-scm.com/docs/user-manual#def_merge) 留下的问题。

- revision

  Synonym for [commit](https://git-scm.com/docs/user-manual#def_commit) (the noun).

  [提交](https://git-scm.com/docs/user-manual#def_commit)（名词）的同义词。

- rewind

  To throw away part of the development, i.e. to assign the [head](https://git-scm.com/docs/user-manual#def_head) to an earlier [revision](https://git-scm.com/docs/user-manual#def_revision).

  抛弃开发的一部分，即将 [head](https://git-scm.com/docs/user-manual#def_head) 分配给一个较早的 [revision](https://git-scm.com/docs/user-manual#def_revision)。

- SCM

  Source code management (tool).

  源代码管理（工具）。

- SHA-1

  "Secure Hash Algorithm 1"; a cryptographic hash function. In the context of Git used as a synonym for [object name](https://git-scm.com/docs/user-manual#def_object_name).

  "Secure Hash Algorithm 1"，一种加密哈希函数。在 Git 的上下文中，它被用作 [对象名称](https://git-scm.com/docs/user-manual#def_object_name) 的同义词。

- shallow clone

  Mostly a synonym to [shallow repository](https://git-scm.com/docs/user-manual#def_shallow_repository) but the phrase makes it more explicit that it was created by running `git clone --depth=...` command.

  大多数情况下，是 [shallow repository](https://git-scm.com/docs/user-manual#def_shallow_repository) 的同义词，但这个短语更明确地说明它是通过运行 `git clone --depth=...` 命令创建的。

- shallow repository

  A shallow [repository](https://git-scm.com/docs/user-manual#def_repository) has an incomplete history some of whose [commits](https://git-scm.com/docs/user-manual#def_commit) have [parents](https://git-scm.com/docs/user-manual#def_parent) cauterized away (in other words, Git is told to pretend that these commits do not have the parents, even though they are recorded in the [commit object](https://git-scm.com/docs/user-manual#def_commit_object)). This is sometimes useful when you are interested only in the recent history of a project even though the real history recorded in the upstream is much larger. A shallow repository is created by giving the `--depth` option to [git-clone[1]](../1/git-clone), and its history can be later deepened with [git-fetch[1]](../1/git-fetch).

  浅 [仓库](https://git-scm.com/docs/user-manual#def_repository) 具有不完整的历史，其中部分 [提交](https://git-scm.com/docs/user-manual#def_commit) 的 [parents](https://git-scm.com/docs/user-manual#def_parent) 被切断了（换句话说，Git 被告知这些提交没有父提交，即使它们在 [commit object](https://git-scm.com/docs/user-manual#def_commit_object) 中记录）。这在您仅对项目的最近历史感兴趣时很有用，即使上游记录的真实历史要大得多。通过给 [git-clone[1]](../1/git-clone) 提供 `--depth` 选项可以创建浅仓库，其历史可以在之后使用 [git-fetch[1]](../1/git-fetch) 扩展。

- stash entry

  An [object](https://git-scm.com/docs/user-manual#def_object) used to temporarily store the contents of a [dirty](https://git-scm.com/docs/user-manual#def_dirty) working directory and the index for future reuse.

  用于临时存储脏的工作目录和索引内容以供将来重用的 [object](https://git-scm.com/docs/user-manual#def_object)。

- submodule

  A [repository](https://git-scm.com/docs/user-manual#def_repository) that holds the history of a separate project inside another repository (the latter of which is called [superproject](https://git-scm.com/docs/user-manual#def_superproject)).

  在另一个仓库（后者称为 [superproject](https://git-scm.com/docs/user-manual#def_superproject)）内部持有单独项目的历史的 [仓库](https://git-scm.com/docs/user-manual#def_repository)。

- superproject

  A [repository](https://git-scm.com/docs/user-manual#def_repository) that references repositories of other projects in its working tree as [submodules](https://git-scm.com/docs/user-manual#def_submodule). The superproject knows about the names of (but does not hold copies of) commit objects of the contained submodules.

  引用其工作树中的其他项目的仓库（称为 [submodules](https://git-scm.com/docs/user-manual#def_submodule)）。超级项目了解所包含子模块的 [commit](https://git-scm.com/docs/user-manual#def_commit_object) 名称（但不持有副本）。

- symref

  Symbolic reference: instead of containing the [SHA-1](https://git-scm.com/docs/user-manual#def_SHA1) id itself, it is of the format *ref: refs/some/thing* and when referenced, it recursively dereferences to this reference. *[HEAD](https://git-scm.com/docs/user-manual#def_HEAD)* is a prime example of a symref. Symbolic references are manipulated with the [git-symbolic-ref[1]](../1/git-symbolic-ref) command.

  符号引用：它不包含 [SHA-1](https://git-scm.com/docs/user-manual#def_SHA1) id 本身，而是具有 *ref: refs/some/thing* 格式，并且在引用时递归地解引用到该引用。*[HEAD](https://git-scm.com/docs/user-manual#def_HEAD)* 就是符号引用的一个典型例子。符号引用可以使用 [git-symbolic-ref[1]](../1/git-symbolic-ref) 命令来操作。

- tag

  A [ref](https://git-scm.com/docs/user-manual#def_ref) under `refs/tags/` namespace that points to an object of an arbitrary type (typically a tag points to either a [tag](https://git-scm.com/docs/user-manual#def_tag_object) or a [commit object](https://git-scm.com/docs/user-manual#def_commit_object)). In contrast to a [head](https://git-scm.com/docs/user-manual#def_head), a tag is not updated by the `commit` command. A Git tag has nothing to do with a Lisp tag (which would be called an [object type](https://git-scm.com/docs/user-manual#def_object_type) in Git’s context). A tag is most typically used to mark a particular point in the commit ancestry [chain](https://git-scm.com/docs/user-manual#def_chain).

  `refs/tags/` 命名空间下的 [ref](https://git-scm.com/docs/user-manual#def_ref)，指向任意类型的对象（通常标签指向 [tag](https://git-scm.com/docs/user-manual#def_tag_object) 或 [commit object](https://git-scm.com/docs/user-manual#def_commit_object)）。与 [head](https://git-scm.com/docs/user-manual#def_head) 不同，标签不会由 `commit` 命令更新。Git 标签与 Lisp 标签（在 Git 上下文中称为 [object type](https://git-scm.com/docs/user-manual#def_object_type)）无关。标签最常用于标记提交历史 [链](https://git-scm.com/docs/user-manual#def_chain) 中的特定点。

- tag object

  An [object](https://git-scm.com/docs/user-manual#def_object) containing a [ref](https://git-scm.com/docs/user-manual#def_ref) pointing to another object, which can contain a message just like a [commit object](https://git-scm.com/docs/user-manual#def_commit_object). It can also contain a (PGP) signature, in which case it is called a "signed tag object".

  一个包含指向另一个对象的 [ref](https://git-scm.com/docs/user-manual#def_ref) 的[对象](https://git-scm.com/docs/user-manual#def_object)，它可以像[提交对象](https://git-scm.com/docs/user-manual#def_commit_object)一样包含消息。如果还包含 (PGP) 签名，则称为 "signed tag object"（签名标签对象）。

- topic branch

  A regular Git [branch](https://git-scm.com/docs/user-manual#def_branch) that is used by a developer to identify a conceptual line of development. Since branches are very easy and inexpensive, it is often desirable to have several small branches that each contain very well defined concepts or small incremental yet related changes.

  开发人员用来标识一个概念性开发线的常规 Git [分支](https://git-scm.com/docs/user-manual#def_branch)。由于分支非常容易和廉价，通常希望有几个小分支，每个分支都包含非常明确定义的概念或相关的小增量更改。

- tree

  Either a [working tree](https://git-scm.com/docs/user-manual#def_working_tree), or a [tree object](https://git-scm.com/docs/user-manual#def_tree_object) together with the dependent [blob](https://git-scm.com/docs/user-manual#def_blob_object) and tree objects (i.e. a stored representation of a working tree).

  一个[工作树](https://git-scm.com/docs/user-manual#def_working_tree)，或者包含依赖的[对象](https://git-scm.com/docs/user-manual#def_object)和树对象的[tree object](https://git-scm.com/docs/user-manual#def_tree_object)（即工作树的存储表示）。

- tree object

  An [object](https://git-scm.com/docs/user-manual#def_object) containing a list of file names and modes along with refs to the associated blob and/or tree objects. A [tree](https://git-scm.com/docs/user-manual#def_tree) is equivalent to a [directory](https://git-scm.com/docs/user-manual#def_directory).

  一个包含文件名和模式列表以及与关联的 blob 和/或树对象的[ref](https://git-scm.com/docs/user-manual#def_ref)的[对象](https://git-scm.com/docs/user-manual#def_object)。树等同于[目录](https://git-scm.com/docs/user-manual#def_directory)。

- tree-ish (also treeish)

  A [tree object](https://git-scm.com/docs/user-manual#def_tree_object) or an [object](https://git-scm.com/docs/user-manual#def_object) that can be recursively dereferenced to a tree object. Dereferencing a [commit object](https://git-scm.com/docs/user-manual#def_commit_object) yields the tree object corresponding to the [revision](https://git-scm.com/docs/user-manual#def_revision)'s top [directory](https://git-scm.com/docs/user-manual#def_directory). The following are all tree-ishes: a [commit-ish](https://git-scm.com/docs/user-manual#def_commit-ish), a tree object, a [tag object](https://git-scm.com/docs/user-manual#def_tag_object) that points to a tree object, a tag object that points to a tag object that points to a tree object, etc.

  可以递归地解引用为树对象的[tree object](https://git-scm.com/docs/user-manual#def_tree_object)或[对象](https://git-scm.com/docs/user-manual#def_object)。解引用[提交对象](https://git-scm.com/docs/user-manual#def_commit_object)会产生与[revision](https://git-scm.com/docs/user-manual#def_revision)的顶级[目录](https://git-scm.com/docs/user-manual#def_directory)对应的树对象。以下都是 tree-ish：[commit-ish](https://git-scm.com/docs/user-manual#def_commit-ish)、树对象、指向树对象的[标签对象](https://git-scm.com/docs/user-manual#def_tag_object)、指向指向树对象的标签对象的标签对象、以此类推。

- unmerged index

  An [index](https://git-scm.com/docs/user-manual#def_index) which contains unmerged [index entries](https://git-scm.com/docs/user-manual#def_index_entry).

  包含未合并[索引条目](https://git-scm.com/docs/user-manual#def_index_entry)的[索引](https://git-scm.com/docs/user-manual#def_index)。

- unreachable object

  An [object](https://git-scm.com/docs/user-manual#def_object) which is not [reachable](https://git-scm.com/docs/user-manual#def_reachable) from a [branch](https://git-scm.com/docs/user-manual#def_branch), [tag](https://git-scm.com/docs/user-manual#def_tag), or any other reference.

  一个从[分支](https://git-scm.com/docs/user-manual#def_branch)、[标签](https://git-scm.com/docs/user-manual#def_tag)或任何其他引用无法[访问](https://git-scm.com/docs/user-manual#def_reachable)的[对象](https://git-scm.com/docs/user-manual#def_object)。

- upstream branch

  The default [branch](https://git-scm.com/docs/user-manual#def_branch) that is merged into the branch in question (or the branch in question is rebased onto). It is configured via branch.<name>.remote and branch.<name>.merge. If the upstream branch of *A* is *origin/B* sometimes we say "*A* is tracking *origin/B*".

  默认合并到所讨论的分支的[分支](https://git-scm.com/docs/user-manual#def_branch)（或所讨论的分支被变基到的分支）。通过 branch.<name>.remote 和 branch.<name>.merge 进行配置。如果 *A* 的上游分支是 *origin/B*，有时我们说 "*A* is tracking *origin/B*"。

- working tree

  The tree of actual checked out files. The working tree normally contains the contents of the [HEAD](https://git-scm.com/docs/user-manual#def_HEAD) commit’s tree, plus any local changes that you have made but not yet committed.

  实际签出文件的树。工作树通常包含[HEAD](https://git-scm.com/docs/user-manual#def_HEAD)提交的树内容，以及您进行的但尚未提交的本地更改。

- worktree

  A repository can have zero (i.e. bare repository) or one or more worktrees attached to it. One "worktree" consists of a "working tree" and repository metadata, most of which are shared among other worktrees of a single repository, and some of which are maintained separately per worktree (e.g. the index, HEAD and pseudorefs like MERGE_HEAD, per-worktree refs and per-worktree configuration file).
  
  一个仓库可以没有工作树（即裸仓库），也可以有一个或多个附加的工作树。一个 "工作树" 由 "工作树" 和仓库元数据组成，其中大部分元数据在单个仓库的其他工作树之间共享，而一些元数据在每个工作树之间单独维护（例如索引、HEAD 和伪引用，如 MERGE_HEAD、每个工作树的引用和每个工作树的配置文件）。

## 附录 A：Git 快速参考

This is a quick summary of the major commands; the previous chapters explain how these work in more detail.

​	这是主要命令的快速摘要；前面的章节更详细地解释了这些命令的工作原理。

### 创建一个新的仓库

From a tarball:

​	从一个 tarball 文件创建：

``` bash
$ tar xzf project.tar.gz
$ cd project
$ git init
Initialized empty Git repository in .git/
$ git add .
$ git commit
```

From a remote repository:

​	从一个远程仓库克隆：

``` bash
$ git clone git://example.com/pub/project.git
$ cd project
```

### 管理分支

``` bash
$ git branch			# list all local branches in this repo 列出该仓库中的所有本地分支
$ git switch test	    # switch working directory to branch "test" 切换到分支 "test"
$ git branch new		# create branch "new" starting at current 创建从当前 HEAD 开始的分支 "new"HEAD
$ git branch -d new		# delete branch "new" 删除分支 "new"
```

Instead of basing a new branch on current HEAD (the default), use:

​	不基于当前 HEAD（默认情况下）创建新分支，使用：

``` bash
$ git branch new test    # branch named "test" 以 "test" 为基础创建新分支
$ git branch new v2.6.15 # tag named v2.6.15 以标签 v2.6.15 为基础创建新分支
$ git branch new HEAD^   # commit before the most recent 以最近提交的前一次提交为基础
$ git branch new HEAD^^  # commit before that 以前一次提交的前一次提交为基础
$ git branch new test~10 # ten commits before tip of branch "test" 以分支 "test" 最新提交之前的 10 次提交为基础
```

Create and switch to a new branch at the same time:

​	同时创建并切换到新分支：

``` bash
$ git switch -c new v2.6.15
```

Update and examine branches from the repository you cloned from:

​	从克隆的仓库更新和查看分支：

``` bash
$ git fetch		# update 更新
$ git branch -r		# list 列出
  origin/master
  origin/next
  ...
$ git switch -c masterwork origin/master
```

Fetch a branch from a different repository, and give it a new name in your repository:

​	从不同的仓库获取一个分支，并在你的仓库中赋予它一个新名称：

``` bash
$ git fetch git://example.com/project.git theirbranch:mybranch
$ git fetch git://example.com/project.git v2.6.15:mybranch
```

Keep a list of repositories you work with regularly:

​	保持对你经常使用的仓库的列表：

``` bash
$ git remote add example git://example.com/project.git
$ git remote			# list remote repositories 列出远程仓库
example
origin
$ git remote show example	# get details 获取详细信息
* remote example
  URL: git://example.com/project.git
  Tracked remote branches
    master
    next
    ...
$ git fetch example		# update branches from example 从 example 更新分支
$ git branch -r			# list all remote branches 列出所有远程分支
```

### 探索历史

``` bash
$ gitk			    # visualize and browse history 可视化浏览历史记录
$ git log		    # list all commits 列出所有提交记录
$ git log src/		    # ...modifying src/ ...列出修改 src/ 的提交
$ git log v2.6.15..v2.6.16  # ...in v2.6.16, not in v2.6.15  ...列出在 v2.6.16 中，不在 v2.6.15 中的提交
$ git log master..test	    # ...in branch test, not in branch  master ...列出在分支 test 中，不在分支 master 中的提交
$ git log test..master	    # ...in branch master, but not in test ...列出在分支 master 中，不在分支 test 中的提交
$ git log test...master	    # ...in one branch, not in both ...列出在一个分支中，不在两个分支中的提交
$ git log -S'foo()'	    # ...where difference contain "foo()" ...差异包含 "foo()" 的提交
$ git log --since="2 weeks ago"
$ git log -p		    # show patches as well 显示补丁信息
$ git show		    # most recent commit 最近的提交
$ git diff v2.6.15..v2.6.16 # diff between two tagged versions 两个标签版本之间的差异
$ git diff v2.6.15..HEAD    # diff with current head 与当前 HEAD 的差异
$ git grep "foo()"	    # search working directory for "foo()" 在工作目录中搜索 "foo()"
$ git grep v2.6.15 "foo()"  # search old tree for "foo()" 在旧版本树中搜索 "foo()"
$ git show v2.6.15:a.txt    # look at old version of a.txt 查看 a.txt 的旧版本
```

Search for regressions:

​	寻找回归问题：

``` bash
$ git bisect start
$ git bisect bad		# current version is bad 当前版本是有问题的
$ git bisect good v2.6.13-rc2	# last known good revision 最近已知的好的版本
Bisecting: 675 revisions left to test after this
				# test here, then: 在这里进行测试，然后：
$ git bisect good		# if this revision is good, or 如果该版本是好的，或者
$ git bisect bad		# if this revision is bad. 如果该版本是有问题的。
				# repeat until done. 重复上述步骤直到完成。
```

### 进行更改

Make sure Git knows who to blame:

​	确保 Git 知道责任者是谁：

``` bash
$ cat >>~/.gitconfig <<\EOF
[user]
	name = Your Name Comes Here
	email = you@yourdomain.example.com
EOF
```

Select file contents to include in the next commit, then make the commit:

​	选择要包含在下一次提交中的文件内容，然后进行提交：

``` bash
$ git add a.txt    # updated file 更新的文件
$ git add b.txt    # new file 新文件
$ git rm c.txt     # old file 旧文件
$ git commit
```

Or, prepare and create the commit in one step:

​	或者，在一步中准备并创建提交：

``` bash
$ git commit d.txt # use latest content only of d.txt 仅使用 d.txt 的最新内容
$ git commit -a	   # use latest content of all tracked files 使用所有已跟踪文件的最新内容
```

### 合并

``` bash
$ git merge test   # merge branch "test" into the current branch 将分支 "test" 合并到当前分支
$ git pull git://example.com/project.git master
		   # fetch and merge in remote branch 从远程分支获取并合并
$ git pull . test  # equivalent to git merge test 等同于 git merge test
```

### 共享你的更改

Importing or exporting patches:

​	导入或导出补丁：

``` bash
$ git format-patch origin..HEAD # format a patch for each commit 为 HEAD 中的每个提交格式化一个补丁
				# in HEAD but not in origin 但不在 origin 中的提交
$ git am mbox # import patches from the mailbox "mbox" 从邮箱 "mbox" 导入补丁
```

Fetch a branch in a different Git repository, then merge into the current branch:

​	从不同的 Git 仓库获取一个分支，然后合并到当前分支：

``` bash
$ git pull git://example.com/project.git theirbranch
```

Store the fetched branch into a local branch before merging into the current branch:

​	将获取的分支存储到本地分支，然后合并到当前分支：

``` bash
$ git pull git://example.com/project.git theirbranch:mybranch
```

After creating commits on a local branch, update the remote branch with your commits:

​	在创建提交的本地分支后，用你的提交更新远程分支：

``` bash
$ git push ssh://example.com/project.git mybranch:theirbranch
```

When remote and local branch are both named "test":

​	当远程分支和本地分支都命名为 "test" 时：

``` bash
$ git push ssh://example.com/project.git test
```

Shortcut version for a frequently used remote repository:

​	常用远程仓库的快捷版本：

``` bash
$ git remote add example ssh://example.com/project.git
$ git push example test
```

### 仓库维护

Check for corruption:

​	检查是否存在损坏：

``` bash
$ git fsck
```

Recompress, remove unused cruft:

​	重新压缩，移除未使用的垃圾：

``` bash
$ git gc
```

## 附录 B：本手册的注释和 todo list

### Todo list

This is a work in progress.

​	本手册还在不断完善中。

The basic requirements:

​	基本要求：

- It must be readable in order, from beginning to end, by someone intelligent with a basic grasp of the UNIX command line, but without any special knowledge of Git. If necessary, any other prerequisites should be specifically mentioned as they arise.
- 必须能够被具有基本UNIX命令行知识的有智慧的人按顺序从头到尾阅读，而无需对Git有任何特殊了解。必要时，其他前提条件应在涉及的地方明确说明。
- Whenever possible, section headings should clearly describe the task they explain how to do, in language that requires no more knowledge than necessary: for example, "importing patches into a project" rather than "the `git am` command"
- 在可能的情况下，章节标题应明确描述它们所解释的任务，使用尽可能简单的语言，只需要必要的知识，例如，“将补丁导入项目”而不是“`git am`命令”。

Think about how to create a clear chapter dependency graph that will allow people to get to important topics without necessarily reading everything in between.

​	考虑如何创建一个清晰的章节依赖图，使人们能够在不必阅读中间所有内容的情况下获得重要的主题。

Scan `Documentation/` for other stuff left out; in particular:

​	扫描 `Documentation/` 以查找其他遗漏的内容；特别是：

- howto’s
- some of `technical/`?
- hooks
- list of commands in [git[1]](../1/git)

Scan email archives for other stuff left out

​	扫描电子邮件存档以查找其他遗漏的内容。

Scan man pages to see if any assume more background than this manual provides.

​	查看手册页面，看看是否有任何假设比本手册提供的背景知识更多。

Add more good examples. Entire sections of just cookbook examples might be a good idea; maybe make an "advanced examples" section a standard end-of-chapter section?

​	添加更多良好的示例。可以单独创建仅包含示例的章节，也许将“高级示例”作为标准的章节放在每个章节的末尾？

Include cross-references to the glossary, where appropriate.

​	在适当的地方添加对词汇表的交叉引用。

Add a section on working with other version control systems, including CVS, Subversion, and just imports of series of release tarballs.

​	添加关于与其他版本控制系统（包括CVS、Subversion以及一系列发布tarball导入）的合作的章节。

Write a chapter on using plumbing and writing scripts.

​	撰写关于使用plumbing和编写脚本的章节。

Alternates, clone -reference, etc.

​	涉及到Alternates、clone -reference等内容。

More on recovery from repository corruption. See: https://lore.kernel.org/git/Pine.LNX.4.64.0702272039540.12485@woody.linux-foundation.org/ https://lore.kernel.org/git/Pine.LNX.4.64.0702141033400.3604@woody.linux-foundation.org/

进一步介绍从仓库损坏中恢复。参见：

- [https://lore.kernel.org/git/Pine.LNX.4.64.0702272039540.12485@woody.linux-foundation.org/](https://lore.kernel.org/git/Pine.LNX.4.64.0702272039540.12485@woody.linux-foundation.org/ ) 

- [https://lore.kernel.org/git/Pine.LNX.4.64.0702141033400.3604@woody.linux-foundation.org/](https://lore.kernel.org/git/Pine.LNX.4.64.0702141033400.3604@woody.linux-foundation.org/)

