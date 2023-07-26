https://git-scm.com/docs/git-pack-redundant

## 名称

git-pack-redundant - Find redundant pack files

## 概述

```
git pack-redundant [--verbose] [--alt-odb] (--all | <pack-filename>…)
```

## 描述

This program computes which packs in your repository are redundant. The output is suitable for piping to `xargs rm` if you are in the root of the repository.

*git pack-redundant* accepts a list of objects on standard input. Any objects given will be ignored when checking which packs are required. This makes the following command useful when wanting to remove packs which contain unreachable objects.

git fsck --full --unreachable | cut -d ' ' -f3 | \ git pack-redundant --all | xargs rm

## 选项

- --all

  Processes all packs. Any filenames on the command line are ignored.

- --alt-odb

  Don’t require objects present in packs from alternate object database (odb) directories to be present in local packs.

- --verbose

  Outputs some statistics to stderr. Has a small performance penalty.

## 另请参阅

[git-pack-objects[1]](../git-pack-objects) [git-repack[1]](../git-repack) [git-prune-packed[1]](../git-prune-packed)

## GIT

  这是[git[1]](../../Git)工具集中的一部分。