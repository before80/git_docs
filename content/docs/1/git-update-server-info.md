https://git-scm.com/docs/git-update-server-info

## 名称

git-update-server-info - Update auxiliary info file to help dumb servers

## 概述

```
git update-server-info [-f | --force]
```

## 描述

A dumb server that does not do on-the-fly pack generations must have some auxiliary information files in $GIT_DIR/info and $GIT_OBJECT_DIRECTORY/info directories to help clients discover what references and packs the server has. This command generates such auxiliary files.

## 选项

- -f

- --force

  update the info files from scratch.

## OUTPUT

Currently the command updates the following files. Please see [gitrepository-layout[5]](../../5/gitrepository-layout) for description of what they are for:

- objects/info/packs
- info/refs

## GIT

  这是[git[1]](../../Git)工具集中的一部分。