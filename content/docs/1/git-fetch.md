+++
title = "git-fetch"
weight = 30
type = "docs"
date = 2023-05-08T15:39:23+08:00
description = ""
isCJKLanguage = true
draft = false

+++

# git-fetch

https://git-scm.com/docs/git-fetch

version 2.41.0

## 名称

​	git-fetch - 从另一个仓库下载对象和引用

## 概述

```
git fetch [<options>] [<repository> [<refspec>…]]
git fetch [<options>] <group>
git fetch --multiple [<options>] [(<repository> | <group>)…]
git fetch --all [<options>]
```

## 描述

​	从一个或多个其他仓库中获取分支和/或标签（统称为“引用”），以及完成它们历史所需的对象。远程跟踪分支将被更新（有关控制此行为的<refspec>的描述，请参见下文）。

​	默认情况下，还会获取指向正在获取的历史记录的任何标签；其效果是获取指向您感兴趣的分支的标签。通过使用 --tags 或 --no-tags 选项，或者配置 remote.<name>.tagOpt，可以更改此默认行为。通过使用显式获取标签的 refspec，还可以获取不指向您感兴趣分支的标签。

​	*git fetch* 可以从单个命名仓库或URL中获取，或者如果提供了<group>并且在配置文件中存在 remotes.<group> 条目，则可以同时从多个仓库中获取（请参阅 [git-config[1]](../git-config)）。

​	当未指定远程时，默认情况下将使用 `origin` 远程，除非为当前分支配置了上游分支。

​	获取的引用名称及其指向的对象名称将被写入 `.git/FETCH_HEAD` 文件。此信息可由脚本或其他 git 命令（例如 [git-pull[1]](../git-pull)）使用。

## 选项

- `--all`

  获取所有远程。

- -a

- `--append`

  将获取的引用名称和对象名称追加到 `.git/FETCH_HEAD` 文件的现有内容中。如果没有此选项，则会覆盖 `.git/FETCH_HEAD` 文件中的旧数据。

- `--atomic`

  使用原子事务来更新本地引用。要么全部更新引用，要么在出现错误时不更新引用。

- `--depth=<depth>`

  限制从每个远程分支历史的顶部获取指定数量的提交。如果在使用 `git clone` 命令创建的*shallow* 仓库上（使用 `--depth=<depth>` 选项，请参阅 [git-clone[1]](../git-clone)）进行获取，则会将历史深化或缩短到指定数量的提交。深化的提交的标签不会被获取。

- `--deepen=<depth>`

  类似于 --depth，但它指定从当前浅边界开始的提交数量，而不是从每个远程分支历史的顶部开始。

- `--shallow-since=<date>`

  将浅仓库的历史深化或缩短到包括 <date> 之后的所有可达提交。

- `--shallow-exclude=<revision>`

  将浅仓库的历史深化或缩短，以排除从指定的远程分支或标签可达的提交。此选项可以指定多次。

- `--unshallow`

  如果源仓库是完整的，则将浅仓库转换为完整仓库，去除浅仓库的所有限制。

  如果源仓库是浅仓库，则尽可能地获取数据，使得当前仓库具有与源仓库相同的历史记录。

- `--update-shallow`

  默认情况下，在从浅仓库获取数据时，`git fetch` 将拒绝需要更新 .git/shallow 的引用。使用此选项会更新 .git/shallow 并接受这些引用。

- `--negotiation-tip=<commit|glob>`

  默认情况下，Git 会向服务器报告从所有本地引用可达的提交，以查找共同的提交，从而尝试减少待接收的 packfile 的大小。如果指定，Git 将仅报告从给定提示可达的提交。当用户知道哪个本地引用可能与正在获取的上游引用有共同的提交时，这对于加快获取速度非常有用。

  此选项可以多次指定；如果是这样，Git 将报告从给定的任何提交可达的提交。

  此选项的参数可以是引用名称的 glob、引用或提交的（可能是缩写的）SHA-1 值。指定 glob 等效于多次指定此选项，每次指定一个匹配的引用名称。

  请参阅 [git-config[1]](../git-config) 中记录的 `fetch.negotiationAlgorithm` 和 `push.negotiate` 配置变量，以及下面介绍的 `--negotiate-only` 选项。

- `--negotiate-only`

  不从服务器获取任何内容，而是打印与提供的 `--negotiation-tip=*` 参数共有的祖先提交。

  这与 `--recurse-submodules=[yes|on-demand]` 不兼容。在内部，它用于实现 `push.negotiate` 选项，请参阅 [git-config[1]](../git-config)。

- `--dry-run`

  显示将要执行的操作，而不进行任何更改。

- `--[no-]write-fetch-head`

  直接在 `$GIT_DIR` 下的 `.git/FETCH_HEAD` 文件中写入获取的远程引用列表。这是默认行为。从命令行传递 `--no-write-fetch-head` 选项会告诉 Git 不写入该文件。在 `--dry-run` 选项下，该文件永远不会被写入。

