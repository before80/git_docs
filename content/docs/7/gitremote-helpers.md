+++
title = "gitremote-helpers"
weight = 30
type = "docs"
date = 2023-07-30T15:39:23+08:00
description = ""
isCJKLanguage = true
draft = false

+++

# gitremote-helpers

https://git-scm.com/docs/gitremote-helpers

version 2.41.0

## 名称

​	gitremote-helpers - 与远程仓库交互的辅助程序

## 概要

```
git remote-<transport> <repository> [<URL>]
```

## 描述

​	远程辅助程序通常不直接由最终用户使用，而是在Git需要与其无法原生支持的远程仓库交互时由Git调用的。每个辅助程序实现了此处文档中的一部分功能。当Git需要使用远程辅助程序与仓库交互时，它会将辅助程序作为独立进程启动，将命令发送到辅助程序的标准输入，并期望从辅助程序的标准输出获得结果。由于远程辅助程序在Git之外作为独立进程运行，因此不需要重新链接Git以添加新的辅助程序，也不需要将辅助程序与Git的实现进行链接。

​	每个辅助程序都必须支持"capabilities"命令，Git使用该命令来确定辅助程序将接受哪些其他命令。这些其他命令可用于发现和更新远程引用，在对象数据库和远程仓库之间传输对象，并更新本地对象存储。

​	Git附带了一个“curl”系列的远程辅助程序，用于处理各种传输协议，例如 *git-remote-http*、*git-remote-https*、*git-remote-ftp* 和 *git-remote-ftps*。它们实现了 *fetch*、*option* 和 *push* 的功能。

## 调用方式

​	远程辅助程序通过一个或（可选地）两个参数调用。第一个参数指定远程仓库，就像在Git中一样；它可以是已配置远程的名称或URL。第二个参数指定URL；通常为 *<transport>://<address>* 形式，但也可以是任意的字符串。为远程辅助程序设置了`GIT_DIR`环境变量，并且可以用于确定在何处存储其他数据或从哪个目录调用辅助Git命令。

​	当Git遇到形式为 *<transport>://<address>* 的URL，其中 *<transport>* 是它无法原生处理的协议时，它会自动将 *git remote-<transport>* 作为第二个参数来调用。如果直接在命令行上遇到这样的URL，第一个参数与第二个参数相同；如果在配置的远程中遇到，则第一个参数是该远程的名称。

​	形式为 *<transport>::<address>* 的URL 显式地指示Git使用 *git remote-<transport>* 并将 *<address>* 作为第二个参数调用。如果直接在命令行上遇到这样的URL，第一个参数是 *<address>*；如果在配置的远程中遇到，则第一个参数是该远程的名称。

​	此外，当配置的远程具有`remote.<name>.vcs`设置为 *<transport>* 时，Git会明确调用 *git remote-<transport>* 并将 *<name>* 作为第一个参数调用。如果设置了第二个参数，则第二个参数是`remote.<name>.url`；否则，省略第二个参数。

## 输入格式

​	Git将一系列命令以每行一个的形式发送给远程辅助程序的标准输入。第一个命令始终是 *capabilities* 命令，对此，远程辅助程序必须打印其支持的功能列表（见下文），后跟一个空行。对 *capabilities* 命令的响应决定了Git在其余命令流中使用哪些命令。

​	命令流以空行终止。在某些情况下（在相关命令的文档中有说明），此空行后跟某些其他协议的负载（例如，pack协议），而在其他情况下，它表示输入的结束。

### 功能

​	每个远程辅助程序预期只支持一组命令。辅助程序支持的操作在响应`capabilities`命令（参见下文的COMMANDS）中向Git声明。

​	接下来，我们列出了所有定义的功能，并为每个功能列出了一个具有该功能的辅助程序必须提供的命令。

#### 推送功能

- *connect*

  可以尝试连接 *git receive-pack*（用于推送）、*git upload-pack* 等，以使用git的本地packfile协议进行通信。这需要一个双向的全双工连接。支持的命令：*connect*。

