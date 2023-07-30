+++
title = "gitcredentials——中英对照版"
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

gitcredentials - Providing usernames and passwords to Git

​	gitcredentials - 向 Git 提供用户名和密码

## 概要

```
git config credential.https://example.com.username myusername
git config credential.helper "$helper $options"
```

## 描述

Git will sometimes need credentials from the user in order to perform operations; for example, it may need to ask for a username and password in order to access a remote repository over HTTP. Some remotes accept a personal access token or OAuth access token as a password. This manual describes the mechanisms Git uses to request these credentials, as well as some features to avoid inputting these credentials repeatedly.

​	有时候，Git 需要用户提供凭据以执行操作；例如，它可能需要要求用户名和密码以便通过 HTTP 访问远程存储库。有些远程存储库可以接受个人访问令牌或 OAuth 访问令牌作为密码。本手册描述了 Git 请求这些凭据的机制，以及一些避免重复输入这些凭据的功能。

## 请求凭据

Without any credential helpers defined, Git will try the following strategies to ask the user for usernames and passwords:

​	在未定义任何凭据助手的情况下，Git 将尝试以下策略来向用户请求用户名和密码：

1. If the `GIT_ASKPASS` environment variable is set, the program specified by the variable is invoked. A suitable prompt is provided to the program on the command line, and the user’s input is read from its standard output.
2. Otherwise, if the `core.askPass` configuration variable is set, its value is used as above.
3. Otherwise, if the `SSH_ASKPASS` environment variable is set, its value is used as above.
4. Otherwise, the user is prompted on the terminal.
5. 如果设置了 `GIT_ASKPASS` 环境变量，则调用该变量指定的程序。程序在命令行上提供适当的提示，并从其标准输出读取用户的输入。
6. 否则，如果设置了 `core.askPass` 配置变量，则使用其值，与上述情况类似。
7. 否则，如果设置了 `SSH_ASKPASS` 环境变量，则使用其值，与上述情况类似。
8. 否则，在终端上提示用户输入。

## 避免重复输入

It can be cumbersome to input the same credentials over and over. Git provides two methods to reduce this annoyance:

​	反复输入相同的凭据可能很繁琐。Git 提供了两种方法来减少这种麻烦：

1. Static configuration of usernames for a given authentication context.
2. Credential helpers to cache or store passwords, or to interact with a system password wallet or keychain.
3. 静态配置给定认证上下文的用户名。
4. 凭据助手来缓存或存储密码，或与系统密码库或密钥链交互。

The first is simple and appropriate if you do not have secure storage available for a password. It is generally configured by adding this to your config:

​	第一种方法简单，并且适用于没有可用于密码的安全存储的情况。通常可以通过在配置文件中添加以下内容来进行配置：

```
[credential "https://example.com"]
	username = me
```

Credential helpers, on the other hand, are external programs from which Git can request both usernames and passwords; they typically interface with secure storage provided by the OS or other programs. Alternatively, a credential-generating helper might generate credentials for certain servers via some API.

​	凭据助手则是外部程序，Git 可以向其请求用户名和密码；它们通常与操作系统或其他程序提供的安全存储进行交互。或者，凭据生成助手可以通过某些 API 为某些服务器生成凭据。

To use a helper, you must first select one to use. Git currently includes the following helpers:

​	要使用助手，首先必须选择一个要使用的助手。Git 当前包括以下几个助手：

