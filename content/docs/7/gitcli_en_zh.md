+++
title = "gitcli——中英对照版"
weight = 30
type = "docs"
date = 2023-05-08T15:39:23+08:00
description = ""
isCJKLanguage = true
draft = false

+++

# gitcli

https://git-scm.com/docs/gitcli

version 2.41.0

## 名称

gitcli - Git command-line interface and conventions

​	gitcli - Git 命令行界面与约定

## 概要

gitcli

## 描述

This manual describes the convention used throughout Git CLI.

​	本手册描述了在 Git 命令行界面中使用的约定。

Many commands take revisions (most often "commits", but sometimes "tree-ish", depending on the context and command) and paths as their arguments. Here are the rules:

​	许多命令以修订版本（通常是"提交"，但有时也可能是"tree-ish"，具体取决于上下文和命令）和路径作为参数。以下是规则：

- Options come first and then args. A subcommand may take dashed options (which may take their own arguments, e.g. "--max-parents 2") and arguments. You SHOULD give dashed options first and then arguments. Some commands may accept dashed options after you have already gave non-option arguments (which may make the command ambiguous), but you should not rely on it (because eventually we may find a way to fix these ambiguity by enforcing the "options then args" rule).

- 选项先出现，然后是参数。子命令可能带有带有破折号的选项（这些选项可能有自己的参数，例如 "--max-parents 2"），然后是参数。你应该首先给出带有破折号的选项，然后是参数。有些命令可能在你已经提供非选项参数后接受带有破折号的选项（这可能使命令模糊不清），但你不应依赖于它（因为最终我们可能会通过强制执行"选项先参数后"的规则来解决这种模糊性）。

- Revisions come first and then paths. E.g. in `git diff v1.0 v2.0 arch/x86 include/asm-x86`, `v1.0` and `v2.0` are revisions and `arch/x86` and `include/asm-x86` are paths.

- 修订版本先出现，然后是路径。例如，在 `git diff v1.0 v2.0 arch/x86 include/asm-x86` 中，`v1.0` 和 `v2.0` 是修订版本，`arch/x86` 和 `include/asm-x86` 是路径。

- When an argument can be misunderstood as either a revision or a path, they can be disambiguated by placing `--` between them. E.g. `git diff -- HEAD` is, "I have a file called HEAD in my work tree. Please show changes between the version I staged in the index and what I have in the work tree for that file", not "show difference between the HEAD commit and the work tree as a whole". You can say `git diff HEAD --` to ask for the latter.

- 当一个参数可能被误解为修订版本或路径时，它们可以通过在它们之间添加 `--` 来消除歧义。例如，`git diff -- HEAD` 表示："我在我的工作树中有一个名为 HEAD 的文件。请显示我在索引中为该文件暂存的版本与工作树中的版本之间的更改"，而不是"显示 HEAD 提交与整个工作树之间的差异"。你可以使用 `git diff HEAD --` 来请求后者。

- Without disambiguating `--`, Git makes a reasonable guess, but errors out and asking you to disambiguate when ambiguous. E.g. if you have a file called HEAD in your work tree, `git diff HEAD` is ambiguous, and you have to say either `git diff HEAD --` or `git diff -- HEAD` to disambiguate.

- 没有 `--` 的情况下，Git 会进行合理的猜测，但如果有歧义，会报错并要求你消除歧义。例如，如果你的工作树中有一个名为 HEAD 的文件，`git diff HEAD` 是模糊不清的，你必须使用 `git diff HEAD --` 或 `git diff -- HEAD` 来消除歧义。

- Because `--` disambiguates revisions and paths in some commands, it cannot be used for those commands to separate options and revisions. You can use `--end-of-options` for this (it also works for commands that do not distinguish between revisions in paths, in which case it is simply an alias for `--`).

