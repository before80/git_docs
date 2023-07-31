+++
title = "Git子模块"
weight = 30
type = "docs"
date = 2023-07-30T15:39:23+08:00
description = ""
isCJKLanguage = true
draft = false
+++

# gitsubmodules - Git子模块

https://git-scm.com/docs/gitsubmodules

version 2.41.0

## 名称

gitsubmodules - 将一个仓库嵌套到另一个仓库中

## 概要

```
.gitmodules, $GIT_DIR/config
git submodule
git <command> --recurse-submodules
```

## 描述

​	子模块是嵌套在另一个仓库中的仓库。子模块有自己的历史记录，嵌套的仓库称为超级项目（superproject）。

​	在文件系统中，子模块通常（但不总是，参见下面的"FORMS"部分）由以下部分组成：(i) 位于超级项目的`$GIT_DIR/modules/`目录下的Git目录，(ii) 位于超级项目工作目录内的工作目录，以及位于子模块工作目录根目录的`.git`文件，该文件指向（i）。

​	假设子模块在`$GIT_DIR/modules/foo/`具有Git目录，工作目录在`path/to/bar/`，则超级项目通过`path/to/bar`处的树中的`gitlink`条目以及其`.gitmodules`文件中的条目（参见[gitmodules[5]](../../5/gitmodules)）来跟踪子模块。

​	`gitlink`条目包含超级项目期望子模块的工作目录所在提交的对象名称。

​	`.gitmodules`文件中的`submodule.foo.*`部分为Git的瓷砖层提供了额外的提示。例如，`submodule.foo.url`设置指定了获取子模块的位置。

​	子模块可用于至少两种不同的用例：

1. 在保持独立历史的同时使用另一个项目。子模块允许您在自己的工作目录内包含另一个项目的工作树，同时保持两个项目的历史记录分开。此外，由于子模块固定在一个任意版本上，因此可以在不影响超级项目的情况下独立开发其他项目，允许超级项目在需要时仅固定到新版本。
2. 将（逻辑上单个的）项目拆分为多个仓库并将它们重新关联。这可以用于克服Git实现的当前限制，以实现更精细的访问控制：
   - Git仓库的大小：以其当前形式，对于包含未通过树之间增量计算进行压缩的内容的大型仓库，Git的扩展性较差。例如，您可以使用子模块来容纳大型二进制资产，这些仓库可以被浅克隆，以便您在本地不需要大量历史记录的情况下工作。
   - 传输大小：以其当前形式，Git需要完整的工作树存在。它不允许在fetch或clone中传输部分树。如果您的项目由多个仓库组成，并作为子模块在超级项目中绑定在一起，您可以避免获取您不感兴趣的仓库的工作树。
   - 访问控制：通过限制用户对子模块的访问，可以实现不同用户的读/写策略。

## 子模块的配置

​	可以使用以下机制配置子模块操作（按优先级从高到低排序）：

- 对于支持将子模块作为其路径规范的一部分的命令，可以通过命令行进行配置。大多数命令都有一个布尔标志`--recurse-submodules`，指定是否递归进入子模块。例如，`grep`和`checkout`就是这样的命令。有些命令采用枚举，例如`fetch`和`push`，您可以在其中指定子模块受到的影响方式。

- 子模块内的配置。这包括子模块中的`$GIT_DIR/config`，以及树中的设置，例如`.gitattributes`或`.gitignore`文件，指定了子模块内部命令的行为。

  例如，子模块的`.gitignore`文件的效果可以在超级项目中运行`git status --ignore-submodules=none`时观察到。这通过在子模块中运行`status`并注意子模块的`.gitignore`文件来收集信息。

  

  运行`git push --recurse-submodules=check`在超级项目中会涉及子模块的`$GIT_DIR/config`文件，因为它会检查子模块是否有任何未发布到任何远程的更改。远程配置在子模块中像往常一样在`$GIT_DIR/config`文件中。

- 超级项目中的配置文件`$GIT_DIR/config`。Git仅递归进入活动子模块（参见下面的"活动子模块"部分）。

  如果子模块尚未初始化，则子模块内的配置尚不存在，因此此处可以配置从何处获取子模块。

- 超级项目中的`.gitmodules`文件。一个项目通常使用此文件来为所需的子模块名称和路径之间的映射建议默认值。

  此文件主要用作超级项目中子模块名称和路径之间的映射，以便可以定位子模块的Git目录。

  如果子模块从未初始化过，则只有在此处找到子模块配置。它作为最后的回退来指定从何处获取子模块。


## 形式

​	子模块可以采用以下形式：

- 在描述中描述的基本形式，包括一个Git目录、一个工作目录、一个`gitlink`和一个`.gitmodules`条目。

