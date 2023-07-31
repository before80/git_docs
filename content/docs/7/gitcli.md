+++
title = "gitcli"
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

​	gitcli - Git 命令行界面与约定

## 概要

gitcli

## 描述

​	本手册描述了在 Git 命令行界面中使用的约定。

​	许多命令以修订版本（通常是"提交"，但有时也可能是"tree-ish"，具体取决于上下文和命令）和路径作为参数。以下是规则：

- 选项先出现，然后是参数。子命令可能带有带有破折号的选项（这些选项可能有自己的参数，例如 "--max-parents 2"），然后是参数。你应该首先给出带有破折号的选项，然后是参数。有些命令可能在你已经提供非选项参数后接受带有破折号的选项（这可能使命令模糊不清），但你不应依赖于它（因为最终我们可能会通过强制执行"选项先参数后"的规则来解决这种模糊性）。

- 修订版本先出现，然后是路径。例如，在 `git diff v1.0 v2.0 arch/x86 include/asm-x86` 中，`v1.0` 和 `v2.0` 是修订版本，`arch/x86` 和 `include/asm-x86` 是路径。

- 当一个参数可能被误解为修订版本或路径时，它们可以通过在它们之间添加 `--` 来消除歧义。例如，`git diff -- HEAD` 表示："我在我的工作树中有一个名为 HEAD 的文件。请显示我在索引中为该文件暂存的版本与工作树中的版本之间的更改"，而不是"显示 HEAD 提交与整个工作树之间的差异"。你可以使用 `git diff HEAD --` 来请求后者。

- 没有 `--` 的情况下，Git 会进行合理的猜测，但如果有歧义，会报错并要求你消除歧义。例如，如果你的工作树中有一个名为 HEAD 的文件，`git diff HEAD` 是模糊不清的，你必须使用 `git diff HEAD --` 或 `git diff -- HEAD` 来消除歧义。

- 由于 `--` 在某些命令中消除修订版本和路径的歧义，因此不能在这些命令中用于分隔选项和修订版本。你可以使用 `--end-of-options` 来实现此目的（对于不区分修订版本和路径的命令，它也适用，此时它只是 `--` 的别名）。

  在编写预计需要处理随机用户输入的脚本时，最好通过在适当的位置放置 `--` 来明确表明哪些参数是哪些。

- 许多命令允许在路径中使用通配符，但你需要保护它们，以免被Shell展开。以下两个示例意义不同：

  ``` bash
  $ git restore *.c
  $ git restore \*.c
  ```

  第一个示例允许Shell展开文件名通配符，你要求将工作树中的所有 dot-C 文件覆盖为索引中的版本。第二个示例将 `*.c` 传递给 Git，你要求将索引中与该模式匹配的路径检出到你的工作树。在运行 `git add hello.c; rm hello.c` 之后，前者在工作树中将不会看到 `hello.c`，而后者会。

- 就像文件系统中的 *.*（点号）指代当前目录一样，使用 *.*（点号）作为 Git 中的存储库名称（一个点存储库）是一个相对路径，并表示当前存储库。

​	以下是编写 Git 脚本时应遵循的有关"标志"的规则：

- 最好使用非破折号形式的 Git 命令，这意味着你应该优先选择 `git foo` 而不是 `git-foo`。
- 将短选项拆分为单独的单词（优先选择 `git foo -a -b` 而不是 `git foo -ab`，后者可能无法正常工作）。
- 当命令行选项带有参数时，使用 *stuck* 形式。换句话说，对于短选项，使用 `git foo -oArg` 而不是 `git foo -o Arg`，对于长选项，使用 `git foo --long-opt=Arg` 而不是 `git foo --long-opt Arg`。带有可选选项参数的选项必须使用 *stuck* 形式。
- 当给命令提供修订版本参数时，请确保该参数不会与工作树中的文件名混淆。例如，不要写 `git log -1 HEAD`，而要写 `git log -1 HEAD --`；如果你碰巧在工作树中有一个名为 `HEAD` 的文件，前者将不起作用。
- 许多命令允许长选项 `--option` 缩写为它们的唯一前缀（例如，如果没有其他以 `opt` 开头的选项，你可以使用 `--opt` 来调用 `--option` 标志），但在编写脚本时应该完全拼写它们；以后的 Git 版本可能会引入一个新的选项，其名称与现有前缀相同，例如 `--optimize`，使以前唯一的前缀不再唯一。