- 由于 `--` 在某些命令中消除修订版本和路径的歧义，因此不能在这些命令中用于分隔选项和修订版本。你可以使用 `--end-of-options` 来实现此目的（对于不区分修订版本和路径的命令，它也适用，此时它只是 `--` 的别名）。

  When writing a script that is expected to handle random user-input, it is a good practice to make it explicit which arguments are which by placing disambiguating `--` at appropriate places.

  在编写预计需要处理随机用户输入的脚本时，最好通过在适当的位置放置 `--` 来明确表明哪些参数是哪些。

- Many commands allow wildcards in paths, but you need to protect them from getting globbed by the shell. These two mean different things:

- 许多命令允许在路径中使用通配符，但你需要保护它们，以免被Shell展开。以下两个示例意义不同：

  ```
  $ git restore *.c
  $ git restore \*.c
  ```

  The former lets your shell expand the fileglob, and you are asking the dot-C files in your working tree to be overwritten with the version in the index. The latter passes the `*.c` to Git, and you are asking the paths in the index that match the pattern to be checked out to your working tree. After running `git add hello.c; rm hello.c`, you will *not* see `hello.c` in your working tree with the former, but with the latter you will.

  第一个示例允许Shell展开文件名通配符，你要求将工作树中的所有 dot-C 文件覆盖为索引中的版本。第二个示例将 `*.c` 传递给 Git，你要求将索引中与该模式匹配的路径检出到你的工作树。在运行 `git add hello.c; rm hello.c` 之后，前者在工作树中将不会看到 `hello.c`，而后者会。

- Just as the filesystem *.* (period) refers to the current directory, using a *.* as a repository name in Git (a dot-repository) is a relative path and means your current repository.

- 就像文件系统中的 *.*（点号）指代当前目录一样，使用 *.*（点号）作为 Git 中的存储库名称（一个点存储库）是一个相对路径，并表示当前存储库。

Here are the rules regarding the "flags" that you should follow when you are scripting Git:

​	以下是编写 Git 脚本时应遵循的有关"标志"的规则：

- It’s preferred to use the non-dashed form of Git commands, which means that you should prefer `git foo` to `git-foo`.
- 最好使用非破折号形式的 Git 命令，这意味着你应该优先选择 `git foo` 而不是 `git-foo`。
- Splitting short options to separate words (prefer `git foo -a -b` to `git foo -ab`, the latter may not even work).
- 将短选项拆分为单独的单词（优先选择 `git foo -a -b` 而不是 `git foo -ab`，后者可能无法正常工作）。
- When a command-line option takes an argument, use the *stuck* form. In other words, write `git foo -oArg` instead of `git foo -o Arg` for short options, and `git foo --long-opt=Arg` instead of `git foo --long-opt Arg` for long options. An option that takes optional option-argument must be written in the *stuck* form.
- 当命令行选项带有参数时，使用 *stuck* 形式。换句话说，对于短选项，使用 `git foo -oArg` 而不是 `git foo -o Arg`，对于长选项，使用 `git foo --long-opt=Arg` 而不是 `git foo --long-opt Arg`。带有可选选项参数的选项必须使用 *stuck* 形式。
- When you give a revision parameter to a command, make sure the parameter is not ambiguous with a name of a file in the work tree. E.g. do not write `git log -1 HEAD` but write `git log -1 HEAD --`; the former will not work if you happen to have a file called `HEAD` in the work tree.
- 当给命令提供修订版本参数时，请确保该参数不会与工作树中的文件名混淆。例如，不要写 `git log -1 HEAD`，而要写 `git log -1 HEAD --`；如果你碰巧在工作树中有一个名为 `HEAD` 的文件，前者将不起作用。
- Many commands allow a long option `--option` to be abbreviated only to their unique prefix (e.g. if there is no other option whose name begins with `opt`, you may be able to spell `--opt` to invoke the `--option` flag), but you should fully spell them out when writing your scripts; later versions of Git may introduce a new option whose name shares the same prefix, e.g. `--optimize`, to make a short prefix that used to be unique no longer unique.
- 许多命令允许长选项 `--option` 缩写为它们的唯一前缀（例如，如果没有其他以 `opt` 开头的选项，你可以使用 `--opt` 来调用 `--option` 标志），但在编写脚本时应该完全拼写它们；以后的 Git 版本可能会引入一个新的选项，其名称与现有前缀相同，例如 `--optimize`，使以前唯一的前缀不再唯一。

