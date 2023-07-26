https://git-scm.com/docs/git-prune-packed

## 名称

git-prune-packed - Remove extra objects that are already in pack files

## 概述

```
git prune-packed [-n | --dry-run] [-q | --quiet]
```

## 描述

This program searches the `$GIT_OBJECT_DIRECTORY` for all objects that currently exist in a pack file as well as the independent object directories.

All such extra objects are removed.

A pack is a collection of objects, individually compressed, with delta compression applied, stored in a single file, with an associated index file.

Packs are used to reduce the load on mirror systems, backup engines, disk storage, etc.

## 选项

- -n

- --dry-run

  Don’t actually remove any objects, only show those that would have been removed.

- -q

- --quiet

  Squelch the progress indicator.

## 另请参阅

[git-pack-objects[1]](../git-pack-objects) [git-repack[1]](../git-repack)

## GIT

  这是[git[1]](../../Git)工具集中的一部分。