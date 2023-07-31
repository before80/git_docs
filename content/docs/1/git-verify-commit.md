https://git-scm.com/docs/git-verify-commit

## 名称

git-verify-commit - Check the GPG signature of commits

## 概述

```
git verify-commit [-v | --verbose] [--raw] <commit>…
```

## 描述

Validates the GPG signature created by *git commit -S*.

## 选项

- `--raw`

  Print the raw gpg status output to standard error instead of the normal human-readable output.

- -v

- `--verbose`

  Print the contents of the commit object before validating it.

- <commit>…

  SHA-1 identifiers of Git commit objects.

## GIT

  这是[git[1]](../../Git)工具集中的一部分。