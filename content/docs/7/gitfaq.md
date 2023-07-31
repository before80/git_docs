+++
title = "gitfaq"
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

gitfaq - Git 使用常见问题解答

## 概要

gitfaq

## 描述

​	本常见问题解答中的示例假定使用标准 POSIX shell，如`bash`或`dash`，以及用户 A U Thor，在托管提供者 `git.example.org` 上拥有帐户 `author`。

## 配置

- `user.name` 应该放什么内容？

  您应该输入您的个人名称，通常是给定名和姓氏的形式。例如，Git 的当前维护者使用 "Junio C Hamano"。这将是存储在您进行每次提交时的名称部分。该配置对于身份验证到远程服务没有任何影响；有关该方面的信息，请参阅 [git-config[1]](../../1/git-config) 中的 `credential.username`。

- `http.postBuffer` 究竟是做什么的？

  此选项更改 Git 在通过 HTTP 或 HTTPS 推送数据到远程时使用的缓冲区大小。如果数据大于此大小，负责 Git 的 HTTP 支持的 libcurl 将使用分块传输编码（chunked transfer encoding），因为在推送数据大小之前无法预先知道。除非您知道远程服务器或中间代理不支持 HTTP/1.1（其中引入了分块传输编码）或已知会在处理分块数据时出现问题，否则将保留此值为默认大小是可以的。通常（错误地）建议将此值提高作为解决通用推送问题的方法，但由于几乎每个服务器和代理至少支持 HTTP/1.1，提高此值通常并不能解决大多数推送问题。在今天的互联网上，不正确支持 HTTP/1.1 和分块传输编码的服务器或代理并不实用，因为它会破坏大量的流量。请注意，增加此值将增加 Git 通过 HTTP 或 HTTPS 进行的每次相关推送使用的内存，因为无论是否使用整个缓冲区，它都会被全部分配。因此，除非您确定需要不同的值，最好将其保留为默认值。

- 如何配置不同的编辑器？

  如果您尚未为 Git 指定特定的编辑器，默认情况下它将使用通过 `VISUAL` 或 `EDITOR` 环境变量配置的编辑器，或者如果两者都未指定，则使用系统默认编辑器（通常是 `vi`）。由于有些人发现 `vi` 难以使用或更喜欢其他编辑器，因此可能希望更改所使用的编辑器。如果要为大多数需要编辑器的程序配置通用编辑器，可以编辑您的 shell 配置文件（例如 `~/.bashrc` 或 `~/.zshenv`），在其中添加一行设置 `EDITOR` 或 `VISUAL` 环境变量为适当的值。例如，如果您喜欢编辑器 `nano`，可以写入以下内容：

  ```
  export VISUAL=nano
  ```

  如果要为 Git 特定地配置编辑器，可以设置 `core.editor` 配置值或 `GIT_EDITOR` 环境变量。有关这些选项的咨询顺序的详细信息，请参阅 [git-var[1]](../../1/git-var)。请注意，在所有情况下，编辑器值都将传递给 shell，因此任何包含空格的参数都应适当引用。此外，如果您的编辑器通常在调用时与终端断开连接，则应该指定一个不会这样做的参数，否则 Git 将无法检测到任何更改。在 Windows 上解决这两个问题的配置示例是 `"C:\Program Files\Vim\gvim.exe" --nofork`，它使用带有空格的文件名并指定了 `--nofork` 选项以避免将进程转为后台运行。

## 凭据

- 在通过 HTTP 推送时，如何指定我的凭据？

  最简单的方法是通过 `credential.helper` 配置使用凭据帮助程序。大多数系统提供了与系统凭据管理器集成的标准选择。例如，Git for Windows 提供了 `wincred` 凭据管理器，macOS 使用 `osxkeychain` 凭据管理器，而带有标准桌面环境的 Unix 系统可以使用 `libsecret` 凭据管理器。所有这些凭据管理器都会将凭据存储在加密存储中，以保护您的密码或令牌的安全性。此外，您还可以使用 `store` 凭据管理器，它会将凭据存储在主目录中的文件中，或者使用 `cache` 凭据管理器，它不会永久存储您的凭据，但会在一定时间内避免提示您输入凭据。您还可以在提示时输入密码。虽然可以将密码（必须进行百分比编码）放在 URL 中，但这样并不特别安全，并且可能导致凭据意外暴露，因此不推荐这样做。

