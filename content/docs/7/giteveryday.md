+++
title = "giteveryday"
weight = 30
type = "docs"
date = 2023-07-30T15:39:23+08:00
description = ""
isCJKLanguage = true
draft = false

+++

# giteveryday

https://git-scm.com/docs/giteveryday

version 2.41.0

## 名称

​	giteveryday - 日常Git使用的最小有用命令集

## 概述

​	每日Git使用的20个左右命令

## 描述

​	为了描述日常Git使用的一小组有用的命令，Git用户可以大致分为四类。

- [个人开发者（独立开发者）](https://git-scm.com/docs/giteveryday#STANDALONE) 需要使用以下命令，这些命令对于任何进行提交的人都是必要的，即使是独立工作的人。
- 如果您与其他人合作，还需要使用 [个人开发者（参与者）](https://git-scm.com/docs/giteveryday#PARTICIPANT) 部分中列出的命令。
- 担任 [集成者（Integrator）](https://git-scm.com/docs/giteveryday#INTEGRATOR) 角色的人除了上述命令外，还需要学习更多命令。
- [仓库管理员](https://git-scm.com/docs/giteveryday#ADMINISTRATION) 部分中的命令适用于负责Git仓库维护和管理的系统管理员。

## 个人开发者（独立开发者）

​	独立的个人开发者不与他人交换补丁，独自在单个仓库中工作，使用以下命令：

- 使用 [git-init[1]](../../1/git-init) 创建一个新的仓库。
- 使用 [git-log[1]](../../1/git-log) 查看提交历史。
- 使用 [git-switch[1]](../../1/git-switch) 和 [git-branch[1]](../../1/git-branch) 切换分支。
- 使用 [git-add[1]](../../1/git-add) 管理索引文件。
- 使用 [git-diff[1]](../../1/git-diff) 和 [git-status[1]](../../1/git-status) 查看当前操作的状态。
- 使用 [git-commit[1]](../../1/git-commit) 推进当前分支。
- 使用 [git-restore[1]](../../1/git-restore) 撤消更改。
- 使用 [git-merge[1]](../../1/git-merge) 在本地分支之间进行合并。
- 使用 [git-rebase[1]](../../1/git-rebase) 维护主题分支。
- 使用 [git-tag[1]](../../1/git-tag) 标记已知的提交点。

### 示例

​	使用压缩包作为新仓库的起点。

``` bash
$ tar zxf frotz.tar.gz
$ cd frotz
$ git init
$ git add . (1)
$ git commit -m "import of frotz source tree."
$ git tag v2.43 (2)
```

1. 添加当前目录下的所有内容。
2. 创建一个轻量级的、无注释的标签。

​	创建主题分支并进行开发。

``` bash
$ git switch -c alsa-audio (1)
$ edit/compile/test
$ git restore curses/ux_audio_oss.c (2)
$ git add curses/ux_audio_alsa.c (3)
$ edit/compile/test
$ git diff HEAD (4)
$ git commit -a -s (5)
$ edit/compile/test
$ git diff HEAD^ (6)
$ git commit -a --amend (7)
$ git switch master (8)
$ git merge alsa-audio (9)
$ git log --since='3 days ago' (10)
$ git log v2.43.. curses/ (11)
```

1. 创建一个新的主题分支。
2. 撤消对 `curses/ux_audio_oss.c` 的失误更改。
3. 如果添加了新文件，则需要告诉Git；如果稍后执行 `git commit -a`，则会捕捉到删除和修改。
4. 查看正在提交的更改。
5. 使用签名提交所有更改，因为您已经进行了测试。
6. 查看包括上次提交在内的所有更改。
7. 修正上次提交，添加所有新更改，并使用原始提交消息。
8. 切换到主分支。
9. 将主题分支合并到主分支。
10. 查看提交日志；还可以使用其他限制输出的选项，例如 `-10`（显示最多10个提交）、`--until=2005-12-10` 等。
11. 仅查看涉及 `curses/` 目录的更改，自 `v2.43` 标签以来的更改。

## 个人开发者（参与者）

​	作为团队项目的参与者，开发者需要学习如何与他人沟通，并在必要时使用以下命令，除了独立开发者需要使用的命令：

- 使用 [git-clone[1]](../../1/git-clone) 从上游克隆至本地仓库。
- 使用 [git-pull[1]](../../1/git-pull) 和 [git-fetch[1]](../../1/git-fetch) 从 "origin" 保持与上游的同步。
- 使用 [git-push[1]](../../1/git-push) 将变更推送到共享仓库，如果您采用 CVS 风格的共享仓库工作流程。
- 使用 [git-format-patch[1]](../../1/git-format-patch) 准备提交邮件，如果您采用 Linux 内核风格的公共论坛工作流程。
- 使用 [git-send-email[1]](../../1/git-send-email) 通过电子邮件提交代码，以免受到邮件客户端的破坏。
- 使用 [git-request-pull[1]](../../1/git-request-pull) 创建变更摘要，供上游拉取。

### 示例

- 克隆上游仓库并在其上工作。将变更提交到上游。

  ```bash
  $ git clone git://git.kernel.org/pub/scm/.../torvalds/linux-2.6 my2.6
  $ cd my2.6
  $ git switch -c mine master (1)
  $ edit/compile/test; git commit -a -s (2)
  $ git format-patch master (3)
  $ git send-email --to="person <email@example.com>" 00*.patch (4)
  $ git switch master (5)
  $ git pull (6)
  $ git log -p ORIG_HEAD.. arch/i386 include/asm-i386 (7)
  $ git ls-remote --heads http://git.kernel.org/.../jgarzik/libata-dev.git (8)
  $ git pull git://git.kernel.org/pub/.../jgarzik/libata-dev.git ALL (9)
  $ git reset --hard ORIG_HEAD (10)
  $ git gc (11)
  ```

  1. 从主分支克隆一个新分支 `mine`。
  2. 反复进行编辑/编译/测试，并进行签名提交。
  3. 从您的分支中提取相对于主分支的补丁。
  4. 将补丁通过邮件发送出去。
  5. 切换回 `master`，准备查看有什么新的变化。
  6. `git pull` 默认从 `origin` 拉取并合并到当前分支。
  7. 在拉取后立即查看上游自上次检查以来的更改，仅限于我们感兴趣的区域。
  8. 检查外部仓库的分支名称（如果不知道）。
  9. 从特定仓库的特定分支 `ALL` 拉取并合并。
  10. 撤消拉取操作。
  11. 清理撤消拉取后留下的未使用对象。

- 推送到另一个仓库。

  ```
  satellite$ git clone mothership:frotz frotz (1)
  satellite$ cd frotz
  satellite$ git config --get-regexp '^(remote|branch)\.' (2)
  remote.origin.url mothership:frotz
  remote.origin.fetch refs/heads/*:refs/remotes/origin/*
  branch.master.remote origin
  branch.master.merge refs/heads/master
  satellite$ git config remote.origin.push \
  	   +refs/heads/*:refs/remotes/satellite/* (3)
  satellite$ edit/compile/test/commit
  satellite$ git push origin (4)
  
  mothership$ cd frotz
  mothership$ git switch master
  mothership$ git merge satellite/master (5)
  ```

  1. mothership 机器上有一个位于您主目录下的 frotz 仓库；从该仓库克隆，以在 satellite 机器上开始一个新的仓库。
  2. 克隆默认设置这些配置变量。它配置 `git pull` 以获取并存储 mothership 机器的分支至本地的 `remotes/origin/*` 远程跟踪分支。
  3. 配置 `git push` 以将所有本地分支推送到 mothership 机器的相应分支。
  4. 推送会将所有工作内容储存为 mothership 机器上 `remotes/satellite/*` 远程跟踪分支上的备份。您可以将此用作备份方法。同样，您可以假装 mothership 是从您那里 "拉取"（当访问是单向时有用）。
  5. 在 mothership 机器上，将 satellite 机器上的工作合并到 master 分支中。



- 基于特定标签创建分支。

  ```bash
  $ git switch -c private2.6.14 v2.6.14 (1)
  $ edit/compile/test; git commit -a
  $ git checkout master
  $ git cherry-pick v2.6.14..private2.6.14 (2)
  ```

  1. 基于一个已知的（但有些落后）标签创建一个私有分支。

  2. 将 `private2.6.14` 分支中的所有更改转发到 `master` 分支，而不进行正式的 "合并"。或使用下面的命令

     ```
     git format-patch -k -m --stdout v2.6.14..private2.6.14 | git am -3 -k
     ```

     作为替代方法提交变更。

​	另一种参与者提交机制是使用 `git request-pull` 或 pull-request 机制（例如在 GitHub（[www.github.com）上使用的方式），以通知上游您的贡献。](http://www.github.xn--com))%2C-ep7ic48q61be48gm9duztf71altpcohb8fia718dom3jpbn./)

## 集成者（Integrator）

​	一个相对核心的人在团队项目中担任集成者角色，接收其他人所做的更改，审查并将它们合并，并发布结果供他人使用，使用的命令除了参与者需要用到的命令外还包括以下命令。

​	这一节还可以被那些回应 `git request-pull` 或者在 GitHub ([www.github.com](http://www.github.com/)) 上提交 pull 请求的人用来将其他人的工作整合到他们的历史中。作为仓库的一个子区域副官，将同时扮演参与者和集成者的角色。

- [git-am[1]](../../1/git-am) 用于应用通过邮件提交的补丁。
- [git-pull[1]](../../1/git-pull) 用于从可信副官那里合并更改。
- [git-format-patch[1]](../../1/git-format-patch) 用于准备并向贡献者发送建议的备选补丁。
- [git-revert[1]](../../1/git-revert) 用于撤消错误的提交。
- [git-push[1]](../../1/git-push) 用于发布最新的开发版本。

### 示例

- 典型的集成者的 Git 日常工作。

  ```bash
  $ git status (1)
  $ git branch --no-merged master (2)
  $ mailx (3)
  & s 2 3 4 5 ./+to-apply
  & s 7 8 ./+hold-linus
  & q
  $ git switch -c topic/one master
  $ git am -3 -i -s ./+to-apply (4)
  $ compile/test
  $ git switch -c hold/linus && git am -3 -i -s ./+hold-linus (5)
  $ git switch topic/one && git rebase master (6)
  $ git switch -C seen next (7)
  $ git merge topic/one topic/two && git merge hold/linus (8)
  $ git switch maint
  $ git cherry-pick master~4 (9)
  $ compile/test
  $ git tag -s -m "GIT 0.99.9x" v0.99.9x (10)
  $ git fetch ko && for branch in master maint next seen (11)
      do
  	git show-branch ko/$branch $branch (12)
      done
  $ git push --follow-tags ko (13)
  ```

  1. 看一下你正在做的事情，如果有的话。
  2. 查看哪些分支尚未合并到 `master`。同样，对于其他的集成分支，如 `maint`、`next` 和 `seen` 也是一样。
  3. 阅读邮件，保存适用的邮件，并保存其他尚未准备好的邮件（其他邮件客户端也可用）。
  4. 使用你的签名应用这些补丁，并进行交互式操作。
  5. 根据需要创建主题分支并再次应用补丁，同样需要你的签名。
  6. 重新基于尚未合并到 `master` 或作为稳定分支的一部分暴露的内部主题分支。
  7. 每次从 `next` 重新开始 `seen`。
  8. 同时捆绑正在进行中的主题分支。
  9. 回溯关键修复。
  10. 创建一个带签名的标签。
  11. 确保 `master` 没有意外地被倒回到已经推送出去的位置。
  12. 在 `git show-branch` 的输出中，`master` 应该有 `ko/master` 有的所有内容，`next` 应该有 `ko/next` 有的所有内容，等等。
  13. 将最新的开发版本推送出去，同时推送指向已推送历史的新标签。

​	在这个示例中，`ko` 缩写指向在 kernel.org 上的 Git 维护者仓库，并且配置如下：

```
(in .git/config)
[remote "ko"]
	url = kernel.org:/pub/scm/git/git.git
	fetch = refs/heads/*:refs/remotes/ko/*
	push = refs/heads/master
	push = refs/heads/next
	push = +refs/heads/seen
	push = refs/heads/maint
```

## 仓库管理

​	仓库管理员使用以下工具来设置和维护开发者对仓库的访问权限。

- [git-daemon[1]](../../1/git-daemon) 用于允许匿名下载仓库内容。
- [git-shell[1]](../../1/git-shell) 可以作为共享中央仓库用户的*受限登录shell*。
- [git-http-backend[1]](../../1/git-http-backend) 提供了Git-over-HTTP（"智能HTTP"）的服务器端实现，允许提供获取和推送服务。
- [gitweb[1]](../../1/gitweb) 提供了Git仓库的Web前端，可以通过[git-instaweb[1]](../../1/git-instaweb)脚本进行设置。

​	[update hook howto](https://git-scm.com/docs/howto/update-hook-example) 提供了一个管理共享中央仓库的良好示例。

​	此外，还有许多其他广泛部署的主机、浏览和审查解决方案，如：

- gitolite、gerrit代码审查、cgit 等。

### 示例

- 我们假设在 /etc/services 文件中包含以下内容

  ```bash
  $ grep 9418 /etc/services
  git		9418/tcp		# Git Version Control System
  ```

  

- 使用inetd运行git-daemon来提供 /pub/scm 的服务。

  ``` bash
  $ grep git /etc/inetd.conf
  git	stream	tcp	nowait	nobody \
    /usr/bin/git-daemon git-daemon --inetd --export-all /pub/scm
  ```

  实际的配置行应该在一行中。

- 使用xinetd运行git-daemon来提供 /pub/scm 的服务。

  ``` bash
  $ cat /etc/xinetd.d/git-daemon
  # default: off
  # description: The Git server offers access to Git repositories
  service git
  {
  	disable = no
  	type            = UNLISTED
  	port            = 9418
  	socket_type     = stream
  	wait            = no
  	user            = nobody
  	server          = /usr/bin/git-daemon
  	server_args     = --inetd --export-all --base-path=/pub/scm
  	log_on_failure  += USERID
  }
  ```

  请查阅你的xinetd(8)文档和设置，这是来自Fedora系统的示例，其他系统可能不同。

- 给开发者使用git-over-ssh提供推送/拉取权限。

  例如：那些使用 `$ git push/pull ssh://host.xz/pub/scm/project` 的用户。

  ``` bash
  $ grep git /etc/passwd (1)
  alice:x:1000:1000::/home/alice:/usr/bin/git-shell
  bob:x:1001:1001::/home/bob:/usr/bin/git-shell
  cindy:x:1002:1002::/home/cindy:/usr/bin/git-shell
  david:x:1003:1003::/home/david:/usr/bin/git-shell
  $ grep git /etc/shells (2)
  /usr/bin/git-shell
  ```

  1. 登录shell设置为 /usr/bin/git-shell，该shell只允许 `git push` 和 `git pull`，用户需要SSH访问该机器。
  2. 在许多发行版中，/etc/shells 需要列出用作登录shell的内容。

- CVS风格的共享仓库。

  ``` bash
  $ grep git /etc/group (1)
  git:x:9418:alice,bob,cindy,david
  $ cd /home/devo.git
  $ ls -l (2)
    lrwxrwxrwx   1 david git    17 Dec  4 22:40 HEAD -> refs/heads/master
    drwxrwsr-x   2 david git  4096 Dec  4 22:40 branches
    -rw-rw-r--   1 david git    84 Dec  4 22:40 config
    -rw-rw-r--   1 david git    58 Dec  4 22:40 description
    drwxrwsr-x   2 david git  4096 Dec  4 22:40 hooks
    -rw-rw-r--   1 david git 37504 Dec  4 22:40 index
    drwxrwsr-x   2 david git  4096 Dec  4 22:40 info
    drwxrwsr-x   4 david git  4096 Dec  4 22:40 objects
    drwxrwsr-x   4 david git  4096 Nov  7 14:58 refs
    drwxrwsr-x   2 david git  4096 Dec  4 22:40 remotes
  $ ls -l hooks/update (3)
    -r-xr-xr-x   1 david git  3536 Dec  4 22:40 update
  $ cat info/allowed-users (4)
  refs/heads/master	alice\|cindy
  refs/heads/doc-update	bob
  refs/tags/v[0-9]*	david
  ```

  1. 将开发者放入相同的git用户组。
  2. 并将共享仓库设置为用户组可写。
  3. 使用文档中的update-hook示例来进行分支策略控制。
  4. alice和cindy可以向master推送，只有bob可以推送到doc-update分支。david是发布管理员，是唯一可以创建和推送版本标签的人。

  

## GIT

  这是[git[1]](../../Git)工具集中的一部分。