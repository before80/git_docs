https://git-scm.com/docs/git-check-mailmap

## 名称

git-check-mailmap - Show canonical names and email addresses of contacts

## 概述

```
git check-mailmap [<options>] <contact>…
```

## 描述

For each “Name <user@host>” or “<user@host>” from the command-line or standard input (when using `--stdin`), look up the person’s canonical name and email address (see "Mapping Authors" below). If found, print them; otherwise print the input as-is.

## 选项

- --stdin

  Read contacts, one per line, from the standard input after exhausting contacts provided on the command-line.

## OUTPUT

For each contact, a single line is output, terminated by a newline. If the name is provided or known to the *mailmap*, “Name <user@host>” is printed; otherwise only “<user@host>” is printed.

## 配置

See `mailmap.file` and `mailmap.blob` in [git-config[1]](../git-config) for how to specify a custom `.mailmap` target file or object.

## MAPPING AUTHORS

See [gitmailmap[5]](../../5/gitmailmap).

## GIT

  这是[git[1]](../../Git)工具集中的一部分。