## 增强的选项解析器

From the Git 1.5.4 series and further, many Git commands (not all of them at the time of the writing though) come with an enhanced option parser.

​	从 Git 1.5.4 系列及以后的版本开始，许多 Git 命令（尽管并非所有命令在写作时都支持）都配备了增强的选项解析器。

Here is a list of the facilities provided by this option parser.

​	以下是此选项解析器提供的功能列表。

### 魔法选项

Commands which have the enhanced option parser activated all understand a couple of magic command-line options:

​	启用增强选项解析器的命令都支持一些魔法命令行选项：

- -h

  gives a pretty printed usage of the command.

  提供命令的漂亮打印使用说明。例如，`$ git describe -h` 的使用说明是：

  ```
  $ git describe -h
  usage: git describe [<options>] <commit-ish>*
     or: git describe [<options>] --dirty
  
      --contains            find the tag that comes after the commit
      --debug               debug search strategy on stderr
      --all                 use any ref
      --tags                use any tag, even unannotated
      --long                always use long format
      --abbrev[=<n>]        use <n> digits to display SHA-1s
  ```

  Note that some subcommand (e.g. `git grep`) may behave differently when there are things on the command line other than `-h`, but `git subcmd -h` without anything else on the command line is meant to consistently give the usage.

  请注意，某些子命令（例如 `git grep`）在命令行上还有其他 `-h` 以外的内容时，可能会有不同的行为，但是 `git subcmd -h` 在命令行上没有其他内容时应始终给出一致的使用说明。

- --help-all

  Some Git commands take options that are only used for plumbing or that are deprecated, and such options are hidden from the default usage. This option gives the full list of options.

  一些 Git 命令接受仅用于 plumbing 或已弃用的选项，这些选项在默认使用说明中是隐藏的。该选项提供完整的选项列表。

### 否定选项

Options with long option names can be negated by prefixing `--no-`. For example, `git branch` has the option `--track` which is *on* by default. You can use `--no-track` to override that behaviour. The same goes for `--color` and `--no-color`.

​	长选项名可以通过在前缀 `--no-` 的形式来否定。例如，`git branch` 有一个默认为 *on* 的选项 `--track`。你可以使用 `--no-track` 来覆盖该行为。`--color` 和 `--no-color` 也是同样的情况。

### 聚合短选项

Commands that support the enhanced option parser allow you to aggregate short options. This means that you can for example use `git rm -rf` or `git clean -fdx`.

​	支持增强选项解析器的命令允许你聚合短选项。这意味着你可以使用 `git rm -rf` 或 `git clean -fdx`。

### 缩写长选项

Commands that support the enhanced option parser accepts unique prefix of a long option as if it is fully spelled out, but use this with a caution. For example, `git commit --amen` behaves as if you typed `git commit --amend`, but that is true only until a later version of Git introduces another option that shares the same prefix, e.g. `git commit --amenity` option.

​	支持增强选项解析器的命令接受长选项的唯一前缀，就像它完整拼写一样，但是要谨慎使用这一点。例如，`git commit --amen` 的行为就好像你输入了 `git commit --amend`，但这只适用于 Git 的较早版本，后来的版本可能会引入一个新选项，其名称与现有前缀相同，例如 `git commit --amenity` 选项。

### 将参数与选项分隔开

You can write the mandatory option parameter to an option as a separate word on the command line. That means that all the following uses work:

