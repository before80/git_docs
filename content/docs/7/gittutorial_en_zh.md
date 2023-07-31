+++
title = "gittutorial——中英对照版"
weight = 30
type = "docs"
date = 2023-07-30T12:00:23+08:00
description = ""
isCJKLanguage = true
draft = false

+++

# gittutorial

[https://git-scm.com/docs/gittutorial](https://git-scm.com/docs/gittutorial)

version 2.41.0

## 名称

gittutorial - A tutorial introduction to Git

​	gittutorial - Git教程入门

## 概要

```bash
git *
```

## 描述

This tutorial explains how to import a new project into Git, make changes to it, and share changes with other developers.

​	本教程解释了如何将一个新项目导入Git，对其进行更改，并与其他开发人员共享更改。

If you are instead primarily interested in using Git to fetch a project, for example, to test the latest version, you may prefer to start with the first two chapters of [The Git User’s Manual](https://git-scm.com/docs/user-manual).

​	如果您主要感兴趣的是使用Git获取一个项目，例如测试最新版本，您可能更喜欢从[Git用户手册](https://git-scm.com/docs/user-manual)的前两章开始。

First, note that you can get documentation for a command such as `git log --graph` with:

​	首先，注意您可以通过以下命令获得`git log --graph`的文档:

```bash
$ man git-log
```

or:

或者：

```bash
$ git help log
```

With the latter, you can use the manual viewer of your choice; see [git-help[1]](../../1/git-help) for more information.

​	对于后者，您可以使用您选择的手册查看器；请参阅[git-help[1]](../../1/git-help)获取更多信息。

It is a good idea to introduce yourself to Git with your name and public email address before doing any operation. The easiest way to do so is:

​	在执行任何操作之前，向Git介绍自己的姓名和公共电子邮件地址是个好主意。最简单的方法是：

```bash
$ git config --global user.name "Your Name Comes Here"
$ git config --global user.email you@yourdomain.example.com
```

## 导入新项目

Assume you have a tarball `project.tar.gz` with your initial work. You can place it under Git revision control as follows.

​	假设您有一个名为`project.tar.gz`的tarball，其中包含您的初始工作。您可以按以下方式将其放入Git版本控制：

```bash
$ tar xzf project.tar.gz
$ cd project
$ git init
```

Git will reply

​	Git将回复：

```
Initialized empty Git repository in .git/
```

You’ve now initialized the working directory—you may notice a new directory created, named `.git`.

​	现在您已初始化工作目录，您可能会注意到一个新的目录被创建，名为`.git`。

Next, tell Git to take a snapshot of the contents of all files under the current directory (note the `.`), with `git add`:

​	接下来，使用`git add`告诉Git获取当前目录（注意`.`）下所有文件的内容的快照：

```bash
$ git add .
```

This snapshot is now stored in a temporary staging area which Git calls the "index". You can permanently store the contents of the index in the repository with `git commit`:

​	该快照现在存储在Git称为“索引”的临时暂存区中。您可以使用`git commit`将索引的内容永久存储在仓库中：

``` bash
$ git commit
```

This will prompt you for a commit message. You’ve now stored the first version of your project in Git.

​	这将提示您输入提交信息。现在，您已经将项目的第一个版本存储在Git中。

## 进行更改

Modify some files, then add their updated contents to the index:

​	修改一些文件，然后将它们的更新内容添加到索引：

``` bash
$ git add file1 file2 file3
```

You are now ready to commit. You can see what is about to be committed using `git diff` with the `--cached` option:

​	现在您准备进行提交。您可以使用带有`--cached`选项的`git diff`来查看即将提交的内容：

``` bash
$ git diff --cached
```

(Without `--cached`, `git diff` will show you any changes that you’ve made but not yet added to the index.) You can also get a brief summary of the situation with `git status`:

（不带`--cached`，`git diff`会显示您已经做出但尚未添加到索引的任何更改。）您也可以使用`git status`来获取简要概述：

``` bash
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)

	modified:   file1
	modified:   file2
	modified:   file3
```

If you need to make any further adjustments, do so now, and then add any newly modified content to the index. Finally, commit your changes with:

​	如果需要进一步进行任何调整，现在进行，然后将任何新修改的内容添加到索引。最后，使用以下命令提交您的更改：

``` bash
$ git commit
```

This will again prompt you for a message describing the change, and then record a new version of the project.

​	这将再次提示您输入描述更改的消息，然后记录项目的新版本。

Alternatively, instead of running `git add` beforehand, you can use

​	或者，您可以在之前不先运行`git add`的情况下使用

``` bash
$ git commit -a
```

which will automatically notice any modified (but not new) files, add them to the index, and commit, all in one step.

它将自动检测到任何已修改（但不是新建）的文件，将它们添加到索引并在一步中进行提交。

A note on commit messages: Though not required, it’s a good idea to begin the commit message with a single short (less than 50 character) line summarizing the change, followed by a blank line and then a more thorough description. The text up to the first blank line in a commit message is treated as the commit title, and that title is used throughout Git. For example, [git-format-patch[1]](../../1/git-format-patch) turns a commit into email, and it uses the title on the Subject line and the rest of the commit in the body.

​	关于提交消息的一点说明：虽然不是必需的，但最好以单个简短（少于50个字符）的行开始提交消息，总结更改，然后是一个空行，然后是更详细的描述。提交消息中第一个空行之前的文本被视为提交标题，并且该标题在整个Git中使用。例如，[git-format-patch[1]](../../1/git-format-patch)将提交转换为电子邮件，并在主题行中使用标题，在正文中使用提交的其余部分。

## Git跟踪内容而不是文件

Many revision control systems provide an `add` command that tells the system to start tracking changes to a new file. Git’s `add` command does something simpler and more powerful: `git add` is used both for new and newly modified files, and in both cases it takes a snapshot of the given files and stages that content in the index, ready for inclusion in the next commit.

​	许多版本控制系统提供一个`add`命令，告诉系统开始跟踪对新文件的更改。Git的`add`命令做了更简单和更强大的事情：`git add`用于新的和新修改的文件，在这两种情况下，它都会对给定文件进行快照并将该内容暂存到索引中，准备包含在下一次提交中。

## 查看项目历史

At any point you can view the history of your changes using

​	在任何时候，您都可以使用以下命令查看更改的历史记录：

``` bash
$ git log
```

If you also want to see complete diffs at each step, use

​	如果您还想在每个步骤看到完整的差异，请使用：

``` bash
$ git log -p
```

Often the overview of the change is useful to get a feel of each step

​	通常，改变的概述对了解每个步骤很有用：

``` bash
$ git log --stat --summary
```

## 管理分支

A single Git repository can maintain multiple branches of development. To create a new branch named `experimental`, use

​	单个Git仓库可以维护多个开发分支。要创建一个名为`experimental`的新分支，请使用：

``` bash
$ git branch experimental
```

If you now run

​	如果现在运行：

``` bash
$ git branch
```

you’ll get a list of all existing branches:

您将得到所有现有分支的列表：

```
  experimental
* master
```

The `experimental` branch is the one you just created, and the `master` branch is a default branch that was created for you automatically. The asterisk marks the branch you are currently on; type

​	`experimental`分支是您刚刚创建的，`master`分支是为您自动创建的默认分支。星号标记当前所在的分支；输入以下命令：

``` bash
$ git switch experimental
```

to switch to the `experimental` branch. Now edit a file, commit the change, and switch back to the `master` branch:

以切换到`experimental`分支。现在编辑一个文件，提交更改，然后切换回`master`分支：

```
(edit file)
$ git commit -a
$ git switch master
```

Check that the change you made is no longer visible, since it was made on the `experimental` branch and you’re back on the `master` branch.

​	检查您所做的更改是否不可见，因为它是在`experimental`分支上进行的，而您现在又回到了`master`分支。

You can make a different change on the `master` branch:

​	您可以在`master`分支上进行不同的更改：

```
(edit file)
$ git commit -a
```

at this point the two branches have diverged, with different changes made in each. To merge the changes made in `experimental` into `master`, run

此时，两个分支已经分叉，各自做出了不同的更改。要将`experimental`中所做的更改合并到`master`中，请运行：

``` bash
$ git merge experimental
```

If the changes don’t conflict, you’re done. If there are conflicts, markers will be left in the problematic files showing the conflict;

​	如果更改没有冲突，您就完成了。如果存在冲突，标记将在有问题的文件中留下冲突；

``` bash
$ git diff
```

will show this. Once you’ve edited the files to resolve the conflicts,

将显示这个。一旦您编辑文件解决了冲突，

``` bash
$ git commit -a
```

will commit the result of the merge. Finally,

将提交合并的结果。最后，

``` bash
$ gitk
```

will show a nice graphical representation of the resulting history.

将显示合并后的历史的漂亮图形表示。

At this point you could delete the `experimental` branch with

​	此时，您可以使用以下命令删除`experimental`分支：

``` bash
$ git branch -d experimental
```

This command ensures that the changes in the `experimental` branch are already in the current branch.

​	该命令确保`experimental`分支中的更改已经包含在当前分支中。

If you develop on a branch `crazy-idea`, then regret it, you can always delete the branch with

​	如果您在一个名为`crazy-idea`的分支上开发，然后后悔了，您可以随时删除该分支：

``` bash
$ git branch -D crazy-idea
```

Branches are cheap and easy, so this is a good way to try something out.

​	分支是廉价而简单的，所以这是一个尝试某些内容的好方法。

## 使用Git进行协作（collaboration）

Suppose that Alice has started a new project with a Git repository in `/home/alice/project`, and that Bob, who has a home directory on the same machine, wants to contribute.

​	假设Alice在`/home/alice/project`中的Git仓库中开始了一个新项目，而Bob在同一台机器上有一个家目录，并且想要做出贡献。

Bob begins with:

​	Bob可以执行以下操作：

``` bash
bob$ git clone /home/alice/project myrepo
```

This creates a new directory `myrepo` containing a clone of Alice’s repository. The clone is on an equal footing with the original project, possessing its own copy of the original project’s history.

​	这将创建一个名为`myrepo`的新目录，其中包含Alice仓库的克隆。该克隆与原始项目平等，拥有其自己的原始项目历史的副本。

Bob then makes some changes and commits them:

​	随后，Bob进行了一些更改并提交了它们：

```
(edit files)
bob$ git commit -a
(repeat as necessary)
```

When he’s ready, he tells Alice to pull changes from the repository at `/home/bob/myrepo`. She does this with:

​	当他准备好后，他告诉Alice从位于`/home/bob/myrepo`的仓库拉取更改。她可以使用以下命令完成：

``` bash
alice$ cd /home/alice/project
alice$ git pull /home/bob/myrepo master
```

This merges the changes from Bob’s `master` branch into Alice’s current branch. If Alice has made her own changes in the meantime, then she may need to manually fix any conflicts.

​	这将将Bob的`master`分支的更改合并到Alice的当前分支。如果Alice同时也进行了自己的更改，她可能需要手动解决任何冲突。

The `pull` command thus performs two operations: it fetches changes from a remote branch, then merges them into the current branch.

​	因此，`pull`命令执行了两个操作：它从远程分支获取更改，然后将它们合并到当前分支中。

Note that in general, Alice would want her local changes committed before initiating this `pull`. If Bob’s work conflicts with what Alice did since their histories forked, Alice will use her working tree and the index to resolve conflicts, and existing local changes will interfere with the conflict resolution process (Git will still perform the fetch but will refuse to merge — Alice will have to get rid of her local changes in some way and pull again when this happens).

​	请注意，一般情况下，Alice在进行此`pull`之前应该将她的本地更改提交。如果Bob的工作与Alice自从历史分叉以来所做的工作发生冲突，Alice将使用她的工作目录和索引来解决冲突，而现有的本地更改将干扰冲突解决过程（Git仍然会执行获取，但会拒绝合并 - Alice必须以某种方式摆脱她的本地更改，并在此发生时再次拉取）。

Alice can peek at what Bob did without merging first, using the `fetch` command; this allows Alice to inspect what Bob did, using a special symbol `FETCH_HEAD`, in order to determine if he has anything worth pulling, like this:

​	Alice可以在合并之前先查看Bob的更改，使用`fetch`命令；这允许Alice检查Bob的更改，使用特殊符号`FETCH_HEAD`，以确定他是否有值得拉取的内容，像这样：

``` bash
alice$ git fetch /home/bob/myrepo master
alice$ git log -p HEAD..FETCH_HEAD
```

This operation is safe even if Alice has uncommitted local changes. The range notation `HEAD..FETCH_HEAD` means "show everything that is reachable from the `FETCH_HEAD` but exclude anything that is reachable from `HEAD`". Alice already knows everything that leads to her current state (`HEAD`), and reviews what Bob has in his state (`FETCH_HEAD`) that she has not seen with this command.

​	即使Alice有未提交的本地更改，这个操作也是安全的。范围表示法`HEAD..FETCH_HEAD`表示“显示从`FETCH_HEAD`可达但从`HEAD`可达的所有内容”。Alice已经知道了导致她当前状态的所有内容（`HEAD`），并通过此命令查看了Bob在他的状态（`FETCH_HEAD`）中未见的内容。

If Alice wants to visualize what Bob did since their histories forked she can issue the following command:

​	如果Alice想要可视化自从历史分叉以来Bob的更改，她可以发出以下命令：

``` bash
$ gitk HEAD..FETCH_HEAD
```

This uses the same two-dot range notation we saw earlier with `git log`.

​	这使用了我们之前在`git log`中看到的双点范围表示法。

Alice may want to view what both of them did since they forked. She can use three-dot form instead of the two-dot form:

​	Alice可能希望查看自从历史分叉以来他们两人所做的所有更改。此时，她可以使用三点形式而不是两点形式：

``` bash
$ gitk HEAD...FETCH_HEAD
```

This means "show everything that is reachable from either one, but exclude anything that is reachable from both of them".

​	这表示“显示从任意一个可达的所有内容，但排除从两者可达的所有内容”。

Please note that these range notation can be used with both `gitk` and `git log`.

​	请注意，这些范围表示法可以在`gitk`和`git log`中使用。

After inspecting what Bob did, if there is nothing urgent, Alice may decide to continue working without pulling from Bob. If Bob’s history does have something Alice would immediately need, Alice may choose to stash her work-in-progress first, do a `pull`, and then finally unstash her work-in-progress on top of the resulting history.

​	在检查完Bob的更改后，如果没有什么紧急的事情，Alice可能决定继续工作而不从Bob拉取。如果Bob的历史确实有Alice立即需要的内容，Alice可以选择先保存她正在进行的工作，然后执行`pull`，最后在生成的历史上重新应用她正在进行的工作。

When you are working in a small closely knit group, it is not unusual to interact with the same repository over and over again. By defining *remote* repository shorthand, you can make it easier:

​	当您在一个小而紧密联系的团队中工作时，经常与同一个仓库进行交互是很常见的。通过定义*remote*仓库缩写，您可以简化这个过程：

``` bash
alice$ git remote add bob /home/bob/myrepo
```

With this, Alice can perform the first part of the `pull` operation alone using the `git fetch` command without merging them with her own branch, using:

​	有了这个，Alice可以使用`git fetch`命令单独执行`pull`操作的第一部分，而不会将它们与她自己的分支合并，像这样：

``` bash
alice$ git fetch bob
```

Unlike the longhand form, when Alice fetches from Bob using a remote repository shorthand set up with `git remote`, what was fetched is stored in a remote-tracking branch, in this case `bob/master`. So after this:

​	与长格式不同，当Alice使用通过`git remote`设置的remote仓库缩写从Bob获取时，获取的内容将存储在远程跟踪分支中，在这种情况下是`bob/master`。因此，在执行了以下操作后：

``` bash
alice$ git log -p master..bob/master
```

shows a list of all the changes that Bob made since he branched from Alice’s `master` branch.

显示了Bob从Alice的`master`分支分叉以来所做的所有更改的列表。

After examining those changes, Alice could merge the changes into her `master` branch:

​	检查了这些更改后，Alice可以将这些更改合并到她的`master`分支中：

``` bash
alice$ git merge bob/master
```

This `merge` can also be done by *pulling from her own remote-tracking branch*, like this:

​	这个`merge`操作也可以通过*从她自己的远程跟踪分支拉取*来完成，像这样：

``` bash
alice$ git pull . remotes/bob/master
```

Note that git pull always merges into the current branch, regardless of what else is given on the command line.

​	请注意，git pull始终将合并到当前分支，不管命令行上还有什么其他内容。

Later, Bob can update his repo with Alice’s latest changes using

​	之后，Bob可以使用以下命令更新他的仓库，获取Alice的最新更改：

``` bash
bob$ git pull
```

Note that he doesn’t need to give the path to Alice’s repository; when Bob cloned Alice’s repository, Git stored the location of her repository in the repository configuration, and that location is used for pulls:

​	请注意，他不需要提供Alice仓库的路径；当Bob克隆了Alice的仓库后，Git将仓库配置中的Alice仓库位置存储下来，这个位置将用于拉取：

``` bash
bob$ git config --get remote.origin.url
/home/alice/project
```

(The complete configuration created by `git clone` is visible using `git config -l`, and the [git-config[1]](../../1/git-config) man page explains the meaning of each option.)

（`git clone`创建的完整配置可以使用`git config -l`查看，[git-config[1]](../../1/git-config)手册解释了每个选项的含义。）

Git also keeps a pristine copy of Alice’s `master` branch under the name `origin/master`:

​	Git还在名为`origin/master`的名称下保存了Alice的`master`分支的原始副本：

``` bash
bob$ git branch -r
  origin/master
```

If Bob later decides to work from a different host, he can still perform clones and pulls using the ssh protocol:

​	如果Bob以后决定从不同的主机工作，他仍然可以使用ssh协议执行克隆和拉取：

``` bash
bob$ git clone alice.org:/home/alice/project myrepo
```

Alternatively, Git has a native protocol, or can use http; see [git-pull[1]](../../1/git-pull) for details.

​	另外，Git还有一个本地协议，或者可以使用http协议；有关详细信息，请参见[git-pull[1]](../../1/git-pull)。

Git can also be used in a CVS-like mode, with a central repository that various users push changes to; see [git-push[1]](../../1/git-push) and [gitcvs-migration[7]](../../7/gitcvs-migration).

​	Git还可以以类似CVS的模式使用，使用一个中央仓库，各个用户向其推送更改；请参见[git-push[1]](../../1/git-push)和[gitcvs-migration[7]](../../7/gitcvs-migration)。

## 探索历史

Git history is represented as a series of interrelated commits. We have already seen that the `git log` command can list those commits. Note that first line of each `git log` entry also gives a name for the commit:

​	Git历史记录表示为一系列相关的提交。我们已经看到`git log`命令可以列出这些提交。请注意，每个`git log`条目的第一行还为提交提供了一个名称：

``` bash
$ git log
commit c82a22c39cbc32576f64f5c6b3f24b99ea8149c7
Author: Junio C Hamano <junkio@cox.net>
Date:   Tue May 16 17:18:22 2006 -0700

    merge-base: Clarify the comments on post processing.
```

We can give this name to `git show` to see the details about this commit.

​	我们可以将此名称提供给`git show`以查看有关此提交的详细信息。

``` bash
$ git show c82a22c39cbc32576f64f5c6b3f24b99ea8149c7
```

But there are other ways to refer to commits. You can use any initial part of the name that is long enough to uniquely identify the commit:

​	但是还有其他方式可以引用提交。您可以使用任何足够长的名称的初始部分，以唯一地标识提交：

``` bash
$ git show c82a22c39c	# the first few characters of the name are
			# usually enough
$ git show HEAD		# the tip of the current branch
$ git show experimental	# the tip of the "experimental" branch
```

Every commit usually has one "parent" commit which points to the previous state of the project:

​	每个提交通常有一个“父”提交，指向项目的上一个状态：

``` bash
$ git show HEAD^  # to see the parent of HEAD
$ git show HEAD^^ # to see the grandparent of HEAD
$ git show HEAD~4 # to see the great-great grandparent of HEAD
```

Note that merge commits may have more than one parent:

​	请注意，合并提交可能有多个父提交：

``` bash
$ git show HEAD^1 # show the first parent of HEAD (same as HEAD^)
$ git show HEAD^2 # show the second parent of HEAD
```

You can also give commits names of your own; after running

​	您还可以给提交取自己的名称；运行后

``` bash
$ git tag v2.5 1b2e1d63ff
```

you can refer to `1b2e1d63ff` by the name `v2.5`. If you intend to share this name with other people (for example, to identify a release version), you should create a "tag" object, and perhaps sign it; see [git-tag[1]](../../1/git-tag) for details.

您可以使用名称`v2.5`来引用`1b2e1d63ff`。如果您打算与其他人共享此名称（例如用于标识发布版本），则应该创建一个“tag”对象，并可能对其进行签名；有关详细信息，请参见[git-tag[1]](../../1/git-tag)。

Any Git command that needs to know a commit can take any of these names. For example:

​	任何需要了解提交的Git命令都可以采用这些名称中的任何一个。例如：

``` bash
$ git diff v2.5 HEAD	 # compare the current HEAD to v2.5
$ git branch stable v2.5 # start a new branch named "stable" based
			 # at v2.5
$ git reset --hard HEAD^ # reset your current branch and working
			 # directory to its state at HEAD^
```

Be careful with that last command: in addition to losing any changes in the working directory, it will also remove all later commits from this branch. If this branch is the only branch containing those commits, they will be lost. Also, don’t use `git reset` on a publicly-visible branch that other developers pull from, as it will force needless merges on other developers to clean up the history. If you need to undo changes that you have pushed, use `git revert` instead.

​	请小心使用最后一条命令：除了丢失工作目录中的所有更改之外，它还将从此分支删除所有较新的提交。如果这个分支是包含这些提交的唯一分支，它们将丢失。此外，请不要在其他开发者拉取的公开可见分支上使用`git reset`，因为它会强制其他开发者进行不必要的合并以清理历史记录。如果需要撤消已推送的更改，请改用`git revert`。

The `git grep` command can search for strings in any version of your project, so

​	`git grep`命令可以在项目的任何版本中搜索字符串，因此

``` bash
$ git grep "hello" v2.5
```

searches for all occurrences of "hello" in `v2.5`.

在`v2.5`中搜索所有出现的“hello”。

If you leave out the commit name, `git grep` will search any of the files it manages in your current directory. So

​	如果省略了提交名称，`git grep`将搜索它当前目录中管理的任何文件。因此，

``` bash
$ git grep "hello"
```

is a quick way to search just the files that are tracked by Git.

是快速搜索Git跟踪的文件的方法。

Many Git commands also take sets of commits, which can be specified in a number of ways. Here are some examples with `git log`:

​	许多Git命令还可以接受一组提交，这些提交可以用多种方式指定。以下是一些与`git log`相关的示例：

``` bash
$ git log v2.5..v2.6            # commits between v2.5 and v2.6
$ git log v2.5..                # commits since v2.5
$ git log --since="2 weeks ago" # commits from the last 2 weeks
$ git log v2.5.. Makefile       # commits since v2.5 which modify
				# Makefile
```

You can also give `git log` a "range" of commits where the first is not necessarily an ancestor of the second; for example, if the tips of the branches `stable` and `master` diverged from a common commit some time ago, then

​	您还可以给`git log`一个提交“范围”，其中第一个提交不一定是第二个提交的祖先；例如，如果`stable`和`master`分支的末尾在某个时间点从一个共同的提交分叉开来，那么

``` bash
$ git log stable..master
```

will list commits made in the `master` branch but not in the stable branch, while

将列出在`master`分支上进行的提交，但不在`stable`分支上，而

``` bash
$ git log master..stable
```

will show the list of commits made on the stable branch but not the `master` branch.

将显示在`stable`分支上进行的提交，而不在`master`分支上。

The `git log` command has a weakness: it must present commits in a list. When the history has lines of development that diverged and then merged back together, the order in which `git log` presents those commits is meaningless.

​	`git log`命令有一个弱点：它必须以列表形式呈现提交。当历史记录有分叉，然后再次合并时，`git log`呈现这些提交的顺序是没有意义的。

Most projects with multiple contributors (such as the Linux kernel, or Git itself) have frequent merges, and `gitk` does a better job of visualizing their history. For example,

​	具有多个贡献者的大多数项目（例如Linux内核或Git本身）经常进行合并，而`gitk`更好地可视化了它们的历史。例如，

``` bash
$ gitk --since="2 weeks ago" drivers/
```

allows you to browse any commits from the last 2 weeks of commits that modified files under the `drivers` directory. (Note: you can adjust gitk’s fonts by holding down the control key while pressing "-" or "+".)

允许您浏览任何在过去2周内修改了`drivers`目录下文件的提交。（注意：您可以通过同时按下Control键并按下“-”或“+”来调整gitk的字体。）

Finally, most commands that take filenames will optionally allow you to precede any filename by a commit, to specify a particular version of the file:

​	最后，大多数需要文件名的命令都可以选择在文件名前面加上提交，以指定文件的特定版本：

``` bash
$ git diff v2.5:Makefile HEAD:Makefile.in
```

You can also use `git show` to see any such file:

​	您也可以使用`git show`查看任何此类文件：

``` bash
$ git show v2.5:Makefile
```

## 下一步

This tutorial should be enough to perform basic distributed revision control for your projects. However, to fully understand the depth and power of Git you need to understand two simple ideas on which it is based:

​	本教程足以让您进行基本的分布式版本控制，但要充分了解Git的深度和强大之处，您需要理解它所基于的两个简单概念：

- The object database is the rather elegant system used to store the history of your project—files, directories, and commits.
- 对象数据库是用于存储项目历史（文件、目录和提交）的相当优雅的系统。
- The index file is a cache of the state of a directory tree, used to create commits, check out working directories, and hold the various trees involved in a merge.
- 索引文件是目录树状态的缓存，用于创建提交、检出工作目录和保存合并涉及的各种树。

Part two of this tutorial explains the object database, the index file, and a few other odds and ends that you’ll need to make the most of Git. You can find it at [gittutorial-2[7]](../../7/gittutorial-2).

​	本教程的第二部分将解释对象数据库、索引文件和其他一些您需要掌握的Git要点。您可以在[gittutorial-2[7]](../../7/gittutorial-2)找到它。

If you don’t want to continue with that right away, a few other digressions that may be interesting at this point are:

​	如果您暂时不想继续学习，这里有一些其他可能会在这个阶段引起兴趣的内容：

- [git-format-patch[1]](../../1/git-format-patch), [git-am[1]](../../1/git-am): These convert series of git commits into emailed patches, and vice versa, useful for projects such as the Linux kernel which rely heavily on emailed patches.
- [git-format-patch[1]](../../1/git-format-patch)和[git-am[1]](../../1/git-am)：这些命令用于将一系列Git提交转换为电子邮件补丁，反之亦然，对于像Linux内核这样大量依赖电子邮件补丁的项目非常有用。
- [git-bisect[1]](../../1/git-bisect): When there is a regression in your project, one way to track down the bug is by searching through the history to find the exact commit that’s to blame. `git bisect` can help you perform a binary search for that commit. It is smart enough to perform a close-to-optimal search even in the case of complex non-linear history with lots of merged branches.
- [git-bisect[1]](../../1/git-bisect)：当项目出现回归问题时，一种追踪错误的方法是通过搜索历史记录找到确切的有问题提交。`git bisect`可以帮助您对该提交进行二分搜索。即使在具有许多合并分支的复杂非线性历史记录的情况下，它也能执行接近最优的搜索。
- [gitworkflows[7]](../../7/gitworkflows): Gives an overview of recommended workflows.
- [gitworkflows[7]](../../7/gitworkflows)：提供推荐工作流程的概述。
- [giteveryday[7]](../../7/giteveryday): Everyday Git with 20 Commands Or So.
- [giteveryday[7]](../../7/giteveryday)：掌握每天使用的约20个Git命令。
- [gitcvs-migration[7]](../../7/gitcvs-migration): Git for CVS users.
- [gitcvs-migration[7]](../../7/gitcvs-migration)：面向CVS用户的Git使用指南。

## 另请参阅

[gittutorial-2[7]](../gittutorial-2), [gitcvs-migration[7]](../gitcvs-migration), [gitcore-tutorial[7]](../gitcore-tutorial), [gitglossary[7]](../gitglossary), [git-help[1]](../../1/git-help), [gitworkflows[7]](../gitworkflows), [giteveryday[7]](../giteveryday), [The Git User’s Manual](https://git-scm.com/docs/user-manual)

## GIT

  这是[git[1]](../../Git)工具集中的一部分。