- *stateless-connect*

  实验性功能；仅供内部使用。可以尝试连接到远程服务器，以使用git的wire-protocol版本2进行通信。有关stateless-connect命令的更多信息，请参阅文档。支持的命令：*stateless-connect*。

- *push*

  可以发现远程引用并将本地提交及其前导历史推送到新的或现有的远程引用。支持的命令：*list for-push*、*push*。

- *export*

  可以发现远程引用，并将指定的对象从fast-import流推送到远程引用。支持的命令：*list for-push*、*export*。

​	如果辅助程序广告 *connect*，则如果可能，Git将使用它，并在连接时回退到其他功能，如果辅助程序在连接时请求如此（请参阅COMMANDS中的*connect*命令）。在选择 *push* 和 *export* 之间时，Git 优先选择 *push*。其他前端可能有其他优先级顺序。

- *no-private-update*

  当使用 *refspec* 功能时，git通常会在成功推送后更新私有引用。当远程辅助程序声明了 *no-private-update* 功能时，此更新被禁用。

#### 获取功能

- *connect*

  可以尝试连接 *git upload-pack*（用于拉取）、*git receive-pack* 等，以使用Git的本地packfile协议进行通信。这需要一个双向的全双工连接。支持的命令：*connect*。

- *stateless-connect*

  实验性功能；仅供内部使用。可以尝试连接到远程服务器，以使用Git的wire-protocol版本2进行通信。有关stateless-connect命令的更多信息，请参阅文档。支持的命令：*stateless-connect*。

- *fetch*

  可以发现远程引用，并将从这些引用可达的对象传输到本地对象存储中。支持的命令：*list*、*fetch*。

- *import*

  可以发现远程引用，并将从这些引用可达的对象输出为fast-import格式的流。支持的命令：*list*、*import*。

- *check-connectivity*

  可以保证在请求克隆时，接收到的pack是自包含且连接的。

- *get*

  可以使用 *get* 命令从给定的URI下载文件。

​	如果辅助程序广告 *connect*，则如果可能，Git将使用它，并在连接时回退到其他功能，如果辅助程序在连接时请求如此（请参阅COMMANDS中的*connect*命令）。在选择 *fetch* 和 *import* 之间时，Git 优先选择 *fetch*。其他前端可能有其他优先级顺序。

#### 其他功能

- *option*

  用于指定设置，如`verbosity`（将输出写入stderr的程度）和`depth`（浅克隆时需要的历史记录量），影响其他命令的执行方式。

- *refspec* <refspec>

  对于实现 *import* 或 *export* 的远程辅助程序，此功能允许将引用约束为私有命名空间，而不是直接写入到refs/heads或refs/remotes中。建议所有提供 *import* 功能的导入程序都使用它。对于 *export* 来说是强制性的。一个辅助程序广告功能 `refspec refs/heads/*:refs/svn/origin/branches/*` 意味着当被要求 `import refs/heads/topic` 时，它输出的流将更新 `refs/svn/origin/branches/topic` 引用。这个功能可以广告多次。第一个适用的refspec优先。使用此功能广告的refspecs的左侧必须涵盖列表命令报告的所有引用。如果没有广告 *refspec* 功能，则有一个隐含的 `refspec *:*`。在为分散式版本控制系统编写远程辅助程序时，建议在本地保留与之交互的仓库的副本，并且让私有命名空间引用指向这个本地仓库，而refs/remotes命名空间用于跟踪远程仓库。

- *bidi-import*

  这修改了 *import* 功能。远程辅助程序可以使用fast-import的 *cat-blob* 和 *ls* 命令检索已存在于fast-import内存中的blob和树的信息。这需要一个从fast-import到远程辅助程序的通道。如果广告功能中附加了 "import"，Git会在fast-import到远程辅助程序的stdin建立一个管道。由此可见，Git和fast-import都连接到远程辅助程序的stdin。由于Git可以向远程辅助程序发送多个命令，因此要求使用 *bidi-import* 的辅助程序在发送数据到fast-import之前缓冲所有 *import* 命令的批处理。这是为了防止在辅助程序的stdin上混合命令和fast-import响应。