​	你可以在命令行上将强制的选项参数作为单独的单词写入选项。这意味着以下所有用法都有效：

```
$ git foo --long-opt=Arg
$ git foo --long-opt Arg
$ git foo -oArg
$ git foo -o Arg
```

However, this is **NOT** allowed for switches with an optional value, where the *stuck* form must be used:

​	然而，对于具有可选值的开关选项，**不**允许这样使用，必须使用 *stuck* 形式：

```
$ git describe --abbrev HEAD     # correct
$ git describe --abbrev=10 HEAD  # correct
$ git describe --abbrev 10 HEAD  # NOT WHAT YOU MEANT
```

## 经常混淆选项的注意事项

Many commands that can work on files in the working tree and/or in the index can take `--cached` and/or `--index` options. Sometimes people incorrectly think that, because the index was originally called cache, these two are synonyms. They are **not** — these two options mean very different things.

​	许多可以在工作树和/或索引中处理文件的命令可以使用 `--cached` 和/或 `--index` 选项。有时人们错误地认为，因为索引最初被称为缓存，这两者是同义词。它们**不是** - 这两个选项意味着完全不同的事情。

- The `--cached` option is used to ask a command that usually works on files in the working tree to **only** work with the index. For example, `git grep`, when used without a commit to specify from which commit to look for strings in, usually works on files in the working tree, but with the `--cached` option, it looks for strings in the index.
- `--cached` 选项用于要求通常在工作树中处理文件的命令**仅**在索引中工作。例如，`git grep` 在没有指定从哪个提交开始查找字符串时，通常会在工作树中处理文件，但是使用 `--cached` 选项时，它会在索引中查找字符串。
- The `--index` option is used to ask a command that usually works on files in the working tree to **also** affect the index. For example, `git stash apply` usually merges changes recorded in a stash entry to the working tree, but with the `--index` option, it also merges changes to the index as well.
- `--index` 选项用于要求通常在工作树中处理文件的命令**同时**影响索引。例如，`git stash apply` 通常将存储条目中记录的更改合并到工作树中，但使用 `--index` 选项时，它也会将更改合并到索引中。

`git apply` command can be used with `--cached` and `--index` (but not at the same time). Usually the command only affects the files in the working tree, but with `--index`, it patches both the files and their index entries, and with `--cached`, it modifies only the index entries.

​	`git apply` 命令可以使用 `--cached` 和 `--index`（但不能同时使用）。通常该命令只会影响工作树中的文件，但是使用 `--index` 时，它会同时对文件和它们的索引条目进行修补，而使用 `--cached` 时，它仅修改索引条目。

See also https://lore.kernel.org/git/7v64clg5u9.fsf@assigned-by-dhcp.cox.net/ and https://lore.kernel.org/git/7vy7ej9g38.fsf@gitster.siamese.dyndns.org/ for further information.	

​	更多信息请参见https://lore.kernel.org/git/7v64clg5u9.fsf@assigned-by-dhcp.cox.net/ 和 https://lore.kernel.org/git/7vy7ej9g38.fsf@gitster.siamese.dyndns.org/。

Some other commands that also work on files in the working tree and/or in the index can take `--staged` and/or `--worktree`.

​	另外，其他一些在工作树和/或索引中处理文件的命令可以使用 `--staged` 和/或 `--worktree`。

- `--staged` is exactly like `--cached`, which is used to ask a command to only work on the index, not the working tree.
- `--worktree` is the opposite, to ask a command to work on the working tree only, not the index.
- The two options can be specified together to ask a command to work on both the index and the working tree.
- `--staged` 完全等同于 `--cached`，用于要求命令仅在索引中工作，不影响工作树。
- `--worktree` 是相反的，用于要求命令仅在工作树中工作，不影响索引。
- 这两个选项可以一起指定，以要求命令在索引和工作树上工作。

## GIT

  这是[git[1]](../../Git)工具集中的一部分。