+++
title = "git-commit——中英对照版"
weight = 30
type = "docs"
date = 2023-05-08T15:39:23+08:00
description = ""
isCJKLanguage = true
draft = true

+++

# git-commit

https://git-scm.com/docs/git-commit

version 2.41.0

## 名称

git-commit - Record changes to the repository

​	git-commit - 记录对仓库的更改

## 概述

```
git commit [-a | --interactive | --patch] [-s] [-v] [-u<mode>] [--amend]
	   [--dry-run] [(-c | -C | --squash) <commit> | --fixup [(amend|reword):]<commit>)]
	   [-F <file> | -m <msg>] [--reset-author] [--allow-empty]
	   [--allow-empty-message] [--no-verify] [-e] [--author=<author>]
	   [--date=<date>] [--cleanup=<mode>] [--[no-]status]
	   [-i | -o] [--pathspec-from-file=<file> [--pathspec-file-nul]]
	   [(--trailer <token>[(=|:)<value>])…] [-S[<keyid>]]
	   [--] [<pathspec>…]
```

## 描述

Create a new commit containing the current contents of the index and the given log message describing the changes. The new commit is a direct child of HEAD, usually the tip of the current branch, and the branch is updated to point to it (unless no branch is associated with the working tree, in which case HEAD is "detached" as described in [git-checkout[1]](../git-checkout)).

​	创建一个新的提交，包含当前索引（即暂存区）的内容以及给定的日志消息，用于描述这些更改。新的提交是 HEAD 的直接子提交，通常是当前分支的最新提交点，并且分支会更新为指向该提交（除非工作树没有关联的分支，此时 HEAD 将会“分离”，如 [git-checkout[1]](../git-checkout) 中所述）。

The content to be committed can be specified in several ways:

​	要提交的内容可以通过多种方式指定：

1. by using [git-add[1]](../git-add) to incrementally "add" changes to the index before using the *commit* command (Note: even modified files must be "added");
2. 使用 [git-add[1]](../git-add) 逐步地在使用 *commit* 命令之前“添加”更改到索引中（注意：即使修改文件也必须“添加”）；
3. by using [git-rm[1]](../git-rm) to remove files from the working tree and the index, again before using the *commit* command;
4. 使用 [git-rm[1]](../git-rm) 在使用 *commit* 命令之前从工作树和索引中移除文件；
5. by listing files as arguments to the *commit* command (without --interactive or --patch switch), in which case the commit will ignore changes staged in the index, and instead record the current content of the listed files (which must already be known to Git);
6. 将文件列为 *commit* 命令的参数（不带 --interactive 或 --patch 开关），在这种情况下，提交将忽略索引中暂存的更改，并记录列出文件的当前内容（这些文件必须已经被 Git 知道）；
7. by using the -a switch with the *commit* command to automatically "add" changes from all known files (i.e. all files that are already listed in the index) and to automatically "rm" files in the index that have been removed from the working tree, and then perform the actual commit;
8. 使用 *commit* 命令的 -a 开关，自动地“添加”所有已知文件的更改（即索引中已经列出的所有文件），并自动地“删除”索引中已从工作树中删除的文件，然后执行实际的提交；
9. by using the --interactive or --patch switches with the *commit* command to decide one by one which files or hunks should be part of the commit in addition to contents in the index, before finalizing the operation. See the “Interactive Mode” section of [git-add[1]](../git-add) to learn how to operate these modes.
10. 使用 *commit* 命令的 --interactive 或 --patch 开关，逐个决定哪些文件或文件块应该作为提交的一部分，除了索引中的内容，然后完成操作。有关如何操作这些模式，请参阅 [git-add[1]](../git-add) 中的“交互模式”部分。

The `--dry-run` option can be used to obtain a summary of what is included by any of the above for the next commit by giving the same set of parameters (options and paths).

​	`--dry-run` 选项可以用于通过使用相同的一组参数（选项和路径）获得下一次提交要包含的内容的摘要。

If you make a commit and then find a mistake immediately after that, you can recover from it with *git reset*.

​	如果您提交后立即发现错误，可以通过 *git reset* 来恢复。

## 选项

- -a

- --all

  Tell the command to automatically stage files that have been modified and deleted, but new files you have not told Git about are not affected.

  命令会自动暂存已修改和已删除的文件，但不会影响尚未添加到 Git 中的新文件。

- -p

- --patch

  Use the interactive patch selection interface to choose which changes to commit. See [git-add[1]](../git-add) for details.

  使用交互式补丁选择界面来选择要提交的更改。有关详情，请参阅 [git-add[1]](../git-add)。

- -C <commit>

- --reuse-message=<commit>

  Take an existing commit object, and reuse the log message and the authorship information (including the timestamp) when creating the commit.

  使用现有的提交对象，并在创建提交时重用其日志消息和作者信息（包括时间戳）。

- -c <commit>

- --reedit-message=<commit>

  Like *-C*, but with `-c` the editor is invoked, so that the user can further edit the commit message.

  类似于 *-C*，但使用 `-c` 选项会调用编辑器，以便用户可以进一步编辑提交消息。

