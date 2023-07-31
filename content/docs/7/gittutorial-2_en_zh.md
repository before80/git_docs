+++
title = "gittutorial-2——中英对照版"
weight = 30
type = "docs"
date = 2023-07-30T12:00:23+08:00
description = ""
isCJKLanguage = true
draft = false

+++

# gittutorial-2

https://git-scm.com/docs/gittutorial-2

version 2.41.0

## 名称

gittutorial-2 - A tutorial introduction to Git: part two

​	gittutorial-2 - Git入门教程：第二部分

## 概述

```
git *
```

## 描述

You should work through [gittutorial[7]](../../7/gittutorial) before reading this tutorial.

​	在阅读本教程之前，您应该先完成[gittutorial[7]](../../7/gittutorial)。

The goal of this tutorial is to introduce two fundamental pieces of Git’s architecture—the object database and the index file—and to provide the reader with everything necessary to understand the rest of the Git documentation.

​	本教程的目标是介绍Git架构的两个基本组成部分——对象数据库和索引文件，并为读者提供理解Git文档中其余内容所需的一切知识。

## Git对象数据库

Let’s start a new project and create a small amount of history:

​	让我们开始一个新项目并创建少量历史记录：

``` bash
$ mkdir test-project
$ cd test-project
$ git init
Initialized empty Git repository in .git/
$ echo 'hello world' > file.txt
$ git add .
$ git commit -a -m "initial commit"
[master (root-commit) 54196cc] initial commit
 1 file changed, 1 insertion(+)
 create mode 100644 file.txt
$ echo 'hello world!' >file.txt
$ git commit -a -m "add emphasis"
[master c4d59f3] add emphasis
 1 file changed, 1 insertion(+), 1 deletion(-)
```

What are the 7 digits of hex that Git responded to the commit with?

​	Git对提交做出的响应中有7个十六进制数字，这是什么含义？

We saw in part one of the tutorial that commits have names like this. It turns out that every object in the Git history is stored under a 40-digit hex name. That name is the SHA-1 hash of the object’s contents; among other things, this ensures that Git will never store the same data twice (since identical data is given an identical SHA-1 name), and that the contents of a Git object will never change (since that would change the object’s name as well). The 7 char hex strings here are simply the abbreviation of such 40 character long strings. Abbreviations can be used everywhere where the 40 character strings can be used, so long as they are unambiguous.

​	在本教程的第一部分中，我们已经了解了提交具有此类名称。事实证明，Git历史中的每个对象都存储在40个十六进制数字的名称下。该名称是对象内容的SHA-1哈希值；除其他外，这确保Git永远不会重复存储相同的数据（因为相同的数据被赋予相同的SHA-1名称），并且Git对象的内容永远不会更改（因为这将同时更改对象的名称）。这里的7个字符的十六进制字符串只是这些40个字符长字符串的缩写。可以在任何可以使用40个字符字符串的地方使用这些缩写，只要它们是无歧义的。

It is expected that the content of the commit object you created while following the example above generates a different SHA-1 hash than the one shown above because the commit object records the time when it was created and the name of the person performing the commit.

​	值得注意的是，由于提交对象记录了创建提交时的时间和执行提交的人的名称，所以根据上面的示例，您所创建的提交对象的内容预计会生成不同的SHA-1哈希值。

We can ask Git about this particular object with the `cat-file` command. Don’t copy the 40 hex digits from this example but use those from your own version. Note that you can shorten it to only a few characters to save yourself typing all 40 hex digits:

​	我们可以使用`cat-file`命令查询有关特定对象的信息。不要从此示例中复制40个十六进制数字，而是使用自己版本的数字。请注意，您可以缩短它以节省输入所有40个十六进制数字的时间：

``` bash
$ git cat-file -t 54196cc2
commit
$ git cat-file commit 54196cc2
tree 92b8b694ffb1675e5975148e1121810081dbdffe
author J. Bruce Fields <bfields@puzzle.fieldses.org> 1143414668 -0500
committer J. Bruce Fields <bfields@puzzle.fieldses.org> 1143414668 -0500

initial commit
```

