+++
title = "暂存区"
weight = 50
type = "docs"
date = 2023-07-26T09:47:23+08:00
description = ""
isCJKLanguage = true
draft = false
+++


# Staging Area - 暂存区

[https://git-scm.com/about/staging-area](https://git-scm.com/about/staging-area)

​	与其他版本控制系统不同的是，Git 有一个叫做 "暂存区staging area"或 "索引index"的东西。这是一个中间区域，在完成提交之前可以对提交的内容进行格式化和审查。

​	Git 与其他工具不同的一点是，它可以快速地对一些文件进行暂存并提交，而不需要提交工作目录中的所有其他修改过的文件，也不需要在提交时在命令行中列出这些文件（即：不需要提交的文件）。

![Index 1](StagingArea_img/index1@2x.png)

索引1
{: caption }

​	这使得您可以暂存仅修改的文件部分。以前您可能会在意识到忘记提交其中一个修改之前，就对文件进行了两个逻辑无关的修改。现在您可以仅暂存当前提交所需的更改，并为下一个提交暂存其他更改。此功能可扩展到所需的任意多个文件更改。

​	当然，如果您不想使用这个功能，Git 也很容易忽略它，只需在提交命令中添加 "-a" 即可将所有文件的所有更改添加到暂存区。

![Index 2](StagingArea_img/index2@2x.png)

索引2
{: caption }