- *export-marks* <file>

  这修改了 *export* 功能，指示Git在完成时将内部的标记表（marks table）导出到指定的 `<file>` 中。详情请参阅[git-fast-export[1]](../../1/git-fast-export)中的 `--export-marks=<file>`。

- *import-marks* <file>

  这修改了 *export* 功能，指示Git在处理任何输入之前加载指定在 `<file>` 中的标记（marks）。详情请参阅[git-fast-export[1]](../../1/git-fast-export)中的 `--import-marks=<file>`。

- *signed-tags*

  这修改了 *export* 功能，指示Git向[git-fast-export[1]](../../1/git-fast-export)传递 `--signed-tags=verbatim`。如果没有此功能，Git将使用 `--signed-tags=warn-strip`。

- *object-format*

  这表示远程辅助程序能够使用显式哈希算法扩展与远程端进行交互。

## 命令

​	调用方在远程辅助程序的标准输入中逐行给出命令。

- *capabilities*

  列出辅助程序的功能，每个功能一行，以空行结尾。每个功能可能前面带有 `*`，表示对于使用远程辅助程序的Git版本来说，这些功能是必需的。任何未知的强制性功能都是致命错误。对此命令的支持是必需的。

- *list*

  列出引用（refs），每行一个，格式为"<value> <name> [<attr> …]"。value 可能是十六进制SHA1哈希值，"@<dest>"表示符号引用（symref），":<keyword> <value>"表示键值对，或者"?"表示辅助程序无法获取引用的值。引用名称后跟一个空格分隔的属性列表；未知的属性将被忽略。列表以空行结束。有关当前定义的属性列表，请参阅 REF LIST ATTRIBUTES。有关当前定义的关键字列表，请参阅 REF LIST KEYWORDS。如果辅助程序具有 "fetch" 或 "import" 功能，则支持此命令。

- *list for-push*

  类似于 *list*，但仅当调用者希望生成推送命令的结果引用列表时使用。支持同时进行push和fetch的辅助程序可以使用它来区分 *list* 输出将用于哪种操作，从而可能减少需要执行的工作量。如果辅助程序具有 "push" 或 "export" 功能，则支持此命令。

- *option* <name> <value>

  将传输辅助程序选项 `<name>` 设置为 `<value>`。输出一行，包含 *ok*（成功设置选项）、*unsupported*（选项未被识别）或 *error <msg>*（支持选项 `<name>`，但 `<value>` 对其无效）。选项应在其他命令之前设置，并可能影响这些命令的行为。有关当前定义的选项列表，请参阅 OPTIONS。如果辅助程序具有 "option" 功能，则支持此命令。

- *fetch* <sha1> <name>

  获取给定的对象，将必要的对象写入数据库。获取命令以批处理形式发送，每行一个，以空行终止。当同一批处理中的所有获取命令都完成时，输出一行空行。只有在 *list* 输出中报告了SHA1值的对象才可以通过此方式获取。可选地，可能会输出 *lock <file>* 行，指示一个文件在 `$GIT_DIR/objects/pack` 下保持一个pack，直到引用可以适当地更新。路径必须以 `.keep` 结尾。这是一种通过仅给出 keep 组件来命名 `<pack,idx,keep>` 元组的机制。保持的pack在并发重打包时不会被删除，即使在获取完成之前其对象可能不被引用。在获取完成时，`.keep` 文件将被删除。如果请求 *check-connectivity* 选项，则如果克隆是自包含且连接的，辅助程序必须输出 *connectivity-ok*。如果辅助程序具有 "fetch" 功能，则支持此命令。

