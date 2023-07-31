https://git-scm.com/docs/git-http-fetch

## 名称

git-http-fetch - Download from a remote Git repository via HTTP

## 概述

```
git http-fetch [-c] [-t] [-a] [-d] [-v] [-w <filename>] [--recover] [--stdin | --packfile=<hash> | <commit>] <URL>
```

## 描述

Downloads a remote Git repository via HTTP.

This command always gets all objects. Historically, there were three options `-a`, `-c` and `-t` for choosing which objects to download. They are now silently ignored.

## 选项

- commit-id

  Either the hash or the filename under [URL]/refs/ to pull.

- -a, -c, -t

  These options are ignored for historical reasons.

- -v

  Report what is downloaded.

- -w <filename>

  Writes the commit-id into the filename under $GIT_DIR/refs/<filename> on the local end after the transfer is complete.

- `--stdin`

  Instead of a commit id on the command line (which is not expected in this case), *git http-fetch* expects lines on stdin in the format`<commit-id>['\t'<filename-as-in--w>]`

- `--packfile=<hash>`

  For internal use only. Instead of a commit id on the command line (which is not expected in this case), *git http-fetch* fetches the packfile directly at the given URL and uses index-pack to generate corresponding .idx and .keep files. The hash is used to determine the name of the temporary file and is arbitrary. The output of index-pack is printed to stdout. Requires --index-pack-args.

- `--index-pack-args=<args>`

  For internal use only. The command to run on the contents of the downloaded pack. Arguments are URL-encoded separated by spaces.

- `--recover`

  Verify that everything reachable from target is fetched. Used after an earlier fetch is interrupted.

## GIT

  这是[git[1]](../../Git)工具集中的一部分。