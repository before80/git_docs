https://git-scm.com/docs/git

## 名称

git —— 愚蠢的内容跟踪器（the stupid content tracker）

## 用法

```
git [-v | --version] [-h | --help] [-C <path>] [-c <name>=<value>]
    [--exec-path[=<path>]] [--html-path] [--man-path] [--info-path]
    [-p|--paginate|-P|--no-pager] [--no-replace-objects] [--bare]
    [--git-dir=<path>] [--work-tree=<path>] [--namespace=<name>]
    [--super-prefix=<path>] [--config-env=<name>=<envvar>]
    <command> [<args>]
```

## 描述

    Git是一个快速、可扩展、分布式的版本控制系统，拥有异常丰富的命令集，既能提供高级操作，又能完全访问内部功能。
    
    参见[gittutorial[7]](../7/gittutorial) 开始学习，然后查看 [giteveryday[7]](../7/giteveryday) 获取有用的最小命令集。[Git 用户手册](../../UserManual)提供了更深入的介绍。
    
    当你掌握了基本概念后，你可以回到这个页面了解 Git 提供了哪些命令。你可以使用 "git help command" 了解更多关于单个 Git 命令的信息。[gitcli[7]](../7/gitcli) 手册页面为你提供了命令行命令语法的概述。
    
    最新 Git 文档的格式化和超链接副本可在[https://git.github.io/htmldocs/git.html](https://git.github.io/htmldocs/git.html) 或 [https://git-scm.com/docs](https://git-scm.com/docs) 查看。

## 选项

- -v
- --version

  Prints the Git suite version that the *git* program came from.This option is internally converted to `git version ...` and accepts the same options as the [git-version[1]](https://git-scm.com/docs/git-version) command. If `--help` is also given, it takes precedence over `--version`.

  打印git程序所来自的Git套件版本。 该选项在内部被转换为git版本...，接受与git-version[1]命令相同的选项。如果同时给出了 --help，它将优先于 --version。
- -h
- --help

  Prints the synopsis and a list of the most commonly used commands. If the option `--all` or `-a` is given then all available commands are printed. If a Git command is named this option will bring up the manual page for that command.Other options are available to control how the manual page is displayed. See [git-help[1]](https://git-scm.com/docs/git-help) for more information, because `git --help ...` is converted internally into `git help ...`.

  打印概要和最常用的命令列表。如果给出选项 --all 或 -a，则会打印所有可用的命令。如果某条Git命令被命名，该选项将调出该命令的手册页面。

  还有其他选项可以控制手册页面的显示方式。更多信息请参见 git-help[1]，因为 git --help ... 在内部被转换为 git help ....
- -C `<path>`

  Run as if git was started in *`<path>`* instead of the current working directory. When multiple `-C` options are given, each subsequent non-absolute `-C <path>` is interpreted relative to the preceding `-C <path>`. If *`<path>`* is present but empty, e.g. `-C ""`, then the current working directory is left unchanged.This option affects options that expect path name like `--git-dir` and `--work-tree` in that their interpretations of the path names would be made relative to the working directory caused by the `-C` option. For example the following invocations are equivalent:`git --git-dir=a.git --work-tree=b -C c status git --git-dir=c/a.git --work-tree=c/b status`

  以 git 在 `<path>` 而不是当前工作目录中启动的方式运行。当给出多个 -C 选项时，每个后续的非绝对值 -C `<path>` 都是相对于前面的 -C `<path>` 解释的。如果`<path>`存在但为空，例如：-C ""，那么当前工作目录将保持不变。

  这个选项影响了像--git-dir和--work-tree这样期待路径名的选项，因为它们对路径名的解释会相对于由-C选项引起的工作目录。例如，下面的调用是等同的。
- -c `<name>`=`<value>`

  Pass a configuration parameter to the command. The value given will override values from configuration files. The `<name>` is expected in the same format as listed by *git config* (subkeys separated by dots).Note that omitting the `=` in `git -c foo.bar ...` is allowed and sets `foo.bar` to the boolean true value (just like `[foo]bar` would in a config file). Including the equals but with an empty value (like `git -c foo.bar= ...`) sets `foo.bar` to the empty string which `git config --type=bool` will convert to `false`.

  向命令传递一个配置参数。给出的值将覆盖配置文件中的值。<名>的格式应与git config列出的格式相同（子键由点分隔）。

  请注意，允许省略git -c foo.bar ...中的=，并将foo.bar设置为布尔真值（就像配置文件中的[foo]bar那样）。包括equals但有一个空值（比如git -c foo.bar= ...）会将foo.bar设为空字符串，git config --type=bool会将其转换为false。
- --config-env=`<name>`=`<envvar>`

  Like `-c <name>=<value>`, give configuration variable *`<name>`* a value, where `<envvar>` is the name of an environment variable from which to retrieve the value. Unlike `-c` there is no shortcut for directly setting the value to an empty string, instead the environment variable itself must be set to the empty string. It is an error if the `<envvar>` does not exist in the environment. `<envvar>` may not contain an equals sign to avoid ambiguity with `<name>` containing one.This is useful for cases where you want to pass transitory configuration options to git, but are doing so on OS’s where other processes might be able to read your cmdline (e.g. `/proc/self/cmdline`), but not your environ (e.g. `/proc/self/environ`). That behavior is the default on Linux, but may not be on your system.Note that this might add security for variables such as `http.extraHeader` where the sensitive information is part of the value, but not e.g. `url.<base>.insteadOf` where the sensitive information can be part of the key.

  和 -c `<name>`=`<value>` 一样，给配置变量 `<name>` 一个值，其中 `<envvar>` 是一个环境变量的名字，可以从中获取数值。与-c不同的是，没有直接将值设置为空字符串的快捷方式，相反，环境变量本身必须被设置为空字符串。如果`<envvar>`在环境中不存在，那是一个错误。`<envvar>`不能包含等号，以避免与包含等号的`<name>`产生歧义。

  这对以下情况很有用：你想把临时配置选项传递给git，但在其他进程可能能够读取你的cmdline（例如/proc/self/cmdline），但不能读取你的environ（例如/proc/self/environ）的操作系统上这样做。这种行为在Linux上是默认的，但在你的系统上可能不是这样。

  请注意，这可能会增加变量的安全性，例如http.extraHeader，其中敏感信息是值的一部分，但不是如url.`<base>`.insteadOf，其中敏感信息可能是键的一部分。
- --exec-path[=`<path>`]

  Path to wherever your core Git programs are installed. This can also be controlled by setting the GIT_EXEC_PATH environment variable. If no path is given, *git* will print the current setting and then exit.

  安装Git核心程序的路径。这也可以通过设置 GIT_EXEC_PATH 环境变量来控制。如果没有给出路径，git 会打印当前的设置，然后退出。
- --html-path

  Print the path, without trailing slash, where Git’s HTML documentation is installed and exit.

  打印Git的HTML文档的安装路径，不带尾部斜线，然后退出。
- --man-path

  Print the manpath (see `man(1)`) for the man pages for this version of Git and exit.

  打印这个版本的Git的manpath（见man(1)），以获取man页，然后退出。
- --info-path

  Print the path where the Info files documenting this version of Git are installed and exit.

  打印记录该版本Git的信息文件的安装路径，然后退出。
- -p
- --paginate

  Pipe all output into *less* (or if set, $PAGER) if standard output is a terminal. This overrides the `pager.<cmd>` configuration options (see the "Configuration Mechanism" section below).

  如果标准输出是一个终端，则将所有输出转入 less（或者如果设置了 $PAGER）。这将覆盖 pager.`<cmd>` 的配置选项（见下面的 "配置机制 "部分）。
- -P
- --no-pager

  Do not pipe Git output into a pager.

  不将Git的输出通过管道输送到pager中。
- --git-dir=`<path>`

  Set the path to the repository (".git" directory). This can also be controlled by setting the `GIT_DIR` environment variable. It can be an absolute path or relative path to current working directory.Specifying the location of the ".git" directory using this option (or `GIT_DIR` environment variable) turns off the repository discovery that tries to find a directory with ".git" subdirectory (which is how the repository and the top-level of the working tree are discovered), and tells Git that you are at the top level of the working tree. If you are not at the top-level directory of the working tree, you should tell Git where the top-level of the working tree is, with the `--work-tree=<path>` option (or `GIT_WORK_TREE` environment variable)If you just want to run git as if it was started in `<path>` then use `git -C <path>`.

  设置存储库的路径（".git "目录）。这也可以通过设置 GIT_DIR 环境变量来控制。它可以是绝对路径，也可以是当前工作目录的相对路径。

  使用这个选项（或GIT_DIR环境变量）指定".git "目录的位置，会关闭试图寻找带有".git "子目录的仓库发现（这是发现仓库和工作树顶层的方式），并告诉Git你在工作树的顶层。如果你不在工作树的顶层目录下，你应该用 --work-tree=`<path>` 选项（或 GIT_WORK_TREE 环境变量）告诉 Git 工作树的顶层在哪里

  如果你只是想运行git，就像它在`<path>`中启动一样，那么使用git -C `<path>`。
- --work-tree=`<path>`

  Set the path to the working tree. It can be an absolute path or a path relative to the current working directory. This can also be controlled by setting the GIT_WORK_TREE environment variable and the core.worktree configuration variable (see core.worktree in [git-config[1]](https://git-scm.com/docs/git-config) for a more detailed discussion).

  设置工作树的路径。它可以是一个绝对路径或相对于当前工作目录的路径。这也可以通过设置GIT_WORK_TREE环境变量和core.worktree配置变量来控制（见git-config[1]中的core.worktree，有更详细的讨论）。
- --namespace=`<path>`

  Set the Git namespace. See [gitnamespaces[7]](https://git-scm.com/docs/gitnamespaces) for more details. Equivalent to setting the `GIT_NAMESPACE` environment variable.

  设置 Git 命名空间。更多细节见 gitnamespaces[7]。相当于设置 GIT_NAMESPACE 环境变量。
- --super-prefix=`<path>`

  Currently for internal use only. Set a prefix which gives a path from above a repository down to its root. One use is to give submodules context about the superproject that invoked it.

  目前只在内部使用。设置一个前缀，提供一个从版本库上方到根的路径。一个用途是给子模块提供关于调用它的超级项目的上下文。
- --bare

  Treat the repository as a bare repository. If GIT_DIR environment is not set, it is set to the current working directory.

  将版本库作为一个裸版本库。如果没有设置 GIT_DIR 环境，它将被设置为当前工作目录。
- --no-replace-objects

  Do not use replacement refs to replace Git objects. See [git-replace[1]](https://git-scm.com/docs/git-replace) for more information.

  不使用替换 refs 来替换 Git 对象。更多信息见 git-replace[1]。
- --literal-pathspecs

  Treat pathspecs literally (i.e. no globbing, no pathspec magic). This is equivalent to setting the `GIT_LITERAL_PATHSPECS` environment variable to `1`.

  按字面意思处理路径规格（即不使用globbing，不使用路径规格魔法）。这相当于将 GIT_LITERAL_PATHSPECS 环境变量设为 1。
- --glob-pathspecs

  Add "glob" magic to all pathspec. This is equivalent to setting the `GIT_GLOB_PATHSPECS` environment variable to `1`. Disabling globbing on individual pathspecs can be done using pathspec magic ":(literal)"

  为所有的pathspec添加 "glob "魔法。这相当于将GIT_GLOB_PATHSPECS环境变量设为1。禁用单个路径规格的globing可以用路径规格魔法":(literal) "来完成。
- --noglob-pathspecs

  Add "literal" magic to all pathspec. This is equivalent to setting the `GIT_NOGLOB_PATHSPECS` environment variable to `1`. Enabling globbing on individual pathspecs can be done using pathspec magic ":(glob)"

  给所有的pathspec添加 "literal "魔法。这相当于将GIT_NOGLOB_PATHSPECS环境变量设为1。在单个路径规格上启用球化，可以使用路径规格魔法":(glob)"
- --icase-pathspecs

  Add "icase" magic to all pathspec. This is equivalent to setting the `GIT_ICASE_PATHSPECS` environment variable to `1`.

  给所有的pathspec添加 "icase "魔法。这相当于将GIT_ICASE_PATHSPECS环境变量设为1。
- --no-optional-locks

  Do not perform optional operations that require locks. This is equivalent to setting the `GIT_OPTIONAL_LOCKS` to `0`.

  不执行需要锁的可选操作。这相当于将GIT_OPTIONAL_LOCKS设为0。
- --list-cmds=group[,group…]

  List commands by group. This is an internal/experimental option and may change or be removed in the future. Supported groups are: builtins, parseopt (builtin commands that use parse-options), main (all commands in libexec directory), others (all other commands in `$PATH` that have git- prefix), list-`<category>` (see categories in command-list.txt), nohelpers (exclude helper commands), alias and config (retrieve command list from config variable completion.commands)

  按组列出命令。这是一个内部/实验性的选项，将来可能会改变或被删除。支持的组有：buildins、parseopt（使用parse-options的内置命令）、main（libexec目录下的所有命令）、others（$PATH中所有其他带有git-前缀的命令）、list-`<category>`（见command-list.txt中的类别）、nohelpers（排除辅助命令）、alias和config（从配置变量completion.commands中检索命令列表）

## (GIT 命令)GIT COMMANDS

We divide Git into high level ("porcelain") commands and low level ("plumbing") commands.

我们把 Git 分为高级（"瓷器"）命令和低级（"管道"）命令。

## High-level commands (porcelain) 高级命令（瓷器）

We separate the porcelain commands into the main commands and some ancillary user utilities.

我们将 porcelain 命令分为主要命令和一些辅助性的用户工具。

### Main porcelain commands 主要的 porcelain 命令

- [git-add[1]](https://git-scm.com/docs/git-add)

  Add file contents to the index

  将文件内容添加到索引中
- [git-am[1]](https://git-scm.com/docs/git-am)

  Apply a series of patches from a mailbox

  从一个邮箱中应用一系列的补丁
- [git-archive[1]](https://git-scm.com/docs/git-archive)

  Create an archive of files from a named tree

  从一个命名的树中创建一个文件档案
- [git-bisect[1]](https://git-scm.com/docs/git-bisect)

  Use binary search to find the commit that introduced a bug

  使用二进制搜索来找到引入错误的提交内容
- [git-branch[1]](https://git-scm.com/docs/git-branch)

  List, create, or delete branches

  列出、创建、或删除分支
- [git-bundle[1]](https://git-scm.com/docs/git-bundle)

  Move objects and refs by archive

  通过存档移动对象和参考文献
- [git-checkout[1]](https://git-scm.com/docs/git-checkout)

  Switch branches or restore working tree files

  切换分支或恢复工作树文件
- [git-cherry-pick[1]](https://git-scm.com/docs/git-cherry-pick)

  Apply the changes introduced by some existing commits

  应用一些现有提交所带来的变化
- [git-citool[1]](https://git-scm.com/docs/git-citool)

  Graphical alternative to git-commit

  替代 git-commit 的图形化工具
- [git-clean[1]](https://git-scm.com/docs/git-clean)

  Remove untracked files from the working tree

  从工作树上删除未被追踪的文件
- [git-clone[1]](https://git-scm.com/docs/git-clone)

  Clone a repository into a new directory

  将一个版本库克隆到一个新的目录中
- [git-commit[1]](https://git-scm.com/docs/git-commit)

  Record changes to the repository

  记录对版本库的修改
- [git-describe[1]](https://git-scm.com/docs/git-describe)

  Give an object a human readable name based on an available ref

  根据一个可用的参考文献，给一个对象起一个人类可读的名字
- [git-diff[1]](https://git-scm.com/docs/git-diff)

  Show changes between commits, commit and working tree, etc

  显示提交、提交和工作树之间的变化，等等。
- [git-fetch[1]](https://git-scm.com/docs/git-fetch)

  Download objects and refs from another repository

  从另一个存储库下载对象和参考文献
- [git-format-patch[1]](https://git-scm.com/docs/git-format-patch)

  Prepare patches for e-mail submission

  为提交电子邮件准备补丁
- [git-gc[1]](https://git-scm.com/docs/git-gc)

  Cleanup unnecessary files and optimize the local repository

  清理不必要的文件，优化本地版本库
- [git-grep[1]](https://git-scm.com/docs/git-grep)

  Print lines matching a pattern

  打印符合模式的行
- [git-gui[1]](https://git-scm.com/docs/git-gui)

  A portable graphical interface to Git

  一个可移植的 Git 图形界面
- [git-init[1]](https://git-scm.com/docs/git-init)

  Create an empty Git repository or reinitialize an existing one

  创建一个空的 Git 仓库或重新初始化一个现有的仓库
- [git-log[1]](https://git-scm.com/docs/git-log)

  Show commit logs

  显示提交日志
- [git-maintenance[1]](https://git-scm.com/docs/git-maintenance)

  Run tasks to optimize Git repository data

  运行任务以优化 Git 仓库的数据
- [git-merge[1]](https://git-scm.com/docs/git-merge)

  Join two or more development histories together

  将两个或多个开发历史连接在一起
- [git-mv[1]](https://git-scm.com/docs/git-mv)

  Move or rename a file, a directory, or a symlink

  移动或重命名一个文件、一个目录或一个符号链接
- [git-notes[1]](https://git-scm.com/docs/git-notes)

  Add or inspect object notes

  添加或检查对象注释
- [git-pull[1]](https://git-scm.com/docs/git-pull)

  Fetch from and integrate with another repository or a local branch

  从另一个版本库或本地分支获取并与之整合
- [git-push[1]](https://git-scm.com/docs/git-push)

  Update remote refs along with associated objects

  更新远程参考文献和相关对象
- [git-range-diff[1]](https://git-scm.com/docs/git-range-diff)

  Compare two commit ranges (e.g. two versions of a branch)

  比较两个提交范围（例如，一个分支的两个版本）。
- [git-rebase[1]](https://git-scm.com/docs/git-rebase)

  Reapply commits on top of another base tip

  在另一个基础提示的基础上重新提交
- [git-reset[1]](https://git-scm.com/docs/git-reset)

  Reset current HEAD to the specified state

  将当前的 HEAD 重置到指定的状态
- [git-restore[1]](https://git-scm.com/docs/git-restore)

  Restore working tree files

  恢复工作树文件
- [git-revert[1]](https://git-scm.com/docs/git-revert)

  Revert some existing commits

  还原一些现有的提交
- [git-rm[1]](https://git-scm.com/docs/git-rm)

  Remove files from the working tree and from the index

  从工作树和索引中删除文件
- [git-shortlog[1]](https://git-scm.com/docs/git-shortlog)

  Summarize *git log* output

  总结 git 日志的输出
- [git-show[1]](https://git-scm.com/docs/git-show)

  Show various types of objects

  显示各种类型的对象
- [git-sparse-checkout[1]](https://git-scm.com/docs/git-sparse-checkout)

  Reduce your working tree to a subset of tracked files

  将您的工作树缩减为一个被追踪的文件子集
- [git-stash[1]](https://git-scm.com/docs/git-stash)

  Stash the changes in a dirty working directory away

  把工作目录中的变化藏起来。
- [git-status[1]](https://git-scm.com/docs/git-status)

  Show the working tree status

  显示工作树的状态
- [git-submodule[1]](https://git-scm.com/docs/git-submodule)

  Initialize, update or inspect submodules

  初始化、更新或检查子模块
- [git-switch[1]](https://git-scm.com/docs/git-switch)

  Switch branches

  切换分支
- [git-tag[1]](https://git-scm.com/docs/git-tag)

  Create, list, delete or verify a tag object signed with GPG

  创建、列出、删除或验证一个用 GPG 签名的标签对象
- [git-worktree[1]](https://git-scm.com/docs/git-worktree)

  Manage multiple working trees

  管理多个工作树
- [gitk[1]](https://git-scm.com/docs/gitk)

  The Git repository browser

  Git 仓库浏览器
- [scalar[1]](https://git-scm.com/docs/scalar)

  A tool for managing large Git repositories

  一个管理大型 Git 仓库的工具

### Ancillary Commands 辅助命令

Manipulators: 操纵器。

- [git-config[1]](https://git-scm.com/docs/git-config)

  Get and set repository or global options

  获取和设置存储库或全局选项
- [git-fast-export[1]](https://git-scm.com/docs/git-fast-export)

  Git data exporter

  Git 数据导出器
- [git-fast-import[1]](https://git-scm.com/docs/git-fast-import)

  Backend for fast Git data importers

  快速 Git 数据导入器的后端
- [git-filter-branch[1]](https://git-scm.com/docs/git-filter-branch)

  Rewrite branches

  重写分支
- [git-mergetool[1]](https://git-scm.com/docs/git-mergetool)

  Run merge conflict resolution tools to resolve merge conflicts

  运行合并冲突解决工具来解决合并冲突
- [git-pack-refs[1]](https://git-scm.com/docs/git-pack-refs)

  Pack heads and tags for efficient repository access

  打包头和标签，以提高版本库的访问效率
- [git-prune[1]](https://git-scm.com/docs/git-prune)

  Prune all unreachable objects from the object database

  从对象数据库中修剪所有不可达的对象
- [git-reflog[1]](https://git-scm.com/docs/git-reflog)

  Manage reflog information

  管理 reflog 信息
- [git-remote[1]](https://git-scm.com/docs/git-remote)

  Manage set of tracked repositories

  管理一组被追踪的仓库
- [git-repack[1]](https://git-scm.com/docs/git-repack)

  Pack unpacked objects in a repository

  打包仓库中未打包的对象
- [git-replace[1]](https://git-scm.com/docs/git-replace)

  Create, list, delete refs to replace objects

  创建、列出、删除替换对象的参考文献

Interrogators:

讯问器。

- [git-annotate[1]](https://git-scm.com/docs/git-annotate)

  Annotate file lines with commit information

  用提交信息对文件行进行注释
- [git-blame[1]](https://git-scm.com/docs/git-blame)

  Show what revision and author last modified each line of a file

  显示文件每一行最后修改的版本和作者
- [git-bugreport[1]](https://git-scm.com/docs/git-bugreport)

  Collect information for user to file a bug report

  收集用户提交错误报告所需的信息
- [git-count-objects[1]](https://git-scm.com/docs/git-count-objects)

  Count unpacked number of objects and their disk consumption

  计算未打包的对象的数量及其磁盘消耗量
- [git-diagnose[1]](https://git-scm.com/docs/git-diagnose)

  Generate a zip archive of diagnostic information

  生成一个诊断信息的 zip 档案
- [git-difftool[1]](https://git-scm.com/docs/git-difftool)

  Show changes using common diff tools

  使用常见的差异工具显示变化
- [git-fsck[1]](https://git-scm.com/docs/git-fsck)

  Verifies the connectivity and validity of the objects in the database

  验证数据库中的对象的连接性和有效性
- [git-help[1]](https://git-scm.com/docs/git-help)

  Display help information about Git

  显示关于 Git 的帮助信息
- [git-instaweb[1]](https://git-scm.com/docs/git-instaweb)

  Instantly browse your working repository in gitweb

  在 gitweb 中即时浏览你的工作仓库
- [git-merge-tree[1]](https://git-scm.com/docs/git-merge-tree)

  Perform merge without touching index or working tree

  在不碰触索引或工作树的情况下进行合并
- [git-rerere[1]](https://git-scm.com/docs/git-rerere)

  Reuse recorded resolution of conflicted merges

  重用记录的冲突合并的解决方案
- [git-show-branch[1]](https://git-scm.com/docs/git-show-branch)

  Show branches and their commits

  显示分支和它们的提交
- [git-verify-commit[1]](https://git-scm.com/docs/git-verify-commit)

  Check the GPG signature of commits

  检查提交文件的 GPG 签名
- [git-verify-tag[1]](https://git-scm.com/docs/git-verify-tag)

  Check the GPG signature of tags

  检查标签的 GPG 签名
- [git-version[1]](https://git-scm.com/docs/git-version)

  Display version information about Git

  显示有关 Git 的版本信息
- [git-whatchanged[1]](https://git-scm.com/docs/git-whatchanged)

  Show logs with difference each commit introduces

  显示每次提交引入的差异日志
- [gitweb[1]](https://git-scm.com/docs/gitweb)

  Git web interface (web frontend to Git repositories)

  Git 网页界面（Git 仓库的网页前台）

### Interacting with Others 与他人交互

These commands are to interact with foreign SCM and with other people via patch over e-mail.

这些命令是为了与国外的 SCM 以及通过电子邮件的补丁与其他人进行互动。

- [git-archimport[1]](https://git-scm.com/docs/git-archimport)

  Import a GNU Arch repository into Git

  导入 GNU Arch 仓库到 Git
- [git-cvsexportcommit[1]](https://git-scm.com/docs/git-cvsexportcommit)

  Export a single commit to a CVS checkout

  导出单个提交到 CVS 签出中
- [git-cvsimport[1]](https://git-scm.com/docs/git-cvsimport)

  Salvage your data out of another SCM people love to hate

  把你的数据从另一个人们喜欢讨厌的 SCM 中救出来
- [git-cvsserver[1]](https://git-scm.com/docs/git-cvsserver)

  A CVS server emulator for Git

  一个用于 Git 的 CVS 服务器仿真器
- [git-imap-send[1]](https://git-scm.com/docs/git-imap-send)

  Send a collection of patches from stdin to an IMAP folder

  从 stdin 发送一组补丁到一个 IMAP 文件夹中
- [git-p4[1]](https://git-scm.com/docs/git-p4)

  Import from and submit to Perforce repositories

  从 Perforce 仓库导入并提交到 Perforce 仓库
- [git-quiltimport[1]](https://git-scm.com/docs/git-quiltimport)

  Applies a quilt patchset onto the current branch

  将一个被子补丁集应用到当前分支上
- [git-request-pull[1]](https://git-scm.com/docs/git-request-pull)

  Generates a summary of pending changes

  生成一份待处理修改的摘要
- [git-send-email[1]](https://git-scm.com/docs/git-send-email)

  Send a collection of patches as emails

  以电子邮件的形式发送补丁集
- [git-svn[1]](https://git-scm.com/docs/git-svn)

  Bidirectional operation between a Subversion repository and Git

  在 Subversion 仓库和 Git 之间的双向操作

### Reset, restore and revert 重置、恢复和还原

There are three commands with similar names: `git reset`, `git restore` and `git revert`.

有三个名字相似的命令：git reset、git restore 和 git revert。

- [git-revert[1]](https://git-scm.com/docs/git-revert) is about making a new commit that reverts the changes made by other commits. git-revert[1] 是指做一个新的提交，恢复其他提交的改动。
- [git-restore[1]](https://git-scm.com/docs/git-restore) is about restoring files in the working tree from either the index or another commit. This command does not update your branch. The command can also be used to restore files in the index from another commit. git-restore[1] 是指从索引或其他提交中恢复工作树上的文件。这个命令不会更新您的分支。该命令也可以用来从其他提交中恢复索引中的文件。
- [git-reset[1]](https://git-scm.com/docs/git-reset) is about updating your branch, moving the tip in order to add or remove commits from the branch. This operation changes the commit history. git-reset[1] 是为了更新你的分支，移动提示，以便从分支中添加或删除提交。这个操作会改变提交历史。

  `git reset` can also be used to restore the index, overlapping with `git restore`.

  git reset 也可以用来恢复索引，与 git restore 重叠。

## Low-level commands (plumbing) 低级别的命令（plumbing）

Although Git includes its own porcelain layer, its low-level commands are sufficient to support development of alternative porcelains. Developers of such porcelains might start by reading about [git-update-index[1]](https://git-scm.com/docs/git-update-index) and [git-read-tree[1]](https://git-scm.com/docs/git-read-tree).

尽管 Git 包含了自己的瓷器层，但它的低级命令足以支持其他瓷器的开发。此类瓷器的开发者可以从阅读 git-update-index[1] 和 git-read-tree[1] 开始。

The interface (input, output, set of options and the semantics) to these low-level commands are meant to be a lot more stable than Porcelain level commands, because these commands are primarily for scripted use. The interface to Porcelain commands on the other hand are subject to change in order to improve the end user experience.

这些低级命令的接口（输入、输出、选项集和语义）要比 Porcelain 级的命令稳定得多，因为这些命令主要是用于脚本的。另一方面，为了改善终端用户的体验，Porcelain命令的接口是可以改变的。

The following description divides the low-level commands into commands that manipulate objects (in the repository, index, and working tree), commands that interrogate and compare objects, and commands that move objects and references between repositories.

下面的描述将低级命令分为操纵对象的命令（在版本库、索引和工作树中）、询问和比较对象的命令，以及在版本库之间移动对象和引用的命令。

### Manipulation commands 操纵命令

- [git-apply[1]](https://git-scm.com/docs/git-apply)

  Apply a patch to files and/or to the index

  给文件和/或索引打上一个补丁
- [git-checkout-index[1]](https://git-scm.com/docs/git-checkout-index)

  Copy files from the index to the working tree

  将索引中的文件复制到工作树中
- [git-commit-graph[1]](https://git-scm.com/docs/git-commit-graph)

  Write and verify Git commit-graph files

  编写并验证 Git 提交图文件
- [git-commit-tree[1]](https://git-scm.com/docs/git-commit-tree)

  Create a new commit object

  创建一个新的提交对象
- [git-hash-object[1]](https://git-scm.com/docs/git-hash-object)

  Compute object ID and optionally creates a blob from a file

  计算对象的 ID，也可以选择从文件中创建一个 blob
- [git-index-pack[1]](https://git-scm.com/docs/git-index-pack)

  Build pack index file for an existing packed archive

  为现有的打包档案建立打包索引文件
- [git-merge-file[1]](https://git-scm.com/docs/git-merge-file)

  Run a three-way file merge

  运行一个三方的文件合并
- [git-merge-index[1]](https://git-scm.com/docs/git-merge-index)

  Run a merge for files needing merging

  对需要合并的文件进行合并
- [git-mktag[1]](https://git-scm.com/docs/git-mktag)

  Creates a tag object with extra validation

  创建一个具有额外验证功能的标签对象
- [git-mktree[1]](https://git-scm.com/docs/git-mktree)

  Build a tree-object from ls-tree formatted text

  从ls-tree格式的文本中建立一个树形对象
- [git-multi-pack-index[1]](https://git-scm.com/docs/git-multi-pack-index)

  Write and verify multi-pack-indexes

  编写并验证多包索引
- [git-pack-objects[1]](https://git-scm.com/docs/git-pack-objects)

  Create a packed archive of objects

  创建一个对象的打包档案
- [git-prune-packed[1]](https://git-scm.com/docs/git-prune-packed)

  Remove extra objects that are already in pack files

  删除已经在打包文件中的额外对象
- [git-read-tree[1]](https://git-scm.com/docs/git-read-tree)

  Reads tree information into the index

  将树的信息读入索引
- [git-symbolic-ref[1]](https://git-scm.com/docs/git-symbolic-ref)

  Read, modify and delete symbolic refs

  读取、修改和删除符号参考文献
- [git-unpack-objects[1]](https://git-scm.com/docs/git-unpack-objects)

  Unpack objects from a packed archive

  从打包的档案中解压对象
- [git-update-index[1]](https://git-scm.com/docs/git-update-index)

  Register file contents in the working tree to the index

  将工作树中的文件内容登记到索引中
- [git-update-ref[1]](https://git-scm.com/docs/git-update-ref)

  Update the object name stored in a ref safely

  安全地更新存储在 ref 中的对象名称
- [git-write-tree[1]](https://git-scm.com/docs/git-write-tree)

  Create a tree object from the current index

  从当前索引中创建一个树状对象

### Interrogation commands 讯问命令

- [git-cat-file[1]](https://git-scm.com/docs/git-cat-file)

  Provide content or type and size information for repository objects

  为存储库对象提供内容或类型和大小信息
- [git-cherry[1]](https://git-scm.com/docs/git-cherry)

  Find commits yet to be applied to upstream

  查找尚未应用到上游的提交内容
- [git-diff-files[1]](https://git-scm.com/docs/git-diff-files)

  Compares files in the working tree and the index

  比较工作树和索引中的文件
- [git-diff-index[1]](https://git-scm.com/docs/git-diff-index)

  Compare a tree to the working tree or index

  将一棵树与工作树或索引进行比较
- [git-diff-tree[1]](https://git-scm.com/docs/git-diff-tree)

  Compares the content and mode of blobs found via two tree objects

  比较通过两个树对象找到的 blobs 的内容和模式
- [git-for-each-ref[1]](https://git-scm.com/docs/git-for-each-ref)

  Output information on each ref

  输出每个参考文献的信息
- [git-for-each-repo[1]](https://git-scm.com/docs/git-for-each-repo)

  Run a Git command on a list of repositories

  在一个仓库列表上运行一个 Git 命令
- [git-get-tar-commit-id[1]](https://git-scm.com/docs/git-get-tar-commit-id)

  Extract commit ID from an archive created using git-archive

  从使用 git-archive 创建的档案中提取提交 ID
- [git-ls-files[1]](https://git-scm.com/docs/git-ls-files)

  Show information about files in the index and the working tree

  显示索引和工作树中的文件信息
- [git-ls-remote[1]](https://git-scm.com/docs/git-ls-remote)

  List references in a remote repository

  列出远程版本库中的引用
- [git-ls-tree[1]](https://git-scm.com/docs/git-ls-tree)

  List the contents of a tree object

  列出一个树对象的内容
- [git-merge-base[1]](https://git-scm.com/docs/git-merge-base)

  Find as good common ancestors as possible for a merge

  为合并找到尽可能好的共同祖先
- [git-name-rev[1]](https://git-scm.com/docs/git-name-rev)

  Find symbolic names for given revs

  为给定的修订版寻找符号化的名字
- [git-pack-redundant[1]](https://git-scm.com/docs/git-pack-redundant)

  Find redundant pack files

  查找多余的打包文件
- [git-rev-list[1]](https://git-scm.com/docs/git-rev-list)

  Lists commit objects in reverse chronological order

  按时间顺序反向排列提交对象
- [git-rev-parse[1]](https://git-scm.com/docs/git-rev-parse)

  Pick out and massage parameters

  挑出并按摩参数
- [git-show-index[1]](https://git-scm.com/docs/git-show-index)

  Show packed archive index

  显示打包的档案索引
- [git-show-ref[1]](https://git-scm.com/docs/git-show-ref)

  List references in a local repository

  列出本地版本库中的引用
- [git-unpack-file[1]](https://git-scm.com/docs/git-unpack-file)

  Creates a temporary file with a blob’s contents

  用blob的内容创建一个临时文件
- [git-var[1]](https://git-scm.com/docs/git-var)

  Show a Git logical variable

  显示一个 Git 逻辑变量
- [git-verify-pack[1]](https://git-scm.com/docs/git-verify-pack)

  Validate packed Git archive files

  验证打包的Git归档文件

In general, the interrogate commands do not touch the files in the working tree.

一般来说，审讯命令不会触及工作树上的文件。

### Syncing repositories 同步存储库

- [git-daemon[1]](https://git-scm.com/docs/git-daemon)

  A really simple server for Git repositories

  一个真正简单的 Git 仓库的服务器
- [git-fetch-pack[1]](https://git-scm.com/docs/git-fetch-pack)

  Receive missing objects from another repository

  从另一个仓库接收丢失的对象
- [git-http-backend[1]](https://git-scm.com/docs/git-http-backend)

  Server side implementation of Git over HTTP

  通过 HTTP 实现 Git 的服务器端
- [git-send-pack[1]](https://git-scm.com/docs/git-send-pack)

  Push objects over Git protocol to another repository

  通过 Git 协议推送对象到另一个仓库
- [git-update-server-info[1]](https://git-scm.com/docs/git-update-server-info)

  Update auxiliary info file to help dumb servers

  更新辅助信息文件以帮助哑巴服务器

The following are helper commands used by the above; end users typically do not use them directly.

以下是上述命令使用的辅助命令；终端用户通常不直接使用它们。

- [git-http-fetch[1]](https://git-scm.com/docs/git-http-fetch)

  Download from a remote Git repository via HTTP

  通过 HTTP 从远程 Git 仓库下载文件
- [git-http-push[1]](https://git-scm.com/docs/git-http-push)

  Push objects over HTTP/DAV to another repository

  通过 HTTP/DAV 推送对象到另一个仓库
- [git-receive-pack[1]](https://git-scm.com/docs/git-receive-pack)

  Receive what is pushed into the repository

  接收被推送到仓库的内容
- [git-shell[1]](https://git-scm.com/docs/git-shell)

  Restricted login shell for Git-only SSH access

  限制登录的 shell，只用于 Git 的 SSH 访问
- [git-upload-archive[1]](https://git-scm.com/docs/git-upload-archive)

  Send archive back to git-archive

  将归档文件送回 git-archive
- [git-upload-pack[1]](https://git-scm.com/docs/git-upload-pack)

  Send objects packed back to git-fetch-pack

  将打包的对象送回 git-fetch-pack

### Internal helper commands 内部辅助命令

These are internal helper commands used by other commands; end users typically do not use them directly.

这些是其他命令使用的内部辅助命令；终端用户通常不会直接使用它们。

- [git-check-attr[1]](https://git-scm.com/docs/git-check-attr)

  Display gitattributes information

  显示 gitattributes 信息
- [git-check-ignore[1]](https://git-scm.com/docs/git-check-ignore)

  Debug gitignore / exclude files

  调试 gitignore / 排除文件
- [git-check-mailmap[1]](https://git-scm.com/docs/git-check-mailmap)

  Show canonical names and email addresses of contacts

  显示联系人的规范名称和电子邮件地址
- [git-check-ref-format[1]](https://git-scm.com/docs/git-check-ref-format)

  Ensures that a reference name is well formed

  确保参考文献的名称格式良好
- [git-column[1]](https://git-scm.com/docs/git-column)

  Display data in columns

  在列中显示数据
- [git-credential[1]](https://git-scm.com/docs/git-credential)

  Retrieve and store user credentials

  检索并存储用户凭证
- [git-credential-cache[1]](https://git-scm.com/docs/git-credential-cache)

  Helper to temporarily store passwords in memory

  用于在内存中临时存储密码的辅助工具
- [git-credential-store[1]](https://git-scm.com/docs/git-credential-store)

  Helper to store credentials on disk

  在磁盘上存储凭证的辅助工具
- [git-fmt-merge-msg[1]](https://git-scm.com/docs/git-fmt-merge-msg)

  Produce a merge commit message

  产生一个合并提交信息
- [git-hook[1]](https://git-scm.com/docs/git-hook)

  Run git hooks

  运行 git 钩子
- [git-interpret-trailers[1]](https://git-scm.com/docs/git-interpret-trailers)

  Add or parse structured information in commit messages

  添加或解析提交消息中的结构化信息
- [git-mailinfo[1]](https://git-scm.com/docs/git-mailinfo)

  Extracts patch and authorship from a single e-mail message

  从单个邮件信息中提取补丁和作者信息
- [git-mailsplit[1]](https://git-scm.com/docs/git-mailsplit)

  Simple UNIX mbox splitter program

  简单的 UNIX mbox 分割器程序
- [git-merge-one-file[1]](https://git-scm.com/docs/git-merge-one-file)

  The standard helper program to use with git-merge-index

  与 git-merge-index 一起使用的标准辅助程序
- [git-patch-id[1]](https://git-scm.com/docs/git-patch-id)

  Compute unique ID for a patch

  计算一个补丁的唯一 ID
- [git-sh-i18n[1]](https://git-scm.com/docs/git-sh-i18n)

  Git’s i18n setup code for shell scripts

  用于 shell 脚本的 Git 的 i18n 设置代码
- [git-sh-setup[1]](https://git-scm.com/docs/git-sh-setup)

  Common Git shell script setup code

  常见的 Git shell 脚本设置代码
- [git-stripspace[1]](https://git-scm.com/docs/git-stripspace)

  Remove unnecessary whitespace

  删除不必要的空白

## 指南 Guides 

The following documentation pages are guides about Git concepts.

以下文档页面是关于 Git 概念的指南。

- [gitcore-tutorial[7]](https://git-scm.com/docs/gitcore-tutorial)

  A Git core tutorial for developers

  面向开发者的 Git 核心教程
- [gitcredentials[7]](https://git-scm.com/docs/gitcredentials)

  Providing usernames and passwords to Git

  为 Git 提供用户名和密码
- [gitcvs-migration[7]](https://git-scm.com/docs/gitcvs-migration)

  Git for CVS users

  为CVS用户提供Git
- [gitdiffcore[7]](https://git-scm.com/docs/gitdiffcore)

  Tweaking diff output

  调整差异输出
- [giteveryday[7]](https://git-scm.com/docs/giteveryday)

  A useful minimum set of commands for Everyday Git

  一套有用的日常 Git 的最低命令
- [gitfaq[7]](https://git-scm.com/docs/gitfaq)

  Frequently asked questions about using Git

  关于使用 Git 的常见问题
- [gitglossary[7]](https://git-scm.com/docs/gitglossary)

  A Git Glossary

  一个Git词汇表
- [gitnamespaces[7]](https://git-scm.com/docs/gitnamespaces)

  Git namespaces

  Git 命名空间
- [gitremote-helpers[7]](https://git-scm.com/docs/gitremote-helpers)

  Helper programs to interact with remote repositories

  与远程仓库互动的辅助程序
- [gitsubmodules[7]](https://git-scm.com/docs/gitsubmodules)

  Mounting one repository inside another

  将一个仓库安装到另一个仓库中
- [gittutorial[7]](https://git-scm.com/docs/gittutorial)

  A tutorial introduction to Git

  一个关于 Git 的教程介绍
- [gittutorial-2[7]](https://git-scm.com/docs/gittutorial-2)

  A tutorial introduction to Git: part two

  Git的教程介绍：第二部分
- [gitworkflows[7]](https://git-scm.com/docs/gitworkflows)

  An overview of recommended workflows with Git

  推荐使用 Git 的工作流程概览

## Repository, command and file interfaces 仓库、命令和文件界面

This documentation discusses repository and command interfaces which users are expected to interact with directly. See `--user-formats` in [git-help[1]](https://git-scm.com/docs/git-help) for more details on the criteria.

本文档讨论的是用户应该直接与之互动的仓库和命令界面。参见 git-help[1] 中的 --user-formats 以了解更多标准的细节。

- [gitattributes[5]](https://git-scm.com/docs/gitattributes)

  Defining attributes per path

  定义每个路径的属性
- [gitcli[7]](https://git-scm.com/docs/gitcli)

  Git command-line interface and conventions

  Git 命令行界面和惯例
- [githooks[5]](https://git-scm.com/docs/githooks)

  Hooks used by Git

  Git 使用的钩子
- [gitignore[5]](https://git-scm.com/docs/gitignore)

  Specifies intentionally untracked files to ignore

  指定故意忽略的未被追踪的文件
- [gitmailmap[5]](https://git-scm.com/docs/gitmailmap)

  Map author/committer names and/or E-Mail addresses

  映射作者/committer 的名字和/或 E-Mail 地址
- [gitmodules[5]](https://git-scm.com/docs/gitmodules)

  Defining submodule properties

  定义子模块的属性
- [gitrepository-layout[5]](https://git-scm.com/docs/gitrepository-layout)

  Git Repository Layout

  Git 仓库的布局
- [gitrevisions[7]](https://git-scm.com/docs/gitrevisions)

  Specifying revisions and ranges for Git

  为Git指定修订版和范围

## File formats, protocols and other developer interfaces 文件格式、协议和其他开发者接口

This documentation discusses file formats, over-the-wire protocols and other git developer interfaces. See `--developer-interfaces` in [git-help[1]](https://git-scm.com/docs/git-help).

本文档讨论了文件格式、线外协议和其他git开发者接口。参见 git-help[1] 中的 -- 开发者接口。

- [gitformat-bundle[5]](https://git-scm.com/docs/gitformat-bundle)

  The bundle file format

  捆绑文件格式
- [gitformat-chunk[5]](https://git-scm.com/docs/gitformat-chunk)

  Chunk-based file formats

  基于 Chunk 的文件格式
- [gitformat-commit-graph[5]](https://git-scm.com/docs/gitformat-commit-graph)

  Git commit-graph format

  Git 提交图的格式
- [gitformat-index[5]](https://git-scm.com/docs/gitformat-index)

  Git index format

  Git 索引格式
- [gitformat-pack[5]](https://git-scm.com/docs/gitformat-pack)

  Git pack format

  Git 数据包格式
- [gitformat-signature[5]](https://git-scm.com/docs/gitformat-signature)

  Git cryptographic signature formats

  Git 的加密签名格式
- [gitprotocol-capabilities[5]](https://git-scm.com/docs/gitprotocol-capabilities)

  Protocol v0 and v1 capabilities

  协议 v0 和 v1 的能力
- [gitprotocol-common[5]](https://git-scm.com/docs/gitprotocol-common)

  Things common to various protocols

  各种协议的共通之处
- [gitprotocol-http[5]](https://git-scm.com/docs/gitprotocol-http)

  Git HTTP-based protocols

  基于 Git HTTP 的协议
- [gitprotocol-pack[5]](https://git-scm.com/docs/gitprotocol-pack)

  How packs are transferred over-the-wire

  包是如何通过网络传输的
- [gitprotocol-v2[5]](https://git-scm.com/docs/gitprotocol-v2)

  Git Wire Protocol, Version 2

  Git 线上协议，第二版

## 配置 Mechanism 配置机制

Git uses a simple text format to store customizations that are per repository and are per user. Such a configuration file may look like this:

Git 使用一种简单的文本格式来存储每个仓库和每个用户的自定义配置。这样的配置文件可能看起来像这样。

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

Various commands read from the configuration file and adjust their operation accordingly. See [git-config[1]](https://git-scm.com/docs/git-config) for a list and more details about the configuration mechanism.

各种命令从配置文件中读取并相应地调整其操作。关于配置机制的列表和更多细节，请参见 git-config[1]。

## Identifier Terminology 标识符术语

- <object>

  Indicates the object name for any type of object.

  表示任何类型的对象的名称。
- <blob>

  Indicates a blob object name.

  表示一个 blob 对象的名称。
- <tree>

  Indicates a tree object name.

  表示一个树形对象的名称。
- <commit>

  Indicates a commit object name.

  表示一个提交对象的名称。
- <tree-ish>

  Indicates a tree, commit or tag object name. A command that takes a `<tree-ish>` argument ultimately wants to operate on a `<tree>` object but automatically dereferences `<commit>` and `<tag>` objects that point at a `<tree>`.

  表示一个树、提交或标签对象的名称。一个接受`<tree-ish>`参数的命令最终要对`<tree>`对象进行操作，但会自动解除对指向`<tree>`的`<commit>`和`<tag>`对象的索引。
- <commit-ish>

  Indicates a commit or tag object name. A command that takes a `<commit-ish>` argument ultimately wants to operate on a `<commit>` object but automatically dereferences `<tag>` objects that point at a `<commit>`.

  表示一个提交或标签对象的名称。一个接受`<commit-ish>`参数的命令最终要对`<commit>`对象进行操作，但会自动解除对指向`<commit>`的`<tag>`对象的引用。
- <type>

  Indicates that an object type is required. Currently one of: `blob`, `tree`, `commit`, or `tag`.

  表示需要一个对象类型。目前是以下之一：blob、树、提交或标签。
- <file>

  Indicates a filename - almost always relative to the root of the tree structure `GIT_INDEX_FILE` describes.

  表示一个文件名--几乎总是相对于GIT_INDEX_FILE描述的树状结构的根。

## Symbolic Identifiers 符号标识符

Any Git command accepting any `<object>` can also use the following symbolic notation:

任何接受任何`<object>`的Git命令也可以使用以下符号标识。

- HEAD

  indicates the head of the current branch.

  表示当前分支的头部。
- <tag>

  a valid tag *name* (i.e. a `refs/tags/<tag>` reference).

  一个有效的标签名称（即一个refs/tags/`<tag>`的引用）。
- <head>

  a valid head *name* (i.e. a `refs/heads/<head>` reference).

  一个有效的头部名称（即对 refs/heads/`<head>` 的引用）。

For a more complete list of ways to spell object names, see "SPECIFYING REVISIONS" section in [gitrevisions[7]](https://git-scm.com/docs/gitrevisions).

更完整的对象名称拼写方式列表，请参见 gitrevisions[7]中的 "SPECIFYING REVISIONS "部分。

## File/Directory Structure 文件/目录结构

Please see the [gitrepository-layout[5]](https://git-scm.com/docs/gitrepository-layout) document.

请参阅 gitrepository-layout[5] 文档。

Read [githooks[5]](https://git-scm.com/docs/githooks) for more details about each hook.

阅读 githooks[5] 以了解每个钩子的更多细节。

Higher level SCMs may provide and manage additional information in the `$GIT_DIR`.

更高级别的 SCM 可以提供并管理 $GIT_DIR 中的额外信息。

## Terminology 术语

Please see [gitglossary[7]](https://git-scm.com/docs/gitglossary).

请参阅 gitglossary[7]。

## Environment Variables 环境变量

Various Git commands pay attention to environment variables and change their behavior. The environment variables marked as "Boolean" take their values the same way as Boolean valued configuration variables, e.g. "true", "yes", "on" and positive numbers are taken as "yes".

各种 Git 命令都会关注环境变量并改变它们的行为。标记为 "布尔 "的环境变量的取值方式与布尔值配置变量相同，例如，"true"、"yes"、"on "和正数被视为 "yes"。

Here are the variables:

以下是这些变量。

### The Git Repository Git 仓库

These environment variables apply to *all* core Git commands. Nb: it is worth noting that they may be used/overridden by SCMS sitting above Git so take care if using a foreign front-end.

这些环境变量适用于所有核心的 Git 命令。注：值得注意的是，这些变量可能会被位于Git之上的SCMS使用/覆盖，所以如果使用国外的前端，请注意。

- `GIT_INDEX_FILE`

  This environment variable specifies an alternate index file. If not specified, the default of `$GIT_DIR/index` is used.

  这个环境变量指定了一个备用的索引文件。如果没有指定，则使用默认的$GIT_DIR/index。
- `GIT_INDEX_VERSION`

  This environment variable specifies what index version is used when writing the index file out. It won’t affect existing index files. By default index file version 2 or 3 is used. See [git-update-index[1]](https://git-scm.com/docs/git-update-index) for more information.

  这个环境变量指定了在写出索引文件时使用的索引版本。它不会影响现有的索引文件。默认情况下，索引文件的版本是2或3。参见 git-slate-index[1] 以获得更多信息。
- `GIT_OBJECT_DIRECTORY`

  If the object storage directory is specified via this environment variable then the sha1 directories are created underneath - otherwise the default `$GIT_DIR/objects` directory is used.

  如果通过这个环境变量指定了对象存储目录，那么 sha1 目录就会在其下创建 -- 否则就使用默认的 $GIT_DIR/objects 目录。
- `GIT_ALTERNATE_OBJECT_DIRECTORIES`

  Due to the immutable nature of Git objects, old objects can be archived into shared, read-only directories. This variable specifies a ":" separated (on Windows ";" separated) list of Git object directories which can be used to search for Git objects. New objects will not be written to these directories.Entries that begin with `"` (double-quote) will be interpreted as C-style quoted paths, removing leading and trailing double-quotes and respecting backslash escapes. E.g., the value `"path-with-\"-and-:-in-it":vanilla-path` has two paths: `path-with-"-and-:-in-it` and `vanilla-path`.

  由于Git对象的不可改变性，旧的对象可以被归档到共享的、只读的目录里。这个变量指定了一个": "分隔（在Windows下为"; "分隔）的Git对象目录列表，可以用来搜索Git对象。新的对象将不会被写入这些目录中。

  以""（双引号）开头的条目将被解释为C风格的引号路径，去掉前面和后面的双引号，并尊重反斜杠转义。例如，"path-with-"-and-:-in-it":vanilla-path的值有两个路径：path-with-"-and-:-in-it和vanilla-path。
- `GIT_DIR`

  If the `GIT_DIR` environment variable is set then it specifies a path to use instead of the default `.git` for the base of the repository. The `--git-dir` command-line option also sets this value.

  如果GIT_DIR环境变量被设置，那么它指定了一个路径来代替默认的.git作为版本库的基础。--git-dir 命令行选项也会设置这个值。
- `GIT_WORK_TREE`

  Set the path to the root of the working tree. This can also be controlled by the `--work-tree` command-line option and the core.worktree configuration variable.

  设置到工作树根部的路径。这也可以由 --work-tree 命令行选项和 core.worktree 配置变量控制。
- `GIT_NAMESPACE`

  Set the Git namespace; see [gitnamespaces[7]](https://git-scm.com/docs/gitnamespaces) for details. The `--namespace` command-line option also sets this value.

  设置 Git 命名空间；详见 gitnamespaces[7]。--namespace 命令行选项也会设置此值。
- `GIT_CEILING_DIRECTORIES`

  This should be a colon-separated list of absolute paths. If set, it is a list of directories that Git should not chdir up into while looking for a repository directory (useful for excluding slow-loading network directories). It will not exclude the current working directory or a GIT_DIR set on the command line or in the environment. Normally, Git has to read the entries in this list and resolve any symlink that might be present in order to compare them with the current directory. However, if even this access is slow, you can add an empty entry to the list to tell Git that the subsequent entries are not symlinks and needn’t be resolved; e.g., `GIT_CEILING_DIRECTORIES=/maybe/symlink::/very/slow/non/symlink`.

  这应该是一个用冒号隔开的绝对路径列表。如果设置了，它是一个目录列表，在寻找仓库目录时，Git不应该向上chdir（对于排除缓慢加载的网络目录很有用）。它不会排除当前工作目录或在命令行或环境中设置的GIT_DIR。通常情况下，Git 需要读取这个列表中的条目，并解决任何可能存在的符号链接，以便将它们与当前目录进行比较。然而，如果连这个访问都很慢，你可以在列表中添加一个空条目，告诉Git后面的条目不是符号链接，不需要解析；例如，GIT_CEILING_DIRECTORIES=/maybe/symlink::/very/slow/non/symlink。
- `GIT_DISCOVERY_ACROSS_FILESYSTEM`

  When run in a directory that does not have ".git" repository directory, Git tries to find such a directory in the parent directories to find the top of the working tree, but by default it does not cross filesystem boundaries. This Boolean environment variable can be set to true to tell Git not to stop at filesystem boundaries. Like `GIT_CEILING_DIRECTORIES`, this will not affect an explicit repository directory set via `GIT_DIR` or on the command line.

  当在一个没有".git "仓库目录的目录下运行时，Git 会尝试在父目录中找到这样的目录来寻找工作树的顶端，但默认情况下它不会跨越文件系统的界限。这个布尔环境变量可以被设置为 "true"，以告诉Git不要在文件系统边界处停止。和GIT_CEILING_DIRECTORIES一样，这不会影响通过GIT_DIR或命令行设置的明确的仓库目录。
- `GIT_COMMON_DIR`

  If this variable is set to a path, non-worktree files that are normally in $GIT_DIR will be taken from this path instead. Worktree-specific files such as HEAD or index are taken from $GIT_DIR. See [gitrepository-layout[5]](https://git-scm.com/docs/gitrepository-layout) and [git-worktree[1]](https://git-scm.com/docs/git-worktree) for details. This variable has lower precedence than other path variables such as GIT_INDEX_FILE, GIT_OBJECT_DIRECTORY…

  如果这个变量被设置为一个路径，通常在 $GIT_DIR 中的非工作树文件将被从这个路径中获取。工作树特定的文件，如 HEAD 或 index，则从 $GIT_DIR 取出。详见 gitrepository-layout[5] 和 git-worktree[1]。这个变量的优先级低于其他路径变量，例如 GIT_INDEX_FILE, GIT_OBJECT_DIRECTORY...
- `GIT_DEFAULT_HASH`

  If this variable is set, the default hash algorithm for new repositories will be set to this value. This value is currently ignored when cloning; the setting of the remote repository is used instead. The default is "sha1". THIS VARIABLE IS EXPERIMENTAL! See `--object-format` in [git-init[1]](https://git-scm.com/docs/git-init).

  如果这个变量被设置，新仓库的默认哈希算法将被设置为这个值。目前这个值在克隆时被忽略，而是使用远程版本库的设置。默认是 "sha1"。这个变量是实验性的! 参见 git-init[1] 中的 --object-format。

### Git Commits -  Git 提交

- `GIT_AUTHOR_NAME`

  The human-readable name used in the author identity when creating commit or tag objects, or when writing reflogs. Overrides the `user.name` and `author.name` configuration settings.

  在创建提交或标签对象时，或在编写 reflogs 时，在作者身份中使用的人类可读的名字。覆盖 user.name 和 author.name 的配置设置。
- `GIT_AUTHOR_EMAIL`

  The email address used in the author identity when creating commit or tag objects, or when writing reflogs. Overrides the `user.email` and `author.email` configuration settings.

  在创建提交或标签对象时，或在编写relogs时，作者身份中使用的电子邮件地址。覆盖user.email和author.email的配置设置。
- `GIT_AUTHOR_DATE`

  The date used for the author identity when creating commit or tag objects, or when writing reflogs. See [git-commit[1]](https://git-scm.com/docs/git-commit) for valid formats.

  在创建提交或标签对象时，或在编写 reflogs 时，作者身份所使用的日期。有效格式见 git-commit[1]。
- `GIT_COMMITTER_NAME`

  The human-readable name used in the committer identity when creating commit or tag objects, or when writing reflogs. Overrides the `user.name` and `committer.name` configuration settings.

  在创建提交或标签对象时，或在编写 reflogs 时，提交者身份所使用的可读名称。覆盖 user.name 和 committer.name 的配置设置。
- `GIT_COMMITTER_EMAIL`

  The email address used in the author identity when creating commit or tag objects, or when writing reflogs. Overrides the `user.email` and `committer.email` configuration settings.

  在创建提交或标签对象时，或在编写relogs时，作者身份所使用的电子邮件地址。覆盖user.email和committer.email的配置设置。
- `GIT_COMMITTER_DATE`

  The date used for the committer identity when creating commit or tag objects, or when writing reflogs. See [git-commit[1]](https://git-scm.com/docs/git-commit) for valid formats.

  在创建提交或标签对象时，或在撰写 reflog 时，用于提交者身份的日期。有效格式见 git-commit[1]。
- `EMAIL`

  The email address used in the author and committer identities if no other relevant environment variable or configuration setting has been set.

  如果没有设置其他相关的环境变量或配置，作者和提交者身份所使用的电子邮件地址。

### Git Diffs - Git 分割

- `GIT_DIFF_OPTS`

  Only valid setting is "--unified=??" or "-u??" to set the number of context lines shown when a unified diff is created. This takes precedence over any "-U" or "--unified" option value passed on the Git diff command line.

  唯一有效的设置是"--unified=?? "或"-u??"，用于设置创建统一差异时显示的上下文行数。这优先于 Git diff 命令行中传递的任何 "-U" 或 "--unified" 选项值。
- `GIT_EXTERNAL_DIFF`

  When the environment variable `GIT_EXTERNAL_DIFF` is set, the program named by it is called to generate diffs, and Git does not use its builtin diff machinery. For a path that is added, removed, or modified, `GIT_EXTERNAL_DIFF` is called with 7 parameters:`path old-file old-hex old-mode new-file new-hex new-mode`where:

  当环境变量GIT_EXTERNAL_DIFF被设置时，由它命名的程序被调用来生成差异，而Git不使用其内置的差异机制。对于一个被添加、删除或修改的路径，GIT_EXTERNAL_DIFF会被调用，有7个参数：path old-file old-hex old-mode new-file new-hex new-mode
  其中。
- <old|new>-file

  are files GIT_EXTERNAL_DIFF can use to read the contents of <old|new>,

  是GIT_EXTERNAL_DIFF可以用来读取<old|new>内容的文件。
- <old|new>-hex

  are the 40-hexdigit SHA-1 hashes,

  是40个六位数的SHA-1哈希值。
- <old|new>-mode

  are the octal representation of the file modes.The file parameters can point at the user’s working file (e.g. `new-file` in "git-diff-files"), `/dev/null` (e.g. `old-file` when a new file is added), or a temporary file (e.g. `old-file` in the index). `GIT_EXTERNAL_DIFF` should not worry about unlinking the temporary file --- it is removed when `GIT_EXTERNAL_DIFF` exits.For a path that is unmerged, `GIT_EXTERNAL_DIFF` is called with 1 parameter, `<path>`.For each path `GIT_EXTERNAL_DIFF` is called, two environment variables, `GIT_DIFF_PATH_COUNTER` and `GIT_DIFF_PATH_TOTAL` are set.

  是文件模式的八进制表示。

  文件参数可以指向用户的工作文件（例如 "git-diff-files "中的new-file）、/dev/null（例如添加新文件时的old-file）、或一个临时文件（例如索引中的old-file）。GIT_EXTERNAL_DIFF不应该担心解除临时文件的链接------当GIT_EXTERNAL_DIFF退出时，它会被删除。 对于一个未合并的路径，

  GIT_EXTERNAL_DIFF被调用，有一个参数，`<path>`。

  对于每个被调用的 GIT_EXTERNAL_DIFF 路径，两个环境变量 GIT_DIFF_PATH_COUNTER 和 GIT_DIFF_PATH_TOTAL 被设置。
- `GIT_DIFF_PATH_COUNTER`

  A 1-based counter incremented by one for every path.

  一个基于1的计数器，每条路径递增1。
- `GIT_DIFF_PATH_TOTAL`

  The total number of paths.

  路径的总数。

### other 其他

- `GIT_MERGE_VERBOSITY`

  A number controlling the amount of output shown by the recursive merge strategy. Overrides merge.verbosity. See [git-merge[1]](https://git-scm.com/docs/git-merge)

  一个控制递归合并策略所显示的输出量的数字。覆盖merge.verbosity。参见 git-merge[1] 。
- `GIT_PAGER`

  This environment variable overrides `$PAGER`. If it is set to an empty string or to the value "cat", Git will not launch a pager. See also the `core.pager` option in [git-config[1]](https://git-scm.com/docs/git-config).

  这个环境变量覆盖了 $PAGER。如果它被设置为空字符串或值 "cat"，Git 将不会启动一个寻呼机。也请参见 git-config[1] 中的 core.pager 选项。
- `GIT_PROGRESS_DELAY`

  A number controlling how many seconds to delay before showing optional progress indicators. Defaults to 2.

  一个数字，控制在显示可选的进度指示器之前要延迟多少秒。默认为 2。
- `GIT_EDITOR`

  This environment variable overrides `$EDITOR` and `$VISUAL`. It is used by several Git commands when, on interactive mode, an editor is to be launched. See also [git-var[1]](https://git-scm.com/docs/git-var) and the `core.editor` option in [git-config[1]](https://git-scm.com/docs/git-config).

  这个环境变量覆盖了 $EDITOR 和 $VISUAL。在交互式模式下，当要启动编辑器时，它被几个Git命令使用。参见 git-var[1] 和 git-config[1] 中的 core.editor 选项。
- `GIT_SEQUENCE_EDITOR`

  This environment variable overrides the configured Git editor when editing the todo list of an interactive rebase. See also [git-rebase[1]](https://git-scm.com/docs/git-rebase) and the `sequence.editor` option in [git-config[1]](https://git-scm.com/docs/git-config).

  在编辑交互式 rebase 的 todo 列表时，这个环境变量会覆盖配置的 Git 编辑器。参见 git-rebase[1] 和 git-config[1] 中的 sequence.editor 选项。
- `GIT_SSH`
- `GIT_SSH_COMMAND`

  If either of these environment variables is set then *git fetch* and *git push* will use the specified command instead of *ssh* when they need to connect to a remote system. The command-line parameters passed to the configured command are determined by the ssh variant. See `ssh.variant` option in [git-config[1]](https://git-scm.com/docs/git-config) for details.`$GIT_SSH_COMMAND` takes precedence over `$GIT_SSH`, and is interpreted by the shell, which allows additional arguments to be included. `$GIT_SSH` on the other hand must be just the path to a program (which can be a wrapper shell script, if additional arguments are needed).Usually it is easier to configure any desired options through your personal `.ssh/config` file. Please consult your ssh documentation for further details.

  如果这两个环境变量中的任何一个被设置了，那么当 git fetch 和 git push 需要连接到远程系统时，将使用指定的命令而不是 ssh。传递给配置的命令的命令行参数是由ssh变量决定的。详情见 git-config[1] 中的 ssh.variant 选项。

  $GIT_SSH_COMMAND 优先于 $GIT_SSH，并由 shell 解释，它允许包含额外的参数。另一方面，$GIT_SSH必须只是一个程序的路径（如果需要额外的参数，它可以是一个封装的shell脚本）。

  通常情况下，通过你的个人.ssh/config文件来配置任何需要的选项会更容易。请查阅你的ssh文档以了解更多细节。
- `GIT_SSH_VARIANT`

  If this environment variable is set, it overrides Git’s autodetection whether `GIT_SSH`/`GIT_SSH_COMMAND`/`core.sshCommand` refer to OpenSSH, plink or tortoiseplink. This variable overrides the config setting `ssh.variant` that serves the same purpose.

  如果设置了这个环境变量，它将覆盖 Git 的自动检测，即 GIT_SSH/GIT_SSH_COMMAND/core.sshCommand 是指 OpenSSH、plink 还是 tortoiseplink。这个变量覆盖了具有相同目的的配置设置ssh.variant。
- `GIT_SSL_NO_VERIFY`

  Setting and exporting this environment variable to any value tells Git not to verify the SSL certificate when fetching or pushing over HTTPS.

  设置并导出这个环境变量的任何值，告诉Git在通过HTTPS获取或推送信息时，不要验证SSL证书。
- `GIT_ASKPASS`

  If this environment variable is set, then Git commands which need to acquire passwords or passphrases (e.g. for HTTP or IMAP authentication) will call this program with a suitable prompt as command-line argument and read the password from its STDOUT. See also the `core.askPass` option in [git-config[1]](https://git-scm.com/docs/git-config).

  如果设置了这个环境变量，那么需要获取密码或口令（例如用于HTTP或IMAP认证）的Git命令将以适当的提示作为命令行参数调用该程序，并从其STDOUT中读取密码。也请参见 git-config[1] 中的 core.askPass 选项。
- `GIT_TERMINAL_PROMPT`

  If this Boolean environment variable is set to false, git will not prompt on the terminal (e.g., when asking for HTTP authentication).

  如果这个布尔环境变量被设置为 "false"，git 将不会在终端进行提示（例如，在要求 HTTP 认证时）。
- `GIT_CONFIG_GLOBAL`
- `GIT_CONFIG_SYSTEM`

  Take the configuration from the given files instead from global or system-level configuration files. If `GIT_CONFIG_SYSTEM` is set, the system config file defined at build time (usually `/etc/gitconfig`) will not be read. Likewise, if `GIT_CONFIG_GLOBAL` is set, neither `$HOME/.gitconfig` nor `$XDG_CONFIG_HOME/git/config` will be read. Can be set to `/dev/null` to skip reading configuration files of the respective level.

  从给定的文件中获取配置，而不是从全局或系统级配置文件中获取。如果设置了GIT_CONFIG_SYSTEM，则不会读取在构建时定义的系统配置文件（通常是/etc/gitconfig）。同样地，如果GIT_CONFIG_GLOBAL被设置，$HOME/.gitconfig和$XDG_CONFIG_HOME/git/config都不会被读取。可以设置为/dev/null以跳过读取相应级别的配置文件。
- `GIT_CONFIG_NOSYSTEM`

  Whether to skip reading settings from the system-wide `$(prefix)/etc/gitconfig` file. This Boolean environment variable can be used along with `$HOME` and `$XDG_CONFIG_HOME` to create a predictable environment for a picky script, or you can set it to true to temporarily avoid using a buggy `/etc/gitconfig` file while waiting for someone with sufficient permissions to fix it.

  是否跳过从全系统的$(prefix)/etc/gitconfig文件读取设置。这个布尔环境变量可以与$HOME和$XDG_CONFIG_HOME一起使用，为一个挑剔的脚本创造一个可预测的环境，或者你可以把它设置为 "true"，在等待有足够权限的人修复它的同时，暂时避免使用有问题的/etc/gitconfig文件。
- `GIT_FLUSH`

  If this environment variable is set to "1", then commands such as *git blame* (in incremental mode), *git rev-list*, *git log*, *git check-attr* and *git check-ignore* will force a flush of the output stream after each record have been flushed. If this variable is set to "0", the output of these commands will be done using completely buffered I/O. If this environment variable is not set, Git will choose buffered or record-oriented flushing based on whether stdout appears to be redirected to a file or not.

  如果这个环境变量被设置为 "1"，那么诸如git blame（增量模式）、git rev-list、git log、git check-attr和git check-ignore等命令将在每条记录被刷新后强制刷新输出流。如果这个变量被设置为 "0"，这些命令的输出将使用完全缓冲的 I/O 来完成。如果这个环境变量没有设置，Git会根据stdout是否出现重定向到文件的情况来选择缓冲式或面向记录的刷新。
- `GIT_TRACE`

  Enables general trace messages, e.g. alias expansion, built-in command execution and external command execution.If this variable is set to "1", "2" or "true" (comparison is case insensitive), trace messages will be printed to stderr.If the variable is set to an integer value greater than 2 and lower than 10 (strictly) then Git will interpret this value as an open file descriptor and will try to write the trace messages into this file descriptor.Alternatively, if the variable is set to an absolute path (starting with a */* character), Git will interpret this as a file path and will try to append the trace messages to it.Unsetting the variable, or setting it to empty, "0" or "false" (case insensitive) disables trace messages.

  启用一般的跟踪信息，例如别名扩展、内置命令执行和外部命令执行。

  如果这个变量被设置为 "1"、"2 "或 "true"（比较时不区分大小写），跟踪信息将被打印到stderr。

  如果该变量被设置为一个大于2且小于10的整数（严格来说），那么Git将把该值解释为一个开放的文件描述符，并尝试将跟踪信息写入该文件描述符中。

  另外，如果该变量被设置为绝对路径（以/字符开头），Git 会将其解释为一个文件路径，并尝试将跟踪信息附加到其中。

  取消该变量，或将其设置为空、"0 "或 "false"（不区分大小写），都会禁用跟踪信息。
- `GIT_TRACE_FSMONITOR`

  Enables trace messages for the filesystem monitor extension. See `GIT_TRACE` for available trace output options.

  启用文件系统监控扩展的跟踪信息。参见 GIT_TRACE 以了解可用的跟踪输出选项。
- `GIT_TRACE_PACK_ACCESS`

  Enables trace messages for all accesses to any packs. For each access, the pack file name and an offset in the pack is recorded. This may be helpful for troubleshooting some pack-related performance problems. See `GIT_TRACE` for available trace output options.

  启用对任何包的所有访问的跟踪信息。对于每次访问，都会记录包的文件名和包中的偏移量。这对解决一些与包相关的性能问题可能有帮助。请参阅 GIT_TRACE 了解可用的跟踪输出选项。
- `GIT_TRACE_PACKET`

  Enables trace messages for all packets coming in or out of a given program. This can help with debugging object negotiation or other protocol issues. Tracing is turned off at a packet starting with "PACK" (but see `GIT_TRACE_PACKFILE` below). See `GIT_TRACE` for available trace output options.

  启用所有进入或离开指定程序的数据包的跟踪信息。这可以帮助调试对象协商或其他协议问题。追踪在以 "PACK "开头的数据包上被关闭（但见下面的GIT_TRACE_PACKFILE）。参见 GIT_TRACE 了解可用的跟踪输出选项。
- `GIT_TRACE_PACKFILE`

  Enables tracing of packfiles sent or received by a given program. Unlike other trace output, this trace is verbatim: no headers, and no quoting of binary data. You almost certainly want to direct into a file (e.g., `GIT_TRACE_PACKFILE=/tmp/my.pack`) rather than displaying it on the terminal or mixing it with other trace output.Note that this is currently only implemented for the client side of clones and fetches.

  启用对指定程序发送或接收的包文件的跟踪。与其他跟踪输出不同，这种跟踪是逐字记录的：没有标题，也没有二进制数据的引号。你几乎肯定希望直接进入一个文件（例如，GIT_TRACE_PACKFILE=/tmp/my.pack），而不是在终端显示或与其他跟踪输出混合。

  注意，目前这只在克隆和提取的客户端实现。
- `GIT_TRACE_PERFORMANCE`

  Enables performance related trace messages, e.g. total execution time of each Git command. See `GIT_TRACE` for available trace output options.

  启用性能相关的跟踪信息，例如每条 Git 命令的总执行时间。请参阅 GIT_TRACE 了解可用的跟踪输出选项。
- `GIT_TRACE_REFS`

  Enables trace messages for operations on the ref database. See `GIT_TRACE` for available trace output options.

  启用对 ref 数据库的操作的跟踪信息。可用的跟踪输出选项见 GIT_TRACE。
- `GIT_TRACE_SETUP`

  Enables trace messages printing the .git, working tree and current working directory after Git has completed its setup phase. See `GIT_TRACE` for available trace output options.

  在Git完成其设置阶段后，启用打印.git、工作树和当前工作目录的跟踪信息。参见 GIT_TRACE 以了解可用的跟踪输出选项。
- `GIT_TRACE_SHALLOW`

  Enables trace messages that can help debugging fetching / cloning of shallow repositories. See `GIT_TRACE` for available trace output options.

  启用跟踪信息，可以帮助调试浅层仓库的获取/克隆。请参阅 GIT_TRACE 以了解可用的跟踪输出选项。
- `GIT_TRACE_CURL`

  Enables a curl full trace dump of all incoming and outgoing data, including descriptive information, of the git transport protocol. This is similar to doing curl `--trace-ascii` on the command line. See `GIT_TRACE` for available trace output options.

  启用 curl 全面跟踪 git 传输协议的所有传入和传出数据，包括描述性信息。这类似于在命令行中做 curl --trace-ascii。参见GIT_TRACE的可用跟踪输出选项。
- `GIT_TRACE_CURL_NO_DATA`

  When a curl trace is enabled (see `GIT_TRACE_CURL` above), do not dump data (that is, only dump info lines and headers).

  当curl跟踪被启用时（见上面的GIT_TRACE_CURL），不转储数据（即只转储信息行和头文件）。
- `GIT_TRACE2`

  Enables more detailed trace messages from the "trace2" library. Output from `GIT_TRACE2` is a simple text-based format for human readability.If this variable is set to "1", "2" or "true" (comparison is case insensitive), trace messages will be printed to stderr.If the variable is set to an integer value greater than 2 and lower than 10 (strictly) then Git will interpret this value as an open file descriptor and will try to write the trace messages into this file descriptor.Alternatively, if the variable is set to an absolute path (starting with a */* character), Git will interpret this as a file path and will try to append the trace messages to it. If the path already exists and is a directory, the trace messages will be written to files (one per process) in that directory, named according to the last component of the SID and an optional counter (to avoid filename collisions).In addition, if the variable is set to `af_unix:[<socket_type>:]<absolute-pathname>`, Git will try to open the path as a Unix Domain Socket. The socket type can be either `stream` or `dgram`.Unsetting the variable, or setting it to empty, "0" or "false" (case insensitive) disables trace messages.See [Trace2 documentation](https://git-scm.com/docs/api-trace2) for full details.

  启用 "trace2 "库中更详细的跟踪信息。GIT_TRACE2的输出是一种简单的基于文本的格式，便于人类阅读。

  如果这个变量被设置为 "1"、"2 "或 "true"（比较时不区分大小写），跟踪信息将被打印到stderr。

  如果该变量被设置为大于2且小于10的整数（严格来说），那么Git会将该值解释为一个开放的文件描述符，并会尝试将跟踪信息写入该文件描述符中。

  另外，如果该变量被设置为绝对路径（以/字符开头），Git 会将其解释为一个文件路径，并尝试将跟踪信息附加到其中。如果该路径已经存在并且是一个目录，那么跟踪信息将被写入该目录中的文件（每个进程一个），根据SID的最后一个组成部分和一个可选的计数器（以避免文件名冲突）来命名。

  此外，如果该变量被设置为af_unix:[<socket_type>:]`<absolute-pathname>`，Git将尝试以Unix域套接字的方式打开该路径。套接字类型可以是stream或dgram。

  取消设置该变量，或将其设置为空、"0 "或 "false"（不区分大小写），都会使跟踪信息失效。

  详细内容请参见Trace2文档。
- `GIT_TRACE2_EVENT`

  This setting writes a JSON-based format that is suited for machine interpretation. See `GIT_TRACE2` for available trace output options and [Trace2 documentation](https://git-scm.com/docs/api-trace2) for full details.

  此项设置会写入适合机器解释的基于JSON的格式。参见 GIT_TRACE2 以了解可用的跟踪输出选项和 Trace2 文档以了解全部细节。
- `GIT_TRACE2_PERF`

  In addition to the text-based messages available in `GIT_TRACE2`, this setting writes a column-based format for understanding nesting regions. See `GIT_TRACE2` for available trace output options and [Trace2 documentation](https://git-scm.com/docs/api-trace2) for full details.

  除了 GIT_TRACE2 中可用的基于文本的信息外，此设置还写出一种基于列的格式，以了解嵌套区域。有关可用的跟踪输出选项，请参见 GIT_TRACE2 和 Trace2 文档以了解完整的细节。
- `GIT_TRACE_REDACT`

  By default, when tracing is activated, Git redacts the values of cookies, the "Authorization:" header, the "Proxy-Authorization:" header and packfile URIs. Set this Boolean environment variable to false to prevent this redaction.

  默认情况下，当追踪被激活时，Git 会节录 cookies、"Authorization: "头、"Proxy-Authorization: "头和 packfile URIs 的值。将这个布尔环境变量设置为false以防止这种编辑。
- `GIT_LITERAL_PATHSPECS`

  Setting this Boolean environment variable to true will cause Git to treat all pathspecs literally, rather than as glob patterns. For example, running `GIT_LITERAL_PATHSPECS=1 git log -- '*.c'` will search for commits that touch the path `*.c`, not any paths that the glob `*.c` matches. You might want this if you are feeding literal paths to Git (e.g., paths previously given to you by `git ls-tree`, `--raw` diff output, etc).

  将这个布尔环境变量设置为 "true "会使Git从字面上处理所有pathspecs，而不是作为glob模式。例如，运行 GIT_LITERAL_PATHSPECS=1 git log -- '*.c' 将搜索触及路径 *.c 的提交，而不是任何与 glob *.c 匹配的路径。如果您向 Git 输入字面路径（例如，之前由 git ls-tree、--raw diff 输出等提供的路径），您可能需要这样。
- `GIT_GLOB_PATHSPECS`

  Setting this Boolean environment variable to true will cause Git to treat all pathspecs as glob patterns (aka "glob" magic).

  将这个布尔环境变量设置为 "true "会使Git将所有的路径规格当作glob模式（又称 "glob "魔法）。
- `GIT_NOGLOB_PATHSPECS`

  Setting this Boolean environment variable to true will cause Git to treat all pathspecs as literal (aka "literal" magic).

  把这个布尔环境变量设置为 "true"，会使Git把所有的pathspecs都当作字面意思（又称 "字面 "魔法）。
- `GIT_ICASE_PATHSPECS`

  Setting this Boolean environment variable to true will cause Git to treat all pathspecs as case-insensitive.

  将这个布尔环境变量设置为 "true"，将使 Git 把所有的 pathspecs 视为不区分大小写的。
- `GIT_REFLOG_ACTION`

  When a ref is updated, reflog entries are created to keep track of the reason why the ref was updated (which is typically the name of the high-level command that updated the ref), in addition to the old and new values of the ref. A scripted Porcelain command can use set_reflog_action helper function in `git-sh-setup` to set its name to this variable when it is invoked as the top level command by the end user, to be recorded in the body of the reflog.

  当一个 ref 被更新时，会创建 reflog 条目来跟踪 ref 被更新的原因（通常是更新 ref 的高级命令的名称），此外还有 ref 的新旧值。脚本化的Porcelain命令可以使用git-sh-setup中的set_reflog_action辅助函数，在它被终端用户作为顶级命令调用时，将其名称设置为这个变量，以记录在reflog的正文中。
- `GIT_REF_PARANOIA`

  If this Boolean environment variable is set to false, ignore broken or badly named refs when iterating over lists of refs. Normally Git will try to include any such refs, which may cause some operations to fail. This is usually preferable, as potentially destructive operations (e.g., [git-prune[1]](https://git-scm.com/docs/git-prune)) are better off aborting rather than ignoring broken refs (and thus considering the history they point to as not worth saving). The default value is `1` (i.e., be paranoid about detecting and aborting all operations). You should not normally need to set this to `0`, but it may be useful when trying to salvage data from a corrupted repository.

  如果这个布尔环境变量被设置为 "false"，那么在迭代 refs 列表时，就会忽略破损或糟糕的 refs 命名。通常情况下，Git 会尝试包含任何这样的 refs，这可能会导致一些操作失败。这通常是可取的，因为潜在的破坏性操作（例如，git-prune[1]）最好是中止，而不是忽略破碎的 refs（从而认为它们指向的历史不值得保存）。默认值是1（即偏执地检测并中止所有操作）。通常你不应该把它设置为0，但在试图从损坏的版本库中抢救数据时，它可能很有用。
- `GIT_ALLOW_PROTOCOL`

  If set to a colon-separated list of protocols, behave as if `protocol.allow` is set to `never`, and each of the listed protocols has `protocol.<name>.allow` set to `always` (overriding any existing configuration). See the description of `protocol.allow` in [git-config[1]](https://git-scm.com/docs/git-config) for more details.

  如果设置为一个用冒号分隔的协议列表，则表现为protocol.allow被设置为从不，并且每个列出的协议的protocol.`<name>`.allow被设置为总是（覆盖任何现有配置）。更多细节请参见 git-config[1] 中关于 protocol.allow 的描述。
- `GIT_PROTOCOL_FROM_USER`

  Set this Boolean environment variable to false to prevent protocols used by fetch/push/clone which are configured to the `user` state. This is useful to restrict recursive submodule initialization from an untrusted repository or for programs which feed potentially-untrusted URLS to git commands. See [git-config[1]](https://git-scm.com/docs/git-config) for more details.

  将此布尔环境变量设置为false，以防止fetch/push/clone使用的协议被配置为用户状态。这对于限制从一个不受信任的仓库递归子模块初始化，或者对于向 git 命令提供潜在不受信任的 URLS 的程序很有用。参见 git-config[1] 以了解更多细节。
- `GIT_PROTOCOL`

  For internal use only. Used in handshaking the wire protocol. Contains a colon *:* separated list of keys with optional values *key[=value]*. Presence of unknown keys and values must be ignored.Note that servers may need to be configured to allow this variable to pass over some transports. It will be propagated automatically when accessing local repositories (i.e., `file://` or a filesystem path), as well as over the `git://` protocol. For git-over-http, it should work automatically in most configurations, but see the discussion in [git-http-backend[1]](https://git-scm.com/docs/git-http-backend). For git-over-ssh, the ssh server may need to be configured to allow clients to pass this variable (e.g., by using `AcceptEnv GIT_PROTOCOL` with OpenSSH).This configuration is optional. If the variable is not propagated, then clients will fall back to the original "v0" protocol (but may miss out on some performance improvements or features). This variable currently only affects clones and fetches; it is not yet used for pushes (but may be in the future).

  仅供内部使用。用于线程协议的握手。包含一个用冒号分隔的键值列表，键[=值]是可选的。未知键和值的存在必须被忽略。

  注意，可能需要对服务器进行配置，以允许该变量在某些传输中传递。在访问本地仓库（即file://或文件系统路径）以及通过git://协议时，它将被自动传播。对于 git-over-http，它在大多数配置中都会自动工作，但请参见 git-http-backend[1] 中的讨论。对于 git-over-ssh，ssh 服务器可能需要配置为允许客户端传递这个变量（例如，通过使用 OpenSSH 的 AcceptEnv GIT_PROTOCOL）。

  这种配置是可选的。如果该变量没有被传播，那么客户端将回到原来的 "v0 "协议（但可能会错过一些性能改进或功能）。这个变量目前只影响克隆和获取；它还没有被用于推送（但将来可能会）。
- `GIT_OPTIONAL_LOCKS`

  If this Boolean environment variable is set to false, Git will complete any requested operation without performing any optional sub-operations that require taking a lock. For example, this will prevent `git status` from refreshing the index as a side effect. This is useful for processes running in the background which do not want to cause lock contention with other operations on the repository. Defaults to `1`.

  如果这个布尔环境变量被设置为false，Git将完成任何请求的操作而不执行任何需要加锁的可选子操作。例如，这将防止git status刷新索引作为一个副作用。这对于在后台运行的进程很有用，因为它们不想与仓库上的其他操作造成锁争夺。默认值为1。
- `GIT_REDIRECT_STDIN`
- `GIT_REDIRECT_STDOUT`
- `GIT_REDIRECT_STDERR`

  Windows-only: allow redirecting the standard input/output/error handles to paths specified by the environment variables. This is particularly useful in multi-threaded applications where the canonical way to pass standard handles via `CreateProcess()` is not an option because it would require the handles to be marked inheritable (and consequently **every** spawned process would inherit them, possibly blocking regular Git operations). The primary intended use case is to use named pipes for communication (e.g. `\\.\pipe\my-git-stdin-123`).Two special values are supported: `off` will simply close the corresponding standard handle, and if `GIT_REDIRECT_STDERR` is `2>&1`, standard error will be redirected to the same handle as standard output.

  仅限Windows：允许将标准输入/输出/错误句柄重定向到环境变量指定的路径。这在多线程应用中特别有用，因为通过CreateProcess()传递标准句柄的典型方式是不可行的，因为它需要将句柄标记为可继承（因此每个生成的进程都会继承它们，可能会阻碍正常的Git操作）。主要的用途是使用命名的管道进行通信（例如：\\.\pipe\my-git-stdin-123）。

  支持两个特殊的值：关闭会简单地关闭相应的标准句柄，如果GIT_REDIRECT_STDERR是2>&1，标准错误会被重定向到与标准输出相同的句柄。
- `GIT_PRINT_SHA1_ELLIPSIS` (deprecated)

  If set to `yes`, print an ellipsis following an (abbreviated) SHA-1 value. This affects indications of detached HEADs ([git-checkout[1]](https://git-scm.com/docs/git-checkout)) and the raw diff output ([git-diff[1]](https://git-scm.com/docs/git-diff)). Printing an ellipsis in the cases mentioned is no longer considered adequate and support for it is likely to be removed in the foreseeable future (along with the variable).

  如果设置为 "是"，在（缩写的）SHA-1值后面打印一个省略号。这将影响到分离的 HEAD（git-checkout[1]）和原始差异输出（git-diff[1]）的指示。在上述情况下打印省略号不再被认为是适当的，对它的支持可能会在可预见的未来被移除（连同该变量）。

## Discussion 讨论

More detail on the following is available from the [Git concepts chapter of the user-manual](https://git-scm.com/docs/user-manual#git-concepts) and [gitcore-tutorial[7]](https://git-scm.com/docs/gitcore-tutorial).

关于以下内容的更多细节，可以从用户手册的 Git 概念一章和 gitcore-tutorial[7] 中获得。

A Git project normally consists of a working directory with a ".git" subdirectory at the top level. The .git directory contains, among other things, a compressed object database representing the complete history of the project, an "index" file which links that history to the current contents of the working tree, and named pointers into that history such as tags and branch heads.

一个 Git 项目通常由一个工作目录组成，其顶层是一个".git "子目录。.git 目录包含，除其他外，一个代表项目完整历史的压缩对象数据库，一个将该历史与工作树的当前内容联系起来的 "索引 "文件，以及指向该历史的命名指针，如标签和分支头。

The object database contains objects of three main types: blobs, which hold file data; trees, which point to blobs and other trees to build up directory hierarchies; and commits, which each reference a single tree and some number of parent commits.

对象数据库包含三种主要类型的对象：持有文件数据的Blobs；指向Blobs和其他树以建立目录层次的树；以及提交，每个提交都引用一个树和一些父提交。

The commit, equivalent to what other systems call a "changeset" or "version", represents a step in the project’s history, and each parent represents an immediately preceding step. Commits with more than one parent represent merges of independent lines of development.

提交，相当于其他系统所说的 "变化集 "或 "版本"，代表项目历史中的一个步骤，每个父级代表紧接着的一个步骤。有多个父级的提交代表了独立开发线的合并。

All objects are named by the SHA-1 hash of their contents, normally written as a string of 40 hex digits. Such names are globally unique. The entire history leading up to a commit can be vouched for by signing just that commit. A fourth object type, the tag, is provided for this purpose.

所有的对象都由其内容的SHA-1哈希值来命名，通常写成40位十六进制数字的字符串。这样的名字是全球唯一的。导致一个提交的整个历史可以通过签署该提交来证明。第四种对象类型，即标签，就是为了这个目的而提供的。

When first created, objects are stored in individual files, but for efficiency may later be compressed together into "pack files".

当第一次创建时，对象被存储在单独的文件中，但为了提高效率，后来可能被压缩在一起成为 "打包文件"。

Named pointers called refs mark interesting points in history. A ref may contain the SHA-1 name of an object or the name of another ref. Refs with names beginning `ref/head/` contain the SHA-1 name of the most recent commit (or "head") of a branch under development. SHA-1 names of tags of interest are stored under `ref/tags/`. A special ref named `HEAD` contains the name of the currently checked-out branch.

被称为ref的命名指针标志着历史上的有趣点。一个ref可以包含一个对象的SHA-1名称或另一个ref的名称。名称以ref/head/开头的参考文献包含开发中的分支的最新提交（或 "头部"）的SHA-1名称。感兴趣的标签的SHA-1名称被存储在ref/tags/下。一个名为HEAD的特殊参考文件包含了当前签出的分支的名称。

The index file is initialized with a list of all paths and, for each path, a blob object and a set of attributes. The blob object represents the contents of the file as of the head of the current branch. The attributes (last modified time, size, etc.) are taken from the corresponding file in the working tree. Subsequent changes to the working tree can be found by comparing these attributes. The index may be updated with new content, and new commits may be created from the content stored in the index.

索引文件的初始化包括一个所有路径的列表，以及每个路径的一个blob对象和一组属性。blob对象代表当前分支头部的文件内容。属性（最后修改时间、大小等）取自工作树中的相应文件。通过比较这些属性，可以发现工作树的后续变化。索引可以用新的内容进行更新，也可以从索引中存储的内容创建新的提交。

The index is also capable of storing multiple entries (called "stages") for a given pathname. These stages are used to hold the various unmerged version of a file when a merge is in progress.

索引还能够为一个给定的路径名存储多个条目（称为 "阶段"）。这些阶段是用来保存一个文件在合并过程中的各种未合并的版本。

## FURTHER DOCUMENTATION 进一步的文件

See the references in the "description" section to get started using Git. The following is probably more detail than necessary for a first-time user.

参见 "描述 "部分的参考文献，开始使用 Git。对于第一次使用的用户来说，下面的内容可能比需要的更详细。

The [Git concepts chapter of the user-manual](https://git-scm.com/docs/user-manual#git-concepts) and [gitcore-tutorial[7]](https://git-scm.com/docs/gitcore-tutorial) both provide introductions to the underlying Git architecture.

用户手册中的 Git 概念一章和 gitcore-tutorial[7] 都对 Git 的底层架构做了介绍。

See [gitworkflows[7]](https://git-scm.com/docs/gitworkflows) for an overview of recommended workflows.

推荐的工作流程概览见 gitworkflows[7]。

See also the [howto](https://git-scm.com/docs/howto-index) documents for some useful examples.

一些有用的例子也请参见 howto 文档。

The internals are documented in the [Git API documentation](https://git-scm.com/docs/api-index).

内部结构在 Git API 文档中有所记载。

Users migrating from CVS may also want to read [gitcvs-migration[7]](https://git-scm.com/docs/gitcvs-migration).

从 CVS 迁移过来的用户可能还需要阅读 gitcvs-migration[7]。

## Authors 作者

Git was started by Linus Torvalds, and is currently maintained by Junio C Hamano. Numerous contributions have come from the Git mailing list [[git@vger.kernel.org](mailto:git@vger.kernel.org)](%5Bgit@vger.kernel.org%5D(mailto:git@vger.kernel.org)). http://www.openhub.net/p/git/contributors/summary gives you a more complete list of contributors.

Git 是由 Linus Torvalds 发起的，目前由 Junio C Hamano 维护。大量的贡献来自于 Git 邮件列表 <git@vger.kernel.org>。http://www.openhub.net/p/git/contributors/summary 给你一个更完整的贡献者名单。

If you have a clone of git.git itself, the output of [git-shortlog[1]](https://git-scm.com/docs/git-shortlog) and [git-blame[1]](https://git-scm.com/docs/git-blame) can show you the authors for specific parts of the project.

如果你有一个git.git本身的克隆，git-shortlog[1]和git-blame[1]的输出可以告诉你项目特定部分的作者。

## Reporting Bugs 报告 BUG

Report bugs to the Git mailing list [[git@vger.kernel.org](mailto:git@vger.kernel.org)](%5Bgit@vger.kernel.org%5D(mailto:git@vger.kernel.org)) where the development and maintenance is primarily done. You do not have to be subscribed to the list to send a message there. See the list archive at https://lore.kernel.org/git for previous bug reports and other discussions.

Issues which are security relevant should be disclosed privately to the Git Security mailing list [[git-security@googlegroups.com](mailto:git-security@googlegroups.com)](%5Bgit-security@googlegroups.com%5D(mailto:git-security@googlegroups.com)).

向 Git 邮件列表 <git@vger.kernel.org> 报告错误，这里主要是进行开发和维护。你不需要订阅该列表也可以在那里发送消息。请参阅列表存档，https://lore.kernel.org/git，查看以前的错误报告和其他讨论。

与安全相关的问题应该在Git安全邮件列表<git-security@googlegroups.com>中私下披露。

## 另请参阅

[gittutorial[7]](https://git-scm.com/docs/gittutorial), [gittutorial-2[7]](https://git-scm.com/docs/gittutorial-2), [giteveryday[7]](https://git-scm.com/docs/giteveryday), [gitcvs-migration[7]](https://git-scm.com/docs/gitcvs-migration), [gitglossary[7]](https://git-scm.com/docs/gitglossary), [gitcore-tutorial[7]](https://git-scm.com/docs/gitcore-tutorial), [gitcli[7]](https://git-scm.com/docs/gitcli), [The Git User’s Manual](https://git-scm.com/docs/user-manual), [gitworkflows[7]](https://git-scm.com/docs/gitworkflows)

gittutorial[7], gittutorial-2[7], giteveryday[7], gitcvs-migration[7], gitglossary[7], gitcore-tutorial[7], gitcli[7], Git用户手册, gitworkflows[7]

## GIT

这是[git[1]](#)工具集中的一部分。