## 增强的选项解析器

​	从 Git 1.5.4 系列及以后的版本开始，许多 Git 命令（尽管并非所有命令在写作时都支持）都配备了增强的选项解析器。

​	以下是此选项解析器提供的功能列表。

### 魔法选项

​	启用增强选项解析器的命令都支持一些魔法命令行选项：

- -h

  提供命令的漂亮打印使用说明。例如，`$ git describe -h` 的使用说明是：

  ``` bash
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

  请注意，某些子命令（例如 `git grep`）在命令行上还有其他 `-h` 以外的内容时，可能会有不同的行为，但是 `git subcmd -h` 在命令行上没有其他内容时应始终给出一致的使用说明。

- `--help-all`

  一些 Git 命令接受仅用于 plumbing 或已弃用的选项，这些选项在默认使用说明中是隐藏的。该选项提供完整的选项列表。

### 否定选项

​	长选项名可以通过在前缀 `--no-` 的形式来否定。例如，`git branch` 有一个默认为 *on* 的选项 `--track`。你可以使用 `--no-track` 来覆盖该行为。`--color` 和 `--no-color` 也是同样的情况。

### 聚合短选项

​	支持增强选项解析器的命令允许你聚合短选项。这意味着你可以使用 `git rm -rf` 或 `git clean -fdx`。

### 缩写长选项

​	支持增强选项解析器的命令接受长选项的唯一前缀，就像它完整拼写一样，但是要谨慎使用这一点。例如，`git commit --amen` 的行为就好像你输入了 `git commit --amend`，但这只适用于 Git 的较早版本，后来的版本可能会引入一个新选项，其名称与现有前缀相同，例如 `git commit --amenity` 选项。

### 将参数与选项分隔开

​	你可以在命令行上将强制的选项参数作为单独的单词写入选项。这意味着以下所有用法都有效：

``` bash
$ git foo --long-opt=Arg
$ git foo --long-opt Arg
$ git foo -oArg
$ git foo -o Arg
```

​	然而，对于具有可选值的开关选项，**不**允许这样使用，必须使用 *stuck* 形式：

``` bash
$ git describe --abbrev HEAD     # correct
$ git describe --abbrev=10 HEAD  # correct
$ git describe --abbrev 10 HEAD  # NOT WHAT YOU MEANT
```

## 经常混淆选项的注意事项

​	许多可以在工作树和/或索引中处理文件的命令可以使用 `--cached` 和/或 `--index` 选项。有时人们错误地认为，因为索引最初被称为缓存，这两者是同义词。它们**不是** - 这两个选项意味着完全不同的事情。

- `--cached` 选项用于要求通常在工作树中处理文件的命令**仅**在索引中工作。例如，`git grep` 在没有指定从哪个提交开始查找字符串时，通常会在工作树中处理文件，但是使用 `--cached` 选项时，它会在索引中查找字符串。
- `--index` 选项用于要求通常在工作树中处理文件的命令**同时**影响索引。例如，`git stash apply` 通常将存储条目中记录的更改合并到工作树中，但使用 `--index` 选项时，它也会将更改合并到索引中。

​	`git apply` 命令可以使用 `--cached` 和 `--index`（但不能同时使用）。通常该命令只会影响工作树中的文件，但是使用 `--index` 时，它会同时对文件和它们的索引条目进行修补，而使用 `--cached` 时，它仅修改索引条目。

​	更多信息请参见https://lore.kernel.org/git/7v64clg5u9.fsf@assigned-by-dhcp.cox.net/ 和 https://lore.kernel.org/git/7vy7ej9g38.fsf@gitster.siamese.dyndns.org/。

​	另外，其他一些在工作树和/或索引中处理文件的命令可以使用 `--staged` 和/或 `--worktree`。

- `--staged` 完全等同于 `--cached`，用于要求命令仅在索引中工作，不影响工作树。
- `--worktree` 是相反的，用于要求命令仅在工作树中工作，不影响索引。
- 这两个选项可以一起指定，以要求命令在索引和工作树上工作。

## GIT

  这是[git[1]](../../Git)工具集中的一部分。