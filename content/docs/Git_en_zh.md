+++
title = "Git——中英对照版"
weight = 30
type = "docs"
date = 2023-07-26T09:47:23+08:00
description = ""
isCJKLanguage = true
draft = false

+++


https://git-scm.com/docs/git

## 名称

git - the stupid content tracker

git - 愚蠢的内容追踪器

## 用法概要

```
git [-v | --version] [-h | --help] [-C <path>] [-c <name>=<value>]
    [--exec-path[=<path>]] [--html-path] [--man-path] [--info-path]
    [-p|--paginate|-P|--no-pager] [--no-replace-objects] [--bare]
    [--git-dir=<path>] [--work-tree=<path>] [--namespace=<name>]
    [--config-env=<name>=<envvar>] <command> [<args>]
```

## 描述

Git is a fast, scalable, distributed revision control system with an unusually rich command set that provides both high-level operations and full access to internals.

​	Git是一个快速、可扩展、分布式的版本控制系统，具有异常丰富的命令集，提供高级操作和完全访问内部的能力。

See [gittutorial[7]](../7/gittutorial) to get started, then see [giteveryday[7]](../7/giteveryday) for a useful minimum set of commands. The [Git User’s Manual](https://git-scm.com/docs/user-manual) has a more in-depth introduction.

​	查看[gittutorial[7]](../7/gittutorial)来开始使用，然后查看[giteveryday[7]](../7/giteveryday)获取一个有用的最小命令集。[Git用户手册](https://git-scm.com/docs/user-manual)提供了更详细的介绍。

After you mastered the basic concepts, you can come back to this page to learn what commands Git offers. You can learn more about individual Git commands with "git help command". [gitcli[7]](../7/gitcli) manual page gives you an overview of the command-line command syntax.

​	在掌握了基本概念后，您可以回到这个页面了解Git提供了哪些命令。您可以使用 "git help command" 命令了解有关单个Git命令的更多信息。[gitcli[7]](../7/gitcli) 手册页面提供了有关命令行命令语法的概述。

A formatted and hyperlinked copy of the latest Git documentation can be viewed at https://git.github.io/htmldocs/git.html or https://git-scm.com/docs.

​	最新的Git文档格式化并带有超链接，可在https://git.github.io/htmldocs/git.html或https://git-scm.com/docs上查看。

## 选项

- -v

- `--version`

  Prints the Git suite version that the *git* program came from.This option is internally converted to `git version ...` and accepts the same options as the [git-version[1]](../1/git-version) command. If `--help` is also given, it takes precedence over `--version`.

  打印 *git* 程序所属的Git套件版本。此选项在内部转换为 `git version ...`，并接受与 [git-version[1]](../1/git-version) 命令相同的选项。如果同时给出 `--help`，则 `--version` 会优先于 `--help`。

- -h

- `--help`

  Prints the synopsis and a list of the most commonly used commands. If the option `--all` or `-a` is given then all available commands are printed. If a Git command is named this option will bring up the manual page for that command.Other options are available to control how the manual page is displayed. See [git-help[1]](../1/git-help) for more information, because `git --help ...` is converted internally into `git help ...`.

  打印简介和最常用的命令列表。如果给出了选项 `--all` 或 `-a`，则打印所有可用的命令。如果命名了一个Git命令，此选项将显示该命令的手册页。其他选项可用于控制手册页的显示方式。有关更多信息，请参阅 [git-help[1]](../1/git-help)，因为 `git --help ...` 在内部转换为 `git help ...`。

- -C <path>

  Run as if git was started in *<path>* instead of the current working directory. When multiple `-C` options are given, each subsequent non-absolute `-C <path>` is interpreted relative to the preceding `-C <path>`. If *<path>* is present but empty, e.g. `-C ""`, then the current working directory is left unchanged.This option affects options that expect path name like `--git-dir` and `--work-tree` in that their interpretations of the path names would be made relative to the working directory caused by the `-C` option. For example the following invocations are equivalent:`git --git-dir=a.git --work-tree=b -C c status git --git-dir=c/a.git --work-tree=c/b status`

  以 *<path>* 目录为起点运行git，而不是当前工作目录。当给出多个 `-C` 选项时，每个后续的非绝对路径 `-C <path>` 都相对于前面的 `-C <path>` 进行解释。如果 *<path>* 存在但为空（例如 `-C ""`），则当前工作目录保持不变。此选项会影响预期路径名称的选项，例如 `--git-dir` 和 `--work-tree`，使得它们的路径名称相对于 `-C` 选项引起的工作目录来解释。例如，以下调用是等效的：`git --git-dir=a.git --work-tree=b -C c status git --git-dir=c/a.git --work-tree=c/b status`

- -c <name>=<value>

  Pass a configuration parameter to the command. The value given will override values from configuration files. The <name> is expected in the same format as listed by *git config* (subkeys separated by dots).Note that omitting the `=` in `git -c foo.bar ...` is allowed and sets `foo.bar` to the boolean true value (just like `[foo]bar` would in a config file). Including the equals but with an empty value (like `git -c foo.bar= ...`) sets `foo.bar` to the empty string which `git config --type=bool` will convert to `false`.

  向命令传递配置参数。给定的值将覆盖配置文件中的值。*<name>* 的格式应与 *git config* 中列出的格式相同（由点分隔的子键）。请注意，允许在 `git -c foo.bar ...` 中省略 `=`，并将 `foo.bar` 设置为布尔值true（就像在配置文件中的`[foo]bar`那样）。包含等号，但值为空（例如 `git -c foo.bar= ...`）将 `foo.bar` 设置为空字符串，`git config --type=bool` 将会将其转换为false。

- `--config-env=<name>=<envvar>`

  Like `-c <name>=<value>`, give configuration variable *<name>* a value, where <envvar> is the name of an environment variable from which to retrieve the value. Unlike `-c` there is no shortcut for directly setting the value to an empty string, instead the environment variable itself must be set to the empty string. It is an error if the `<envvar>` does not exist in the environment. `<envvar>` may not contain an equals sign to avoid ambiguity with `<name>` containing one.This is useful for cases where you want to pass transitory configuration options to git, but are doing so on OS’s where other processes might be able to read your cmdline (e.g. `/proc/self/cmdline`), but not your environ (e.g. `/proc/self/environ`). That behavior is the default on Linux, but may not be on your system.Note that this might add security for variables such as `http.extraHeader` where the sensitive information is part of the value, but not e.g. `url.<base>.insteadOf` where the sensitive information can be part of the key.

  与 `-c <name>=<value>` 类似，为配置变量 *<name>* 提供一个值，其中 *<envvar>* 是要从中检索值的环境变量的名称。与 `-c` 不同，不能直接将值设置为空字符串，而是必须将环境变量本身设置为空字符串。如果环境中不存在 `<envvar>`，则是一个错误。为了避免与包含等号的 *<name>* 产生歧义，*<envvar>* 不得包含等号。这对于您想向Git传递瞬态配置选项但在OS上这样做时（例如 `/proc/self/cmdline`），其他进程可能能够读取您的命令行（例如 `/proc/self/environ`），但不能读取您的环境。这在Linux上是默认行为，但可能不是在您的系统上。请注意，这可能会增加`http.extraHeader`等变量的安全性，其中敏感信息是值的一部分，但不会增加`url.<base>.insteadOf`等变量的安全性，其中敏感信息可以成为键的一部分。

- `--exec-path[=<path>]`

  Path to wherever your core Git programs are installed. This can also be controlled by setting the GIT_EXEC_PATH environment variable. If no path is given, *git* will print the current setting and then exit.

  指定核心Git程序所安装的路径。这也可以通过设置GIT_EXEC_PATH环境变量来控制。如果没有给出路径，git将打印当前设置并退出。

- `--html-path`

  Print the path, without trailing slash, where Git’s HTML documentation is installed and exit.

  打印Git的HTML文档安装路径（不包含尾部斜线），然后退出。

- `--man-path`

  Print the manpath (see `man(1)`) for the man pages for this version of Git and exit.

  打印本版本Git的man页面路径（参见`man(1)`），然后退出。

- `--info-path`

  Print the path where the Info files documenting this version of Git are installed and exit.

  打印安装有本版本Git信息文件的路径，然后退出。

- -p

- `--paginate`

  Pipe all output into *less* (or if set, $PAGER) if standard output is a terminal. This overrides the `pager.<cmd>` configuration options (see the "Configuration Mechanism" section below).

  如果标准输出是终端，则将所有输出通过*less*（或者如果设置了$PAGER，则通过$PAGER）分页显示。这将覆盖配置选项`pager.<cmd>`（请参阅下面的"配置机制"部分）。

- -P

- `--no-pager`

  Do not pipe Git output into a pager.

  不要将Git输出传递给分页器。

- `--git-dir=<path>`

  Set the path to the repository (".git" directory). This can also be controlled by setting the `GIT_DIR` environment variable. It can be an absolute path or relative path to current working directory.Specifying the location of the ".git" directory using this option (or `GIT_DIR` environment variable) turns off the repository discovery that tries to find a directory with ".git" subdirectory (which is how the repository and the top-level of the working tree are discovered), and tells Git that you are at the top level of the working tree. If you are not at the top-level directory of the working tree, you should tell Git where the top-level of the working tree is, with the `--work-tree=<path>` option (or `GIT_WORK_TREE` environment variable)If you just want to run git as if it was started in `<path>` then use `git -C <path>`.

  设置仓库（".git"目录）的路径。这也可以通过设置GIT_DIR环境变量来控制。它可以是绝对路径或相对于当前工作目录的相对路径。使用此选项（或GIT_DIR环境变量）指定“.git”目录的位置（这会关闭试图找到具有“.git”子目录的目录（这是查找仓库和工作树顶级的方法），并告诉Git您正在工作树的顶级位置。如果您不在工作树的顶级目录中，则应该使用 `--work-tree=<path>` 选项（或 `GIT_WORK_TREE` 环境变量）告诉Git工作树的顶级位置。如果您只想在 `<path>` 中运行git，可以使用 `git -C <path>`。

- `--work-tree=<path>`

  Set the path to the working tree. It can be an absolute path or a path relative to the current working directory. This can also be controlled by setting the GIT_WORK_TREE environment variable and the core.worktree configuration variable (see core.worktree in [git-config[1]](../1/git-config) for a more detailed discussion).

  设置工作树的路径。它可以是绝对路径或相对于当前工作目录的路径。这也可以通过设置GIT_WORK_TREE环境变量和core.worktree配置变量来控制（有关更详细的讨论，请参阅[git-config[1]](../1/git-config)中的core.worktree）。

- `--namespace=<path>`

  Set the Git namespace. See [gitnamespaces[7]](../7/gitnamespaces) for more details. Equivalent to setting the `GIT_NAMESPACE` environment variable.

  设置Git命名空间。有关更多详细信息，请参阅[gitnamespaces[7]](../7/gitnamespaces)。等同于设置 `GIT_NAMESPACE` 环境变量。

- `--bare`

  Treat the repository as a bare repository. If GIT_DIR environment is not set, it is set to the current working directory.

  将仓库视为裸仓库。如果未设置GIT_DIR环境变量，它将被设置为当前工作目录。

- `--no-replace-objects`

  Do not use replacement refs to replace Git objects. See [git-replace[1]](../1/git-replace) for more information.

  不使用替代引用来替换Git对象。有关更多信息，请参阅[git-replace[1]](../1/git-replace)。

- `--literal-pathspecs`

  Treat pathspecs literally (i.e. no globbing, no pathspec magic). This is equivalent to setting the `GIT_LITERAL_PATHSPECS` environment variable to `1`.

  字面对待路径规范（即没有通配符，没有路径规范魔法）。这等同于将 `GIT_LITERAL_PATHSPECS` 环境变量设置为 `1`。

- `--glob-pathspecs`

  Add "glob" magic to all pathspec. This is equivalent to setting the `GIT_GLOB_PATHSPECS` environment variable to `1`. Disabling globbing on individual pathspecs can be done using pathspec magic ":(literal)"

  对所有路径规范添加"glob"魔法。这等同于将 `GIT_GLOB_PATHSPECS` 环境变量设置为 `1`。使用路径规范魔法“:(literal)”可以在单个路径规范上禁用通配符。

- `--noglob-pathspecs`

  Add "literal" magic to all pathspec. This is equivalent to setting the `GIT_NOGLOB_PATHSPECS` environment variable to `1`. Enabling globbing on individual pathspecs can be done using pathspec magic ":(glob)"

  对所有路径规范添加"literal"魔法。这等同于将 `GIT_NOGLOB_PATHSPECS` 环境变量设置为 `1`。使用路径规范魔法“:(glob)”可以在单个路径规范上启用通配符。

- `--icase-pathspecs`

  Add "icase" magic to all pathspec. This is equivalent to setting the `GIT_ICASE_PATHSPECS` environment variable to `1`.

  对所有路径规范添加"icase"魔法。这等同于将 `GIT_ICASE_PATHSPECS` 环境变量设置为 `1`。

- `--no-optional-locks`

  Do not perform optional operations that require locks. This is equivalent to setting the `GIT_OPTIONAL_LOCKS` to `0`.

  不执行需要锁定的可选操作。这等同于将 `GIT_OPTIONAL_LOCKS` 设置为 `0`。

- `--list-cmds=group[,group…]`

  List commands by group. This is an internal/experimental option and may change or be removed in the future. Supported groups are: builtins, parseopt (builtin commands that use parse-options), main (all commands in libexec directory), others (all other commands in `$PATH` that have git- prefix), list-<category> (see categories in command-list.txt), nohelpers (exclude helper commands), alias and config (retrieve command list from config variable completion.commands)

  按组列出命令。这是一个内部/实验性选项，可能会在将来更改或删除。支持的组有：builtins、parseopt（使用parse-options的内置命令），main（libexec目录中的所有命令），others（$PATH中有git-前缀的所有其他命令），list-<category>（请参阅command-list.txt中的类别），nohelpers（排除助手命令），alias和config（从配置变量completion.commands中检索命令列表）

- `--attr-source=<tree-ish>`

  Read gitattributes from <tree-ish> instead of the worktree. See [gitattributes[5]](../5/gitattributes). This is equivalent to setting the `GIT_ATTR_SOURCE` environment variable.
  
  从 *<tree-ish>* 中读取gitattributes，而不是工作树。有关详细信息，请参阅[gitattributes[5]](../5/gitattributes)。这等同于设置 `GIT_ATTR_SOURCE` 环境变量。

## Git命令

We divide Git into high level ("porcelain") commands and low level ("plumbing") commands.

​	我们将Git分为高级（"porcelain"）命令和低级（"plumbing"）命令。

## 高级命令（porcelain）

We separate the porcelain commands into the main commands and some ancillary user utilities.

​	我们将"porcelain"命令分为主要命令和一些辅助用户工具。

### 主要porcelain命令

- [git-add[1]](../1/git-add)

  Add file contents to the index

  将文件内容添加到索引

- [git-am[1]](../1/git-am)

  Apply a series of patches from a mailbox

  从邮箱应用一系列补丁

- [git-archive[1]](../1/git-archive)

  Create an archive of files from a named tree

  从指定的树创建一个文件归档

- [git-bisect[1]](../1/git-bisect)

  Use binary search to find the commit that introduced a bug

  使用二分查找找出引入错误的提交

- [git-branch[1]](../1/git-branch)

  List, create, or delete branches

  列出、创建或删除分支

- [git-bundle[1]](../1/git-bundle)

  Move objects and refs by archive

  通过归档移动对象和引用

- [git-checkout[1]](../1/git-checkout)

  Switch branches or restore working tree files

  切换分支或还原工作树文件

- [git-cherry-pick[1]](../1/git-cherry-pick)

  Apply the changes introduced by some existing commits

  应用已存在提交引入的更改

- [git-citool[1]](../1/git-citool)

  Graphical alternative to git-commit

  git-commit的图形界面替代品

- [git-clean[1]](../1/git-clean)

  Remove untracked files from the working tree

  从工作树中删除未跟踪的文件

- [git-clone[1]](../1/git-clone)

  Clone a repository into a new directory

  克隆仓库到新目录

- [git-commit[1]](../1/git-commit)

  Record changes to the repository

  记录对仓库的更改

- [git-describe[1]](../1/git-describe)

  Give an object a human readable name based on an available ref

  根据可用的引用为对象提供人类可读的名称

- [git-diff[1]](../1/git-diff)

  Show changes between commits, commit and working tree, etc

  显示提交之间的更改、提交和工作树等的差异

- [git-fetch[1]](../1/git-fetch)

  Download objects and refs from another repository

  从另一个仓库下载对象和引用

- [git-format-patch[1]](../1/git-format-patch)

  Prepare patches for e-mail submission

  为电子邮件提交准备补丁

- [git-gc[1]](../1/git-gc)

  Cleanup unnecessary files and optimize the local repository

  清理不必要的文件并优化本地仓库

- [git-grep[1]](../1/git-grep)

  Print lines matching a pattern

  打印匹配模式的行

- [git-gui[1]](../1/git-gui)

  A portable graphical interface to Git

  Git的便携式图形界面

- [git-init[1]](../1/git-init)

  Create an empty Git repository or reinitialize an existing one

  创建一个空的Git仓库或重新初始化现有的仓库

- [git-log[1]](../1/git-log)

  Show commit logs

  显示提交日志

- [git-maintenance[1]](../1/git-maintenance)

  Run tasks to optimize Git repository data

  运行任务来优化Git仓库数据

- [git-merge[1]](../1/git-merge)

  Join two or more development histories together

  将两个或多个开发历史记录合并在一起

- [git-mv[1]](../1/git-mv)

  Move or rename a file, a directory, or a symlink

  移动或重命名文件、目录或符号链接

- [git-notes[1]](../1/git-notes)

  Add or inspect object notes

  添加或检查对象注释

- [git-pull[1]](../1/git-pull)

  Fetch from and integrate with another repository or a local branch

  从另一个仓库或本地分支获取并整合

- [git-push[1]](../1/git-push)

  Update remote refs along with associated objects

  更新远程引用以及相关联的对象

- [git-range-diff[1]](../1/git-range-diff)

  Compare two commit ranges (e.g. two versions of a branch)

  比较两个提交范围（例如分支的两个版本）

- [git-rebase[1]](../1/git-rebase)

  Reapply commits on top of another base tip

  将提交重新应用到另一个基本提示之上

- [git-reset[1]](../1/git-reset)

  Reset current HEAD to the specified state

  将当前HEAD重置为指定状态

- [git-restore[1]](../1/git-restore)

  Restore working tree files

  还原工作树文件

- [git-revert[1]](../1/git-revert)

  Revert some existing commits

  撤消一些现有的提交

- [git-rm[1]](../1/git-rm)

  Remove files from the working tree and from the index

  从工作树和索引中删除文件

- [git-shortlog[1]](../1/git-shortlog)

  Summarize *git log* output

  汇总 *git log* 的输出

- [git-show[1]](../1/git-show)

  Show various types of objects

  显示各种类型的对象

- [git-sparse-checkout[1]](../1/git-sparse-checkout)

  Reduce your working tree to a subset of tracked files

  将工作树减少为跟踪文件的子集

- [git-stash[1]](../1/git-stash)

  Stash the changes in a dirty working directory away

  将脏工作目录中的更改保存

- [git-status[1]](../1/git-status)

  Show the working tree status

  显示工作树状态

- [git-submodule[1]](../1/git-submodule)

  Initialize, update or inspect submodules

  初始化、更新或检查子模块

- [git-switch[1]](../1/git-switch)

  Switch branches

  切换分支

- [git-tag[1]](../1/git-tag)

  Create, list, delete or verify a tag object signed with GPG

  创建、列出、删除或验证带有GPG签名的标签对象

- [git-worktree[1]](../1/git-worktree)

  Manage multiple working trees

  管理多个工作树

- [gitk[1]](../1/gitk)

  The Git repository browser

  Git仓库浏览器

- [scalar[1]](../1/scalar)

  A tool for managing large Git repositories
  
  用于管理大型Git仓库的工具

### 辅助命令

Manipulators:

操纵命令：

- [git-config[1]](../1/git-config)

  Get and set repository or global options

  获取并设置仓库或全局选项

- [git-fast-export[1]](../1/git-fast-export)

  Git data exporter

  Git数据导出器

- [git-fast-import[1]](../1/git-fast-import)

  Backend for fast Git data importers

  快速Git数据导入器的后端

- [git-filter-branch[1]](../1/git-filter-branch)

  Rewrite branches

  重写分支

- [git-mergetool[1]](../1/git-mergetool)

  Run merge conflict resolution tools to resolve merge conflicts

  运行合并冲突解决工具来解决合并冲突

- [git-pack-refs[1]](../1/git-pack-refs)

  Pack heads and tags for efficient repository access

  为高效访问仓库打包头和标签

- [git-prune[1]](../1/git-prune)

  Prune all unreachable objects from the object database

  从对象数据库中清除所有不可访问的对象

- [git-reflog[1]](../1/git-reflog)

  Manage reflog information

  管理reflog信息

- [git-remote[1]](../1/git-remote)

  Manage set of tracked repositories

  管理跟踪的仓库集

- [git-repack[1]](../1/git-repack)

  Pack unpacked objects in a repository

  将未打包的对象打包到仓库中

- [git-replace[1]](../1/git-replace)

  Create, list, delete refs to replace objects
  
  创建、列出、删除替换对象的引用

Interrogators:

询问命令：

- [git-annotate[1]](../1/git-annotate)

  Annotate file lines with commit information

  为文件行添加提交信息的注解

- [git-blame[1]](../1/git-blame)

  Show what revision and author last modified each line of a file

  显示每行文件最后修改的修订版本和作者

- [git-bugreport[1]](../1/git-bugreport)

  Collect information for user to file a bug report

  收集用户的信息以便提交错误报告

- [git-count-objects[1]](../1/git-count-objects)

  Count unpacked number of objects and their disk consumption

  统计未压缩对象的数量及其磁盘占用量

- [git-diagnose[1]](../1/git-diagnose)

  Generate a zip archive of diagnostic information

  生成包含诊断信息的zip存档

- [git-difftool[1]](../1/git-difftool)

  Show changes using common diff tools

  使用常见的diff工具显示更改

- [git-fsck[1]](../1/git-fsck)

  Verifies the connectivity and validity of the objects in the database

  验证数据库中对象的连通性和有效性

- [git-help[1]](../1/git-help)

  Display help information about Git

  显示有关Git的帮助信息

- [git-instaweb[1]](../1/git-instaweb)

  Instantly browse your working repository in gitweb

  即时在gitweb中浏览您的工作仓库

- [git-merge-tree[1]](../1/git-merge-tree)

  Perform merge without touching index or working tree

  执行合并而不影响索引或工作树

- [git-rerere[1]](../1/git-rerere)

  Reuse recorded resolution of conflicted merges

  重用记录的解决冲突合并结果

- [git-show-branch[1]](../1/git-show-branch)

  Show branches and their commits

  显示分支及其提交

- [git-verify-commit[1]](../1/git-verify-commit)

  Check the GPG signature of commits

  验证提交的GPG签名

- [git-verify-tag[1]](../1/git-verify-tag)

  Check the GPG signature of tags

  验证标签的GPG签名

- [git-version[1]](../1/git-version)

  Display version information about Git

  显示有关Git的版本信息

- [git-whatchanged[1]](../1/git-whatchanged)

  Show logs with difference each commit introduces

  显示每个提交引入的差异日志

- [gitweb[1]](../1/gitweb)

  Git web interface (web frontend to Git repositories)
  
  Git网络接口（Git仓库的Web前端）

### 与他人交互

These commands are to interact with foreign SCM and with other people via patch over e-mail.

​	这些命令用于与其他SCM以及通过电子邮件补丁与他人进行交互。

- [git-archimport[1]](../1/git-archimport)

  Import a GNU Arch repository into Git

  将GNU Arch仓库导入到Git中

- [git-cvsexportcommit[1]](../1/git-cvsexportcommit)

  Export a single commit to a CVS checkout

  将单个提交导出到CVS检出

- [git-cvsimport[1]](../1/git-cvsimport)

  Salvage your data out of another SCM people love to hate

  从其他人厌恶的SCM中拯救数据

- [git-cvsserver[1]](../1/git-cvsserver)

  A CVS server emulator for Git

  用于Git的CVS服务器仿真器

- [git-imap-send[1]](../1/git-imap-send)

  Send a collection of patches from stdin to an IMAP folder

  将一系列补丁从stdin发送到IMAP文件夹

- [git-p4[1]](../1/git-p4)

  Import from and submit to Perforce repositories

  与Perforce仓库进行导入和提交

- [git-quiltimport[1]](../1/git-quiltimport)

  Applies a quilt patchset onto the current branch

  将quilt补丁集应用于当前分支

- [git-request-pull[1]](../1/git-request-pull)

  Generates a summary of pending changes

  生成待处理更改的摘要

- [git-send-email[1]](../1/git-send-email)

  Send a collection of patches as emails

  将一系列补丁作为电子邮件发送

- [git-svn[1]](../1/git-svn)

  Bidirectional operation between a Subversion repository and Git
  
  Subversion仓库与Git之间的双向操作

### 重置、恢复和撤销 Reset, restore and revert

There are three commands with similar names: `git reset`, `git restore` and `git revert`.

​	有三个命令具有相似的名称：`git reset`、`git restore`和`git revert`。

- [git-revert[1]](../1/git-revert) is about making a new commit that reverts the changes made by other commits.

- [git-revert[1]](../1/git-revert) 用于创建一个新的提交，撤消其他提交所做的更改。

- [git-restore[1]](../1/git-restore) is about restoring files in the working tree from either the index or another commit. This command does not update your branch. The command can also be used to restore files in the index from another commit.

- [git-restore[1]](../1/git-restore) 用于从索引或另一个提交中恢复工作树中的文件。该命令不会更新您的分支。此命令还可用于从其他提交中恢复索引中的文件。

- [git-reset[1]](../1/git-reset) is about updating your branch, moving the tip in order to add or remove commits from the branch. This operation changes the commit history.

- [git-reset[1]](../1/git-reset) 用于更新您的分支，移动分支头以添加或删除分支上的提交。此操作会更改提交历史记录。

  `git reset` can also be used to restore the index, overlapping with `git restore`.
  
  `git reset` 也可以用于恢复索引，与 `git restore` 重叠。

## 低级命令（plumbing）

Although Git includes its own porcelain layer, its low-level commands are sufficient to support development of alternative porcelains. Developers of such porcelains might start by reading about [git-update-index[1]](../1/git-update-index) and [git-read-tree[1]](../1/git-read-tree).

​	虽然Git包含了自己的Porcelain层，但其低级命令足以支持开发替代Porcelain的工具。开发这些工具的开发人员可能会首先阅读有关 [git-update-index[1]](../1/git-update-index) 和 [git-read-tree[1]](../1/git-read-tree)的信息。

The interface (input, output, set of options and the semantics) to these low-level commands are meant to be a lot more stable than Porcelain level commands, because these commands are primarily for scripted use. The interface to Porcelain commands on the other hand are subject to change in order to improve the end user experience.

​	这些低级命令的接口（输入、输出、选项集和语义）更稳定，比Porcelain级别的命令更稳定，因为这些命令主要用于脚本使用。另一方面，Porcelain命令的接口可能会改变，以改进最终用户体验。

The following description divides the low-level commands into commands that manipulate objects (in the repository, index, and working tree), commands that interrogate and compare objects, and commands that move objects and references between repositories.

​	以下描述将低级命令分为操纵对象（在仓库、索引和工作树中）、询问和比较对象的命令，以及在仓库之间移动对象和引用的命令。

### 操作命令

- [git-apply[1]](../1/git-apply)

  Apply a patch to files and/or to the index

  将补丁应用于文件和/或索引

- [git-checkout-index[1]](../1/git-checkout-index)

  Copy files from the index to the working tree

  从索引复制文件到工作树

- [git-commit-graph[1]](../1/git-commit-graph)

  Write and verify Git commit-graph files

  编写并验证Git提交图文件

- [git-commit-tree[1]](../1/git-commit-tree)

  Create a new commit object

  创建一个新的提交对象

- [git-hash-object[1]](../1/git-hash-object)

  Compute object ID and optionally creates a blob from a file

  计算对象ID并可选择从文件创建blob

- [git-index-pack[1]](../1/git-index-pack)

  Build pack index file for an existing packed archive

  为现有的打包存档构建打包索引文件

- [git-merge-file[1]](../1/git-merge-file)

  Run a three-way file merge

  运行三方文件合并

- [git-merge-index[1]](../1/git-merge-index)

  Run a merge for files needing merging

  对需要合并的文件运行合并

- [git-mktag[1]](../1/git-mktag)

  Creates a tag object with extra validation

  创建带有额外验证的标签对象

- [git-mktree[1]](../1/git-mktree)

  Build a tree-object from ls-tree formatted text

  从ls-tree格式的文本构建树对象

- [git-multi-pack-index[1]](../1/git-multi-pack-index)

  Write and verify multi-pack-indexes

  编写并验证多pack索引

- [git-pack-objects[1]](../1/git-pack-objects)

  Create a packed archive of objects

  创建打包对象的打包存档

- [git-prune-packed[1]](../1/git-prune-packed)

  Remove extra objects that are already in pack files

  删除已包含在pack文件中的额外对象

- [git-read-tree[1]](../1/git-read-tree)

  Reads tree information into the index

  将树信息读入索引

- [git-symbolic-ref[1]](../1/git-symbolic-ref)

  Read, modify and delete symbolic refs

  读取、修改和删除符号引用

- [git-unpack-objects[1]](../1/git-unpack-objects)

  Unpack objects from a packed archive

  从打包存档中解压对象

- [git-update-index[1]](../1/git-update-index)

  Register file contents in the working tree to the index

  将工作树中的文件内容注册到索引

- [git-update-ref[1]](../1/git-update-ref)

  Update the object name stored in a ref safely

  安全地更新存储在引用中的对象名称

- [git-write-tree[1]](../1/git-write-tree)

  Create a tree object from the current index
  
  从当前索引创建树对象

### 询问命令 Interrogation commands

- [git-cat-file[1]](../1/git-cat-file)

  Provide content or type and size information for repository objects

  为仓库对象提供内容、类型和大小信息

- [git-cherry[1]](../1/git-cherry)

  Find commits yet to be applied to upstream

  查找尚未应用于上游的提交

- [git-diff-files[1]](../1/git-diff-files)

  Compares files in the working tree and the index

  比较工作树和索引中的文件

- [git-diff-index[1]](../1/git-diff-index)

  Compare a tree to the working tree or index

  将树与工作树或索引进行比较

- [git-diff-tree[1]](../1/git-diff-tree)

  Compares the content and mode of blobs found via two tree objects

  比较通过两个树对象找到的blob的内容和模式

- [git-for-each-ref[1]](../1/git-for-each-ref)

  Output information on each ref

  输出每个引用的信息

- [git-for-each-repo[1]](../1/git-for-each-repo)

  Run a Git command on a list of repositories

  在仓库列表上运行Git命令

- [git-get-tar-commit-id[1]](../1/git-get-tar-commit-id)

  Extract commit ID from an archive created using git-archive

  从使用git-archive创建的存档中提取提交ID

- [git-ls-files[1]](../1/git-ls-files)

  Show information about files in the index and the working tree

  显示索引和工作树中文件的信息

- [git-ls-remote[1]](../1/git-ls-remote)

  List references in a remote repository

  列出远程仓库中的引用

- [git-ls-tree[1]](../1/git-ls-tree)

  List the contents of a tree object

  列出树对象的内容

- [git-merge-base[1]](../1/git-merge-base)

  Find as good common ancestors as possible for a merge

  查找合并的尽可能好的共同祖先

- [git-name-rev[1]](../1/git-name-rev)

  Find symbolic names for given revs

  为给定的修订版本查找符号名称

- [git-pack-redundant[1]](../1/git-pack-redundant)

  Find redundant pack files

  查找冗余的pack文件

- [git-rev-list[1]](../1/git-rev-list)

  Lists commit objects in reverse chronological order

  按时间顺序列出提交对象

- [git-rev-parse[1]](../1/git-rev-parse)

  Pick out and massage parameters

  选择并调整参数

- [git-show-index[1]](../1/git-show-index)

  Show packed archive index

  显示打包的存档索引

- [git-show-ref[1]](../1/git-show-ref)

  List references in a local repository

  列出本地仓库中的引用

- [git-unpack-file[1]](../1/git-unpack-file)

  Creates a temporary file with a blob’s contents

  创建一个带有blob内容的临时文件

- [git-var[1]](../1/git-var)

  Show a Git logical variable

  显示Git逻辑变量

- [git-verify-pack[1]](../1/git-verify-pack)

  Validate packed Git archive files
  
  验证打包的Git存档文件

In general, the interrogate commands do not touch the files in the working tree.

​	总的来说，询问命令不会修改工作树中的文件。

### 同步仓库

- [git-daemon[1]](../1/git-daemon)

  A really simple server for Git repositories

  一个非常简单的用于Git仓库的服务器

- [git-fetch-pack[1]](../1/git-fetch-pack)

  Receive missing objects from another repository

  从另一个仓库接收丢失的对象

- [git-http-backend[1]](../1/git-http-backend)

  Server side implementation of Git over HTTP

  Git在HTTP上的服务器端实现

- [git-send-pack[1]](../1/git-send-pack)

  Push objects over Git protocol to another repository

  通过Git协议将对象推送到另一个仓库

- [git-update-server-info[1]](../1/git-update-server-info)

  Update auxiliary info file to help dumb servers
  
  更新辅助信息文件以帮助非智能服务器

The following are helper commands used by the above; end users typically do not use them directly.

​	以下是由上述命令使用的辅助命令；最终用户通常不直接使用它们。

- [git-http-fetch[1]](../1/git-http-fetch)

  Download from a remote Git repository via HTTP

  通过HTTP从远程Git仓库下载

- [git-http-push[1]](../1/git-http-push)

  Push objects over HTTP/DAV to another repository

  通过HTTP/DAV将对象推送到另一个仓库

- [git-receive-pack[1]](../1/git-receive-pack)

  Receive what is pushed into the repository

  接收被推送到仓库中的内容

- [git-shell[1]](../1/git-shell)

  Restricted login shell for Git-only SSH access

  用于Git-only SSH访问的受限登录Shell

- [git-upload-archive[1]](../1/git-upload-archive)

  Send archive back to git-archive

  将存档发送回git-archive

- [git-upload-pack[1]](../1/git-upload-pack)

  Send objects packed back to git-fetch-pack
  
  将打包对象发送回git-fetch-pack

### 内部辅助命令

These are internal helper commands used by other commands; end users typically do not use them directly.

​	这些是由其他命令使用的内部辅助命令；最终用户通常不直接使用它们。

- [git-check-attr[1]](../1/git-check-attr)

  Display gitattributes information

  显示gitattributes信息

- [git-check-ignore[1]](../1/git-check-ignore)

  Debug gitignore / exclude files

  调试gitignore / 排除文件

- [git-check-mailmap[1]](../1/git-check-mailmap)

  Show canonical names and email addresses of contacts

  显示联系人的规范名称和电子邮件地址

- [git-check-ref-format[1]](../1/git-check-ref-format)

  Ensures that a reference name is well formed

  确保引用名称格式正确

- [git-column[1]](../1/git-column)

  Display data in columns

  以列形式显示数据

- [git-credential[1]](../1/git-credential)

  Retrieve and store user credentials

  检索和存储用户凭据

- [git-credential-cache[1]](../1/git-credential-cache)

  Helper to temporarily store passwords in memory

  用于暂时在内存中存储密码的辅助程序

- [git-credential-store[1]](../1/git-credential-store)

  Helper to store credentials on disk

  用于在磁盘上存储凭据的辅助程序

- [git-fmt-merge-msg[1]](../1/git-fmt-merge-msg)

  Produce a merge commit message

  生成合并提交消息

- [git-hook[1]](../1/git-hook)

  Run git hooks

  运行git钩子

- [git-interpret-trailers[1]](../1/git-interpret-trailers)

  Add or parse structured information in commit messages

  在提交消息中添加或解析结构化信息

- [git-mailinfo[1]](../1/git-mailinfo)

  Extracts patch and authorship from a single e-mail message

  从单个电子邮件消息中提取补丁和作者信息

- [git-mailsplit[1]](../1/git-mailsplit)

  Simple UNIX mbox splitter program

  简单的UNIX mbox拆分程序

- [git-merge-one-file[1]](../1/git-merge-one-file)

  The standard helper program to use with git-merge-index

  与git-merge-index一起使用的标准辅助程序

- [git-patch-id[1]](../1/git-patch-id)

  Compute unique ID for a patch

  为补丁计算唯一ID

- [git-sh-i18n[1]](../1/git-sh-i18n)

  Git’s i18n setup code for shell scripts

  Git的用于shell脚本的i18n设置代码

- [git-sh-setup[1]](../1/git-sh-setup)

  Common Git shell script setup code

  通用Git shell脚本设置代码

- [git-stripspace[1]](../1/git-stripspace)

  Remove unnecessary whitespace
  
  移除不必要的空格

## 指南

The following documentation pages are guides about Git concepts.

​	以下文档页面是关于Git概念的指南。

- [gitcore-tutorial[7]](../7/gitcore-tutorial)

  A Git core tutorial for developers

  面向开发人员的Git核心教程

- [gitcredentials[7]](../7/gitcredentials)

  Providing usernames and passwords to Git

  向Git提供用户名和密码

- [gitcvs-migration[7]](../7/gitcvs-migration)

  Git for CVS users

  针对CVS用户的Git指南

- [gitdiffcore[7]](../7/gitdiffcore)

  Tweaking diff output

  调整diff输出

- [giteveryday[7]](../7/giteveryday)

  A useful minimum set of commands for Everyday Git

  Git的有用最小命令集

- [gitfaq[7]](../7/gitfaq)

  Frequently asked questions about using Git

  使用Git时的常见问题

- [gitglossary[7]](../7/gitglossary)

  A Git Glossary

  Git词汇表

- [gitnamespaces[7]](../7/gitnamespaces)

  Git namespaces

  Git命名空间

- [gitremote-helpers[7]](../7/gitremote-helpers)

  Helper programs to interact with remote repositories

  用于与远程仓库交互的辅助程序

- [gitsubmodules[7]](../7/gitsubmodules)

  Mounting one repository inside another

  将一个仓库挂载到另一个仓库内部

- [gittutorial[7]](../7/gittutorial)

  A tutorial introduction to Git

  Git的教程入门

- [gittutorial-2[7]](../7/gittutorial-2)

  A tutorial introduction to Git: part two

  Git的教程入门：第二部分

- [gitworkflows[7]](../7/gitworkflows)

  An overview of recommended workflows with Git
  
  推荐的Git工作流程概述

## 仓库、命令和文件接口

This documentation discusses repository and command interfaces which users are expected to interact with directly. See `--user-formats` in [git-help[1]](../1/git-help) for more details on the criteria.

​	本文档讨论用户预期直接与之交互的仓库和命令接口。有关标准的详细信息，请参阅 [git-help[1]](../1/git-help) 中的 `--user-formats`。

- [gitattributes[5]](../5/gitattributes)

  Defining attributes per path

  定义每个路径的属性

- [gitcli[7]](../7/gitcli)

  Git command-line interface and conventions

  Git命令行界面和约定

- [githooks[5]](../5/githooks)

  Hooks used by Git

  Git使用的钩子

- [gitignore[5]](../5/gitignore)

  Specifies intentionally untracked files to ignore

  指定要忽略的意图未跟踪的文件

- [gitmailmap[5]](../5/gitmailmap)

  Map author/committer names and/or E-Mail addresses

  映射作者/提交者的名称和/或电子邮件地址

- [gitmodules[5]](../5/gitmodules)

  Defining submodule properties

  定义子模块属性

- [gitrepository-layout[5]](../5/gitrepository-layout)

  Git Repository Layout

  Git仓库布局

- [gitrevisions[7]](../7/gitrevisions)

  Specifying revisions and ranges for Git
  
  指定Git的修订和范围

## 文件格式、协议和其他开发者接口

This documentation discusses file formats, over-the-wire protocols and other git developer interfaces. See `--developer-interfaces` in [git-help[1]](../1/git-help).

​	本文档讨论文件格式、通过网络传输的协议和其他git开发者接口。有关标准的详细信息，请参阅 [git-help[1]](../1/git-help) 中的 `--developer-interfaces`。

- [gitformat-bundle[5]](../5/gitformat-bundle)

  The bundle file format

  Bundle文件格式

- [gitformat-chunk[5]](../5/gitformat-chunk)

  Chunk-based file formats

  基于块的文件格式

- [gitformat-commit-graph[5]](../5/gitformat-commit-graph)

  Git commit-graph format

  Git提交图格式

- [gitformat-index[5]](../5/gitformat-index)

  Git index format

  Git索引格式

- [gitformat-pack[5]](../5/gitformat-pack)

  Git pack format

  Git包格式

- [gitformat-signature[5]](../5/gitformat-signature)

  Git cryptographic signature formats

  Git加密签名格式

- [gitprotocol-capabilities[5]](../5/gitprotocol-capabilities)

  Protocol v0 and v1 capabilities

  协议 v0 和 v1 的能力

- [gitprotocol-common[5]](../5/gitprotocol-common)

  Things common to various protocols

  各种协议的共同点

- [gitprotocol-http[5]](../5/gitprotocol-http)

  Git HTTP-based protocols

  基于Git的HTTP协议

- [gitprotocol-pack[5]](../5/gitprotocol-pack)

  How packs are transferred over-the-wire

  包在网络上传输的方式

- [gitprotocol-v2[5]](../5/gitprotocol-v2)

  Git Wire Protocol, Version 2
  
  Git 传输协议，版本 2

## 配置机制

Git uses a simple text format to store customizations that are per repository and are per user. Such a configuration file may look like this:

​	Git使用简单的文本格式来存储每个仓库和每个用户的自定义设置。这样的配置文件可能是这样的：

```
#
# A '#' or ';' character indicates a comment.
#

; core variables
[core]
	; Don't trust file modes
	filemode = false

; user identity
[user]
	name = "Junio C Hamano"
	email = "gitster@pobox.com"
```

Various commands read from the configuration file and adjust their operation accordingly. See [git-config[1]](../1/git-config) for a list and more details about the configuration mechanism.

​	各种命令从配置文件中读取并相应地调整其操作。有关配置机制的列表和更多详细信息，请参阅 [git-config[1]](../1/git-config)。

## 标识符术语

- `<object>`

  Indicates the object name for any type of object.

  表示任何类型的对象的对象名称。

- `<blob>`

  Indicates a blob object name.

  表示一个 blob 对象的对象名称。

- `<tree>`

  Indicates a tree object name.

  表示一个 tree 对象的对象名称。

- `<commit>`

  Indicates a commit object name.

  表示一个commit 对象的对象名称。

- `<tree-ish>`

  Indicates a tree, commit or tag object name. A command that takes a <tree-ish> argument ultimately wants to operate on a <tree> object but automatically dereferences <commit> and <tag> objects that point at a <tree>.

  表示一个tree、commit 或 tag 对象的对象名称。接受 <tree-ish> 参数的命令最终会在一个 `<tree>` 对象上操作，但会自动解引用指向 `<tree>` 的 `<commit>` 和 `<tag>` 对象。

- `<commit-ish>`

  Indicates a commit or tag object name. A command that takes a <commit-ish> argument ultimately wants to operate on a <commit> object but automatically dereferences <tag> objects that point at a <commit>.

  表示一个提交或标签对象的对象名称。接受 `<commit-ish>` 参数的命令最终会在一个 `<commit>` 对象上操作，但会自动解引用指向 `<commit>` 的 `<tag>` 对象。

- `<type>`

  Indicates that an object type is required. Currently one of: `blob`, `tree`, `commit`, or `tag`.

  表示需要一个对象类型。目前可用的类型有：`blob`、`tree`、`commit` 或 `tag`。

- `<file>`

  Indicates a filename - almost always relative to the root of the tree structure `GIT_INDEX_FILE` describes.
  
  表示一个文件名 —— 几乎总是相对于树结构的根目录，即 `GIT_INDEX_FILE` 描述的位置。

## 符号标识符

Any Git command accepting any <object> can also use the following symbolic notation:

​	任何接受任意 `<object>` 的 Git 命令也可以使用以下符号表示法：

- HEAD

  indicates the head of the current branch.

  表示当前分支的头。

- `<tag>`

  a valid tag *name* (i.e. a `refs/tags/<tag>` reference).

  一个有效的 tag *名称*（即一个 `refs/tags/<tag>` 引用）。

- `<head>`

  a valid head *name* (i.e. a `refs/heads/<head>` reference).
  
  一个有效的 head*名称*（即一个 `refs/heads/<head>` 引用）。

For a more complete list of ways to spell object names, see "SPECIFYING REVISIONS" section in [gitrevisions[7]](../7/gitrevisions).

​	有关更多对象名称拼写方式的完整列表，请参阅 [gitrevisions[7]](../7/gitrevisions) 中的 "SPECIFYING REVISIONS" 部分。

## 文件/目录结构

Please see the [gitrepository-layout[5]](../5/gitrepository-layout) document.

​	请参阅 [gitrepository-layout[5]](../5/gitrepository-layout) 文档。

Read [githooks[5]](../5/githooks) for more details about each hook.

​	有关每个钩子的更多详细信息，请阅读 [githooks[5]](../5/githooks)。

Higher level SCMs may provide and manage additional information in the `$GIT_DIR`.

​	更高级的版本控制系统（SCM）可能在 `$GIT_DIR` 中提供和管理其他信息。

## 术语

Please see [gitglossary[7]](../7/gitglossary).

​	请参阅 [gitglossary[7]](../7/gitglossary)。

## 环境变量

Various Git commands pay attention to environment variables and change their behavior. The environment variables marked as "Boolean" take their values the same way as Boolean valued configuration variables, e.g. "true", "yes", "on" and positive numbers are taken as "yes".

​	各种 Git 命令会关注环境变量并改变它们的行为。被标记为 "Boolean" 的环境变量的值与布尔值配置变量相同，例如 "true"、"yes"、"on" 和正数都被视为 "yes"。

Here are the variables:

​	以下是这些变量：

### Git 仓库

These environment variables apply to *all* core Git commands. Nb: it is worth noting that they may be used/overridden by SCMS sitting above Git so take care if using a foreign front-end.

​	这些环境变量适用于*所有*核心 Git 命令。值得注意的是，它们可能会被位于 Git 之上的版本控制系统使用/覆盖，因此在使用外部前端时要小心。

- `GIT_INDEX_FILE`

  This environment variable specifies an alternate index file. If not specified, the default of `$GIT_DIR/index` is used.

  此环境变量指定备用的索引文件。如果未指定，则使用默认的 `$GIT_DIR/index`。

- `GIT_INDEX_VERSION`

  This environment variable specifies what index version is used when writing the index file out. It won’t affect existing index files. By default index file version 2 or 3 is used. See [git-update-index[1]](../1/git-update-index) for more information.

  此环境变量指定写出索引文件时使用的索引版本。它不会影响现有的索引文件。默认情况下，使用索引文件版本2或3。有关更多信息，请参阅 [git-update-index[1]](../1/git-update-index)。

- `GIT_OBJECT_DIRECTORY`

  If the object storage directory is specified via this environment variable then the sha1 directories are created underneath - otherwise the default `$GIT_DIR/objects` directory is used.

  如果通过此环境变量指定了对象存储目录，则 sha1 目录将创建在其中；否则使用默认的 `$GIT_DIR/objects` 目录。

- `GIT_ALTERNATE_OBJECT_DIRECTORIES`

  Due to the immutable nature of Git objects, old objects can be archived into shared, read-only directories. This variable specifies a ":" separated (on Windows ";" separated) list of Git object directories which can be used to search for Git objects. New objects will not be written to these directories.Entries that begin with `"` (double-quote) will be interpreted as C-style quoted paths, removing leading and trailing double-quotes and respecting backslash escapes. E.g., the value `"path-with-\"-and-:-in-it":vanilla-path` has two paths: `path-with-"-and-:-in-it` and `vanilla-path`.

  由于 Git 对象的不可变性，旧对象可以存档到共享的只读目录中。该变量指定由冒号分隔（在 Windows 上为分号分隔）的 Git 对象目录列表，用于搜索 Git 对象。新对象将不会写入这些目录。以 `"`（双引号）开头的条目将被解释为 C 样式引用的路径，删除前导和尾随的双引号并保留反斜杠转义。例如，值 `"path-with-\"-and-:-in-it":vanilla-path` 包含两个路径：`path-with-"-and-:-in-it` 和 `vanilla-path`。

- `GIT_DIR`

  If the `GIT_DIR` environment variable is set then it specifies a path to use instead of the default `.git` for the base of the repository. The `--git-dir` command-line option also sets this value.

  如果设置了 `GIT_DIR` 环境变量，则它指定用于仓库基础的路径，而不是默认的 `.git`。`--git-dir` 命令行选项也会设置这个值。

- `GIT_WORK_TREE`

  Set the path to the root of the working tree. This can also be controlled by the `--work-tree` command-line option and the core.worktree configuration variable.

  设置工作树的根目录路径。这也可以通过 `--work-tree` 命令行选项和 core.worktree 配置变量来控制。

- `GIT_NAMESPACE`

  Set the Git namespace; see [gitnamespaces[7]](../7/gitnamespaces) for details. The `--namespace` command-line option also sets this value.

  设置 Git 命名空间；有关详细信息，请参阅 [gitnamespaces[7]](../7/gitnamespaces)。`--namespace` 命令行选项也会设置这个值。

- `GIT_CEILING_DIRECTORIES`

  This should be a colon-separated list of absolute paths. If set, it is a list of directories that Git should not chdir up into while looking for a repository directory (useful for excluding slow-loading network directories). It will not exclude the current working directory or a GIT_DIR set on the command line or in the environment. Normally, Git has to read the entries in this list and resolve any symlink that might be present in order to compare them with the current directory. However, if even this access is slow, you can add an empty entry to the list to tell Git that the subsequent entries are not symlinks and needn’t be resolved; e.g., `GIT_CEILING_DIRECTORIES=/maybe/symlink::/very/slow/non/symlink`.

  这应该是一个由冒号分隔的绝对路径列表。如果设置，这是一个 Git 在查找仓库目录时不应该进入的目录列表（对于排除加载缓慢的网络目录很有用）。它不会排除当前工作目录或在命令行或环境中设置的 GIT_DIR。通常，Git 必须读取此列表中的条目并解析其中可能存在的符号链接，以将它们与当前目录进行比较。但是，如果即使这个访问也很慢，您可以在列表中添加一个空条目，告诉 Git 后续条目不是符号链接，不需要解析；例如，`GIT_CEILING_DIRECTORIES=/maybe/symlink::/very/slow/non/symlink`。

- `GIT_DISCOVERY_ACROSS_FILESYSTEM`

  When run in a directory that does not have ".git" repository directory, Git tries to find such a directory in the parent directories to find the top of the working tree, but by default it does not cross filesystem boundaries. This Boolean environment variable can be set to true to tell Git not to stop at filesystem boundaries. Like `GIT_CEILING_DIRECTORIES`, this will not affect an explicit repository directory set via `GIT_DIR` or on the command line.

  当在一个没有 ".git" 仓库目录的目录中运行时，Git尝试在父目录中找到这样的目录以找到工作树的顶部，但默认情况下它不会跨越文件系统边界。这个布尔型环境变量可以设置为true，告诉Git不要在文件系统边界停止。与 `GIT_CEILING_DIRECTORIES` 一样，这不会影响通过 `GIT_DIR` 或命令行设置的显式仓库目录。

- `GIT_COMMON_DIR`

  If this variable is set to a path, non-worktree files that are normally in `$GIT_DIR` will be taken from this path instead. Worktree-specific files such as HEAD or index are taken from $GIT_DIR. See [gitrepository-layout[5]](../5/gitrepository-layout) and [git-worktree[1]](../1/git-worktree) for details. This variable has lower precedence than other path variables such as GIT_INDEX_FILE, GIT_OBJECT_DIRECTORY…

  如果将此变量设置为路径，则通常在 `$GIT_DIR` 中的非工作树文件将从此路径中获取。从 $GIT_DIR 获取工作树特定的文件，例如 HEAD 或 index。有关详细信息，请参阅 [gitrepository-layout[5]](../5/gitrepository-layout) 和 [git-worktree[1]](../1/git-worktree)。此变量的优先级低于其他路径变量，例如 GIT_INDEX_FILE、GIT_OBJECT_DIRECTORY…

- `GIT_DEFAULT_HASH`

  If this variable is set, the default hash algorithm for new repositories will be set to this value. This value is ignored when cloning and the setting of the remote repository is always used. The default is "sha1". THIS VARIABLE IS EXPERIMENTAL! See `--object-format` in [git-init[1]](../1/git-init).
  
  如果设置了此变量，新仓库的默认哈希算法将设置为此值。当克隆时，此值将被忽略，并始终使用远程仓库的设置。默认值为 "sha1"。此变量是实验性的！请参阅 [git-init[1]](../1/git-init) 中的 `--object-format`。

### Git 提交

- `GIT_AUTHOR_NAME`

  The human-readable name used in the author identity when creating commit or tag objects, or when writing reflogs. Overrides the `user.name` and `author.name` configuration settings.

  创建提交或标签对象，或写入reflog时使用的作者身份的可读名称。覆盖 `user.name` 和 `author.name` 配置设置。

- `GIT_AUTHOR_EMAIL`

  The email address used in the author identity when creating commit or tag objects, or when writing reflogs. Overrides the `user.email` and `author.email` configuration settings.

  创建提交或标签对象，或写入reflog时使用的作者身份的电子邮件地址。覆盖 `user.email` 和 `author.email` 配置设置。

- `GIT_AUTHOR_DATE`

  The date used for the author identity when creating commit or tag objects, or when writing reflogs. See [git-commit[1]](../1/git-commit) for valid formats.

  创建提交或标签对象，或写入reflog时使用的作者身份的日期。有关有效格式，请参阅 [git-commit[1]](../1/git-commit)。

- `GIT_COMMITTER_NAME`

  The human-readable name used in the committer identity when creating commit or tag objects, or when writing reflogs. Overrides the `user.name` and `committer.name` configuration settings.

  创建提交或标签对象，或写入reflog时使用的提交者身份的可读名称。覆盖 `user.name` 和 `committer.name` 配置设置。

- `GIT_COMMITTER_EMAIL`

  The email address used in the author identity when creating commit or tag objects, or when writing reflogs. Overrides the `user.email` and `committer.email` configuration settings.

  创建提交或标签对象，或写入reflog时使用的提交者身份的电子邮件地址。覆盖 `user.email` 和 `committer.email` 配置设置。

- `GIT_COMMITTER_DATE`

  The date used for the committer identity when creating commit or tag objects, or when writing reflogs. See [git-commit[1]](../1/git-commit) for valid formats.

  创建提交或标签对象，或写入reflog时使用的提交者身份的日期。有关有效格式，请参阅 [git-commit[1]](../1/git-commit)。

- `EMAIL`

  The email address used in the author and committer identities if no other relevant environment variable or configuration setting has been set.
  
  如果没有设置其他相关的环境变量或配置设置，则在作者和提交者身份中使用的电子邮件地址。

### Git Diffs

- `GIT_DIFF_OPTS`

  Only valid setting is "--unified=??" or "-u??" to set the number of context lines shown when a unified diff is created. This takes precedence over any "-U" or "--unified" option value passed on the Git diff command line.

  唯一有效的设置是 "--unified=??" 或 "-u??"，用于设置创建统一差异时显示的上下文行数。这优先于在Git差异命令行上传递的任何 "-U" 或 "--unified" 选项值。

- `GIT_EXTERNAL_DIFF`

  When the environment variable `GIT_EXTERNAL_DIFF` is set, the program named by it is called to generate diffs, and Git does not use its builtin diff machinery. For a path that is added, removed, or modified, `GIT_EXTERNAL_DIFF` is called with 7 parameters:`path old-file old-hex old-mode new-file new-hex new-mode`where:

  当环境变量 `GIT_EXTERNAL_DIFF` 被设置时，通过它命名的程序将被调用来生成差异，而Git不使用其内置的差异机制。对于添加、删除或修改的路径，将以7个参数调用 `GIT_EXTERNAL_DIFF`：`path old-file old-hex old-mode new-file new-hex new-mode`，其中：

  - `<old|new>`-file: are files GIT_EXTERNAL_DIFF can use to read the contents of <old|new>,

  - `<old|new>`-file：是 `GIT_EXTERNAL_DIFF` 可用于读取 `<old|new>` 的文件

  - `<old|new>`-hex: are the 40-hexdigit SHA-1 hashes, 是40位十六进制SHA-1哈希值，

  - `<old|new>`-mode: are the octal representation of the file modes.是文件模式的八进制表示。

    The file parameters can point at the user’s working file (e.g. `new-file` in "git-diff-files"), `/dev/null` (e.g. `old-file` when a new file is added), or a temporary file (e.g. `old-file` in the index). `GIT_EXTERNAL_DIFF` should not worry about unlinking the temporary file — it is removed when `GIT_EXTERNAL_DIFF` exits.

    文件参数可以指向用户的工作文件（例如 "git-diff-files" 中的 `new-file`）、`/dev/null`（例如添加新文件时的 `old-file`）或临时文件（例如索引中的 `old-file`）。`GIT_EXTERNAL_DIFF` 不应担心删除临时文件 - 它在 `GIT_EXTERNAL_DIFF` 退出时被删除。

    For a path that is unmerged, `GIT_EXTERNAL_DIFF` is called with 1 parameter, <path>.

    对于未合并的路径，`GIT_EXTERNAL_DIFF` 以1个参数 `<path>` 被调用。

    For each path `GIT_EXTERNAL_DIFF` is called, two environment variables, `GIT_DIFF_PATH_COUNTER` and `GIT_DIFF_PATH_TOTAL` are set.

    对于每个调用 `GIT_EXTERNAL_DIFF` 的路径，会设置两个环境变量 `GIT_DIFF_PATH_COUNTER` 和 `GIT_DIFF_PATH_TOTAL`。

- `GIT_DIFF_PATH_COUNTER`

  A 1-based counter incremented by one for every path.

  一个从1开始的计数器，对每个路径递增一次。

- `GIT_DIFF_PATH_TOTAL`

  The total number of paths.
  
  路径的总数。

### 其他

- `GIT_MERGE_VERBOSITY`

  A number controlling the amount of output shown by the recursive merge strategy. Overrides merge.verbosity. See [git-merge[1]](../1/git-merge)

  一个控制递归合并策略输出量的数字。覆盖了 merge.verbosity。参见 [git-merge[1]](../1/git-merge)。

- `GIT_PAGER`

  This environment variable overrides `$PAGER`. If it is set to an empty string or to the value "cat", Git will not launch a pager. See also the `core.pager` option in [git-config[1]](../1/git-config).

  此环境变量覆盖了 `$PAGER`。如果设置为空字符串或值为 "cat"，Git 将不会启动分页器。请参阅 [git-config[1]](../1/git-config) 中的 `core.pager` 选项。

- `GIT_PROGRESS_DELAY`

  A number controlling how many seconds to delay before showing optional progress indicators. Defaults to 2.

  控制在显示可选的进度指示器之前延迟多少秒。默认为2秒。

- `GIT_EDITOR`

  This environment variable overrides `$EDITOR` and `$VISUAL`. It is used by several Git commands when, on interactive mode, an editor is to be launched. See also [git-var[1]](../1/git-var) and the `core.editor` option in [git-config[1]](../1/git-config).

  此环境变量覆盖了 `$EDITOR` 和 `$VISUAL`。在交互模式下启动编辑器时，多个 Git 命令将使用它。请参阅 [git-var[1]](../1/git-var) 和 [git-config[1]](../1/git-config) 中的 `core.editor` 选项。

- `GIT_SEQUENCE_EDITOR`

  This environment variable overrides the configured Git editor when editing the todo list of an interactive rebase. See also [git-rebase[1]](../1/git-rebase) and the `sequence.editor` option in [git-config[1]](../1/git-config).

  此环境变量覆盖了配置的 Git 编辑器，用于编辑交互式 rebase 的待办列表。请参阅 [git-rebase[1]](../1/git-rebase) 和 [git-config[1]](../1/git-config) 中的 `sequence.editor` 选项。

- `GIT_SSH`

- `GIT_SSH_COMMAND`

  If either of these environment variables is set then *git fetch* and *git push* will use the specified command instead of *ssh* when they need to connect to a remote system. The command-line parameters passed to the configured command are determined by the ssh variant. See `ssh.variant` option in [git-config[1]](../1/git-config) for details.`$GIT_SSH_COMMAND` takes precedence over `$GIT_SSH`, and is interpreted by the shell, which allows additional arguments to be included. `$GIT_SSH` on the other hand must be just the path to a program (which can be a wrapper shell script, if additional arguments are needed).Usually it is easier to configure any desired options through your personal `.ssh/config` file. Please consult your ssh documentation for further details.

  如果设置了这两个环境变量中的任意一个，则 *git fetch* 和 *git push* 在需要连接到远程系统时将使用指定的命令代替 *ssh*。由 ssh 变种确定配置的命令的命令行参数。有关详情，请参阅 [git-config[1]](../1/git-config) 中的 `ssh.variant` 选项。`$GIT_SSH_COMMAND` 优先于 `$GIT_SSH`，由 shell 解释，允许包含额外的参数。另一方面，`$GIT_SSH` 必须是程序的路径（如果需要额外的参数，可以是包装器 shell 脚本）。通常，更容易通过个人的 `.ssh/config` 文件配置所需的任何选项。请参阅您的 ssh 文档以获取更多详情。

- `GIT_SSH_VARIANT`

  If this environment variable is set, it overrides Git’s autodetection whether `GIT_SSH`/`GIT_SSH_COMMAND`/`core.sshCommand` refer to OpenSSH, plink or tortoiseplink. This variable overrides the config setting `ssh.variant` that serves the same purpose.

  如果设置了此环境变量，它将覆盖 Git 的自动检测，以确定 `GIT_SSH`/`GIT_SSH_COMMAND`/`core.sshCommand` 是否指向 OpenSSH、plink 或 tortoiseplink。此变量覆盖了 `ssh.variant` 配置设置，具有相同的目的。

- `GIT_SSL_NO_VERIFY`

  Setting and exporting this environment variable to any value tells Git not to verify the SSL certificate when fetching or pushing over HTTPS.

  将此环境变量设置为任何值告诉 Git 在通过 HTTPS 拉取或推送时不要验证 SSL 证书。

- `GIT_ATTR_SOURCE`

  Sets the treeish that gitattributes will be read from.

  设置用于读取 gitattributes 的 treeish。

- `GIT_ASKPASS`

  If this environment variable is set, then Git commands which need to acquire passwords or passphrases (e.g. for HTTP or IMAP authentication) will call this program with a suitable prompt as command-line argument and read the password from its STDOUT. See also the `core.askPass` option in [git-config[1]](../1/git-config).

  如果设置了此环境变量，则需要获取密码或密码短语的 Git 命令（例如，用于 HTTP 或 IMAP 身份验证）将调用此程序，并使用适当的提示作为命令行参数，并从其 STDOUT 读取密码。请参阅 [git-config[1]](../1/git-config) 中的 `core.askPass` 选项。

- `GIT_TERMINAL_PROMPT`

  If this Boolean environment variable is set to false, git will not prompt on the terminal (e.g., when asking for HTTP authentication).

  如果将此布尔型环境变量设置为 false，Git 将不会在终端上提示（例如，在请求 HTTP 身份验证时）。

- `GIT_CONFIG_GLOBAL`

- `GIT_CONFIG_SYSTEM`

  Take the configuration from the given files instead from global or system-level configuration files. If `GIT_CONFIG_SYSTEM` is set, the system config file defined at build time (usually `/etc/gitconfig`) will not be read. Likewise, if `GIT_CONFIG_GLOBAL` is set, neither `$HOME/.gitconfig` nor `$XDG_CONFIG_HOME/git/config` will be read. Can be set to `/dev/null` to skip reading configuration files of the respective level.

  从给定文件中获取配置，而不是全局或系统级配置文件。如果设置了 `GIT_CONFIG_SYSTEM`，则不会读取在构建时定义的系统配置文件（通常为 `/etc/gitconfig`）。同样，如果设置了 `GIT_CONFIG_GLOBAL`，则不会读取 `$HOME/.gitconfig` 或 `$XDG_CONFIG_HOME/git/config`。可以将其设置为 `/dev/null`，以跳过读取相应级别的配置文件。

- `GIT_CONFIG_NOSYSTEM`

  Whether to skip reading settings from the system-wide `$(prefix)/etc/gitconfig` file. This Boolean environment variable can be used along with `$HOME` and `$XDG_CONFIG_HOME` to create a predictable environment for a picky script, or you can set it to true to temporarily avoid using a buggy `/etc/gitconfig` file while waiting for someone with sufficient permissions to fix it.

  是否跳过读取系统级别的 `$(prefix)/etc/gitconfig` 文件。此布尔型环境变量可以与 `$HOME` 和 `$XDG_CONFIG_HOME` 一起使用，以为挑剔的脚本创建可预测的环境，或者可以将其设置为 true，以暂时避免使用有问题的 `/etc/gitconfig` 文件，同时等待有足够权限的用户修复它。

- `GIT_FLUSH`

  If this environment variable is set to "1", then commands such as *git blame* (in incremental mode), *git rev-list*, *git log*, *git check-attr* and *git check-ignore* will force a flush of the output stream after each record have been flushed. If this variable is set to "0", the output of these commands will be done using completely buffered I/O. If this environment variable is not set, Git will choose buffered or record-oriented flushing based on whether stdout appears to be redirected to a file or not.

  如果将此环境变量设置为 "1"，那么诸如 *git blame*（在增量模式下）、*git rev-list*、*git log*、*git check-attr* 和 *git check-ignore* 等命令将在每个记录刷新后强制刷新输出流。如果此变量设置为 "0"，则这些命令的输出将使用完全缓冲的 I/O 进行。如果未设置此环境变量，Git 将根据 stdout 是否重定向到文件来选择缓冲或记录导向的刷新。

- `GIT_TRACE`

  Enables general trace messages, e.g. alias expansion, built-in command execution and external command execution.If this variable is set to "1", "2" or "true" (comparison is case insensitive), trace messages will be printed to stderr.If the variable is set to an integer value greater than 2 and lower than 10 (strictly) then Git will interpret this value as an open file descriptor and will try to write the trace messages into this file descriptor.Alternatively, if the variable is set to an absolute path (starting with a */* character), Git will interpret this as a file path and will try to append the trace messages to it.Unsetting the variable, or setting it to empty, "0" or "false" (case insensitive) disables trace messages.

  启用一般的跟踪消息，例如别名扩展、内置命令执行和外部命令执行。如果此变量设置为 "1"、"2" 或 "true"（比较不区分大小写），跟踪消息将打印到 stderr。如果该变量设置为大于 2 且小于 10（严格）的整数值，则 Git 将将此值解释为打开的文件描述符，并尝试将跟踪消息写入此文件描述符。或者，如果该变量设置为绝对路径（以 `/ `字符开始），Git 将将其解释为文件路径，并尝试将跟踪消息追加到其中。取消设置该变量或将其设置为空、"0" 或 "false"（比较不区分大小写）将禁用跟踪消息。

- `GIT_TRACE_FSMONITOR`

  Enables trace messages for the filesystem monitor extension. See `GIT_TRACE` for available trace output options.

  启用文件系统监视扩展的跟踪消息。有关可用的跟踪输出选项，请参阅 `GIT_TRACE`。

- `GIT_TRACE_PACK_ACCESS`

  Enables trace messages for all accesses to any packs. For each access, the pack file name and an offset in the pack is recorded. This may be helpful for troubleshooting some pack-related performance problems. See `GIT_TRACE` for available trace output options.

  启用所有对任何包的访问的跟踪消息。对于每次访问，记录包文件名和包中的偏移量。这可能有助于排除某些与包相关的性能问题。有关可用的跟踪输出选项，请参阅 `GIT_TRACE`。

- `GIT_TRACE_PACKET`

  Enables trace messages for all packets coming in or out of a given program. This can help with debugging object negotiation or other protocol issues. Tracing is turned off at a packet starting with "PACK" (but see `GIT_TRACE_PACKFILE` below). See `GIT_TRACE` for available trace output options.

  启用给定程序的所有进出数据包的跟踪消息。这有助于调试对象协商或其他协议问题。跟踪在以 "PACK" 开头的数据包时关闭（但请参阅下面的 `GIT_TRACE_PACKFILE`）。有关可用的跟踪输出选项，请参阅 `GIT_TRACE`。

- `GIT_TRACE_PACKFILE`

  Enables tracing of packfiles sent or received by a given program. Unlike other trace output, this trace is verbatim: no headers, and no quoting of binary data. You almost certainly want to direct into a file (e.g., `GIT_TRACE_PACKFILE=/tmp/my.pack`) rather than displaying it on the terminal or mixing it with other trace output.Note that this is currently only implemented for the client side of clones and fetches.

  启用对给定程序发送或接收的包文件的跟踪。与其他跟踪输出不同，此跟踪是逐字的：无头部，也不对二进制数据进行引用。通常，您应该将其重定向到文件（例如，`GIT_TRACE_PACKFILE=/tmp/my.pack`），而不是在终端上显示它或将其与其他跟踪输出混合在一起。请注意，目前仅对克隆和获取的客户端部分实现了此功能。

- `GIT_TRACE_PERFORMANCE`

  Enables performance related trace messages, e.g. total execution time of each Git command. See `GIT_TRACE` for available trace output options.

  启用与性能相关的跟踪消息，例如每个 Git 命令的总执行时间。有关可用的跟踪输出选项，请参阅 `GIT_TRACE`。

- `GIT_TRACE_REFS`

  Enables trace messages for operations on the ref database. See `GIT_TRACE` for available trace output options.

  启用对引用数据库操作的跟踪消息。有关可用的跟踪输出选项，请参阅 `GIT_TRACE`。

- `GIT_TRACE_SETUP`

  Enables trace messages printing the .git, working tree and current working directory after Git has completed its setup phase. See `GIT_TRACE` for available trace output options.

  启用在 Git 完成设置阶段后打印 .git、工作树和当前工作目录的跟踪消息。有关可用的跟踪输出选项，请参阅 `GIT_TRACE`。

- `GIT_TRACE_SHALLOW`

  Enables trace messages that can help debugging fetching / cloning of shallow repositories. See `GIT_TRACE` for available trace output options.

  启用可帮助调试获取 / 克隆浅层仓库的跟踪消息。有关可用的跟踪输出选项，请参阅 `GIT_TRACE`。

- `GIT_TRACE_CURL`

  Enables a curl full trace dump of all incoming and outgoing data, including descriptive information, of the git transport protocol. This is similar to doing curl `--trace-ascii` on the command line. See `GIT_TRACE` for available trace output options.

  启用 git 传输协议的 curl 完整跟踪转储，包括描述信息和传入/传出数据的详细信息。这类似于在命令行上使用 curl `--trace-ascii`。有关可用的跟踪输出选项，请参阅 `GIT_TRACE`。

- `GIT_TRACE_CURL_NO_DATA`

  When a curl trace is enabled (see `GIT_TRACE_CURL` above), do not dump data (that is, only dump info lines and headers).

  当启用 curl 跟踪（参见上面的 `GIT_TRACE_CURL`）时，不要转储数据（即只转储信息行和头）。

- `GIT_TRACE2`

  Enables more detailed trace messages from the "trace2" library. Output from `GIT_TRACE2` is a simple text-based format for human readability.If this variable is set to "1", "2" or "true" (comparison is case insensitive), trace messages will be printed to stderr.If the variable is set to an integer value greater than 2 and lower than 10 (strictly) then Git will interpret this value as an open file descriptor and will try to write the trace messages into this file descriptor.Alternatively, if the variable is set to an absolute path (starting with a */* character), Git will interpret this as a file path and will try to append the trace messages to it. If the path already exists and is a directory, the trace messages will be written to files (one per process) in that directory, named according to the last component of the SID and an optional counter (to avoid filename collisions).In addition, if the variable is set to `af_unix:[<socket_type>:]<absolute-pathname>`, Git will try to open the path as a Unix Domain Socket. The socket type can be either `stream` or `dgram`.Unsetting the variable, or setting it to empty, "0" or "false" (case insensitive) disables trace messages.See [Trace2 documentation](https://git-scm.com/docs/api-trace2) for full details.

  启用 "trace2" 库的更详细的跟踪消息。`GIT_TRACE2` 的输出是用于人类可读性的简单文本格式。如果此变量设置为 "1"、"2" 或 "true"（比较不区分大小写），跟踪消息将打印到 stderr。如果该变量设置为大于 2 且小于 10（严格）的整数值，则 Git 将将此值解释为打开的文件描述符，并尝试将跟踪消息写入此文件描述符。或者，如果该变量设置为绝对路径（以 */* 字符开始），Git 将将其解释为文件路径，并尝试将跟踪消息追加到其中。如果路径已存在且是目录，则将跟踪消息写入该目录中的文件（每个进程一个文件），文件名根据 SID 的最后一个组件和可选计数器命名（以避免文件名冲突）。此外，如果变量设置为 `af_unix:[<socket_type>:]<absolute-pathname>`，Git 将尝试打开路径作为 Unix 域套接字。套接字类型可以是 `stream` 或 `dgram`。取消设置该变量或将其设置为空、"0" 或 "false"（比较不区分大小写）将禁用跟踪消息。有关完整详情，请参阅 [Trace2 文档](https://git-scm.com/docs/api-trace2)。

- `GIT_TRACE2_EVENT`

  This setting writes a JSON-based format that is suited for machine interpretation. See `GIT_TRACE2` for available trace output options and [Trace2 documentation](https://git-scm.com/docs/api-trace2) for full details.

  此设置会写入适用于机器解释的基于 JSON 的格式。有关可用的跟踪输出选项，请参阅 `GIT_TRACE2`，有关完整详情，请参阅 [Trace2 文档](https://git-scm.com/docs/api-trace2)。

- `GIT_TRACE2_PERF`

  In addition to the text-based messages available in `GIT_TRACE2`, this setting writes a column-based format for understanding nesting regions. See `GIT_TRACE2` for available trace output options and [Trace2 documentation](https://git-scm.com/docs/api-trace2) for full details.

  除了 `GIT_TRACE2` 中可用的基于文本的消息外，此设置还会写入基于列的格式，用于理解嵌套区域。有关可用的跟踪输出选项，请参阅 `GIT_TRACE2`，完整详情请参阅 [Trace2 文档](https://git-scm.com/docs/api-trace2)。

- `GIT_TRACE_REDACT`

  By default, when tracing is activated, Git redacts the values of cookies, the "Authorization:" header, the "Proxy-Authorization:" header and packfile URIs. Set this Boolean environment variable to false to prevent this redaction.

  默认情况下，当启用跟踪时，Git 会删除 cookie 值、"Authorization:" 头、"Proxy-Authorization:" 头和 packfile URIs 的值。将此布尔型环境变量设置为 false 可以阻止此删除。

- `GIT_LITERAL_PATHSPECS`

  Setting this Boolean environment variable to true will cause Git to treat all pathspecs literally, rather than as glob patterns. For example, running `GIT_LITERAL_PATHSPECS=1 git log -- '*.c'` will search for commits that touch the path `*.c`, not any paths that the glob `*.c` matches. You might want this if you are feeding literal paths to Git (e.g., paths previously given to you by `git ls-tree`, `--raw` diff output, etc).

  将此布尔型环境变量设置为 true，Git 将会将所有 pathspecs 视为字面量，而不是作为 glob 模式。例如，运行 `GIT_LITERAL_PATHSPECS=1 git log -- '*.c'` 将搜索与路径 `*.c` 相符的提交，而不是匹配 `*.c` 的任何路径。如果您将字面路径提供给 Git（例如，以前通过 `git ls-tree`、`--raw` diff 输出等给出的路径），可能需要使用此选项。

- `GIT_GLOB_PATHSPECS`

  Setting this Boolean environment variable to true will cause Git to treat all pathspecs as glob patterns (aka "glob" magic).

  将此布尔型环境变量设置为 true，Git 将会将所有 pathspecs 视为 glob 模式（也称为 "glob" 魔术）。

- `GIT_NOGLOB_PATHSPECS`

  Setting this Boolean environment variable to true will cause Git to treat all pathspecs as literal (aka "literal" magic).

  将此布尔型环境变量设置为 true，Git 将会将所有 pathspecs 视为字面量（也称为 "literal" 魔术）。

- `GIT_ICASE_PATHSPECS`

  Setting this Boolean environment variable to true will cause Git to treat all pathspecs as case-insensitive.

  将此布尔型环境变量设置为 true，Git 将会将所有 pathspecs 视为不区分大小写

- `GIT_REFLOG_ACTION`

  When a ref is updated, reflog entries are created to keep track of the reason why the ref was updated (which is typically the name of the high-level command that updated the ref), in addition to the old and new values of the ref. A scripted Porcelain command can use set_reflog_action helper function in `git-sh-setup` to set its name to this variable when it is invoked as the top level command by the end user, to be recorded in the body of the reflog.

  更新引用时，将创建 reflog 条目以跟踪更新引用的原因（通常是更新引用的高级别命令的名称），以及引用的旧值和新值。当作为最终用户调用的顶级命令时，脚本化的 Porcelain 命令可以在 `git-sh-setup` 中使用 set_reflog_action 辅助函数将其名称设置为此变量，以便在 reflog 的正文中记录。

- `GIT_REF_PARANOIA`

  If this Boolean environment variable is set to false, ignore broken or badly named refs when iterating over lists of refs. Normally Git will try to include any such refs, which may cause some operations to fail. This is usually preferable, as potentially destructive operations (e.g., [git-prune[1]](../1/git-prune)) are better off aborting rather than ignoring broken refs (and thus considering the history they point to as not worth saving). The default value is `1` (i.e., be paranoid about detecting and aborting all operations). You should not normally need to set this to `0`, but it may be useful when trying to salvage data from a corrupted repository.

  如果将此布尔型环境变量设置为 false，在遍历引用列表时忽略损坏或命名错误的引用。通常情况下，Git 将尝试包含任何此类引用，这可能会导致某些操作失败。这通常是更可取的，因为潜在破坏性的操作（例如，[git-prune[1]](../1/git-prune)）最好中止，而不是忽略损坏的引用（从而认为它们指向的历史不值得保存）。默认值为 `1`（即对于检测和中止所有操作保持警惕）。通常情况下，不需要将其设置为 `0`，但在尝试从已损坏的仓库中恢复数据时可能会有用。

- `GIT_ALLOW_PROTOCOL`

  If set to a colon-separated list of protocols, behave as if `protocol.allow` is set to `never`, and each of the listed protocols has `protocol.<name>.allow` set to `always` (overriding any existing configuration). See the description of `protocol.allow` in [git-config[1]](../1/git-config) for more details.

  如果设置为以冒号分隔的协议列表，则行为就像 `protocol.allow` 被设置为 `never`，并且列出的每个协议都被设置为 `protocol.<name>.allow` 为 `always`（覆盖任何现有的配置）。有关更多详情，请参阅 [git-config[1]](../1/git-config) 中 `protocol.allow` 的描述。

- `GIT_PROTOCOL_FROM_USER`

  Set this Boolean environment variable to false to prevent protocols used by fetch/push/clone which are configured to the `user` state. This is useful to restrict recursive submodule initialization from an untrusted repository or for programs which feed potentially-untrusted URLS to git commands. See [git-config[1]](../1/git-config) for more details.

  将此布尔型环境变量设置为 false，可以阻止由配置为 `user` 状态的 fetch/push/clone 使用的协议。这对于限制从不受信任的仓库进行递归子模块初始化或向 Git 命令提供可能不受信任的 URLS 的程序非常有用。有关更多详情，请参阅 [git-config[1]](../1/git-config)。

- `GIT_PROTOCOL`

  For internal use only. Used in handshaking the wire protocol. Contains a colon *:* separated list of keys with optional values *key[=value]*. Presence of unknown keys and values must be ignored.Note that servers may need to be configured to allow this variable to pass over some transports. It will be propagated automatically when accessing local repositories (i.e., `file://` or a filesystem path), as well as over the `git://` protocol. For git-over-http, it should work automatically in most configurations, but see the discussion in [git-http-backend[1]](../1/git-http-backend). For git-over-ssh, the ssh server may need to be configured to allow clients to pass this variable (e.g., by using `AcceptEnv GIT_PROTOCOL` with OpenSSH).This configuration is optional. If the variable is not propagated, then clients will fall back to the original "v0" protocol (but may miss out on some performance improvements or features). This variable currently only affects clones and fetches; it is not yet used for pushes (but may be in the future).

  仅供内部使用。用于在握手过程中进行协议的传输。包含以冒号 *:* 分隔的键列表，每个键可以有可选的值 *key[=value]*。应忽略未知键和值。请注意，服务器可能需要配置以允许此变量通过某些传输。它将在访问本地仓库（即 `file://` 或文件系统路径）时自动传播，以及通过 `git://` 协议传播。对于 git-over-http，它在大多数配置中应该自动工作，但请参阅 [git-http-backend[1]](../1/git-http-backend) 中的讨论。对于 git-over-ssh，可能需要配置 ssh 服务器以允许客户端传递此变量（例如，使用 OpenSSH 的 `AcceptEnv GIT_PROTOCOL`）。此配置是可选的。如果不传播此变量，则客户端将回退到原始的 "v0" 协议（但可能会错过某些性能改进或功能）。目前，此变量仅影响克隆和获取；对于推送尚未使用（但将来可能会使用）。

- `GIT_OPTIONAL_LOCKS`

  If this Boolean environment variable is set to false, Git will complete any requested operation without performing any optional sub-operations that require taking a lock. For example, this will prevent `git status` from refreshing the index as a side effect. This is useful for processes running in the background which do not want to cause lock contention with other operations on the repository. Defaults to `1`.

  如果将此布尔型环境变量设置为 false，则 Git 将在完成任何需要锁定的可选子操作时，完成任何请求的操作。例如，这将防止 `git status` 刷新索引作为副作用。这对于在后台运行的进程非常有用，这些进程不想与仓库上的其他操作引起锁争用。默认为 `1`。

- `GIT_REDIRECT_STDIN`

- `GIT_REDIRECT_STDOUT`

- `GIT_REDIRECT_STDERR`

  Windows-only: allow redirecting the standard input/output/error handles to paths specified by the environment variables. This is particularly useful in multi-threaded applications where the canonical way to pass standard handles via `CreateProcess()` is not an option because it would require the handles to be marked inheritable (and consequently **every** spawned process would inherit them, possibly blocking regular Git operations). The primary intended use case is to use named pipes for communication (e.g. `\\.\pipe\my-git-stdin-123`).Two special values are supported: `off` will simply close the corresponding standard handle, and if `GIT_REDIRECT_STDERR` is `2>&1`, standard error will be redirected to the same handle as standard output.

  仅适用于 Windows：允许将标准输入/输出/错误处理重定向到由环境变量指定的路径。这在多线程应用程序中特别有用，其中通过 `CreateProcess()` 传递标准句柄的规范方式不可行，因为这将要求将句柄标记为可继承的（从而导致**每个**生成的进程都将继承它们，可能阻塞常规 Git 操作）。主要的预期用例是使用命名管道进行通信（例如，`\\.\pipe\my-git-stdin-123`）。支持两个特殊值：`off` 将简单地关闭相应的标准句柄，如果 `GIT_REDIRECT_STDERR` 为 `2>&1`，则标准错误将重定向到与标准输出相同的句柄。

- `GIT_PRINT_SHA1_ELLIPSIS` (deprecated)

  If set to `yes`, print an ellipsis following an (abbreviated) SHA-1 value. This affects indications of detached HEADs ([git-checkout[1]](../1/git-checkout)) and the raw diff output ([git-diff[1]](../1/git-diff)). Printing an ellipsis in the cases mentioned is no longer considered adequate and support for it is likely to be removed in the foreseeable future (along with the variable).
  
  如果设置为 `yes`，在（缩写的）SHA-1 值后面打印省略号。这会影响分离的 HEADs（[git-checkout[1]](../1/git-checkout)）和原始 diff 输出（[git-diff[1]](../1/git-diff)）的指示。在上述情况下打印省略号已不再被认为是合适的，对此的支持可能会在可预见的将来被删除（以及此变量）。

## 讨论

More detail on the following is available from the [Git concepts chapter of the user-manual](https://git-scm.com/docs/user-manual#git-concepts) and [gitcore-tutorial[7]](../7/gitcore-tutorial).

​	关于下面内容的更多细节可在[用户手册中的 Git 概念章节](https://git-scm.com/docs/user-manual#git-concepts)和[gitcore-tutorial[7]](../7/gitcore-tutorial)中找到。

A Git project normally consists of a working directory with a ".git" subdirectory at the top level. The .git directory contains, among other things, a compressed object database representing the complete history of the project, an "index" file which links that history to the current contents of the working tree, and named pointers into that history such as tags and branch heads.

​	一个 Git 项目通常由一个工作目录和位于顶层的 ".git" 子目录组成。.git 目录包含了项目完整历史的压缩对象数据库，一个 "index" 文件将历史链接到当前工作树的内容，以及指向历史中具体位置的命名指针，例如标签和分支头。

The object database contains objects of three main types: blobs, which hold file data; trees, which point to blobs and other trees to build up directory hierarchies; and commits, which each reference a single tree and some number of parent commits.

​	对象数据库包含三种主要类型的对象：blob，保存文件数据；tree，指向 blob 和其他 tree 以构建目录层次结构；以及 commit，每个 commit 引用单个 tree 和一些父提交。

The commit, equivalent to what other systems call a "changeset" or "version", represents a step in the project’s history, and each parent represents an immediately preceding step. Commits with more than one parent represent merges of independent lines of development.

​	commit 相当于其他系统中称为 "changeset" 或 "version" 的内容，表示项目历史的一步，每个父提交代表着前一步。具有多个父提交的 commit 表示合并独立开发线。

All objects are named by the SHA-1 hash of their contents, normally written as a string of 40 hex digits. Such names are globally unique. The entire history leading up to a commit can be vouched for by signing just that commit. A fourth object type, the tag, is provided for this purpose.

​	所有对象都由其内容的 SHA-1 哈希命名，通常写为一个包含 40 个十六进制数字的字符串。这样的名称在全球范围内是唯一的。对于一个 commit，可以通过签署该 commit 来保证所有历史。第四种对象类型，tag，用于此目的。

When first created, objects are stored in individual files, but for efficiency may later be compressed together into "pack files".

​	当首次创建时，对象存储在单独的文件中，但为了效率，后来可能会被一起压缩成 "pack files"。

Named pointers called refs mark interesting points in history. A ref may contain the SHA-1 name of an object or the name of another ref. Refs with names beginning `ref/head/` contain the SHA-1 name of the most recent commit (or "head") of a branch under development. SHA-1 names of tags of interest are stored under `ref/tags/`. A special ref named `HEAD` contains the name of the currently checked-out branch.

​	称为 refs 的命名指针标记了历史上的有趣点。一个 ref 可能包含一个对象的 SHA-1 名称或另一个 ref 的名称。名称以 `ref/heads/` 开头的 refs 包含开发中的分支的最近一次提交（或 "head"）的 SHA-1 名称。感兴趣的标签的 SHA-1 名称存储在 `ref/tags/` 下。名为 `HEAD` 的特殊 ref 包含当前检出分支的名称。

The index file is initialized with a list of all paths and, for each path, a blob object and a set of attributes. The blob object represents the contents of the file as of the head of the current branch. The attributes (last modified time, size, etc.) are taken from the corresponding file in the working tree. Subsequent changes to the working tree can be found by comparing these attributes. The index may be updated with new content, and new commits may be created from the content stored in the index.

​	index 文件初始化时包含所有路径的列表，对于每个路径，它包含一个 blob 对象和一组属性。blob 对象表示文件的内容，作为当前分支头的内容。属性（最后修改时间、大小等）取自工作树中对应文件的属性。对工作树的后续更改可以通过比较这些属性找到。index 可能会更新为新内容，并可以从 index 中存储的内容创建新的提交。

The index is also capable of storing multiple entries (called "stages") for a given pathname. These stages are used to hold the various unmerged version of a file when a merge is in progress.

​	index 还能够为给定的路径名存储多个条目（称为 "stages"）。这些 stages 用于在合并进行时保留文件的各个未合并版本。

## 更多文档 FURTHER DOCUMENTATION

See the references in the "description" section to get started using Git. The following is probably more detail than necessary for a first-time user.

​	查看 "描述" 部分中的引用，以开始使用 Git。以下内容可能比初次使用者需要的更详细。

The [Git concepts chapter of the user-manual](https://git-scm.com/docs/user-manual#git-concepts) and [gitcore-tutorial[7]](../7/gitcore-tutorial) both provide introductions to the underlying Git architecture.

​	[用户手册中的 Git 概念章节](https://git-scm.com/docs/user-manual#git-concepts)和[gitcore-tutorial[7]](../7/gitcore-tutorial)都提供了对底层 Git 架构的介绍。

See [gitworkflows[7]](../7/gitworkflows) for an overview of recommended workflows.

​	参见[gitworkflows[7]](../7/gitworkflows)以了解推荐的工作流程概述。

See also the [howto](https://git-scm.com/docs/howto-index) documents for some useful examples.

​	还可以参考 [howto](https://git-scm.com/docs/howto-index) 文档，其中包含一些有用的示例。

The internals are documented in the [Git API documentation](https://git-scm.com/docs/api-index).

​	内部结构在 [Git API 文档](https://git-scm.com/docs/api-index) 中有所记录。

Users migrating from CVS may also want to read [gitcvs-migration[7]](../7/gitcvs-migration).

​	从 CVS 迁移的用户可能还想阅读 [gitcvs-migration[7]](../7/gitcvs-migration)。

## 作者

Git was started by Linus Torvalds, and is currently maintained by Junio C Hamano. Numerous contributions have come from the Git mailing list <[git@vger.kernel.org](mailto:git@vger.kernel.org)>. http://www.openhub.net/p/git/contributors/summary gives you a more complete list of contributors.http://www.openhub.net/p/git/contributors/summary 提供了更完整的贡献者列表。

​	Git 是由 Linus Torvalds 开始的，目前由 Junio C Hamano 维护。许多贡献来自 Git 邮件列表 <[git@vger.kernel.org](mailto:git@vger.kernel.org)>。

If you have a clone of git.git itself, the output of [git-shortlog[1]](../1/git-shortlog) and [git-blame[1]](../1/git-blame) can show you the authors for specific parts of the project.

如果您克隆了 git.git 本身，[git-shortlog[1]](../1/git-shortlog) 和 [git-blame[1]](../1/git-blame) 的输出可以显示项目特定部分的作者。

## 报告漏洞

Report bugs to the Git mailing list <[git@vger.kernel.org](mailto:git@vger.kernel.org)> where the development and maintenance is primarily done. You do not have to be subscribed to the list to send a message there. See the list archive at https://lore.kernel.org/git for previous bug reports and other discussions.

​	将漏洞报告发送到 Git 邮件列表 <[git@vger.kernel.org](mailto:git@vger.kernel.org)>，那里是主要进行开发和维护的地方。您无需订阅该列表即可发送消息。在 https://lore.kernel.org/git 上可以查看以前的漏洞报告和其他讨论。

Issues which are security relevant should be disclosed privately to the Git Security mailing list <[git-security@googlegroups.com](mailto:git-security@googlegroups.com)>.

​	涉及安全的问题应当私下披露给 Git 安全邮件列表 <[git-security@googlegroups.com](mailto:git-security@googlegroups.com)>。

## 参见

[gittutorial[7]](../7/gittutorial), [gittutorial-2[7]](../7/gittutorial-2), [giteveryday[7]](../7/giteveryday), [gitcvs-migration[7]](../7/gitcvs-migration), [gitglossary[7]](../7/gitglossary), [gitcore-tutorial[7]](../7/gitcore-tutorial), [gitcli[7]](../7/gitcli), [The Git User’s Manual](https://git-scm.com/docs/user-manual), [gitworkflows[7]](../7/gitworkflows)

## GIT

Part of the [git[1]](../1/git) suite

​	Git 套件的一部分 [git[1]](../1/git)

