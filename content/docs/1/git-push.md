+++
title = "git-push"
weight = 30
type = "docs"
date = 2023-05-08T15:39:23+08:00
description = ""
isCJKLanguage = true
draft = false

+++

# git-push

https://git-scm.com/docs/git-push

version 2.41.0

## 名称

​	git-push - 更新远程引用以及相关对象

## 概述

```
git push [--all | --mirror | --tags] [--follow-tags] [--atomic] [-n | --dry-run] [--receive-pack=<git-receive-pack>]
	   [--repo=<repository>] [-f | --force] [-d | --delete] [--prune] [-v | --verbose]
	   [-u | --set-upstream] [-o <string> | --push-option=<string>]
	   [--[no-]signed|--signed=(true|false|if-asked)]
	   [--force-with-lease[=<refname>[:<expect>]] [--force-if-includes]]
	   [--no-verify] [<repository> [<refspec>…]]
```

## 描述

​	使用本地引用更新远程引用，同时发送必要的对象以完成给定的引用。

​	每次将内容推送到存储库时，您都可以通过在存储库中设置*钩子*来执行有趣的操作。有关详情，请参阅[git-receive-pack[1]](../git-receive-pack)的文档。

​	当命令行未使用`<repository>`参数指定推送目标时，将查询当前分支的`branch.*.remote`配置以确定推送位置。如果找不到配置，则默认为*origin*。

​	当命令行未使用`<refspec>...`参数或`--all`、`--mirror`、`--tags`选项指定要推送的内容时，命令会通过查询`remote.*.push`配置找到默认的`<refspec>`，如果找不到，则会根据`push.default`配置决定要推送的内容（有关`push.default`的含义，请参见[git-config[1]](../git-config)）。

​	当命令行和配置都未指定要推送的内容时，将使用默认行为，即对于`push.default`的`simple`值：将当前分支推送到相应的上游分支，但为了安全起见，如果上游分支的名称与本地分支不同，则会中止推送。

## 选项

