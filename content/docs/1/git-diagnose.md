https://git-scm.com/docs/git-diagnose

## 名称

git-diagnose - Generate a zip archive of diagnostic information

## 概述

```
git diagnose [(-o | --output-directory) <path>] [(-s | --suffix) <format>]
	       [--mode=<mode>]
```

## 描述

Collects detailed information about the user’s machine, Git client, and repository state and packages that information into a zip archive. The generated archive can then, for example, be shared with the Git mailing list to help debug an issue or serve as a reference for independent debugging.

By default, the following information is captured in the archive:

- *git version --build-options*
- The path to the repository root
- The available disk space on the filesystem
- The name and size of each packfile, including those in alternate object stores
- The total count of loose objects, as well as counts broken down by `.git/objects` subdirectory

Additional information can be collected by selecting a different diagnostic mode using the `--mode` option.

This tool differs from [git-bugreport[1]](../git-bugreport) in that it collects much more detailed information with a greater focus on reporting the size and data shape of repository contents.

## 选项

- -o <path>

- --output-directory <path>

  Place the resulting diagnostics archive in `<path>` instead of the current directory.

- -s <format>

- --suffix <format>

  Specify an alternate suffix for the diagnostics archive name, to create a file named *git-diagnostics-<formatted suffix>*. This should take the form of a strftime(3) format string; the current local time will be used.

- `--mode=(stats|all)`

  Specify the type of diagnostics that should be collected. The default behavior of *git diagnose* is equivalent to `--mode=stats`.The `--mode=all` option collects everything included in `--mode=stats`, as well as copies of `.git`, `.git/hooks`, `.git/info`, `.git/logs`, and `.git/objects/info` directories. This additional information may be sensitive, as it can be used to reconstruct the full contents of the diagnosed repository. Users should exercise caution when sharing an archive generated with `--mode=all`.

## GIT

  这是[git[1]](../../Git)工具集中的一部分。