- -f

- `--force`

  当使用 `<src>:<dst>` refspec 执行 *git fetch* 时，它可能会拒绝更新本地分支，具体参见下文的 `<refspec>` 部分。此选项将覆盖该检查。

- -k

- `--keep`

  保留下载的 pack。

- `--multiple`

  允许同时指定多个 <仓库> 和 <组> 参数。不能指定 <refspec>。

- `--[no-]auto-maintenance`

- `--[no-]auto-gc`

  在最后运行 `git maintenance run --auto` 以执行需要的自动仓库维护。（`--[no-]auto-gc` 是一个同义词。）默认启用此选项。

- `--[no-]write-commit-graph`

  获取后写入提交图。这将覆盖配置设置 `fetch.writeCommitGraph`。

- `--prefetch`

  修改配置的 refspec，将所有引用放入 `refs/prefetch/` 命名空间。

  参见 [git-maintenance[1]](../git-maintenance) 中的 `prefetch` 任务。

- -p

- `--prune`

  获取前，删除不再存在于远程的任何远程跟踪引用。如果仅因为默认标签自动跟踪或使用 --tags 选项而获取标签，则标签不会被修剪。但是，如果标签是因为显式 refspec 被获取的（无论是在命令行上还是在远程配置中，例如如果使用 --mirror 选项克隆了远程），则它们也会被修剪。通过提供 `--prune-tags` 为提供 tag refspec 提供了一个快捷方式。

  有关更多详细信息，请参见下面的 PRUNING 部分。

- -P

- `--prune-tags`

  获取前，如果启用了 `--prune`，则删除不再存在于远程的任何本地标签。与 `--prune` 一起使用时，请更加谨慎，因为它将删除已创建的任何本地引用（本地标签）。此选项是提供显式标签 refspec 以及 `--prune` 的简便方式，请参见有关其文档的讨论。有关更多详细信息，请参见下面的 PRUNING 部分。

- -n

- `--no-tags`

  默认情况下，会获取指向从远程仓库下载的对象的标签，并在本地存储。此选项禁用此自动标签跟踪。可使用 remote.<name>.tagOpt 设置指定远程的默认行为。请参阅 [git-config[1]](../git-config)。

- `--refetch`

  与服务器协商以避免传输本地已经存在的提交和相关对象，此选项会像重新克隆一样获取所有对象。使用此选项重新应用配置中的部分克隆过滤器或使用 `--filter=` 更改过滤器定义。自动的后续获取维护将执行对象数据库的打包合并以删除任何重复的对象。

- `--refmap=<refspec>`

  在命令行上列出的引用进行获取时，使用指定的 refspec（可以多次给出）将引用映射到远程跟踪分支，而不是为远程仓库的 `remote.*.fetch` 配置变量的值。将一个空的 `<refspec>` 提供给 `--refmap` 选项会导致 Git 忽略配置的 refspec，并完全依赖于作为命令行参数提供的 refspec。有关详细信息，请参见 "已配置的远程跟踪分支" 部分。

- -t

- `--tags`

  从远程获取所有标签（即将远程标签 `refs/tags/*` 获取为具有相同名称的本地标签），除了其他将被获取的内容。仅使用此选项不会使标签被修剪，即使使用了 --prune（尽管如果标签也是显式 refspec 的目标，则标签可能仍然会被修剪；请参阅 `--prune`）。

- `--recurse-submodules[=yes|on-demand|no]`

  此选项控制是否以及在何种条件下还应获取子模块的新提交。在递归遍历子模块时，`git fetch` 总是尝试获取“更改”的子模块，即一个子模块具有提交，这些提交在新获取的超级仓库提交中被引用，但在本地子模块克隆中缺失。只要更改的子模块存在于本地，例如在 `$GIT_DIR/modules/` 中存在（请参阅 [gitsubmodules[7]](../7/gitsubmodules)）；如果上游添加了一个新的子模块，则该子模块不能被获取，直到它被克隆，例如通过 `git submodule update`。

  当设置为 *on-demand* 时，只会获取更改的子模块。当设置为 *yes* 时，将获取所有已填充的子模块，并获取未填充但已更改的子模块。当设置为 *no* 时，将不获取子模块。

  当未指定此选项时，如果设置了 `fetch.recurseSubmodules` 的值（请参阅 [git-config[1]](../git-config)），则使用该值，默认为 *on-demand*。当此选项在没有任何值的情况下使用时，默认值为 *yes*。

- -j

- `--jobs=<n>`

  用于所有形式的获取的并行子任务数。

  如果指定了 `--multiple` 选项，不同的远程将并行获取。如果获取多个子模块，则将并行获取它们。要独立控制它们，使用配置设置 `fetch.parallel` 和 `submodule.fetchJobs`（请参阅 [git-config[1]](../git-config)）。

  通常，并行递归和多远程获取将更快。默认情况下，获取是顺序执行的，而不是并行执行。

