+++
title = "gitcredentials"
weight = 30
type = "docs"
date = 2023-07-30T15:39:23+08:00
description = ""
isCJKLanguage = true
draft = false
+++

# gitcredentials 

https://git-scm.com/docs/gitcredentials

version 2.41.0

## 名称

​	gitcredentials - 向 Git 提供用户名和密码

## 概要

```
git config credential.https://example.com.username myusername
git config credential.helper "$helper $options"
```

## 描述

​	有时候，Git 需要用户提供凭据以执行操作；例如，它可能需要要求用户名和密码以便通过 HTTP 访问远程存储库。有些远程存储库可以接受个人访问令牌或 OAuth 访问令牌作为密码。本手册描述了 Git 请求这些凭据的机制，以及一些避免重复输入这些凭据的功能。

## 请求凭据

​	在未定义任何凭据助手的情况下，Git 将尝试以下策略来向用户请求用户名和密码：

1. 如果设置了 `GIT_ASKPASS` 环境变量，则调用该变量指定的程序。程序在命令行上提供适当的提示，并从其标准输出读取用户的输入。
2. 否则，如果设置了 `core.askPass` 配置变量，则使用其值，与上述情况类似。
3. 否则，如果设置了 `SSH_ASKPASS` 环境变量，则使用其值，与上述情况类似。
4. 否则，在终端上提示用户输入。

## 避免重复输入

​	反复输入相同的凭据可能很繁琐。Git 提供了两种方法来减少这种麻烦：

1. 静态配置给定认证上下文的用户名。
2. 凭据助手来缓存或存储密码，或与系统密码库或密钥链交互。

​	第一种方法简单，并且适用于没有可用于密码的安全存储的情况。通常可以通过在配置文件中添加以下内容来进行配置：

```
[credential "https://example.com"]
	username = me
```

​	凭据助手则是外部程序，Git 可以向其请求用户名和密码；它们通常与操作系统或其他程序提供的安全存储进行交互。或者，凭据生成助手可以通过某些 API 为某些服务器生成凭据。

​	要使用助手，首先必须选择一个要使用的助手。Git 当前包括以下几个助手：

- cache

  在内存中缓存凭据一小段时间。详见 [git-credential-cache[1]](../../1/git-credential-cache) 获取详细信息。

- store

  永久地将凭据存储在磁盘上。详见 [git-credential-store[1]](../../1/git-credential-store) 获取详细信息。

​	你也可能安装了第三方的助手；在 `git help -a` 的输出中搜索 `credential-*`，并查阅各个助手的文档。选择了一个助手后，可以通过将其名称放入 `credential.helper` 变量中来告诉 Git 使用它。

1. 找到一个助手。

   ```
   $ git help -a | grep credential-
   credential-foo
   ```

2. 阅读其描述。

   ```
   $ git help credential-foo
   ```

3. 告诉 Git 使用它。

   ```
   $ git config --global credential.helper foo
   ```

## 凭据上下文

​	Git 视每个凭据为由 URL 定义的上下文。此上下文用于查找上下文特定的配置，并传递给任何助手，助手可能将其用作安全存储的索引。

​	例如，假设我们正在访问 `https://example.com/foo.git`。当 Git 查看配置文件以查看是否有与此上下文匹配的部分时，如果上下文是配置文件中模式的更具体的子集，则它们将被视为匹配。例如，如果在配置文件中有以下内容：

```
[credential "https://example.com"]
	username = foo
```

那么我们将匹配：协议都相同，主机都相同，而"模式" URL 对路径组件不关心。然而，这个上下文将不会匹配：

```
[credential "https://kernel.org"]
	username = foo
```

因为主机名不同。或者 `foo.example.com` 也不匹配；Git 对主机名进行完全比较，而不考虑两个主机是否属于同一域。同样，对于 `http://example.com` 的配置条目也不匹配：Git 对协议进行完全比较。但是，你可以在域名中使用通配符和其他模式匹配技术，就像 `http.<URL>.*` 选项一样。

​	如果"模式" URL 中包含路径组件，则它也必须完全匹配：上下文 `https://example.com/bar/baz.git` 将匹配配置条目 `https://example.com/bar/baz.git`（除了匹配 `https://example.com` 的配置条目之外），但不会匹配配置条目 `https://example.com/bar`。

## 配置选项

​	凭据上下文的选项可以在 `credential.*`（适用于所有凭据）或 `credential.<URL>.*` 中配置，其中 `<URL>` 与上述描述的上下文匹配。

​	以下选项在这两个位置上都可以配置：

