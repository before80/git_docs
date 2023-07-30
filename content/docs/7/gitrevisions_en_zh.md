+++
title = "gitrevisions——中英"
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

gitrevisions - Specifying revisions and ranges for Git

​	gitrevisions - 指定 Git 中的修订版本和范围

## 概要

gitrevisions

## 描述

Many Git commands take revision parameters as arguments. Depending on the command, they denote a specific commit or, for commands which walk the revision graph (such as [git-log[1\]](https://git-scm.com/docs/git-log)), all commits which are reachable from that commit. For commands that walk the revision graph one can also specify a range of revisions explicitly.

​	许多 Git 命令将修订版本参数作为参数。根据命令，它们表示特定的提交或者对于遍历修订版本图的命令（例如 [git-log[1\]](https://git-scm.com/docs/git-log)），表示所有从该提交可达的提交。对于遍历修订版本图的命令，还可以显式地指定一系列修订版本。

In addition, some Git commands (such as [git-show[1\]](https://git-scm.com/docs/git-show) and [git-push[1\]](https://git-scm.com/docs/git-push)) can also take revision parameters which denote other objects than commits, e.g. blobs ("files") or trees ("directories of files").

​	此外，一些 Git 命令（例如 [git-show[1\]](https://git-scm.com/docs/git-show) 和 [git-push[1\]](https://git-scm.com/docs/git-push)）还可以接受表示与提交不同类型的对象的修订版本参数，例如 blob（“文件”）或 tree（“文件目录”）。

## 指定修订版本

A revision parameter *<rev>* typically, but not necessarily, names a commit object. It uses what is called an *extended SHA-1* syntax. Here are various ways to spell object names. The ones listed near the end of this list name trees and blobs contained in a commit.

​	修订版本参数 *<rev>* 通常但不一定是一个提交对象的名称。它使用称为*扩展 SHA-1*语法。以下是表示对象名称的各种方式。列表末尾列出的那些名称表示包含在提交中的 tree 和 blob。

> Note
>
> This document shows the "raw" syntax as seen by git. The shell and other UIs might require additional quoting to protect special characters and to avoid word splitting.
>
> 注意
>
> ​	本文档显示 git 看到的“原始”语法。Shell 和其他用户界面可能需要额外的引号来保护特殊字符，并避免单词拆分。

- *<sha1>*, e.g. *dae86e1950b1277e545cee180551750029cfe735*, *dae86e*

- *<sha1>*，例如 *dae86e1950b1277e545cee180551750029cfe735*，*dae86e*

  The full SHA-1 object name (40-byte hexadecimal string), or a leading substring that is unique within the repository. E.g. dae86e1950b1277e545cee180551750029cfe735 and dae86e both name the same commit object if there is no other object in your repository whose object name starts with dae86e.

  完整的 SHA-1 对象名称（40 字节的十六进制字符串），或者在存储库中唯一的前导子串。例如，如果存储库中没有以 dae86e 开头的其他对象名称，则 dae86e1950b1277e545cee180551750029cfe735 和 dae86e 表示相同的提交对象。

- *<describeOutput>*, e.g. *v1.7.4.2-679-g3bee7fb*

- *<describeOutput>*，例如 *v1.7.4.2-679-g3bee7fb*

  Output from `git describe`; i.e. a closest tag, optionally followed by a dash and a number of commits, followed by a dash, a *g*, and an abbreviated object name.

  来自 `git describe` 的输出；即，最近的标签，可选地跟随破折号和一些提交数量，再跟随破折号、一个 *g* 和一个缩写的对象名称。

- *<refname>*, e.g. *master*, *heads/master*, *refs/heads/master*

- *<refname>*，例如 *master*，*heads/master*，*refs/heads/master*

  A symbolic ref name. E.g. *master* typically means the commit object referenced by *refs/heads/master*. If you happen to have both *heads/master* and *tags/master*, you can explicitly say *heads/master* to tell Git which one you mean. When ambiguous, a *<refname>* is disambiguated by taking the first match in the following rules:If *$GIT_DIR/<refname>* exists, that is what you mean (this is usually useful only for `HEAD`, `FETCH_HEAD`, `ORIG_HEAD`, `MERGE_HEAD` and `CHERRY_PICK_HEAD`);otherwise, *refs/<refname>* if it exists;otherwise, *refs/tags/<refname>* if it exists;otherwise, *refs/heads/<refname>* if it exists;otherwise, *refs/remotes/<refname>* if it exists;otherwise, *refs/remotes/<refname>/HEAD* if it exists.`HEAD` names the commit on which you based the changes in the working tree. `FETCH_HEAD` records the branch which you fetched from a remote repository with your last `git fetch` invocation. `ORIG_HEAD` is created by commands that move your `HEAD` in a drastic way (`git am`, `git merge`, `git rebase`, `git reset`), to record the position of the `HEAD` before their operation, so that you can easily change the tip of the branch back to the state before you ran them. `MERGE_HEAD` records the commit(s) which you are merging into your branch when you run `git merge`. `CHERRY_PICK_HEAD` records the commit which you are cherry-picking when you run `git cherry-pick`.Note that any of the *refs/** cases above may come either from the `$GIT_DIR/refs` directory or from the `$GIT_DIR/packed-refs` file. While the ref name encoding is unspecified, UTF-8 is preferred as some output processing may assume ref names in UTF-8.

  一个符号引用名称。例如，*master* 通常表示由 *refs/heads/master* 引用的提交对象。如果您碰巧同时拥有 *heads/master* 和 *tags/master*，可以显式地指定 *heads/master* 以告诉 Git 您指的是哪一个。在模糊时，*<refname>* 通过以下规则的第一个匹配来消除模糊：如果 *$GIT_DIR/<refname>* 存在，那就是您指的（这通常只对 `HEAD`、`FETCH_HEAD`、`ORIG_HEAD`、`MERGE_HEAD` 和 `CHERRY_PICK_HEAD` 有用）；否则，如果存在 *refs/<refname>*，那就是您指的；否则，如果存在 *refs/tags/<refname>*，那就是您指的；否则，如果存在 *refs/heads/<refname>*，那就是您指的；否则，如果存在 *refs/remotes/<refname>*，那就是您指的；否则，如果存在 *refs/remotes/<refname>/HEAD*，那就是您指的。`HEAD` 表示您在工作树中所基于的提交。`FETCH_HEAD` 记录您最后一次 `git fetch` 命令从远程存储库获取的分支。`ORIG_HEAD` 由将您的 `HEAD` 以 drastiv 方式移动的命令（`git am`、`git merge`、`git rebase`、`git reset`）创建，以记录其操作之前 `HEAD` 的位置，这样您就可以轻松地将分支的提示更改回运行它们之前的状态。`MERGE_HEAD` 记录在运行 `git merge` 时正在合并到您的分支的提交。`CHERRY_PICK_HEAD` 记录在运行 `git cherry-pick` 时正在进行樱桃拣选的提交。请注意，上述任何一个 *refs/** 都可能来自 `$GIT_DIR/refs` 目录或 `$GIT_DIR/packed-refs` 文件。尽管参考名称编码未指定，但推荐使用 UTF-8，因为一些输出处理可能会假定 UTF-8 编码的参考名称。

- *@*

  *@* alone is a shortcut for `HEAD`.

  *@* 单独是 `HEAD` 的一个快捷方式。

- *[<refname>]@{<date>}*, e.g. *master@{yesterday}*, *HEAD@{5 minutes ago}*

- *[<refname>]@{<date>}*，例如 *master@{yesterday}*，*HEAD@{5 minutes ago}*

  A ref followed by the suffix *@* with a date specification enclosed in a brace pair (e.g. *{yesterday}*, *{1 month 2 weeks 3 days 1 hour 1 second ago}* or *{1979-02-26 18:30:00}*) specifies the value of the ref at a prior point in time. This suffix may only be used immediately following a ref name and the ref must have an existing log (*$GIT_DIR/logs/<ref>*). Note that this looks up the state of your **local** ref at a given time; e.g., what was in your local *master* branch last week. If you want to look at commits made during certain times, see `--since` and `--until`.

  一个 ref，后面跟随后缀 *@*，并用大括号对括起来的日期说明（例如 *{yesterday}*、*{1 month 2 weeks 3 days 1 hour 1 second ago}* 或 *{1979-02-26 18:30:00}*），指定了先前时间点上 ref 的值。此后缀只能立即跟在 ref 名称后面，而且 ref 必须具有现有的日志（*$GIT_DIR/logs/<ref>*）。请注意，这会查找给定时间点上 **本地** ref 的状态；例如，上周您本地 *master* 分支中的内容。如果您想查看在特定时间内进行的提交，请参阅 `--since` 和 `--until`。

- *<refname>@{<n>}*, e.g. *master@{1}*

- *<refname>@{<n>}*，例如 *master@{1}*

  A ref followed by the suffix *@* with an ordinal specification enclosed in a brace pair (e.g. *{1}*, *{15}*) specifies the n-th prior value of that ref. For example *master@{1}* is the immediate prior value of *master* while *master@{5}* is the 5th prior value of *master*. This suffix may only be used immediately following a ref name and the ref must have an existing log (*$GIT_DIR/logs/<refname>*).

  一个 ref，后面跟随后缀 *@*，并用大括号对括起来的序数说明（例如 *{1}*、*{15}*），指定了该 ref 的第 n 个之前的值。例如 *master@{1}* 是 *master* 的前一个值，而 *master@{5}* 是 *master* 的第 5 个之前的值。此后缀只能立即跟在 ref 名称后面，而且 ref 必须具有现有的日志（*$GIT_DIR/logs/<refname>*）。

- *@{<n>}*, e.g. *@{1}*

- *@{<n>}*，例如 *@{1}*

  You can use the *@* construct with an empty ref part to get at a reflog entry of the current branch. For example, if you are on branch *blabla* then *@{1}* means the same as *blabla@{1}*.

  您可以使用 *@* 结构并且没有 ref 部分来访问当前分支的 reflog 条目。例如，如果您位于 *blabla* 分支上，则 *@{1}* 的含义与 *blabla@{1}* 相同。

- *@{-<n>}*, e.g. *@{-1}*

- *@{-<n>}*，例如 *@{-1}*

  The construct *@{-<n>}* means the <n>th branch/commit checked out before the current one.

  结构 *@{-<n>}* 表示当前分支之前第 n 个分支/提交检出的位置。

- *[<branchname>]@{upstream}*, e.g. *master@{upstream}*, *@{u}*

- *[<branchname>]@{upstream}*，例如 *master@{upstream}*，*@{u}*

  A branch B may be set up to build on top of a branch X (configured with `branch.<name>.merge`) at a remote R (configured with `branch.<name>.remote`). B@{u} refers to the remote-tracking branch for the branch X taken from remote R, typically found at `refs/remotes/R/X`.

  分支 B 可能被设置为在一个远程存储库 R 上构建（使用 `branch.<name>.merge` 配置），并且构建的分支可能在这个远程存储库中找到（通常位于 `refs/remotes/R/X`）。B@{u} 指的是从远程存储库 R 取得的分支 X 的远程跟踪分支。

- *[<branchname>]@{push}*, e.g. *master@{push}*, *@{push}*

- *[<branchname>]@{push}*，例如 *master@{push}*，*@{push}*

  The suffix *@{push}* reports the branch "where we would push to" if `git push` were run while `branchname` was checked out (or the current `HEAD` if no branchname is specified). Like for *@{upstream}*, we report the remote-tracking branch that corresponds to that branch at the remote.Here’s an example to make it more clear:

  后缀 *@{push}* 报告如果在检出了 branchname 的情况下运行 `git push`，则“我们将推送到的分支”。如果没有指定 branchname，则报告与远程对应的 remote-tracking 分支。就像 *@{upstream}* 一样，我们报告对应于该分支在远程的 remote-tracking 分支。下面是一个示例，以使其更清楚：

  ```
  $ git config push.default current
  $ git config remote.pushdefault myfork
  $ git switch -c mybranch origin/master
  
  $ git rev-parse --symbolic-full-name @{upstream}
  refs/remotes/origin/master
  
  $ git rev-parse --symbolic-full-name @{push}
  refs/remotes/myfork/mybranch
  ```

  Note in the example that we set up a triangular workflow, where we pull from one location and push to another. In a non-triangular workflow, *@{push}* is the same as *@{upstream}*, and there is no need for it.This suffix is also accepted when spelled in uppercase, and means the same thing no matter the case.

  注意在示例中，我们设置了一个三角形的工作流程，其中我们从一个位置拉取，然后推送到另一个位置。在非三角形的工作流程中，*@{push}* 与 *@{upstream}* 相同，无需使用它。这个后缀在大写拼写时也是被接受的，并且不管大小写，其含义都是相同的。

- *<rev>^[<n>]*, e.g. *HEAD^, v1.5.1^0*

- *<rev>^[<n>]*，例如 *HEAD^, v1.5.1^0*

  A suffix *^* to a revision parameter means the first parent of that commit object. *^<n>* means the <n>th parent (i.e. *<rev>^* is equivalent to *<rev>^1*). As a special rule, *<rev>^0* means the commit itself and is used when *<rev>* is the object name of a tag object that refers to a commit object.

  在修订版本参数后加上后缀 *^* 表示该提交对象的第一个父提交。*^<n>* 表示第 n 个父提交（即 *<rev>^* 等同于 *<rev>^1*）。作为特殊规则，*<rev>^0* 表示提交本身，用于当 *<rev>* 是指向提交对象的标签对象的对象名称时

- *<rev>~[<n>]*, e.g. *HEAD~, master~3*

- *<rev>~[<n>]*，例如 *HEAD~, master~3*

  A suffix *~* to a revision parameter means the first parent of that commit object. A suffix *~<n>* to a revision parameter means the commit object that is the <n>th generation ancestor of the named commit object, following only the first parents. I.e. *<rev>~3* is equivalent to *<rev>^^^* which is equivalent to *<rev>^1^1^1*. See below for an illustration of the usage of this form.

  在修订版本参数后加上后缀 *~* 表示该提交对象的第一个父提交。在修订版本参数后加上后缀 *~<n>* 表示指定的提交对象的第 n 代祖先提交，只沿着第一个父提交进行遍历。例如，*<rev>~3* 等同于 *<rev>^^^*，即 *<rev>^1^1^1*。下面是此形式用法的示例。

- *<rev>^{<type>}*, e.g. *v0.99.8^{commit}*

- *<rev>^{<type>}*，例如 *v0.99.8^{commit}*

  A suffix *^* followed by an object type name enclosed in brace pair means dereference the object at *<rev>* recursively until an object of type *<type>* is found or the object cannot be dereferenced anymore (in which case, barf). For example, if *<rev>* is a commit-ish, *<rev>^{commit}* describes the corresponding commit object. Similarly, if *<rev>* is a tree-ish, *<rev>^{tree}* describes the corresponding tree object. *<rev>^0* is a short-hand for *<rev>^{commit}*.*<rev>^{object}* can be used to make sure *<rev>* names an object that exists, without requiring *<rev>* to be a tag, and without dereferencing *<rev>*; because a tag is already an object, it does not have to be dereferenced even once to get to an object.*<rev>^{tag}* can be used to ensure that *<rev>* identifies an existing tag object.

  后缀 *^* 后跟在大括号中的对象类型名称，表示递归地解引用 *<rev>* 直到找到类型为 *<type>* 的对象，或者对象无法再被解引用（在这种情况下报错）。例如，如果 *<rev>* 是提交样式（commit-ish），*<rev>^{commit}* 描述了相应的提交对象。类似地，如果 *<rev>* 是树样式（tree-ish），*<rev>^{tree}* 描述了相应的树对象。*<rev>^0* 是 *<rev>^{commit}* 的简写。*<rev>^{object}* 可用于确保 *<rev>* 是一个存在的对象，而不要求对 *<rev>* 进行解引用；因为标签本身就是一个对象，所以不需要解引用即可得到一个对象。*<rev>^{tag}* 可用于确保 *<rev>* 标识一个已存在的标签对象。

- *<rev>^{}*, e.g. *v0.99.8^{}*

- *<rev>^{}*，例如 *v0.99.8^{}*

  A suffix *^* followed by an empty brace pair means the object could be a tag, and dereference the tag recursively until a non-tag object is found.

  后缀 *^* 后跟一个空的大括号对表示对象可能是一个标签，然后递归地解引用该标签，直到找到一个非标签对象为止。

- *<rev>^{/<text>}*, e.g. *HEAD^{/fix nasty bug}*

- *<rev>^{/<text>}*，例如 *HEAD^{/fix nasty bug}*

  A suffix *^* to a revision parameter, followed by a brace pair that contains a text led by a slash, is the same as the *:/fix nasty bug* syntax below except that it returns the youngest matching commit which is reachable from the *<rev>* before *^*.

  后缀 *^* 表示修订版本参数，后面跟随一个带有斜杠引导的文本的大括号对，与下面的 *:/fix nasty bug* 语法相同，只是它返回在 *^* 之前的 *<rev>* 可达的最年轻的匹配提交。

- *:/<text>*, e.g. *:/fix nasty bug*

- *:/<text>*，例如 *:/fix nasty bug*

  A colon, followed by a slash, followed by a text, names a commit whose commit message matches the specified regular expression. This name returns the youngest matching commit which is reachable from any ref, including HEAD. The regular expression can match any part of the commit message. To match messages starting with a string, one can use e.g. *:/^foo*. The special sequence *:/!* is reserved for modifiers to what is matched. *:/!-foo* performs a negative match, while *:/!!foo* matches a literal *!* character, followed by *foo*. Any other sequence beginning with *:/!* is reserved for now. Depending on the given text, the shell’s word splitting rules might require additional quoting.

  冒号后面跟着斜杠，再后面跟着一个文本，表示提交消息与指定的正则表达式匹配的提交。此名称返回从任何引用（包括 HEAD）可达的最年轻的匹配提交。正则表达式可以匹配提交消息的任何部分。要匹配以某个字符串开头的消息，可以使用例如 *:/^foo*。特殊序列 *:/!* 保留为匹配的修饰符。*:/!-foo* 表示负匹配，而 *:/!!foo* 匹配一个字面上的 *!* 字符，后面跟着 *foo*。任何其他以 *:/!* 开头的序列现在也是保留的。根据给定的文本，Shell 的单词拆分规则可能需要额外的引用。

- *<rev>:<path>*, e.g. *HEAD:README*, *master:./README*

- *<rev>:<path>*，例如 *HEAD:README*，*master:./README*

  A suffix *:* followed by a path names the blob or tree at the given path in the tree-ish object named by the part before the colon. A path starting with *./* or *../* is relative to the current working directory. The given path will be converted to be relative to the working tree’s root directory. This is most useful to address a blob or tree from a commit or tree that has the same tree structure as the working tree.

  冒号后面跟着路径，在树对象名前的部分指定了树对象中给定路径的 blob 或树。以 *./* 或 *../* 开头的路径相对于当前工作目录。给定的路径将被转换为相对于工作树根目录的路径。这对于从与工作树具有相同树结构的提交或树中地址化 blob 或树非常有用。

- *:[<n>:]<path>*, e.g. *:0:README*, *:README*

- *:[<n>:]<path>*，例如 *:0:README*，*:README*

  A colon, optionally followed by a stage number (0 to 3) and a colon, followed by a path, names a blob object in the index at the given path. A missing stage number (and the colon that follows it) names a stage 0 entry. During a merge, stage 1 is the common ancestor, stage 2 is the target branch’s version (typically the current branch), and stage 3 is the version from the branch which is being merged.
  
  冒号，后面可能跟着阶段号（0 到 3）和一个冒号，再后面跟着路径，指定了索引中给定路径的 blob 对象。缺少阶段号（和后面的冒号）表示阶段 0 条目。在合并过程中，阶段 1 是公共祖先，阶段 2 是目标分支版本（通常是当前分支），阶段 3 是正在合并的分支中的版本。

Here is an illustration, by Jon Loeliger. Both commit nodes B and C are parents of commit node A. Parent commits are ordered left-to-right.

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

History traversing commands such as `git log` operate on a set of commits, not just a single commit.

​	像 `git log` 这样的历史遍历命令会操作一组提交，而不仅仅是单个提交。

For these commands, specifying a single revision, using the notation described in the previous section, means the set of commits `reachable` from the given commit.

​	对于这些命令，使用前面描述的符号表示的单个修订版本意味着这些提交是从给定提交可达的。

Specifying several revisions means the set of commits reachable from any of the given commits.

​	指定多个修订版本意味着这些提交是从给定的任何一个提交可达的。

A commit’s reachable set is the commit itself and the commits in its ancestry chain.

​	提交的可达集是该提交本身和其祖先链中的提交。

There are several notations to specify a set of connected commits (called a "revision range"), illustrated below.

​	有几种符号用于指定一组连接的提交（称为“修订范围”），下面进行说明。

### 提交排除

- *^<rev>* (caret) Notation

- *^<rev>*（插入符号）符号

  To exclude commits reachable from a commit, a prefix *^* notation is used. E.g. *^r1 r2* means commits reachable from *r2* but exclude the ones reachable from *r1* (i.e. *r1* and its ancestors).
  
  要排除从一个提交可达的提交，使用前缀 *^* 符号。例如 *^r1 r2* 表示从 *r2* 可达的提交，但排除从 *r1* 可达的提交（即 *r1* 及其祖先）。

### 点范围表示法

- The *..* (two-dot) Range Notation

- *..*（双点）范围表示法

  The *^r1 r2* set operation appears so often that there is a shorthand for it. When you have two commits *r1* and *r2* (named according to the syntax explained in SPECIFYING REVISIONS above), you can ask for commits that are reachable from r2 excluding those that are reachable from r1 by *^r1 r2* and it can be written as *r1..r2*.

  *^r1 r2* 集合操作出现得如此频繁，以至于有一个缩写形式。当有两个提交 *r1* 和 *r2*（根据上面“指定修订版本”的语法命名），您可以通过 *^r1 r2* 得到从 r2 可达的提交，并排除从 r1 可达的提交，这可以写成 *r1..r2*。

- The *...* (three-dot) Symmetric Difference Notation

- *...*（三点）对称差异表示法

  A similar notation *r1...r2* is called symmetric difference of *r1* and *r2* and is defined as *r1 r2 --not $(git merge-base --all r1 r2)*. It is the set of commits that are reachable from either one of *r1* (left side) or *r2* (right side) but not from both.
  
  类似的符号 *r1...r2* 称为 *r1* 和 *r2* 的对称差异，并被定义为 *r1 r2 --not $(git merge-base --all r1 r2)*。它是可达于 *r1*（左边）或 *r2*（右边）中的一方的提交集，但不可达于两者。

In these two shorthand notations, you can omit one end and let it default to HEAD. For example, *origin..* is a shorthand for *origin..HEAD* and asks "What did I do since I forked from the origin branch?" Similarly, *..origin* is a shorthand for *HEAD..origin* and asks "What did the origin do since I forked from them?" Note that *..* would mean *HEAD..HEAD* which is an empty range that is both reachable and unreachable from HEAD.

​	在这两种缩写表示法中，您可以省略一端，让它默认为 HEAD。例如，*origin..* 是 *origin..HEAD* 的缩写，询问“自从我从 origin 分支分叉以来我做了什么？”类似地，*..origin* 是 *HEAD..origin* 的缩写，询问“自从我从它们那里分叉以来 origin 做了什么？”请注意，*..* 表示 *HEAD..HEAD*，这是一个空范围，从 HEAD 可达，也从 HEAD 不可达。

Commands that are specifically designed to take two distinct ranges (e.g. "git range-diff R1 R2" to compare two ranges) do exist, but they are exceptions. Unless otherwise noted, all "git" commands that operate on a set of commits work on a single revision range. In other words, writing two "two-dot range notation" next to each other, e.g.

​	虽然确实有一些专门设计用于采用两个不同范围的 “git” 命令（例如 “git range-diff R1 R2” 来比较两个范围），但它们是例外情况。除非另有说明，所有操作一组提交的“git”命令都适用于单个修订范围。换句话说，将两个“两点范围表示法”写在一起，例如：

```
$ git log A..B C..D
```

does **not** specify two revision ranges for most commands. Instead it will name a single connected set of commits, i.e. those that are reachable from either B or D but are reachable from neither A or C. In a linear history like this:

对于大多数命令来说 **不** 表示两个修订范围。相反，它将命名一个单个的连接提交集，即从 B 或 D 可达，但从 A 或 C 不可达的提交集。在这样的线性历史中：

```
---A---B---o---o---C---D
```

because A and B are reachable from C, the revision range specified by these two dotted ranges is a single commit D.

因为 A 和 B 可达于 C，这两个点范围指定的修订范围是一个单个的提交 D。

### 其他 <rev>^ 父修饰符缩写



Three other shorthands exist, particularly useful for merge commits, for naming a set that is formed by a commit and its parent commits.

​	还有三个其他缩写，特别适用于合并提交，用于命名由一个提交及其父提交形成的集合。

The *r1^@* notation means all parents of *r1*.

​	*<rev>^@* 表示 *<rev>* 的所有父提交。

The *r1^!* notation includes commit *r1* but excludes all of its parents. By itself, this notation denotes the single commit *r1*.

​	*<rev>^!* 包括提交 *<rev>*，但排除所有父提交。单独使用此符号表示仅包括提交 *<rev>*。

The *<rev>^-[<n>]* notation includes *<rev>* but excludes the <n>th parent (i.e. a shorthand for *<rev>^<n>..<rev>*), with *<n>* = 1 if not given. This is typically useful for merge commits where you can just pass *<commit>^-* to get all the commits in the branch that was merged in merge commit *<commit>* (including *<commit>* itself).

​	*<rev>^-<n>* 包括 *<rev>*，但排除第 n 个父提交（即 *<rev>^<n>..<rev>* 的简写），如果没有给出 *<n>*，则为 1。这通常对于合并提交非常有用，其中您可以只传递 *<commit>^-* 来获取合并提交 *<commit>* 中合并的分支中的所有提交（包括 *<commit>* 本身）。

While *<rev>^<n>* was about specifying a single commit parent, these three notations also consider its parents. For example you can say *HEAD^2^@*, however you cannot say *HEAD^@^2*.

​	虽然 *<rev>^<n>* 用于指定单个提交父级，但这三个符号也考虑它的父级。例如，可以使用 *HEAD^2^@*，但不能使用 *HEAD^@^2*。

## 修订范围摘要

- *<rev>*

  Include commits that are reachable from <rev> (i.e. <rev> and its ancestors).

  包括可由 <rev> 可达的提交（即 <rev> 及其祖先）。

- *^<rev>*

  Exclude commits that are reachable from <rev> (i.e. <rev> and its ancestors).

  排除可由 <rev> 可达的提交（即 <rev> 及其祖先）。

- *<rev1>..<rev2>*

  Include commits that are reachable from <rev2> but exclude those that are reachable from <rev1>. When either <rev1> or <rev2> is omitted, it defaults to `HEAD`.

  包括可由 <rev2> 可达的提交，但排除可由 <rev1> 可达的提交。当省略 <rev1> 或 <rev2> 时，默认为 `HEAD`。

- *<rev1>...<rev2>*

  Include commits that are reachable from either <rev1> or <rev2> but exclude those that are reachable from both. When either <rev1> or <rev2> is omitted, it defaults to `HEAD`.

  包括可由 <rev1> 或 <rev2> 可达的提交，但排除可由两者都可达的提交。当省略 <rev1> 或 <rev2> 时，默认为 `HEAD`。

- *<rev>^@*, e.g. *HEAD^@*

  A suffix *^* followed by an at sign is the same as listing all parents of *<rev>* (meaning, include anything reachable from its parents, but not the commit itself).

  后缀 *^* 后面跟着一个 at 符号，表示列出 *<rev>* 的所有父提交（意味着包括可由其父提交可达的任何内容，但不包括提交本身）。

- *<rev>^!*, e.g. *HEAD^!*

  A suffix *^* followed by an exclamation mark is the same as giving commit *<rev>* and all its parents prefixed with *^* to exclude them (and their ancestors).

  后缀 *^* 后面跟着一个感叹号，表示给定提交 *<rev>* 及其所有父提交，前面都带有 *^*，从中排除它们（和它们的祖先）。

- *<rev>^-<n>*, e.g. *HEAD^-, HEAD^-2*

  Equivalent to *<rev>^<n>..<rev>*, with *<n>* = 1 if not given.
  
  相当于 *<rev>^<n>..<rev>*，如果未给出 *<n>*，则为 1。

Here are a handful of examples using the Loeliger illustration above, with each step in the notation’s expansion and selection carefully spelt out:

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