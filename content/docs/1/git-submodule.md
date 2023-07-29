https://git-scm.com/docs/git-submodule

## 名称

git-submodule - 初始化、更新或检查子模块

## 概述

```
git submodule [--quiet] [--cached]
git submodule [--quiet] add [<options>] [--] <repository> [<path>]
git submodule [--quiet] status [--cached] [--recursive] [--] [<path>…]
git submodule [--quiet] init [--] [<path>…]
git submodule [--quiet] deinit [-f|--force] (--all|[--] <path>…)
git submodule [--quiet] update [<options>] [--] [<path>…]
git submodule [--quiet] set-branch [<options>] [--] <path>
git submodule [--quiet] set-url [--] <path> <newurl>
git submodule [--quiet] summary [<options>] [--] [<path>…]
git submodule [--quiet] foreach [--recursive] <command>
git submodule [--quiet] sync [--recursive] [--] [<path>…]
git submodule [--quiet] absorbgitdirs [--] [<path>…]
```

## 描述

检查、更新和管理子模块。

有关子模块的更多信息，请参阅[gitsubmodules[7]](../../7/gitsubmodules).

## 命令

With no arguments, shows the status of existing submodules. Several subcommands are available to perform operations on the submodules.

没有参数时，显示现有子模块的状态。有几个子命令可用于对子模块执行操作。

- add [-b <branch>] [-f|--force] [--name <name>] [--reference <repository>] [--depth <depth>] [--] <repository> [<path>]

  Add the given repository as a submodule at the given path to the changeset to be committed next to the current project: the current project is termed the "superproject".

  将给定的仓库作为子模块添加到下一个要提交到当前项目旁边的更改集的给定路径中：当前项目称为“超级项目”。

  <repository> is the URL of the new submodule’s origin repository. This may be either an absolute URL, or (if it begins with ./ or ../), the location relative to the superproject’s default remote repository (Please note that to specify a repository *foo.git* which is located right next to a superproject *bar.git*, you’ll have to use `../foo.git` instead of `./foo.git` - as one might expect when following the rules for relative URLs - because the evaluation of relative URLs in Git is identical to that of relative directories).

  <repository>是新子模块的起点仓库的URL。这可以是绝对URL，也可以是（如果以./或../开头）相对于超级项目的默认远程仓库的位置（请注意，要指定一个名为foo.git的仓库，它位于超级项目bar.git旁边，您必须使用../foo.git而不是./foo.git - 因为在Git中相对URL的计算与相对目录的计算相同）。

  The default remote is the remote of the remote-tracking branch of the current branch. If no such remote-tracking branch exists or the HEAD is detached, "origin" is assumed to be the default remote. If the superproject doesn’t have a default remote configured the superproject is its own authoritative upstream and the current working directory is used instead.

  默认远程是当前分支的远程跟踪分支的远程。如果不存在此类远程跟踪分支或HEAD是分离的，则假定“origin”是默认远程。如果超级项目没有配置默认远程，则超级项目是其自身的官方上游，当前工作目录将代替。

  The optional argument <path> is the relative location for the cloned submodule to exist in the superproject. If <path> is not given, the canonical part of the source repository is used ("repo" for "/path/to/repo.git" and "foo" for "host.xz:foo/.git"). If <path> exists and is already a valid Git repository, then it is staged for commit without cloning. The <path> is also used as the submodule’s logical name in its configuration entries unless `--name` is used to specify a logical name.

  可选参数 <path> 是子模块在超级项目中的相对位置。如果未给出 <path>，则使用源仓库的规范部分（对于 "/path/to/repo.git" 使用 "repo"，对于 "host.xz:foo/.git" 使用 "foo"）。如果 <path> 存在且已经是有效的 Git 仓库，则它将被暂存以进行提交，而无需克隆。除非使用 --name 指定逻辑名称，否则 <path> 也用作子模块在其配置条目中的逻辑名称。

  The given URL is recorded into `.gitmodules` for use by subsequent users cloning the superproject. If the URL is given relative to the superproject’s repository, the presumption is the superproject and submodule repositories will be kept together in the same relative location, and only the superproject’s URL needs to be provided. git-submodule will correctly locate the submodule using the relative URL in `.gitmodules`.

  给定的 URL 记录在 .gitmodules 中，供随后克隆超级项目的用户使用。如果 URL 相对于超级项目的仓库，则假定超级项目和子模块仓库将在相同的相对位置保持在一起，只需提供超级项目的 URL 即可。git-submodule 将使用 .gitmodules 中的相对 URL 正确地定位子模块。