- 如何从环境变量中读取密码或令牌？

  `credential.helper` 配置选项还可以接受一个任意的 shell 命令，该命令在标准输出上生成凭据协议。这在将凭据传递到容器时非常有用。可以通过以感叹号开头来指定此类 shell 命令。如果您的密码或令牌存储在 `GIT_TOKEN` 中，可以运行以下命令来设置凭据帮助程序：

  ``` bash
  $ git config credential.helper \
  	'!f() { echo username=author; echo "password=$GIT_TOKEN"; };f'
  ```

  

- 如何更改保存在凭据管理器中的密码或令牌？

  通常，如果密码或令牌无效，Git 将擦除它并提示您输入新的。但是，有时并不总是发生这种情况。要更改密码或令牌，可以擦除现有凭据，然后 Git 将提示您输入新的凭据。要擦除凭据，请使用以下语法（替换您的用户名和主机名）：

  ``` bash
  $ echo url=https://author@git.example.org | git credential reject
  ```

  

- 如何在使用 HTTP 时在同一托管提供者上使用多个帐户？

  通常，区分这些帐户的最简单方法是在 URL 中使用用户名。例如，如果您在 `git.example.org` 上拥有 `author` 和 `committer` 帐户，则可以使用以下 URL：https://author@git.example.org/org1/project1.git 和 https://committer@git.example.org/org2/project2.git。这样，当您使用凭据帮助程序时，它会自动尝试查找适用于您帐户的正确凭据。如果您已经设置了一个远程，可以使用类似 `git remote set-url origin https://author@git.example.org/org1/project1.git` 的内容更改 URL（有关详情，请参阅 [git-remote[1]](../../1/git-remote)）。

- 如何在使用 SSH 时在同一托管提供者上使用多个帐户？

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

  然后，您可以调整推送 URL，使用 `git@example_author` 或 `git@example_committer` 替代 `git@example.org`（例如，`git remote set-url git@example_author:org1/project1.git`）。

## 常见问题

- 我在最后一次提交中犯了一个错误。我该如何更改？

  您可以对工作树进行适当的更改，然后运行 `git add <file>` 或 `git rm <file>`，视情况选择将更改暂存，然后运行 `git commit --amend`。您的更改将包含在提交中，并且系统将再次提示您编辑提交消息；如果您希望原始消息原封不动地使用，则可以在 `git commit` 后加上 `--no-edit` 选项，或者在编辑器打开时直接保存并退出。

- 我做了一个有错误的更改，并且已将其包含在主分支中。我应该如何撤消它？

  处理此问题的通常方法是使用 `git revert`。这将保留原始更改的历史记录，说明它是有价值的贡献，同时引入一个新的提交，撤消原始更改，因为原始更改有问题。还原的提交消息会指示被还原的提交，并通常编辑以包含撤消原因的解释。

- 如何忽略对已跟踪文件的更改？

  Git 不提供直接的方法来实现此目的。原因是如果 Git 需要覆盖此文件，例如在检出时，它无法确定文件的更改是重要的还是可以安全地删除的。因此，它必须采取安全的方式并始终保留它们。虽然可能尝试使用 `git update-index` 的某些功能，例如 assume-unchanged 和 skip-worktree 标志，但这些对于此目的并不正常工作，不应该以此方式使用。如果您的目标是修改配置文件，通常有用的方法是将一个文件检入仓库作为模板或默认设置，然后将其复制并根据需要进行修改。通常会忽略第二个修改后的文件，以防止意外提交它。

