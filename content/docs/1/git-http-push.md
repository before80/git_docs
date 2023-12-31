https://git-scm.com/docs/git-http-push

## 名称

git-http-push - Push objects over HTTP/DAV to another repository

## 概述

```
git http-push [--all] [--dry-run] [--force] [--verbose] <URL> <ref> [<ref>…]
```

## 描述

Sends missing objects to remote repository, and updates the remote branch.

**NOTE**: This command is temporarily disabled if your libcurl is older than 7.16, as the combination has been reported not to work and sometimes corrupts repository.

## 选项

- `--all`

  Do not assume that the remote repository is complete in its current state, and verify all objects in the entire local ref’s history exist in the remote repository.

- `--force`

  Usually, the command refuses to update a remote ref that is not an ancestor of the local ref used to overwrite it. This flag disables the check. What this means is that the remote repository can lose commits; use it with care.

- `--dry-run`

  Do everything except actually send the updates.

- `--verbose`

  Report the list of objects being walked locally and the list of objects successfully sent to the remote repository.

- -d

- -D

  Remove <ref> from remote repository. The specified branch cannot be the remote HEAD. If -d is specified the following other conditions must also be met:Remote HEAD must resolve to an object that exists locallySpecified branch resolves to an object that exists locallySpecified branch is an ancestor of the remote HEAD

- <ref>…

  The remote refs to update.

## SPECIFYING THE REFS

A *<ref>* specification can be either a single pattern, or a pair of such patterns separated by a colon ":" (this means that a ref name cannot have a colon in it). A single pattern *<name>* is just a shorthand for *<name>:<name>*.

Each pattern pair *<src>:<dst>* consists of the source side (before the colon) and the destination side (after the colon). The ref to be pushed is determined by finding a match that matches the source side, and where it is pushed is determined by using the destination side.

- It is an error if *<src>* does not match exactly one of the local refs.
- If *<dst>* does not match any remote ref, either
  - it has to start with "refs/"; <dst> is used as the destination literally in this case.
  - <src> == <dst> and the ref that matched the <src> must not exist in the set of remote refs; the ref matched <src> locally is used as the name of the destination.

Without `--force`, the <src> ref is stored at the remote only if <dst> does not exist, or <dst> is a proper subset (i.e. an ancestor) of <src>. This check, known as "fast-forward check", is performed in order to avoid accidentally overwriting the remote ref and lose other peoples' commits from there.

With `--force`, the fast-forward check is disabled for all refs.

Optionally, a <ref> parameter can be prefixed with a plus *+* sign to disable the fast-forward check only on that ref.

## GIT

  这是[git[1]](../../Git)工具集中的一部分。