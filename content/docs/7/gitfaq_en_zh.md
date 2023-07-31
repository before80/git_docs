+++
title = "gitfaq——中英对照版"
weight = 30
type = "docs"
date = 2023-07-30T15:39:23+08:00
description = ""
isCJKLanguage = true
draft = false
+++

# gitfaq

https://git-scm.com/docs/gitfaq

version 2.41.0

## 名称

gitfaq - Frequently asked questions about using Git

gitfaq - Git 使用常见问题解答

## 概要

gitfaq

## 描述

The examples in this FAQ assume a standard POSIX shell, like `bash` or `dash`, and a user, A U Thor, who has the account `author` on the hosting provider `git.example.org`.

​	本常见问题解答中的示例假定使用标准 POSIX shell，如`bash`或`dash`，以及用户 A U Thor，在托管提供者 `git.example.org` 上拥有帐户 `author`。

## 配置

- What should I put in `user.name`?

- `user.name` 应该放什么内容？

  You should put your personal name, generally a form using a given name and family name. For example, the current maintainer of Git uses "Junio C Hamano". This will be the name portion that is stored in every commit you make.This configuration doesn’t have any effect on authenticating to remote services; for that, see `credential.username` in [git-config[1]](../../1/git-config).

  您应该输入您的个人名称，通常是给定名和姓氏的形式。例如，Git 的当前维护者使用 "Junio C Hamano"。这将是存储在您进行每次提交时的名称部分。该配置对于身份验证到远程服务没有任何影响；有关该方面的信息，请参阅 [git-config[1]](../../1/git-config) 中的 `credential.username`。

- What does `http.postBuffer` really do?

- `http.postBuffer` 究竟是做什么的？

  This option changes the size of the buffer that Git uses when pushing data to a remote over HTTP or HTTPS. If the data is larger than this size, libcurl, which handles the HTTP support for Git, will use chunked transfer encoding since it isn’t known ahead of time what the size of the pushed data will be.Leaving this value at the default size is fine unless you know that either the remote server or a proxy in the middle doesn’t support HTTP/1.1 (which introduced the chunked transfer encoding) or is known to be broken with chunked data. This is often (erroneously) suggested as a solution for generic push problems, but since almost every server and proxy supports at least HTTP/1.1, raising this value usually doesn’t solve most push problems. A server or proxy that didn’t correctly support HTTP/1.1 and chunked transfer encoding wouldn’t be that useful on the Internet today, since it would break lots of traffic.Note that increasing this value will increase the memory used on every relevant push that Git does over HTTP or HTTPS, since the entire buffer is allocated regardless of whether or not it is all used. Thus, it’s best to leave it at the default unless you are sure you need a different value.

  此选项更改 Git 在通过 HTTP 或 HTTPS 推送数据到远程时使用的缓冲区大小。如果数据大于此大小，负责 Git 的 HTTP 支持的 libcurl 将使用分块传输编码（chunked transfer encoding），因为在推送数据大小之前无法预先知道。除非您知道远程服务器或中间代理不支持 HTTP/1.1（其中引入了分块传输编码）或已知会在处理分块数据时出现问题，否则将保留此值为默认大小是可以的。通常（错误地）建议将此值提高作为解决通用推送问题的方法，但由于几乎每个服务器和代理至少支持 HTTP/1.1，提高此值通常并不能解决大多数推送问题。在今天的互联网上，不正确支持 HTTP/1.1 和分块传输编码的服务器或代理并不实用，因为它会破坏大量的流量。请注意，增加此值将增加 Git 通过 HTTP 或 HTTPS 进行的每次相关推送使用的内存，因为无论是否使用整个缓冲区，它都会被全部分配。因此，除非您确定需要不同的值，最好将其保留为默认值。

- How do I configure a different editor?

