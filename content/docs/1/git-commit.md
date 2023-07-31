+++
title = "git-commit"
weight = 30
type = "docs"
date = 2023-05-08T15:39:23+08:00
description = ""
isCJKLanguage = true
draft = false

+++

# git-commit

https://git-scm.com/docs/git-commit

version 2.41.0

## 名称

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

​	创建一个新的提交，包含当前索引（即暂存区）的内容以及给定的日志消息，用于描述这些更改。新的提交是 HEAD 的直接子提交，通常是当前分支的最新提交点，并且分支会更新为指向该提交（除非工作树没有关联的分支，此时 HEAD 将会“分离”，如 [git-checkout[1]](../git-checkout) 中所述）。

​	要提交的内容可以通过多种方式指定：

1. 使用 [git-add[1]](../git-add) 逐步地在使用 *commit* 命令之前“添加”更改到索引中（注意：即使修改文件也必须“添加”）；
2. 使用 [git-rm[1]](../git-rm) 在使用 *commit* 命令之前从工作树和索引中移除文件；
3. 将文件列为 *commit* 命令的参数（不带 --interactive 或 --patch 开关），在这种情况下，提交将忽略索引中暂存的更改，并记录列出文件的当前内容（这些文件必须已经被 Git 知道）；
4. 使用 *commit* 命令的 -a 开关，自动地“添加”所有已知文件的更改（即索引中已经列出的所有文件），并自动地“删除”索引中已从工作树中删除的文件，然后执行实际的提交；
5. 使用 *commit* 命令的 --interactive 或 --patch 开关，逐个决定哪些文件或文件块应该作为提交的一部分，除了索引中的内容，然后完成操作。有关如何操作这些模式，请参阅 [git-add[1]](../git-add) 中的“交互模式”部分。

​	`--dry-run` 选项可以用于通过使用相同的一组参数（选项和路径）获得下一次提交要包含的内容的摘要。

​	如果您提交后立即发现错误，可以通过 *git reset* 来恢复。

## 选项

- -a

- `--all`

  命令会自动暂存已修改和已删除的文件，但不会影响尚未添加到 Git 中的新文件。

- -p

- `--patch`

  使用交互式补丁选择界面来选择要提交的更改。有关详情，请参阅 [git-add[1]](../git-add)。

- -C <commit>

- `--reuse-message=<commit>`

  使用现有的提交对象，并在创建提交时重用其日志消息和作者信息（包括时间戳）。

- -c <commit>

- `--reedit-message=<commit>`

  类似于 *-C*，但使用 `-c` 选项会调用编辑器，以便用户可以进一步编辑提交消息。

- `--fixup=[(amend|reword):]<commit>`

  创建一个新的提交，用于“修正” `<commit>`，当通过 `git rebase --autosquash` 应用时。简单的 `--fixup=<commit>` 创建一个 "fixup!" 提交，它会更改 `<commit>` 的内容，但保留其日志消息不变。`--fixup=amend:<commit>` 类似，但它创建一个 "amend!" 提交，它还将 `<commit>` 的日志消息替换为 "amend!" 提交的日志消息。

  `--fixup=reword:<commit>` 创建一个 "amend!" 提交，将 `<commit>` 的日志消息替换为它自己的日志消息，但不会更改 `<commit>` 的内容。通过 `--fixup=<commit>` 创建的提交的主题由 "fixup!" 开头，后面跟着 <commit> 的主题行，并且被 `git rebase --autosquash` 特别识别。可以使用 -m 选项来补充创建的提交的日志消息，但是这些附加评论将在 "fixup!" 提交通过 `git rebase --autosquash` 合并到 `<commit>` 时被丢弃。

  通过 `--fixup=amend:<commit>` 创建的提交类似，但其主题的前缀是 "amend!"。将 <commit> 的日志消息复制到 "amend!" 提交的日志消息中，并在编辑器中打开它以便进行优化。当 `git rebase --autosquash` 将 "amend!" 提交合并到 `<commit>` 时，将用 "amend!" 提交的优化日志消息替换 `<commit>` 的日志消息。除非指定了 `--allow-empty-message`，否则 "amend!" 提交的日志消息为空是错误的。

  `--fixup=reword:<commit>` 是 `--fixup=amend:<commit> --only` 的简写形式。它创建一个 "amend!" 提交，只有日志消息（忽略索引中的任何更改）。当通过 `git rebase --autosquash` 进行合并时，它将替换 `<commit>` 的日志消息，而不会做其他更改。

  当通过 `git rebase --autosquash` 应用时，“fixup!” 和 “amend!” 提交不会更改 `<commit>` 的作者信息。有关详情，请参阅 [git-rebase[1]](../git-rebase)。

