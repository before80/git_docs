+++
title = "Git核心教程"
weight = 30
type = "docs"
date = 2023-07-26T09:47:23+08:00
description = ""
isCJKLanguage = true
draft = false

+++

# Gitcore Tutorial - Git核心教程



https://git-scm.com/docs/gitcore-tutorial

version 2.41.0

## 名称

​	gitcore-tutorial - 面向开发者的Git核心教程

## 概述

```
git *
```



## 描述

​	本教程解释了如何使用"core" Git命令来设置和使用Git仓库。

​	如果您只需要使用Git作为版本控制系统，您可能更喜欢从"A Tutorial Introduction to Git" ([gittutorial[7]](https://chat.openai.com/gittutorial))或[Git用户手册](https://git-scm.com/docs/user-manual)开始。

​	然而，如果您想了解Git的内部工作原理，对这些低级工具的理解可能会很有帮助。

​	核心Git通常被称为"plumbing"，而构建在其上的更美观的用户界面称为"porcelain"。您可能不经常直接使用plumbing，但当porcelain无法完成任务时，了解plumbing的工作原理可能很有用。

​	在撰写本文档时，许多porcelain命令仍然是shell脚本。出于简单起见，本文仍使用它们作为示例，以说明如何组合plumbing形成porcelain命令。源代码树中包含了一些这样的脚本，位于contrib/examples/目录下供参考。虽然这些脚本不再作为shell脚本实现，但对于plumbing命令的描述仍然有效。

> 注意
>
> ​	更深层次的技术细节通常标记为"注意"，您可以在第一次阅读时跳过。

## 创建Git仓库

​	创建一个新的Git仓库非常简单：所有的Git仓库都是空的，您唯一需要做的就是找到一个子目录，用它作为工作树——对于全新的项目，可以是一个空目录，或者是您希望导入到Git中的现有工作树。

​	作为我们的第一个示例，我们将从头开始创建一个全新的仓库，没有预先存在的文件，我们将其称为*git-tutorial*。首先创建一个子目录，进入该子目录，并使用*git init*初始化Git基础设施：

``` bash
$ mkdir git-tutorial
$ cd git-tutorial
$ git init
```

Git会回复

```
Initialized empty Git repository in .git/
```

这表示Git告诉您没有进行任何奇怪的操作，并且已为您的新项目创建了一个本地的`.git`目录设置。现在，您将拥有一个`.git`目录，并可以使用*ls*查看其中的内容。对于您的新空项目，除其他内容外，应该显示三个条目：

- 一个名为`HEAD`的文件，其中包含`ref: refs/heads/master`。这类似于符号链接，并指向相对于`HEAD`文件的`refs/heads/master`。

  不用担心`HEAD`链接指向的文件尚不存在——您还没有创建开始`HEAD`开发分支的提交。
- 一个名为`objects`的子目录，其中包含您项目的所有对象。您通常不需要直接查看对象，但您可能想知道这些对象包含您的仓库中所有真实的*数据*。
- 一个名为`refs`的子目录，其中包含对象的引用。

​	特别是，`refs`子目录将包含两个其他子目录，分别命名为`heads`和`tags`。它们正如它们的名称所暗示的那样：它们包含对多个不同开发（也称为分支）的*heads*的引用，以及对您在仓库中创建的任何*tags*的引用。

​	一个注意：特殊的`master` head是默认分支，这就是为什么`.git/HEAD`文件被创建时指向它，即使它尚不存在。基本上，`HEAD`链接始终应该指向您当前正在工作的分支，而您始终希望开始工作时能够使用`master`分支。

​	但是，这只是一个约定，并且您可以使用任何您想要的名称来命名分支，甚至不必拥有`master`分支。一些Git工具会假定`.git/HEAD`是有效的，尽管如此。

> 注意
>
> ​	*对象*通过其160位SHA-1哈希（也称为*对象名称*）来标识，对对象的引用始终是该SHA-1名称的40字节十六进制表示。`refs`子目录中的文件预期包含这些十六进制引用（通常在末尾有一个`\n`），因此当您开始填充您的树时，您应该期望在这些`refs`子目录中看到一些包含这些引用的41字节文件。

> 注意
>
>  在完成本教程后，高级用户可能希望查看[gitrepository-layout[5]](https://chat.openai.com/5/gitrepository-layout)。

​	您现在已经创建了您的第一个Git仓库。当然，由于它是空的，这并没有什么用，所以让我们开始向其中填充数据。

## 填充Git仓库

​	我们将保持简单和愚蠢，所以我们首先将填充一些简单的文件，以便对此有所了解。

​	首先，只需创建任何您希望在Git仓库中维护的随机文件。我们将从一些不好的示例开始，只是为了感受一下它是如何工作的：

``` bash
$ echo "Hello World" >hello
$ echo "Silly example" >example
```

​	现在，您已经在您的工作树（也称为*工作目录*）中创建了两个文件，但要实际检入您的工作成果，您需要执行两个步骤：

- 填充*索引*文件（也称为*cache*），以包含有关您的工作树状态的信息。
- 将该索引文件提交为一个对象。

​	第一步非常简单：当您想告诉Git有关工作树的任何更改时，您使用*git update-index*程序。该程序通常只接受您希望更新的文件名列表，但为了避免简单的错误，除非您使用`--add`标志（添加新条目）或`--remove`标志（删除条目），否则它会拒绝将新条目添加到索引中（或删除现有条目）。

​	因此，要使用刚刚创建的两个文件填充索引，您可以执行以下操作：

``` bash
$ git update-index --add hello example
```

现在，您已经告诉Git跟踪这两个文件。

​	实际上，当您执行上面的操作时，如果现在查看对象目录，您会注意到Git已经将两个新对象添加到对象数据库中。如果您完全按照上面的步骤执行，现在可以执行以下操作：

``` bash
$ ls .git/objects/??/*
```

然后会看到两个文件：

```
.git/objects/55/7db03de997c86a4a028e1ebd3a1ceb225be238
.git/objects/f2/4c74a2e500f5ee1332c86b94199f52b1d1d962
```

这两个文件分别对应名称为`557db...`和`f24c7...`的对象。

​	如果您愿意，可以使用*git cat-file*查看这些对象，但您必须使用对象名称，而不是对象的文件名：

``` bash
$ git cat-file -t 557db03de997c86a4a028e1ebd3a1ceb225be238
```

其中`-t`告诉*git cat-file*告诉您对象的"类型"。Git将告诉您您有一个"blob"对象（即，一个常规文件），您可以使用以下命令查看其内容：

``` bash
$ git cat-file blob 557db03
```

这将输出"Hello World"。对象`557db03`其实就是文件`hello`的内容。

> 注意
>
> ​	不要将该对象与文件`hello`本身混淆。该对象只是文件特定的**内容**，而无论您以后如何更改文件`hello`中的内容，我们刚刚查看的对象将永远不会改变。对象是不可变的。

> 注意
>
> ​	第二个示例演示了在大多数地方可以缩写对象名称为只有前几个十六进制数字。

​	不过，正如我们之前提到的，您通常不会实际查看对象本身，并且键入长长的40个字符的十六进制名称不是您通常想要做的事情。上面的插曲只是为了显示*git update-index*做了一些魔术，实际上将文件的内容保存到了Git对象数据库中。

​	更新索引还做了其他事情：它创建了一个`.git/index`文件。这是描述您当前工作树的索引，您应该对此非常清楚。再次强调，您通常不需要担心索引文件本身，但您应该知道实际上您还没有真正地将文件"检入"到Git中，您只是**告诉**Git有关它们。

​	但是，由于Git知道这些文件，因此现在您可以开始使用一些最基本的Git命令来操作这些文件或查看它们的状态。

​	特别地，我们暂时还不会将这两个文件提交到 Git 中，我们将首先向 `hello` 添加另一行：

``` bash
$ echo "It's a new day for git" >>hello
```

现在，你已经告诉 Git 关于 `hello` 的先前状态，可以使用 *git diff-files* 命令询问 Git 相对于旧索引的树上的更改：

``` bash
$ git diff-files
```

​	哎呀，这个输出不太易读。它只输出了内部版本的 *diff*，但实际上只是告诉你它已经注意到 "hello" 被修改了，并且旧的对象内容已被替换为其他内容。

​	为了使其可读，我们可以使用 `-p` 标志告诉 *git diff-files* 将差异输出为补丁：

``` bash
$ git diff-files -p
diff --git a/hello b/hello
index 557db03..263414f 100644
--- a/hello
+++ b/hello
@@ -1 +1,2 @@
 Hello World
+It's a new day for git
```

​	这是我们通过向 `hello` 添加另一行导致的更改的差异。

​	换句话说，*git diff-files* 始终显示记录在索引中与当前工作树中的差异。这非常有用。

​	一个常见的缩写是 `git diff-files -p`，你可以简写成 `git diff`，它会执行相同的操作。

``` bash
$ git diff
diff --git a/hello b/hello
index 557db03..263414f 100644
--- a/hello
+++ b/hello
@@ -1 +1,2 @@
 Hello World
+It's a new day for git
```

## 提交Git状态

​	现在，我们想进入 Git 的下一阶段，这是将 Git 知道的索引中的文件作为一个真实的树提交。我们通过两个阶段来完成：创建一个 *tree* 对象，然后将该 *tree* 对象与对该树的解释以及我们如何到达该状态的信息一起作为一个 *commit* 对象提交。

​	创建 tree 对象是简单的，使用 *git write-tree* 完成。没有选项或其他输入：`git write-tree` 将采用当前的索引状态，并写入描述整个索引的对象。换句话说，我们现在正在将所有不同的文件名与其内容（及其权限）绑定在一起，并创建了一个类似于 Git 中的 "目录" 对象：

``` bash
$ git write-tree
```

这将输出结果 tree 的名称，本例中（如果你完全按照我的描述执行），应该是：

```
8988da15d077d4829fc51d8544c097def6644dbb
```

这又是另一个难以理解的对象名称。同样，如果你愿意，你可以使用 `git cat-file -t 8988d...` 查看该对象不是 "blob" 对象，而是 "tree" 对象（你也可以使用 `git cat-file` 实际输出原始对象内容，但将会看到一个主要是二进制混乱的输出，因此这没那么有趣）。

​	但是，通常你永远不会单独使用 *git write-tree*，因为通常你总是使用 *git commit-tree* 命令将一个 tree 提交为一个 commit 对象。实际上，完全不使用 *git write-tree* 单独使用，而是将其结果直接作为 *git commit-tree* 的参数。

​	*git commit-tree* 通常需要多个参数 - 它想知道一个提交的 *parent* 是什么，但由于这是该新存储库中的第一个提交，并且它没有父提交，所以我们只需要传入树的对象名称。然而，*git commit-tree* 还希望从标准输入中获取提交消息，并将结果的对象名称写入其标准输出。

​	这就是我们创建 `.git/refs/heads/master` 文件的地方，`HEAD` 指向该文件。该文件应该包含指向 master 分支的树的引用，由于这正是 *git commit-tree* 输出的，因此我们可以使用一系列简单的 shell 命令来完成这个过程：

``` bash
$ tree=$(git write-tree)
$ commit=$(echo 'Initial commit' | git commit-tree $tree)
$ git update-ref HEAD $commit
```

​	在本例中，这创建了一个与其他任何内容都无关的全新提交。通常，你只会对项目执行这一次，以后的所有提交都将在先前的提交之上。

​	同样，通常你永远不会手动执行此操作。有一个有用的脚本叫做 `git commit`，它会为你完成所有这些工作。所以你可以只输入 `git commit`，它将为你完成上述的魔术脚本。

## 进行更改

​	还记得我们是如何对文件 `hello` 进行 *git update-index*，然后在稍后修改了 `hello`，并且可以将 `hello` 的新状态与我们保存在索引文件中的状态进行比较的吗？

​	此外，还记得我说过 *git write-tree* 将索引文件的内容写入树，因此我们刚才提交的实际上是文件 `hello` 的**原始**内容，而不是新的内容。我们之所以这样做，是为了展示索引状态与工作树状态的差异，以及即使我们提交了东西，它们也不必匹配。

​	与之前一样，如果在我们的 git-tutorial 项目中运行 `git diff-files -p`，我们将仍然看到上次看到的相同差异：索引文件在提交任何内容时没有改变。然而，现在我们已经提交了一些东西，我们还可以学会使用一个新的命令：*git diff-index*。

​	与 *git diff-files* 不同，它显示的是索引中的差异，*git diff-index* 显示的是一个提交的 **tree** 与索引文件或工作树之间的差异。换句话说，*git diff-index* 需要与 tree 进行比较，而之前我们不能这样做，因为我们没有任何东西可以与其进行比较。

​	但是现在我们可以执行以下命令：

``` bash
$ git diff-index -p HEAD
```

（其中 `-p` 的含义与 *git diff-files* 中相同），它将显示相同的差异，但原因完全不同。现在我们在比较工作树与索引文件之间的状态，而不是与我们刚刚写入的树进行比较。刚好这两者显然是相同的，所以我们得到了相同的结果。

​	同样，由于这是一个常见操作，你也可以使用以下简写：

``` bash
$ git diff HEAD
```

它为你执行了上述操作。

​	换句话说，*git diff-index* 通常会将一个 tree 与工作树进行比较，但给定了 `--cached` 标志后，它会告诉它改为与仅与索引缓存内容进行比较，并完全忽略当前的工作树状态。因为我们刚刚将索引文件写入 HEAD，因此执行 `git diff-index --cached -p HEAD` 将返回一组空的差异，这正是它所做的。

> 注意
>
> ​	*git diff-index* 通常始终使用索引进行比较，因此它将树与工作树进行比较并不严格准确。特别是，用于进行比较的文件列表（"元数据"）始终来自索引文件，而与是否使用 `--cached` 标志无关。`--cached` 标志只确定要比较的文件 **内容** 是否来自工作树。当你意识到 Git 简单地从未知（或不关心）的文件开始并不重要时，这并不难理解。Git 永远不会**主动**寻找要比较的文件，它期望你告诉它文件是什么，这就是索引的存在目的。

​	然而，接下来的步骤是提交我们所做的**更改**，为了更好地理解正在发生的事情，请记住 "工作树内容"、"索引文件" 和 "提交树" 之间的区别。我们在工作树中有一些要提交的更改，我们必须始终通过索引文件进行操作，因此我们首先需要更新索引缓存：

``` bash
$ git update-index hello
```

（请注意，这次我们不需要 `--add` 标志，因为 Git 已经知道这个文件）。

​	请注意，此处的`git diff-*` 版本之间的差异。在我们更新了索引中的 `hello` 之后，`git diff-files -p` 不再显示任何差异，但 `git diff-index -p HEAD` 仍然显示当前状态与我们提交的状态不同。事实上，现在 *git diff-index* 显示的差异无论是否使用 `--cached` 标志，都是相同的，因为现在索引与工作树是一致的。

​	现在，既然我们已经更新了索引中的 `hello`，我们可以提交新版本。我们可以再次手动编写树，并提交树（这次我们必须使用 `-p HEAD` 标志，以告诉提交 HEAD 是新提交的 **父**，这不再是初始提交了），但是你已经执行了一次了，所以这次我们只使用有用的脚本：

``` bash
$ git commit
```

它会为你启动编辑器以编写提交消息，并向你介绍你所做的一些内容。

​	写下你想要的任何消息，所有以 *#* 开头的行都将被删除，其余的行将用作更改的提交消息。如果你决定此时不想提交任何内容（你可以继续编辑内容并更新索引），你可以留空提交消息。否则，`git commit` 将为你提交更改。

​	现在，你已经创建了你的第一个真正的 Git 提交。如果你对查看 `git commit` 的实际操作感兴趣，请随时调查：这是一些非常简单的 shell 脚本，用于生成有用的提交消息头部，并且有几个一行的命令执行了提交本身 (*git commit*)。

## 检查更改

​	创建更改是有用的，但如果以后可以查看更改内容，那将更加有用。对此，最有用的命令是 *diff* 家族中的另一个命令，即 *git diff-tree*。

​	*git diff-tree* 可以接收两个任意的树对象，并告诉你它们之间的差异。更常见的用法是，你可以只给它一个单独的提交对象，它会自动找到该提交的父提交，并直接显示差异。因此，要获得我们已经多次看到的相同差异，现在可以执行以下命令：

``` bash
$ git diff-tree -p HEAD
```

（再次说明，`-p` 意味着以可读性更好的补丁形式显示差异），它将显示 `HEAD`（即最后一次提交）的实际更改内容。

> **注意**
>
> ​	以下是 Jon Loeliger 的 ASCII 图，用于说明各种 *diff-* 命令之间的比较。
>
> ```
>         diff-tree
>           +----+
>          |    |
>           |    |
>              V    V
>           +-----------+
>           | Object DB |
>           |  Backing  |
>           |   Store   |
>           +-----------+
>             ^    ^
>             |    |
>             |    |  diff-index --cached
>             |    |
>    diff-index  |    V
>             |  +-----------+
>             |  |   Index   |
>             |  |  "cache"  |
>          |  +-----------+
>             |    ^
>             |    |
>             |    |  diff-files
>             |    |
>             V    V
>           +-----------+
>           |  Working  |
>           | Directory |
>           +-----------+
>    ```



​	更有趣的是，你还可以给 *git diff-tree* 加上 `--pretty` 标志，它会显示提交消息、作者和日期，你还可以让它显示一系列差异。或者，你可以让它处于“静默”模式，不显示差异，而只显示实际的提交消息。

​	实际上，结合 *git rev-list* 程序（用于生成修订版本列表），*git diff-tree* 实际上是一个变化的不竭源泉。你可以用一个简单的脚本将 `git rev-list` 的输出管道传给 `git diff-tree --stdin`，这正是早期版本的 `git log` 是如何实现的。

## 给版本打标签

​	在 Git 中，有两种类型的标签，一种是“轻量级”标签，另一种是“带注释”的标签。

​	“轻量级”标签实际上只是一个分支，除了将它放在 `.git/refs/tags/` 子目录中，而不称其为 `head` 外，与普通分支没有任何区别。因此，最简单的标签形式仅涉及：

``` bash
$ git tag my-first-tag
```

它会将当前的 `HEAD` 写入 `.git/refs/tags/my-first-tag` 文件，此后你就可以使用这个符号名称来代表特定的状态。你可以执行以下操作：

``` bash
$ git diff my-first-tag
```

来查看当前状态与标签之间的差异，在这一点上，差异显然是空的。但如果你继续开发并提交内容，你可以使用标签作为“锚点”来查看自从标记它以来发生了什么变化。

​	“带注释”的标签实际上是一个真正的 Git 对象，不仅包含指向要标记的状态的指针，还包含一个小的标签名称和消息，还可以选择包含一个 PGP 签名，表示是的，你真的打了这个标签。你可以通过向 *git tag* 添加 `-a` 或 `-s` 标志来创建这些带注释的标签：

``` bash
$ git tag -s <tagname>
```

它会对当前的 `HEAD` 进行签名（但你也可以给它另一个参数，指定要标记的对象，例如，你可以通过 `git tag <tagname> mybranch` 来标记当前 `mybranch` 点）。

​	通常，你只会为主要版本发布或类似的情况使用带注释的标签，而轻量级标签可用于任何你想要标记的场合，只要你决定要记住某个特定的点，只需为其创建一个私有标签，你就有了一个漂亮的符号名称来表示该点的状态。

## 复制存储库

​	Git 存储库通常是完全自包含和可移植的。与 CVS 不同，Git 没有“存储库”和“工作树”的区分。Git 存储库通常**就是**工作树，本地 Git 信息隐藏在 `.git` 子目录中。没有其他内容。所见即所得。

> 注意
>
> ​	你可以告诉 Git 将 Git 内部信息与它跟踪的目录分离开来，但是我们暂时忽略这一点：它不是正常项目的工作方式，它实际上只适用于特殊用途。因此，“Git 信息始终直接与其描述的工作树相关联”这种心理模型可能在技术上不是100％准确，但对于所有正常的用途来说，这是一个很好的模型。

​	这有两个含义：

- 如果你对创建的教程存储库厌倦了（或者你犯了一个错误并想重新开始），你可以简单地执行：

  ```bash
  $ rm -rf git-tutorial
  ```

  它就会被删除。没有外部存储库，项目之外没有历史记录。
- 如果你想要移动或复制 Git 存储库，你可以这样做。有 *git clone* 命令，但是如果你只想创建一个存储库的副本（包含所有完整的历史记录），你可以用普通的 `cp -a git-tutorial new-git-tutorial` 来实现。

  需要注意的是，当你移动或复制一个 Git 存储库时，你的 Git 索引文件（它缓存了各种信息，特别是与所涉及文件的某些“stat”信息）可能需要刷新。所以在执行 `cp -a` 创建一个新副本之后，你需要执行以下操作：

  ```bash
  $ git update-index --refresh
  ```

  在新存储库中确保索引文件是最新的。

​	请注意，即使跨机器复制，第二点也是正确的。你可以使用**任何**普通的复制机制，例如 *scp*、*rsync* 或 *wget* 来复制一个远程 Git 存储库。

​	复制远程存储库时，你至少需要在此操作时更新索引缓存，尤其是对于他人的存储库，通常你希望确保索引缓存处于某种已知状态（你不知道他们已经做了什么尚未检查入库的更改），因此通常你会在 *git update-index* 前面加上以下命令：

``` bash
$ git read-tree --reset HEAD
$ git update-index --refresh
```

这将强制从 `HEAD` 指向的树重建索引。它将索引内容重置为 `HEAD`，然后 *git update-index* 确保将所有索引条目与签出文件匹配。如果原始存储库的工作树中有未提交的更改，`git update-index --refresh` 将通知你需要更新它们。

​	以上内容也可以简化为：

``` bash
$ git reset
```

事实上，许多常见的 Git 命令组合可以使用 `git xyz` 接口脚本化。通过查看各种 git 脚本执行的操作，你可以学到很多东西。例如，`git reset` 以前是上述两个命令在 *git reset* 中实现的，但像 *git status* 和 *git commit* 这样的一些命令则是围绕基本的 Git 命令编写的稍复杂的脚本。

​	许多（或者说大多数？）公共远程存储库不会包含任何检出文件，甚至不会包含索引文件，它们只包含实际的 Git 核心文件。这样的存储库通常甚至没有 `.git` 子目录，而是直接包含所有 Git 文件。

​	要在本地创建这种“原始” Git 存储库的活动副本，你首先要为该项目创建自己的子目录，然后将原始存储库内容复制到 `.git` 目录中。例如，要为 Git 存储库创建自己的副本，可以执行以下操作：

``` bash
$ mkdir my-git
$ cd my-git
$ rsync -rL rsync://rsync.kernel.org/pub/scm/git/git.git/ .git
```

然后执行：

``` bash
$ git read-tree HEAD
```

来填充索引。但是，现在你填充了索引，而且有了所有的 Git 内部文件，但你会注意到实际上没有任何工作树文件可供使用。要获取这些文件，你需要使用以下命令检出它们：

``` bash
$ git checkout-index -u -a
```

其中 `-u` 标志表示希望检出保持索引是最新的（这样你就不需要之后再刷新它），`-a` 标志表示“检出所有文件”（如果你有旧版本的检出树的副本，可能还需要首先添加 `-f` 标志，告诉 *git checkout-index* 强制覆盖任何旧文件）。

​	同样，所有这些都可以简化为：

``` bash
$ git clone git://git.kernel.org/pub/scm/git/git.git/ my-git
$ cd my-git
$ git checkout
```

它会为你执行所有上述操作。

​	现在，你已经成功复制了别人（我的）的远程存储库，并检出了它。

## 创建新分支

​	在 Git 中，分支实际上只是指向 Git 对象数据库的指针，位于 `.git/refs/` 子目录中。正如我们之前讨论的，`HEAD` 分支只是一个指向这些对象指针之一的符号链接。

​	你可以随时通过选择项目历史中的任意一个点，然后将该对象的 SHA-1 名称写入 `.git/refs/heads/` 下的文件中来创建一个新分支。你可以使用任何你想要的文件名（甚至是子目录），但约定是“常规”分支叫做 `master`。但这只是一种约定，并没有强制要求。

​	以一个例子来展示，让我们回到之前用过的 git-tutorial 存储库，并在其中创建一个分支。你可以通过简单地说你想要检出一个新的分支来实现：

``` bash
$ git switch -c mybranch
```

将在当前 `HEAD` 位置创建一个新分支，并切换到它。

> 注意
>
> ​	如果你决定在历史中的某个点开始你的新分支，而不是当前的 `HEAD`，你可以告诉 *git switch* 检出的基础是什么。换句话说，如果有一个早期的标签或分支，你可以执行 `$ git switch -c mybranch earlier-commit`，它会在早期提交处创建新分支 `mybranch`，并检出该时刻的状态。

​	你随时可以通过以下方式返回到你原来的 `master` 分支：

``` bash
$ git switch master
```

（或其他任何分支名称）如果你忘记了当前所在的分支，可以简单地执行：

``` bash
$ cat .git/HEAD
```

来查看它所指向的位置。要获取你拥有的分支列表，你可以执行：

``` bash
$ git branch
```

它实际上只是围绕 `ls .git/refs/heads` 的一个简单脚本。当前所在的分支前面将有一个星号。

​	有时，你可能希望创建一个新分支，*但不*实际检出和切换到它。如果是这样，只需使用命令：

``` bash
$ git branch <branchname> [startingpoint]
```

它将仅仅创建分支，但不会执行其他任何操作。然后稍后，一旦你决定实际上要在该分支上开发，你可以使用普通的 *git switch* 命令并将分支名作为参数切换到该分支上。

## 合并两个分支

​	创建分支的一个想法是，在其中做一些（可能是实验性的）工作，最终将其合并回主分支。因此，假设你创建了上述 `mybranch`，它最初与原始的 `master` 分支相同，让我们确保我们在该分支上，并在其中做一些工作。

``` bash
$ git switch mybranch
$ echo "Work, work, work" >>hello
$ git commit -m "Some work." -i hello
```

​	在这里，我们只是在 `hello` 文件中添加了一行，我们使用了一种简写方式，直接通过文件名将 `hello` 加入到了 `git update-index` 和 `git commit` 中（使用 `-i` 标志告诉 Git 在进行提交时 *包含* 这个文件，除了在提交时已经对索引文件所做的更改）。`-m` 标志用于从命令行给出提交日志消息。

​	现在，为了让事情变得更有趣，假设其他人在原始分支上做了一些工作，并通过回到主分支并在那里以不同的方式编辑同一个文件来模拟：

``` bash
$ git switch master
```

​	在这里，花点时间查看 `hello` 的内容，并注意它不包含我们刚刚在 `mybranch` 中所做的工作，因为这个工作在 `master` 分支上并没有发生。然后执行：

``` bash
$ echo "Play, play, play" >>hello
$ echo "Lots of fun" >>example
$ git commit -m "Some fun." -i hello example
```

因为主分支显然处于更好的心情。

​	现在，你有了两个分支，并决定要合并这些工作。在我们这样做之前，让我们先介绍一个很酷的图形化工具，帮助你查看发生了什么：

``` bash
$ gitk --all
```

将以图形方式显示你两个分支的历史记录（`--all` 的意思是：通常它只会显示当前的 `HEAD`），以及它们的历史记录。你还可以看到它们是如何从一个共同源头衍生出来的。

​	无论如何，让我们退出 *gitk*（`^Q` 或者文件菜单），并决定要将我们在 `mybranch` 分支上所做的工作合并到 `master` 分支（当前也是我们的 `HEAD`）。为了做到这一点，有一个很好的脚本叫做 *git merge*，它想知道你要解决哪些分支以及合并的内容：

``` bash
$ git merge -m "Merge work in mybranch" mybranch
```

其中第一个参数将在自动解决合并时用作提交消息。

​	现在，在这种情况下，我们故意创建了一个需要手动修复的合并冲突的情况，因此 Git 会尽可能自动完成（在这种情况下只是合并了 `example` 文件，因为在 `mybranch` 分支中它没有差异），并显示：

```
	Auto-merging hello
	CONFLICT (content): Merge conflict in hello
	Automatic merge failed; fix conflicts and then commit the result.
```

​	它告诉你它执行了“自动合并”，但由于 `hello` 中存在冲突，合并失败了。

​	别担心。它将（微不足道的）冲突留在 `hello` 中，这个冲突的形式你应该已经很熟悉，如果你以前用过 CVS 的话，所以让我们只是在编辑器中打开 `hello`，然后以某种方式解决它。我建议让 `hello` 包含所有四行：

```
Hello World
It's a new day for git
Play, play, play
Work, work, work
```

​	一旦你对手动合并满意，只需执行：

``` bash
$ git commit -i hello
```

​	它会非常响亮地警告你现在正在提交一个合并（这是正确的，所以不要担心），然后你可以写一个关于你在 *git merge* 中的冒险的小合并消息。

​	完成后，再次启动 `gitk --all` 以图形方式查看历史记录。请注意，`mybranch` 仍然存在，你可以切换到它，并继续在其中工作（如果你想的话）。`mybranch` 分支将不包含这次合并，但是下次当你从 `master` 分支合并它时，Git 将知道你是如何合并它的，所以你不需要再次执行*那个*合并。

​	另一个有用的工具，特别是如果你不总是在 X-Window 环境中工作，是 `git show-branch`。

``` bash
$ git show-branch --topo-order --more=1 master mybranch
* [master] Merge work in mybranch
 ! [mybranch] Some work.
--
-  [master] Merge work in mybranch
*+ [mybranch] Some work.
*  [master^] Some fun.
```

​	前两行表示它显示两个分支以及它们的顶部提交的标题，你当前在 `master` 分支上（注意星号 `*` 字符），稍后的输出行的第一列用于显示包含在 `master` 分支中的提交，第二列用于 `mybranch` 分支。显示了三个提交以及它们的标题。所有这些提交在第一列都有非空字符（`*` 表示当前分支上的普通提交，`-` 是合并提交），这意味着它们现在是 `master` 分支的一部分。只有 "Some work" 提交在第二列中带有加号 `+` 字符，因为 `mybranch` 尚未合并以将这些提交从主分支中并入。括号内的字符串是你可以用来命名提交的短名称。在上面的示例中，*master* 和 *mybranch* 是分支的头部。`master^` 是 *master* 分支头部的第一个父提交。如果你想看到更复杂的情况，请参阅 [gitrevisions[7]](https://chat.openai.com/gitrevisions)。

> 注意
>
> ​	如果没有使用 *--more=1* 选项，*git show-branch* 将不会输出 *[master^]* 提交，因为 *[mybranch]* 提交是 *master* 和 *mybranch* 两个分支末端的共同祖先。详情请参见 [git-show-branch[1]](https://chat.openai.com/1/git-show-branch)。

> 注意
>
> ​	如果在合并后的 *master* 分支上有更多的提交，合并提交本身将不会默认显示在 *git show-branch* 中。你需要提供 `--sparse` 选项才能使合并提交在这种情况下可见。

​	现在，假设你是在 `mybranch` 中完成所有工作的人，你辛勤工作的成果最终已经合并到了 `master` 分支。让我们回到 `mybranch`，运行 *git merge* 来将“上游的更改”合并回你的分支。

``` bash
$ git switch mybranch
$ git merge -m "Merge upstream changes." master
```

​	这将输出类似于以下内容（实际的提交对象名称将会不同）：

```
Updating from ae3a2da... to a80b4aa....
Fast-forward (no commit created; -m option ignored)
 example | 1 +
 hello   | 1 +
 2 files changed, 2 insertions(+)
```

​	由于你的分支中没有包含任何比已经合并到 `master` 分支中更多的内容，合并操作实际上没有进行合并。相反，它只是将你的分支的顶部更新到了 `master` 分支的顶部。这通常被称为*快进合并*。

​	你可以再次运行 `gitk --all` 来查看提交的衍生关系，或者运行 *show-branch*，它会告诉你这个情况。

``` bash
$ git show-branch master mybranch
! [master] Merge work in mybranch
 * [mybranch] Merge work in mybranch
--
-- [master] Merge work in mybranch
```

## 合并外部工作

​	通常，你更可能与他人合并而不是与自己的分支合并，因此值得指出，Git 也非常容易实现合并，事实上，它与执行 *git merge* 并没有太大的区别。实际上，远程合并最终实际上只是“从远程存储库获取工作到一个临时标签”，然后再执行 *git merge*。

​	从远程存储库获取内容是通过 *git fetch* 完成的：

``` bash
$ git fetch <remote-repository>
```

​	可以使用以下传输方式来命名要下载的存储库：

- SSH

  `remote.machine:/path/to/repo.git/` 或 `ssh://remote.machine/path/to/repo.git/`这种传输方式可用于上传和下载，并且需要你在远程机器上拥有 `ssh` 的登录权限。它通过交换两端都有的 head 提交来找出对方缺少的对象集，并传输（接近）最小的对象集。这是迄今为止在仓库之间交换 Git 对象的最有效方式。
- Local directory 本地目录

  `/path/to/repo.git/`这种传输方式与 SSH 传输方式相同，但使用 *sh* 在本地机器上运行两端，而不是通过 *ssh* 在远程机器上运行另一端。
- Git Native  Git 本地传输

  `git://remote.machine/path/to/repo.git/`这种传输方式是为匿名下载而设计的。与 SSH 传输方式一样，它找出了下游端缺少的对象集，并传输（接近）最小的对象集。
- HTTP(S)

  `http://remote.machine/path/to/repo.git/`从 http 和 https URL 下载的程序首先通过查看 `repo.git/refs/` 目录下指定的 refname 获取远程站点的顶层提交对象名称，然后通过使用该提交对象的对象名称从 `repo.git/objects/xx/xxx...` 下载提交对象。然后它读取提交对象以查找其父提交和关联的树对象；它重复这个过程，直到获取所有必需的对象。由于这种行为，它们有时也被称为*提交步行器*。*提交步行器*有时也被称为*笨传输*，因为它们不需要像 Git 本地传输那样需要任何 Git 意识的智能服务器。任何不支持目录索引的基本 HTTP 服务器都可以。但你必须准备好你的仓库，帮助*笨传输*下载程序通过 *git update-server-info*。

​	一旦你从远程存储库获取内容，你就可以将其与当前分支进行合并。

​	然而，这种“获取”然后立即“合并”的情况非常常见，因此称为 `git pull`，你只需要执行以下命令即可：

``` bash
$ git pull <remote-repository>
```

并且可以选择将远程端的分支名作为第二个参数提供。

> 注意
>
> ​	你甚至可以完全不使用任何分支，只需保留尽可能多的本地仓库，每个仓库都对应一个分支，并使用*git pull*在它们之间进行合并，就像你在不同分支之间进行合并一样。这种方法的优点是可以让你为每个分支保持一组文件，每次在多个开发线上进行切换可能更容易。当然，你会付出更多的磁盘使用费用来保持多个工作树，但是如今的磁盘空间很便宜。

​	很可能你会时不时地从同一个远程存储库进行拉取。作为一种简便方法，你可以将远程存储库的 URL 存储在本地存储库的配置文件中，如下所示：

``` bash
$ git config remote.linus.url http://www.kernel.org/pub/scm/git/git.git/
```

然后在 *git pull* 中使用“linus”关键字，而不是使用完整的 URL。

​	示例：

1. `git pull linus`
2. `git pull linus tag v0.99.1`

上述示例等同于：

1. `git pull http://www.kernel.org/pub/scm/git/git.git/ HEAD`
2. `git pull http://www.kernel.org/pub/scm/git/git.git/ tag v0.99.1`

## 合并的工作原理是怎样的？

​	我们说过，本教程展示了“下水道”是如何帮助你应对不能冲洗的“瓷器”，但到目前为止，我们还没有讨论合并的工作原理。如果你是第一次阅读本教程，我建议你跳转到“发布你的工作”一节，稍后再回到这里。

​	好的，还跟着我吗？为了给我们提供一个示例来参考，让我们回到之前的存储库，其中包含“hello”和“example”文件，并将自己带回到合并前的状态：

``` bash
$ git show-branch --more=2 master mybranch
! [master] Merge work in mybranch
 * [mybranch] Merge work in mybranch
--
-- [master] Merge work in mybranch
+* [master^2] Some work.
+* [master^] Some fun.
```

​	记住，在运行 *git merge* 之前，我们的 `master` 头指针位于“Some fun.” 提交，而 `mybranch` 头指针位于“Some work.” 提交。

``` bash
$ git switch -C mybranch master^2
$ git switch master
$ git reset --hard master^
```

​	倒回后，提交结构应该如下所示：

``` bash
$ git show-branch
* [master] Some fun.
 ! [mybranch] Some work.
--
*  [master] Some fun.
 + [mybranch] Some work.
*+ [master^] Initial commit
```

​	现在我们准备手动进行合并实验。

​	`git merge` 命令在合并两个分支时使用3路合并算法。首先，它找到它们之间的共同祖先。它使用的命令是 *git merge-base*：

``` bash
$ mb=$(git merge-base HEAD mybranch)
```

​	该命令将共同祖先的提交对象名称写入标准输出，所以我们将其输出捕获到一个变量中，因为我们将在下一步中使用它。顺便说一下，在这种情况下，共同祖先提交是“Initial commit”提交。你可以通过以下命令查看：

``` bash
$ git name-rev --name-only --tags $mb
my-first-tag
```

​	找到共同祖先提交后，第二步是：

``` bash
$ git read-tree -m -u $mb HEAD mybranch
```

​	这是我们之前已经见过的相同的 *git read-tree* 命令，但是与之前的例子不同，它接受三个树，而不是两个。这将把每个树的内容读入索引文件中的不同*阶段*（第一个树放在阶段1，第二个树放在阶段2，依此类推）。将三个树读入三个阶段后，在所有三个阶段中相同的路径被*折叠*到阶段0中。还有在两个阶段中相同的路径也会被折叠到阶段0中，并从阶段2或阶段3中取出 SHA-1 值，取决于哪个与阶段1不同（即只有一个分支与共同祖先不同）。

​	在*折叠*操作之后，三个树中不同的路径将保留在非零阶段中。此时，你可以使用以下命令检查索引文件：

``` bash
$ git ls-files --stage
100644 7f8b141b65fdcee47321e399a2598a235a032422 0	example
100644 557db03de997c86a4a028e1ebd3a1ceb225be238 1	hello
100644 ba42a2a96e3027f3333e13ede4ccf4498c3ae942 2	hello
100644 cc44c73eb783565da5831b4d820c962954019b69 3	hello
```

​	在我们的两个文件的示例中，我们没有未更改的文件，所以只有 *example* 会进行折叠。但是在实际的大型项目中，当一次提交中只有少量文件发生变化时，这种*折叠*会快速地将大部分路径合并在一起，留下只有少量实际更改的非零阶段。

​	要查看仅有非零阶段的内容，使用 `--unmerged` 标志：

``` bash
$ git ls-files --unmerged
100644 557db03de997c86a4a028e1ebd3a1ceb225be238 1	hello
100644 ba42a2a96e3027f3333e13ede4ccf4498c3ae942 2	hello
100644 cc44c73eb783565da5831b4d820c962954019b69 3	hello
```

​	合并的下一步是使用3路合并来合并这三个文件版本。这是通过将 *git merge-one-file* 命令作为 *git merge-index* 命令的参数之一来完成的：

``` bash
$ git merge-index git-merge-one-file hello
Auto-merging hello
ERROR: Merge conflict in hello
fatal: merge program failed
```

​	*git merge-one-file* 脚本被调用，并带有描述这三个版本的参数，并负责将合并结果留在工作树中。这是一个相当简单的 shell 脚本，并最终调用 RCS 套件中的 *merge* 程序来执行文件级别的3路合并。在这种情况下，*merge* 检测到冲突，并在工作树中留下具有冲突标记的合并结果。如果此时再次运行 `ls-files --stage`，可以看到：

``` bash
$ git ls-files --stage
100644 7f8b141b65fdcee47321e399a2598a235a032422 0	example
100644 557db03de997c86a4a028e1ebd3a1ceb225be238 1	hello
100644 ba42a2a96e3027f3333e13ede4ccf4498c3ae942 2	hello
100644 cc44c73eb783565da5831b4d820c962954019b69 3	hello
```

​	这是 *git merge* 将控制权交还给你后的索引文件和工作文件状态，留下了待你解决的冲突合并。请注意，路径 `hello` 仍然处于未合并状态，在这一点上，使用 *git diff* 查看的是与阶段2（即你的版本）的差异。

## 发布你的工作

​	因此，我们可以使用来自远程存储库的他人的工作，但是**你**该如何准备一个存储库以便让其他人从中拉取内容？

​	你在你的工作树中进行真正的工作，工作树的主要存储库则悬挂在其中作为它的`.git`子目录。**你本可以**使该存储库可以远程访问，并要求其他人从中拉取内容，但在实际情况下，通常不是这样做的。一个推荐的方式是拥有一个公共存储库，使其对其他人可见，当你在主要工作树中的更改已经准备就绪时，可以从中更新公共存储库。这通常称为*推送（pushing）*。

> 注意
>
> ​	这个公共存储库还可以进一步镜像，这就是 `kernel.org` 上的 Git 存储库是如何管理的。

​	将你的本地（私有）存储库的更改发布到远程（公共）存储库需要在远程机器上具有写权限。你需要在那里拥有一个SSH帐户，以便运行单个命令 *git-receive-pack*。

​	首先，在远程机器上创建一个空存储库，用于容纳你的公共存储库。稍后，通过向其推送内容，这个空存储库将被填充并保持最新状态。显然，只需要进行一次此操作。

> 注意
>
> ​	*git push* 使用一对命令，*git send-pack* 在你的本地机器上，以及*git-receive-pack*在远程机器上。两者之间的网络通信在内部使用SSH连接。

​	你的私有存储库的Git目录通常是 `.git`，而公共存储库通常以项目名称命名，例如 `<project>.git`。让我们为项目`my-git`创建这样一个公共存储库。登录到远程机器后，创建一个空目录：

``` bash
$ mkdir my-git.git
```

​	然后，通过运行 *git init* 将该目录变成一个Git存储库，但这次由于其名称不是通常的`.git`，我们稍微有些不同的操作：

``` bash
$ GIT_DIR=my-git.git git init
```

​	确保这个目录可以通过你选择的传输方式对其他人可见。同时，你需要确保你的 `$PATH` 中有 *git-receive-pack* 程序。

> 注意
>
> ​	许多sshd安装在直接运行程序时不会调用你的登录shell作为登录shell；这意味着如果你的登录shell是*bash*，那么只有`.bashrc`会被读取，而不会读取`.bash_profile`。为了解决这个问题，请确保`.bashrc`设置了 `$PATH`，这样你就可以运行 *git-receive-pack* 程序。

> 注意
>
> ​	如果你计划将此存储库发布为通过http访问的存储库，那么现在应该执行 `mv my-git.git/hooks/post-update.sample my-git.git/hooks/post-update`。这将确保每次你向这个存储库推送内容时，都会运行`git update-server-info`。

​	你的“公共存储库”现在已经准备好接受你的更改。回到拥有你的私有存储库的机器。然后运行此命令：

``` bash
$ git push <public-host>:/path/to/my-git.git master
```

​	这将使你的公共存储库与命名分支头（在这个例子中是 `master`）以及你当前存储库中可访问的对象保持同步。

​	作为一个真实的示例，这是我如何更新我的公共Git存储库。Kernel.org的镜像网络会负责将更改传播到其他公开可见的机器：

``` bash
$ git push master.kernel.org:/pub/scm/git/git.git/
```

## 打包你的存储库

​	之前，我们看到`.git/objects/??/`目录下的每个Git对象存储了一个文件。这种表示方式在创建时非常高效且安全，但在网络传输时不那么方便。由于Git对象一旦创建就是不可变的，因此有一种方法可以通过“将它们打包在一起”来优化存储。命令

``` bash
$ git repack
```

​	可以为你完成这个过程。如果你按照本教程的示例进行操作，现在你的`.git/objects/??/`目录中累积了大约17个对象。*git repack* 命令会告诉你它打包了多少个对象，并将打包文件存储在`.git/objects/pack`目录中。

> 注意
>
> ​	在`.git/objects/pack`目录中你会看到两个文件：`pack-*.pack` 和 `pack-*.idx`。它们彼此密切相关，如果出于某种原因你需要手动将它们复制到另一个存储库中，你应该确保它们一起复制。前者包含打包中所有对象的数据，而后者包含随机访问的索引。

​	如果你比较担心，运行 *git verify-pack* 命令可以检测是否有损坏的打包文件，但不用太担心。我们的程序总是完美的 ;-)

​	一旦你打包了对象，你就不需要再保留包含在打包文件中的未打包对象。

``` bash
$ git prune-packed
```

将会帮你删除它们。

​	如果你感兴趣，你可以在运行 `git prune-packed` 前后尝试运行 `find .git/objects -type f`。另外，`git count-objects` 命令会告诉你你的存储库中有多少未打包对象，以及它们占用了多少空间。

> 注意
>
> ​	对于 HTTP 传输，`git pull` 稍显麻烦，因为打包的存储库可能包含相对较少的对象但是却有一个相对较大的打包文件。如果你希望从你的公共存储库中进行许多HTTP拉取，你可能需要经常进行重打包和裁剪，或者从不进行。

​	如果此时再次运行 `git repack`，它会显示“Nothing new to pack.”。当你继续开发并积累更改时，再次运行 `git repack` 将创建一个新的打包文件，其中包含自你上次打包存储库以来创建的对象。我们建议你在初始导入后尽快打包你的项目（除非你从头开始创建项目），然后根据项目的活动程度定期运行 `git repack`。

​	当存储库通过 `git push` 和 `git pull` 进行同步时，源存储库中打包的对象通常以未打包的形式存储在目标存储库中。虽然这样可以在两端使用不同的打包策略，但这也意味着你可能需要定期重新打包两个存储库。

## 与他人合作

​	尽管Git是一个真正的分布式系统，但是通常使用一个非正式的开发者层次结构来组织项目。Linux内核的开发就是以这种方式进行的。在[Randy Dunlap的演示文稿](https://web.archive.org/web/20120915203609/http://www.xenotime.net/linux/mentor/linux-mentoring-2006.pdf)中，有一个很好的示例（第17页，“Merges to Mainline”）。

​	应该强调的是，这种层次结构是纯粹**非正式**的。Git中没有任何东西强制执行这种层次结构所暗示的“补丁流”的链条。你不必从一个远程存储库中拉取内容。

​	“项目领导者”的推荐工作流程如下：

1. 在你的本地机器上准备好你的主要存储库。你的工作都在这里完成。
2. 准备一个供其他人访问的公共存储库。

   如果其他人通过低级传输协议（HTTP）从你的存储库拉取内容，你需要确保这个存储库是“低级传输友好”的。在`git init`之后，从标准模板中复制`$GIT_DIR/hooks/post-update.sample`，它会包含一个对*git update-server-info*的调用，但你需要手动启用这个hook，使用`mv post-update.sample post-update`。这将确保*git update-server-info*保持必要的文件最新。
5. 从你的主要存储库推送到公共存储库。
7. 对公共存储库进行*git repack*。这会创建一个大的打包文件，其中包含初始对象集作为基线，并可能包含*git prune*（如果用于从存储库拉取的传输支持打包存储库）。
9. 在你的主要存储库中继续工作。你的更改包括你自己的修改、通过电子邮件接收的补丁以及从你的“子系统维护者”的“公共”存储库拉取后产生的合并。

   你可以随时在私有存储库中重新打包。
11. 将你的更改推送到公共存储库，并向公众宣布。
13. 每隔一段时间，对公共存储库进行*git repack*。然后回到第5步并继续工作。

​	一个“子系统维护者”推荐的工作循环是这样的，他在该项目上工作并拥有一个自己的“公共存储库”：

1. 通过在“项目领导者”的公共存储库上运行 *git clone* 来准备你的工作存储库。用于初始克隆的URL保存在 `remote.origin.url` 配置变量中。
3. 准备一个供他人访问的公共存储库，就像“项目领导者”一样。
5. 将“项目领导者”的公共存储库中的打包文件复制到你的公共存储库中，除非“项目领导者”的存储库与你的机器位于同一台机器上。在这种情况下，你可以使用`objects/info/alternates`文件指向你借用的存储库。
7. 从你的主要存储库向公共存储库推送内容。运行 *git repack*，并且如果用于从你的存储库拉取的传输支持打包存储库，则可能运行 *git prune*。
9. 在你的主要存储库中继续工作。你的更改包括你自己的修改、通过电子邮件接收的补丁，以及从“项目领导者”的“公共”存储库以及可能是你的“子系统维护者”的“公共”存储库拉取后产生的合并。

   你可以随时在私有存储库中重新打包。
11. 将你的更改推送到你的公共存储库，并要求你的“项目领导者”和可能的“子系统维护者”从其中拉取内容。
13. 每隔一段时间，对公共存储库进行*git repack*。然后回到第5步并继续工作。

​	对于一个没有“公共”存储库的“个人开发者”，推荐的工作循环有所不同，具体如下：

1. 通过*git clone*“项目领导者”的公共存储库（或如果你在子系统上工作，则是“子系统维护者”的存储库）来准备你的工作存储库。用于初始克隆的URL保存在 `remote.origin.url` 配置变量中。
3. 在*master*分支上的你的存储库中进行工作。
5. 时不时地从上游的公共存储库运行 `git fetch origin`。这只执行`git pull`的前半部分，不进行合并。公共存储库的头部保存在`.git/refs/remotes/origin/master`中。
7. 使用 `git cherry origin` 查看哪些补丁已被接受，或者使用 `git rebase origin` 将未合并的更改移植到更新后的上游。
9. 使用 `git format-patch origin` 准备补丁，然后将其发送给上游以供审阅。然后回到第2步并继续。

## 与他人合作，共享存储库风格

​	如果你来自CVS的背景，前面提到的合作风格可能对你来说是新的。不用担心，Git也支持你可能更熟悉的“共享公共存储库”合作风格。

​	详情请查看 [gitcvs-migration[7]](https://chat.openai.com/gitcvs-migration)。

## 将你的工作打包在一起

​	很可能你会同时处理多个任务。使用Git来管理这些相对独立的任务非常简便，你可以使用分支来管理它们。

​	我们之前已经了解了分支的工作原理，使用了两个分支的“fun and work”示例。如果有多于两个分支，原理是一样的。假设你从“master”分支出发，在“master”分支上有一些新代码，而在“commit-fix”和“diff-fix”两个分支上有两个独立的修复：

``` bash
$ git show-branch
! [commit-fix] Fix commit message normalization.
 ! [diff-fix] Fix rename detection.
  * [master] Release candidate #1
---
 +  [diff-fix] Fix rename detection.
 +  [diff-fix~1] Better common substring algorithm.
+   [commit-fix] Fix commit message normalization.
  * [master] Release candidate #1
++* [diff-fix~2] Pretty-print messages.
```

​	这两个修复都经过了充分的测试，现在你想要将它们都合并。你可以先合并 *diff-fix* 然后再合并 *commit-fix*，像这样：

``` bash
$ git merge -m "Merge fix in diff-fix" diff-fix
$ git merge -m "Merge fix in commit-fix" commit-fix
```

​	这将导致：

``` bash
$ git show-branch
! [commit-fix] Fix commit message normalization.
 ! [diff-fix] Fix rename detection.
  * [master] Merge fix in commit-fix
---
  - [master] Merge fix in commit-fix
+ * [commit-fix] Fix commit message normalization.
  - [master~1] Merge fix in diff-fix
 +* [diff-fix] Fix rename detection.
 +* [diff-fix~1] Better common substring algorithm.
  * [master~2] Release candidate #1
++* [master~3] Pretty-print messages.
```

​	然而，没有特定的理由要先合并一个分支，然后再合并另一个分支，特别是当你有一组真正独立的更改时（如果顺序很重要，那么它们就不是独立的）。你可以选择一次将这两个分支合并到当前分支上。首先，让我们撤销刚才的操作并重新开始。我们希望在合并这两个分支之前，将 master 分支恢复到 `master~2`：

``` bash
$ git reset --hard master~2
```

​	你可以确保 `git show-branch` 与你刚刚运行 *git merge* 命令前的状态相匹配。然后，不再运行两个 *git merge* 命令，而是将这两个分支头合并（这被称为 *Octopus 合并*）：

``` bash
$ git merge commit-fix diff-fix
$ git show-branch
! [commit-fix] Fix commit message normalization.
 ! [diff-fix] Fix rename detection.
  * [master] Octopus merge of branches 'diff-fix' and 'commit-fix'
---
  - [master] Octopus merge of branches 'diff-fix' and 'commit-fix'
+ * [commit-fix] Fix commit message normalization.
 +* [diff-fix] Fix rename detection.
 +* [diff-fix~1] Better common substring algorithm.
  * [master~1] Release candidate #1
++* [master~2] Pretty-print messages.
```

​	请注意，不要仅仅因为你可以做 Octopus 合并就去这样做。Octopus 是有效的，如果你同时合并了两个以上的独立更改，这样做会使查看提交历史更容易。但是，如果你在合并的分支中有冲突，并且需要手动解决冲突，那就意味着那些分支中的开发实际上并不是独立的。这时你应该一次合并两个分支，记录你如何解决冲突以及为什么你更喜欢某一方所做的更改。否则，项目历史将变得更难追踪，而不是更容易。

## 另请参阅

[gittutorial[7]](../gittutorial), [gittutorial-2[7]](../gittutorial-2), [gitcvs-migration[7]](../gitcvs-migration), [git-help[1]](../../1/git-help), [giteveryday[7]](../giteveryday), [The Git User’s Manual](https://git-scm.com/docs/user-manual)

## GIT

  这是[git[1]](../../Git)工具集中的一部分。