- 如何配置不同的编辑器？

  If you haven’t specified an editor specifically for Git, it will by default use the editor you’ve configured using the `VISUAL` or `EDITOR` environment variables, or if neither is specified, the system default (which is usually `vi`). Since some people find `vi` difficult to use or prefer a different editor, it may be desirable to change the editor used.If you want to configure a general editor for most programs which need one, you can edit your shell configuration (e.g., `~/.bashrc` or `~/.zshenv`) to contain a line setting the `EDITOR` or `VISUAL` environment variable to an appropriate value. For example, if you prefer the editor `nano`, then you could write the following:

  如果您尚未为 Git 指定特定的编辑器，默认情况下它将使用通过 `VISUAL` 或 `EDITOR` 环境变量配置的编辑器，或者如果两者都未指定，则使用系统默认编辑器（通常是 `vi`）。由于有些人发现 `vi` 难以使用或更喜欢其他编辑器，因此可能希望更改所使用的编辑器。如果要为大多数需要编辑器的程序配置通用编辑器，可以编辑您的 shell 配置文件（例如 `~/.bashrc` 或 `~/.zshenv`），在其中添加一行设置 `EDITOR` 或 `VISUAL` 环境变量为适当的值。例如，如果您喜欢编辑器 `nano`，可以写入以下内容：

  ```
  export VISUAL=nano
  ```

  If you want to configure an editor specifically for Git, you can either set the `core.editor` configuration value or the `GIT_EDITOR` environment variable. You can see [git-var[1]](../../1/git-var) for details on the order in which these options are consulted.Note that in all cases, the editor value will be passed to the shell, so any arguments containing spaces should be appropriately quoted. Additionally, if your editor normally detaches from the terminal when invoked, you should specify it with an argument that makes it not do that, or else Git will not see any changes. An example of a configuration addressing both of these issues on Windows would be the configuration `"C:\Program Files\Vim\gvim.exe" --nofork`, which quotes the filename with spaces and specifies the `--nofork` option to avoid backgrounding the process.

  如果要为 Git 特定地配置编辑器，可以设置 `core.editor` 配置值或 `GIT_EDITOR` 环境变量。有关这些选项的咨询顺序的详细信息，请参阅 [git-var[1]](../../1/git-var)。请注意，在所有情况下，编辑器值都将传递给 shell，因此任何包含空格的参数都应适当引用。此外，如果您的编辑器通常在调用时与终端断开连接，则应该指定一个不会这样做的参数，否则 Git 将无法检测到任何更改。在 Windows 上解决这两个问题的配置示例是 `"C:\Program Files\Vim\gvim.exe" --nofork`，它使用带有空格的文件名并指定了 `--nofork` 选项以避免将进程转为后台运行。

## 凭据

- How do I specify my credentials when pushing over HTTP?

- 在通过 HTTP 推送时，如何指定我的凭据？

  The easiest way to do this is to use a credential helper via the `credential.helper` configuration. Most systems provide a standard choice to integrate with the system credential manager. For example, Git for Windows provides the `wincred` credential manager, macOS has the `osxkeychain` credential manager, and Unix systems with a standard desktop environment can use the `libsecret` credential manager. All of these store credentials in an encrypted store to keep your passwords or tokens secure.In addition, you can use the `store` credential manager which stores in a file in your home directory, or the `cache` credential manager, which does not permanently store your credentials, but does prevent you from being prompted for them for a certain period of time.You can also just enter your password when prompted. While it is possible to place the password (which must be percent-encoded) in the URL, this is not particularly secure and can lead to accidental exposure of credentials, so it is not recommended.

  最简单的方法是通过 `credential.helper` 配置使用凭据帮助程序。大多数系统提供了与系统凭据管理器集成的标准选择。例如，Git for Windows 提供了 `wincred` 凭据管理器，macOS 使用 `osxkeychain` 凭据管理器，而带有标准桌面环境的 Unix 系统可以使用 `libsecret` 凭据管理器。所有这些凭据管理器都会将凭据存储在加密存储中，以保护您的密码或令牌的安全性。此外，您还可以使用 `store` 凭据管理器，它会将凭据存储在主目录中的文件中，或者使用 `cache` 凭据管理器，它不会永久存储您的凭据，但会在一定时间内避免提示您输入凭据。您还可以在提示时输入密码。虽然可以将密码（必须进行百分比编码）放在 URL 中，但这样并不特别安全，并且可能导致凭据意外暴露，因此不推荐这样做。

- How do I read a password or token from an environment variable?