- status [--cached] [--recursive] [--] [<path>…]

  Show the status of the submodules. This will print the SHA-1 of the currently checked out commit for each submodule, along with the submodule path and the output of *git describe* for the SHA-1. Each SHA-1 will possibly be prefixed with `-` if the submodule is not initialized, `+` if the currently checked out submodule commit does not match the SHA-1 found in the index of the containing repository and `U` if the submodule has merge conflicts.

  显示子模块的状态。这将打印每个子模块当前签出提交的 SHA-1，以及子模块路径和 SHA-1 的 git describe 输出。如果子模块未初始化，则每个 SHA-1 可能会带有前缀“-”，如果当前签出的子模块提交与包含仓库的索引中找到的 SHA-1 不匹配，则可能会带有前缀“+”，如果子模块存在合并冲突，则可能会带有前缀“U”。

  If `--cached` is specified, this command will instead print the SHA-1 recorded in the superproject for each submodule.

  如果指定了 --cached，则此命令将打印超级项目记录的每个子模块的 SHA-1。

  If `--recursive` is specified, this command will recurse into nested submodules, and show their status as well.

  如果指定了 --recursive，则此命令将递归进入嵌套的子模块，并显示它们的状态。

  If you are only interested in changes of the currently initialized submodules with respect to the commit recorded in the index or the HEAD, [git-status[1]](../git-status) and [git-diff[1]](../git-diff) will provide that information too (and can also report changes to a submodule’s work tree).

  如果您只关心与索引或 HEAD 中记录的提交相比当前初始化的子模块的更改，则 git-status[1] 和 git-diff[1] 也会提供该信息（还可以报告对子模块的工作树的更改）。

- init [--] [<path>…]

  Initialize the submodules recorded in the index (which were added and committed elsewhere) by setting `submodule.$name.url` in .git/config. It uses the same setting from `.gitmodules` as a template. If the URL is relative, it will be resolved using the default remote. If there is no default remote, the current repository will be assumed to be upstream.

  通过在 .git/config 中设置 submodule.$name.url，初始化已在索引中记录的子模块（在其他地方添加并提交）。它使用 .gitmodules 中的相同设置作为模板。如果 URL 是相对的，则将使用默认远程解析它。如果没有默认远程，则假定当前仓库是上游。

  Optional <path> arguments limit which submodules will be initialized. If no path is specified and submodule.active has been configured, submodules configured to be active will be initialized, otherwise all submodules are initialized.

  可选的 <path> 参数限制哪些子模块将被初始化。如果未指定路径并且已经配置了 submodule.active，则将初始化配置为 active 的子模块，否则将初始化所有子模块。

  When present, it will also copy the value of `submodule.$name.update`. This command does not alter existing information in .git/config. You can then customize the submodule clone URLs in .git/config for your local setup and proceed to `git submodule update`; you can also just use `git submodule update --init` without the explicit *init* step if you do not intend to customize any submodule locations.

  See the add subcommand for the definition of default remote.

  如果存在，则还将复制`submodule.$name.update`的值。该命令不会更改.git/config中的现有信息。然后，您可以在.git/config中自定义子模块克隆URL以进行本地设置，并继续执行`git submodule update`。如果您不打算自定义任何子模块位置，则也可以只使用`git submodule update --init`而无需显式init步骤。

  有关默认远程的定义，请参见add子命令。

