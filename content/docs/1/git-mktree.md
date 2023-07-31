https://git-scm.com/docs/git-mktree

## 名称

git-mktree - Build a tree-object from ls-tree formatted text

## 概述

```
git mktree [-z] [--missing] [--batch]
```

## 描述

Reads standard input in non-recursive `ls-tree` output format, and creates a tree object. The order of the tree entries is normalized by mktree so pre-sorting the input is not required. The object name of the tree object built is written to the standard output.

## 选项

- -z

  Read the NUL-terminated `ls-tree -z` output instead.

- `--missing`

  Allow missing objects. The default behaviour (without this option) is to verify that each tree entry’s sha1 identifies an existing object. This option has no effect on the treatment of gitlink entries (aka "submodules") which are always allowed to be missing.

- `--batch`

  Allow building of more than one tree object before exiting. Each tree is separated by a single blank line. The final new-line is optional. Note - if the `-z` option is used, lines are terminated with NUL.

## GIT

  这是[git[1]](../../Git)工具集中的一部分。