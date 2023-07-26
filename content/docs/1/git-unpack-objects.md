https://git-scm.com/docs/git-unpack-objects

## 名称

git-unpack-objects - Unpack objects from a packed archive

## 概述

```
git unpack-objects [-n] [-q] [-r] [--strict]
```

## 描述

Read a packed archive (.pack) from the standard input, expanding the objects contained within and writing them into the repository in "loose" (one object per file) format.

Objects that already exist in the repository will **not** be unpacked from the packfile. Therefore, nothing will be unpacked if you use this command on a packfile that exists within the target repository.

See [git-repack[1]](../git-repack) for options to generate new packs and replace existing ones.

## 选项

- -n

  Dry run. Check the pack file without actually unpacking the objects.

- -q

  The command usually shows percentage progress. This flag suppresses it.

- -r

  When unpacking a corrupt packfile, the command dies at the first corruption. This flag tells it to keep going and make the best effort to recover as many objects as possible.

- --strict

  Don’t write objects with broken content or links.

- --max-input-size=<size>

  Die, if the pack is larger than <size>.

## GIT

  这是[git[1]](../../Git)工具集中的一部分。