- deinit [-f|--force] (--all|[--] <path>…)

  Unregister the given submodules, i.e. remove the whole `submodule.$name` section from .git/config together with their work tree. Further calls to `git submodule update`, `git submodule foreach` and `git submodule sync` will skip any unregistered submodules until they are initialized again, so use this command if you don’t want to have a local checkout of the submodule in your working tree anymore.

  注销给定的子模块，即从.git/config中删除整个submodule.$name部分以及其工作树。对于未注册的子模块，后续调用git submodule update、git submodule foreach和git submodule sync将跳过它们，直到再次初始化它们，因此如果您不再想在工作树中具有子模块的本地副本，请使用此命令。

  When the command is run without pathspec, it errors out, instead of deinit-ing everything, to prevent mistakes.

  如果在不带路径的情况下运行该命令，则会出错，而不是一切都解除初始化，以防止出错。

  If `--force` is specified, the submodule’s working tree will be removed even if it contains local modifications.

  如果指定了--force，则即使工作树包含本地修改，也将删除子模块的工作树。

  If you really want to remove a submodule from the repository and commit that use [git-rm[1]](../git-rm) instead. 

  如果您确实要从仓库中删除子模块并提交，请改用[git-rm[1]](../git-rm) 。有关删除选项，请参见[gitsubmodules[7]](../../7/gitsubmodules)。

  See [gitsubmodules[7]](../../7/gitsubmodules) for removal options.

- update [--init] [--remote] [-N|--no-fetch] [--[no-]recommend-shallow] [-f|--force] [--checkout|--rebase|--merge] [--reference <repository>] [--depth <depth>] [--recursive] [--jobs <n>] [--[no-]single-branch] [--filter <filter spec>] [--] [<path>…]

  Update the registered submodules to match what the superproject expects by cloning missing submodules, fetching missing commits in submodules and updating the working tree of the submodules. The "updating" can be done in several ways depending on command line options and the value of `submodule.<name>.update` configuration variable. The command line option takes precedence over the configuration variable. If neither is given, a *checkout* is performed. The *update* procedures supported both from the command line as well as through the `submodule.<name>.update` configuration are:

  通过克隆缺失的子模块、获取子模块中缺失的提交和更新子模块的工作树，更新已注册的子模块以匹配超级项目的期望。"updating"可以通过命令行选项和submodule.<name>.update配置变量的值以几种不同的方式完成。命令行选项优先于配置变量。如果两者都没有给出，将执行检出操作。更新程序既可以从命令行支持，也可以通过submodule.<name>.update配置支持，其中包括：

  **checkout**

  the commit recorded in the superproject will be checked out in the submodule on a detached HEAD.

  checkout - 在子模块上分离HEAD，检出在超级项目中记录的提交。

  If `--force` is specified, the submodule will be checked out (using `git checkout --force`), even if the commit specified in the index of the containing repository already matches the commit checked out in the submodule.

  如果指定了--force，则会强制检出子模块（使用git checkout --force），即使容纳仓库的索引中指定的提交已与子模块中检出的提交匹配。

  **rebase**

  the current branch of the submodule will be rebased onto the commit recorded in the superproject.

  在子模块中，将当前分支变基到超级项目中记录的提交上。

  **merge**

  the commit recorded in the superproject will be merged into the current branch in the submodule.

   在子模块中，将超级项目中记录的提交合并到当前分支上。

  The following *update* procedures are only available via the `submodule.<name>.update` configuration variable:

  以下更新程序仅可通过submodule.<name>.update配置变量使用：

  **custom command**

  arbitrary shell command that takes a single argument (the sha1 of the commit recorded in the superproject) is executed. When `submodule.<name>.update` is set to *!command*, the remainder after the exclamation mark is the custom command.

  自定义命令 - 执行一个任意的shell命令，它需要一个参数（超级项目中记录的提交的SHA1）。当submodule.<name>.update被设置为!command时，感叹号后面的内容是自定义命令。

  **none**

  the submodule is not updated.

  none - 不更新子模块。

  If the submodule is not yet initialized, and you just want to use the setting as stored in `.gitmodules`, you can automatically initialize the submodule with the `--init` option.

  如果子模块尚未初始化，并且您只想使用存储在.gitmodules中的设置，您可以使用--init选项自动初始化子模块。

  If `--recursive` is specified, this command will recurse into the registered submodules, and update any nested submodules within.

  如果指定了--recursive，则此命令将递归到已注册的子模块中，并更新其中的任何嵌套子模块。

  If `--filter <filter spec>` is specified, the given partial clone filter will be applied to the submodule. See [git-rev-list[1]](../git-rev-list) for details on filter specifications.

  如果指定了--filter <filter spec>，则会将给定的部分克隆筛选器应用于子模块。有关筛选器说明，请参见git-rev-list[1]。