- cache

  Cache credentials in memory for a short period of time. See [git-credential-cache[1\]](https://git-scm.com/docs/git-credential-cache) for details.

  在内存中缓存凭据一小段时间。详见 [git-credential-cache[1\]](https://git-scm.com/docs/git-credential-cache) 获取详细信息。

- store

  Store credentials indefinitely on disk. See [git-credential-store[1\]](https://git-scm.com/docs/git-credential-store) for details.

  永久地将凭据存储在磁盘上。详见 [git-credential-store[1\]](https://git-scm.com/docs/git-credential-store) 获取详细信息。

You may also have third-party helpers installed; search for `credential-*` in the output of `git help -a`, and consult the documentation of individual helpers. Once you have selected a helper, you can tell Git to use it by putting its name into the credential.helper variable.

​	你也可能安装了第三方的助手；在 `git help -a` 的输出中搜索 `credential-*`，并查阅各个助手的文档。选择了一个助手后，可以通过将其名称放入 `credential.helper` 变量中来告诉 Git 使用它。

1. Find a helper.

2. 找到一个助手。

   ```
   $ git help -a | grep credential-
   credential-foo
   ```

3. Read its description.

4. 阅读其描述。

   ```
   $ git help credential-foo
   ```

5. Tell Git to use it.

6. 告诉 Git 使用它。

   ```
   $ git config --global credential.helper foo
   ```

## 凭据上下文

Git considers each credential to have a context defined by a URL. This context is used to look up context-specific configuration, and is passed to any helpers, which may use it as an index into secure storage.

​	Git 视每个凭据为由 URL 定义的上下文。此上下文用于查找上下文特定的配置，并传递给任何助手，助手可能将其用作安全存储的索引。

For instance, imagine we are accessing `https://example.com/foo.git`. When Git looks into a config file to see if a section matches this context, it will consider the two a match if the context is a more-specific subset of the pattern in the config file. For example, if you have this in your config file:

​	例如，假设我们正在访问 `https://example.com/foo.git`。当 Git 查看配置文件以查看是否有与此上下文匹配的部分时，如果上下文是配置文件中模式的更具体的子集，则它们将被视为匹配。例如，如果在配置文件中有以下内容：

```
[credential "https://example.com"]
	username = foo
```

then we will match: both protocols are the same, both hosts are the same, and the "pattern" URL does not care about the path component at all. However, this context would not match:

那么我们将匹配：协议都相同，主机都相同，而"模式" URL 对路径组件不关心。然而，这个上下文将不会匹配：

```
[credential "https://kernel.org"]
	username = foo
```

because the hostnames differ. Nor would it match `foo.example.com`; Git compares hostnames exactly, without considering whether two hosts are part of the same domain. Likewise, a config entry for `http://example.com` would not match: Git compares the protocols exactly. However, you may use wildcards in the domain name and other pattern matching techniques as with the `http.<URL>.*` options.

因为主机名不同。或者 `foo.example.com` 也不匹配；Git 对主机名进行完全比较，而不考虑两个主机是否属于同一域。同样，对于 `http://example.com` 的配置条目也不匹配：Git 对协议进行完全比较。但是，你可以在域名中使用通配符和其他模式匹配技术，就像 `http.<URL>.*` 选项一样。

If the "pattern" URL does include a path component, then this too must match exactly: the context `https://example.com/bar/baz.git` will match a config entry for `https://example.com/bar/baz.git` (in addition to matching the config entry for `https://example.com`) but will not match a config entry for `https://example.com/bar`.

​	如果"模式" URL 中包含路径组件，则它也必须完全匹配：上下文 `https://example.com/bar/baz.git` 将匹配配置条目 `https://example.com/bar/baz.git`（除了匹配 `https://example.com` 的配置条目之外），但不会匹配配置条目 `https://example.com/bar`。

## 配置选项

Options for a credential context can be configured either in `credential.*` (which applies to all credentials), or `credential.<URL>.*`, where <URL> matches the context as described above.

​	凭据上下文的选项可以在 `credential.*`（适用于所有凭据）或 `credential.<URL>.*` 中配置，其中 `<URL>` 与上述描述的上下文匹配。

The following options are available in either location:

​	以下选项在这两个位置上都可以配置：

- helper

  The name of an external credential helper, and any associated options. If the helper name is not an absolute path, then the string `git credential-` is prepended. The resulting string is executed by the shell (so, for example, setting this to `foo --option=bar` will execute `git credential-foo --option=bar` via the shell. See the manual of specific helpers for examples of their use.If there are multiple instances of the `credential.helper` configuration variable, each helper will be tried in turn, and may provide a username, password, or nothing. Once Git has acquired both a username and a non-expired password, no more helpers will be tried.If `credential.helper` is configured to the empty string, this resets the helper list to empty (so you may override a helper set by a lower-priority config file by configuring the empty-string helper, followed by whatever set of helpers you would like).

  外部凭据助手的名称，以及任何相关选项。如果助手名称不是绝对路径，则在前面添加字符串 `git credential-`。结果字符串由 Shell 执行（因此，例如，将此设置为 `foo --option=bar` 将通过 Shell 执行 `git credential-foo --option=bar`）。有关其用法示例，请参阅特定助手的手册。如果有多个 `credential.helper` 配置变量的实例，则将依次尝试每个助手，并可能提供用户名、密码或空。一旦 Git 获取到用户名和未过期的密码，就不会再尝试更多的助手。如果将 `credential.helper` 配置为空字符串，则会将助手列表重置为空（因此，可以通过配置空字符串助手，然后是所需的助手集来覆盖较低优先级配置文件设置的助手）。

- username

  A default username, if one is not provided in the URL.

  默认的用户名，如果在 URL 中没有提供。

- useHttpPath

  By default, Git does not consider the "path" component of an http URL to be worth matching via external helpers. This means that a credential stored for `https://example.com/foo.git` will also be used for `https://example.com/bar.git`. If you do want to distinguish these cases, set this option to `true`.

  默认情况下，Git 不会通过外部助手来匹配 http URL 的 "path" 组件。这意味着为 `https://example.com/foo.git` 存储的凭据也将用于 `https://example.com/bar.git`。如果确实希望区分这些情况，请将此选项设置为 `true`。

## 自定义助手

You can write your own custom helpers to interface with any system in which you keep credentials.

​	您可以编写自己的自定义助手来与任何保存凭据的系统进行交互。

Credential helpers are programs executed by Git to fetch or save credentials from and to long-term storage (where "long-term" is simply longer than a single Git process; e.g., credentials may be stored in-memory for a few minutes, or indefinitely on disk).

​	凭据助手是由 Git 执行的程序，用于从长期存储中获取或保存凭据（"长期" 意味着超过一个 Git 进程；例如，凭据可以在内存中存储几分钟，或永久地存储在磁盘上）。

Each helper is specified by a single string in the configuration variable `credential.helper` (and others, see [git-config[1\]](https://git-scm.com/docs/git-config)). The string is transformed by Git into a command to be executed using these rules:

​	每个助手都由配置变量 `credential.helper`（以及其他变量，详见 [git-config[1\]](https://git-scm.com/docs/git-config)）中的一个字符串指定。Git 将根据以下规则将字符串转换为要执行的命令：

1. If the helper string begins with "!", it is considered a shell snippet, and everything after the "!" becomes the command.
2. Otherwise, if the helper string begins with an absolute path, the verbatim helper string becomes the command.
3. Otherwise, the string "git credential-" is prepended to the helper string, and the result becomes the command.
4. 如果助手字符串以 "!" 开头，则被视为 Shell 片段，"!" 之后的部分成为命令。
5. 否则，如果助手字符串以绝对路径开头，则原始助手字符串成为命令。
6. 否则，字符串 "git credential-" 被添加到助手字符串之前，并将结果作为命令。

The resulting command then has an "operation" argument appended to it (see below for details), and the result is executed by the shell.

​	然后，在该命令上附加一个 "operation" 参数（详见下文），并通过 Shell 执行该结果。

Here are some example specifications:

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

Generally speaking, rule (3) above is the simplest for users to specify. Authors of credential helpers should make an effort to assist their users by naming their program "git-credential-$NAME", and putting it in the `$PATH` or `$GIT_EXEC_PATH` during installation, which will allow a user to enable it with `git config credential.helper $NAME`.

​	一般来说，上述规则（3）对于用户来说是最简单的。凭据助手的作者应该努力帮助用户，将其程序命名为 "git-credential-$NAME"，并在安装过程中将其放置在 `$PATH` 或 `$GIT_EXEC_PATH` 中，这将允许用户使用 `git config credential.helper $NAME` 来启用它。

When a helper is executed, it will have one "operation" argument appended to its command line, which is one of:

​	当执行助手时，它的命令行将附加一个 "operation" 参数，该参数是以下之一：

- `get`

  Return a matching credential, if any exists.

  如果存在匹配的凭据，则返回它。

- `store`

  Store the credential, if applicable to the helper.

  将凭据存储（如果适用于该助手）。

- `erase`

  Remove a matching credential, if any, from the helper’s storage.

  从助手的存储中删除匹配的凭据（如果有）。

The details of the credential will be provided on the helper’s stdin stream. The exact format is the same as the input/output format of the `git credential` plumbing command (see the section `INPUT/OUTPUT FORMAT` in [git-credential[1\]](https://git-scm.com/docs/git-credential) for a detailed specification).

​	凭据的详细信息将在助手的标准输入流上提供。其确切格式与 `git credential` 基础命令的输入/输出格式相同（请参阅 [git-credential[1\]](https://git-scm.com/docs/git-credential) 中的 "INPUT/OUTPUT FORMAT" 部分，获取详细规范）。

For a `get` operation, the helper should produce a list of attributes on stdout in the same format (see [git-credential[1\]](https://git-scm.com/docs/git-credential) for common attributes). A helper is free to produce a subset, or even no values at all if it has nothing useful to provide. Any provided attributes will overwrite those already known about by Git’s credential subsystem. Unrecognised attributes are silently discarded.

​	对于 `get` 操作，助手应该在标准输出上以相同格式生成一系列属性（请参阅 [git-credential[1\]](https://git-scm.com/docs/git-credential) 获取常见属性）。助手可以自由地生成子集，甚至根本不提供任何值，如果没有有用的内容可以提供。提供的任何属性都将覆盖 Git 凭据子系统已知的属性。未被识别的属性会被静默丢弃。

While it is possible to override all attributes, well behaving helpers should refrain from doing so for any attribute other than username and password.

​	虽然可以覆盖所有属性，但行为良好的助手应该在除了用户名和密码之外的任何属性上都不这样做。

If a helper outputs a `quit` attribute with a value of `true` or `1`, no further helpers will be consulted, nor will the user be prompted (if no credential has been provided, the operation will then fail).

​	如果助手输出带有值为 `true` 或 `1` 的 `quit` 属性，则不会再查阅其他助手，也不会提示用户（如果未提供凭据，则操作将失败）。

Similarly, no more helpers will be consulted once both username and password had been provided.

​	同样地，在提供了用户名和密码之后，不再查阅更多的助手。

For a `store` or `erase` operation, the helper’s output is ignored.

​	对于 `store` 或 `erase` 操作，将忽略助手的输出。

If a helper fails to perform the requested operation or needs to notify the user of a potential issue, it may write to stderr.

​	如果助手未能执行请求的操作或需要通知用户可能出现的问题，则可以写入 stderr。

If it does not support the requested operation (e.g., a read-only store or generator), it should silently ignore the request.

​	如果不支持请求的操作（例如只读存储或生成器），则应该静默地忽略请求。

If a helper receives any other operation, it should silently ignore the request. This leaves room for future operations to be added (older helpers will just ignore the new requests).

​	如果助手接收到任何其他操作，则应该静默地忽略请求。这为未来添加新的操作留出了空间（旧的助手将忽略新的请求）。

## GIT

  这是[git[1]](../../Git)工具集中的一部分。