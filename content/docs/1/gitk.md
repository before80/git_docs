+++
title = "gitk"
weight = 30
type = "docs"
date = 2023-05-08T15:39:23+08:00
description = ""
isCJKLanguage = true
draft = false

+++

# gitk

https://git-scm.com/docs/gitk

## 名称

gitk - The Git repository browser

## 概述

```
gitk [<options>] [<revision range>] [--] [<path>…]
```

## 描述

Displays changes in a repository or a selected set of commits. This includes visualizing the commit graph, showing information related to each commit, and the files in the trees of each revision.

## 选项

To control which revisions to show, gitk supports most options applicable to the *git rev-list* command. It also supports a few options applicable to the *git diff-** commands to control how the changes each commit introduces are shown. Finally, it supports some gitk-specific options.

gitk generally only understands options with arguments in the *sticked* form (see [gitcli[7]](../../7/gitcli)) due to limitations in the command-line parser.

### rev-list options and arguments

This manual page describes only the most frequently used options. See [git-rev-list[1]](../git-rev-list) for a complete list.

- `--all`

  Show all refs (branches, tags, etc.).

- `--branches[=<pattern>]`

- `--tags[=<pattern>]`

- `--remotes[=<pattern>]`

  Pretend as if all the branches (tags, remote branches, resp.) are listed on the command line as *<commit>*. If *<pattern>* is given, limit refs to ones matching given shell glob. If pattern lacks *?*, ***, or *[*, */** at the end is implied.

- `--since=<date>`

  Show commits more recent than a specific date.

- `--until=<date>`

  Show commits older than a specific date.

- `--date-order`

  Sort commits by date when possible.

- `--merge`

  After an attempt to merge stops with conflicts, show the commits on the history between two branches (i.e. the HEAD and the MERGE_HEAD) that modify the conflicted files and do not exist on all the heads being merged.

- `--left-right`

  Mark which side of a symmetric difference a commit is reachable from. Commits from the left side are prefixed with a `<` symbol and those from the right with a `>` symbol.

- `--full-history`

  When filtering history with *<path>…*, does not prune some history. (See "History simplification" in [git-log[1]](../git-log) for a more detailed explanation.)

- `--simplify-merges`

  Additional option to `--full-history` to remove some needless merges from the resulting history, as there are no selected commits contributing to this merge. (See "History simplification" in [git-log[1]](../git-log) for a more detailed explanation.)

- `--ancestry-path`

  When given a range of commits to display (e.g. *commit1..commit2* or *commit2 ^commit1*), only display commits that exist directly on the ancestry chain between the *commit1* and *commit2*, i.e. commits that are both descendants of *commit1*, and ancestors of *commit2*. (See "History simplification" in [git-log[1]](../git-log) for a more detailed explanation.)

- -L<start>,<end>:<file>

- -L:<funcname>:<file>

  Trace the evolution of the line range given by *<start>,<end>*, or by the function name regex *<funcname>*, within the *<file>*. You may not give any pathspec limiters. This is currently limited to a walk starting from a single revision, i.e., you may only give zero or one positive revision arguments, and *<start>* and *<end>* (or *<funcname>*) must exist in the starting revision. You can specify this option more than once. Implies `--patch`. Patch output can be suppressed using `--no-patch`, but other diff formats (namely `--raw`, `--numstat`, `--shortstat`, `--dirstat`, `--summary`, `--name-only`, `--name-status`, `--check`) are not currently implemented.*<start>* and *<end>* can take one of these forms:numberIf *<start>* or *<end>* is a number, it specifies an absolute line number (lines count from 1).`/regex/`This form will use the first line matching the given POSIX regex. If *<start>* is a regex, it will search from the end of the previous `-L` range, if any, otherwise from the start of file. If *<start>* is `^/regex/`, it will search from the start of file. If *<end>* is a regex, it will search starting at the line given by *<start>*.+offset or -offsetThis is only valid for *<end>* and will specify a number of lines before or after the line given by *<start>*.If `:<funcname>` is given in place of *<start>* and *<end>*, it is a regular expression that denotes the range from the first funcname line that matches *<funcname>*, up to the next funcname line. `:<funcname>` searches from the end of the previous `-L` range, if any, otherwise from the start of file. `^:<funcname>` searches from the start of file. The function names are determined in the same way as `git diff` works out patch hunk headers (see *Defining a custom hunk-header* in [gitattributes[5]](../../5/gitattributes)).

- <revision range>

  Limit the revisions to show. This can be either a single revision meaning show from the given revision and back, or it can be a range in the form "*<from>*..*<to>*" to show all revisions between *<from>* and back to *<to>*. Note, more advanced revision selection can be applied. For a more complete list of ways to spell object names, see [gitrevisions[7]](../../7/gitrevisions).

- <path>…

  Limit commits to the ones touching files in the given paths. Note, to avoid ambiguity with respect to revision names use "--" to separate the paths from any preceding options.

### gitk-specific options

- `--argscmd=<command>`

  Command to be run each time gitk has to determine the revision range to show. The command is expected to print on its standard output a list of additional revisions to be shown, one per line. Use this instead of explicitly specifying a *<revision range>* if the set of commits to show may vary between refreshes.

- `--select-commit=<ref>`

  Select the specified commit after loading the graph. Default behavior is equivalent to specifying *--select-commit=HEAD*.

## 示例

- gitk v2.6.12.. include/scsi drivers/scsi

  Show the changes since version *v2.6.12* that changed any file in the include/scsi or drivers/scsi subdirectories

- gitk --since="2 weeks ago" -- gitk

  Show the changes during the last two weeks to the file *gitk*. The "--" is necessary to avoid confusion with the **branch** named *gitk*

- gitk --max-count=100 --all -- Makefile

  Show at most 100 changes made to the file *Makefile*. Instead of only looking for changes in the current branch look in all branches.

## Files

User configuration and preferences are stored at:

- `$XDG_CONFIG_HOME/git/gitk` if it exists, otherwise
- `$HOME/.gitk` if it exists

If neither of the above exist then `$XDG_CONFIG_HOME/git/gitk` is created and used by default. If *$XDG_CONFIG_HOME* is not set it defaults to `$HOME/.config` in all cases.

## History

Gitk was the first graphical repository browser. It’s written in tcl/tk.

*gitk* is actually maintained as an independent project, but stable versions are distributed as part of the Git suite for the convenience of end users.

gitk-git/ comes from Paul Mackerras’s gitk project:

```
git://ozlabs.org/~paulus/gitk
```

## 另请参阅

- *qgit(1)*

  A repository browser written in C++ using Qt.

- *tig(1)*

  A minimal repository browser and Git tool output highlighter written in C using Ncurses.

## GIT

  这是[git[1]](../../Git)工具集中的一部分。