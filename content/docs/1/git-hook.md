https://git-scm.com/docs/git-hook

## 名称

git-hook - Run git hooks

## 概述

```
git hook run [--ignore-missing] [--to-stdin=<path>] <hook-name> [-- <hook-args>]
```

## 描述

A command interface to running git hooks (see [githooks[5]](../../5/githooks)), for use by other scripted git commands.

## SUBCOMMANDS

- run

  Run the `<hook-name>` hook. See [githooks[5]](../../5/githooks) for supported hook names.Any positional arguments to the hook should be passed after a mandatory `--` (or `--end-of-options`, see [gitcli[7]](../../7/gitcli)). See [githooks[5]](../../5/githooks) for arguments hooks might expect (if any).

## 选项

- `--to-stdin`

  For "run"; Specify a file which will be streamed into the hook’s stdin. The hook will receive the entire file from beginning to EOF.

- `--ignore-missing`

  Ignore any missing hook by quietly returning zero. Used for tools that want to do a blind one-shot run of a hook that may or may not be present.

## 另请参阅

[githooks[5]](../../5/githooks)

## GIT

  这是[git[1]](../../Git)工具集中的一部分。