- set-branch (-b|--branch) <branch> [--] <path>

- set-branch (-d|--default) [--] <path>

  Sets the default remote tracking branch for the submodule. The `--branch` option allows the remote branch to be specified. The `--default` option removes the submodule.<name>.branch configuration key, which causes the tracking branch to default to the remote *HEAD*.

  为子模块设置默认的远程跟踪分支。--branch选项允许指定远程分支。--default选项将删除submodule.<name>.branch配置键，导致跟踪分支默认为远程HEAD。

- set-url [--] <path> <newurl>

  Sets the URL of the specified submodule to <newurl>. Then, it will automatically synchronize the submodule’s new remote URL configuration.

  将指定子模块的 URL 设置为 <newurl>。然后，它将自动同步子模块的新远程 URL 配置。

- summary [--cached|--files] [(-n|--summary-limit) <n>] [commit] [--] [<path>…]

  Show commit summary between the given commit (defaults to HEAD) and working tree/index. For a submodule in question, a series of commits in the submodule between the given super project commit and the index or working tree (switched by `--cached`) are shown. If the option `--files` is given, show the series of commits in the submodule between the index of the super project and the working tree of the submodule (this option doesn’t allow to use the `--cached` option or to provide an explicit commit).

  显示给定提交（默认为 HEAD）和工作树/索引之间的提交摘要。对于一个子模块，它会显示在给定的超级项目提交和索引或工作树之间的一系列提交。如果给定 --files 选项，则显示在超级项目的索引和子模块的工作树之间的一系列提交（此选项不允许使用 --cached 选项或提供显式提交）。

  Using the `--submodule=log` option with [git-diff[1]](../git-diff) will provide that information too.

  使用 git-diff[1] 中的 --submodule=log 选项也会提供这些信息。

- foreach [--recursive] <command>

  Evaluates an arbitrary shell command in each checked out submodule. The command has access to the variables $name, $sm_path, $displaypath, $sha1 and $toplevel: $name is the name of the relevant submodule section in `.gitmodules`, $sm_path is the path of the submodule as recorded in the immediate superproject, $displaypath contains the relative path from the current working directory to the submodules root directory, $sha1 is the commit as recorded in the immediate superproject, and $toplevel is the absolute path to the top-level of the immediate superproject. Note that to avoid conflicts with *$PATH* on Windows, the *$path* variable is now a deprecated synonym of *$sm_path* variable. Any submodules defined in the superproject but not checked out are ignored by this command. Unless given `--quiet`, foreach prints the name of each submodule before evaluating the command. If `--recursive` is given, submodules are traversed recursively (i.e. the given shell command is evaluated in nested submodules as well). A non-zero return from the command in any submodule causes the processing to terminate. This can be overridden by adding *|| :* to the end of the command.

  As an example, the command below will show the path and currently checked out commit for each submodule:`git submodule foreach 'echo $sm_path `git rev-parse HEAD`'`

  在每个已检出的子模块中评估任意 shell 命令。该命令可以访问变量 $name、$sm_path、$displaypath、$sha1 和 $toplevel：$name 是.gitmodules 中相关子模块部分的名称，$sm_path 是在立即超级项目中记录的子模块路径，$displaypath 包含从当前工作目录到子模块根目录的相对路径，$sha1 是在立即超级项目中记录的提交，$toplevel 是立即超级项目的顶层绝对路径。请注意，为避免在 Windows 上与 $PATH 冲突，$path 变量现在是 $sm_path 变量的弃用同义词。此命令忽略超级项目中定义但未检出的任何子模块。除非给出 --quiet，foreach 在评估命令之前会打印每个子模块的名称。如果给定了 --recursive，则会递归遍历子模块（即在嵌套子模块中评估给定的 shell 命令）。在任何子模块中，命令返回非零值都会导致处理终止。可以通过在命令末尾添加 || : 来覆盖此行为。

  例如，下面的命令将显示每个子模块的路径和当前检出的提交：

  git submodule foreach 'echo $sm_path `git rev-parse HEAD`'

