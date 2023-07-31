+++
title = "gitrevisions"
weight = 30
type = "docs"
date = 2023-07-30T15:39:23+08:00
description = ""
isCJKLanguage = true
draft = false
+++

# gitrevisions 

https://git-scm.com/docs/gitrevisions

version 2.41.0

## 名称

​	gitrevisions - 指定 Git 中的修订版本和范围

## 概要

gitrevisions

## 描述

​	许多 Git 命令将修订版本参数作为参数。根据命令，它们表示特定的提交或者对于遍历修订版本图的命令（例如 [git-log[1]](../../1/git-log)），表示所有从该提交可达的提交。对于遍历修订版本图的命令，还可以显式地指定一系列修订版本。

​	此外，一些 Git 命令（例如 [git-show[1]](../../1/git-show) 和 [git-push[1]](../../1/git-push)）还可以接受表示与提交不同类型的对象的修订版本参数，例如 blob（“文件”）或 tree（“文件目录”）。

## 指定修订版本

​	修订版本参数 *<rev>* 通常但不一定是一个提交对象的名称。它使用称为*扩展 SHA-1*语法。以下是表示对象名称的各种方式。列表末尾列出的那些名称表示包含在提交中的 tree 和 blob。

> 注意
>
> ​	本文档显示 git 看到的“原始”语法。Shell 和其他用户界面可能需要额外的引号来保护特殊字符，并避免单词拆分。

- *<sha1>*，例如 *dae86e1950b1277e545cee180551750029cfe735*，*dae86e*

  完整的 SHA-1 对象名称（40 字节的十六进制字符串），或者在存储库中唯一的前导子串。例如，如果存储库中没有以 dae86e 开头的其他对象名称，则 dae86e1950b1277e545cee180551750029cfe735 和 dae86e 表示相同的提交对象。

- *<describeOutput>*，例如 *v1.7.4.2-679-g3bee7fb*

  Output from `git describe`; i.e. a closest tag, optionally followed by a dash and a number of commits, followed by a dash, a *g*, and an abbreviated object name.

  来自 `git describe` 的输出；即，最近的标签，可选地跟随破折号和一些提交数量，再跟随破折号、一个 *g* 和一个缩写的对象名称。