- 如何从环境变量中读取密码或令牌？

  The `credential.helper` configuration option can also take an arbitrary shell command that produces the credential protocol on standard output. This is useful when passing credentials into a container, for example.Such a shell command can be specified by starting the option value with an exclamation point. If your password or token were stored in the `GIT_TOKEN`, you could run the following command to set your credential helper:

  `credential.helper` 配置选项还可以接受一个任意的 shell 命令，该命令在标准输出上生成凭据协议。这在将凭据传递到容器时非常有用。可以通过以感叹号开头来指定此类 shell 命令。如果您的密码或令牌存储在 `GIT_TOKEN` 中，可以运行以下命令来设置凭据帮助程序：

  ``` bash
  $ git config credential.helper \
  	'!f() { echo username=author; echo "password=$GIT_TOKEN"; };f'
  ```

  

- How do I change the password or token I’ve saved in my credential manager?

- 如何更改保存在凭据管理器中的密码或令牌？

  Usually, if the password or token is invalid, Git will erase it and prompt for a new one. However, there are times when this doesn’t always happen. To change the password or token, you can erase the existing credentials and then Git will prompt for new ones. To erase credentials, use a syntax like the following (substituting your username and the hostname):

  通常，如果密码或令牌无效，Git 将擦除它并提示您输入新的。但是，有时并不总是发生这种情况。要更改密码或令牌，可以擦除现有凭据，然后 Git 将提示您输入新的凭据。要擦除凭据，请使用以下语法（替换您的用户名和主机名）：

  ``` bash
  $ echo url=https://author@git.example.org | git credential reject
  ```

  

- How do I use multiple accounts with the same hosting provider using HTTP?

- 如何在使用 HTTP 时在同一托管提供者上使用多个帐户？

  Usually the easiest way to distinguish between these accounts is to use the username in the URL. For example, if you have the accounts `author` and `committer` on `git.example.org`, you can use the URLs https://author@git.example.org/org1/project1.git and https://committer@git.example.org/org2/project2.git. This way, when you use a credential helper, it will automatically try to look up the correct credentials for your account. If you already have a remote set up, you can change the URL with something like `git remote set-url origin https://author@git.example.org/org1/project1.git` (see [git-remote[1]](../../1/git-remote) for details).

  通常，区分这些帐户的最简单方法是在 URL 中使用用户名。例如，如果您在 `git.example.org` 上拥有 `author` 和 `committer` 帐户，则可以使用以下 URL：https://author@git.example.org/org1/project1.git 和 https://committer@git.example.org/org2/project2.git。这样，当您使用凭据帮助程序时，它会自动尝试查找适用于您帐户的正确凭据。如果您已经设置了一个远程，可以使用类似 `git remote set-url origin https://author@git.example.org/org1/project1.git` 的内容更改 URL（有关详情，请参阅 [git-remote[1]](../../1/git-remote)）。

- How do I use multiple accounts with the same hosting provider using SSH?

- 如何在使用 SSH 时在同一托管提供者上使用多个帐户？

  With most hosting providers that support SSH, a single key pair uniquely identifies a user. Therefore, to use multiple accounts, it’s necessary to create a key pair for each account. If you’re using a reasonably modern OpenSSH version, you can create a new key pair with something like `ssh-keygen -t ed25519 -f ~/.ssh/id_committer`. You can then register the public key (in this case, `~/.ssh/id_committer.pub`; note the `.pub`) with the hosting provider.Most hosting providers use a single SSH account for pushing; that is, all users push to the `git` account (e.g., `git@git.example.org`). If that’s the case for your provider, you can set up multiple aliases in SSH to make it clear which key pair to use. For example, you could write something like the following in `~/.ssh/config`, substituting the proper private key file:

  对于大多数支持 SSH 的托管提供者，单个密钥对唯一标识一个用户。因此，要使用多个帐户，需要为每个帐户创建一个密钥对。如果您使用的是相当现代的 OpenSSH 版本，可以使用类似 `ssh-keygen -t ed25519 -f ~/.ssh/id_committer` 的命令创建新的密钥对。然后，可以将公钥（在本例中为 `~/.ssh/id_committer.pub`，请注意 `.pub` 后缀）注册到托管提供者中。大多数托管提供者将使用一个 SSH 帐户进行推送；也就是说，所有用户都会推送到 `git` 帐户（例如，`git@git.example.org`）。如果您的提供者是这样的情况，您可以在 SSH 中设置多个别名，以明确使用哪个密钥对。例如，可以在 `~/.ssh/config` 中写入类似以下内容，替换正确的私钥文件：

  ```
  # This is the account for author on git.example.org.
  # 这是 git.example.org 上作者的帐户。
  Host example_author
  	HostName git.example.org
  	User git
  	# This is the key pair registered for author with git.example.org.
  	# 这是为 git.example.org 上作者注册的密钥对。
  	IdentityFile ~/.ssh/id_author
  	IdentitiesOnly yes
  # This is the account for committer on git.example.org.
  # 这是 git.example.org 上提交者的帐户。
  Host example_committer
  	HostName git.example.org
  	User git
  	# This is the key pair registered for committer with git.example.org.
  	# 这是为 git.example.org 上提交者注册的密钥对。
  	IdentityFile ~/.ssh/id_committer
  	IdentitiesOnly yes
  ```

  Then, you can adjust your push URL to use `git@example_author` or `git@example_committer` instead of `git@example.org` (e.g., `git remote set-url git@example_author:org1/project1.git`).

  然后，您可以调整推送 URL，使用 `git@example_author` 或 `git@example_committer` 替代 `git@example.org`（例如，`git remote set-url git@example_author:org1/project1.git`）。