A tree can refer to one or more "blob" objects, each corresponding to a file. In addition, a tree can also refer to other tree objects, thus creating a directory hierarchy. You can examine the contents of any tree using ls-tree (remember that a long enough initial portion of the SHA-1 will also work):

​	一个树可以引用一个或多个"blob"对象，每个对象对应一个文件。此外，一个树还可以引用其他树对象，从而创建一个目录层次结构。您可以使用`ls-tree`命令检查任何树的内容（记住足够长的SHA-1前缀也可以使用）：

``` bash
$ git ls-tree 92b8b694
100644 blob 3b18e512dba79e4c8300dd08aeb37f8e728b8dad    file.txt
```

Thus we see that this tree has one file in it. The SHA-1 hash is a reference to that file’s data:

​	因此，我们可以看到这个树包含一个文件。SHA-1哈希是对该文件数据的引用：

``` bash
$ git cat-file -t 3b18e512
blob
```

A "blob" is just file data, which we can also examine with cat-file:

​	"blob"只是文件数据，我们也可以使用`cat-file`查看它：

``` bash
$ git cat-file blob 3b18e512
hello world
```

Note that this is the old file data; so the object that Git named in its response to the initial tree was a tree with a snapshot of the directory state that was recorded by the first commit.

​	请注意，这是旧文件的数据；因此，Git在其对初始树的响应中命名的对象是具有目录状态快照的树，该树由第一个提交记录记录。

All of these objects are stored under their SHA-1 names inside the Git directory:

​	所有这些对象都存储在Git目录中的SHA-1名称下：

``` bash
$ find .git/objects/
.git/objects/
.git/objects/pack
.git/objects/info
.git/objects/3b
.git/objects/3b/18e512dba79e4c8300dd08aeb37f8e728b8dad
.git/objects/92
.git/objects/92/b8b694ffb1675e5975148e1121810081dbdffe
.git/objects/54
.git/objects/54/196cc2703dc165cbd373a65a4dcf22d50ae7f7
.git/objects/a0
.git/objects/a0/423896973644771497bdc03eb99d5281615b51
.git/objects/d0
.git/objects/d0/492b368b66bdabf2ac1fd8c92b39d3db916e59
.git/objects/c4
.git/objects/c4/d59f390b9cfd4318117afde11d601c1085f241
```

and the contents of these files is just the compressed data plus a header identifying their length and their type. The type is either a blob, a tree, a commit, or a tag.

这些文件的内容只是压缩的数据加上标识其长度和类型的头部。类型可以是blob、tree、commit或tag。

The simplest commit to find is the HEAD commit, which we can find from .git/HEAD:

​	最简单的提交是HEAD提交，我们可以从.git/HEAD找到它：

``` bash
$ cat .git/HEAD
ref: refs/heads/master
```

As you can see, this tells us which branch we’re currently on, and it tells us this by naming a file under the .git directory, which itself contains a SHA-1 name referring to a commit object, which we can examine with cat-file:

​	如您所见，这告诉我们当前所在的分支，它通过为.git目录下的文件命名来指定一个引用到提交对象的SHA-1名称，我们可以使用cat-file查看它：

``` bash
$ cat .git/refs/heads/master
c4d59f390b9cfd4318117afde11d601c1085f241
$ git cat-file -t c4d59f39
commit
$ git cat-file commit c4d59f39
tree d0492b368b66bdabf2ac1fd8c92b39d3db916e59
parent 54196cc2703dc165cbd373a65a4dcf22d50ae7f7
author J. Bruce Fields <bfields@puzzle.fieldses.org> 1143418702 -0500
committer J. Bruce Fields <bfields@puzzle.fieldses.org> 1143418702 -0500

add emphasis
```

The "tree" object here refers to the new state of the tree:

​	这里的"tree"对象引用新的树状态：

``` bash
$ git ls-tree d0492b36
100644 blob a0423896973644771497bdc03eb99d5281615b51    file.txt
$ git cat-file blob a0423896
hello world!
```

