https://git-scm.com/docs/git-for-each-repo

## 名称

git-for-each-repo - Run a Git command on a list of repositories

## 概述

```
git for-each-repo --config=<config> [--] <arguments>
```

## 描述

Run a Git command on a list of repositories. The arguments after the known options or `--` indicator are used as the arguments for the Git subprocess.

THIS COMMAND IS EXPERIMENTAL. THE BEHAVIOR MAY CHANGE.

For example, we could run maintenance on each of a list of repositories stored in a `maintenance.repo` config variable using

```
git for-each-repo --config=maintenance.repo maintenance run
```

This will run `git -C <repo> maintenance run` for each value `<repo>` in the multi-valued config variable `maintenance.repo`.

## 选项

- `--config=<config>`

  Use the given config variable as a multi-valued list storing absolute path names. Iterate on that list of paths to run the given arguments.These config values are loaded from system, global, and local Git config, as available. If `git for-each-repo` is run in a directory that is not a Git repository, then only the system and global config is used.

## SUBPROCESS BEHAVIOR

If any `git -C <repo> <arguments>` subprocess returns a non-zero exit code, then the `git for-each-repo` process returns that exit code without running more subprocesses.

Each `git -C <repo> <arguments>` subprocess inherits the standard file descriptors `stdin`, `stdout`, and `stderr`.

## GIT

  这是[git[1]](../../Git)工具集中的一部分。