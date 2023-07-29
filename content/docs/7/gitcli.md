https://git-scm.com/docs/gitcli

## 名称

gitcli - Git 命令行接口和约定

## 概述

gitcli

## 描述

​	本手册描述了 Git CLI 中使用的约定。

​	许多命令将修订版本（通常是"提交"，但有时是"tree-ish"，取决于上下文和命令）和路径作为它们的参数。以下是规则：

- 选项首先出现，然后是参数。子命令可以接受带破折号的选项（这些选项可能带有自己的参数，例如"--max-parents 2"）和参数。您应该先给出带破折号的选项，然后是参数。一些命令可能在您已经给出非选项参数后接受带破折号的选项（这可能会使命令模糊不清），但您不应该依赖于它（因为最终我们可能会找到一种方法通过执行"选项然后参数"的规则来解决这些模糊不清）。

- 修订版本首先出现，然后是路径。例如，在 `git diff v1.0 v2.0 arch/x86 include/asm-x86` 中，`v1.0` 和 `v2.0` 是修订版本，`arch/x86` 和 `include/asm-x86` 是路径。

- 当一个参数既可以被误解为修订版本又可以被误解为路径时，它们可以通过在它们之间放置 `--` 来消除歧义。例如，`git diff -- HEAD` 的意思是"我有一个名为 HEAD 的文件在我的工作树中。请显示我为该文件暂存到索引中的版本和我在工作树中拥有的版本之间的变化"，而不是"显示 HEAD 提交和整个工作树之间的差异"。您可以说 `git diff HEAD --` 以请求后者。

- 如果没有使用 `--` 进行消歧，Git 会做出合理的猜测，但在模棱两可时会出错并要求您消除歧义。例如，如果您的工作树中有一个名为 HEAD 的文件，则 `git diff HEAD` 是模棱两可的，您必须说 `git diff HEAD --` 或 `git diff -- HEAD` 以消除歧义。

- 因为 `--` 在某些命令中消除了修订版本和路径的歧义，所以这些命令不能用它来分隔选项和修订版本。你可以使用 `--end-of-options` 来实现这个目的（对于那些不区分修订版本和路径的命令，此时它只是 `--` 的别名）。

  当编写预期处理随机用户输入的脚本时，通过在适当的位置放置消歧的 `--` 来明确哪个参数是哪个参数是一种好的做法。

- Many commands allow wildcards in paths, but you need to protect them from getting globbed by the shell. These two mean different things:

- 许多命令允许在路径中使用通配符，但是你需要防止它们被Shell匹配。下面这两个命令有不同的含义：

  ```
  $ git restore *.c
  $ git restore \*.c
  ```

  前者允许Shell扩展文件通配符，你要求在工作树中将所有后缀为`.c`的文件用索引中的版本进行覆盖。后者将`*.c`传递给Git，你要求检出与该模式匹配的索引中的路径到你的工作树中。运行`git add hello.c; rm hello.c`后，前者中你将看不到`hello.c`在你的工作树中，但在后者中你将看到它。

- 就像文件系统中的`.`（点）代表当前目录一样，在Git中使用一个`.`作为仓库名称（一个点仓库）是一个相对路径，代表你当前的仓库。


​	以下是你在编写Git脚本时应遵循的"flags"规则：

- 最好使用非破折号形式的Git命令，这意味着你应该使用`git foo`而不是`git-foo`。
- 将短选项拆分成单独的单词（使用`git foo -a -b`而不是`git foo -ab`，后者可能甚至无法工作）。
- 当命令行选项需要参数时，请使用 stuck 形式。换句话说，对于短选项，请使用`git foo -oArg`而不是`git foo -o Arg`，对于长选项，请使用`git foo --long-opt=Arg`而不是`git foo --long-opt Arg`。接受可选选项参数的选项必须以 stuck 形式书写。
- 当向命令提供修订版本参数时，请确保该参数不会与工作树中的文件名混淆。例如，不要写`git log -1 HEAD`，而要写`git log -1 HEAD --`；如果你在工作树中恰好有一个名为HEAD的文件，前者将无法工作。
- 许多命令允许长选项`--option`仅缩写为它们的唯一前缀（例如，如果没有其他选项的名称以`opt`开头，则可以使用`--opt`来调用`--option`标志），但在编写脚本时应该完全拼写它们；后续版本的Git可能会引入一个新选项，其名称与先前的前缀相同，例如`--optimize`，使得原来的短前缀不再是唯一的。

## 增强选项解析器 