- <repository>

  执行推送操作的“远程”存储库。此参数可以是 URL（请参阅下面的[GIT URLS](https://git-scm.com/docs/git-push#URLS)部分）或远程名称（请参阅下面的[REMOTES](https://git-scm.com/docs/git-push#REMOTES)部分）。

- <refspec>…

  指定要使用源对象更新的目标引用。`<refspec>`参数的格式是可选的加号 `+`，后跟源对象 `<src>`，再后跟冒号 `:`，最后是目标引用 `<dst>`。

  `<src>`通常是要推送的分支的名称，但它可以是任意的"SHA-1 表达式"，例如 `master~4` 或 `HEAD`（请参阅[gitrevisions[7]](../7/gitrevisions)）。

  `<dst>`表示要使用此推送更新远程端的哪个引用。此处不能使用任意的表达式，必须使用实际的引用名称。如果`git push [<repository>]`没有任何`<refspec>`参数，并且使用`remote.<repository>.push`配置变量设置为使用`<src>`更新目标的某个引用，则可以省略 `:<dst>` 部分——这样的推送将会更新一个在命令行上没有指定`<refspec>`的引用。否则，如果缺少 `:<dst>`，将更新与 `<src>` 相同的引用。

  如果 `<dst>` 不以 `refs/` 开头（例如 `refs/heads/master`），我们将根据被推送的 `<src>` 的类型以及 `<dst>` 是否模糊不清来推断它属于目标 `<repository>` 的 `refs/*` 中的何处。

  - 如果 `<dst>` 明确地引用了远程端的一个引用，则推送到该引用。

  - 如果 `<src>` 解析为以 `refs/heads/` 或 `refs/tags/` 开头的引用，则将其前缀加到 `<dst>`。

  - 将来可能会添加其他的模糊解决方法，但目前其他任何情况都将因为错误而退出，并根据`advice.pushUnqualifiedRefname`配置（请参阅[git-config[1]](../git-config)）建议您可能想要推送到的refs/命名空间。

  `<src>` 引用的对象用于更新远程端的 `<dst>` 引用。是否允许此操作取决于`refs/*`中的 `<dst>` 引用的位置，详细说明请参见下面的各节，这些“更新”指的是除删除以外的任何修改，因为后面的几节中提到删除是有所不同的。

  `refs/heads/*` 命名空间仅接受提交对象，并且仅在可以快进时更新。

  The `refs/tags/*` namespace will accept any kind of object (as commits, trees and blobs can be tagged), and any updates to them will be rejected.

  `refs/tags/*` 命名空间接受任何类型的对象（因为提交、树和blob可以被标记），并且对它们的任何更新都将被拒绝。

  可以将任何类型的对象推送到 `refs/{tags,heads}/*` 以外的任何命名空间。对于标签和提交，这些将被视为与 `refs/heads/*` 内的提交相同，以确定是否允许更新。

  也就是说，`refs/{tags,heads}/*` 以外的提交和标签的快进是允许的，即使在快进的内容不是提交，而是一个指向新提交的标签对象，而该新提交是原来标签（或提交）指向的提交的快进。也允许使用完全不同的标签替换标签，如果它指向相同的提交，以及推送已剥离的标签，即推送现有标签对象指向的提交，或者推送现有提交指向的新标签对象。

  也就是说，`refs/{tags,heads}/*` 以外的提交和标签的快进是允许的，即使在快进的内容不是提交，而是一个指向新提交的标签对象，而该新提交是原来标签（或提交）指向的提交的快进。也允许使用完全不同的标签替换标签，如果它指向相同的提交，以及推送已剥离的标签，即推送现有标签对象指向的提交，或者推送现有提交指向的新标签对象。

  上述关于不允许更新的规则可以通过在 refspec 前面添加可选的前导`+`（或使用`--force`命令行选项）来覆盖。唯一的例外是无论如何强制，`refs/heads/*` 命名空间都不会接受非提交对象。钩子和配置也可以覆盖或修改这些规则，例如请参阅[git-config[1]](../git-config)中的`receive.denyNonFastForwards`和[githooks[5]](../5/githooks)中的`pre-receive`和`update`。

  推送空的 `<src>` 允许您从远程存储库中删除 `<dst>` 引用。删除总是可以接受的，不需要在 refspec（或 `--force`）中添加前导`+`，除非被配置或钩子禁止。请参阅[git-config[1]](../git-config)中的`receive.denyDeletes`和[githooks[5]](../5/githooks)中的`pre-receive`和`update`。

  特殊的 refspec `:`（或 `+:` 以允许非快进式更新）指示Git推送"匹配"分支：对于本地存在的每个分支，如果远程端已经存在同名分支，则更新远程端。

  `tag <tag>` 的意思与 `refs/tags/<tag>:refs/tags/<tag>` 相同。

- `--all`

  推送所有分支（即 `refs/heads/` 下的引用）；不能与其他 `<refspec>` 一起使用。

- `--prune`

  删除没有本地对应项的远程分支。例如，如果没有同名的本地分支，则将删除远程分支 `tmp`。这也适用于 refspec，例如 `git push --prune remote refs/heads/*:refs/tmp/*` 将确保如果 `refs/heads/foo` 不存在，则会删除远程的 `refs/tmp/foo`。

- `--mirror`

  不指定每个要推送的引用，而是指定将所有 `refs/` 下的引用（包括但不限于 `refs/heads/`、`refs/remotes/` 和 `refs/tags/`）镜像到远程存储库。新创建的本地引用将被推送到远程端，本地更新的引用将被强制更新到远程端，删除的引用将从远程端删除。如果设置了配置选项 `remote.<remote>.mirror`，则默认使用此选项。

- -n

- `--dry-run`

  除了实际发送更新之外，执行所有操作。

- `--porcelain`

  生成机器可读的输出。每个引用的输出状态行将用制表符分隔，并发送到标准输出而不是标准错误输出。将提供引用的完整符号名称。

- -d

- `--delete`

  从远程存储库中删除所有列出的引用。这与在所有引用前加冒号相同。

- `--tags`

  推送所有 `refs/tags` 下的引用，除了在命令行中明确列出的 refspecs。

- `--follow-tags`

  推送所有将在没有此选项的情况下被推送的引用，以及指向可从被推送引用所指向的提交的 `refs/tags` 中的带注释标签。可以使用配置变量 `push.followTags` 指定此选项。更多信息，请参见[git-config[1]](../git-config)中的 `push.followTags`。

- `--[no-]signed`

- `--signed=(true|false|if-asked)`

  对推送请求进行 GPG 签名，以便允许钩子检查或记录它。如果为`false`或`--no-signed`，则不会尝试签名。如果为`true`或`--signed`，如果服务器不支持签名推送，则推送将失败。如果设置为`if-asked`，则仅在服务器支持签名推送时才签名。如果实际调用 `gpg --sign` 失败，则推送也会失败。有关接收端的详细信息，请参阅[git-receive-pack[1]](../git-receive-pack)。

- `--[no-]atomic`

  如果远程端支持，使用原子事务。要么所有引用都会被更新，要么出现错误时不会更新任何引用。如果服务器不支持原子推送，则推送将失败。

- -o <option>

- `--push-option=<option>`

  将给定的字符串传递给服务器，服务器将它们传递给 pre-receive 和 post-receive 钩子。给定的字符串不能包含 NUL 或 LF 字符。当给定多个 `--push-option=<option>` 时，它们按照命令行上列出的顺序都发送到另一端。当从命令行中没有给定 `--push-option=<option>` 时，将使用配置变量 `push.pushOption` 的值。

- `--receive-pack=<git-receive-pack>`

- `--exec=<git-receive-pack>`

  远程端 *git-receive-pack* 程序的路径。在通过 ssh 推送到远程存储库时，如果您在默认的 $PATH 上没有该程序的目录，则有时可能会很有用。

- `--[no-]force-with-lease`

- `--force-with-lease=<refname>`

- `--force-with-lease=<refname>:<expect>`

  通常，"git push" 拒绝更新不是用来覆盖它的本地引用的远程引用。

  如果当前的远程引用值是预期值，则此选项会覆盖此限制。否则，“git push”会失败。

  想象一下，您必须 rebase 已经发布的内容。为了替换您最初发布的历史记录，您将必须绕过"必须快进"规则。如果在您进行 rebase 时，其他人在原始历史记录之上构建了内容，远程端的分支尖端可能会随着他们的提交而前进，并且盲目使用 `--force` 进行推送会导致丢失他们的工作。

  此选项允许您说出您期望要更新的历史记录是您重新基础并希望替换的历史记录。如果远程引用仍然指向您指定的提交，则可以确保没有其他人对引用进行了任何更改。这类似于在没有明确锁定的情况下为引用签订“租约”，只有在"租约"仍然有效时，远程引用才会更新。

  单独使用 `--force-with-lease` 而不指定细节，将保护将要更新的所有远程引用，要求它们的当前值与我们为其创建的远程跟踪分支的当前值相同。

  `--force-with-lease=<refname>`，在不指定期望值的情况下，将保护指定的引用（独立），如果将要更新，则要求其当前值与我们为其创建的远程跟踪分支的当前值相同。

  `--force-with-lease=<refname>:<expect>` 将保护指定的引用（独立），如果将要更新，则要求其当前值与指定的值 `<expect>` 相同（此值允许与我们为该 refname 创建的远程跟踪分支的值不同，或者在使用此形式时，我们甚至不必拥有该远程跟踪分支）。如果 `<expect>` 是空字符串，则指定的引用不得已存在。

  注意，除了`--force-with-lease=<refname>:<expect>`明确指定引用的预期当前值的所有其他形式仍然是实验性的，并且随着对此功能的实践经验的积累，它们的语义可能会更改。

  "--no-force-with-lease" 将取消命令行上先前的所有 --force-with-lease。

  安全性的一般注意事项：在不指定预期值的情况下使用此选项，即作为 `--force-with-lease` 或 `--force-with-lease=<refname>` 提供，与任何隐式在后台运行 `git fetch` 的东西非常不兼容，例如在cron作业中在您的存储库上运行`git fetch origin`。

  它所提供的相对于 `--force` 的保护措施是确保未基于您的工作的后续更改被覆盖，但是如果某些后台进程正在后台更新引用，则这很容易被击败。我们除了远程跟踪信息之外，没有其他任何东西可供引用使用，这是用于引用的启发式算法，用于确定您可能已经看到并且愿意覆盖的引用。

  如果您的编辑器或其他系统正在为您在后台运行`git fetch`，则可以通过简单地设置另一个远程端来缓解这个问题：

  ```
  git remote add origin-push $(git config remote.origin.url)
  git fetch origin-push
  ```

  现在，当后台进程运行`git fetch origin`时，`origin-push`上的引用将不会更新，因此像这样的命令：

  ```
  git push --force-with-lease origin-push
  ```

  除非您手动运行 `git fetch origin-push`，否则将失败。当然，如果运行 `git fetch --all` 这样的命令，这种方法完全会被击败，此时您需要禁用它或者进行更繁琐的操作，比如：

  ```
  git fetch              # update 'master' from remote
  git tag base master    # mark our base point
  git rebase -i master   # rewrite some commits
  git push --force-with-lease=master:base master:master
  ```

  换句话说，为您已经看到并且愿意覆盖的上游代码版本创建一个名为 `base` 的标签，然后重写历史记录，并且最后如果远程版本仍然在 `base` 处，则强制推送更改到 `master`，而不管后台中的本地 `remotes/origin/master` 已更新到什么位置。

  或者，同时将 `--force-if-includes` 作为辅助选项与 `--force-with-lease[=<refname>]` 一起指定（即不指定远程引用必须指向的确切提交，或者保护哪些远程引用），在“push”时会验证远程跟踪引用的顶部是否在本地已经集成，然后再允许进行强制更新。

- -f

- `--force`

  通常，此命令将拒绝更新不是本地引用的祖先的远程引用。另外，当使用 `--force-with-lease` 选项时，此命令将拒绝更新当前值与期望值不匹配的远程引用。此标志禁用这些检查，并可能导致远程存储库丢失提交。请谨慎使用。请注意，`--force` 适用于所有要推送的引用，因此在 `push.default` 设置为 `matching` 或使用 `remote.*.push` 配置了多个推送目标的情况下使用它可能会覆盖除当前分支之外的引用（包括严格落后于其远程对应分支的本地引用）。要仅对一个分支进行强制推送，可以在要推送的 refspec 前面加上 `+`（例如 `git push origin +master` 强制推送到 `master` 分支）。有关详细信息，请参阅上面的 `<refspec>...` 部分。

- `--[no-]force-if-includes`

  仅当本地已集成远程跟踪引用的顶部时才强制更新。此选项启用检查，以验证远程跟踪引用的顶部是否可以从依赖于它的本地分支的 "reflog" 条目访问，用于重写。通过拒绝未进行此类检查的强制更新，此检查确保了远程的任何更新已在本地合并。如果在不指定 `--force-with-lease` 的情况下传递该选项，或者与 `--force-with-lease=<refname>:<expect>` 一起指定该选项，它将是一个“无操作”。使用 `--no-force-if-includes` 可以禁用此行为。

- `--repo=<repository>`

  此选项等同于 `<repository>` 参数。如果两者都指定，则命令行参数优先。

- -u

- `--set-upstream`

  对于每个已经更新或成功推送的分支，添加上游（跟踪）引用，用于无参数 [git-pull[1]](../git-pull) 和其他命令。有关更多信息，请参阅 [git-config[1]](../git-config) 中的 `branch.<name>.merge` 配置选项。

- `--[no-]thin`

  这些选项会传递给 [git-send-pack[1]](../git-send-pack)。当发送方和接收方共享许多相同对象时，薄传输会显著减少发送的数据量。默认为 `--thin`。

- -q

- `--quiet`

  抑制所有输出，包括更新引用的列表，除非发生错误。进度不会报告到标准错误流。

- -v

- `--verbose`

  以详细模式运行。

- `--progress`

  默认情况下，如果将进度状态附加到终端，进度状态会通过标准错误流报告，除非指定了 -q。此标志强制显示进度状态，即使标准错误流未定向到终端。

- `--no-recurse-submodules`

- `--recurse-submodules=check|on-demand|only|no`

  用于确保在要推送的修订版本中使用的所有子模块提交在远程跟踪分支上都可用。如果使用 `check`，Git 将验证所有在要推送的修订版本中更改的子模块提交是否在子模块的至少一个远程端可用。如果有任何提交丢失，则推送将中止并退出非零状态。如果使用 `on-demand`，所有在要推送的修订版本中更改的子模块都将被推送。如果 `on-demand` 无法推送所有必要的修订版本，则也会中止推送并退出非零状态。如果使用 `only`，将推送所有子模块，同时超级项目保持未推送状态。可以使用值 `no` 或使用 `--no-recurse-submodules` 来覆盖 `push.recurseSubmodules` 配置变量，当不需要子模块递归时使用。

  使用 `on-demand` 或 `only` 时，如果子模块具有 "push.recurseSubmodules={on-demand,only}" 或 "submodule.recurse" 配置，则会进一步递归。在这种情况下，"only" 被视为 "on-demand"。

- `--[no-]verify`

  切换 pre-push hook（参见 [githooks[5]](../5/githooks)）。默认为 --verify，给钩子一个阻止推送的机会。使用 --no-verify 将完全绕过钩子。

- -4

- `--ipv4`

  仅使用 IPv4 地址，忽略 IPv6 地址。

- -6

- `--ipv6`

  仅使用 IPv6 地址，忽略 IPv4 地址。

## GIT URLS

​	一般来说，URL 包含有关传输协议、远程服务器地址和存储库路径的信息。根据传输协议的不同，其中的一些信息可能缺失。

​	Git 支持 ssh、git、http 和 https 协议（此外，ftp 和 ftps 可用于获取，但效率较低且已弃用，请勿使用）。

​	原生传输（即 git:// URL）不进行身份验证，应谨慎在不安全的网络上使用。

​	以下是可用于它们的语法：

- ssh://[user@]host.xz[:port]/path/to/repo.git/
- git://host.xz[:port]/path/to/repo.git/
- http[s]://host.xz[:port]/path/to/repo.git/
- ftp[s]://host.xz[:port]/path/to/repo.git/

还可以使用类似 scp 的语法与 ssh 协议一起使用：

- [user@]host.xz:path/to/repo.git/

仅当第一个冒号前没有斜杠时，才会识别此语法。这有助于区分包含冒号的本地路径。例如，本地路径 `foo:bar` 可以被指定为绝对路径或 `./foo:bar` 以避免被误解为 ssh URL。

ssh 和 git 协议还支持 ~username 展开：

- ssh://[user@]host.xz[:port]/~[user]/path/to/repo.git/
- git://host.xz[:port]/~[user]/path/to/repo.git/
- [user@]host.xz:/~[user]/path/to/repo.git/

对于本地存储库，Git 本身也支持以下语法：

- /path/to/repo.git/
- file:///path/to/repo.git/

这两种语法大体上是等效的，但在克隆时，前者会暗示 --local 选项。有关详情，请参阅 [git-clone[1]](../git-clone)。

*git clone*、*git fetch* 和 *git pull*，但不包括 *git push*，还可以接受合适的捆绑文件。有关详情，请参阅 [git-bundle[1]](../git-bundle)。

当 Git 不知道如何处理某种传输协议时，它会尝试使用相应的 *remote-<transport>* 远程助手（如果存在）。要明确请求一个远程助手，可以使用以下语法：

- <transport>::<address>

其中 <address> 可以是路径、服务器和路径，或者是特定远程助手识别的任意类似 URL 的字符串。有关详情，请参阅 [gitremote-helpers[7]](../7/gitremote-helpers)。

如果有大量名称类似的远程存储库，并且您希望使用不同的格式（使得您使用的 URL 将被重写为有效的 URL），可以创建以下格式的配置部分：

```
	[url "<actual url base>"]
		insteadOf = <other url base>
```

例如，使用以下配置：

```
	[url "git://git.host.xz/"]
		insteadOf = host.xz:/path/to/
		insteadOf = work:
```

URL 如 "work:repo.git" 或 "host.xz:/path/to/repo.git" 将在接受 URL 的任何上下文中被重写为 "git://git.host.xz/repo.git"。

如果要仅重写推送的 URL，请创建以下格式的配置部分：

```
	[url "<actual url base>"]
		pushInsteadOf = <other url base>
```

例如，使用以下配置：

```
	[url "ssh://example.org/"]
		pushInsteadOf = git://example.org/
```

URL 如 "git://example.org/path/to/repo.git" 将在推送时被重写为 "ssh://example.org/path/to/repo.git"，但拉取仍将使用原始 URL。

## REMOTES

​	可以使用以下之一的名称作为 `<repository>` 参数，而不是使用 URL：

- Git 配置文件 `$GIT_DIR/config` 中的远程；
- `$GIT_DIR/remotes` 目录中的文件；或
- `$GIT_DIR/branches` 目录中的文件。

​	所有这些方法还允许您省略命令行上的 refspec，因为它们每个都包含 git 默认会使用的 refspec。

### 在配置文件中命名的远程

​	您可以选择提供之前使用 [git-remote[1]](../git-remote)、[git-config[1]](../git-config) 配置的远程的名称。该远程的 URL 将用于访问存储库。该远程的 refspec 将在您未在命令行上提供 refspec 时被默认使用。配置文件中的条目如下：

```
	[remote "<name>"]
		url = <URL>
		pushurl = <pushurl>
		push = <refspec>
		fetch = <refspec>
```

​	`<pushurl>` 仅用于推送。它是可选的，默认为 `<URL>`。推送到一个远程会影响所有定义的 pushurl，或者在未定义 pushurl 的情况下，影响所有定义的 url。然而，仅首次定义的 url 会被用于 fetch。

### 在 `$GIT_DIR/remotes` 目录中命名的文件



​	您可以选择提供 `$GIT_DIR/remotes` 目录中的文件的名称。此文件中的 URL 将用于访问存储库。该文件中的 refspec 将在您未在命令行上提供 refspec 时作为默认值使用。此文件应具有以下格式：

```
	URL: one of the above URL format
	Push: <refspec>
	Pull: <refspec>
```

`Push:` 行用于 *git push*，`Pull:` 行用于 *git pull* 和 *git fetch*。可以为其他分支映射指定多个 `Push:` 和 `Pull:` 行。

### 在 `$GIT_DIR/branches` 目录中命名的文件

​	您可以选择提供 `$GIT_DIR/branches` 目录中的文件的名称。此文件中的 URL 将用于访问存储库。此文件应具有以下格式：

```
	<URL>#<head>
```

`<URL>` is required; `#<head>` is optional.

`<URL>` 是必需的，`#<head>` 是可选的。

​	根据操作，如果您在命令行上未提供 refspec，则 Git 将使用以下 refspec 之一。`<branch>` 是 `$GIT_DIR/branches` 中的文件名，`<head>` 默认为 `master`。

git fetch 使用：

```
	refs/heads/<head>:refs/heads/<branch>
```

git push 使用：

```
	HEAD:refs/heads/<head>
```

## 输出

​	"git push" 的输出取决于使用的传输方法；本节描述了在通过 Git 协议（无论是本地还是通过 ssh）进行推送时的输出。

​	推送的状态以表格形式输出，每行代表一个引用的状态。每行的格式如下：

```
 <flag> <summary> <from> -> <to> (<reason>)
```

​	如果使用了 --porcelain 选项，则输出的每一行格式为：

```
 <flag> \t <from>:<to> \t <summary> (<reason>)
```

​	仅在使用 --porcelain 或 --verbose 选项时，才显示已更新的引用的状态。

- flag

  一个单个字符，指示引用的状态：（空格）表示成功推送快进；`+` 表示成功强制更新；`-` 表示成功删除引用；`*` 表示成功推送新的引用；`!` 表示引用被拒绝或推送失败；`=` 表示引用是最新的，无需推送。

- summary

  对于成功推送的引用，摘要显示引用的旧值和新值，以适合作为 `git log` 参数使用（在大多数情况下，这是 `<old>..<new>`，对于强制非快进更新，是 `<old>...<new>`）。

  对于失败的更新，将给出更多详细信息：

  - rejected

    Git 根本没有尝试发送引用，通常是因为它不是快进，并且您没有强制更新。

  - remote rejected

    远程端拒绝了更新。通常是由于远程端上的钩子，或者因为远程存储库设置了以下安全选项之一：`receive.denyCurrentBranch`（用于推送到已检出分支）、`receive.denyNonFastForwards`（用于强制非快进更新）、`receive.denyDeletes` 或 `receive.denyDeleteCurrent`。请参阅 [git-config[1]](../git-config)。

  - remote failure

    远程端没有报告引用的成功更新，可能是因为远程端出现了临时错误、网络连接中断或其他临时错误。

- from

  被推送的本地引用的名称，去除其 `refs/<type>/` 前缀。对于删除操作，省略本地引用的名称。

- to

  被更新的远程引用的名称，去除其 `refs/<type>/` 前缀。

- reason

  人类可读的解释。对于成功推送的引用，不需要解释。对于失败的引用，会描述失败的原因。

## 关于FAST-FORWARDS（快进更新）的注意事项

​	当更新将一个分支（或更一般地说，一个引用）从指向提交 A 改为指向另一个提交 B 时，只有当 B 是 A 的后代时，它被称为快进更新。

​	在从 A 快进更新到 B 时，原始提交 A 构建在其之上的提交集是新提交 B 构建在其之上的提交集的子集。因此，它不会丢失任何历史记录。

​	相比之下，非快进更新会丢失历史记录。例如，假设您和其他人从相同的提交 X 开始，您构建了导致提交 B 的历史记录，而另一个人构建了导致提交 A 的历史记录。历史记录如下所示：

```
      B
     /
 ---X---A
```

​	进一步假设其他人已经将导致 A 的更改推送回您们两个从中获取原始提交 X 的原始存储库。

​	其他人执行的推送将更新指向提交 X 的分支，使其指向提交 A。这是一个快进更新。

​	但是，如果您尝试推送，您将尝试将分支（现在指向 A）更新为提交 B。这不是快进更新。如果您这样做，提交 A 引入的更改将丢失，因为现在每个人都将从 B 开始构建。

​	默认情况下，该命令不允许非快进更新，以防止这种历史丢失。

​	如果您不想丢失您的工作（从 X 到 B 的历史记录）或其他人的工作（从 X 到 A 的历史记录），您需要首先从存储库获取历史记录，创建包含双方更改的历史记录，并将结果推送回去。

​	您可以执行 "git pull"，解决潜在的冲突，然后执行 "git push"。"git pull" 将在提交 A 和 B 之间创建合并提交 C。

```
      B---C
     /   /
 ---X---A
```

​	将 A 更新为这个合并提交将是快进的，您的推送将被接受。

​	或者，您可以将位于 X 和 B 之间的更改在 A 的基础上重新应用，使用 "git pull --rebase"，然后将结果推送回去。这个变基将在 A 的基础上创建一个新的提交 D，其中包含 X 到 B 之间的更改。

```
      B   D
     /   /
 ---X---A
```

​	同样，在 A 更新为此提交时，将是快进的，您的推送将被接受。

​	还有另一种常见情况，当您尝试推送时可能遇到非快进拒绝，即使您正在推送到没有其他人推送的存储库。在您自己推送了提交 A 后（在本节中的第一张图片中），用 "git commit --amend" 将其替换为提交 B，并尝试将其推送出去，因为忘记了您已经推送了 A。在这种情况下，只有当您确定在此期间没有其他人获取过您之前的提交 A（并且开始在其之上构建）时，才可以运行 "git push --force" 来覆盖它。换句话说，"git push --force" 是一种方法，用于意图丢弃历史记录的情况。

## 示例

- `git push`

  相当于 `git push <remote>`，其中 <remote> 是当前分支的远程（如果当前分支没有配置远程，则为 `origin`）。

- `git push origin`

  在没有其他配置的情况下，将当前分支推送到配置的上游（`branch.<name>.merge` 配置变量）如果它与当前分支同名，则更新，否则出错。如果没有提供 <refspec>，则此命令的默认行为可以通过设置远程的 `push` 选项或 `push.default` 配置变量来配置。例如，要将默认配置为只推送当前分支到 `origin`，可以使用 `git config remote.origin.push HEAD`。任何有效的 <refspec>（例如下面示例中的那些）都可以配置为 `git push origin` 的默认值。

- `git push origin :`

  推送“匹配”的分支到 `origin`。有关“匹配”分支的描述，请参见上面的[选项](https://git-scm.com/docs/git-push#OPTIONS)部分中的<refspec>。

- `git push origin master`

  在源存储库中查找与 `master` 匹配的引用（很可能找到 `refs/heads/master`），并将其与 `origin` 存储库中的相同引用（例如 `refs/heads/master`）进行更新。如果 `master` 在远程不存在，则将创建它。

- `git push origin HEAD`

  一种方便的方式将当前分支推送到远程的相同名称上。

- `git push mothership master:satellite/master dev:satellite/dev`

  使用与 `master` 匹配的源引用（例如 `refs/heads/master`）来更新 `mothership` 存储库中与 `satellite/master` 匹配的引用（最可能是 `refs/remotes/satellite/master`）；对于 `dev` 和 `satellite/dev` 也是一样。

  有关匹配语义的讨论，请参阅上面描述的 `<refspec>...` 部分。

  这是模拟在 `mothership` 上运行 `git fetch`，使用相反方向运行的 `git push` 来整合在 `satellite` 上完成的工作的方法，通常在您只能单向建立连接的情况下需要这样做（即 satellite 可以 ssh 连接到 mothership，但 mothership 不能启动与 satellite 的连接，因为后者位于防火墙后或者没有运行 sshd）。

  在 `satellite` 机器上运行此 `git push` 后，您将 ssh 连接到 `mothership` 并在那里运行 `git merge`，以完成在 `mothership` 上运行的 `git pull` 的模拟，以拉取在 `satellite` 上进行的更改。

- `git push origin HEAD:master`

  将当前分支推送到 `origin` 存储库中与 `master` 匹配的远程引用。这种形式方便地将当前分支推送，无需考虑其本地名称。

- `git push origin master:refs/heads/experimental`

  通过复制当前 `master` 分支在 `origin` 存储库中创建 `experimental` 分支。只有在本地名称与远程名称不同的情况下才需要这种形式，否则仅使用引用名称本身即可。

- `git push origin :experimental`

  查找 `origin` 存储库中与 `experimental` 匹配的引用（例如 `refs/heads/experimental`），然后删除它。

- `git push origin +dev:master`

  使用 `dev` 分支更新 `origin` 存储库的 `master` 分支，允许非快进更新。**这可能导致 origin 存储库中的未引用提交。**考虑以下情况，其中无法进行快进：
  
  ```
  	    o---o---o---A---B  origin/master
  		     \
  		      X---Y---Z  dev
  ```
  
  上述命令将更改 origin 存储库为
  
  ```
  		      A---B  (unnamed branch)
  		     /
  	    o---o---o---X---Y---Z  master
  ```
  
  提交 A 和 B 将不再属于带有符号名称的分支，因此将被 `git gc` 命令从 origin 存储库中删除。

## 安全性

​	fetch 和 push 协议没有设计用于防止一方从另一个存储库窃取未经意图共享的数据。如果您有需要保护免受恶意同行窃取的私人数据，则最好的选择是将其存储在另一个存储库中。这适用于客户端和服务器。特别是，服务器上的命名空间对于读访问控制是无效的；您应该只将对整个存储库具有读访问权限的客户端授予对命名空间的读访问权限。

​	已知的攻击向量如下：

1. 受害者发送“have”行，广告它拥有的对象 ID，这些对象 ID 不是明确意图共享的，但可以在同行也拥有它们的情况下用于优化传输。攻击者选择一个要窃取的对象 ID X，并发送一个指向 X 的引用，但不需要发送 X 的内容，因为受害者已经拥有它。现在受害者相信攻击者拥有 X，并且稍后向攻击者发送 X 的内容。 （这种攻击对于客户端在服务器上执行最直接，通过在客户端具有访问权限的命名空间中创建指向 X 的引用，然后获取它。服务器对客户端执行此操作最可能的方法是将 X “合并” 到公共分支中，并希望用户在此分支上进行额外的工作并将其推回服务器，而不会注意到合并。）
2. 与 #1 中相同，攻击者选择要窃取的对象 ID X。受害者发送攻击者已经拥有的对象 Y，并且攻击者错误地声称拥有 X 而不拥有 Y，因此受害者将 Y 作为相对于 X 的增量发送。增量泄露了 X 中与 Y 相似的区域给攻击者。

## 配置

​	本节以下的所有内容都是从 [git-config[1]](../git-config) 文档中选择性地包含的。其中的内容与文档中找到的内容相同：

- push.autoSetupRemote

  如果设置为 "true"，则在默认推送时假设 `--set-upstream`，当当前分支没有上游跟踪时生效；该选项对于 push.default 选项 *simple*、*upstream* 和 *current* 生效。如果默认情况下您希望将新分支推送到默认的远程（就像 *push.default=current* 的行为）并且还希望设置上游跟踪，则此选项非常有用。最适合使用此选项的工作流程是 *simple* 中央工作流程，其中所有分支都预期在远程上具有相同的名称。

- push.default

  定义在没有提供 refspec 的情况下 `git push` 应该执行的操作（无论是来自命令行、配置还是其他地方）。不同的值非常适合特定的工作流程；例如，在纯中央工作流程中（即获取源等于推送目标），`upstream` 可能是您想要的。可能的值包括：

  - `nothing` - 除非给出 refspec，否则不推送任何内容（出错）。这主要适用于希望始终明确的人，以避免错误。

  - `current` - 将当前分支推送以更新接收端上同名的分支。适用于中央和非中央工作流程。

  - `upstream` - 将当前分支推送回通常集成到当前分支中的分支（称为 `@{upstream}`）。只有在将推送到通常从中拉取的同一存储库时才有意义（即中央工作流程）。

  - `tracking` - This is a deprecated synonym for `upstream`.
  - `tracking` - 这是 `upstream` 的一个已弃用的同义词。

  - `simple` - 推送与远程上相同名称的当前分支。如果您正在进行中央工作流程（即从同一存储库推送和拉取，通常为 `origin`），则需要配置具有相同名称的上游分支。自 Git 2.0 版本以来，这是默认选项，并且是适合初学者的最安全选项。

  - `matching` - 推送两端具有相同名称的所有分支。这使得您推送到的存储库记住将要被推送出去的一组分支（例如，如果您总是在那里推送 *maint* 和 *master*，而没有其他分支，则您推送到的存储库将具有这两个分支，您本地的 *maint* 和 *master* 将被推送到那里）。

    要有效地使用这种模式，您必须确保在运行 *git push* 之前，您希望推送出去的 *所有* 分支都准备好被推送出去，因为这种模式的整体目的是允许您一次推送所有分支。如果您通常只完成一个分支的工作并推送结果，而其他分支没有完成，那么这种模式不适合您。此外，此模式不适用于推送到共享中央存储库，因为其他人可能会在其中添加新的分支，或在您无法控制的范围内更新现有分支的 tip。

    这曾经是默认值，但自 Git 2.0 版本以来不再是默认值（`simple` 是新的默认值）。

- push.followTags

  如果设置为 true，则默认启用 `--follow-tags` 选项。您可以在推送时覆盖此配置，通过指定 `--no-follow-tags`。

- push.gpgSign

  可设置为布尔值，或字符串 *if-asked*。为 true 时，会导致所有推送都使用 GPG 签名，就像将 `--signed` 传递给 [git-push[1]](../git-push) 一样。字符串 *if-asked* 会导致推送在服务器支持的情况下使用签名，就像将 `--signed=if-asked` 传递给 *git push* 一样。false 值可以覆盖较低优先级配置文件中的值。显式命令行标志始终会覆盖此配置选项。

- push.pushOption

  当命令行中没有给出 `--push-option=<option>` 参数时，`git push` 表现得好像每个该变量的 <value> 值都被给定为 `--push-option=<value>`。

  这是一个多值变量，可以在较高优先级的配置文件（例如存储库中的 `.git/config`）中使用空值来清除从较低优先级配置文件（例如 `$HOME/.gitconfig`）继承的值。

  ```
  Example:
  
  /etc/gitconfig
    push.pushoption = a
    push.pushoption = b
  
  ~/.gitconfig
    push.pushoption = c
  
  repo/.git/config
    push.pushoption =
    push.pushoption = b
  
  This will result in only b (a and c are cleared).
  ```

  

- push.recurseSubmodules

  可以是 "check"、"on-demand"、"only" 或 "no"，具有与 "push --recurse-submodules" 相同的行为。如果未设置，则默认为 *no*，除非设置了 *submodule.recurse*（在这种情况下，true 值表示 *on-demand*）。

- push.useForceIfIncludes

  如果设置为 "true"，则相当于在命令行中给 [git-push[1]](../git-push) 指定了 `--force-if-includes` 选项。在推送时添加 `--no-force-if-includes` 可以覆盖此配置设置。

- push.negotiate

  如果设置为 "true"，则在客户端和服务器尝试找到共同提交的协商轮次中，试图减少发送的 packfile 大小。如果为 "false"，Git 将仅依赖服务器的引用广告来找到共同的提交。

- push.useBitmaps

  如果设置为 "false"，则即使 `pack.useBitmaps` 是 "true"，也会禁用 "git push" 的位图使用，但不会阻止其他 Git 操作使用位图。默认值为 true。

## GIT

 这是[git[1]](../../Git)工具集中的一部分。