and the "parent" object refers to the previous commit:

而"parent"对象引用上一个提交：

``` bash
$ git cat-file commit 54196cc2
tree 92b8b694ffb1675e5975148e1121810081dbdffe
author J. Bruce Fields <bfields@puzzle.fieldses.org> 1143414668 -0500
committer J. Bruce Fields <bfields@puzzle.fieldses.org> 1143414668 -0500

initial commit
```

The tree object is the tree we examined first, and this commit is unusual in that it lacks any parent.

​	树对象是我们最初检查的树，这个提交不同寻常之处在于它没有任何父提交。

Most commits have only one parent, but it is also common for a commit to have multiple parents. In that case the commit represents a merge, with the parent references pointing to the heads of the merged branches.

​	大多数提交只有一个父提交，但是一个提交也可能有多个父提交。在这种情况下，提交表示合并，其中父引用指向合并分支的头。

Besides blobs, trees, and commits, the only remaining type of object is a "tag", which we won’t discuss here; refer to [git-tag[1]](../../1/git-tag) for details.

​	除了blob、tree和commit，剩下的唯一对象类型是"tag"，这里不会详细讨论它；有关详情，请参阅[git-tag[1]](../../1/git-tag)。

So now we know how Git uses the object database to represent a project’s history:

​	现在我们知道Git如何使用对象数据库表示项目的历史：

- "commit" objects refer to "tree" objects representing the snapshot of a directory tree at a particular point in the history, and refer to "parent" commits to show how they’re connected into the project history.
- "commit"对象引用"tree"对象，表示历史上特定时间点的目录树快照，并引用"parent"提交以展示它们如何连接到项目历史。
- "tree" objects represent the state of a single directory, associating directory names to "blob" objects containing file data and "tree" objects containing subdirectory information.
- "tree"对象表示单个目录的状态，将目录名与包含文件数据的"blob"对象和包含子目录信息的"tree"对象关联起来。
- "blob" objects contain file data without any other structure.
- "blob"对象只包含文件数据，没有其他结构。
- References to commit objects at the head of each branch are stored in files under .git/refs/heads/.
- 每个分支头上引用的提交对象的引用存储在.git/refs/heads/目录下。
- The name of the current branch is stored in .git/HEAD.
- 当前分支的名称存储在.git/HEAD中。

Note, by the way, that lots of commands take a tree as an argument. But as we can see above, a tree can be referred to in many different ways—by the SHA-1 name for that tree, by the name of a commit that refers to the tree, by the name of a branch whose head refers to that tree, etc.--and most such commands can accept any of these names.

​	顺便说一下，许多命令将树作为参数。但是如上所示，可以用许多不同的方式引用树——通过该树的SHA-1名称、引用该树的提交的名称、引用该树的分支的名称等等——大多数这样的命令都可以接受这些名称之一。

In command synopses, the word "tree-ish" is sometimes used to designate such an argument.

​	在命令概要中，有时会使用"tree-ish"一词来指代此类参数。

## 索引文件

The primary tool we’ve been using to create commits is `git-commit -a`, which creates a commit including every change you’ve made to your working tree. But what if you want to commit changes only to certain files? Or only certain changes to certain files?

​	我们一直在使用的主要工具来创建提交是`git commit -a`，它会创建包含您对工作目录所做的所有更改的提交。但是，如果您只想提交特定的文件更改怎么办？或者只提交特定文件的某些更改？

If we look at the way commits are created under the cover, we’ll see that there are more flexible ways creating commits.

​	如果我们查看提交是如何在后台创建的，我们会发现有更灵活的方式来创建提交。

Continuing with our test-project, let’s modify file.txt again:

​	继续使用我们的test-project，再次修改file.txt：

``` bash
$ echo "hello world, again" >>file.txt
```

but this time instead of immediately making the commit, let’s take an intermediate step, and ask for diffs along the way to keep track of what’s happening:

