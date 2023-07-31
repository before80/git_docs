+++
title = "git-add"
weight = 30
type = "docs"
date = 2023-05-08T15:39:23+08:00
description = ""
isCJKLanguage = true
draft = true

+++

# git-add

https://git-scm.com/docs/git-add

version 2.41.0

## 名称

git-add - Add file contents to the index

​	git-add - 将文件内容添加到索引（即暂存区 staging area）

## 概述

```
git add [--verbose | -v] [--dry-run | -n] [--force | -f] [--interactive | -i] [--patch | -p]
	  [--edit | -e] [--[no-]all | --[no-]ignore-removal | [--update | -u]] [--sparse]
	  [--intent-to-add | -N] [--refresh] [--ignore-errors] [--ignore-missing] [--renormalize]
	  [--chmod=(+|-)x] [--pathspec-from-file=<file> [--pathspec-file-nul]]
	  [--] [<pathspec>…]
```

## 描述

This command updates the index using the current content found in the working tree, to prepare the content staged for the next commit. It typically adds the current content of existing paths as a whole, but with some options it can also be used to add content with only part of the changes made to the working tree files applied, or remove paths that do not exist in the working tree anymore.

​	此命令使用当前工作树中的内容更新索引，以准备将内容暂存到下一次提交中。它通常将现有路径的当前内容作为整体添加，但使用一些选项，还可以添加仅应用于工作树文件的部分更改的内容，或删除工作树中不再存在的路径。

The "index" holds a snapshot of the content of the working tree, and it is this snapshot that is taken as the contents of the next commit. Thus after making any changes to the working tree, and before running the commit command, you must use the `add` command to add any new or modified files to the index.

​	“索引”保存工作树的内容快照，并且正是该快照作为下一次提交的内容。因此，在对工作树进行任何更改并在运行提交命令之前，必须使用 `add` 命令将任何新文件或已修改的文件添加到索引中。

This command can be performed multiple times before a commit. It only adds the content of the specified file(s) at the time the add command is run; if you want subsequent changes included in the next commit, then you must run `git add` again to add the new content to the index.

​	此命令可以在提交之前执行多次。它仅在运行 `git add` 命令时添加指定文件的内容；如果您希望将后续更改包含在下一次提交中，那么必须再次运行 `git add` 将新内容添加到索引中。

The `git status` command can be used to obtain a summary of which files have changes that are staged for the next commit.

​	`git status` 命令可用于获取已暂存以供下一次提交的更改的摘要。

The `git add` command will not add ignored files by default. If any ignored files were explicitly specified on the command line, `git add` will fail with a list of ignored files. Ignored files reached by directory recursion or filename globbing performed by Git (quote your globs before the shell) will be silently ignored. The *git add* command can be used to add ignored files with the `-f` (force) option.

​	默认情况下，`git add` 命令不会添加被忽略的文件。如果命令行上明确指定了任何被忽略的文件，`git add` 将失败并显示被忽略的文件列表。由 Git 执行的目录递归或文件名模式匹配（在 shell 中引用模式）所达到的被忽略的文件将被静默忽略。您可以使用 `-f`（force）选项来强制使用 `git add` 命令添加被忽略的文件。

Please see [git-commit[1]](../git-commit) for alternative ways to add content to a commit.