- *push* +<src>:<dst>

  将给定的本地 `<src>` 提交或分支推送到由 `<dst>` 描述的远程分支。一个批处理序列由一个或多个 *push* 命令组成，以空行终止（如果只有一个要推送的引用，则单个 *push* 命令后跟一个空行）。例如，下面是两个 *push* 批处理，第一个批处理要求远程辅助程序将本地引用 *master* 推送到远程引用 *master* 和本地的 `HEAD` 推送到远程 *branch*，第二个批处理要求推送引用 *foo* 到引用 *bar*（使用 `+` 表示强制更新）。

  ```
  push refs/heads/master:refs/heads/master
  push HEAD:refs/heads/branch
  \n
  push +refs/heads/foo:refs/heads/bar
  \n
  ```

  在最后一个 *push* 命令后的空行之前，可以输入零个或多个协议选项。推送完成后，输出一个或多个 *ok <dst>* 或 *error <dst> <why>?* 行，指示每个推送引用的成功或失败。状态报告输出以空行终止。如果 <why> 字段中包含LF，则可能会在C样式字符串中引用 <why>。如果辅助程序具有 "push" 功能，则支持此命令。

- *import* <name>

  生成导入当前命名引用的fast-import流。如果需要，它可能还导入其他引用以有效构建历史记录。该脚本写入辅助程序特定的私有命名空间。应该将命名引用的值写入此命名空间中的位置，该位置通过将 "refspec" 功能的refspecs应用于引用名称来得到。对于与外部版本控制系统的互操作特别有用。与 *push* 一样，一个或多个 *import* 的批处理以空行终止。对于每个 *import* 批处理，远程辅助程序应该生成一个以 *done* 命令结束的fast-import流。请注意，如果使用 *bidi-import* 功能，必须在开始将数据发送到fast-import之前，将完整的批处理序列缓冲，以防止在辅助程序的stdin上混合命令和fast-import响应。如果辅助程序具有 "import" 功能，则支持此命令。

- *export*

  指示远程辅助程序，任何后续的输入都是fast-import流（由 *git fast-export* 生成），其中包含应该推送到远程的对象。与外部版本控制系统的互操作特别有用。如果指定了 *export-marks* 和 *import-marks* 功能，则它们会影响此命令，因为它们会传递给 *git fast-export*，然后将为本地对象加载/存储标记表。这可用于实现增量操作。如果辅助程序具有 "export" 功能，则支持此命令。

- *connect* <service>

  连接到给定的服务。辅助程序的标准输入和标准输出与指定的服务（git前缀包含在服务名称中，因此例如获取使用 *git-upload-pack* 作为服务）在远程端连接。对此命令的有效回复是空行（连接建立）、*fallback*（不支持智能传输，回退到愚蠢传输）以及仅打印错误消息后退出（无法连接，不要尝试回退）。在换行符终止肯定（空）回复后，服务的输出开始。连接结束后，远程辅助程序退出。如果辅助程序具有 "connect" 功能，则支持此命令。

- *stateless-connect* <service>

  实验性功能；仅供内部使用。连接到给定的远程服务，以使用git的wire-protocol版本2进行通信。对此命令的有效回复是空行（连接建立）、*fallback*（不支持智能传输，回退到愚蠢传输）以及仅打印错误消息后退出（无法连接，不要尝试回退）。在换行符终止肯定（空）回复后，服务的输出开始。消息（请求和响应）必须由零个或多个PKT-LINE组成，在刷新数据包中终止。响应消息将在刷新数据包之后包含响应结束数据包，表示响应的结束。客户端不应该期望服务器在请求-响应对之间存储任何状态。连接结束后，远程辅助程序退出。如果辅助程序具有 "stateless-connect" 功能，则支持此命令。

- *get* <uri> <path>

  从给定的 `<uri>` 下载文件到给定的 `<path>` 中。如果 `<path>.temp` 存在，则Git会认为 `.temp` 文件是先前尝试的部分下载，将从该位置恢复下载。

​	如果发生致命错误，程序会将错误消息写入stderr并退出。如果子进程在完成当前命令的有效响应之前关闭连接，调用方应该期望已经打印了适当的错误消息。额外的命令可能受支持，可以从辅助程序报告的功能中确定。