但是这一次，我们不会立即进行提交，而是采取一个中间步骤，请求在进行更改的同时生成差异，以跟踪发生的情况：

``` bash
$ git diff
--- a/file.txt
+++ b/file.txt
@@ -1 +1,2 @@
 hello world!
+hello world, again
$ git add file.txt
$ git diff
```

The last diff is empty, but no new commits have been made, and the head still doesn’t contain the new line:

​	最后一个差异为空，但是没有创建新的提交，头还不包含新的行：

``` bash
$ git diff HEAD
diff --git a/file.txt b/file.txt
index a042389..513feba 100644
--- a/file.txt
+++ b/file.txt
@@ -1 +1,2 @@
 hello world!
+hello world, again
```

So *git diff* is comparing against something other than the head. The thing that it’s comparing against is actually the index file, which is stored in .git/index in a binary format, but whose contents we can examine with ls-files:

​	因此，*git diff*正在与头不同的东西进行比较。它要比较的东西实际上是索引文件，索引文件以二进制格式存储在.git/index中，但我们可以使用ls-files查看其内容：

``` bash
$ git ls-files --stage
100644 513feba2e53ebbd2532419ded848ba19de88ba00 0       file.txt
$ git cat-file -t 513feba2
blob
$ git cat-file blob 513feba2
hello world!
hello world, again
```

So what our *git add* did was store a new blob and then put a reference to it in the index file. If we modify the file again, we’ll see that the new modifications are reflected in the *git diff* output:

​	因此，我们的*git add*操作是存储了一个新的blob，并将其引用放入索引文件。如果我们再次修改该文件，我们将看到新的修改反映在*git diff*输出中：

``` bash
$ echo 'again?' >>file.txt
$ git diff
index 513feba..ba3da7b 100644
--- a/file.txt
+++ b/file.txt
@@ -1,2 +1,3 @@
 hello world!
 hello world, again
+again?
```

With the right arguments, *git diff* can also show us the difference between the working directory and the last commit, or between the index and the last commit:

​	通过正确的参数，*git diff*还可以显示工作目录与上一次提交之间的差异，或者索引与上一次提交之间的差异：

``` bash
$ git diff HEAD
diff --git a/file.txt b/file.txt
index a042389..ba3da7b 100644
--- a/file.txt
+++ b/file.txt
@@ -1 +1,3 @@
 hello world!
+hello world, again
+again?
$ git diff --cached
diff --git a/file.txt b/file.txt
index a042389..513feba 100644
--- a/file.txt
+++ b/file.txt
@@ -1 +1,2 @@
 hello world!
+hello world, again
```

At any time, we can create a new commit using *git commit* (without the "-a" option), and verify that the state committed only includes the changes stored in the index file, not the additional change that is still only in our working tree:

​	随时，我们可以使用*git commit*（不带"-a"选项）创建一个新的提交，并验证提交的状态仅包含存储在索引文件中的更改，而不包括仍然只存在于工作目录中的附加更改：

``` bash
$ git commit -m "repeat"
$ git diff HEAD
diff --git a/file.txt b/file.txt
index 513feba..ba3da7b 100644
--- a/file.txt
+++ b/file.txt
@@ -1,2 +1,3 @@
 hello world!
 hello world, again
+again?
```

So by default *git commit* uses the index to create the commit, not the working tree; the "-a" option to commit tells it to first update the index with all changes in the working tree.

​	因此，默认情况下，*git commit*使用索引来创建提交，而不是工作目录；使用"-a"选项进行提交时，它告诉Git首先将所有工作目录中的更改更新到索引中。

Finally, it’s worth looking at the effect of *git add* on the index file:

​	最后，值得注意的是*git add*对索引文件的影响：

``` bash
$ echo "goodbye, world" >closing.txt
$ git add closing.txt
```

The effect of the *git add* was to add one entry to the index file:

​	*git add*的影响是向索引文件添加了一个条目：