​	有关将内容添加到提交的其他方式，请参阅 [git-commit[1\]](https://chat.openai.com/git-commit)。

## 选项

- `<pathspec>`…

  Files to add content from. Fileglobs (e.g. `*.c`) can be given to add all matching files. Also a leading directory name (e.g. `dir` to add `dir/file1` and `dir/file2`) can be given to update the index to match the current state of the directory as a whole (e.g. specifying `dir` will record not just a file `dir/file1` modified in the working tree, a file `dir/file2` added to the working tree, but also a file `dir/file3` removed from the working tree). Note that older versions of Git used to ignore removed files; use `--no-all` option if you want to add modified or new files but ignore removed ones.For more details about the `<pathspec>` syntax, see the *pathspec* entry in [gitglossary[7]](../../7/gitglossary).
  
  要添加内容的文件。可以使用文件通配符（例如 `*.c`）来添加所有匹配的文件。也可以给出前导目录名（例如 `dir`，以更新索引以与整个目录的当前状态相匹配）。例如，指定 `dir` 将记录不仅在工作树中修改的文件 `dir/file1`，在工作树中添加的文件 `dir/file2`，还会从工作树中删除的文件 `dir/file3`。注意，旧版本的 Git 会忽略已删除的文件；如果您想添加已修改或新建的文件但忽略已删除的文件，请使用 `--no-all` 选项。有关 `<pathspec>` 语法的更多详细信息，请参阅 [gitglossary[7\]](https://chat.openai.com/7/gitglossary) 中的“pathspec”条目。
- -n
- --dry-run

  Don’t actually add the file(s), just show if they exist and/or will be ignored.
  
  仅显示文件是否存在或将被忽略，不实际添加文件。
- -v
- --verbose

  Be verbose.
  
  显示详细输出。
- -f
- --force

  Allow adding otherwise ignored files.
  
  允许添加被忽略的文件。
- --sparse

  Allow updating index entries outside of the sparse-checkout cone. Normally, `git add` refuses to update index entries whose paths do not fit within the sparse-checkout cone, since those files might be removed from the working tree without warning. See [git-sparse-checkout[1]](../git-sparse-checkout) for more details.
  
  允许更新超出 sparse-checkout 范围的索引条目。通常，`git add` 拒绝更新不符合 sparse-checkout 范围的索引条目，因为这些文件可能在工作树中被删除而没有警告。有关更多详细信息，请参阅 [git-sparse-checkout[1\]](https://chat.openai.com/git-sparse-checkout)。
- -i
- --interactive

  Add modified contents in the working tree interactively to the index. Optional path arguments may be supplied to limit operation to a subset of the working tree. See “Interactive mode” for details.
  
  交互式地将工作树中的修改内容添加到索引。可以提供可选的路径参数，以将操作限制为工作树的子集。详细信息请参阅“交互模式”。
- -p
- --patch

  Interactively choose hunks of patch between the index and the work tree and add them to the index. This gives the user a chance to review the difference before adding modified contents to the index.This effectively runs `add --interactive`, but bypasses the initial command menu and directly jumps to the `patch` subcommand. See “Interactive mode” for details.
  
  选择索引与工作树之间的补丁的交互式部分，并将它们添加到索引。这使用户有机会在将修改的内容添加到索引之前查看差异。这实际上运行了 `add --interactive`，但绕过了初始的命令菜单，直接跳转到 `patch` 子命令。详细信息请参阅“交互模式”。
- -e
- --edit

  Open the diff vs. the index in an editor and let the user edit it. After the editor was closed, adjust the hunk headers and apply the patch to the index.The intent of this option is to pick and choose lines of the patch to apply, or even to modify the contents of lines to be staged. This can be quicker and more flexible than using the interactive hunk selector. However, it is easy to confuse oneself and create a patch that does not apply to the index. See EDITING PATCHES below.
  
  在编辑器中打开索引与 diff，并允许用户进行编辑。在编辑器关闭后，调整块标题并将补丁应用于索引。此选项的目的是选择和选择要应用的补丁行，或者甚至修改要暂存的行内容。这比使用交互式补丁选择器更快且更灵活。但是，容易混淆自己并创建无法应用于索引的补丁。请参阅下面的“编辑补丁”部分。
- -u
- --update

  Update the index just where it already has an entry matching `<pathspec>`. This removes as well as modifies index entries to match the working tree, but adds no new files.If no `<pathspec>` is given when `-u` option is used, all tracked files in the entire working tree are updated (old versions of Git used to limit the update to the current directory and its subdirectories).
  
  仅在索引中已经存在与 `<pathspec>` 匹配的条目时更新索引。这将删除并修改索引条目以与工作树匹配，但不添加新文件。如果在使用 `-u` 选项时未给出 `<pathspec>`，则将更新整个工作树中的所有跟踪文件（旧版本的 Git 会将更新限制为当前目录及其子目录）。
- -A
- --all
- --no-ignore-removal

  Update the index not only where the working tree has a file matching `<pathspec>` but also where the index already has an entry. This adds, modifies, and removes index entries to match the working tree.If no `<pathspec>` is given when `-A` option is used, all files in the entire working tree are updated (old versions of Git used to limit the update to the current directory and its subdirectories).
  
  不仅在工作树中有与 `<pathspec>` 匹配的文件时更新索引，还在索引中已经有的条目时更新索引。这将添加、修改和删除索引条目以与工作树匹配。如果在使用 `-A` 选项时未给出 `<pathspec>`，则将更新整个工作树中的所有文件（旧版本的 Git 会将更新限制为当前目录及其子目录）。
- --no-all
- --ignore-removal

  Update the index by adding new files that are unknown to the index and files modified in the working tree, but ignore files that have been removed from the working tree. This option is a no-op when no `<pathspec>` is used.This option is primarily to help users who are used to older versions of Git, whose "git add `<pathspec>`…" was a synonym for "git add --no-all `<pathspec>`…", i.e. ignored removed files.
  
  更新索引，添加工作树中未知的新文件和修改的文件，但忽略从工作树中删除的文件。当未使用 `<pathspec>` 时，此选项不执行任何操作。此选项主要是帮助习惯于旧版本 Git 的用户，这些版本中的“git add `<pathspec>`…”是“git add --no-all `<pathspec>`…”的同义词，即忽略被删除的文件。
- -N
- --intent-to-add

  Record only the fact that the path will be added later. An entry for the path is placed in the index with no content. This is useful for, among other things, showing the unstaged content of such files with `git diff` and committing them with `git commit -a`.
  
  仅记录该路径稍后将要添加的事实。将路径的条目放置在索引中，但没有内容。这对于使用 `git diff` 显示此类文件的未暂存内容，并使用 `git commit -a` 提交它们非常有用。
- --refresh

  Don’t add the file(s), but only refresh their stat() information in the index.
  
  不添加文件，仅刷新索引中的 stat() 信息。
- --ignore-errors

  If some files could not be added because of errors indexing them, do not abort the operation, but continue adding the others. The command shall still exit with non-zero status. The configuration variable `add.ignoreErrors` can be set to true to make this the default behaviour.
  
  如果由于对文件进行索引时出现错误而无法添加某些文件，则不中止操作，而是继续添加其他文件。命令仍将以非零状态退出。可以设置配置变量 `add.ignoreErrors` 为 true，以使其成为默认行为。
- --ignore-missing

  This option can only be used together with --dry-run. By using this option the user can check if any of the given files would be ignored, no matter if they are already present in the work tree or not.
  
  此选项只能与 --dry-run 一起使用。使用此选项，用户可以检查任何给定的文件是否会被忽略，无论它们是否已存在于工作树中。
- --no-warn-embedded-repo

  By default, `git add` will warn when adding an embedded repository to the index without using `git submodule add` to create an entry in `.gitmodules`. This option will suppress the warning (e.g., if you are manually performing operations on submodules).
  
  默认情况下，当向索引添加嵌入的存储库时而不使用 `git submodule add` 在 `.gitmodules` 中创建条目时，`git add` 会发出警告。此选项将抑制警告（例如，如果您手动对子模块执行操作）。
- --renormalize

  Apply the "clean" process freshly to all tracked files to forcibly add them again to the index. This is useful after changing `core.autocrlf` configuration or the `text` attribute in order to correct files added with wrong CRLF/LF line endings. This option implies `-u`. Lone CR characters are untouched, thus while a CRLF cleans to LF, a CRCRLF sequence is only partially cleaned to CRLF.
  
  将“干净”过程重新应用到所有跟踪的文件中，以强制再次将它们添加到索引中。在更改 `core.autocrlf` 配置或 `text` 属性后，以修正使用错误的 CRLF/LF 行结尾添加的文件时，这非常有用。此选项隐含了 `-u` 选项。孤立的 CR 字符保持不变，因此，虽然 CRLF 会变为 LF，但 CRCRLF 序列只会部分清除为 CRLF。
- --chmod=(+|-)x

  Override the executable bit of the added files. The executable bit is only changed in the index, the files on disk are left unchanged.
  
  覆盖已添加文件的可执行位。可执行位仅在索引中更改，磁盘上的文件保持不变。
- --pathspec-from-file=`<file>`

  Pathspec is passed in `<file>` instead of commandline args. If `<file>` is exactly `-` then standard input is used. Pathspec elements are separated by LF or CR/LF. Pathspec elements can be quoted as explained for the configuration variable `core.quotePath` (see [git-config[1]](../git-config)). See also `--pathspec-file-nul` and global `--literal-pathspecs`.
  
  将 pathspec 传递给 `<file>` 而不是命令行参数。如果 `<file>` 恰好是 `-`，则使用标准输入。pathspec 元素由 LF 或 CR/LF 分隔。pathspec 元素可以引用配置变量 `core.quotePath` 中解释的方式进行引用（参见 [git-config[1\]](https://chat.openai.com/git-config)）。另请参阅 `--pathspec-file-nul` 和全局 `--literal-pathspecs`。
- --pathspec-file-nul

  Only meaningful with `--pathspec-from-file`. Pathspec elements are separated with NUL character and all other characters are taken literally (including newlines and quotes).
  
  仅与 `--pathspec-from-file` 一起使用时才有意义。pathspec 元素以 NUL 字符分隔，所有其他字符都被视为字面值（包括换行符和引号）。

---

  This option can be used to separate command-line options from the list of files, (useful when filenames might be mistaken for command-line options).

​	此选项可用于将命令行选项与文件列表分隔开（当文件名可能被错误地认为是命令行选项时很有用）。

## 示例

- Adds content from all `*.txt` files under `Documentation` directory and its subdirectories:
- 添加 `Documentation` 目录及其子目录中所有 `*.txt` 文件的内容：

  ``` bash
  $ git add Documentation/\*.txt
  ```

  Note that the asterisk `*` is quoted from the shell in this example; this lets the command include the files from subdirectories of `Documentation/` directory.
  
  注意，在此示例中，星号 `*` 在 shell 中被引用，这使得命令包含 `Documentation/` 目录的子目录中的文件。
- Considers adding content from all `git-*.sh` scripts:
- 考虑添加所有 `git-*.sh` 脚本的内容：

  ``` bash
  $ git add git-*.sh
  ```

  Because this example lets the shell expand the asterisk (i.e. you are listing the files explicitly), it does not consider `subdir/git-foo.sh`.
  
  因为此示例允许 shell 展开星号（即，您明确列出文件），所以它不会考虑 `subdir/git-foo.sh`。

## 交互模式

When the command enters the interactive mode, it shows the output of the *status* subcommand, and then goes into its interactive command loop.

​	当命令进入交互模式时，它显示“status”子命令的输出，然后进入交互式命令循环。

The command loop shows the list of subcommands available, and gives a prompt "What now> ". In general, when the prompt ends with a single `>`, you can pick only one of the choices given and type return, like this:

​	命令循环显示可用的子命令列表，并给出提示：“What now> ”。通常，当提示以单个 `>` 结束时，您可以从给定的选择中选择一个并键入回车，例如：

```
    *** Commands ***
      1: status       2: update       3: revert       4: add untracked
      5: patch        6: diff         7: quit         8: help
    What now> 1
```

You also could say `s` or `sta` or `status` above as long as the choice is unique.

​	您还可以说 `s`、`sta` 或 `status` 来选择上面的选项，只要选择是唯一的即可。

The main command loop has 6 subcommands (plus help and quit).

​	主命令循环有 6 个子命令（以及帮助和退出）。

- status

  This shows the change between HEAD and index (i.e. what will be committed if you say `git commit`), and between index and working tree files (i.e. what you could stage further before `git commit` using `git add`) for each path. A sample output looks like this:
  
  这显示 HEAD 与索引之间的更改（即，如果执行 `git commit`，将提交的内容），以及索引与工作树文件之间的更改（即，使用 `git add` 可以进一步暂存的内容）。示例输出如下：
  
  ```
                staged     unstaged path
       1:       binary      nothing foo.png
       2:     +403/-35        +1/-1 add-interactive.c
  ```
  
  It shows that foo.png has differences from HEAD (but that is binary so line count cannot be shown) and there is no difference between indexed copy and the working tree version (if the working tree version were also different, *binary* would have been shown in place of *nothing*). The other file, add-interactive.c, has 403 lines added and 35 lines deleted if you commit what is in the index, but working tree file has further modifications (one addition and one deletion).
  
  它显示 foo.png 与 HEAD 之间的差异（但由于是二进制文件，无法显示行数），并且索引副本与工作树版本之间没有差异（如果工作树版本也有差异，则在“nothing”的位置显示“binary”）。其他文件 add-interactive.c 在索引中有 403 行添加和 35 行删除，如果您提交索引中的内容，则工作树文件有进一步修改（一个添加和一个删除）。
- update

  This shows the status information and issues an "Update>>" prompt. When the prompt ends with double `>>`, you can make more than one selection, concatenated with whitespace or comma. Also you can say ranges. E.g. "2-5 7,9" to choose 2,3,4,5,7,9 from the list. If the second number in a range is omitted, all remaining patches are taken. E.g. "7-" to choose 7,8,9 from the list. You can say `*` to choose everything.
  
  这显示状态信息并发出“Update>>”提示。当提示以双 `>>` 结束时，您可以通过空格或逗号连接的方式选择多个选择。您还可以使用范围。例如，“2-5 7,9”选择列表中的 2、3、4、5、7、9。如果省略范围中的第二个数字，则会选择所有剩余的补丁。例如，“7-”选择列表中的 7、8、9。您可以说 `*` 选择所有内容。
  
  What you chose are then highlighted with `*`, like this:
  
  您选择的内容将使用 `*` 高亮显示，如下所示：
  
  ```
             staged     unstaged path
    1:       binary      nothing foo.png
  * 2:     +403/-35        +1/-1 add-interactive.c
  ```
  
  To remove selection, prefix the input with `-` like this:`Update>> -2`After making the selection, answer with an empty line to stage the contents of working tree files for selected paths in the index.
  
  要删除选择，请在输入前加上 `-`，例如：`Update>> -2`。选择后，回答空行将会将选定路径的工作树文件内容暂存到索引中。
- revert

  This has a very similar UI to *update*, and the staged information for selected paths are reverted to that of the HEAD version. Reverting new paths makes them untracked.
  
  这与 *update* 具有非常相似的用户界面，所选路径的已暂存信息将还原为 HEAD 版本。还原新路径会使它们变为未跟踪状态。
- add untracked

  This has a very similar UI to *update* and *revert*, and lets you add untracked paths to the index.
  
  这与 *update* 和 *revert* 具有非常相似的用户界面，并允许您将未跟踪的路径添加到索引中。
- patch

  This lets you choose one path out of a *status* like selection. After choosing the path, it presents the diff between the index and the working tree file and asks you if you want to stage the change of each hunk. You can select one of the following options and type return:
  
  这使您可以从类似“status”的选择中选择一个路径。选择路径后，它会显示索引与工作树文件之间的差异，并询问您是否要将每个补丁的更改暂存。您可以选择以下选项之一并键入回车：
  
  ```
  y - stage this hunk
  n - do not stage this hunk
  q - quit; do not stage this hunk or any of the remaining ones
  a - stage this hunk and all later hunks in the file
  d - do not stage this hunk or any of the later hunks in the file
  g - select a hunk to go to
  / - search for a hunk matching the given regex
  j - leave this hunk undecided, see next undecided hunk
  J - leave this hunk undecided, see next hunk
  k - leave this hunk undecided, see previous undecided hunk
  K - leave this hunk undecided, see previous hunk
  s - split the current hunk into smaller hunks
  e - manually edit the current hunk
  ? - print help
  y - 暂存此块
  n - 不暂存此块
  q - 退出；不暂存此块或剩余的块
  a - 暂存此块以及文件中后续的所有块
  d - 不暂存此块或文件中后续的所有块
  g - 选择要前往的块
  / - 搜索与给定正则表达式匹配的块
  j - 保留此块为未决状态，查看下一个未决状态的块
  J - 保留此块为未决状态，查看下一个块
  k - 保留此块为未决状态，查看上一个未决状态的块
  K - 保留此块为未决状态，查看上一个块
  s - 将当前块拆分为较小的块
  e - 手动编辑当前块
  ? - 打印帮助
  ```
  
  After deciding the fate for all hunks, if there is any hunk that was chosen, the index is updated with the selected hunks.You can omit having to type return here, by setting the configuration variable `interactive.singleKey` to `true`.
  
  在决定所有块的命运后，如果有任何选择的块，则索引将使用所选的块进行更新。您可以通过将配置变量 `interactive.singleKey` 设置为 `true` 来省略在此处输入回车。
- diff

  This lets you review what will be committed (i.e. between HEAD and index).
  
  这使您可以审查将要提交的内容（即 HEAD 与索引之间的差异）。

## 编辑补丁

Invoking `git add -e` or selecting `e` from the interactive hunk selector will open a patch in your editor; after the editor exits, the result is applied to the index. You are free to make arbitrary changes to the patch, but note that some changes may have confusing results, or even result in a patch that cannot be applied. If you want to abort the operation entirely (i.e., stage nothing new in the index), simply delete all lines of the patch. The list below describes some common things you may see in a patch, and which editing operations make sense on them.

​	调用 `git add -e` 或从交互式补丁选择器中选择 `e` 将会在编辑器中打开一个补丁；编辑器退出后，结果将被应用于索引。您可以随意对补丁进行任意更改，但请注意，某些更改可能会导致混淆的结果，甚至可能导致无法应用补丁。如果您要完全中止操作（即在索引中不暂存任何新内容），只需删除补丁的所有行。下面列出了一些您可能在补丁中看到的常见事物，以及对它们进行的编辑操作。

- added content
- 添加的内容

  Added content is represented by lines beginning with "+". You can prevent staging any addition lines by deleting them.
  
  添加的内容由以 “+” 开头的行表示。您可以通过删除它们来阻止暂存任何添加行。
- removed content
- 删除的内容

  Removed content is represented by lines beginning with "-". You can prevent staging their removal by converting the "-" to a " " (space).
  
  删除的内容由以 “-” 开头的行表示。您可以通过将 “-” 转换为 “ ”（空格）来阻止暂存删除的行。请注意，此操作并不会撤消删除；它只会阻止删除的内容进入索引。
- modified content
- 修改的内容

  Modified content is represented by "-" lines (removing the old content) followed by "+" lines (adding the replacement content). You can prevent staging the modification by converting "-" lines to " ", and removing "+" lines. Beware that modifying only half of the pair is likely to introduce confusing changes to the index.
  
  修改的内容由 “-” 行（删除旧内容）和 “+” 行（添加替换内容）组成。您可以通过将 “-” 行转换为 “ ”，并删除 “+” 行来阻止暂存修改。请注意，只修改一对中的一半可能会导致对索引的混淆性更改。

There are also more complex operations that can be performed. But beware that because the patch is applied only to the index and not the working tree, the working tree will appear to "undo" the change in the index. For example, introducing a new line into the index that is in neither the HEAD nor the working tree will stage the new line for commit, but the line will appear to be reverted in the working tree.

​	还有一些更复杂的操作。但请注意，由于补丁仅应用于索引而不是工作树，因此工作树将看起来“撤消”了索引中的更改。例如，在索引中引入一个既不在 HEAD 也不在工作树中的新行，将会将新行暂存以提交，但在工作树中，该行将看起来已被还原。

Avoid using these constructs, or do so with extreme caution.

​	请避免使用这些操作，或者在极度谨慎的情况下使用。

- removing untouched content
- 移除未更改的内容

  Content which does not differ between the index and working tree may be shown on context lines, beginning with a " " (space). You can stage context lines for removal by converting the space to a "-". The resulting working tree file will appear to re-add the content.
  
  在索引和工作树之间没有差异的内容可能会显示在上下文行中，以 “ ”（空格） 开头。您可以通过将空格转换为 “-” 来暂存上下文行。结果的工作树文件将看起来重新添加了内容。
- modifying existing content
- 修改现有内容

  One can also modify context lines by staging them for removal (by converting " " to "-") and adding a "+" line with the new content. Similarly, one can modify "+" lines for existing additions or modifications. In all cases, the new modification will appear reverted in the working tree.
  
  也可以通过将上下文行暂存为删除（将 “ ” 转换为 “-”）并添加一个以 “+” 开头的行来修改上下文行。同样，也可以修改添加或修改的 “+” 行。在所有情况下，新的修改将在工作树中呈现为已还原。
- new content
- 新的内容

  You may also add new content that does not exist in the patch; simply add new lines, each starting with "+". The addition will appear reverted in the working tree.
  
  您还可以添加未在补丁中存在的新内容；只需添加以 “+” 开头的新行。添加的内容将在工作树中呈现为已还原。

There are also several operations which should be avoided entirely, as they will make the patch impossible to apply:

​	还有一些操作应完全避免，因为它们将导致无法应用补丁：

- adding context (" ") or removal ("-") lines
- deleting context or removal lines
- modifying the contents of context or removal lines
- 添加上下文（以 " " 开头）或删除（以 "-" 开头）行
- 删除上下文或删除行
- 修改上下文或删除行的内容

## 配置

Everything below this line in this section is selectively included from the [git-config[1]](../git-config) documentation. The content is the same as what’s found there:

​	以下部分中的所有内容均来自 [git-config[1\]](https://chat.openai.com/git-config) 文档的选择性包含。其内容与那里找到的内容相同：

- add.ignoreErrors
- add.ignore-errors (deprecated)
- add.ignore-errors（已弃用）

  Tells *git add* to continue adding files when some files cannot be added due to indexing errors. Equivalent to the `--ignore-errors` option of [git-add[1]](../git-add). `add.ignore-errors` is deprecated, as it does not follow the usual naming convention for configuration variables.
  
  告诉 *git add* 在由于索引错误而无法添加某些文件时继续添加文件。相当于 [git-add[1\]](https://chat.openai.com/git-add) 的 `--ignore-errors` 选项。`add.ignore-errors` 已弃用，因为它不符合常规的配置变量命名约定。
- add.interactive.useBuiltin

  Unused configuration variable. Used in Git versions v2.25.0 to v2.36.0 to enable the built-in version of [git-add[1]](../git-add)'s interactive mode, which then became the default in Git versions v2.37.0 to v2.39.0.
  
  未使用的配置变量。在 Git 版本 v2.25.0 到 v2.36.0 中用于启用 [git-add[1\]](https://chat.openai.com/git-add) 的内置交互模式，然后在 Git 版本 v2.37.0 到 v2.39.0 中成为默认设置。

## 另请参阅

[git-status[1]](../git-status) [git-rm[1]](../git-rm) [git-reset[1]](../git-reset) [git-mv[1]](../git-mv) [git-commit[1]](../git-commit) [git-update-index[1]](../git-update-index)

## GIT

  这是[git[1]](../../Git)工具集中的一部分。