## 常见问题

- I’ve made a mistake in the last commit. How do I change it?

- 我在最后一次提交中犯了一个错误。我该如何更改？

  You can make the appropriate change to your working tree, run `git add <file>` or `git rm <file>`, as appropriate, to stage it, and then `git commit --amend`. Your change will be included in the commit, and you’ll be prompted to edit the commit message again; if you wish to use the original message verbatim, you can use the `--no-edit` option to `git commit` in addition, or just save and quit when your editor opens.

  您可以对工作树进行适当的更改，然后运行 `git add <file>` 或 `git rm <file>`，视情况选择将更改暂存，然后运行 `git commit --amend`。您的更改将包含在提交中，并且系统将再次提示您编辑提交消息；如果您希望原始消息原封不动地使用，则可以在 `git commit` 后加上 `--no-edit` 选项，或者在编辑器打开时直接保存并退出。

- I’ve made a change with a bug and it’s been included in the main branch. How should I undo it?

- 我做了一个有错误的更改，并且已将其包含在主分支中。我应该如何撤消它？

  The usual way to deal with this is to use `git revert`. This preserves the history that the original change was made and was a valuable contribution, but also introduces a new commit that undoes those changes because the original had a problem. The commit message of the revert indicates the commit which was reverted and is usually edited to include an explanation as to why the revert was made.

  处理此问题的通常方法是使用 `git revert`。这将保留原始更改的历史记录，说明它是有价值的贡献，同时引入一个新的提交，撤消原始更改，因为原始更改有问题。还原的提交消息会指示被还原的提交，并通常编辑以包含撤消原因的解释。

- How do I ignore changes to a tracked file?

- 如何忽略对已跟踪文件的更改？

  Git doesn’t provide a way to do this. The reason is that if Git needs to overwrite this file, such as during a checkout, it doesn’t know whether the changes to the file are precious and should be kept, or whether they are irrelevant and can safely be destroyed. Therefore, it has to take the safe route and always preserve them.It’s tempting to try to use certain features of `git update-index`, namely the assume-unchanged and skip-worktree bits, but these don’t work properly for this purpose and shouldn’t be used this way.If your goal is to modify a configuration file, it can often be helpful to have a file checked into the repository which is a template or set of defaults which can then be copied alongside and modified as appropriate. This second, modified file is usually ignored to prevent accidentally committing it.

  Git 不提供直接的方法来实现此目的。原因是如果 Git 需要覆盖此文件，例如在检出时，它无法确定文件的更改是重要的还是可以安全地删除的。因此，它必须采取安全的方式并始终保留它们。虽然可能尝试使用 `git update-index` 的某些功能，例如 assume-unchanged 和 skip-worktree 标志，但这些对于此目的并不正常工作，不应该以此方式使用。如果您的目标是修改配置文件，通常有用的方法是将一个文件检入仓库作为模板或默认设置，然后将其复制并根据需要进行修改。通常会忽略第二个修改后的文件，以防止意外提交它。

- I asked Git to ignore various files, yet they are still tracked