- `--squash=<commit>`

  为 `rebase --autosquash` 构造提交消息。提交消息的主题行来自指定的提交，并带有 "squash! " 前缀。可以与其他提交消息选项 (`-m`/`-c`/`-C`/`-F`) 一起使用。有关详情，请参阅 [git-rebase[1]](../git-rebase)。

- `--reset-author`

  在使用 -C/-c/--amend 选项或在合并冲突的 cherry-pick 后提交时，声明生成的提交的作者是提交者。这还会更新作者的时间戳。

- `--short`

  在进行干运行时，以简短格式输出。有关详情，请参阅 [git-status[1]](../git-status)。隐含了 `--dry-run`。

- `--branch`

  即使在简短格式中也显示分支和追踪信息。

- `--porcelain`

  在进行干运行时，以瓷器格式输出。有关详情，请参阅 [git-status[1]](../git-status)。隐含了 `--dry-run`。

- `--long`

  在进行干运行时，以长格式输出。隐含了 `--dry-run`。

- -z

- `--null`

  在显示 `short` 或 `porcelain` 状态输出时，直接打印文件名，并以 NUL 终止条目，而不是 LF。如果没有给定格式，则隐含了 `--porcelain` 输出格式。如果没有使用 `-z` 选项，则对于包含 "非常规" 字符的文件名，将对其进行引用，如配置变量 `core.quotePath`（参见 [git-config[1]](../git-config)）中所解释的。

- -F <file>

- `--file=<file>`

  使用给定文件中的内容作为提交消息。使用 *-* 从标准输入读取消息。

- `--author=<author>`

  覆盖提交作者。使用标准的 `A U Thor <author@example.com>` 格式指定明确的作者。否则，将假设 <author> 是一个模式，并使用该作者搜索现有的提交（即 rev-list --all -i --author=<author>）；然后将提交作者复制自第一个找到的提交。

- `--date=<date>`

  覆盖提交中使用的作者日期。

- -m <msg>

- `--message=<msg>`

  将给定的 <msg> 作为提交消息。如果给定了多个 `-m` 选项，则它们的值将作为单独的段落连接。`-m` 选项与 `-c`、`-C` 和 `-F` 选项互斥。

- -t <file>

- `--template=<file>`

  编辑提交消息时，使用给定文件中的内容启动编辑器。`commit.template` 配置变量通常用于隐含地给命令提供此选项。该机制可用于希望向参与者提供一些有关按照什么顺序编写消息的提示的项目。如果用户退出编辑器而未编辑消息，则提交将被中止。当通过其他方式给出消息时（例如使用 `-m` 或 `-F` 选项），此选项无效。

- -s

- `--signoff`