​	从 Git 1.5.4 系列及以后版本开始，许多 Git 命令（尽管在撰写本文时并非所有命令）都配备了增强选项解析器。

​	以下是该选项解析器提供的功能列表：

### 魔术选项 

​	启用增强选项解析器的命令都能理解一些魔术命令行选项：

- -h

  以漂亮格式显示命令的使用说明。

  ```bash
  $ git describe -h
  usage: git describe [<options>] <commit-ish>*
     or: git describe [<options>] --dirty
  
      --contains            找到该提交之后的标签
      --debug               在stderr上调试搜索策略
      --all                 使用任意ref
      --tags                使用任意标签，即使未注解
      --long                总是使用长格式
      --abbrev[=<n>]        use <n> digits to display SHA-1s
  ```

  注意，一些子命令（例如 `git grep`）在命令行中除了`-h` 之外还有其他东西时可能会有所不同，但 `git subcmd -h` 而没有其他内容的命令行意味着一致地给出使用说明。

- --help-all

  一些 Git 命令使用的选项仅用于工具命令或已过时，此类选项在默认使用说明中被隐藏。此选项可给出完整的选项列表。

### 否定选项 

​	带有长选项名称的选项可以通过在前缀中加上 `--no-` 进行否定。例如，`git branch` 有一个默认启用的 `--track` 选项，您可以使用 `--no-track` 来覆盖该行为。对于 `--color` 和 `--no-color` 也是如此。

### 聚合短选项 

​	支持增强选项解析器的命令允许您聚合短选项。这意味着您可以使用 `git rm -rf` 或 `git clean -fdx`。

### 缩写长选项 

​	支持增强选项解析器的命令接受长选项的唯一前缀，就好像它完全拼写出来一样，但使用时要小心。例如，`git commit --amen` 行为就像您键入了 `git commit --amend`，但这只在 Git 的稍后版本中没有其他选项共享相同前缀，例如 `git commit --amenity` 选项时才是正确的。

### 将参数与选项分离 

​	您可以将必需的选项参数作为命令行上的单独单词编写。这意味着以下所有用法都有效：

``` bash
$ git foo --long-opt=Arg
$ git foo --long-opt Arg
$ git foo -oArg
$ git foo -o Arg
```

​	但是，对于具有可选值的开关，这是不允许的，必须使用 stuck 格式：

``` bash
$ git describe --abbrev HEAD     # correct
$ git describe --abbrev=10 HEAD  # correct
$ git describe --abbrev 10 HEAD  # NOT WHAT YOU MEANT
```

## 经常混淆的选项说明 

​	许多可以在工作树和/或索引中处理文件的命令可以采用`--cached`和/或`--index`选项。有时人们错误地认为，因为索引最初被称为缓存（cache），所以这两个是同义词。实际上它们不是同义词 —— 这两个选项意味着非常不同的东西。

- `--cached` 选项用于要求一个通常在工作树中处理文件的命令仅在索引中处理。例如，当没有指定要从哪个提交中查找字符串时，`git grep` 通常在工作树中处理文件，但是使用 `--cached` 选项，它会在索引中查找字符串。
- `--index` 选项用于要求一个通常在工作树中处理文件的命令也影响索引。例如，`git stash apply` 通常会将一个stash条目中记录的更改合并到工作树中，但是若使用了 `--index` 选项，它也会将更改合并到索引中。

​	`git apply`命令可以使用`--cached`和`--index`选项（但不能同时使用）。通常，该命令仅影响工作树中的文件，但是使用`--index`选项，它会修补文件和它们的索引条目，而使用`--cached`，它只修改索引条目。

​	有关更多信息，请参见[https://lore.kernel.org/git/7v64clg5u9.fsf@assigned-by-dhcp.cox.net/](https://lore.kernel.org/git/7v64clg5u9.fsf@assigned-by-dhcp.cox.net/)和[https://lore.kernel.org/git/7vy7ej9g38.fsf@gitster.siamese.dyndns.org/](https://lore.kernel.org/git/7vy7ej9g38.fsf@gitster.siamese.dyndns.org/)。

​	还有一些也可以在工作树和/或索引中处理文件的命令可以采用`--staged`和/或`--worktree`选项。

- `--staged`完全像`--cached`，用于要求一个命令仅在索引中处理，而不是在工作树中处理。
- `--worktree` 则相反的，用于要求一个命令仅在工作树中处理，而不是在索引中处理。
- 这两个选项可以一起指定，以要求命令在索引和工作树上同时处理。

## GIT

  这是[git[1]](../../Git)工具集中的一部分。