- 我要求Git忽略各种文件，但它们仍然被追踪

  A `gitignore` file ensures that certain file(s) which are not tracked by Git remain untracked. However, sometimes particular file(s) may have been tracked before adding them into the `.gitignore`, hence they still remain tracked. To untrack and ignore files/patterns, use `git rm --cached <file/pattern>` and add a pattern to `.gitignore` that matches the <file>. See [gitignore[5]](../../5/gitignore) for details.

  `gitignore` 文件确保 Git 不追踪某些未被 Git 跟踪的文件。然而，有时特定的文件在添加到 `.gitignore` 前可能已经被追踪，因此它们仍然被跟踪。要取消跟踪并忽略文件/模式，使用 `git rm --cached <file/pattern>` 并将一个模式添加到 `.gitignore` 中以匹配 `<file>`。详细信息请参阅 [gitignore[5]](../../5/gitignore)。

- How do I know if I want to do a fetch or a pull?

- 我如何知道我是要执行 fetch 还是 pull？

  A fetch stores a copy of the latest changes from the remote repository, without modifying the working tree or current branch. You can then at your leisure inspect, merge, rebase on top of, or ignore the upstream changes. A pull consists of a fetch followed immediately by either a merge or rebase. See [git-pull[1]](../../1/git-pull).

  `fetch` 存储了来自远程仓库的最新更改的副本，而不修改工作树或当前分支。然后，您可以在适当的时候检查、合并、重新基于或忽略上游更改。`pull` 包含一个 fetch，紧接着是合并或变基。更多信息请参阅 [git-pull[1]](../../1/git-pull)。

## 合并和变基

- What kinds of problems can occur when merging long-lived branches with squash merges?

- 使用 squash merge 将长期存在的分支合并时可能会出现什么问题？

  In general, there are a variety of problems that can occur when using squash merges to merge two branches multiple times. These can include seeing extra commits in `git log` output, with a GUI, or when using the `...` notation to express a range, as well as the possibility of needing to re-resolve conflicts again and again.When Git does a normal merge between two branches, it considers exactly three points: the two branches and a third commit, called the *merge base*, which is usually the common ancestor of the commits. The result of the merge is the sum of the changes between the merge base and each head. When you merge two branches with a regular merge commit, this results in a new commit which will end up as a merge base when they’re merged again, because there is now a new common ancestor. Git doesn’t have to consider changes that occurred before the merge base, so you don’t have to re-resolve any conflicts you resolved before.When you perform a squash merge, a merge commit isn’t created; instead, the changes from one side are applied as a regular commit to the other side. This means that the merge base for these branches won’t have changed, and so when Git goes to perform its next merge, it considers all of the changes that it considered the last time plus the new changes. That means any conflicts may need to be re-resolved. Similarly, anything using the `...` notation in `git diff`, `git log`, or a GUI will result in showing all of the changes since the original merge base.As a consequence, if you want to merge two long-lived branches repeatedly, it’s best to always use a regular merge commit.

  通常，使用 squash merge 多次合并两个分支时可能会出现各种问题。这些问题可能包括在 `git log` 输出、GUI 或使用 `...` 表示范围时看到额外的提交，以及可能需要一次又一次重新解决冲突。当 Git 在两个分支之间进行正常合并时，它会考虑三个点：这两个分支和第三个提交，称为 *合并基础*，通常是这些提交的共同祖先。合并的结果是合并基础和每个头之间的更改总和。当您使用常规合并提交合并两个分支时，结果是生成一个新的提交，当它们再次合并时，该提交将作为合并基础，因为现在有了一个新的共同祖先。Git 不必考虑在合并基础之前发生的更改，因此您不必重新解决之前解决的任何冲突。当您执行 squash merge 时，并不会创建合并提交；相反，它将一个分支的更改作为常规提交应用到另一个分支。这意味着这些分支的合并基础不会更改，因此当 Git 进行下一次合并时，它会考虑上次考虑的所有更改以及新的更改。这意味着任何冲突可能需要重新解决。类似地，使用 `git diff`、`git log` 或 GUI 中的 `...` 表示范围时，将显示自原始合并基础以来的所有更改。因此，如果您想要多次合并两个长期存在的分支，最好始终使用常规合并提交。

- If I make a change on two branches but revert it on one, why does the merge of those branches include the change?