​	其他命令可能会得到支持，这取决于辅助程序报告的功能。

## REF LIST ATTRIBUTES

​	*list* 命令会生成一份引用（refs）列表，其中每个引用后面可能跟着一组属性。以下是已定义的引用列表属性。

- *unchanged*

  此引用自上次导入或获取以来未更改，尽管辅助程序可能无法确定产生了什么值。

## REF LIST KEYWORDS

​	*list* 命令可能会生成一组键值对列表。以下是已定义的键。

- *object-format*

  这些引用使用给定的哈希算法。仅当服务器和客户端都支持 object-format 扩展时才使用此关键字。

## 选项

​	以下选项是定义的，并且（在适当的情况下）如果远程辅助程序具有 *option* 功能，则由Git设置。

- *option verbosity* <n>

  更改辅助程序显示的消息的详细程度。对于 <n> 的值为0，表示进程静默操作，辅助程序仅输出错误消息。默认的详细程度是1，较高的 <n> 值对应于在命令行上传递的 -v 标志的数量。

- *option progress* {*true*|*false*}

  在命令执行期间启用（或禁用）传输辅助程序显示的进度消息。

- *option depth* <depth>

  深化浅仓库的历史记录。

- 'option deepen-since <timestamp>

  根据时间深化浅仓库的历史记录。

- 'option deepen-not <ref>

  排除ref，深化浅仓库的历史记录。多个选项叠加。

- *option deepen-relative {'true*|*false*}

  相对于当前边界深化浅仓库的历史记录。只有在与 "option depth" 一起使用时有效。

- *option followtags* {*true*|*false*}

  如果启用，辅助程序应在获取命令期间自动获取带注释的标签对象，如果标签所指向的对象在获取命令期间已传输。如果辅助程序未获取标签，通常会发送第二个获取命令以请求特定的标签。某些辅助程序可以使用此选项避免进行第二个网络连接。

*option dry-run* {*true*|*false*}: 如果为true，则假装操作已成功完成，但实际上不会更改任何仓库数据。对于大多数辅助程序，这仅适用于 *push*（如果支持的话）。

- *option servpath <c-style-quoted-path>*

  为下一次连接设置服务路径（--upload-pack、--receive-pack等）。远程辅助程序可能支持此选项，但不能依赖于在连接请求发生之前设置此选项。

- *option check-connectivity* {*true*|*false*}

  请求辅助程序检查克隆的连接性。

- *option force* {*true*|*false*}

  请求辅助程序执行强制更新。默认为 *false*。

- *option cloning* {*true*|*false*}

  通知辅助程序这是一个克隆请求（即当前仓库保证为空）。

- *option update-shallow* {*true*|*false*}

  允许扩展 .git/shallow，如果新的引用需要它。

- *option pushcert* {*true*|*false*}

  GPG签名推送。

- 'option push-option <string>

  以推送选项传输 <string>。由于推送选项不能包含LF或NUL字符，因此不对字符串进行编码。

- *option from-promisor* {*true*|*false*}

  表示这些对象正在从承诺方获取。

- *option no-dependents* {*true*|*false*}

  表示只需要获取所需的对象，而不需要它们的依赖对象。

- *option atomic* {*true*|*false*}

  在推送时，请求远程服务器以单个原子事务更新引用。如果成功，所有引用都将被更新，否则将不会更新任何引用。如果远程端不支持此功能，则推送将失败。

- *option object-format* {*true*|algorithm}

  如果为 *true*，表示调用方希望从远程传回哈希算法信息。在获取引用时使用此模式。如果设置为算法，则表示调用方希望使用该算法与远程端进行交互。

## 另请参阅

[git-remote[1]](../../1/git-remote)

[git-remote-ext[1]](../../1/git-remote-ext)

[git-remote-fd[1]](../../1/git-remote-fd)

[git-fast-import[1]](../../1/git-fast-import)

## GIT

  这是[git[1]](../../Git)工具集中的一部分。