- --fixup=[(amend|reword):]<commit>

  Create a new commit which "fixes up" `<commit>` when applied with `git rebase --autosquash`. Plain `--fixup=<commit>` creates a "fixup!" commit which changes the content of `<commit>` but leaves its log message untouched. `--fixup=amend:<commit>` is similar but creates an "amend!" commit which also replaces the log message of `<commit>` with the log message of the "amend!" commit.

  创建一个新的提交，用于“修正” `<commit>`，当通过 `git rebase --autosquash` 应用时。简单的 `--fixup=<commit>` 创建一个 "fixup!" 提交，它会更改 `<commit>` 的内容，但保留其日志消息不变。`--fixup=amend:<commit>` 类似，但它创建一个 "amend!" 提交，它还将 `<commit>` 的日志消息替换为 "amend!" 提交的日志消息。

  `--fixup=reword:<commit>` creates an "amend!" commit which replaces the log message of `<commit>` with its own log message but makes no changes to the content of `<commit>`.The commit created by plain `--fixup=<commit>` has a subject composed of "fixup!" followed by the subject line from <commit>, and is recognized specially by `git rebase --autosquash`. The `-m` option may be used to supplement the log message of the created commit, but the additional commentary will be thrown away once the "fixup!" commit is squashed into `<commit>` by `git rebase --autosquash`.

  `--fixup=reword:<commit>` 创建一个 "amend!" 提交，将 `<commit>` 的日志消息替换为它自己的日志消息，但不会更改 `<commit>` 的内容。通过 `--fixup=<commit>` 创建的提交的主题由 "fixup!" 开头，后面跟着 <commit> 的主题行，并且被 `git rebase --autosquash` 特别识别。可以使用 -m 选项来补充创建的提交的日志消息，但是这些附加评论将在 "fixup!" 提交通过 `git rebase --autosquash` 合并到 `<commit>` 时被丢弃。

  The commit created by `--fixup=amend:<commit>` is similar but its subject is instead prefixed with "amend!". The log message of <commit> is copied into the log message of the "amend!" commit and opened in an editor so it can be refined. When `git rebase --autosquash` squashes the "amend!" commit into `<commit>`, the log message of `<commit>` is replaced by the refined log message from the "amend!" commit. It is an error for the "amend!" commit’s log message to be empty unless `--allow-empty-message` is specified.

  通过 `--fixup=amend:<commit>` 创建的提交类似，但其主题的前缀是 "amend!"。将 <commit> 的日志消息复制到 "amend!" 提交的日志消息中，并在编辑器中打开它以便进行优化。当 `git rebase --autosquash` 将 "amend!" 提交合并到 `<commit>` 时，将用 "amend!" 提交的优化日志消息替换 `<commit>` 的日志消息。除非指定了 `--allow-empty-message`，否则 "amend!" 提交的日志消息为空是错误的。

  `--fixup=reword:<commit>` is shorthand for `--fixup=amend:<commit> --only`. It creates an "amend!" commit with only a log message (ignoring any changes staged in the index). When squashed by `git rebase --autosquash`, it replaces the log message of `<commit>` without making any other changes.

  `--fixup=reword:<commit>` 是 `--fixup=amend:<commit> --only` 的简写形式。它创建一个 "amend!" 提交，只有日志消息（忽略索引中的任何更改）。当通过 `git rebase --autosquash` 进行合并时，它将替换 `<commit>` 的日志消息，而不会做其他更改。

  Neither "fixup!" nor "amend!" commits change authorship of `<commit>` when applied by `git rebase --autosquash`. See [git-rebase[1]](../git-rebase) for details.

  当通过 `git rebase --autosquash` 应用时，“fixup!” 和 “amend!” 提交不会更改 `<commit>` 的作者信息。有关详情，请参阅 [git-rebase[1]](../git-rebase)。

- --squash=<commit>

  Construct a commit message for use with `rebase --autosquash`. The commit message subject line is taken from the specified commit with a prefix of "squash! ". Can be used with additional commit message options (`-m`/`-c`/`-C`/`-F`). See [git-rebase[1]](../git-rebase) for details.

  为 `rebase --autosquash` 构造提交消息。提交消息的主题行来自指定的提交，并带有 "squash! " 前缀。可以与其他提交消息选项 (`-m`/`-c`/`-C`/`-F`) 一起使用。有关详情，请参阅 [git-rebase[1]](../git-rebase)。

- --reset-author

  When used with -C/-c/--amend options, or when committing after a conflicting cherry-pick, declare that the authorship of the resulting commit now belongs to the committer. This also renews the author timestamp.

  在使用 -C/-c/--amend 选项或在合并冲突的 cherry-pick 后提交时，声明生成的提交的作者是提交者。这还会更新作者的时间戳。

- --short

  When doing a dry-run, give the output in the short-format. See [git-status[1]](../git-status) for details. Implies `--dry-run`.

  在进行干运行时，以简短格式输出。有关详情，请参阅 [git-status[1]](../git-status)。隐含了 `--dry-run`。

- --branch

  Show the branch and tracking info even in short-format.

  即使在简短格式中也显示分支和追踪信息。

- --porcelain

  When doing a dry-run, give the output in a porcelain-ready format. See [git-status[1]](../git-status) for details. Implies `--dry-run`.

  在进行干运行时，以瓷器格式输出。有关详情，请参阅 [git-status[1]](../git-status)。隐含了 `--dry-run`。

- --long

  When doing a dry-run, give the output in the long-format. Implies `--dry-run`.

  在进行干运行时，以长格式输出。隐含了 `--dry-run`。

- -z

- --null

  When showing `short` or `porcelain` status output, print the filename verbatim and terminate the entries with NUL, instead of LF. If no format is given, implies the `--porcelain` output format. Without the `-z` option, filenames with "unusual" characters are quoted as explained for the configuration variable `core.quotePath` (see [git-config[1]](../git-config)).

  在显示 `short` 或 `porcelain` 状态输出时，直接打印文件名，并以 NUL 终止条目，而不是 LF。如果没有给定格式，则隐含了 `--porcelain` 输出格式。如果没有使用 `-z` 选项，则对于包含 "非常规" 字符的文件名，将对其进行引用，如配置变量 `core.quotePath`（参见 [git-config[1]](../git-config)）中所解释的。

- -F <file>

- --file=<file>

  Take the commit message from the given file. Use *-* to read the message from the standard input.

  使用给定文件中的内容作为提交消息。使用 *-* 从标准输入读取消息。