- sync [--recursive] [--] [<path>…]

  Synchronizes submodules' remote URL configuration setting to the value specified in `.gitmodules`. It will only affect those submodules which already have a URL entry in .git/config (that is the case when they are initialized or freshly added). This is useful when submodule URLs change upstream and you need to update your local repositories accordingly.`git submodule sync` synchronizes all submodules while `git submodule sync -- A` synchronizes submodule "A" only.If `--recursive` is specified, this command will recurse into the registered submodules, and sync any nested submodules within.

  将子模块的远程 URL 配置与 .gitmodules 中指定的值同步。它仅会影响已经在 .git/config 中有 URL 条目的子模块（在初始化或新增时会出现这种情况）。当子模块的 URL 在上游发生更改时，您需要相应地更新本地仓库时，这很有用。

- absorbgitdirs

  If a git directory of a submodule is inside the submodule, move the git directory of the submodule into its superproject’s `$GIT_DIR/modules` path and then connect the git directory and its working directory by setting the `core.worktree` and adding a .git file pointing to the git directory embedded in the superprojects git directory.A repository that was cloned independently and later added as a submodule or old setups have the submodules git directory inside the submodule instead of embedded into the superprojects git directory.This command is recursive by default.

## 选项

- -q

- --quiet

  Only print error messages.

- --progress

  This option is only valid for add and update commands. Progress status is reported on the standard error stream by default when it is attached to a terminal, unless -q is specified. This flag forces progress status even if the standard error stream is not directed to a terminal.

- --all

  This option is only valid for the deinit command. Unregister all submodules in the working tree.

- -b <branch>

- --branch <branch>

  Branch of repository to add as submodule. The name of the branch is recorded as `submodule.<name>.branch` in `.gitmodules` for `update --remote`. A special value of `.` is used to indicate that the name of the branch in the submodule should be the same name as the current branch in the current repository. If the option is not specified, it defaults to the remote *HEAD*.

- -f

- --force

  This option is only valid for add, deinit and update commands. When running add, allow adding an otherwise ignored submodule path. When running deinit the submodule working trees will be removed even if they contain local changes. When running update (only effective with the checkout procedure), throw away local changes in submodules when switching to a different commit; and always run a checkout operation in the submodule, even if the commit listed in the index of the containing repository matches the commit checked out in the submodule.

- --cached

  This option is only valid for status and summary commands. These commands typically use the commit found in the submodule HEAD, but with this option, the commit stored in the index is used instead.

- --files

  This option is only valid for the summary command. This command compares the commit in the index with that in the submodule HEAD when this option is used.

- -n

- --summary-limit

  This option is only valid for the summary command. Limit the summary size (number of commits shown in total). Giving 0 will disable the summary; a negative number means unlimited (the default). This limit only applies to modified submodules. The size is always limited to 1 for added/deleted/typechanged submodules.

- --remote

  This option is only valid for the update command. Instead of using the superproject’s recorded SHA-1 to update the submodule, use the status of the submodule’s remote-tracking branch. The remote used is branch’s remote (`branch.<name>.remote`), defaulting to `origin`. The remote branch used defaults to the remote `HEAD`, but the branch name may be overridden by setting the `submodule.<name>.branch` option in either `.gitmodules` or `.git/config` (with `.git/config` taking precedence).This works for any of the supported update procedures (`--checkout`, `--rebase`, etc.). The only change is the source of the target SHA-1. For example, `submodule update --remote --merge` will merge upstream submodule changes into the submodules, while `submodule update --merge` will merge superproject gitlink changes into the submodules.In order to ensure a current tracking branch state, `update --remote` fetches the submodule’s remote repository before calculating the SHA-1. If you don’t want to fetch, you should use `submodule update --remote --no-fetch`.Use this option to integrate changes from the upstream subproject with your submodule’s current HEAD. Alternatively, you can run `git pull` from the submodule, which is equivalent except for the remote branch name: `update --remote` uses the default upstream repository and `submodule.<name>.branch`, while `git pull` uses the submodule’s `branch.<name>.merge`. Prefer `submodule.<name>.branch` if you want to distribute the default upstream branch with the superproject and `branch.<name>.merge` if you want a more native feel while working in the submodule itself.