- 如果我在两个分支上都做了更改，但在其中一个分支上对其进行了还原，为什么合并这些分支会包括这个更改？

  By default, when Git does a merge, it uses a strategy called the `ort` strategy, which does a fancy three-way merge. In such a case, when Git performs the merge, it considers exactly three points: the two heads and a third point, called the *merge base*, which is usually the common ancestor of those commits. Git does not consider the history or the individual commits that have happened on those branches at all.As a result, if both sides have a change and one side has reverted that change, the result is to include the change. This is because the code has changed on one side and there is no net change on the other, and in this scenario, Git adopts the change.If this is a problem for you, you can do a rebase instead, rebasing the branch with the revert onto the other branch. A rebase in this scenario will revert the change, because a rebase applies each individual commit, including the revert. Note that rebases rewrite history, so you should avoid rebasing published branches unless you’re sure you’re comfortable with that. See the NOTES section in [git-rebase[1]](../../1/git-rebase) for more details.

  默认情况下，当 Git 进行合并时，它使用一种称为 `ort` 策略的高级三路合并策略。在这种情况下，当 Git 执行合并时，它会考虑两个头和第三个点，称为 *合并基础*，通常是这些提交的共同祖先。Git 不考虑这些分支上的历史或个别提交。因此，如果两侧都有更改，并且一侧已经对该更改进行了还原，结果将包含该更改。这是因为代码在一侧发生了更改，而另一侧没有净更改，因此在这种情况下，Git 采用该更改。如果这对您造成了问题，可以使用变基来解决，在这种情况下，将对执行还原的分支变基到另一个分支上。在这种情况下，变基将还原该更改，因为变基会应用每个单独的提交，包括还原。请注意，变基会重写历史记录，因此除非您确定您对此感到舒适，否则应避免对已发布的分支进行变基。详细信息请参阅 [git-rebase[1]](../../1/git-rebase) 中的 NOTES 部分。

## 钩子

- How do I use hooks to prevent users from making certain changes?

- 我如何使用钩子来阻止用户进行某些更改？

  The only safe place to make these changes is on the remote repository (i.e., the Git server), usually in the `pre-receive` hook or in a continuous integration (CI) system. These are the locations in which policy can be enforced effectively.It’s common to try to use `pre-commit` hooks (or, for commit messages, `commit-msg` hooks) to check these things, which is great if you’re working as a solo developer and want the tooling to help you. However, using hooks on a developer machine is not effective as a policy control because a user can bypass these hooks with `--no-verify` without being noticed (among various other ways). Git assumes that the user is in control of their local repositories and doesn’t try to prevent this or tattle on the user.In addition, some advanced users find `pre-commit` hooks to be an impediment to workflows that use temporary commits to stage work in progress or that create fixup commits, so it’s better to push these kinds of checks to the server anyway.

  唯一安全的地方来进行这些更改是在远程仓库（即 Git 服务器）上，通常在 `pre-receive` 钩子或持续集成（CI）系统中。这些是可以有效实施策略的位置。在个人开发人员的机器上尝试使用 `pre-commit` 钩子（或提交消息的 `commit-msg` 钩子）来检查这些问题是常见的做法，如果您作为独立开发人员工作并希望工具帮助您，这是很好的。然而，在开发人员机器上使用钩子作为策略控制并不有效，因为用户可以使用 `--no-verify` 来绕过这些钩子而不被察觉（还有其他各种方法）。Git 假定用户控制其本地仓库，不会阻止此操作或告发用户。另外，一些高级用户发现 `pre-commit` 钩子对于使用临时提交暂存工作进度或创建 fixup 提交的工作流程是一种阻碍，因此最好将这些检查推到服务器上。

## 跨平台问题

- I’m on Windows and my text files are detected as binary.

