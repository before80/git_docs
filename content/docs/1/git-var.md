https://git-scm.com/docs/git-var

## 名称

git-var - Show a Git logical variable

## 概述

```
git var (-l | <variable>)
```

## 描述

Prints a Git logical variable. Exits with code 1 if the variable has no value.

## 选项

- -l

  Cause the logical variables to be listed. In addition, all the variables of the Git configuration file .git/config are listed as well. (However, the configuration variables listing functionality is deprecated in favor of `git config -l`.)

## 示例

```
$ git var GIT_AUTHOR_IDENT
Eric W. Biederman <ebiederm@lnxi.com> 1121223278 -0600
```

## VARIABLES

- GIT_AUTHOR_IDENT

  The author of a piece of code.

- GIT_COMMITTER_IDENT

  The person who put a piece of code into Git.

- GIT_EDITOR

  Text editor for use by Git commands. The value is meant to be interpreted by the shell when it is used. Examples: `~/bin/vi`, `$SOME_ENVIRONMENT_VARIABLE`, `"C:\Program Files\Vim\gvim.exe" --nofork`. The order of preference is the `$GIT_EDITOR` environment variable, then `core.editor` configuration, then `$VISUAL`, then `$EDITOR`, and then the default chosen at compile time, which is usually *vi*.

- GIT_SEQUENCE_EDITOR

  Text editor used to edit the *todo* file while running `git rebase -i`. Like `GIT_EDITOR`, the value is meant to be interpreted by the shell when it is used. The order of preference is the `$GIT_SEQUENCE_EDITOR` environment variable, then `sequence.editor` configuration, and then the value of `git var GIT_EDITOR`.

- GIT_PAGER

  Text viewer for use by Git commands (e.g., *less*). The value is meant to be interpreted by the shell. The order of preference is the `$GIT_PAGER` environment variable, then `core.pager` configuration, then `$PAGER`, and then the default chosen at compile time (usually *less*).

- GIT_DEFAULT_BRANCH

  The name of the first branch created in newly initialized repositories.

## 另请参阅

[git-commit-tree[1]](../git-commit-tree) [git-tag[1]](../git-tag) [git-config[1]](../git-config)

## GIT

  这是[git[1]](../../Git)工具集中的一部分。