- `--no-signoff`

  在提交日志消息的末尾，由提交者添加 `Signed-off-by` 尾注。签名的含义取决于您要提交的项目。例如，它可以证明提交者在项目的许可证下有权提交工作，或者同意某些贡献者代表，例如开发者证书。 （有关 Linux 内核和 Git 项目使用的签名，请参阅 [http://developercertificate.org](http://developercertificate.org/)）。请咨询您要贡献的项目的文档或领导层，了解在该项目中如何使用签名。`--no-signoff` 选项可以用于抵消命令行上之前使用的 `--signoff` 选项。

- --trailer <token>[(=|:)<value>]

  指定应作为尾注应用的（<token>，<value>）对。 （例如，`git commit --trailer "Signed-off-by:C O Mitter \ <committer@example.com>" --trailer "Helped-by:C O Mitter \ <committer@example.com>"` 将向提交消息添加 "Signed-off-by" 尾注和 "Helped-by" 尾注）。 `trailer.*` 配置变量（参见 [git-interpret-trailers[1]](../git-interpret-trailers)）可用于定义是否省略重复的尾注，每个尾注在尾注运行中出现的位置，以及其他详细信息。

- -n

- `--[no-]verify`

  默认情况下，会运行预提交和提交消息挂钩。给出 `--no-verify` 或 `-n` 中的任何选项时，这些挂钩将被绕过。另请参阅 [githooks[5]](../5/githooks)。

- `--allow-empty`

  通常，记录与其唯一父提交具有完全相同的树的提交是错误的，命令会阻止您进行此类提交。此选项绕过了这种安全性，主要供外部 SCM 接口脚本使用。

- `--allow-empty-message`

  与 --allow-empty 一样，此命令主要供外部 SCM 接口脚本使用。它允许您创建一个没有使用像 [git-commit-tree[1]](../git-commit-tree) 等工具的空提交消息的提交。

- `--cleanup=<mode>`

  此选项确定在提交之前如何清理提供的提交消息。*<mode>* 可以是 `strip`、`whitespace`、`verbatim`、`scissors` 或 `default`。

  - strip

    去除前导和尾随空行，尾随空白，注释，并折叠连续的空行。

  - whitespace

    与 `strip` 相同，但不会移除 # 注释。

  - verbatim

    不对消息进行任何更改。

  - scissors

    与 `whitespace` 相同，但是一旦编辑消息，就会截断以下发现的行，即从（包括）下面找到的行开始，如果消息要进行编辑。 "`#`" 可以通过 core.commentChar 进行自定义。

    ```
    # ------------------------ >8 ------------------------
    ```

    

  - default

    如果消息要进行编辑，则与 `strip` 相同。否则与 `whitespace` 相同。

  默认值可以通过 `commit.cleanup` 配置变量更改（参见 [git-config[1]](../git-config)）。

- -e

- `--edit`

  通常，从 `-F` 文件中获取的消息、命令行中的消息（使用 `-m` 选项）以及提交对象中获取的消息（使用 `-C` 选项）都会原样使用作为提交日志消息。此选项允许您进一步编辑从这些源中获取的消息。

- `--no-edit`

  使用选定的提交消息而不启动编辑器。例如，`git commit --amend --no-edit` 会修改一个提交而不更改其提交消息。

- `--amend`

  通过创建一个新的提交来替换当前分支的最新提交。记录的树会像往常一样准备好（包括 `-i` 和 `-o` 选项的影响和显式的路径规范），并且使用原始提交的消息作为起点，而不是空消息，当没有通过命令行的其他选项（例如 `-m`，`-F`，`-c` 等）指定其他消息时。新的提交具有与当前提交相同的父提交和作者（`--reset-author` 选项可以取消这一点）。这相当于：

  ```
  $ git reset --soft HEAD^
  $ ... do something else to come up with the right tree ...
  $ git commit -c ORIG_HEAD
  ```

  但是可以用于修改合并提交。如果修改已经发布的提交，请理解重写历史的影响（参见 [git-rebase[1]](../git-rebase) 中的 "RECOVERING FROM UPSTREAM REBASE" 部分）。

- `--no-post-rewrite`

  绕过 post-rewrite 钩子。

- -i

- `--include`

  在将暂存内容提交为提交之前，也将命令行上指定的路径的内容暂存起来。除非你正在结束一个存在冲突的合并，否则通常不需要这样做。

- -o

- `--only`

  通过获取命令行上指定的路径的已更新的工作树内容进行提交，而忽略已为其他路径暂存的任何内容。这是 *git commit* 的默认操作模式，如果命令行上给定了任何路径，则可以省略此选项。如果此选项与 `--amend` 一起使用，则无需指定路径，这可用于修改最后一次提交而不提交已经暂存的更改。如果与 `--allow-empty` 选项一起使用，则也不需要路径，将创建一个空提交。

- `--pathspec-from-file=<file>`

  将路径规范传递给 `<file>`，而不是命令行参数。如果 `<file>` 正好是 `-`，则使用标准输入。路径规范元素由 LF 或 CR/LF 分隔。路径规范元素可以像配置变量 `core.quotePath`（参见 [git-config[1]](../git-config)）中所解释的那样进行引用。还请参阅 `--pathspec-file-nul` 和全局 `--literal-pathspecs`。

- `--pathspec-file-nul`

  仅在 `--pathspec-from-file` 中具有意义。路径规范元素以 NUL 字符分隔，所有其他字符均按原样处理（包括换行符和引号）。

- -u[<mode>]

- `--untracked-files[=<mode>]`

  显示未跟踪的文件。

  模式参数是可选的（默认为 `all`），用于指定对未跟踪文件的处理方式。当未使用 -u 时，默认值为 `normal`，即显示未跟踪的文件和目录。

  可能的选项有：

  - `no` - 不显示未跟踪的文件
  - `normal` - 显示未跟踪的文件和目录
  - `all` - 还显示未跟踪目录中的各个文件。

  默认值可以通过 status.showUntrackedFiles 配置变量（参见 [git-config[1]](../git-config)）进行更改。

- -v

- `--verbose`

  在提交消息模板底部显示 HEAD 提交与将要提交的内容之间的统一差异，以帮助用户描述提交，提醒提交所做的更改。请注意，此差异输出不会在每行前面添加 *#* 字符。此差异不会成为提交消息的一部分。有关详细信息，请参阅 [git-config[1]](../git-config) 中的 commit.verbose 配置变量。如果指定两次，还会显示将要提交的内容与工作树文件之间的统一差异，即已跟踪文件的未暂存更改。

- -q

- `--quiet`

  禁止提交的摘要消息。

- `--dry-run`

  不创建提交，而是显示要提交的路径列表，保留未提交的本地更改以及未跟踪的路径列表。

- `--status`

  在使用编辑器准备提交消息时，将 [git-status[1]](../git-status) 的输出包含在提交消息模板中。默认为开启，但可以用于覆盖配置变量 commit.status。

- `--no-status`

  在使用编辑器准备默认提交消息时，不包含 [git-status[1]](../git-status) 的输出。

- `-S[<keyid>]`

- `--gpg-sign[=<keyid>]`

- `--no-gpg-sign`

  对提交进行 GPG 签名。keyid 参数是可选的，默认为提交者的身份；如果指定，则必须将其与选项粘贴在一起，不要有空格。`--no-gpg-sign` 用于同时取消 `commit.gpgSign` 配置变量和之前的 `--gpg-sign`。

- `--`

  不再将任何参数解释为选项。

- `<pathspec>…`

  当在命令行上给出路径规范时，提交匹配路径规范的文件内容，而不记录已添加到索引中的更改。这些文件的内容也将在已暂存的内容之上为下一次提交暂存。有关详细信息，请参阅 [gitglossary[7]](../7/gitglossary) 中的 *pathspec* 条目。

## 示例

​	在记录您自己的工作时，您在工作树中修改的文件的内容会暂时存储到称为 "index" 的暂存区域中，使用 *git add*。可以将文件还原回最后一次提交的状态，只在索引中还原而不在工作树中还原，使用 `git restore --staged <file>`，这将有效地撤消 *git add*，防止这个文件的更改参与到下一次提交中。通过使用这些命令逐步构建要提交的状态，`git commit`（没有任何路径名参数）用于记录到目前为止暂存的内容。这是该命令的最基本形式。例如：

``` bash
$ edit hello.c
$ git rm goodbye.c
$ git add hello.c
$ git commit
```

​	与每个单独更改后都暂存文件不同，您可以告诉 `git commit` 注意那些在工作树中跟踪的文件的更改，并为您执行相应的 `git add` 和 `git rm`。也就是说，如果工作树中没有其他更改，则此示例与之前的示例相同：

``` bash
$ edit hello.c
$ rm goodbye.c
$ git commit -a
```

​	命令 `git commit -a` 首先查看您的工作树，注意到您修改了 hello.c 并删除了 goodbye.c，然后为您执行必要的 `git add` 和 `git rm`。

​	在对许多文件进行更改后，您可以通过在 `git commit` 中提供路径名来更改记录更改的顺序。当给定路径名时，该命令将创建一个仅记录对命名路径所做更改的提交：

``` bash
$ edit hello.c hello.h
$ git add hello.c hello.h
$ edit Makefile
$ git commit Makefile
```

​	这将创建一个记录对 `Makefile` 的修改的提交。暂存的 `hello.c` 和 `hello.h` 的更改不包含在结果提交中。然而，它们的更改并没有丢失-它们仍然被暂存，并仅是被拖回。在上述序列之后，如果执行：

``` bash
$ git commit
```

这第二个提交会如预期地记录对 `hello.c` 和 `hello.h` 的更改。

​	在合并后（由 *git merge* 或 *git pull* 启动）因为冲突而停止之后，已经干净地合并的路径已经被暂存以待提交，而有冲突的路径处于未合并状态。您需要首先通过 *git status* 检查哪些路径与之冲突，并在工作树中手动修复它们后，再像平常一样使用 *git add* 暂存结果：

``` bash
$ git status | grep unmerged
unmerged: hello.c
$ edit hello.c
$ git add hello.c
```

​	在解决冲突并暂存结果之后，`git ls-files -u` 将不再提及有冲突的路径。完成后，运行 `git commit` 最终记录合并：

``` bash
$ git commit
```

​	与记录自己的更改的情况一样，可以使用 `-a` 选项来节省输入。一个区别是，在合并解决期间，不能使用带有路径名的 `git commit` 来改变记录更改的顺序，因为合并应该作为一个单一的提交记录。实际上，当给定路径名时，该命令拒绝运行（但请参阅 `-i` 选项）。

## 提交信息

​	作者和提交者信息取自以下环境变量（如果已设置）：

```
GIT_AUTHOR_NAME
GIT_AUTHOR_EMAIL
GIT_AUTHOR_DATE
GIT_COMMITTER_NAME
GIT_COMMITTER_EMAIL
GIT_COMMITTER_DATE
```

（nb "<"、">" 和 "\n" 被删除）

​	作者和提交者的名称通常是某种形式的个人名称（即其他人用来称呼您的名称），尽管 Git 不强制或要求任何特定的形式。可以使用任意 Unicode，但要遵守上述约束。该名称对身份验证没有影响；有关此问题，请参见 [git-config[1]](../git-config) 中的 `credential.username` 变量。

​	如果（其中一些）这些环境变量没有设置，信息将取自配置项 `user.name` 和 `user.email`，或者如果没有设置，则使用环境变量 EMAIL，或者如果未设置该变量，则使用系统用户名和用于传出邮件的主机名（从 `/etc/mailname` 获取，并在该文件不存在时回退到完全限定的主机名）。

​	如果设置了 `author.name` 和 `committer.name` 及其对应的电子邮件选项，则它们将覆盖 `user.name` 和 `user.email`，如果设置了它们，则会被环境变量覆盖。

​	通常情况下，只设置 `user.name` 和 `user.email` 变量即可；其他选项提供了更复杂的用例。

## 日期格式

​	`GIT_AUTHOR_DATE` 和 `GIT_COMMITTER_DATE` 环境变量支持以下日期格式：

- Git internal format

- Git 内部格式

  它是 `<unix-timestamp> <time-zone-offset>`，其中 `<unix-timestamp>` 是自 UNIX 纪元以来的秒数。`<time-zone-offset>` 是相对于 UTC 的正或负偏移。例如，CET（比 UTC 提前 1 小时）为 `+0100`。

- RFC 2822

  由 RFC 2822 描述的标准电子邮件格式，例如 `Thu, 07 Apr 2005 22:13:13 +0200`。

- ISO 8601

  ISO 8601 标准指定的时间和日期，例如 `2005-04-07T22:13:13`。解析器还接受空格作为 `T` 字符的替代。秒的小数部分将被忽略，例如 `2005-04-07T22:13:13.019` 将被视为 `2005-04-07T22:13:13`。注意，还接受以下格式的日期部分：`YYYY.MM.DD`、`MM/DD/YYYY` 和 `DD.MM.YYYY`。

​	除了识别上述所有日期格式，`--date` 选项还会尝试理解其他更加人性化的日期格式，例如 "yesterday" 或 "last Friday at noon"。

## 讨论

​	虽然不是必需的，但最好以一个单独的简短（少于 50 个字符）的行开始提交消息，概述更改内容，然后是一个空行，然后是更详细的描述。提交消息中第一个空行之前的文本被视为提交标题，并在 Git 中使用此标题。例如，[git-format-patch[1]](../git-format-patch) 将提交转换为电子邮件时，将使用标题作为主题行，将提交的其余部分作为正文。

Git is to some extent character encoding agnostic.

​	在某种程度上，Git 是字符编码不可知的。

- 对象 blob 的内容是未经解释的字节序列。在核心级别没有编码转换。

- 路径名以 UTF-8 归一化形式 C 进行编码。这适用于树对象、索引文件、引用名称以及命令行参数、环境变量和配置文件（`.git/config`，参见 [git-config[1]](../git-config)，[gitignore[5]](../5/gitignore)，[gitattributes[5]](../5/gitattributes) 和 [gitmodules[5]](../5/gitmodules)）中的路径名。

  请注意，Git 在核心级别将路径名简单地视为非 NUL 字节的序列，没有路径名编码转换（在 Mac 和 Windows 上除外）。因此，即使在使用遗留扩展 ASCII 编码的平台和文件系统上使用非 ASCII 路径名也基本上能正常工作。然而，在这些系统上创建的存储库将在基于 UTF-8 的系统上（如 Linux、Mac、Windows）和反之亦然上无法正常工作。此外，许多基于 Git 的工具简单地假设路径名是 UTF-8 编码，并且不能正确显示其他编码。

- 提交日志消息通常使用 UTF-8 编码，但也支持其他扩展 ASCII 编码。包括 ISO-8859-x、CP125x 和许多其他编码，但不支持 UTF-16/32、EBCDIC 和 CJK 多字节编码（GBK、Shift-JIS、Big5、EUC-x、CP9xx 等）。


​	尽管我们鼓励提交日志消息以 UTF-8 编码，但核心和 Git 命令工具设计为不对项目强制使用 UTF-8。如果某个特定项目的所有参与者发现使用传统编码更方便，则 Git 不禁止这样做。然而，需要记住一些事项。

1. *git commit* 和 *git commit-tree* 在提交消息看起来不像有效的 UTF-8 字符串时发出警告，除非您显式表示您的项目使用传统编码。表示的方法是在 `.git/config` 文件中有 `i18n.commitEncoding`，例如：

   ```
   [i18n]
   	commitEncoding = ISO-8859-1
   ```

   使用上述设置创建的提交对象会在其 `encoding` 标头中记录 `i18n.commitEncoding` 的值。这是为了帮助稍后查看它们的其他人。缺少此标头意味着提交日志消息已用 UTF-8 编码。

2. *git log*、*git show*、*git blame* 等会查看提交对象的 `encoding` 标头，并尝试重新编码日志消息为 UTF-8，除非另有指定。您可以通过 `i18n.logOutputEncoding` 在 `.git/config` 文件中指定所需的输出编码，例如：

   ```
   [i18n]
   	logOutputEncoding = ISO-8859-1
   ```

   如果没有此配置变量，将使用 `i18n.commitEncoding` 的值。


​	请注意，我们故意选择不在进行提交时重新编码提交日志消息，以便在提交对象级别强制使用 UTF-8，因为重新编码为 UTF-8 不一定是可逆操作。

## 环境变量和配置变量

​	用于编辑提交日志消息的编辑器将按以下顺序选择：`GIT_EDITOR` 环境变量、core.editor 配置变量、`VISUAL` 环境变量或 `EDITOR` 环境变量。有关详细信息，请参见 [git-var[1]](../git-var)。

​	在本节的此行上方的所有内容不包括在 [git-config[1]](../git-config) 文档中。接下来的内容与该文档中的内容相同：

- commit.cleanup

  此设置覆盖 `git commit` 的 `--cleanup` 选项的默认值。有关详细信息，请参见 [git-commit[1]](../git-commit)。更改默认值在您始终希望保留提交日志消息中以 `#` 字符开头的行时很有用，如果是这样，您可以执行 `git config commit.cleanup whitespace`（请注意，如果您这样做，您将需要自行删除提交日志模板中以 `#` 开头的帮助行）。

- commit.gpgSign

  一个布尔值，用于指定是否对所有提交进行 GPG 签名。在执行重新基础等操作时使用此选项可能导致大量提交被签名。使用代理可能方便，以避免多次输入 GPG 密码。

- commit.status

  一个布尔值，用于启用/禁用在使用编辑器准备提交消息时，在提交消息模板中包含状态信息。默认值为 true。

- commit.template

  指定用作新提交消息模板的文件的路径名。

- commit.verbose

  一个布尔值或整数，用于指定 `git commit` 的详细级别。请参阅 [git-commit[1]](../git-commit)。

## 钩子

​	此命令可以运行 `commit-msg`、`prepare-commit-msg`、`pre-commit`、`post-commit` 和 `post-rewrite` 钩子。有关更多信息，请参见 [githooks[5]](../5/githooks)。

## 文件

- `$GIT_DIR/COMMIT_EDITMSG`

  此文件包含正在进行中的提交的提交消息。如果 `git commit` 在创建提交之前由于错误而退出，用户提供的任何提交消息（例如，在编辑器会话中）将保存在此文件中，但会被下一次 `git commit` 的调用覆盖。

## 另请参阅

[git-add[1]](../git-add), [git-rm[1]](../git-rm), [git-mv[1]](../git-mv), [git-merge[1]](../git-merge), [git-commit-tree[1]](../git-commit-tree)

## GIT

  这是[git[1]](../../Git)工具集中的一部分。