- 我使用 Windows，但我的文本文件被检测为二进制文件。

  Git works best when you store text files as UTF-8. Many programs on Windows support UTF-8, but some do not and only use the little-endian UTF-16 format, which Git detects as binary. If you can’t use UTF-8 with your programs, you can specify a working tree encoding that indicates which encoding your files should be checked out with, while still storing these files as UTF-8 in the repository. This allows tools like [git-diff[1]](../../1/git-diff) to work as expected, while still allowing your tools to work.To do so, you can specify a [gitattributes[5]](../../5/gitattributes) pattern with the `working-tree-encoding` attribute. For example, the following pattern sets all C files to use UTF-16LE-BOM, which is a common encoding on Windows:

  当您将文本文件存储为 UTF-8 时，Git 的效果最好。Windows 上的许多程序支持 UTF-8，但有些不支持，只使用小端 UTF-16 格式，Git 将其检测为二进制文件。如果您不能在程序中使用 UTF-8，可以指定工作树编码，以指示应使用哪种编码检出文件，同时在存储库中仍以 UTF-8 存储这些文件。这样一来，像 [git-diff[1]](../../1/git-diff) 这样的工具将按预期工作，同时您的工具也可以正常工作。要做到这一点，您可以在 [gitattributes[5]](../../5/gitattributes) 文件中使用 `working-tree-encoding` 属性指定一个模式。例如，以下模式设置所有 C 文件使用 UTF-16LE-BOM，这是 Windows 上常用的编码：

  ```
  *.c	working-tree-encoding=UTF-16LE-BOM
  ```

  You will need to run `git add --renormalize` to have this take effect. Note that if you are making these changes on a project that is used across platforms, you’ll probably want to make it in a per-user configuration file or in the one in `$GIT_DIR/info/attributes`, since making it in a `.gitattributes` file in the repository will apply to all users of the repository.See the following entry for information about normalizing line endings as well, and see [gitattributes[5]](../../5/gitattributes) for more information about attribute files.

  您需要运行 `git add --renormalize` 来使其生效。请注意，如果您在跨平台使用的项目中进行这些更改，您可能希望在每个用户的配置文件或 `$GIT_DIR/info/attributes` 中进行更改，因为在存储库的 `.gitattributes` 文件中进行更改会应用于存储库的所有用户。有关规范化行结束符的信息，请参阅下面的条目，有关属性文件的更多信息，请参阅 [gitattributes[5]](../../5/gitattributes)。

- I’m on Windows and git diff shows my files as having a `^M` at the end.

- 我使用 Windows，git diff 显示文件末尾有一个 `^M`。

  By default, Git expects files to be stored with Unix line endings. As such, the carriage return (`^M`) that is part of a Windows line ending is shown because it is considered to be trailing whitespace. Git defaults to showing trailing whitespace only on new lines, not existing ones.You can store the files in the repository with Unix line endings and convert them automatically to your platform’s line endings. To do that, set the configuration option `core.eol` to `native` and see the following entry for information about how to configure files as text or binary.You can also control this behavior with the `core.whitespace` setting if you don’t wish to remove the carriage returns from your line endings.

  默认情况下，Git 期望文件以 Unix 行结束符存储。因此，Windows 行结束符的一部分 `^M` 被显示，因为它被认为是尾随空格。Git 默认只显示新行上的尾随空格，而不显示现有行上的尾随空格。您可以将文件存储在存储库中时使用 Unix 行结束符，并自动将其转换为您平台的行结束符。为此，将配置选项 `core.eol` 设置为 `native`，有关如何将文件配置为文本或二进制的信息，请参阅下面的条目。如果您不希望从行结束符中删除回车符，还可以通过 `core.whitespace` 设置来控制此行为。

- Why do I have a file that’s always modified?

