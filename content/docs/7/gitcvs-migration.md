+++
title = "gitcvs-migration"
weight = 30
type = "docs"
date = 2023-07-30T15:39:23+08:00
description = ""
isCJKLanguage = true
draft = false
+++

# gitcvs-migration

https://git-scm.com/docs/gitcvs-migration

version 2.41.0

## 名称

​	gitcvs-migration - 面向 CVS 用户的 Git

## 概述

```
git cvsimport *
```

## 描述

​	Git 与 CVS 的不同之处在于每个工作树都包含有完整项目历史的存储库，并且没有存储库本身比其他存储库更重要。然而，您可以通过指定单个共享存储库来模拟 CVS 模型，供其他人进行同步；本文档解释了如何实现这一点。

​	需要对 Git 有一些基本了解。阅读 [gittutorial[7]](../../7/gittutorial) 和 [gitglossary[7]](../../7/gitglossary) 应该足够。

## 针对共享存储库的开发

​	假设在主机 foo.com 上设置了共享存储库 /pub/repo.git。然后，作为个人提交者，您可以通过 ssh 克隆共享存储库：

``` bash
$ git clone foo.com:/pub/repo.git/ my-project
$ cd my-project
```

然后进行修改。*cvs update* 的等效操作是：

``` bash
$ git pull origin
```

这将合并自克隆操作以来其他人可能完成的工作。如果您的工作树中有未提交的更改，在运行 git pull 之前请先提交这些更改。

> 注意
>
> ​	*pull* 命令知道从哪里获取更新，这是因为在第一个 *git clone* 命令中设置了某些配置变量；有关详细信息，请参阅 `git config -l` 和 [git-config[1]](../../1/git-config) 手册页。

​	您可以通过先提交更改，然后使用 *git push* 命令将更改更新到共享存储库：

``` bash
$ git push origin master
```

来将您的更改更新到共享存储库。如果其他人最近更新了存储库，*git push* 就像 *cvs commit* 一样会出现冲突，在这种情况下，您必须先拉取任何更改，然后再次尝试推送。

​	在上面的 *git push* 命令中，我们指定要更新的远程分支的名称（`master`）。如果省略了该名称，*git push* 将尝试更新远程存储库中与本地存储库具有相同名称的任何分支。因此，最后一个 *push* 可以使用以下任一命令完成：

``` bash
$ git push origin
$ git push foo.com:/pub/project.git/
```

只要共享存储库没有除了 `master` 以外的任何分支。

## 设置共享存储库

​	我们假设您已经为您的项目创建了一个 Git 存储库，可能是从头开始创建的，或者从 tarball 创建的（参见 [gittutorial[7]](../../7/gittutorial)），或者从已经存在的 CVS 存储库导入的（参见下一节）。

​	假设您现有的存储库位于 /home/alice/myproject。创建一个新的“裸”存储库（没有工作树的存储库）并将您的项目提取到其中：

``` bash
$ mkdir /pub/my-repo.git
$ cd /pub/my-repo.git
$ git --bare init --shared
$ git --bare fetch /home/alice/myproject master:master
```

​	然后，将每个团队成员都赋予此存储库的读写访问权限。一个简单的方法是给所有团队成员提供主机上托管存储库的 ssh 访问权限。如果您不想给他们在主机上提供完整的 shell，还有一个只允许用户执行 Git 推送和拉取操作的受限制 shell；请参阅 [git-shell[1]](../../1/git-shell)。

​	将所有提交者放入同一组，并将该组设置为存储库的可写组：

``` bash
$ chgrp -R $group /pub/my-repo.git
```

​	确保提交者的 umask 最大为 027，以便他们创建的目录可以被其他组成员写入和搜索。

## 导入 CVS 存档

> 注意
>
> ​	这些说明使用 git 随附的 `git-cvsimport` 脚本，但其他导入工具可能提供更好的结果。有关其他选项，请参阅 [git-cvsimport[1]](../../1/git-cvsimport) 中的注释。

​	首先，请安装版本 2.1 或更高版本的 cvsps（https://github.com/andreyvit/cvsps），并确保其在您的路径中。然后，切换到您感兴趣的项目的已签出的 CVS 工作目录，并运行 [git-cvsimport[1]](../../1/git-cvsimport)：

``` bash
$ git cvsimport -C <destination> <module>
```

​	这将在目录 <destination> 中放置所命名的 CVS 模块的 Git 存档，如果需要，将创建该目录。

​	导入会从 CVS 检出每个文件的每个修订版本。据报道，cvsimport 每秒平均可以处理约二十个修订版本，因此对于中等大小的项目，这应该不会花费超过几分钟。较大的项目或远程存储库可能需要更长时间。

​	主干存储在名为 `origin` 的 Git 分支中，其他 CVS 分支存储在具有相同名称的 Git 分支中。主干的最新版本也保留在 `master` 分支上，因此您可以立即开始添加自己的更改。

​	导入是增量的，因此如果下个月再次调用它，它将提取在此期间进行的任何 CVS 更新。为使其工作，您不能修改导入的分支；相反，为自己的更改创建新的分支，并根据需要合并导入的分支。

​	如果您想要一个共享存储库，则需要裸克隆已导入目录，如上面所述。然后，对于合并增量导入的目的，将已导入的目录视为另一个开发克隆。

## 高级共享存储库管理

​	Git 允许您指定在某些点运行的脚本，称为“钩子”。您可以使用这些钩子，例如，将所有提交发送到共享存储库的邮件列表。请参阅 [githooks[5]](../../5/githooks)。

​	您可以使用更新钩子来执行更细粒度的权限控制。请参阅 [使用更新钩子控制对分支的访问权限](https://git-scm.com/docs/howto/update-hook-example)。

## 向 Git 存储库提供 CVS 访问权限

​	还可以为 Git 存储库提供真正的 CVS 访问权限，以便开发人员仍然可以使用 CVS；请参阅 [git-cvsserver[1]](../../1/git-cvsserver) 获取详细信息。

## 替代性开发模型

​	CVS 用户习惯于为一组开发者提供对一个共同存储库的提交访问权限。正如我们所见，这也适用于 Git。然而，Git 的分布式特性允许其他开发模型，您可能首先要考虑是否有一个更适合您的项目的模型。

​	例如，您可以选择一个人来维护项目的主要公共存储库。其他开发者然后克隆此存储库，并在自己的克隆中工作。当他们有一系列满意的更改时，他们会请求维护者从包含这些更改的分支中拉取。维护者审核其更改并将其拉入主要存储库，其他开发者根据需要从该存储库拉取以保持协调。Linux 内核和其他项目使用此模型的变体。

​	对于小团队，开发者可能只需从彼此的存储库拉取更改，无需一个中央维护者。



## 另请参阅

[gittutorial[7]](../gittutorial), [gittutorial-2[7]](../gittutorial-2), [gitcore-tutorial[7]](../gitcore-tutorial), [gitglossary[7]](../gitglossary), [giteveryday[7]](../giteveryday), [The Git User’s Manual](https://git-scm.com/docs/user-manual)

## GIT

  这是[git[1]](../../Git)工具集中的一部分。