``` bash
$ git ls-files --stage
100644 8b9743b20d4b15be3955fc8d5cd2b09cd2336138 0       closing.txt
100644 513feba2e53ebbd2532419ded848ba19de88ba00 0       file.txt
```

And, as you can see with cat-file, this new entry refers to the current contents of the file:

​	如您所见，这个新条目引用了文件的当前内容：

``` bash
$ git cat-file blob 8b9743b2
goodbye, world
```

The "status" command is a useful way to get a quick summary of the situation:

​	"status"命令是一个快速了解情况的有用方式：

``` bash
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)

	new file:   closing.txt

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)

	modified:   file.txt
```

Since the current state of closing.txt is cached in the index file, it is listed as "Changes to be committed". Since file.txt has changes in the working directory that aren’t reflected in the index, it is marked "changed but not updated". At this point, running "git commit" would create a commit that added closing.txt (with its new contents), but that didn’t modify file.txt.

​	由于closing.txt的当前状态被缓存在索引文件中，因此它被列为"要提交的更改"。由于file.txt在工作目录中有未反映在索引中的更改，因此它标记为"已修改但未更新"。此时，运行"git commit"将创建一个提交，其中添加了closing.txt（带有其新内容），但不会修改file.txt。

Also, note that a bare `git diff` shows the changes to file.txt, but not the addition of closing.txt, because the version of closing.txt in the index file is identical to the one in the working directory.

​	此外，请注意，裸`git diff`显示file.txt的更改，但不显示closing.txt的添加，因为索引文件中closing.txt的版本与工作目录中的版本相同。

In addition to being the staging area for new commits, the index file is also populated from the object database when checking out a branch, and is used to hold the trees involved in a merge operation. See [gitcore-tutorial[7]](../../7/gitcore-tutorial) and the relevant man pages for details.

​	除了作为新提交的暂存区域，索引文件还在检出分支时从对象数据库中填充，并用于保存合并操作涉及的树。有关详情，请参阅[gitcore-tutorial[7]](../../7/gitcore-tutorial)以及相关的手册页。

## 下一步？

At this point you should know everything necessary to read the man pages for any of the git commands; one good place to start would be with the commands mentioned in [giteveryday[7]](../../7/giteveryday). You should be able to find any unknown jargon in [gitglossary[7]](../../7/gitglossary).

​	现在您应该已经了解了阅读git命令的手册页所需的所有知识；一个好的开始点是使用[giteveryday[7]](../../7/giteveryday)中提到的命令。您应该能够在[gitglossary[7]](../../7/gitglossary)中找到任何未知的术语。

The [Git User’s Manual](https://git-scm.com/docs/user-manual) provides a more comprehensive introduction to Git.

​	[Git用户手册](https://git-scm.com/docs/user-manual)提供了更全面的Git入门指南。

[gitcvs-migration[7]](../../7/gitcvs-migration) explains how to import a CVS repository into Git, and shows how to use Git in a CVS-like way.

​	[gitcvs-migration[7]](../../7/gitcvs-migration)解释了如何将CVS存储库导入Git，并演示了如何以类似CVS的方式使用Git。

For some interesting examples of Git use, see the [howtos](https://git-scm.com/docs/howto-index).

​	对于一些有趣的Git用法示例，请参阅[howtos](https://git-scm.com/docs/howto-index)。

For Git developers, [gitcore-tutorial[7]](../../7/gitcore-tutorial) goes into detail on the lower-level Git mechanisms involved in, for example, creating a new commit.

​	对于Git开发人员，[gitcore-tutorial[7]](../../7/gitcore-tutorial)详细介绍了涉及创建新提交等低级别Git机制。

## 另请参阅

[gittutorial[7]](../gittutorial), [gitcvs-migration[7]](../gitcvs-migration), [gitcore-tutorial[7]](../gitcore-tutorial), [gitglossary[7]](../gitglossary), [git-help[1]](../../1/git-help), [giteveryday[7]](../giteveryday), [The Git User’s Manual](https://git-scm.com/docs/user-manual)

## GIT

 	这是[git[1]](../../Git)工具集中的一部分。