- 为什么我的一个文件一直显示为已修改？

  Internally, Git always stores file names as sequences of bytes and doesn’t perform any encoding or case folding. However, Windows and macOS by default both perform case folding on file names. As a result, it’s possible to end up with multiple files or directories whose names differ only in case. Git can handle this just fine, but the file system can store only one of these files, so when Git reads the other file to see its contents, it looks modified.It’s best to remove one of the files such that you only have one file. You can do this with commands like the following (assuming two files `AFile.txt` and `afile.txt`) on an otherwise clean working tree:

  在内部，Git 总是将文件名存储为字节序列，不执行任何编码或大小写转换。然而，Windows 和 macOS 默认情况下都对文件名进行大小写折叠。因此，可能会出现多个文件或目录，它们的名称只在大小写方面有所不同。Git 可以很好地处理这一点，但文件系统只能存储其中一个文件，因此当 Git 读取其他文件以查看其内容时，它会显示为已修改。最好的方法是删除其中一个文件，以便您只有一个文件。您可以使用以下命令（假设两个文件名分别为 `AFile.txt` 和 `afile.txt`）在一个干净的工作树上执行此操作：

  ``` bash
  $ git rm --cached AFile.txt
  $ git commit -m 'Remove files conflicting in case'
  $ git checkout .
  ```

  This avoids touching the disk, but removes the additional file. Your project may prefer to adopt a naming convention, such as all-lowercase names, to avoid this problem from occurring again; such a convention can be checked using a `pre-receive` hook or as part of a continuous integration (CI) system.It is also possible for perpetually modified files to occur on any platform if a smudge or clean filter is in use on your system but a file was previously committed without running the smudge or clean filter. To fix this, run the following on an otherwise clean working tree:

  这样可以避免触碰磁盘，同时删除了额外的文件。您的项目可能希望采用命名约定，例如全部使用小写名称，以避免再次发生此问题；可以使用 `pre-receive` 钩子或作为持续集成（CI）系统的一部分检查这种约定。如果在您的系统上使用了 smudge 或 clean 过滤器，但是在运行 smudge 或 clean 过滤器之前先前提交了一个文件，则在任何平台上都可能发生永久性修改的文件问题。要修复这个问题，在一个干净的工作树上运行以下命令：

  ``` bash
  $ git add --renormalize .
  ```

  

- What’s the recommended way to store files in Git?

- 存储文件在 Git 中的推荐方法是什么？

  While Git can store and handle any file of any type, there are some settings that work better than others. In general, we recommend that text files be stored in UTF-8 without a byte-order mark (BOM) with LF (Unix-style) endings. We also recommend the use of UTF-8 (again, without BOM) in commit messages. These are the settings that work best across platforms and with tools such as `git diff` and `git merge`.Additionally, if you have a choice between storage formats that are text based or non-text based, we recommend storing files in the text format and, if necessary, transforming them into the other format. For example, a text-based SQL dump with one record per line will work much better for diffing and merging than an actual database file. Similarly, text-based formats such as Markdown and AsciiDoc will work better than binary formats such as Microsoft Word and PDF.Similarly, storing binary dependencies (e.g., shared libraries or JAR files) or build products in the repository is generally not recommended. Dependencies and build products are best stored on an artifact or package server with only references, URLs, and hashes stored in the repository.We also recommend setting a [gitattributes[5]](../../5/gitattributes) file to explicitly mark which files are text and which are binary. If you want Git to guess, you can set the attribute `text=auto`. For example, the following might be appropriate in some projects:

  虽然 Git 可以存储和处理任何类型的文件，但某些设置效果比其他设置更好。一般来说，我们建议将文本文件存储为 UTF-8，没有字节顺序标记 (BOM)，并使用 LF（Unix 风格）换行符。我们还建议在提交消息中使用 UTF-8（同样没有 BOM）。这些设置在各个平台和工具（如 `git diff` 和 `git merge`）上效果最好。另外，如果您可以选择使用基于文本或非文本的存储格式，我们建议将文件存储为文本格式，并在必要时将其转换为其他格式。例如，基于文本的 SQL 转储，每行一个记录，比实际数据库文件更适合进行差异和合并。类似地，文本格式，如 Markdown 和 AsciiDoc，比二进制格式，如 Microsoft Word 和 PDF，效果更好。类似地，通常不建议将二进制依赖项（如共享库或 JAR 文件）或构建产物存储在存储库中。依赖项和构建产物最好存储在工件或包服务器上，而在存储库中只存储引用、URL 和哈希值。我们还建议设置一个 [gitattributes[5]](../../5/gitattributes) 文件，明确标记哪些文件是文本，哪些是二进制。如果希望 Git 猜测，可以设置属性 `text=auto`。例如，在某些项目中可能适用以下设置：

  ```
  # By default, guess.
  # 默认情况下，进行猜测。
  *	text=auto
  # Mark all C files as text.
  # 将所有 C 文件标记为文本。
  *.c	text
  # Mark all JPEG files as binary.
  # 将所有 JPEG 文件标记为二进制。
  *.jpg	binary
  ```

  These settings help tools pick the right format for output such as patches and result in files being checked out in the appropriate line ending for the platform.

  这些设置有助于工具选择适当的格式，用于输出如补丁，并导致文件在适当的平台换行符下检出。





## GIT

  这是[git[1]](../../Git)工具集中的一部分。