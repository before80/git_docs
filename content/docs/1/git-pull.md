+++
title = "git-pull"
weight = 30
type = "docs"
date = 2023-05-08T15:39:23+08:00
description = ""
isCJKLanguage = true
draft = false

+++

# git-pull

https://git-scm.com/docs/git-pull

version 2.41.0

## 名称

​	git-pull - 从另一个存储库或本地分支获取并集成更改

## 概述

```
git pull [<options>] [<repository> [<refspec>…]]
```

## 描述

​	将远程存储库中的更改合并到当前分支。如果当前分支落后于远程分支，默认情况下会将当前分支快进以与远程分支匹配。如果当前分支和远程分支已分歧，则用户需要指定如何使用 `--rebase` 或 `--no-rebase`（或 `pull.rebase` 中的相应配置选项）来协调分歧的分支。

​	更确切地说，`git pull` 使用给定的参数运行 `git fetch`，然后根据配置选项或命令行标志调用 `git rebase` 或 `git merge` 来协调分歧的分支。

​	`<repository>` 应该是作为 [git-fetch[1]](../git-fetch) 传递的远程存储库的名称。`<refspec>` 可以指定任意远程引用（例如，标签的名称），甚至可以是具有相应的远程跟踪分支的引用集合（例如，refs/heads/*:refs/remotes/origin/*），但通常它是远程存储库中的一个分支的名称。

​	`<repository>` 和 `<branch>` 的默认值从当前分支的 "remote" 和 "merge" 配置中读取，这些配置由 [git-branch[1]](../git-branch) `--track` 设置。

​	假设存在以下历史记录，并且当前分支为 "`master`"：

```
	  A---B---C master on origin
	 /
    D---E---F---G master
	^
	origin/master in your repository
```

​	然后 "`git pull`" 将获取并重新应用远程 `master` 分支的更改，自它从本地 `master` 分支（即 `E`）分叉后，直到当前提交（`C`）为止，并将结果记录在一个新提交中，同时记录两个父提交的名称和用户描述更改的日志消息。

```
	  A---B---C origin/master
	 /         \
    D---E---F---G---H master
```

​	有关详情，请参阅 [git-merge[1]](../git-merge)，包括冲突的呈现和处理方式。

​	在 Git 1.7.0 或更高版本中，要取消冲突合并，请使用 `git reset --merge`。**警告**：在较旧版本的 Git 中，不建议在有未提交的更改时运行 *git pull*：虽然可能，但会使您处于一个可能很难解决冲突的状态。

​	如果任何远程更改与本地未提交的更改重叠，则合并将自动取消，并保持工作树不变。通常最好在进行拉取之前让所有本地更改工作正常，或者使用 [git-stash[1]](../git-stash) 将它们保存起来。

## 选项

- -q

- `--quiet`

  将此选项传递给底层的 git-fetch，以在传输过程中压制报告，以及底层的 git-merge，以在合并期间压制输出。

- -v

- `--verbose`

  将 `--verbose` 传递给 git-fetch 和 git-merge。

- `--[no-]recurse-submodules[=yes|on-demand|no]`

  此选项控制是否应该获取已填充子模块的新提交，并且是否应该更新活动子模块的工作树（参见 [git-fetch[1]](../git-fetch)、[git-config[1]](../git-config) 和 [gitmodules[5]](../5/gitmodules)）。如果使用 rebase 进行检出，本地子模块提交也将被 rebase。如果使用 merge 进行更新，则将解决子模块冲突并检出。

### 与merging相关的选项

- `--commit`

- `--no-commit`

  执行合并并提交结果。此选项可用于覆盖 --no-commit。仅在合并时有用。

  使用 --no-commit 执行合并，并在创建合并提交之前停止，以便用户有机会检查和进一步调整合并结果后再提交。

  注意，快进更新不会创建合并提交，因此没有办法使用 `--no-commit` 停止这些合并。因此，如果希望确保合并命令不更改或更新您的分支，请在 `--no-ff` 和 `--no-commit` 中使用 `--no-ff`。

- `--edit`

- -e

- `--no-edit`

  在将成功的机械合并提交前调用编辑器，以进一步编辑自动生成的合并消息，以便用户可以解释和证明合并。`--no-edit` 选项可用于接受自动生成的消息（这通常是不鼓励的）。

  较旧的脚本可能依赖于不允许用户编辑合并日志消息的历史行为。当运行 `git merge` 时，它们将打开一个编辑器。为了更容易调整这些脚本以适应更新后的行为，可以在它们的开始处将环境变量 `GIT_MERGE_AUTOEDIT` 设置为 `no`。

- `--cleanup=<mode>`

  此选项确定合并消息在提交之前将如何清理。有关更多详情，请参阅 [git-commit[1]](../git-commit)。此外，如果 *<mode>* 的值是 `scissors`，则会在提交合并冲突时将 scissors 追加到 `MERGE_MSG`，然后传递给提交机制。

- `--ff-only`

  仅在没有分歧的本地历史时更新到新的历史。当未提供协调分歧历史的方法（通过 --rebase=* 标志）时，这是默认行为。

- `--ff`

- `--no-ff`

  当进行合并而不是变基时，指定合并处理合并历史已经是当前历史的后代时的方式。如果请求合并，则 `--ff` 是默认值，除非合并的是不存储在 `refs/tags/` 层次结构中的注释（可能带有签名）的标签，在这种情况下，默认假定为 `--no-ff`。

  使用 `--ff` 时，如果可能，将合并解决为快进（仅更新分支指针以匹配合并的分支；不创建合并提交）。当不可能时（当合并的历史不是当前历史的后代时），将创建合并提交。

  使用 `--no-ff` 时，在所有情况下都创建合并提交，即使合并可以解决为快进。

- -S[<keyid>]

- `--gpg-sign[=<keyid>]`

- `--no-gpg-sign`

  PG 签名生成合并提交。`keyid` 参数是可选的，默认为提交者的身份；如果指定，必须将其粘贴到选项中而不加空格。`--no-gpg-sign` 用于撤消 `commit.gpgSign` 配置变量和之前的 `--gpg-sign`。

- `--log[=<n>]`

- `--no-log`

  除了分支名称，还使用最多 <n> 个实际合并的一行描述填充日志消息。还请参阅 [git-fmt-merge-msg[1]](../git-fmt-merge-msg)。仅在合并时有用。

  使用 `--no-log` 时，不列出实际合并的一行描述。

- `--signoff`

- `--no-signoff`

  在提交日志消息的末尾添加一个提交者签名（Signed-off-by）尾注。签名的含义取决于您提交的项目。例如，它可以证明提交者有权在项目的许可下提交工作，或者同意某些贡献者声明，例如开发者原始证书。 （有关 Linux 内核和 Git 项目使用的签名，请参阅 [http://developercertificate.org](http://developercertificate.org/)）。请咨询您正在贡献的项目的文档或领导，了解这些签名在该项目中的用途。

  `--no-signoff` 选项可用于撤消之前在命令行上使用的 --signoff 选项。

- `--stat`

- -n

- `--no-stat`

  在合并结束时显示一个 diffstat。diffstat 也由配置选项 merge.stat 控制。

  使用 -n 或 `--no-stat`时，不显示合并结束时的 diffstat。

- `--squash`

- `--no-squash`

  生成工作树和索引状态，就好像进行了真正的合并一样（除了合并信息），但实际上不会进行提交，移动 `HEAD`，或记录 `$GIT_DIR/MERGE_HEAD`（以使下一个 `git commit` 命令创建合并提交）。

  这允许您在当前分支的顶部创建一个单独的提交，其效果与合并另一个分支相同（或者在八爪鱼的情况下更多）。

  使用 --no-squash 时，执行合并并提交结果。此选项可用于覆盖 --squash。使用 --squash 时，不允许使用 --commit，并将失败。

  仅在合并时有用。

- `--[no-]verify`

  默认情况下，会运行 pre-merge 和 commit-msg 钩子。使用 `--no-verify` 可以绕过这些钩子。另请参阅 [githooks[5]](../5/githooks)。

  仅在合并时有用。

- -s <strategy>

- `--strategy=<strategy>`

  使用给定的合并策略；可以多次提供它们，以指定它们的尝试顺序。如果没有 `-s` 选项，则使用内置的合并策略列表（合并单一头时为 `ort`，否则为 `octopus`）。

- -X <option>

- `--strategy-option=<option>`

  将合并策略特定的选项传递给合并策略。

- `--verify-signatures`

- `--no-verify-signatures`

  验证要合并的分支侧的尖端提交是否用有效的密钥签名，即具有有效 uid 的密钥：在默认的信任模型中，这意味着签名密钥已由受信任的密钥签名。如果分支侧的尖端提交没有用有效的密钥签名，合并将中止。

  仅在合并时有用。

- `--summary`

- `--no-summary`

  --stat 和 --no-stat 的同义词；它们已被弃用，并将在将来被删除。

- `--autostash`

- `--no-autostash`

  在操作开始之前自动创建一个临时存储项，并在操作结束后将其记录在特殊的 ref `MERGE_AUTOSTASH` 中。这意味着您可以在脏工作树上运行操作。但是，请小心使用：成功合并后的最终存储应用可能会导致非常复杂的冲突。

- `--allow-unrelated-histories`

  默认情况下，`git merge` 命令拒绝合并不共享共同祖先的历史。当合并两个最初独立开始的项目的历史时，可以使用此选项来覆盖此安全性。由于这是非常罕见的情况，因此不存在启用此选项的配置变量，并且不会添加。

  仅在合并时有用。

- -r

- `--rebase[=false|true|merges|interactive]`

  当为 true 时，在获取后将当前分支变基到上游分支。如果有与上游分支相对应的远程跟踪分支，并且上游分支自上次获取以来已被变基，则变基将使用该信息避免变基非本地更改。

  当设置为 merges 时，使用 `git rebase --rebase-merges` 进行变基，以便本地合并提交包含在变基中（有关详情，请参阅 [git-rebase[1]](../git-rebase)）。

  当为 false 时，将上游分支合并到当前分支。

  当为 interactive 时，启用交互模式的变基。

  See `pull.rebase`, `branch.<name>.rebase` and `branch.autoSetupRebase` in [git-config[1]](../git-config) if you want to make `git pull` always use `--rebase` instead of merging.

  如果要使 `git pull` 总是使用 `--rebase` 而不是合并，请参阅 [git-config[1]](../git-config) 中的 `pull.rebase`、`branch.<name>.rebase` 和 `branch.autoSetupRebase`。

  > 注意
>
  > ​	这是一种潜在的*危险*操作模式。它会重写历史记录，这在您已经发布该历史记录时并不好。除非您已经仔细阅读 [git-rebase[1]](../git-rebase)，否则不要使用此选项。

- `--no-rebase`

  This is shorthand for `--rebase=false`.
  
  这是 `--rebase=false` 的缩写形式。

### 与fetching相关的选项

- `--all`

  获取所有远程存储库。

- -a

- `--append`

  将获取的引用的 ref 名称和对象名称追加到 `.git/FETCH_HEAD` 的现有内容中。如果没有此选项，`.git/FETCH_HEAD` 中的旧数据将被覆盖。

- `--atomic`

  使用原子事务更新本地 ref。要么更新所有 ref，要么在出现错误时不更新任何 ref。

- `--depth=<depth>`

  将获取限制为每个远程分支历史的指定提交数。如果通过 `git clone` 选项 `--depth=<depth>` 创建了*浅* 存储库（请参阅 [git-clone[1]](../git-clone)），则将深化或缩短历史记录以指定的提交数。不获取深化提交的标签。

- `--deepen=<depth>`

  类似于 `--depth`，不过它是指定从当前浅边界开始的提交数，而不是从每个远程分支历史的尖端开始的提交数。

- `--shallow-since=<date>`

  将浅存储库的历史记录深化或缩短，以包括 <date> 之后的所有可达提交。

- `--shallow-exclude=<revision>`

  将浅存储库的历史深化或缩短，以排除从指定的远程分支或标签可达的提交。此选项可以多次指定。

- `--unshallow`

  如果源存储库是完整的，则将浅存储库转换为完整存储库，删除浅存储库所施加的所有限制。如果源存储库是浅存储库，则尽可能获取更多内容，以使当前存储库具有与源存储库相同的历史记录。

  如果源存储库是完整的，则将浅存储库转换为完整的存储库，消除浅存储库 imposed limitations imposed by shallow repositories。如果源存储库是浅存储库，则尽可能获取数据，使当前存储库具有与源存储库相同的历史记录。

- `--update-shallow`

  默认情况下，当从浅存储库中获取时，`git fetch` 拒绝需要更新 `.git/shallow` 的引用。此选项更新 `.git/shallow` 并接受这样的引用。

- `--negotiation-tip=<commit|glob>`

  默认情况下，Git 会报告到服务器上的所有本地引用可达的提交，以查找共同的提交，以减小待接收 packfile 的大小。如果指定，Git 只会报告从给定的引用点可达的提交。当用户知道哪个本地引用可能与正在获取的上游引用有共同提交时，这对于加快获取速度很有用。

  可以多次指定此选项；如果是这样，Git 将按命令行上列出的顺序将所有提交报告给对方。

  此选项的参数可以是 ref 名称的通配符、引用或（可能是缩写的）提交的 SHA-1。指定通配符等效于多次指定此选项，每次匹配一个引用名称。

  另请参阅 [git-config[1]](../git-config) 中记录的 `fetch.negotiationAlgorithm` 和 `push.negotiate` 配置变量，以及下面的 `--negotiate-only` 选项。

- `--negotiate-only`

  不从服务器获取任何内容，而是打印与服务器共有的提供的 `--negotiation-tip=*` 参数的祖先。这与 `--recurse-submodules=[yes|on-demand]` 不兼容。内部上使用此选项来实现 `push.negotiate` 选项，请参阅 [git-config[1]](../git-config)。

- `--dry-run`

  显示将执行的操作，但不进行任何更改。

- -f

- `--force`

  当使用 `<src>:<dst>` refspec 时，*git fetch* 可能会拒绝更新本地分支，如 [git-fetch[1]](../git-fetch) 文档中的 `<refspec>` 部分所讨论的那样。此选项覆盖该检查。

- -k

- `--keep`

  保留下载的 pack。

- `--prefetch`

  修改配置的 refspec，将所有引用放入 `refs/prefetch/` 命名空间。请参阅 [git-maintenance[1]](../git-maintenance) 中的 `prefetch` 任务。

- -p

- `--prune`

  在获取之前，删除在远程不存在的任何远程跟踪引用。如果仅因默认的标签自动跟踪或由于 `--tags` 选项而获取标签，则不会对标签进行修剪。但是，如果标签是由于显式 refspec（无论是在命令行上还是在远程配置中，例如如果使用 `--mirror` 选项克隆了远程），则它们也会被修剪。提供 `--prune-tags` 等同于提供标签 refspec。

- `--no-tags`

  默认情况下，从远程存储库下载的对象所指向的标签会被获取并存储在本地。此选项禁用此自动标签跟踪功能。可以使用 remote.<name>.tagOpt 设置来指定远程的默认行为。请参阅 [git-config[1]](../git-config)。

- `--refmap=<refspec>`

  在获取命令行上列出的引用时，使用指定的 refspec（可以多次给出）将引用映射到远程跟踪分支，而不是使用配置变量 `remote.*.fetch` 的值。向 `--refmap` 选项提供一个空的 `<refspec>` 可以使 Git 忽略配置的 refspec，并完全依赖于作为命令行参数提供的 refspec。有关详细信息，请参阅 "已配置的远程跟踪分支" 部分。

- -t

- `--tags`

  从远程获取所有标签（即将远程标签 `refs/tags/*` 获取到具有相同名称的本地标签），以及其他可能获取的内容。仅使用此选项不会将标签暴露给修剪，即使使用了 --prune（尽管如果它们也是显式 refspec 的目标，则标签可能会被修剪；请参阅 `--prune`）。

- -j

- `--jobs=<n>`

  用于所有形式的获取的并行子进程数。如果指定了 --multiple 选项，则不同的远程将并行获取。如果获取了多个子模块，则它们将并行获取。要单独控制它们，请使用配置设置 fetch.parallel 和 submodule.fetchJobs（请参阅 [git-config[1]](../git-config)）。通常，并行递归和多远程获取速度会更快。默认情况下，获取是顺序执行的，而不是并行的。

- `--set-upstream`

  If the remote is fetched successfully, add upstream (tracking) reference, used by argument-less [git-pull[1]](../git-pull) and other commands. For more information, see `branch.<name>.merge` and `branch.<name>.remote` in [git-config[1]](../git-config).

  如果成功获取远程，则添加上游（跟踪）引用，供无参数 [git-pull[1]](../git-pull) 和其他命令使用。有关更多信息，请参阅 [git-config[1]](../git-config) 中的 `branch.<name>.merge` 和 `branch.<name>.remote`。

- --upload-pack <upload-pack>

  如果给定，并且要获取的存储库由 *git fetch-pack* 处理，则会将 `--exec=<upload-pack>` 传递给该命令，以指定在对端运行命令的非默认路径。

- `--progress`

  默认情况下，如果标准错误流连接到终端，则在标准错误流上报告进度状态（除非使用 -q）。此标志会强制在标准错误流未连接到终端时仍报告进度状态。

- -o <option>

- `--server-option=<option>`

  使用协议版本 2 进行通信时，将给定的字符串传输到服务器。给定的字符串不得包含 NUL 或 LF 字符。服务器对服务器选项的处理（包括未知选项）与服务器特定。当给定多个 `--server-option=<option>` 时，它们将按照命令行上列出的顺序全部发送到对方。

- `--show-forced-updates`

  默认情况下，git 检查在获取期间是否进行了强制更新。这可以通过 fetch.showForcedUpdates 禁用，但 --show-forced-updates 选项确保进行此检查。请参阅 [git-config[1]](../git-config)。

- `--no-show-forced-updates`

  默认情况下，git 检查在获取期间是否进行了强制更新。传递 --no-show-forced-updates 或将 fetch.showForcedUpdates 设置为 false，以跳过此检查以提高性能。如果在 *git-pull* 中使用，则 --ff-only 选项仍将在尝试进行快进更新之前检查强制更新。请参阅 [git-config[1]](../git-config)。

- -4

- `--ipv4`

  仅使用 IPv4 地址，忽略 IPv6 地址。

- -6

- `--ipv6`

  仅使用 IPv6 地址，忽略 IPv4 地址。

- <repository>

  执行获取或拉取操作的"远程"存储库。此参数可以是 URL（请参阅下面的 [GIT URLS](https://git-scm.com/docs/git-pull#URLS) 部分）或远程的名称（请参阅下面的 [REMOTES](https://git-scm.com/docs/git-pull#REMOTES) 部分）。

- <refspec>

  指定要获取的引用和要更新的本地引用。当命令行上没有出现 <refspec> 时，将从 `remote.<repository>.fetch` 变量中读取要获取的引用（请参阅 [git-fetch[1]](../git-fetch) 中的 "CONFIGURED REMOTE-TRACKING BRANCHES" 部分）。
  
  <refspec> 参数的格式是一个可选的加号 `+`，后跟源 <src>，后跟冒号 `:`，后跟目标引用 <dst>。当 <dst> 为空字符串时，可以省略冒号。通常，<src> 是一个引用，但也可以是完全拼写的十六进制对象名称。
  
  <refspec> 可以在其 <src> 中包含 `*`，以指示简单的模式匹配。这样的 refspec 的功能类似于使用相同前缀匹配任何引用的 glob。模式 <refspec> 必须在 <src> 和 <dst> 中都有一个 `*`。它将通过将 `*` 替换为从源匹配的内容来将引用映射到目标。
  
  如果 refspec 以 `^` 为前缀，则它将被解释为负 refspec。与指定要获取的引用或要更新的本地引用不同，这样的 refspec 将指定要排除的引用。如果一个引用与至少一个正 refspec 匹配，且不与任何负 refspec 匹配，则将视为匹配。负 refspec 对于限制模式 refspec 的范围非常有用，以便不包含特定引用。负 refspec 本身可以是模式 refspec。但是，它们只能包含一个 <src>，不指定 <dst>。也不支持完全拼写的十六进制对象名称。
  
  `tag <tag>` means the same as `refs/tags/<tag>:refs/tags/<tag>`; it requests fetching everything up to the given tag.
  
  `tag <tag>` 的含义与 `refs/tags/<tag>:refs/tags/<tag>` 相同；它请求获取直到给定标签的所有内容。
  
  匹配 <src> 的远程引用会被获取，并且如果 <dst> 不为空字符串，则会尝试更新与之匹配的本地引用。
  
  是否允许不带 `--force` 更新取决于它被获取到的 ref 命名空间、正在获取的对象类型以及该更新是否被视为快进。通常，与推送相同的规则适用于获取，有关这些规则的信息，请参阅 [git-push[1]](../git-push) 的 `<refspec>...` 部分。*git fetch* 的特定例外情况如下所示。
  
  在 Git 版本 2.20 之前，与使用 [git-push[1]](../git-push) 推送时不同，任何对 `refs/tags/*` 的更新都会在 refspec 中没有 `+`（或 `--force`）的情况下（或 `--force`）接受。在获取时，我们通常将所有标签更新从远程不加选择地视为强制获取。自 Git 版本 2.20 以来，获取以更新 `refs/tags/*` 的方式与推送时相同。即没有在 refspec 中（或使用 `--force` 命令行选项）时将拒绝任何更新。
  
  与使用 [git-push[1]](../git-push) 推送时不同，任何在 `refs/{tags,heads}/*` 之外的更新都将在 refspec 中没有 `+`（或 `--force`）的情况下接受，无论是交换例如树对象与 blob，还是提交与没有上一个提交作为祖先的另一个提交等。
  
  与使用 [git-push[1]](../git-push) 推送时不同，没有配置将修改这些规则的方法，也没有类似于 `pre-receive` 钩子的 `pre-fetch` 钩子。
  
  与使用 [git-push[1]](../git-push) 推送时一样，关于不允许作为更新的内容的所有规则都可以通过在 refspec 前添加可选的前导 `+`（或使用 `--force` 命令行选项）来覆盖。唯一的例外是无论如何强制也不会使 `refs/heads/*` 命名空间接受非提交对象。
  
  > 注意
  >
  > ​	当知道要获取的远程分支经常被回退和重新基于时，预计其新的 tip 不会是其上次获取时的祖先（存储在上次获取时的远程跟踪分支中）。对于这样的分支，您可能需要使用 `+` 符号来指示需要非快进更新。没有办法确定或声明分支将在具有此行为的存储库中可用；拉取用户只需知道这是分支的预期使用模式。
  
  > 注意
  >
  > ​	在 *git pull* 命令行上直接列出多个 <refspec> 与为 <repository> 在配置中有多个 `remote.<repository>.fetch` 条目并且在不带任何显式 <refspec> 参数的情况下运行 *git pull* 命令之间有区别。在命令行上明确列出的 <refspec> 始终会在获取后合并到当前分支。换句话说，如果列出多个远程引用，*git pull* 将创建一个 Octopus 合并。另一方面，如果在命令行上没有列出任何显式的 <refspec> 参数，则 *git pull* 将获取在 `remote.<repository>.fetch` 配置中找到的所有 <refspec> 并仅合并找到的第一个 <refspec> 到当前分支。这是因为很少做 Octopus 合并，而通过获取多个远程头来跟踪多个远程头经常很有用。

## GIT URLS

​	通常情况下，URL 包含有关传输协议、远程服务器地址和存储库路径的信息。根据传输协议，其中的一些信息可能不存在。

​	Git 支持 ssh、git、http 和 https 协议（除此之外，还可以使用 ftp 和 ftps 进行获取，但这是效率低且已被弃用的，请不要使用）。

​	原生传输（即 git:// URL）不进行身份验证，应谨慎在不安全的网络上使用。

​	以下语法可与这些协议一起使用：

- ssh://[user@]host.xz[:port]/path/to/repo.git/
- git://host.xz[:port]/path/to/repo.git/
- http[s]://host.xz[:port]/path/to/repo.git/
- ftp[s]://host.xz[:port]/path/to/repo.git/

​	ssh 协议还可以使用类似于 scp 的语法：

- [user@]host.xz:path/to/repo.git/

​	当冒号之前没有斜杠时，才会识别此语法。这有助于区分包含冒号的本地路径。例如，本地路径 `foo:bar` 可以被指定为绝对路径或 `./foo:bar`，以避免被误解为 ssh URL。

​	ssh 和 git 协议还支持 `~username` 扩展：

- ssh://[user@]host.xz[:port]/~[user]/path/to/repo.git/
- git://host.xz[:port]/~[user]/path/to/repo.git/
- [user@]host.xz:/~[user]/path/to/repo.git/

​	对于本地存储库，Git 也原生支持以下语法：

- /path/to/repo.git/
- file:///path/to/repo.git/

​	这两种语法在大多数情况下是等效的，但在克隆时，前者意味着使用 --local 选项。有关详细信息，请参阅 [git-clone[1]](../git-clone)。

​	*git clone*、*git fetch* 和 *git pull*（但不包括 *git push*）还可以接受适当的捆绑文件。请参阅 [git-bundle[1]](../git-bundle)。

​	当 Git 不知道如何处理特定的传输协议时，它会尝试使用远程辅助程序 *remote-<transport>*（如果存在）。为了显式请求远程辅助程序，可以使用以下语法：

- <transport>::<address>

其中，<address> 可以是路径、服务器和路径的组合，或者是特定远程辅助程序识别的任意类似于 URL 的字符串。有关详细信息，请参阅 [gitremote-helpers[7]](../7/gitremote-helpers)。

​	如果有大量同名的远程存储库，并且想要为它们使用不同的格式（以使您使用的URL将被重写为有效的URL），可以创建以下形式的配置节：

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

在任何需要URL的上下文中，类似于 "work:repo.git" 或 "host.xz:/path/to/repo.git" 的URL将被重写为 "git://git.host.xz/repo.git"。

​	如果只想重写推送的URL，可以创建以下形式的配置节：

```
	[url "<actual url base>"]
		pushInsteadOf = <other url base>
```

​	例如，使用以下内容：

```
	[url "ssh://example.org/"]
		pushInsteadOf = git://example.org/
```

对于推送，类似于 "git://example.org/path/to/repo.git" 的URL将被重写为 "ssh://example.org/path/to/repo.git"，但是拉取仍将使用原始URL。

## 远程

​	在 `<repository>` 参数中，可以使用以下内容之一的名称，而不是使用 URL：

- Git 配置文件（`$GIT_DIR/config`）中的远程。
- `$GIT_DIR/remotes` 目录中的文件。
- `$GIT_DIR/branches` 目录中的文件。

所有这些都允许在命令行中省略 refspec，因为它们都包含 Git 默认会使用的 refspec。

### 在配置文件中命名的远程

​	您可以选择提供先前使用 [git-remote[1]](../git-remote)、[git-config[1]](../git-config) 或手动编辑 `$GIT_DIR/config` 文件配置的远程的名称。此远程的 URL 将用于访问存储库。当您未在命令行上提供 refspec 时，此远程的 refspec 将作为默认值使用。配置文件中的条目应如下所示：

```
	[remote "<name>"]
		url = <URL>
		pushurl = <pushurl>
		push = <refspec>
		fetch = <refspec>
```

​	`<pushurl>` 仅用于推送。它是可选的，默认为 `<URL>`。向远程推送将影响所有已定义的推送 URL，或者如果未定义任何推送 URL，则影响所有已定义的 URL。但是，获取操作仅在定义多个 URL 时从第一个已定义的 URL 获取。

### `$GIT_DIR/remotes` 中的命名文件

​	您可以选择提供 `$GIT_DIR/remotes` 中文件的名称。此文件中的 URL 将用于访问存储库。当您未在命令行上提供 refspec 时，此文件中的 refspec 将作为默认值使用。此文件应具有以下格式：

```
	URL: one of the above URL format
	Push: <refspec>
	Pull: <refspec>
```

`Push:` 行用于 *git push*，`Pull:` 行用于 *git pull* 和 *git fetch*。可以为额外的分支映射指定多个 `Push:` 和 `Pull:` 行。

### `$GIT_DIR/branches` 中的命名文件

​	您可以选择提供 `$GIT_DIR/branches` 中文件的名称。此文件中的 URL 将用于访问存储库。此文件应具有以下格式：

```
	<URL>#<head>
```

`<URL>` 是必需的，`#<head>` 是可选的。

​	根据操作，git 将使用以下 refspec 之一（如果在命令行上未提供 refspec）。`<branch>` 是 `$GIT_DIR/branches` 中的文件名，并且 `<head>` 默认为 `master`。

git fetch 使用：

```
	refs/heads/<head>:refs/heads/<branch>
```

git push 使用：

```
	HEAD:refs/heads/<head>
```

## 合并策略

​	合并机制（`git merge` 和 `git pull` 命令）允许选择后端 *合并策略*，并通过 `-s` 选项进行选择。某些策略还可以接受它们自己的选项，可以通过给 `git merge` 和/或 `git pull` 传递 `-X<option>` 参数来传递这些选项。

- ort

  这是拉取或合并一个分支时的默认合并策略。此策略只能使用三路合并算法解决两个分支的冲突。当存在多个可用于三路合并的共同祖先时，它会创建合并的共同祖先的树，并将其用作三路合并的参考树。据实际从 Linux 2.6 内核开发历史中获取的合并提交的测试显示，相比之前的默认算法 `recursive`，此策略可以减少合并冲突，同时不会导致错误的合并。此外，此策略可以检测和处理涉及重命名的合并。它不使用检测到的复制操作。该算法的名称是一个首字母缩写（“Ostensibly Recursive’s Twin”），它来源于它作为先前默认算法 `recursive` 的替代品而编写的事实。

  *ort* 策略可以使用以下选项：

  - ours

    此选项强制将冲突的块自动与“我们”的版本解决干净。不与我们方冲突的来自其他分支的更改会反映在合并结果中。对于二进制文件，整个内容都将采用我们的版本。

    请不要与 *ours* 合并策略混淆，后者根本不查看其他分支包含的内容。它丢弃其他分支的所有内容，声明我们的历史包含其中发生的一切。

  - theirs

    这与 *ours* 相反；请注意，与 *ours* 不同，没有 *theirs* 合并策略以混淆此合并选项。

  - ignore-space-change

  - ignore-all-space

  - ignore-space-at-eol

  - ignore-cr-at-eol

    对于三路合并，将具有指定类型空格更改的行视为未更改。不会忽略包含其他更改的行中的空格更改。另请参阅 [git-diff[1]](../git-diff) 的 `-b`、`-w`、`--ignore-space-at-eol` 和 `--ignore-cr-at-eol` 选项。

    - 如果 *their* 版本只对行进行空格更改，则使用 *our* 版本；

    - 如果 *our* 版本对行进行空格更改，但 *their* 版本包含实质性更改，则使用 *their* 版本；
- 否则，合并将按常规方式进行。
  

  
- renormalize
  
  解决三路合并时，运行虚拟的检出和检入，以处理文件的所有三个阶段。此选项用于在合并具有不同干净筛选器或行尾标准化规则的分支时使用。有关详细信息，请参阅 [gitattributes[5]](../5/gitattributes) 中的“在具有不同签入/签出属性的分支上合并”部分。
  
- no-renormalize
  
  禁用 `renormalize` 选项。这将覆盖 `merge.renormalize` 配置变量。
  
- find-renames[=<n>]
  
    打开重命名检测，可选择设置相似性阈值。这是默认行为。这将覆盖 *merge.renames* 配置变量。另请参阅 [git-diff[1]](../git-diff) 的 `--find-renames` 选项。
  
  - subtree[=<path>]

    此选项是 *subtree* 策略的更高级形式，当合并树 A 和 B 时，如果 B 对应于 A 的子树，则首先将 B 调整为与 A 的树结构匹配，而不是在同级读取树。这种调整也适用于共同祖先树。

- recursive

  此策略只能使用三路合并算法解决两个分支的冲突。当存在多个可用于三路合并的共同祖先时，它会创建合并的共同祖先的树，并将其用作三路合并的参考树。据实际从 Linux 2.6 内核开发历史中获取的合并提交的测试显示，相比之前的默认算法 `recursive`，此策略可以减少合并冲突，同时不会导致错误的合并。此外，此策略可以检测和处理涉及重命名的合并。它不使用检测到的复制操作。自 Git v0.99.9k 到 v2.33.0，这是解决两个分支的默认策略。

  *recursive* 策略使用与 *ort* 相同的选项。但是，*recursive* 还有三个 *ort* 忽略的其他选项（未在上述文本中记录），这些选项可能与 *recursive* 策略一起使用：patienceDeprecated 代表 `diff-algorithm=patience` 的同义词。

  - diff-algorithm=[patience|minimal|histogram|myers]

    在合并时使用不同的 diff 算法，这可以帮助避免由于不重要的匹配行（例如不同函数中的大括号）导致的错误合并。另请参阅 [git-diff[1]](../git-diff) 的 `--diff-algorithm`。请注意，`ort` 特别使用 `diff-algorithm=histogram`，而 `recursive` 默认使用 `diff.algorithm` 配置设置。

  - no-renames

    关闭重命名检测。这将覆盖 `merge.renames` 配置变量。另请参阅 [git-diff[1]](../git-diff) 的 `--no-renames` 选项。

- resolve

  此策略只能使用三路合并算法解决两个头（即当前分支和另一个从中拉取的分支）的冲突。它尝试仔细检测交叉合并的不明确性。它不处理重命名。

- octopus

  This resolves cases with more than two heads, but refuses to do a complex merge that needs manual resolution. It is primarily meant to be used for bundling topic branch heads together. This is the default merge strategy when pulling or merging more than one branch.

  此策略解决多于两个头的情况，但拒绝进行需要手动解决的复杂合并。它主要用于捆绑主题分支头。这是拉取或合并多个分支时的默认合并策略。

- ours

  此策略解决任意数量的头，但合并的结果树始终是当前分支头的树，从而忽略所有其他分支的更改。它用于替代旧的侧分支开发历史。请注意，这与 *recursive* 合并策略的 `-Xours` 选项不同。

- subtree

  这是一个修改版的 `ort` 策略。当合并树 A 和 B 时，如果 B 对应于 A 的子树，则首先将 B 调整为与 A 的树结构匹配，而不是在同级读取树。此调整也适用于共同祖先树。

​	对于使用三路合并的策略（包括默认的 *ort* 策略），如果在两个分支上进行了更改，但后来在其中一个分支上撤销了更改，那么该更改将出现在合并结果中；有些人可能会对这种行为感到困惑。这是因为在执行合并时，仅考虑头和合并基准，而不考虑个别提交。因此，合并算法认为撤销的更改实际上没有发生任何更改，并将其替换为已更改的版本。

## 默认行为

​	通常，人们在使用 `git pull` 时没有提供任何参数。传统上，这相当于执行 `git pull origin`。然而，如果分支 `<name>` 上存在配置 `branch.<name>.remote`，则使用该值代替 `origin`。

​	为了确定要从中获取数据的URL，首先查找配置 `remote.<origin>.url` 的值，如果没有这个变量，则使用 `$GIT_DIR/remotes/<origin>` 中的 `URL:` 行的值。

​	在不提供命令行上的任何 refspec 参数的情况下运行命令时，将查找配置变量 `remote.<origin>.fetch` 的值，并且如果没有，则查找 `$GIT_DIR/remotes/<origin>` 并使用其 `Pull:` 行。除了在选项部分描述的 refspec 格式之外，您还可以使用类似以下的 globbing refspec：

```
refs/heads/*:refs/remotes/origin/*
```

​	在不提供命令行上的任何 refspec 参数的情况下运行命令时，将查找配置变量 `remote.<origin>.fetch` 的值，并且如果没有，则查找 `$GIT_DIR/remotes/<origin>` 并使用其 `Pull:` 行。除了在选项部分描述的 refspec 格式之外，您还可以使用类似以下的 globbing refspec：

​	

​	决定在获取后合并哪个远程分支的规则有点复杂，为了不破坏向后兼容性。

​	如果在 `git pull` 命令行中提供了显式的 refspec，则会将所有这些 refspec 进行合并。

​	当命令行上没有提供 refspec 时，`git pull` 将使用配置或 `$GIT_DIR/remotes/<origin>` 中的 refspec。在这种情况下，以下规则适用：

1. 如果当前分支 `<name>` 的 `branch.<name>.merge` 配置存在，则该配置指定了要合并的远程站点的分支名称。
5. 如果 refspec 是一个 globbing refspec，则不进行合并操作。
6. 否则，合并第一个 refspec 的远程分支。

## 示例

- 更新从中克隆的仓库的远程跟踪分支，然后将其中一个分支合并到当前分支：

  ``` bash
  $ git pull
  $ git pull origin
  ```

  通常合并的分支是远程仓库的 HEAD，但是选择是由 `branch.<name>.remote` 和 `branch.<name>.merge` 选项决定的；详情请参阅 [git-config[1]](../git-config)。

- 合并远程分支 `next` 到当前分支：

  ``` bash
  $ git pull origin next
  ```

  这将暂时将 `next` 复制到 FETCH_HEAD，并更新远程跟踪分支 `origin/next`。也可以通过执行 fetch 和 merge 完成相同的操作：

  ``` bash
  $ git fetch origin
  $ git merge origin/next
  ```


​	如果您尝试的拉取操作导致复杂的冲突，并且想要重新开始，您可以使用 *git reset* 进行恢复。

## 安全性

​	fetch 和 push 协议不是设计用于防止一方从另一个仓库窃取未经共享的数据。如果您有需要保护免受恶意对等方窃取的私有数据，最好的选择是将其存储在另一个仓库中。这适用于客户端和服务器。特别是服务器上的命名空间对于读取访问控制是无效的；应该仅向信任对整个仓库进行读取访问的客户端授予对命名空间的读取访问权限。

​	已知的攻击向量如下：

1. 受害者发送“have”行，广告其具有的对象 ID，这些对象 ID 并未明确共享，但可以用于优化传输（如果对等方也具有这些对象）。攻击者选择要窃取的对象 ID X，并发送对 X 的引用，但不需要发送 X 的内容，因为受害者已经拥有它。现在，受害者相信攻击者具有 X，并在稍后将 X 的内容发送回攻击者。（此攻击对于客户端在服务器上执行是最直接的，它在客户端具有访问权限的命名空间中为 X 创建一个引用，然后获取它。服务器对客户端执行此攻击的最可能方式是将 X 合并到公共分支中，并希望用户在此分支上进行其他工作，并将其推送回服务器，而不会注意到合并。）
3. 与 #1 相似，攻击者选择要窃取的对象 ID X。受害者发送攻击者已经拥有的对象 Y，并且攻击者虚假声称拥有 X 而没有 Y，因此受害者将 Y 作为针对 X 的 delta 发送。该 delta 向攻击者显示了 X 的某些区域与 Y 相似。

## BUGS

​	目前，`--recurse-submodules` 只能在已经检出子模块的情况下获取新提交。例如，如果上游在刚刚获取的超级仓库的提交中添加了一个新的子模块，那么子模块本身无法被获取，这将使得以后无法检出该子模块而不必再次获取。这预计将在未来的 Git 版本中修复。

## 另请参阅

[git-fetch[1]](../git-fetch), [git-merge[1]](../git-merge), [git-config[1]](../git-config)

## GIT

  这是[git[1]](../../Git)工具集中的一部分。