- -N

- --no-fetch

  This option is only valid for the update command. Don’t fetch new objects from the remote site.

- --checkout

  This option is only valid for the update command. Checkout the commit recorded in the superproject on a detached HEAD in the submodule. This is the default behavior, the main use of this option is to override `submodule.$name.update` when set to a value other than `checkout`. If the key `submodule.$name.update` is either not explicitly set or set to `checkout`, this option is implicit.

- --merge

  This option is only valid for the update command. Merge the commit recorded in the superproject into the current branch of the submodule. If this option is given, the submodule’s HEAD will not be detached. If a merge failure prevents this process, you will have to resolve the resulting conflicts within the submodule with the usual conflict resolution tools. If the key `submodule.$name.update` is set to `merge`, this option is implicit.

- --rebase

  This option is only valid for the update command. Rebase the current branch onto the commit recorded in the superproject. If this option is given, the submodule’s HEAD will not be detached. If a merge failure prevents this process, you will have to resolve these failures with [git-rebase[1]](../git-rebase). If the key `submodule.$name.update` is set to `rebase`, this option is implicit.

- --init

  This option is only valid for the update command. Initialize all submodules for which "git submodule init" has not been called so far before updating.

- --name

  This option is only valid for the add command. It sets the submodule’s name to the given string instead of defaulting to its path. The name must be valid as a directory name and may not end with a */*.

- --reference <repository>

  This option is only valid for add and update commands. These commands sometimes need to clone a remote repository. In this case, this option will be passed to the [git-clone[1]](../git-clone) command.**NOTE**: Do **not** use this option unless you have read the note for [git-clone[1]](../git-clone)'s `--reference`, `--shared`, and `--dissociate` options carefully.

- --dissociate

  This option is only valid for add and update commands. These commands sometimes need to clone a remote repository. In this case, this option will be passed to the [git-clone[1]](../git-clone) command.**NOTE**: see the NOTE for the `--reference` option.

- --recursive

  This option is only valid for foreach, update, status and sync commands. Traverse submodules recursively. The operation is performed not only in the submodules of the current repo, but also in any nested submodules inside those submodules (and so on).

- --depth

  This option is valid for add and update commands. Create a *shallow* clone with a history truncated to the specified number of revisions. See [git-clone[1]](../git-clone)

- --[no-]recommend-shallow

  This option is only valid for the update command. The initial clone of a submodule will use the recommended `submodule.<name>.shallow` as provided by the `.gitmodules` file by default. To ignore the suggestions use `--no-recommend-shallow`.

- -j <n>

- --jobs <n>

  This option is only valid for the update command. Clone new submodules in parallel with as many jobs. Defaults to the `submodule.fetchJobs` option.

- --[no-]single-branch

  This option is only valid for the update command. Clone only one branch during update: HEAD or one specified by --branch.

- <path>…

  Paths to submodule(s). When specified this will restrict the command to only operate on the submodules found at the specified paths. (This argument is required with add).

## FILES

When initializing submodules, a `.gitmodules` file in the top-level directory of the containing repository is used to find the url of each submodule. This file should be formatted in the same way as `$GIT_DIR/config`. The key to each submodule url is "submodule.$name.url". See [gitmodules[5]](../../5/gitmodules) for details.

## 另请参阅

[gitsubmodules[7]](../../7/gitsubmodules), [gitmodules[5]](../../5/gitmodules).

## GIT

  这是[git[1]](../../Git)工具集中的一部分。