- --author=<author>

  Override the commit author. Specify an explicit author using the standard `A U Thor <author@example.com>` format. Otherwise <author> is assumed to be a pattern and is used to search for an existing commit by that author (i.e. rev-list --all -i --author=<author>); the commit author is then copied from the first such commit found.

  覆盖提交作者。使用标准的 `A U Thor <author@example.com>` 格式指定明确的作者。否则，将假设 <author> 是一个模式，并使用该作者搜索现有的提交（即 rev-list --all -i --author=<author>）；然后将提交作者复制自第一个找到的提交。

- --date=<date>

  Override the author date used in the commit.

  覆盖提交中使用的作者日期。

- -m <msg>

- --message=<msg>

  Use the given <msg> as the commit message. If multiple `-m` options are given, their values are concatenated as separate paragraphs.The `-m` option is mutually exclusive with `-c`, `-C`, and `-F`.

  将给定的 <msg> 作为提交消息。如果给定了多个 `-m` 选项，则它们的值将作为单独的段落连接。`-m` 选项与 `-c`、`-C` 和 `-F` 选项互斥。

- -t <file>

- --template=<file>

  When editing the commit message, start the editor with the contents in the given file. The `commit.template` configuration variable is often used to give this option implicitly to the command. This mechanism can be used by projects that want to guide participants with some hints on what to write in the message in what order. If the user exits the editor without editing the message, the commit is aborted. This has no effect when a message is given by other means, e.g. with the `-m` or `-F` options.

  编辑提交消息时，使用给定文件中的内容启动编辑器。`commit.template` 配置变量通常用于隐含地给命令提供此选项。该机制可用于希望向参与者提供一些有关按照什么顺序编写消息的提示的项目。如果用户退出编辑器而未编辑消息，则提交将被中止。当通过其他方式给出消息时（例如使用 `-m` 或 `-F` 选项），此选项无效。

- -s

- --signoff

