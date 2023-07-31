+++
title = "gitnamespaces"
weight = 30
type = "docs"
date = 2023-07-30T15:39:23+08:00
description = ""
isCJKLanguage = true
draft = false
+++

# gitnamespaces 

https://git-scm.com/docs/gitnamespaces

version 2.32.2

## 名称

​	gitnamespaces - Git 命名空间

## 概述

```
GIT_NAMESPACE=<namespace> git upload-pack
GIT_NAMESPACE=<namespace> git receive-pack
```

## 描述

​	Git 支持将单个存储库的引用分割为多个命名空间，每个命名空间都有自己的分支、标签和 HEAD。Git 可以将每个命名空间作为独立的存储库暴露出来，以供拉取和推送使用，同时共享对象存储，并将所有引用暴露给诸如 [git-gc[1]](../../1/git-gc) 等操作。

​	将多个存储库作为单个存储库的命名空间存储可以避免存储相同对象的重复副本，例如存储相同源代码的多个分支。alternates 机制提供了类似的支持来避免重复，但 alternates 不能防止新对象在没有持续维护的情况下添加到存储库时的重复，而命名空间则可以。

​	要指定命名空间，请将 `GIT_NAMESPACE` 环境变量设置为该命名空间。对于每个引用命名空间，Git 将相应的引用存储在 `refs/namespaces/` 目录下。例如，`GIT_NAMESPACE=foo` 将引用存储在 `refs/namespaces/foo/` 下。您还可以通过 [git[1]](../../1/git) 命令的 `--namespace` 选项指定命名空间。

​	请注意，包含 `/` 的命名空间将扩展为一个命名空间的层次结构；例如，`GIT_NAMESPACE=foo/bar` 将引用存储在 `refs/namespaces/foo/refs/namespaces/bar/` 下。这使得 `GIT_NAMESPACE` 中的路径行为成为层次结构，因此使用 `GIT_NAMESPACE=foo/bar` 克隆将产生与使用 `GIT_NAMESPACE=foo` 克隆，并从该存储库克隆具有 `GIT_NAMESPACE=bar` 的结果相同。这还避免了奇怪的命名空间路径，比如 `foo/refs/heads/`，否则可能会在 `refs` 目录中生成目录/文件冲突。

​	[git-upload-pack[1]](../../1/git-upload-pack) 和 [git-receive-pack[1]](../../1/git-receive-pack) 会根据 `GIT_NAMESPACE` 指定的名称重新编写引用名称。git-upload-pack 和 git-receive-pack 将忽略指定命名空间之外的所有引用。

​	智能 HTTP 服务器 [git-http-backend[1]](../../1/git-http-backend) 将 `GIT_NAMESPACE` 传递给后端程序；请参阅 [git-http-backend[1]](../../1/git-http-backend) 中的示例配置以将存储库命名空间暴露为存储库。

​	对于简单的本地测试，您可以使用 [git-remote-ext[1]](../../1/git-remote-ext)：

```
git clone ext::'git --namespace=foo %s /tmp/prefixed.git'
```

## 安全性

​	拉取和推送协议不设计防止一方从另一个存储库窃取未打算共享的数据。如果您有需要保护免受恶意同行窃取的私有数据，则最好将其存储在另一个存储库中。这适用于客户端和服务器。特别是，在服务器上使用的命名空间对于读取访问控制没有效果；您应该只向信任具有对整个存储库的读取访问权限的客户端授予对命名空间的读取访问权限。

The known attack vectors are as follows:

​	已知的攻击向量如下：

1. 受害者发送 "have" 行广告其具有但不明确打算共享的对象 ID。攻击者选择要窃取的对象 ID X 并发送一个指向 X 的引用，但不需要发送 X 的内容，因为受害者已经拥有它。现在，受害者认为攻击者拥有 X，并在以后将 X 的内容发送回给攻击者。（客户端对服务器执行此攻击最直接，通过在客户端可以访问的命名空间中创建对 X 的引用，然后获取它。服务器对客户端执行此攻击的最可能方法是将 X "合并" 到公共分支中，并希望用户在此分支上进行其他工作并将其推送回服务器而不注意到合并。）
3. 类似于 #1，攻击者选择要窃取的对象 ID X。受害者发送攻击者已经拥有的对象 Y，并且攻击者虚假地声称拥有 X 而没有 Y，因此受害者将 Y 作为相对于 X 的增量发送。增量向攻击者透露了 X 的与 Y 类似的区域。

## GIT

  这是[git[1]](../../Git)工具集中的一部分。