- helper

  外部凭据助手的名称，以及任何相关选项。如果助手名称不是绝对路径，则在前面添加字符串 `git credential-`。结果字符串由 Shell 执行（因此，例如，将此设置为 `foo --option=bar` 将通过 Shell 执行 `git credential-foo --option=bar`）。有关其用法示例，请参阅特定助手的手册。如果有多个 `credential.helper` 配置变量的实例，则将依次尝试每个助手，并可能提供用户名、密码或空。一旦 Git 获取到用户名和未过期的密码，就不会再尝试更多的助手。如果将 `credential.helper` 配置为空字符串，则会将助手列表重置为空（因此，可以通过配置空字符串助手，然后是所需的助手集来覆盖较低优先级配置文件设置的助手）。

- username

  默认的用户名，如果在 URL 中没有提供。

- useHttpPath

  默认情况下，Git 不会通过外部助手来匹配 http URL 的 "path" 组件。这意味着为 `https://example.com/foo.git` 存储的凭据也将用于 `https://example.com/bar.git`。如果确实希望区分这些情况，请将此选项设置为 `true`。

## 自定义助手

​	您可以编写自己的自定义助手来与任何保存凭据的系统进行交互。

​	凭据助手是由 Git 执行的程序，用于从长期存储中获取或保存凭据（"长期" 意味着超过一个 Git 进程；例如，凭据可以在内存中存储几分钟，或永久地存储在磁盘上）。

​	每个助手都由配置变量 `credential.helper`（以及其他变量，详见 [git-config[1]](../../1/git-config)）中的一个字符串指定。Git 将根据以下规则将字符串转换为要执行的命令：

1. 如果助手字符串以 "!" 开头，则被视为 Shell 片段，"!" 之后的部分成为命令。
2. 否则，如果助手字符串以绝对路径开头，则原始助手字符串成为命令。
3. 否则，字符串 "git credential-" 被添加到助手字符串之前，并将结果作为命令。

​	然后，在该命令上附加一个 "operation" 参数（详见下文），并通过 Shell 执行该结果。

​	以下是一些示例配置：

```
# run "git credential-foo"
# 运行 "git credential-foo"
[credential]
	helper = foo

# same as above, but pass an argument to the helper
# 与上面相同，但向助手传递一个参数
[credential]
	helper = "foo --bar=baz"

# the arguments are parsed by the shell, so use shell
# quoting if necessary
# 参数由 Shell 解析，因此如果需要，使用 Shell 引用
[credential]
	helper = "foo --bar='whitespace arg'"

# you can also use an absolute path, which will not use the git wrapper
# 也可以使用绝对路径，这样不会使用 git 封装
[credential]
	helper = "/path/to/my/helper --with-arguments"

# or you can specify your own shell snippet
# 或者可以指定自己的 Shell 片段
[credential "https://example.com"]
	username = your_user
	helper = "!f() { test \"$1\" = get && echo \"password=$(cat $HOME/.secret)\"; }; f"
```

​	一般来说，上述规则（3）对于用户来说是最简单的。凭据助手的作者应该努力帮助用户，将其程序命名为 "git-credential-$NAME"，并在安装过程中将其放置在 `$PATH` 或 `$GIT_EXEC_PATH` 中，这将允许用户使用 `git config credential.helper $NAME` 来启用它。

​	当执行助手时，它的命令行将附加一个 "operation" 参数，该参数是以下之一：

- `get`

  如果存在匹配的凭据，则返回它。

- `store`

  将凭据存储（如果适用于该助手）。

- `erase`

  从助手的存储中删除匹配的凭据（如果有）。

​	凭据的详细信息将在助手的标准输入流上提供。其确切格式与 `git credential` 基础命令的输入/输出格式相同（请参阅 [git-credential[1]](../../1/git-credential) 中的 "INPUT/OUTPUT FORMAT" 部分，获取详细规范）。

​	对于 `get` 操作，助手应该在标准输出上以相同格式生成一系列属性（请参阅 [git-credential[1]](../../1/git-credential) 获取常见属性）。助手可以自由地生成子集，甚至根本不提供任何值，如果没有有用的内容可以提供。提供的任何属性都将覆盖 Git 凭据子系统已知的属性。未被识别的属性会被静默丢弃。

​	虽然可以覆盖所有属性，但行为良好的助手应该在除了用户名和密码之外的任何属性上都不这样做。

​	如果助手输出带有值为 `true` 或 `1` 的 `quit` 属性，则不会再查阅其他助手，也不会提示用户（如果未提供凭据，则操作将失败）。

​	同样地，在提供了用户名和密码之后，不再查阅更多的助手。

​	对于 `store` 或 `erase` 操作，将忽略助手的输出。

​	如果助手未能执行请求的操作或需要通知用户可能出现的问题，则可以写入 stderr。

​	如果不支持请求的操作（例如只读存储或生成器），则应该静默地忽略请求。

​	如果助手接收到任何其他操作，则应该静默地忽略请求。这为未来添加新的操作留出了空间（旧的助手将忽略新的请求）。

## GIT

  这是[git[1]](../../Git)工具集中的一部分。