- --no-signoff

  Add a `Signed-off-by` trailer by the committer at the end of the commit log message. The meaning of a signoff depends on the project to which you’re committing. For example, it may certify that the committer has the rights to submit the work under the project’s license or agrees to some contributor representation, such as a Developer Certificate of Origin. (See [http://developercertificate.org](http://developercertificate.org/) for the one used by the Linux kernel and Git projects.) Consult the documentation or leadership of the project to which you’re contributing to understand how the signoffs are used in that project.The --no-signoff option can be used to countermand an earlier --signoff option on the command line.

  在提交日志消息的末尾，由提交者添加 `Signed-off-by` 尾注。签名的含义取决于您要提交的项目。例如，它可以证明提交者在项目的许可证下有权提交工作，或者同意某些贡献者代表，例如开发者证书。 （有关 Linux 内核和 Git 项目使用的签名，请参阅 [http://developercertificate.org](http://developercertificate.org/)）。请咨询您要贡献的项目的文档或领导层，了解在该项目中如何使用签名。`--no-signoff` 选项可以用于抵消命令行上之前使用的 `--signoff` 选项。

- --trailer <token>[(=|:)<value>]

  Specify a (<token>, <value>) pair that should be applied as a trailer. (e.g. `git commit --trailer "Signed-off-by:C O Mitter \ <committer@example.com>" --trailer "Helped-by:C O Mitter \ <committer@example.com>"` will add the "Signed-off-by" trailer and the "Helped-by" trailer to the commit message.) The `trailer.*` configuration variables ([git-interpret-trailers[1]](../git-interpret-trailers)) can be used to define if a duplicated trailer is omitted, where in the run of trailers each trailer would appear, and other details.

  指定应作为尾注应用的（<token>，<value>）对。 （例如，`git commit --trailer "Signed-off-by:C O Mitter \ <committer@example.com>" --trailer "Helped-by:C O Mitter \ <committer@example.com>"` 将向提交消息添加 "Signed-off-by" 尾注和 "Helped-by" 尾注）。 `trailer.*` 配置变量（参见 [git-interpret-trailers[1]](../git-interpret-trailers)）可用于定义是否省略重复的尾注，每个尾注在尾注运行中出现的位置，以及其他详细信息。

- -n

- --[no-]verify

  By default, the pre-commit and commit-msg hooks are run. When any of `--no-verify` or `-n` is given, these are bypassed. See also [githooks[5]](../../5/githooks).

  默认情况下，会运行预提交和提交消息挂钩。给出 `--no-verify` 或 `-n` 中的任何选项时，这些挂钩将被绕过。另请参阅 [githooks[5]](../5/githooks)。

- --allow-empty

  Usually recording a commit that has the exact same tree as its sole parent commit is a mistake, and the command prevents you from making such a commit. This option bypasses the safety, and is primarily for use by foreign SCM interface scripts.

  通常，记录与其唯一父提交具有完全相同的树的提交是错误的，命令会阻止您进行此类提交。此选项绕过了这种安全性，主要供外部 SCM 接口脚本使用。

- --allow-empty-message

  Like --allow-empty this command is primarily for use by foreign SCM interface scripts. It allows you to create a commit with an empty commit message without using plumbing commands like [git-commit-tree[1]](../git-commit-tree).

  与 --allow-empty 一样，此命令主要供外部 SCM 接口脚本使用。它允许您创建一个没有使用像 [git-commit-tree[1]](../git-commit-tree) 等工具的空提交消息的提交。

- --cleanup=<mode>

  This option determines how the supplied commit message should be cleaned up before committing. The *<mode>* can be `strip`, `whitespace`, `verbatim`, `scissors` or `default`.

  此选项确定在提交之前如何清理提供的提交消息。*<mode>* 可以是 `strip`、`whitespace`、`verbatim`、`scissors` 或 `default`。

  - strip

    Strip leading and trailing empty lines, trailing whitespace, commentary and collapse consecutive empty lines.

    去除前导和尾随空行，尾随空白，注释，并折叠连续的空行。

  - whitespace

    Same as `strip` except #commentary is not removed.

    与 `strip` 相同，但不会移除 # 注释。

  - verbatim

    Do not change the message at all.

    不对消息进行任何更改。

  - scissors

    Same as `whitespace` except that everything from (and including) the line found below is truncated, if the message is to be edited. "`#`" can be customized with core.commentChar.

    与 `whitespace` 相同，但是一旦编辑消息，就会截断以下发现的行，即从（包括）下面找到的行开始，如果消息要进行编辑。 "`#`" 可以通过 core.commentChar 进行自定义。

    ```
    # ------------------------ >8 ------------------------
    ```

    

  - default

    Same as `strip` if the message is to be edited. Otherwise `whitespace`.
    
    如果消息要进行编辑，则与 `strip` 相同。否则与 `whitespace` 相同。

  The default can be changed by the `commit.cleanup` configuration variable (see [git-config[1]](../git-config)).

  默认值可以通过 `commit.cleanup` 配置变量更改（参见 [git-config[1]](../git-config)）。

- -e

- --edit

  The message taken from file with `-F`, command line with `-m`, and from commit object with `-C` are usually used as the commit log message unmodified. This option lets you further edit the message taken from these sources.

  通常，从 `-F` 文件中获取的消息、命令行中的消息（使用 `-m` 选项）以及提交对象中获取的消息（使用 `-C` 选项）都会原样使用作为提交日志消息。此选项允许您进一步编辑从这些源中获取的消息。

- --no-edit

  Use the selected commit message without launching an editor. For example, `git commit --amend --no-edit` amends a commit without changing its commit message.

  使用选定的提交消息而不启动编辑器。例如，`git commit --amend --no-edit` 会修改一个提交而不更改其提交消息。

- --amend

  Replace the tip of the current branch by creating a new commit. The recorded tree is prepared as usual (including the effect of the `-i` and `-o` options and explicit pathspec), and the message from the original commit is used as the starting point, instead of an empty message, when no other message is specified from the command line via options such as `-m`, `-F`, `-c`, etc. The new commit has the same parents and author as the current one (the `--reset-author` option can countermand this).It is a rough equivalent for:

  通过创建一个新的提交来替换当前分支的最新提交。记录的树会像往常一样准备好（包括 `-i` 和 `-o` 选项的影响和显式的路径规范），并且使用原始提交的消息作为起点，而不是空消息，当没有通过命令行的其他选项（例如 `-m`，`-F`，`-c` 等）指定其他消息时。新的提交具有与当前提交相同的父提交和作者（`--reset-author` 选项可以取消这一点）。这相当于：

  ```
  $ git reset --soft HEAD^
  $ ... do something else to come up with the right tree ...
  $ git commit -c ORIG_HEAD
  ```

  but can be used to amend a merge commit.You should understand the implications of rewriting history if you amend a commit that has already been published. (See the "RECOVERING FROM UPSTREAM REBASE" section in [git-rebase[1]](../git-rebase).)

  但是可以用于修改合并提交。如果修改已经发布的提交，请理解重写历史的影响（参见 [git-rebase[1]](../git-rebase) 中的 "RECOVERING FROM UPSTREAM REBASE" 部分）。

- --no-post-rewrite

  Bypass the post-rewrite hook.

  绕过 post-rewrite 钩子。

- -i

- --include

  Before making a commit out of staged contents so far, stage the contents of paths given on the command line as well. This is usually not what you want unless you are concluding a conflicted merge.

  在将暂存内容提交为提交之前，也将命令行上指定的路径的内容暂存起来。除非你正在结束一个存在冲突的合并，否则通常不需要这样做。

- -o

- --only

  Make a commit by taking the updated working tree contents of the paths specified on the command line, disregarding any contents that have been staged for other paths. This is the default mode of operation of *git commit* if any paths are given on the command line, in which case this option can be omitted. If this option is specified together with `--amend`, then no paths need to be specified, which can be used to amend the last commit without committing changes that have already been staged. If used together with `--allow-empty` paths are also not required, and an empty commit will be created.

  通过获取命令行上指定的路径的已更新的工作树内容进行提交，而忽略已为其他路径暂存的任何内容。这是 *git commit* 的默认操作模式，如果命令行上给定了任何路径，则可以省略此选项。如果此选项与 `--amend` 一起使用，则无需指定路径，这可用于修改最后一次提交而不提交已经暂存的更改。如果与 `--allow-empty` 选项一起使用，则也不需要路径，将创建一个空提交。

- --pathspec-from-file=<file>

  Pathspec is passed in `<file>` instead of commandline args. If `<file>` is exactly `-` then standard input is used. Pathspec elements are separated by LF or CR/LF. Pathspec elements can be quoted as explained for the configuration variable `core.quotePath` (see [git-config[1]](../git-config)). See also `--pathspec-file-nul` and global `--literal-pathspecs`.

  将路径规范传递给 `<file>`，而不是命令行参数。如果 `<file>` 正好是 `-`，则使用标准输入。路径规范元素由 LF 或 CR/LF 分隔。路径规范元素可以像配置变量 `core.quotePath`（参见 [git-config[1]](../git-config)）中所解释的那样进行引用。还请参阅 `--pathspec-file-nul` 和全局 `--literal-pathspecs`。

- --pathspec-file-nul

  Only meaningful with `--pathspec-from-file`. Pathspec elements are separated with NUL character and all other characters are taken literally (including newlines and quotes).

  仅在 `--pathspec-from-file` 中具有意义。路径规范元素以 NUL 字符分隔，所有其他字符均按原样处理（包括换行符和引号）。

- -u[<mode>]

- --untracked-files[=<mode>]

  Show untracked files.

  显示未跟踪的文件。

  The mode parameter is optional (defaults to *all*), and is used to specify the handling of untracked files; when -u is not used, the default is *normal*, i.e. show untracked files and directories.

  模式参数是可选的（默认为 `all`），用于指定对未跟踪文件的处理方式。当未使用 -u 时，默认值为 `normal`，即显示未跟踪的文件和目录。

  The possible options are:

  可能的选项有：

  - *no* - Show no untracked files
  - *normal* - Shows untracked files and directories
  - *all* - Also shows individual files in untracked directories.
  - `no` - 不显示未跟踪的文件
  - `normal` - 显示未跟踪的文件和目录
  - `all` - 还显示未跟踪目录中的各个文件。

  The default can be changed using the status.showUntrackedFiles configuration variable documented in [git-config[1]](../git-config).

  默认值可以通过 status.showUntrackedFiles 配置变量（参见 [git-config[1]](../git-config)）进行更改。

- -v

- --verbose

  Show unified diff between the HEAD commit and what would be committed at the bottom of the commit message template to help the user describe the commit by reminding what changes the commit has. Note that this diff output doesn’t have its lines prefixed with *#*. This diff will not be a part of the commit message. See the `commit.verbose` configuration variable in [git-config[1]](../git-config).If specified twice, show in addition the unified diff between what would be committed and the worktree files, i.e. the unstaged changes to tracked files.

  在提交消息模板底部显示 HEAD 提交与将要提交的内容之间的统一差异，以帮助用户描述提交，提醒提交所做的更改。请注意，此差异输出不会在每行前面添加 *#* 字符。此差异不会成为提交消息的一部分。有关详细信息，请参阅 [git-config[1]](../git-config) 中的 commit.verbose 配置变量。如果指定两次，还会显示将要提交的内容与工作树文件之间的统一差异，即已跟踪文件的未暂存更改。

- -q

- --quiet

  Suppress commit summary message.

  禁止提交的摘要消息。

- --dry-run

  Do not create a commit, but show a list of paths that are to be committed, paths with local changes that will be left uncommitted and paths that are untracked.

  不创建提交，而是显示要提交的路径列表，保留未提交的本地更改以及未跟踪的路径列表。

- --status

  Include the output of [git-status[1]](../git-status) in the commit message template when using an editor to prepare the commit message. Defaults to on, but can be used to override configuration variable commit.status.

  在使用编辑器准备提交消息时，将 [git-status[1]](../git-status) 的输出包含在提交消息模板中。默认为开启，但可以用于覆盖配置变量 commit.status。

- --no-status

  Do not include the output of [git-status[1]](../git-status) in the commit message template when using an editor to prepare the default commit message.

  在使用编辑器准备默认提交消息时，不包含 [git-status[1]](../git-status) 的输出。

- `-S[<keyid>]`

- `--gpg-sign[=<keyid>]`

- --no-gpg-sign

  GPG-sign commits. The `keyid` argument is optional and defaults to the committer identity; if specified, it must be stuck to the option without a space. `--no-gpg-sign` is useful to countermand both `commit.gpgSign` configuration variable, and earlier `--gpg-sign`.

  对提交进行 GPG 签名。keyid 参数是可选的，默认为提交者的身份；如果指定，则必须将其与选项粘贴在一起，不要有空格。`--no-gpg-sign` 用于同时取消 `commit.gpgSign` 配置变量和之前的 `--gpg-sign`。

- `--`

  Do not interpret any more arguments as options.

  不再将任何参数解释为选项。

- `<pathspec>…`

  When pathspec is given on the command line, commit the contents of the files that match the pathspec without recording the changes already added to the index. The contents of these files are also staged for the next commit on top of what have been staged before.For more details, see the *pathspec* entry in [gitglossary[7]](../../7/gitglossary).
  
  当在命令行上给出路径规范时，提交匹配路径规范的文件内容，而不记录已添加到索引中的更改。这些文件的内容也将在已暂存的内容之上为下一次提交暂存。有关详细信息，请参阅 [gitglossary[7]](../7/gitglossary) 中的 *pathspec* 条目。

## 示例

When recording your own work, the contents of modified files in your working tree are temporarily stored to a staging area called the "index" with *git add*. A file can be reverted back, only in the index but not in the working tree, to that of the last commit with `git restore --staged <file>`, which effectively reverts *git add* and prevents the changes to this file from participating in the next commit. After building the state to be committed incrementally with these commands, `git commit` (without any pathname parameter) is used to record what has been staged so far. This is the most basic form of the command. An example:

​	在记录您自己的工作时，您在工作树中修改的文件的内容会暂时存储到称为 "index" 的暂存区域中，使用 *git add*。可以将文件还原回最后一次提交的状态，只在索引中还原而不在工作树中还原，使用 `git restore --staged <file>`，这将有效地撤消 *git add*，防止这个文件的更改参与到下一次提交中。通过使用这些命令逐步构建要提交的状态，`git commit`（没有任何路径名参数）用于记录到目前为止暂存的内容。这是该命令的最基本形式。例如：

``` bash
$ edit hello.c
$ git rm goodbye.c
$ git add hello.c
$ git commit
```

Instead of staging files after each individual change, you can tell `git commit` to notice the changes to the files whose contents are tracked in your working tree and do corresponding `git add` and `git rm` for you. That is, this example does the same as the earlier example if there is no other change in your working tree:

​	与每个单独更改后都暂存文件不同，您可以告诉 `git commit` 注意那些在工作树中跟踪的文件的更改，并为您执行相应的 `git add` 和 `git rm`。也就是说，如果工作树中没有其他更改，则此示例与之前的示例相同：

``` bash
$ edit hello.c
$ rm goodbye.c
$ git commit -a
```

The command `git commit -a` first looks at your working tree, notices that you have modified hello.c and removed goodbye.c, and performs necessary `git add` and `git rm` for you.

​	命令 `git commit -a` 首先查看您的工作树，注意到您修改了 hello.c 并删除了 goodbye.c，然后为您执行必要的 `git add` 和 `git rm`。

After staging changes to many files, you can alter the order the changes are recorded in, by giving pathnames to `git commit`. When pathnames are given, the command makes a commit that only records the changes made to the named paths:

​	在对许多文件进行更改后，您可以通过在 `git commit` 中提供路径名来更改记录更改的顺序。当给定路径名时，该命令将创建一个仅记录对命名路径所做更改的提交：

``` bash
$ edit hello.c hello.h
$ git add hello.c hello.h
$ edit Makefile
$ git commit Makefile
```

This makes a commit that records the modification to `Makefile`. The changes staged for `hello.c` and `hello.h` are not included in the resulting commit. However, their changes are not lost — they are still staged and merely held back. After the above sequence, if you do:

​	这将创建一个记录对 `Makefile` 的修改的提交。暂存的 `hello.c` 和 `hello.h` 的更改不包含在结果提交中。然而，它们的更改并没有丢失-它们仍然被暂存，并仅是被拖回。在上述序列之后，如果执行：

``` bash
$ git commit
```

this second commit would record the changes to `hello.c` and `hello.h` as expected.

这第二个提交会如预期地记录对 `hello.c` 和 `hello.h` 的更改。

After a merge (initiated by *git merge* or *git pull*) stops because of conflicts, cleanly merged paths are already staged to be committed for you, and paths that conflicted are left in unmerged state. You would have to first check which paths are conflicting with *git status* and after fixing them manually in your working tree, you would stage the result as usual with *git add*:

​	在合并后（由 *git merge* 或 *git pull* 启动）因为冲突而停止之后，已经干净地合并的路径已经被暂存以待提交，而有冲突的路径处于未合并状态。您需要首先通过 *git status* 检查哪些路径与之冲突，并在工作树中手动修复它们后，再像平常一样使用 *git add* 暂存结果：

``` bash
$ git status | grep unmerged
unmerged: hello.c
$ edit hello.c
$ git add hello.c
```

After resolving conflicts and staging the result, `git ls-files -u` would stop mentioning the conflicted path. When you are done, run `git commit` to finally record the merge:

​	在解决冲突并暂存结果之后，`git ls-files -u` 将不再提及有冲突的路径。完成后，运行 `git commit` 最终记录合并：

``` bash
$ git commit
```

As with the case to record your own changes, you can use `-a` option to save typing. One difference is that during a merge resolution, you cannot use `git commit` with pathnames to alter the order the changes are committed, because the merge should be recorded as a single commit. In fact, the command refuses to run when given pathnames (but see `-i` option).

​	与记录自己的更改的情况一样，可以使用 `-a` 选项来节省输入。一个区别是，在合并解决期间，不能使用带有路径名的 `git commit` 来改变记录更改的顺序，因为合并应该作为一个单一的提交记录。实际上，当给定路径名时，该命令拒绝运行（但请参阅 `-i` 选项）。

## 提交信息

Author and committer information is taken from the following environment variables, if set:

​	作者和提交者信息取自以下环境变量（如果已设置）：

```
GIT_AUTHOR_NAME
GIT_AUTHOR_EMAIL
GIT_AUTHOR_DATE
GIT_COMMITTER_NAME
GIT_COMMITTER_EMAIL
GIT_COMMITTER_DATE
```

(nb "<", ">" and "\n"s are stripped)

（nb "<"、">" 和 "\n" 被删除）

The author and committer names are by convention some form of a personal name (that is, the name by which other humans refer to you), although Git does not enforce or require any particular form. Arbitrary Unicode may be used, subject to the constraints listed above. This name has no effect on authentication; for that, see the `credential.username` variable in [git-config[1]](../git-config).

​	作者和提交者的名称通常是某种形式的个人名称（即其他人用来称呼您的名称），尽管 Git 不强制或要求任何特定的形式。可以使用任意 Unicode，但要遵守上述约束。该名称对身份验证没有影响；有关此问题，请参见 [git-config[1]](../git-config) 中的 `credential.username` 变量。

In case (some of) these environment variables are not set, the information is taken from the configuration items `user.name` and `user.email`, or, if not present, the environment variable EMAIL, or, if that is not set, system user name and the hostname used for outgoing mail (taken from `/etc/mailname` and falling back to the fully qualified hostname when that file does not exist).

​	如果（其中一些）这些环境变量没有设置，信息将取自配置项 `user.name` 和 `user.email`，或者如果没有设置，则使用环境变量 EMAIL，或者如果未设置该变量，则使用系统用户名和用于传出邮件的主机名（从 `/etc/mailname` 获取，并在该文件不存在时回退到完全限定的主机名）。

The `author.name` and `committer.name` and their corresponding email options override `user.name` and `user.email` if set and are overridden themselves by the environment variables.

​	如果设置了 `author.name` 和 `committer.name` 及其对应的电子邮件选项，则它们将覆盖 `user.name` 和 `user.email`，如果设置了它们，则会被环境变量覆盖。

The typical usage is to set just the `user.name` and `user.email` variables; the other options are provided for more complex use cases.

​	通常情况下，只设置 `user.name` 和 `user.email` 变量即可；其他选项提供了更复杂的用例。

## 日期格式

The `GIT_AUTHOR_DATE` and `GIT_COMMITTER_DATE` environment variables support the following date formats:

​	`GIT_AUTHOR_DATE` 和 `GIT_COMMITTER_DATE` 环境变量支持以下日期格式：

- Git internal format

- Git 内部格式

  It is `<unix-timestamp> <time-zone-offset>`, where `<unix-timestamp>` is the number of seconds since the UNIX epoch. `<time-zone-offset>` is a positive or negative offset from UTC. For example CET (which is 1 hour ahead of UTC) is `+0100`.

  它是 `<unix-timestamp> <time-zone-offset>`，其中 `<unix-timestamp>` 是自 UNIX 纪元以来的秒数。`<time-zone-offset>` 是相对于 UTC 的正或负偏移。例如，CET（比 UTC 提前 1 小时）为 `+0100`。

- RFC 2822

  The standard email format as described by RFC 2822, for example `Thu, 07 Apr 2005 22:13:13 +0200`.

  由 RFC 2822 描述的标准电子邮件格式，例如 `Thu, 07 Apr 2005 22:13:13 +0200`。

- ISO 8601

  Time and date specified by the ISO 8601 standard, for example `2005-04-07T22:13:13`. The parser accepts a space instead of the `T` character as well. Fractional parts of a second will be ignored, for example `2005-04-07T22:13:13.019` will be treated as `2005-04-07T22:13:13`.NoteIn addition, the date part is accepted in the following formats: `YYYY.MM.DD`, `MM/DD/YYYY` and `DD.MM.YYYY`.
  
  ISO 8601 标准指定的时间和日期，例如 `2005-04-07T22:13:13`。解析器还接受空格作为 `T` 字符的替代。秒的小数部分将被忽略，例如 `2005-04-07T22:13:13.019` 将被视为 `2005-04-07T22:13:13`。注意，还接受以下格式的日期部分：`YYYY.MM.DD`、`MM/DD/YYYY` 和 `DD.MM.YYYY`。

In addition to recognizing all date formats above, the `--date` option will also try to make sense of other, more human-centric date formats, such as relative dates like "yesterday" or "last Friday at noon".

​	除了识别上述所有日期格式，`--date` 选项还会尝试理解其他更加人性化的日期格式，例如 "yesterday" 或 "last Friday at noon"。

## 讨论

Though not required, it’s a good idea to begin the commit message with a single short (less than 50 character) line summarizing the change, followed by a blank line and then a more thorough description. The text up to the first blank line in a commit message is treated as the commit title, and that title is used throughout Git. For example, [git-format-patch[1]](../git-format-patch) turns a commit into email, and it uses the title on the Subject line and the rest of the commit in the body.

​	虽然不是必需的，但最好以一个单独的简短（少于 50 个字符）的行开始提交消息，概述更改内容，然后是一个空行，然后是更详细的描述。提交消息中第一个空行之前的文本被视为提交标题，并在 Git 中使用此标题。例如，[git-format-patch[1]](../git-format-patch) 将提交转换为电子邮件时，将使用标题作为主题行，将提交的其余部分作为正文。

Git is to some extent character encoding agnostic.

​	在某种程度上，Git 是字符编码不可知的。

- The contents of the blob objects are uninterpreted sequences of bytes. There is no encoding translation at the core level.

- 对象 blob 的内容是未经解释的字节序列。在核心级别没有编码转换。

- Path names are encoded in UTF-8 normalization form C. This applies to tree objects, the index file, ref names, as well as path names in command line arguments, environment variables and config files (`.git/config` (see [git-config[1]](../git-config)), [gitignore[5]](../../5/gitignore), [gitattributes[5]](../../5/gitattributes) and [gitmodules[5]](../../5/gitmodules)).

- 路径名以 UTF-8 归一化形式 C 进行编码。这适用于树对象、索引文件、引用名称以及命令行参数、环境变量和配置文件（`.git/config`，参见 [git-config[1]](../git-config)，[gitignore[5]](../5/gitignore)，[gitattributes[5]](../5/gitattributes) 和 [gitmodules[5]](../5/gitmodules)）中的路径名。

  Note that Git at the core level treats path names simply as sequences of non-NUL bytes, there are no path name encoding conversions (except on Mac and Windows). Therefore, using non-ASCII path names will mostly work even on platforms and file systems that use legacy extended ASCII encodings. However, repositories created on such systems will not work properly on UTF-8-based systems (e.g. Linux, Mac, Windows) and vice versa. Additionally, many Git-based tools simply assume path names to be UTF-8 and will fail to display other encodings correctly.

  请注意，Git 在核心级别将路径名简单地视为非 NUL 字节的序列，没有路径名编码转换（在 Mac 和 Windows 上除外）。因此，即使在使用遗留扩展 ASCII 编码的平台和文件系统上使用非 ASCII 路径名也基本上能正常工作。然而，在这些系统上创建的存储库将在基于 UTF-8 的系统上（如 Linux、Mac、Windows）和反之亦然上无法正常工作。此外，许多基于 Git 的工具简单地假设路径名是 UTF-8 编码，并且不能正确显示其他编码。

- Commit log messages are typically encoded in UTF-8, but other extended ASCII encodings are also supported. This includes ISO-8859-x, CP125x and many others, but *not* UTF-16/32, EBCDIC and CJK multi-byte encodings (GBK, Shift-JIS, Big5, EUC-x, CP9xx etc.).

- 提交日志消息通常使用 UTF-8 编码，但也支持其他扩展 ASCII 编码。包括 ISO-8859-x、CP125x 和许多其他编码，但不支持 UTF-16/32、EBCDIC 和 CJK 多字节编码（GBK、Shift-JIS、Big5、EUC-x、CP9xx 等）。

Although we encourage that the commit log messages are encoded in UTF-8, both the core and Git Porcelain are designed not to force UTF-8 on projects. If all participants of a particular project find it more convenient to use legacy encodings, Git does not forbid it. However, there are a few things to keep in mind.

​	尽管我们鼓励提交日志消息以 UTF-8 编码，但核心和 Git 命令工具设计为不对项目强制使用 UTF-8。如果某个特定项目的所有参与者发现使用传统编码更方便，则 Git 不禁止这样做。然而，需要记住一些事项。

1. *git commit* and *git commit-tree* issues a warning if the commit log message given to it does not look like a valid UTF-8 string, unless you explicitly say your project uses a legacy encoding. The way to say this is to have `i18n.commitEncoding` in `.git/config` file, like this:

2. *git commit* 和 *git commit-tree* 在提交消息看起来不像有效的 UTF-8 字符串时发出警告，除非您显式表示您的项目使用传统编码。表示的方法是在 `.git/config` 文件中有 `i18n.commitEncoding`，例如：

   ```
   [i18n]
   	commitEncoding = ISO-8859-1
   ```

   Commit objects created with the above setting record the value of `i18n.commitEncoding` in its `encoding` header. This is to help other people who look at them later. Lack of this header implies that the commit log message is encoded in UTF-8.

   使用上述设置创建的提交对象会在其 `encoding` 标头中记录 `i18n.commitEncoding` 的值。这是为了帮助稍后查看它们的其他人。缺少此标头意味着提交日志消息已用 UTF-8 编码。

3. *git log*, *git show*, *git blame* and friends look at the `encoding` header of a commit object, and try to re-code the log message into UTF-8 unless otherwise specified. You can specify the desired output encoding with `i18n.logOutputEncoding` in `.git/config` file, like this:

4. *git log*、*git show*、*git blame* 等会查看提交对象的 `encoding` 标头，并尝试重新编码日志消息为 UTF-8，除非另有指定。您可以通过 `i18n.logOutputEncoding` 在 `.git/config` 文件中指定所需的输出编码，例如：

   ```
   [i18n]
   	logOutputEncoding = ISO-8859-1
   ```

   If you do not have this configuration variable, the value of `i18n.commitEncoding` is used instead.

   如果没有此配置变量，将使用 `i18n.commitEncoding` 的值。

Note that we deliberately chose not to re-code the commit log message when a commit is made to force UTF-8 at the commit object level, because re-coding to UTF-8 is not necessarily a reversible operation.

​	请注意，我们故意选择不在进行提交时重新编码提交日志消息，以便在提交对象级别强制使用 UTF-8，因为重新编码为 UTF-8 不一定是可逆操作。

## 环境变量和配置变量

The editor used to edit the commit log message will be chosen from the `GIT_EDITOR` environment variable, the core.editor configuration variable, the `VISUAL` environment variable, or the `EDITOR` environment variable (in that order). See [git-var[1]](../git-var) for details.

​	用于编辑提交日志消息的编辑器将按以下顺序选择：`GIT_EDITOR` 环境变量、core.editor 配置变量、`VISUAL` 环境变量或 `EDITOR` 环境变量。有关详细信息，请参见 [git-var[1]](../git-var)。

Everything above this line in this section isn’t included from the [git-config[1]](../git-config) documentation. The content that follows is the same as what’s found there:

​	在本节的此行上方的所有内容不包括在 [git-config[1]](../git-config) 文档中。接下来的内容与该文档中的内容相同：

- commit.cleanup

  This setting overrides the default of the `--cleanup` option in `git commit`. See [git-commit[1]](../git-commit) for details. Changing the default can be useful when you always want to keep lines that begin with comment character `#` in your log message, in which case you would do `git config commit.cleanup whitespace` (note that you will have to remove the help lines that begin with `#` in the commit log template yourself, if you do this).

  此设置覆盖 `git commit` 的 `--cleanup` 选项的默认值。有关详细信息，请参见 [git-commit[1]](../git-commit)。更改默认值在您始终希望保留提交日志消息中以 `#` 字符开头的行时很有用，如果是这样，您可以执行 `git config commit.cleanup whitespace`（请注意，如果您这样做，您将需要自行删除提交日志模板中以 `#` 开头的帮助行）。

- commit.gpgSign

  A boolean to specify whether all commits should be GPG signed. Use of this option when doing operations such as rebase can result in a large number of commits being signed. It may be convenient to use an agent to avoid typing your GPG passphrase several times.

  一个布尔值，用于指定是否对所有提交进行 GPG 签名。在执行重新基础等操作时使用此选项可能导致大量提交被签名。使用代理可能方便，以避免多次输入 GPG 密码。

- commit.status

  A boolean to enable/disable inclusion of status information in the commit message template when using an editor to prepare the commit message. Defaults to true.

  一个布尔值，用于启用/禁用在使用编辑器准备提交消息时，在提交消息模板中包含状态信息。默认值为 true。

- commit.template

  Specify the pathname of a file to use as the template for new commit messages.

  指定用作新提交消息模板的文件的路径名。

- commit.verbose

  A boolean or int to specify the level of verbose with `git commit`. See [git-commit[1]](../git-commit).
  
  一个布尔值或整数，用于指定 `git commit` 的详细级别。请参阅 [git-commit[1]](../git-commit)。

## 钩子

This command can run `commit-msg`, `prepare-commit-msg`, `pre-commit`, `post-commit` and `post-rewrite` hooks. See [githooks[5]](../../5/githooks) for more information.

​	此命令可以运行 `commit-msg`、`prepare-commit-msg`、`pre-commit`、`post-commit` 和 `post-rewrite` 钩子。有关更多信息，请参见 [githooks[5]](../5/githooks)。

## 文件

- `$GIT_DIR/COMMIT_EDITMSG`

  This file contains the commit message of a commit in progress. If `git commit` exits due to an error before creating a commit, any commit message that has been provided by the user (e.g., in an editor session) will be available in this file, but will be overwritten by the next invocation of `git commit`.
  
  此文件包含正在进行中的提交的提交消息。如果 `git commit` 在创建提交之前由于错误而退出，用户提供的任何提交消息（例如，在编辑器会话中）将保存在此文件中，但会被下一次 `git commit` 的调用覆盖。

## 另请参阅

[git-add[1]](../git-add), [git-rm[1]](../git-rm), [git-mv[1]](../git-mv), [git-merge[1]](../git-merge), [git-commit-tree[1]](../git-commit-tree)

## GIT

  这是[git[1]](../../Git)工具集中的一部分。