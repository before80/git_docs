https://git-scm.com/docs/git-verify-pack

## 名称

git-verify-pack - Validate packed Git archive files

## 概述

```
git verify-pack [-v | --verbose] [-s | --stat-only] [--] <pack>.idx…
```

## 描述

Reads given idx file for packed Git archive created with the *git pack-objects* command and verifies idx file and the corresponding pack file.

## 选项

- <pack>.idx …

  The idx files to verify.

- -v

- `--verbose`

  After verifying the pack, show list of objects contained in the pack and a histogram of delta chain length.

- -s

- `--stat-only`

  Do not verify the pack contents; only show the histogram of delta chain length. With `--verbose`, list of objects is also shown.

- --

  Do not interpret any more arguments as options.

## OUTPUT FORMAT

When specifying the -v option the format used is:

```
SHA-1 type size size-in-packfile offset-in-packfile
```

for objects that are not deltified in the pack, and

```
SHA-1 type size size-in-packfile offset-in-packfile depth base-SHA-1
```

for objects that are deltified.

## GIT

  这是[git[1]](../../Git)工具集中的一部分。