- 我要求Git忽略各种文件，但它们仍然被追踪

  `gitignore` 文件确保 Git 不追踪某些未被 Git 跟踪的文件。然而，有时特定的文件在添加到 `.gitignore` 前可能已经被追踪，因此它们仍然被跟踪。要取消跟踪并忽略文件/模式，使用 `git rm --cached <file/pattern>` 并将一个模式添加到 `.gitignore` 中以匹配 `<file>`。详细信息请参阅 [gitignore[5]](../../5/gitignore)。

- 我如何知道我是要执行 fetch 还是 pull？

  `fetch` 存储了来自远程仓库的最新更改的副本，而不修改工作树或当前分支。然后，您可以在适当的时候检查、合并、重新基于或忽略上游更改。`pull` 包含一个 fetch，紧接着是合并或变基。更多信息请参阅 [git-pull[1]](../../1/git-pull)。

## 合并和变基

- 使用 squash merge 将长期存在的分支合并时可能会出现什么问题？

  通常，使用 squash merge 多次合并两个分支时可能会出现各种问题。这些问题可能包括在 `git log` 输出、GUI 或使用 `...` 表示范围时看到额外的提交，以及可能需要一次又一次重新解决冲突。当 Git 在两个分支之间进行正常合并时，它会考虑三个点：这两个分支和第三个提交，称为 *合并基础*，通常是这些提交的共同祖先。合并的结果是合并基础和每个头之间的更改总和。当您使用常规合并提交合并两个分支时，结果是生成一个新的提交，当它们再次合并时，该提交将作为合并基础，因为现在有了一个新的共同祖先。Git 不必考虑在合并基础之前发生的更改，因此您不必重新解决之前解决的任何冲突。当您执行 squash merge 时，并不会创建合并提交；相反，它将一个分支的更改作为常规提交应用到另一个分支。这意味着这些分支的合并基础不会更改，因此当 Git 进行下一次合并时，它会考虑上次考虑的所有更改以及新的更改。这意味着任何冲突可能需要重新解决。类似地，使用 `git diff`、`git log` 或 GUI 中的 `...` 表示范围时，将显示自原始合并基础以来的所有更改。因此，如果您想要多次合并两个长期存在的分支，最好始终使用常规合并提交。

- 如果我在两个分支上都做了更改，但在其中一个分支上对其进行了还原，为什么合并这些分支会包括这个更改？

  默认情况下，当 Git 进行合并时，它使用一种称为 `ort` 策略的高级三路合并策略。在这种情况下，当 Git 执行合并时，它会考虑两个头和第三个点，称为 *合并基础*，通常是这些提交的共同祖先。Git 不考虑这些分支上的历史或个别提交。因此，如果两侧都有更改，并且一侧已经对该更改进行了还原，结果将包含该更改。这是因为代码在一侧发生了更改，而另一侧没有净更改，因此在这种情况下，Git 采用该更改。如果这对您造成了问题，可以使用变基来解决，在这种情况下，将对执行还原的分支变基到另一个分支上。在这种情况下，变基将还原该更改，因为变基会应用每个单独的提交，包括还原。请注意，变基会重写历史记录，因此除非您确定您对此感到舒适，否则应避免对已发布的分支进行变基。详细信息请参阅 [git-rebase[1]](../../1/git-rebase) 中的 NOTES 部分。

## 钩子

- 我如何使用钩子来阻止用户进行某些更改？

  唯一安全的地方来进行这些更改是在远程仓库（即 Git 服务器）上，通常在 `pre-receive` 钩子或持续集成（CI）系统中。这些是可以有效实施策略的位置。在个人开发人员的机器上尝试使用 `pre-commit` 钩子（或提交消息的 `commit-msg` 钩子）来检查这些问题是常见的做法，如果您作为独立开发人员工作并希望工具帮助您，这是很好的。然而，在开发人员机器上使用钩子作为策略控制并不有效，因为用户可以使用 `--no-verify` 来绕过这些钩子而不被察觉（还有其他各种方法）。Git 假定用户控制其本地仓库，不会阻止此操作或告发用户。另外，一些高级用户发现 `pre-commit` 钩子对于使用临时提交暂存工作进度或创建 fixup 提交的工作流程是一种阻碍，因此最好将这些检查推到服务器上。

