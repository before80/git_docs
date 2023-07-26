https://git-scm.com/docs/git-verify-tag

## 名称

git-verify-tag - Check the GPG signature of tags

## 概述

```
git verify-tag [-v | --verbose] [--format=<format>] [--raw] <tag>…
```

## 描述

Validates the gpg signature created by *git tag*.

## 选项

- --raw

  Print the raw gpg status output to standard error instead of the normal human-readable output.

- -v

- --verbose

  Print the contents of the tag object before validating it.

- <tag>…

  SHA-1 identifiers of Git tag objects.

## GIT

  这是[git[1]](../../Git)工具集中的一部分。