- `--no-recurse-submodules`

  禁用子模块的递归获取（这与使用 `--recurse-submodules=no` 选项具有相同的效果）。

- `--set-upstream`

  如果成功获取远程仓库，则添加上游（跟踪）引用，供不带参数的 [git-pull[1]](../git-pull) 和其他命令使用。有关详细信息，请参阅 [git-config[1]](../git-config) 中的 `branch.<name>.merge` 和 `branch.<name>.remote`。

- `--submodule-prefix=<path>`

  在信息性消息（如 "Fetching submodule foo"）中，将 `<path>` 前置到打印的路径。此选项在递归子模块时内部使用。

- `--recurse-submodules-default=[yes|on-demand]`

  此选项在内部用于临时为 `--recurse-submodules` 选项提供非负默认值。所有其他配置 fetch 子模块递归的方法（例如 [gitmodules[5]](../5/gitmodules) 和 [git-config[1]](../git-config) 中的设置）会覆盖此选项，直接指定 `--[no-]recurse-submodules` 也会覆盖该选项。

- -u

- `--update-head-ok`

  默认情况下，*git fetch* 拒绝更新与当前分支对应的 head。此标志禁用此检查。这仅用于 *git pull* 与 *git fetch* 之间的内部通信，除非你正在实现自己的 Porcelain，否则不应使用它。

- --upload-pack <upload-pack>

  当给出并且要获取的仓库由 *git fetch-pack* 处理时，会将 `--exec=<upload-pack>` 传递给命令，以指定在另一端运行的命令的非默认路径。

- -q

- `--quiet`

  将 --quiet 传递给 git-fetch-pack，并使任何其他内部使用的 git 命令静音。进度不会报告到标准错误流。

- -v

- `--verbose`

  Be verbose.

  显示详细信息。

- `--progress`

  默认情况下，当附加到终端时，进度状态会在标准错误流上报告，除非指定了 -q。此标志强制进度状态，即使标准错误流未指向终端。

- -o <option>

- `--server-option=<option>`

  在使用协议版本 2 进行通信时，将给定的字符串传递给服务器。给定的字符串不能包含 NUL 或 LF 字符。服务器对服务器选项的处理（包括未知选项）是特定于服务器的。当给出多个 `--server-option=<option>` 时，它们按照命令行上列出的顺序全部发送到对端。

- `--show-forced-updates`

  默认情况下，git 在获取时会检查分支是否被强制更新。可以通过 fetch.showForcedUpdates 禁用此功能，但 --show-forced-updates 选项保证执行此检查。请参阅 [git-config[1]](../git-config)。

- `--no-show-forced-updates`

  默认情况下，git 在获取时会检查分支是否被强制更新。传递 --no-show-forced-updates 或将 fetch.showForcedUpdates 设置为 false 可以出于性能原因跳过此检查。如果在 *git-pull* 中使用，则 --ff-only 选项仍然会在尝试快进更新之前检查强制更新。请参阅 [git-config[1]](../git-config)。

- -4

- `--ipv4`

  仅使用 IPv4 地址，忽略 IPv6 地址。

- -6

- `--ipv6`

  仅使用 IPv6 地址，忽略 IPv4 地址。