## 跨平台问题

- 我使用 Windows，但我的文本文件被检测为二进制文件。

  当您将文本文件存储为 UTF-8 时，Git 的效果最好。Windows 上的许多程序支持 UTF-8，但有些不支持，只使用小端 UTF-16 格式，Git 将其检测为二进制文件。如果您不能在程序中使用 UTF-8，可以指定工作树编码，以指示应使用哪种编码检出文件，同时在存储库中仍以 UTF-8 存储这些文件。这样一来，像 [git-diff[1]](../../1/git-diff) 这样的工具将按预期工作，同时您的工具也可以正常工作。要做到这一点，您可以在 [gitattributes[5]](../../5/gitattributes) 文件中使用 `working-tree-encoding` 属性指定一个模式。例如，以下模式设置所有 C 文件使用 UTF-16LE-BOM，这是 Windows 上常用的编码：

  ```
  *.c	working-tree-encoding=UTF-16LE-BOM
  ```

  您需要运行 `git add --renormalize` 来使其生效。请注意，如果您在跨平台使用的项目中进行这些更改，您可能希望在每个用户的配置文件或 `$GIT_DIR/info/attributes` 中进行更改，因为在存储库的 `.gitattributes` 文件中进行更改会应用于存储库的所有用户。有关规范化行结束符的信息，请参阅下面的条目，有关属性文件的更多信息，请参阅 [gitattributes[5]](../../5/gitattributes)。

- 我使用 Windows，git diff 显示文件末尾有一个 `^M`。

  默认情况下，Git 期望文件以 Unix 行结束符存储。因此，Windows 行结束符的一部分 `^M` 被显示，因为它被认为是尾随空格。Git 默认只显示新行上的尾随空格，而不显示现有行上的尾随空格。您可以将文件存储在存储库中时使用 Unix 行结束符，并自动将其转换为您平台的行结束符。为此，将配置选项 `core.eol` 设置为 `native`，有关如何将文件配置为文本或二进制的信息，请参阅下面的条目。如果您不希望从行结束符中删除回车符，还可以通过 `core.whitespace` 设置来控制此行为。

- 为什么我的一个文件一直显示为已修改？

  在内部，Git 总是将文件名存储为字节序列，不执行任何编码或大小写转换。然而，Windows 和 macOS 默认情况下都对文件名进行大小写折叠。因此，可能会出现多个文件或目录，它们的名称只在大小写方面有所不同。Git 可以很好地处理这一点，但文件系统只能存储其中一个文件，因此当 Git 读取其他文件以查看其内容时，它会显示为已修改。最好的方法是删除其中一个文件，以便您只有一个文件。您可以使用以下命令（假设两个文件名分别为 `AFile.txt` 和 `afile.txt`）在一个干净的工作树上执行此操作：

  ``` bash
  $ git rm --cached AFile.txt
  $ git commit -m 'Remove files conflicting in case'
  $ git checkout .
  ```

  这样可以避免触碰磁盘，同时删除了额外的文件。您的项目可能希望采用命名约定，例如全部使用小写名称，以避免再次发生此问题；可以使用 `pre-receive` 钩子或作为持续集成（CI）系统的一部分检查这种约定。如果在您的系统上使用了 smudge 或 clean 过滤器，但是在运行 smudge 或 clean 过滤器之前先前提交了一个文件，则在任何平台上都可能发生永久性修改的文件问题。要修复这个问题，在一个干净的工作树上运行以下命令：

  ``` bash
  $ git add --renormalize .
  ```

  

- 存储文件在 Git 中的推荐方法是什么？

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

  这些设置有助于工具选择适当的格式，用于输出如补丁，并导致文件在适当的平台换行符下检出。





## GIT

  这是[git[1]](../../Git)工具集中的一部分。