- "旧形式"子模块：一个工作目录，带有嵌入的`.git`目录，在超级项目中跟踪`gitlink`和`.gitmodules`条目。这通常在使用较早版本的Git生成的仓库中找到。

  可以手动构造这些旧形式的仓库。

  当取消初始化或删除子模块时（参见下文），子模块的Git目录会自动移动到超级项目的`$GIT_DIR/modules/<name>/`中。

- 取消初始化的子模块：一个`gitlink`和一个`.gitmodules`条目，但没有子模块的工作目录。子模块的Git目录可能还在，因为取消初始化后，Git目录会保留。该目录应该是工作目录，但现在为空。

  可以通过运行`git submodule deinit`来取消初始化子模块。除了清空工作目录外，此命令仅修改超级项目的`$GIT_DIR/config`文件，因此超级项目的历史记录不受影响。可以使用`git submodule init`来撤销此操作。

- 已删除的子模块：可以通过运行`git rm <submodule path> && git commit`来删除子模块。可以使用`git revert`来撤销此操作。

  删除操作会删除超级项目的跟踪数据，包括`gitlink`条目和`.gitmodules`文件中的部分。子模块的工作目录会从文件系统中删除，但Git目录会保留，以便能够在不需要从另一个仓库获取的情况下检出过去的提交。

  要完全删除子模块，请手动删除`$GIT_DIR/modules/<name>/`。

  


## 活动子模块

​	如果满足以下条件之一，子模块被视为活动：

1. 如果`submodule.<name>.active`设置为`true`

   或者

2. 如果子模块的路径与`submodule.active`中的路径规范匹配

   或者

5. 如果设置了`submodule.<name>.url`


并且这些条件按照此顺序进行评估。

例如：

```
[submodule "foo"]
  active = false
  url = https://example.org/foo
[submodule "bar"]
  active = true
  url = https://example.org/bar
[submodule "baz"]
  url = https://example.org/baz
```

​	在上面的配置中，只有子模块*bar*和*baz*是活动的，*bar*由于（1），*baz*由于（3）。*foo*是非活动的，因为（1）优先于（3）。

​	请注意，（3）是历史遗留问题，如果（1）和（2）指定子模块不活动，将忽略它。换句话说，如果我们有一个`submodule.<name>.active`设置为`false`，或者如果子模块的路径在`submodule.active`的路径规范中被排除，那么.url字段的存在与否无关。这在下面的示例中进行了说明。

```
[submodule "foo"]
  active = true
  url = https://example.org/foo
[submodule "bar"]
  url = https://example.org/bar
[submodule "baz"]
  url = https://example.org/baz
[submodule "bob"]
  ignore = true
[submodule]
  active = b*
  active = :(exclude) baz
```

​	在这个示例中，除了*baz*（foo、bar、bob）之外的所有子模块都是活动的。*foo*由于其自己的活动标志，其他所有子模块由于子模块活动路径规范，该规范指定除*baz*之外以*b*开头的任何子模块都是活动的，而与.url字段的存在与否无关。

## 第三方库的工作流程

```
# 添加一个子模块
git submodule add <URL> <path>
# 偶尔将子模块更新到新版本：
git -C <path> checkout <new version>
git add <path>
git commit -m "update submodule to new version"
# 在超级项目中查看子模块列表
git submodule status
# 查看关于删除子模块的 FORMS
```

## 人工拆分仓库的工作流程

```
# 启用递归，使得相关命令默认递归进入子模块
git config --global submodule.recurse true
# 与下面的其他命令不同，clone仍然需要自己的递归标志：
git clone --recurse <URL> <directory>
cd <directory>
# 了解代码：
git grep foo
git ls-files --recurse-submodules
```

> 注意
>
> ​	`git ls-files`也需要自己的`--recurse-submodules`标志。

```
# Get new code
# 获取新代码
git fetch
git pull --rebase
# Change worktree
# 更改工作树
git checkout
git reset
```

## 实现细节

​	在克隆或拉取包含子模块的仓库时，默认情况下不会检出子模块；您可以指示`clone`递归进入子模块。`git submodule`的`init`和`update`子命令将在您的工作树中维护已检出的子模块，并保持适当的修订。或者，您可以设置`submodule.recurse`，使`checkout`递归进入子模块（请注意，`submodule.recurse`还会影响其他Git命令，详见[git-config[1]](../../1/git-config)获取完整列表）。

## 另请参阅

[git-submodule[1]](../../1/git-submodule), [gitmodules[5]](../../5/gitmodules).

## GIT

  	这是[git[1]](../../Git)工具集中的一部分。