- <repository>

  "远程" 仓库，是获取或拉取操作的源头。此参数可以是 URL（请参阅下文的 [GIT URLS](https://git-scm.com/docs/git-fetch#URLS) 部分）或远程的名称（请参阅下文的 [REMOTES](https://git-scm.com/docs/git-fetch#REMOTES) 部分）。

- <group>

  引用 remotes.<group> 配置文件中的仓库列表的名称。（请参阅 [git-config[1]](../git-config)。）

- <refspec>

  ​	指定要获取的引用以及要更新的本地引用。当命令行上没有 <refspec> 时，要获取的引用将从 `remote.<repository>.fetch` 变量中读取，参见下文的 [CONFIGURED REMOTE-TRACKING BRANCHES](https://git-scm.com/docs/git-fetch#CRTB)部分。

  <refspec> 参数的格式是一个可选的加号 `+`，后跟源 <src>，再后跟冒号 `:`，再后跟目标引用 <dst>。当 <dst> 为空时，可以省略冒号。通常，<src> 是一个引用，但也可以是完整的十六进制对象名称。

  ​	<refspec> 可以在 <src> 中包含 `*`，表示简单模式匹配。这样的 refspec 的功能类似于匹配具有相同前缀的任何引用的 glob。模式 <refspec> 必须在 <src> 和 <dst> 中都有一个 `*`。它将引用映射到目标，通过用源匹配内容替换 `*`。

  ​	如果 refspec 以 `^` 为前缀，则将其解释为负 refspec。这样的 refspec 不会指定要获取的引用或要更新的本地引用，而是指定要排除的引用。如果引用与至少一个正 refspec 匹配，并且不匹配任何负 refspec，则将被视为匹配引用。负 refspec 可以用于限制模式 refspec 的范围，使其不包括特定引用。负 refspec 本身可以是模式 refspec。但是，它们只能包含一个 <src>，不指定 <dst>。也不支持完整拼写的十六进制对象名称。

  ​	`tag <tag>` 的意思与 `refs/tags/<tag>:refs/tags/<tag>` 相同；它请求获取直到给定的标签的所有内容。

  ​	与 <src> 匹配的远程引用将被获取，如果 <dst> 不为空，则尝试更新与之匹配的本地引用。

  ​	是否允许不使用 `--force` 进行此更新取决于它正在获取到的引用命名空间、正在获取的对象类型以及更新是否被视为快进。通常，在获取时应用与推送时相同的规则，请参阅 [git-push[1]](../git-push) 中的 `<refspec>...` 部分以了解这些规则。*git fetch* 的特定异常规则将在下面进行说明。

  ​	在 Git 版本 2.20 之前，与使用 [git-push[1]](../git-push) 进行推送不同，将接受所有 `refs/tags/*` 的更新，而不需要在 refspec 中使用 `+`（或 `--force`）。当获取时，我们普遍地认为来自远程的所有标签更新都是强制获取。自 Git 版本 2.20 起，获取以更新 `refs/tags/*` 的方式与推送相同。即。任何更新将在 refspec 中没有 `+`（或 `--force`）的情况下被拒绝。

  ​	与使用 [git-push[1]](../git-push) 进行推送不同，任何在 `refs/{tags,heads}/*` 之外的更新将在 refspec 中没有 `+`（或 `--force`）的情况下被接受，无论是将树对象换为 blob，还是将提交换为另一个没有前一个提交作为祖先的提交等。

  ​	与使用 [git-push[1]](../git-push) 进行推送不同，没有配置可以修改这些规则，也没有类似 `pre-receive` 钩子的 `pre-fetch` 钩子。

  ​	与使用 [git-push[1]](../git-push) 进行推送时一样，所有关于不允许的更新的规则都可以通过在 refspec 中添加可选的前导 `+`（或使用 `--force` 命令行选项）来覆盖。这唯一的例外是不可能使 `refs/heads/*` 命名空间接受非提交对象。

  > 注意
  >
  > ​	当要获取的远程分支已知会经常被回退和重新提交时，预期其新的 tip 不会是其先前 tip 的后代（在上次获取时存储在您的 remote-tracking 分支中）。在这种情况下，您会想要使用 `+` 符号来指示需要对此类分支执行非快进更新。没有办法确定或声明分支将在具有此行为的仓库中可用；拉取用户必须简单地知道这是分支的预期用法模式。

- `--stdin`

  从标准输入读取每行一个 refspec，除了作为参数提供的 refspec 之外。不支持 "tag <name>" 格式。

## GIT URLS

​	一般来说，URL 包含有关传输协议、远程服务器地址和仓库路径的信息。根据传输协议，其中的某些信息可能不存在。

​	Git 支持 ssh、git、http 和 https 协议（此外，ftp 和 ftps 可用于获取，但这是低效和不推荐的，请不要使用）。

​	原生传输（即 git:// URL）不进行身份验证，在不安全的网络上使用时需小心。

​	以下语法可以与它们一起使用：

- ssh://[user@]host.xz[:port]/path/to/repo.git/
- git://host.xz[:port]/path/to/repo.git/
- http[s]://host.xz[:port]/path/to/repo.git/
- ftp[s]://host.xz[:port]/path/to/repo.git/

​	ssh 协议还可以使用类似 scp 的语法：

- [user@]host.xz:path/to/repo.git/

​	只有在第一个冒号之前没有斜杠时，才会识别此语法。这有助于区分包含冒号的本地路径。例如，本地路径 `foo:bar` 可以被指定为绝对路径或 `./foo:bar`，以避免被误解为 ssh URL。

​	ssh 和 git 协议还支持 `~username` 扩展：

- ssh://[user@]host.xz[:port]/~[user]/path/to/repo.git/
- git://host.xz[:port]/~[user]/path/to/repo.git/
- [user@]host.xz:/~[user]/path/to/repo.git/

​	对于本地仓库，还支持由 Git 原生支持的以下语法： 

- /path/to/repo.git/
- file:///path/to/repo.git/

​	这两种语法大部分情况下是等效的，除非在克隆时，前一种意味着使用 --local 选项。有关详细信息，请参阅 [git-clone[1]](../git-clone)。

​	*git clone*、*git fetch* 和 *git pull*，但不包括 *git push*，还可以接受适当的捆绑文件。请参阅 [git-bundle[1]](../git-bundle)。

​	当 Git 不知道如何处理特定的传输协议时，它会尝试使用 *remote-<transport>* 远程助手（如果存在）。要显式请求远程助手，可以使用以下语法：

- <transport>::<address>

​	其中 <address> 可以是路径、服务器和路径，或特定远程助手识别的任意 URL 类似字符串。有关详细信息，请参阅 [gitremote-helpers[7]](../7/gitremote-helpers)。

​	如果存在许多类似命名的远程仓库，并且希望对它们使用不同的格式（使您使用的 URL 将被重写为有效的 URL），可以创建以下形式的配置部分：

```
	[url "<actual url base>"]
		insteadOf = <other url base>
```

​	例如，使用以下内容：

```
	[url "git://git.host.xz/"]
		insteadOf = host.xz:/path/to/
		insteadOf = work:
```

像 "work:repo.git" 或 "host.xz:/path/to/repo.git" 这样的 URL 将在任何接受 URL 的上下文中被重写为 "git://git.host.xz/repo.git"。

​	如果只想重写推送的 URL，可以创建以下形式的配置部分：

```
	[url "<actual url base>"]
		pushInsteadOf = <other url base>
```

​	例如，使用以下内容：

```
	[url "ssh://example.org/"]
		pushInsteadOf = git://example.org/
```

像 "git://example.org/path/to/repo.git" 这样的 URL 将在推送时被重写为 "ssh://example.org/path/to/repo.git"，但拉取仍将使用原始 URL。

## 远程

​	作为 `<repository>` 参数，可以使用以下内容之一的名称：

- Git 配置文件（`$GIT_DIR/config`）中的远程，
- `$GIT_DIR/remotes` 目录中的文件，或
- `$GIT_DIR/branches` 目录中的文件。

​	所有这些都允许您省略命令行中的 refspec，因为它们各自包含一个 refspec，Git 默认会使用它。

### 配置文件中的命名远程

​	您可以选择提供之前使用 [git-remote[1]](../git-remote)、[git-config[1]](../git-config) 配置的远程的名称，甚至可以通过手动编辑 `$GIT_DIR/config` 文件进行配置。该远程的 URL 将用于访问仓库。当您在命令行上不提供 refspec 时，该远程的 refspec 将作为默认值。配置文件中的条目如下所示：

```
	[remote "<name>"]
		url = <URL>
		pushurl = <pushurl>
		push = <refspec>
		fetch = <refspec>
```

​	`<pushurl>` 仅用于推送。它是可选的，默认为 `<URL>`。向远程推送会影响所有已定义的推送 URL，如果未定义推送 URL，则会影响所有已定义的 URL。然而，仅当定义了 fetch refspec 时，fetch 才会从第一个已定义的 URL 获取。

### `$GIT_DIR/remotes` 目录中的命名文件

​	您可以选择提供 `$GIT_DIR/remotes` 目录中文件的名称。该文件中的 URL 将用于访问仓库。当您在命令行上不提供 refspec 时，该文件中的 refspec 将作为默认值。该文件应具有以下格式：

```
	URL: one of the above URL format
	Push: <refspec>
	Pull: <refspec>
```

`Push:` 行用于 *git push*，`Pull:` 行用于 *git pull* 和 *git fetch*。可以为额外的分支映射指定多个 `Push:` 和 `Pull:` 行。

### `$GIT_DIR/branches` 目录中的命名文件

​	您可以选择提供 `$GIT_DIR/branches` 目录中文件的名称。该文件中的 URL 将用于访问仓库。该文件应具有以下格式：

```
	<URL>#<head>
```

`<URL>` 是必需的；`#<head>` 是可选的。

​	根据操作，如果您在命令行上不提供 refspec，则 Git 将使用以下 refspec 之一。`<branch>` 是 `$GIT_DIR/branches` 中该文件的名称，默认为 `master`。

​	git fetch 使用：

```
	refs/heads/<head>:refs/heads/<branch>
```

​	git push 使用：

```
	HEAD:refs/heads/<head>
```

## 配置远程跟踪分支

​	您经常通过定期和重复地从远程仓库获取内容来与其进行交互。为了跟踪这样的远程仓库的进展，`git fetch` 允许您配置 `remote.<repository>.fetch` 配置变量。

​	通常，这样的变量可能如下所示：

```
[remote "origin"]
	fetch = +refs/heads/*:refs/remotes/origin/*
```

​	该配置有两种用法：

- 当运行 `git fetch` 时，没有在命令行上指定要获取的分支和/或标签，例如 `git fetch origin` 或 `git fetch`，则将使用 `remote.<repository>.fetch` 值作为 refspec，即它们指定要获取的引用和要更新的本地引用。上述示例将复制存在于 `origin` 中的所有分支（即与值的左侧匹配的任何引用 `refs/heads/*`）并将其存储到 `refs/remotes/origin/*` 层次结构中的相应远程跟踪分支。
- 当使用显式的分支和/或标签运行 `git fetch`，例如 `git fetch origin master`，则命令行上给定的 `<refspec>` 决定要获取的内容（例如示例中的 `master`，它是 `master:` 的简写，这意味着“获取 *master* 分支，但不明确指定从命令行更新它的远程跟踪分支”），而配置文件中的 `remote.<repository>.fetch` 值确定要更新的远程跟踪分支（如果有）。当以这种方式使用时，`remote.<repository>.fetch` 值不会影响决定要获取的内容（即它们在命令行列出 refspec 时不会作为 refspec 使用），它们只用作映射，决定了将获取的引用存储在何处。

​	您可以通过在命令行上提供 `--refmap=<refspec>` 参数来覆盖 `remote.<repository>.fetch` 值的后一种用法。

## PRUNING（清理）

​	Git 的默认处理方式是除非明确丢弃，否则将保留数据；这也适用于保留对已删除分支的远程本地引用。

​	如果允许这些过时的引用不断积累，可能会导致在具有大量和繁忙的分支变更的大型仓库上性能变差，比如 `git branch -a --contains <commit>` 这样的命令输出会变得冗长，以及影响其他使用完整已知引用集的操作。

​	这些远程跟踪引用可以使用以下任意一种命令进行一次性删除：

```
# While fetching
$ git fetch --prune <name>

# Only prune, don't fetch
$ git remote prune <name>
```

​	要在正常工作流程中对引用进行清理，而无需记得运行这些命令，可以全局设置 `fetch.prune` 或在配置中为每个远程设置 `remote.<name>.prune`。有关详细信息，请参阅 [git-config[1]](../git-config)。

​	但是，这里有一点要注意，即清理功能实际上并不关心分支，而是根据远程的 refspec 进行本地 ←→ 远程引用的清理（请参阅上面的 `<refspec>` 和 PRUNING 部分）。

​	因此，如果远程的 refspec 包括 `refs/tags/*:refs/tags/*`，或者您手动运行了 `git fetch --prune <name> "refs/tags/*:refs/tags/*"` 这样的命令，将删除的不是过时的远程跟踪分支，而是本地不存在的任何标签。

​	这可能不是您所期望的行为，例如，您想要清理远程 `<name>`，但同时也明确地从它获取标签，这样当您从它获取时，将删除您的所有本地标签，而其中大部分标签可能根本不是来自 `<name>` 远程。

​	因此，在使用类似 `refs/tags/*:refs/tags/*` 或任何其他可能将多个远程的引用映射到相同本地命名空间的 refspec 时，要小心。

​	由于保持与远程的分支和标签同步是常见的用例，`--prune-tags` 选项可以与 `--prune` 一起使用，以清理本地不存在于远程的标签，并强制更新那些有差异的标签。可以通过 `fetch.pruneTags` 或 `remote.<name>.pruneTags` 在配置中启用标签清理。有关详细信息，请参阅 [git-config[1]](../git-config)。

​	`--prune-tags` 选项等效于在远程的 refspecs 中声明 `refs/tags/*:refs/tags/*`。这可能会导致一些看似奇怪的交互：

```
# These both fetch tags
$ git fetch --no-tags origin 'refs/tags/*:refs/tags/*'
$ git fetch --no-tags --prune-tags origin
```

​	之所以在没有提供 `--prune` 或其配置版本的情况下不出错，是为了保持配置版本的灵活性，并在命令行标志和配置版本之间保持 1=1 的映射关系。

​	例如，您可以在 `~/.gitconfig` 中配置 `fetch.pruneTags=true`，这样每次运行 `git fetch --prune` 时都会清理标签，而不会使每次在没有 `--prune` 的情况下运行 `git fetch` 出错。

​	使用 `--prune-tags` 清理标签也适用于从 URL 而不是命名远程获取的情况。这些命令都将清理未在 origin 上找到的标签：

``` bash
$ git fetch origin --prune --prune-tags
$ git fetch origin --prune 'refs/tags/*:refs/tags/*'
$ git fetch <url of origin> --prune --prune-tags
$ git fetch <url of origin> --prune 'refs/tags/*:refs/tags/*'
```

## 输出

​	"git fetch" 的输出取决于使用的传输方法；本节描述了通过 Git 协议（本地或通过 SSH）和 Smart HTTP 协议进行获取时的输出。

​	获取状态以表格形式输出，每行表示单个引用的状态。每行的格式如下：

```
 <flag> <summary> <from> -> <to> [<reason>]
```

​	只有在使用 --verbose 选项时，才显示已更新的引用的状态。

​	在紧凑输出模式下（通过配置变量 fetch.output 指定），如果在另一个字符串中找到整个 `<from>` 或 `<to>`，则将在另一个字符串中用 `*` 替换它。例如，`master -> origin/master` 变为 `master -> origin/*`。

- flag

  表示引用状态的单个字符：(空格) 表示成功获取的快进引用；`+` 表示成功强制更新的引用；`-` 表示成功清理的引用；`t` 表示成功更新的标签；`*` 表示成功获取的新引用；`!` 表示被拒绝或更新失败的引用；`=` 表示已是最新的引用，不需要获取。

- summary

  对于成功获取的引用，摘要显示了引用的旧值和新值，以便作为 `git log` 的参数使用（在大多数情况下是 `<old>..<new>`，对于强制非快进更新是 `<old>...<new>`）。

- from

  对于成功获取的引用，摘要显示了引用的旧值和新值，以便作为 `git log` 的参数使用（在大多数情况下是 `<old>..<new>`，对于强制非快进更新是 `<old>...<new>`）。

- to

  正在更新的本地引用的名称，去除了其 `refs/<type>/` 前缀。

- reason

  一个人类可读的解释。对于成功获取的引用，不需要解释。对于失败的引用，将描述失败原因。

## 示例

- 更新远程跟踪分支：

  ``` bash
  $ git fetch origin
  ```

  上述命令将复制所有分支从远程 `refs/heads/` 命名空间，并将它们存储到本地 `refs/remotes/origin/` 命名空间中，除非使用 `remote.<repository>.fetch` 选项来指定非默认的 refspec。

- 显式使用 refspecs：

  ``` bash
  $ git fetch origin +seen:seen maint:tmp
  ```

  这将通过从 `seen` 和 `maint` 分支（分别是 `seen` 和 `maint` 值）获取，更新（或创建，如果需要的话）本地仓库中的 `seen` 和 `tmp` 分支。

  `seen` 分支将被更新，即使它不是快进，因为它以加号开头；`tmp` 不会被更新。

- 查看远程分支，而无需在本地仓库中配置远程：

  ``` bash
  $ git fetch git://git.kernel.org/pub/scm/git/git.git maint
  $ git log FETCH_HEAD
  ```

  第一个命令从 `git://git.kernel.org/pub/scm/git/git.git` 仓库获取 `maint` 分支，第二个命令使用 `FETCH_HEAD` 查看分支的提交历史。获取的对象最终将被 Git 的内置管理删除（请参阅 [git-gc[1]](../git-gc)）。
  

## 安全性

​	获取（fetch）和推送（push）协议并不设计用于防止一方从另一个仓库窃取不打算共享的数据。如果您有需要保护免受恶意同级的私有数据，最好的选择是将其存储在另一个仓库中。这适用于客户端和服务器端。特别是，服务器上的命名空间对于读取访问控制并不有效；应该只向信任能够读取整个仓库的客户端授予对命名空间的读取访问权限。

​	已知的攻击向量如下：

1. 受害者发送 "have" 行，广告它拥有的对象的 ID，这些对象不是明确打算共享的，但如果同级也有这些对象，则可以用于优化传输。攻击者选择一个对象 ID X 进行窃取，并发送指向 X 的引用，但不必发送 X 的内容，因为受害者已经有了它。现在，受害者认为攻击者拥有 X，并稍后将 X 的内容发送回给攻击者。（对于客户端对服务器执行这种攻击是最直接的方式，方法是在客户端有访问权限的命名空间中创建一个指向 X 的引用，然后获取它。服务器对客户端执行这种攻击的最可能方式是将 X "合并" 到公共分支中，并希望用户在此分支上进行额外的工作，并将其推送回服务器而不注意到合并。）
2. 与 #1 类似，攻击者选择一个对象 ID X 进行窃取。受害者发送一个攻击者已经拥有的对象 Y，并且攻击者错误地声称拥有 X 而没有 Y，因此受害者将 Y 作为相对于 X 的增量发送。该增量向攻击者揭示了 X 的某些区域与 Y 相似的信息。

## 配置

​	以下是本节中包含的与配置有关的内容，内容与 [git-config[1]](../git-config) 中的内容相同：

- fetch.recurseSubmodules

  此选项控制 `git fetch`（以及 `git pull` 中的底层获取）是否会递归获取已有内容的子模块。此选项可以设置为布尔值或 *on-demand*。将其设置为布尔值会更改 fetch 和 pull 的行为，当设置为 true 时，会无条件地递归进入子模块；当设置为 false 时，则不递归。当设置为 *on-demand* 时，只有在超级仓库检索更新子模块引用的提交时，fetch 和 pull 才会递归进入已有内容的子模块。默认值是 *on-demand*，或者如果设置了 *submodule.recurse*，则使用 *submodule.recurse* 的值。

- fetch.fsckObjects

  如果设置为 true，则 git-fetch-pack 将检查所有获取的对象。有关检查的内容，请参阅 `transfer.fsckObjects`。默认值为 false。如果未设置，则使用 `transfer.fsckObjects` 的值。

- fetch.fsck.<msg-id>

  类似于 `fsck.<msg-id>`，但由 [git-fetch-pack[1]](../git-fetch-pack) 使用，而不是由 [git-fsck[1]](../git-fsck) 使用。有关详细信息，请参阅 `fsck.<msg-id>` 的文档。

- fetch.fsck.skipList

  类似于 `fsck.skipList`，但由 [git-fetch-pack[1]](../git-fetch-pack) 使用，而不是由 [git-fsck[1]](../git-fsck) 使用。有关详细信息，请参阅 `fsck.skipList` 的文档。

- fetch.unpackLimit

  如果通过 Git 原生传输获取的对象数量低于此限制，则对象将解包为松散的对象文件。但是，如果接收到的对象数量等于或超过此限制，则接收的 pack 将作为 pack 存储，同时添加任何丢失的增量基础。从推送中存储 pack 可以使推送操作更快地完成，尤其是在慢速文件系统上。如果未设置，则使用 `transfer.unpackLimit` 的值。

- fetch.prune

  如果为 true，则 fetch 将自动表现得好像在命令行上给出了 `--prune` 选项。另请参阅 `remote.<name>.prune` 和 [git-fetch[1]](../git-fetch) 的 PRUNING 部分。

- fetch.pruneTags

  如果为 true，则 fetch 将自动表现得好像在修剪时提供了 `refs/tags/*:refs/tags/*` refspec。这允许同时设置此选项和 `fetch.prune`，以保持与上游引用的 1=1 映射关系。另请参阅 `remote.<name>.pruneTags` 和 [git-fetch[1]](../git-fetch) 的 PRUNING 部分。

- fetch.output

  控制如何打印引用更新状态。有效值为 `full` 和 `compact`。默认值是 `full`。有关详细信息，请参阅 [git-fetch[1]](../git-fetch) 中的 OUTPUT 部分。

- fetch.negotiationAlgorithm

  在与服务器协商要发送的 packfile 内容时，控制发送本地仓库中提交信息的方式。设置为 "consecutive" 使用一种算法，遍历连续的提交并检查每个提交。设置为 "skipping" 使用一种跳过提交的算法，以便更快地收敛，但可能会导致比必要的 packfile 更大；或者设置为 "noop" 以根本不发送任何信息，这几乎肯定会导致比必要的 packfile 更大，但会跳过协商步骤。设置为 "default" 以覆盖先前的设置并使用默认行为。默认值通常为 "consecutive"，但如果 `feature.experimental` 为 true，则默认为 "skipping"。未知的值将导致 *git fetch* 报错。

  另请参阅 [git-fetch[1]](../git-fetch) 中的 `--negotiate-only` 和 `--negotiation-tip` 选项。

- fetch.showForcedUpdates

  设置为 false，以启用 [git-fetch[1]](../git-fetch) 和 [git-pull[1]](../git-pull) 命令中的 `--no-show-forced-updates`。默认值为 true。

- fetch.parallel

  指定并行运行的最大获取操作数量（子模块或在 [git-fetch[1]](../git-fetch) 中 `--multiple` 选项生效时的远程仓库）。

  值为 0 将给出一些合理的默认值。如果未设置，默认值为 1。

  对于子模块，可以使用 `submodule.fetchJobs` 配置设置来覆盖此设置。

- fetch.writeCommitGraph

  设置为 true，以便在每个从远程下载 pack 文件的 `git fetch` 命令后写入提交图。使用 `--split` 选项，大多数执行将在现有的提交图文件之上创建一个非常小的提交图文件。偶尔，这些文件将合并，写入可能需要更长时间。更新的提交图文件有助于许多 Git 命令的性能，包括 `git merge-base`、`git push -f` 和 `git log --graph`。默认值为 false。

- fetch.bundleURI

  此值存储用于从原始 Git 服务器进行增量获取之前，从 bundle URI 下载 Git 对象数据的 URI。这类似于 [git-clone[1]](../git-clone) 中的 `--bundle-uri` 选项的行为。如果 `git clone --bundle-uri` 使用了 `--bundle-uri` 选项，则会设置 `fetch.bundleURI` 值，如果提供的 bundle URI 包含用于增量获取组织的 bundle list。

  如果修改了此值，并且您的仓库具有 `fetch.bundleCreationToken` 值，则在从新的 bundle URI 获取之前删除该 `fetch.bundleCreationToken` 值。

- fetch.bundleCreationToken

  当使用 `fetch.bundleURI` 从使用 "creationToken" 启发式的 bundle list 进行增量获取时，此配置值存储下载的 bundle 的最大 `creationToken` 值。如果广告的 `creationToken` 不严格大于此值，则将使用此值阻止将来下载 bundle。
  
  创建 token 值由提供服务的特定 bundle URI 选择。如果修改了 `fetch.bundleURI` 中的 URI，则在获取之前确保删除 `fetch.bundleCreationToken` 的值。

## BUGS

使用 --recurse-submodules 仅能获取在本地存在的子模块中的新提交，例如在 `$GIT_DIR/modules/` 中存在的子模块。如果上游添加了一个新的子模块，则在对其进行克隆（例如通过 `git submodule update`）之前，无法获取该子模块。这预计将在将来的 Git 版本中修复。

## 另请参阅

[git-pull[1]](../git-pull)

## GIT

  这是[git[1]](../../Git)工具集中的一部分。