- *<refname>*，例如 *master*，*heads/master*，*refs/heads/master*

  一个符号引用名称。例如，*master* 通常表示由 *refs/heads/master* 引用的提交对象。如果您碰巧同时拥有 *heads/master* 和 *tags/master*，可以显式地指定 *heads/master* 以告诉 Git 您指的是哪一个。在模糊时，*<refname>* 通过以下规则的第一个匹配来消除模糊：如果 *$GIT_DIR/<refname>* 存在，那就是您指的（这通常只对 `HEAD`、`FETCH_HEAD`、`ORIG_HEAD`、`MERGE_HEAD` 和 `CHERRY_PICK_HEAD` 有用）；否则，如果存在 *refs/<refname>*，那就是您指的；否则，如果存在 *refs/tags/<refname>*，那就是您指的；否则，如果存在 *refs/heads/<refname>*，那就是您指的；否则，如果存在 *refs/remotes/<refname>*，那就是您指的；否则，如果存在 *refs/remotes/<refname>/HEAD*，那就是您指的。`HEAD` 表示您在工作树中所基于的提交。`FETCH_HEAD` 记录您最后一次 `git fetch` 命令从远程存储库获取的分支。`ORIG_HEAD` 由将您的 `HEAD` 以 drastiv 方式移动的命令（`git am`、`git merge`、`git rebase`、`git reset`）创建，以记录其操作之前 `HEAD` 的位置，这样您就可以轻松地将分支的提示更改回运行它们之前的状态。`MERGE_HEAD` 记录在运行 `git merge` 时正在合并到您的分支的提交。`CHERRY_PICK_HEAD` 记录在运行 `git cherry-pick` 时正在进行樱桃拣选的提交。请注意，上述任何一个 *refs/** 都可能来自 `$GIT_DIR/refs` 目录或 `$GIT_DIR/packed-refs` 文件。尽管参考名称编码未指定，但推荐使用 UTF-8，因为一些输出处理可能会假定 UTF-8 编码的参考名称。

- *@*

  *@* 单独是 `HEAD` 的一个快捷方式。

- *[<refname>]@{<date>}*，例如 *master@{yesterday}*，*HEAD@{5 minutes ago}*

  一个 ref，后面跟随后缀 *@*，并用大括号对括起来的日期说明（例如 *{yesterday}*、*{1 month 2 weeks 3 days 1 hour 1 second ago}* 或 *{1979-02-26 18:30:00}*），指定了先前时间点上 ref 的值。此后缀只能立即跟在 ref 名称后面，而且 ref 必须具有现有的日志（*$GIT_DIR/logs/<ref>*）。请注意，这会查找给定时间点上 **本地** ref 的状态；例如，上周您本地 *master* 分支中的内容。如果您想查看在特定时间内进行的提交，请参阅 `--since` 和 `--until`。

- *<refname>@{<n>}*，例如 *master@{1}*

  一个 ref，后面跟随后缀 *@*，并用大括号对括起来的序数说明（例如 *{1}*、*{15}*），指定了该 ref 的第 n 个之前的值。例如 *master@{1}* 是 *master* 的前一个值，而 *master@{5}* 是 *master* 的第 5 个之前的值。此后缀只能立即跟在 ref 名称后面，而且 ref 必须具有现有的日志（*$GIT_DIR/logs/<refname>*）。

- *@{<n>}*，例如 *@{1}*

  您可以使用 *@* 结构并且没有 ref 部分来访问当前分支的 reflog 条目。例如，如果您位于 *blabla* 分支上，则 *@{1}* 的含义与 *blabla@{1}* 相同。

- *@{-<n>}*，例如 *@{-1}*

  结构 *@{-<n>}* 表示当前分支之前第 n 个分支/提交检出的位置。

- *[<branchname>]@{upstream}*，例如 *master@{upstream}*，*@{u}*

  分支 B 可能被设置为在一个远程存储库 R 上构建（使用 `branch.<name>.merge` 配置），并且构建的分支可能在这个远程存储库中找到（通常位于 `refs/remotes/R/X`）。B@{u} 指的是从远程存储库 R 取得的分支 X 的远程跟踪分支。

- *[<branchname>]@{push}*，例如 *master@{push}*，*@{push}*

  后缀 *@{push}* 报告如果在检出了 branchname 的情况下运行 `git push`，则“我们将推送到的分支”。如果没有指定 branchname，则报告与远程对应的 remote-tracking 分支。就像 *@{upstream}* 一样，我们报告对应于该分支在远程的 remote-tracking 分支。下面是一个示例，以使其更清楚：

  ``` bash
  $ git config push.default current
  $ git config remote.pushdefault myfork
  $ git switch -c mybranch origin/master
  
  $ git rev-parse --symbolic-full-name @{upstream}
  refs/remotes/origin/master
  
  $ git rev-parse --symbolic-full-name @{push}
  refs/remotes/myfork/mybranch
  ```

  注意在示例中，我们设置了一个三角形的工作流程，其中我们从一个位置拉取，然后推送到另一个位置。在非三角形的工作流程中，*@{push}* 与 *@{upstream}* 相同，无需使用它。这个后缀在大写拼写时也是被接受的，并且不管大小写，其含义都是相同的。

- *<rev>^[<n>]*，例如 *HEAD^, v1.5.1^0*

  在修订版本参数后加上后缀 *^* 表示该提交对象的第一个父提交。*^<n>* 表示第 n 个父提交（即 *<rev>^* 等同于 *<rev>^1*）。作为特殊规则，*<rev>^0* 表示提交本身，用于当 *<rev>* 是指向提交对象的标签对象的对象名称时

- *<rev>~[<n>]*，例如 *HEAD~, master~3*

  在修订版本参数后加上后缀 *~* 表示该提交对象的第一个父提交。在修订版本参数后加上后缀 *~<n>* 表示指定的提交对象的第 n 代祖先提交，只沿着第一个父提交进行遍历。例如，*<rev>~3* 等同于 *<rev>^^^*，即 *<rev>^1^1^1*。下面是此形式用法的示例。

- *<rev>^{<type>}*，例如 *v0.99.8^{commit}*

  后缀 *^* 后跟在大括号中的对象类型名称，表示递归地解引用 *<rev>* 直到找到类型为 *<type>* 的对象，或者对象无法再被解引用（在这种情况下报错）。例如，如果 *<rev>* 是提交样式（commit-ish），*<rev>^{commit}* 描述了相应的提交对象。类似地，如果 *<rev>* 是树样式（tree-ish），*<rev>^{tree}* 描述了相应的树对象。*<rev>^0* 是 *<rev>^{commit}* 的简写。*<rev>^{object}* 可用于确保 *<rev>* 是一个存在的对象，而不要求对 *<rev>* 进行解引用；因为标签本身就是一个对象，所以不需要解引用即可得到一个对象。*<rev>^{tag}* 可用于确保 *<rev>* 标识一个已存在的标签对象。

- *<rev>^{}*，例如 *v0.99.8^{}*

  后缀 *^* 后跟一个空的大括号对表示对象可能是一个标签，然后递归地解引用该标签，直到找到一个非标签对象为止。

- *<rev>^{/<text>}*，例如 *HEAD^{/fix nasty bug}*

  后缀 *^* 表示修订版本参数，后面跟随一个带有斜杠引导的文本的大括号对，与下面的 *:/fix nasty bug* 语法相同，只是它返回在 *^* 之前的 *<rev>* 可达的最年轻的匹配提交。

- *:/<text>*，例如 *:/fix nasty bug*

  冒号后面跟着斜杠，再后面跟着一个文本，表示提交消息与指定的正则表达式匹配的提交。此名称返回从任何引用（包括 HEAD）可达的最年轻的匹配提交。正则表达式可以匹配提交消息的任何部分。要匹配以某个字符串开头的消息，可以使用例如 *:/^foo*。特殊序列 *:/!* 保留为匹配的修饰符。*:/!-foo* 表示负匹配，而 *:/!!foo* 匹配一个字面上的 *!* 字符，后面跟着 *foo*。任何其他以 *:/!* 开头的序列现在也是保留的。根据给定的文本，Shell 的单词拆分规则可能需要额外的引用。

- *<rev>:<path>*，例如 *HEAD:README*，*master:./README*

  冒号后面跟着路径，在树对象名前的部分指定了树对象中给定路径的 blob 或树。以 *./* 或 *../* 开头的路径相对于当前工作目录。给定的路径将被转换为相对于工作树根目录的路径。这对于从与工作树具有相同树结构的提交或树中地址化 blob 或树非常有用。

- *:[<n>:]<path>*，例如 *:0:README*，*:README*

  冒号，后面可能跟着阶段号（0 到 3）和一个冒号，再后面跟着路径，指定了索引中给定路径的 blob 对象。缺少阶段号（和后面的冒号）表示阶段 0 条目。在合并过程中，阶段 1 是公共祖先，阶段 2 是目标分支版本（通常是当前分支），阶段 3 是正在合并的分支中的版本。


​	以下是 Jon Loeliger 绘制的示例。提交节点 B 和 C 都是提交节点 A 的父提交。父提交按从左到右的顺序排列。

```
G   H   I   J
 \ /     \ /
  D   E   F
   \  |  / \
    \ | /   |
     \|/    |
      B     C
       \   /
        \ /
         A
A =      = A^0
B = A^   = A^1     = A~1
C =      = A^2
D = A^^  = A^1^1   = A~2
E = B^2  = A^^2
F = B^3  = A^^3
G = A^^^ = A^1^1^1 = A~3
H = D^2  = B^^2    = A^^^2  = A~2^2
I = F^   = B^3^    = A^^3^
J = F^2  = B^3^2   = A^^3^2
```

## 指定范围

​	像 `git log` 这样的历史遍历命令会操作一组提交，而不仅仅是单个提交。

​	对于这些命令，使用前面描述的符号表示的单个修订版本意味着这些提交是从给定提交可达的。

​	指定多个修订版本意味着这些提交是从给定的任何一个提交可达的。

​	提交的可达集是该提交本身和其祖先链中的提交。

​	有几种符号用于指定一组连接的提交（称为“修订范围”），下面进行说明。

### 提交排除

- *^<rev>*（插入符号）符号

  要排除从一个提交可达的提交，使用前缀 *^* 符号。例如 *^r1 r2* 表示从 *r2* 可达的提交，但排除从 *r1* 可达的提交（即 *r1* 及其祖先）。


### 点范围表示法

- *..*（双点）范围表示法

  *^r1 r2* 集合操作出现得如此频繁，以至于有一个缩写形式。当有两个提交 *r1* 和 *r2*（根据上面“指定修订版本”的语法命名），您可以通过 *^r1 r2* 得到从 r2 可达的提交，并排除从 r1 可达的提交，这可以写成 *r1..r2*。

- *...*（三点）对称差异表示法

  类似的符号 *r1...r2* 称为 *r1* 和 *r2* 的对称差异，并被定义为 *r1 r2 --not $(git merge-base --all r1 r2)*。它是可达于 *r1*（左边）或 *r2*（右边）中的一方的提交集，但不可达于两者。


​	在这两种缩写表示法中，您可以省略一端，让它默认为 HEAD。例如，*origin..* 是 *origin..HEAD* 的缩写，询问“自从我从 origin 分支分叉以来我做了什么？”类似地，*..origin* 是 *HEAD..origin* 的缩写，询问“自从我从它们那里分叉以来 origin 做了什么？”请注意，*..* 表示 *HEAD..HEAD*，这是一个空范围，从 HEAD 可达，也从 HEAD 不可达。

​	虽然确实有一些专门设计用于采用两个不同范围的 “git” 命令（例如 “git range-diff R1 R2” 来比较两个范围），但它们是例外情况。除非另有说明，所有操作一组提交的“git”命令都适用于单个修订范围。换句话说，将两个“两点范围表示法”写在一起，例如：

``` bash
$ git log A..B C..D
```

对于大多数命令来说 **不** 表示两个修订范围。相反，它将命名一个单个的连接提交集，即从 B 或 D 可达，但从 A 或 C 不可达的提交集。在这样的线性历史中：

```
---A---B---o---o---C---D
```

因为 A 和 B 可达于 C，这两个点范围指定的修订范围是一个单个的提交 D。

### 其他 <rev>^ 父修饰符缩写



​	还有三个其他缩写，特别适用于合并提交，用于命名由一个提交及其父提交形成的集合。

​	*<rev>^@* 表示 *<rev>* 的所有父提交。

​	*<rev>^!* 包括提交 *<rev>*，但排除所有父提交。单独使用此符号表示仅包括提交 *<rev>*。

​	*<rev>^-<n>* 包括 *<rev>*，但排除第 n 个父提交（即 *<rev>^<n>..<rev>* 的简写），如果没有给出 *<n>*，则为 1。这通常对于合并提交非常有用，其中您可以只传递 *<commit>^-* 来获取合并提交 *<commit>* 中合并的分支中的所有提交（包括 *<commit>* 本身）。

​	虽然 *<rev>^<n>* 用于指定单个提交父级，但这三个符号也考虑它的父级。例如，可以使用 *HEAD^2^@*，但不能使用 *HEAD^@^2*。

## 修订范围摘要

- *<rev>*

  包括可由 <rev> 可达的提交（即 <rev> 及其祖先）。

- *^<rev>*

  排除可由 <rev> 可达的提交（即 <rev> 及其祖先）。

- *<rev1>..<rev2>*

  包括可由 <rev2> 可达的提交，但排除可由 <rev1> 可达的提交。当省略 <rev1> 或 <rev2> 时，默认为 `HEAD`。

- *<rev1>...<rev2>*

  包括可由 <rev1> 或 <rev2> 可达的提交，但排除可由两者都可达的提交。当省略 <rev1> 或 <rev2> 时，默认为 `HEAD`。

- *<rev>^@*, e.g. *HEAD^@*

  后缀 *^* 后面跟着一个 at 符号，表示列出 *<rev>* 的所有父提交（意味着包括可由其父提交可达的任何内容，但不包括提交本身）。

- *<rev>^!*, e.g. *HEAD^!*

  后缀 *^* 后面跟着一个感叹号，表示给定提交 *<rev>* 及其所有父提交，前面都带有 *^*，从中排除它们（和它们的祖先）。

- *<rev>^-<n>*, e.g. *HEAD^-, HEAD^-2*

  相当于 *<rev>^<n>..<rev>*，如果未给出 *<n>*，则为 1。

​	以上是使用上述 Loeliger 图解的一些示例，每一步符号的扩展和选择都仔细说明了。

```
   Args   Expanded arguments    Selected commits
   D                            G H D
   D F                          G H I J D F
   ^G D                         H D
   ^D B                         E I J F B
   ^D B C                       E I J F B C
   C                            I J F C
   B..C   = ^B C                C
   B...C  = B ^F C              G H D E B C
   B^-    = B^..B
	  = ^B^1 B              E I J F B
   C^@    = C^1
	  = F                   I J F
   B^@    = B^1 B^2 B^3
	  = D E F               D G H E F I J
   C^!    = C ^C^@
	  = C ^C^1
	  = C ^F                C
   B^!    = B ^B^@
	  = B ^B^1 ^B^2 ^B^3
	  = B ^D ^E ^F          B
   F^! D  = F ^I ^J D           G H D F
```

## 另请参阅

[git-rev-parse[1]](../../1/git-rev-parse)

## GIT

  这是[git[1]](../../Git)工具集中的一部分。