+++
title = "gitdiffcore——中英对照版"
weight = 30
type = "docs"
date = 2023-07-30T15:39:23+08:00
description = ""
isCJKLanguage = true
draft = false

+++

# gitdiffcore

https://git-scm.com/docs/gitdiffcore

version 2.41.0

## 名称

gitdiffcore - Tweaking diff output

gitdiffcore - 调整 diff 输出

## 概要

```
git diff *
```

## 描述

The diff commands *git diff-index*, *git diff-files*, and *git diff-tree* can be told to manipulate differences they find in unconventional ways before showing *diff* output. The manipulation is collectively called "diffcore transformation". This short note describes what they are and how to use them to produce *diff* output that is easier to understand than the conventional kind.

​	`git diff-*` 系列命令包括 *git diff-index*、*git diff-files* 和 *git diff-tree*，它们可以在显示 *diff* 输出之前以非传统方式操纵找到的差异。这种操纵统称为 "diffcore 变换"。本简要说明描述了它们是什么以及如何使用它们生成比传统类型更易理解的 *diff* 输出。

## 操作链

The `git diff-*` family works by first comparing two sets of files:

​	`git diff-*` 系列命令首先比较两组文件：

- *git diff-index* compares contents of a "tree" object and the working directory (when `--cached` flag is not used) or a "tree" object and the index file (when `--cached` flag is used);
- *git diff-index* 比较一个 "tree" 对象和工作目录（当未使用 `--cached` 标志时），或者比较一个 "tree" 对象和索引文件（当使用 `--cached` 标志时）；
- *git diff-files* compares contents of the index file and the working directory;
- *git diff-files* 比较索引文件和工作目录的内容；
- *git diff-tree* compares contents of two "tree" objects;
- *git diff-tree* 比较两个 "tree" 对象。

In all of these cases, the commands themselves first optionally limit the two sets of files by any pathspecs given on their command-lines, and compare corresponding paths in the two resulting sets of files.

​	在所有这些情况下，这些命令首先可以选择通过它们的命令行上给定的任何路径规范来限制两组文件，并比较两组文件中相应路径的文件。

The pathspecs are used to limit the world diff operates in. They remove the filepairs outside the specified sets of pathnames. E.g. If the input set of filepairs included:

​	路径规范用于限制 diff 操作的范围。它们删除指定路径名集之外的文件对。例如，如果输入的文件对集包括：

```
:100644 100644 bcd1234... 0123456... M junkfile
```

but the command invocation was `git diff-files myfile`, then the junkfile entry would be removed from the list because only "myfile" is under consideration.

但命令调用为 `git diff-files myfile`，那么 "junkfile" 条目将从列表中删除，因为只有 "myfile" 在考虑范围之内。

The result of comparison is passed from these commands to what is internally called "diffcore", in a format similar to what is output when the -p option is not used. E.g.

​	比较的结果以类似于未使用 -p 选项时输出的格式传递给称为 "diffcore" 的内部机制。例如：

```
in-place edit  :100644 100644 bcd1234... 0123456... M file0
create         :000000 100644 0000000... 1234567... A file4
delete         :100644 000000 1234567... 0000000... D file5
unmerged       :000000 000000 0000000... 0000000... U file6
```

The diffcore mechanism is fed a list of such comparison results (each of which is called "filepair", although at this point each of them talks about a single file), and transforms such a list into another list. There are currently 5 such transformations:

​	diffcore 机制接收一系列这样的比较结果（每个结果称为 "filepair"，尽管此时每个结果只涉及一个单独的文件），并将这样的列表转换为另一个列表。目前有 5 种这样的转换：

- diffcore-break
- diffcore-rename
- diffcore-merge-broken
- diffcore-pickaxe
- diffcore-order
- diffcore-rotate

These are applied in sequence. The set of filepairs *git diff-** commands find are used as the input to diffcore-break, and the output from diffcore-break is used as the input to the next transformation. The final result is then passed to the output routine and generates either diff-raw format (see Output format sections of the manual for *git diff-** commands) or diff-patch format.

​	这些转换按顺序应用。*git diff-* 命令找到的文件对被用作 diffcore-break 的输入，而 diffcore-break 的输出又用作下一个变换的输入。最终结果然后传递给输出例程，并生成 diff-raw 格式（参见 *git diff-* 命令的手册中的输出格式部分）或 diff-patch 格式。

## diffcore-break：用于拆分完全重写

The second transformation in the chain is diffcore-break, and is controlled by the -B option to the *git diff-** commands. This is used to detect a filepair that represents "complete rewrite" and break such filepair into two filepairs that represent delete and create. E.g. If the input contained this filepair:

​	操作链中的第二个转换是 diffcore-break，并由 *git diff-* 命令的 -B 选项控制。它用于检测表示 "完全重写" 的文件对，并将这样的文件对拆分为表示删除和创建的两个文件对。例如，如果输入包含了这样的文件对：

```
:100644 100644 bcd1234... 0123456... M file0
```

and if it detects that the file "file0" is completely rewritten, it changes it to:

并且它检测到文件 "file0" 被完全重写，它会将其改为：

```
:100644 000000 bcd1234... 0000000... D file0
:000000 100644 0000000... 0123456... A file0
```

For the purpose of breaking a filepair, diffcore-break examines the extent of changes between the contents of the files before and after modification (i.e. the contents that have "bcd1234…" and "0123456…" as their SHA-1 content ID, in the above example). The amount of deletion of original contents and insertion of new material are added together, and if it exceeds the "break score", the filepair is broken into two. The break score defaults to 50% of the size of the smaller of the original and the result (i.e. if the edit shrinks the file, the size of the result is used; if the edit lengthens the file, the size of the original is used), and can be customized by giving a number after "-B" option (e.g. "-B75" to tell it to use 75%).

​	为了拆分文件对，diffcore-break 检查文件在修改之前和修改之后的内容之间的变化范围（即上述示例中具有 "bcd1234…" 和 "0123456…" 作为 SHA-1 内容 ID 的内容）。原始内容的删除量和新内容的插入量被加在一起，如果超过 "拆分分数"，则文件对将被拆分为两个。拆分分数默认为原始内容和结果（即如果编辑缩小了文件，则使用结果的大小；如果编辑增加了文件的长度，则使用原始的大小）的较小者大小的 50%，可以通过在 -B 选项后面给定数字来自定义（例如 "-B75" 表示使用 75%）。

## diffcore-rename：用于检测重命名和复制

This transformation is used to detect renames and copies, and is controlled by the -M option (to detect renames) and the -C option (to detect copies as well) to the *git diff-** commands. If the input contained these filepairs:

​	该转换用于检测重命名和复制，并由 *git diff-* 命令的 -M 选项（用于检测重命名）和 -C 选项（用于检测复制）控制。如果输入包含了这样的文件对：

```
:100644 000000 0123456... 0000000... D fileX
:000000 100644 0000000... 0123456... A file0
```

and the contents of the deleted file fileX is similar enough to the contents of the created file file0, then rename detection merges these filepairs and creates:

并且删除的文件 fileX 的内容与创建的文件 file0 的内容足够相似，则重命名检测将合并这些文件对并创建：

```
:100644 100644 0123456... 0123456... R100 fileX file0
```

When the "-C" option is used, the original contents of modified files, and deleted files (and also unmodified files, if the "--find-copies-harder" option is used) are considered as candidates of the source files in rename/copy operation. If the input were like these filepairs, that talk about a modified file fileY and a newly created file file0:

​	当使用 "-C" 选项时，被修改的文件的原始内容以及删除的文件的原始内容（还有未修改的文件，如果使用 "--find-copies-harder" 选项）被视为重命名/复制操作的源文件候选。如果输入如下文件对，涉及一个被修改的文件 fileY 和一个新创建的文件 file0：

```
:100644 100644 0123456... 1234567... M fileY
:000000 100644 0000000... bcd3456... A file0
```

the original contents of fileY and the resulting contents of file0 are compared, and if they are similar enough, they are changed to:

将会比较文件Y的原始内容和文件0的结果内容，如果它们足够相似，则变为：

```
:100644 100644 0123456... 1234567... M fileY
:100644 100644 0123456... bcd3456... C100 fileY file0
```

In both rename and copy detection, the same "extent of changes" algorithm used in diffcore-break is used to determine if two files are "similar enough", and can be customized to use a similarity score different from the default of 50% by giving a number after the "-M" or "-C" option (e.g. "-M8" to tell it to use 8/10 = 80%).

​	在重命名和复制检测中，与 diffcore-break 中使用的 "变化范围" 算法相同用于确定两个文件是否 "足够相似"，并且可以通过在 "-M" 或 "-C" 选项后面给定数字来定制使用不同于默认值 50% 的相似性分数（例如 "-M8" 表示使用 8/10 = 80%）。

Note that when rename detection is on but both copy and break detection are off, rename detection adds a preliminary step that first checks if files are moved across directories while keeping their filename the same. If there is a file added to a directory whose contents is sufficiently similar to a file with the same name that got deleted from a different directory, it will mark them as renames and exclude them from the later quadratic step (the one that pairwise compares all unmatched files to find the "best" matches, determined by the highest content similarity). So, for example, if a deleted docs/ext.txt and an added docs/config/ext.txt are similar enough, they will be marked as a rename and prevent an added docs/ext.md that may be even more similar to the deleted docs/ext.txt from being considered as the rename destination in the later step. For this reason, the preliminary "match same filename" step uses a bit higher threshold to mark a file pair as a rename and stop considering other candidates for better matches. At most, one comparison is done per file in this preliminary pass; so if there are several remaining ext.txt files throughout the directory hierarchy after exact rename detection, this preliminary step may be skipped for those files.

​	注意，当启用重命名检测但关闭复制和拆分检测时，重命名检测会首先检查文件是否在保持其文件名不变的同时移动到其他目录。如果在某个目录中添加了一个文件，其内容与从不同目录中删除的文件具有足够相似性，它将标记为重命名，并在稍后的二次方步骤中排除它们（该步骤逐对比较所有不匹配的文件，以找到 "最佳" 匹配，由最高的内容相似性确定）。例如，如果删除了 docs/ext.txt 并添加了 docs/config/ext.txt，它们足够相似，它们将被标记为重命名，并防止将可能与已删除的 docs/ext.txt 更相似的已添加的 docs/ext.md 考虑为后续步骤中的重命名目标。因此，初步的 "匹配相同文件名" 步骤使用了稍高的阈值来标记文件对为重命名并停止考虑其他更好匹配的候选。对于此初步遍历的每个文件最多执行一次比较；因此，如果在精确重命名检测后目录层次结构中仍然存在几个 ext.txt 文件，则可能会跳过这些文件的此初步步骤。

Note. When the "-C" option is used with `--find-copies-harder` option, *git diff-** commands feed unmodified filepairs to diffcore mechanism as well as modified ones. This lets the copy detector consider unmodified files as copy source candidates at the expense of making it slower. Without `--find-copies-harder`, `git diff-*`commands can detect copies only if the file that was copied happened to have been modified in the same changeset.

​	注意。当使用 `--find-copies-harder` 选项与 "-C" 选项一起使用时，*git diff-* 命令会将未修改的文件对作为 diffcore 机制的输入，除了已修改的文件对。这允许复制检测器将未修改的文件视为复制源文件候选，但会导致速度较慢。如果不使用 `--find-copies-harder`，`git diff-*` 命令只能在被复制的文件恰好在同一个变更集中被修改时才能检测到复制。

## diffcore-merge-broken：用于合并完全重写

This transformation is used to merge filepairs broken by diffcore-break, and not transformed into rename/copy by diffcore-rename, back into a single modification. This always runs when diffcore-break is used.

​	该转换用于合并由 diffcore-break 拆分的文件对，并且在 diffcore-rename 中未转换为重命名/复制的文件对。这在使用 diffcore-break 时总是执行。

For the purpose of merging broken filepairs back, it uses a different "extent of changes" computation from the ones used by diffcore-break and diffcore-rename. It counts only the deletion from the original, and does not count insertion. If you removed only 10 lines from a 100-line document, even if you added 910 new lines to make a new 1000-line document, you did not do a complete rewrite. diffcore-break breaks such a case in order to help diffcore-rename to consider such filepairs as candidate of rename/copy detection, but if filepairs broken that way were not matched with other filepairs to create rename/copy, then this transformation merges them back into the original "modification".

​	为了将被拆分的文件对合并回来，它使用与 diffcore-break 和 diffcore-rename 不同的 "变化范围" 计算。它仅计算原始内容的删除量，不计算插入量。如果您从一个包含 100 行的文档中删除了 10 行，即使您添加了 910 行新内容以生成一个包含 1000 行的新文档，您也没有进行完全重写。diffcore-break 通过拆分这种情况来帮助 diffcore-rename 将这样的文件对视为重命名/复制检测的候选文件对，但如果以这种方式拆分的文件对未与其他文件对匹配以创建重命名/复制，那么此转换将它们合并回 "修改"。

The "extent of changes" parameter can be tweaked from the default 80% (that is, unless more than 80% of the original material is deleted, the broken pairs are merged back into a single modification) by giving a second number to -B option, like these:

​	"变化范围" 参数可以通过在 -B 选项后面给定第二个数字来调整，默认为 80%（即，除非删除原始内容的比例超过 80%，否则被拆分的文件对将合并回 "修改"），例如：

- -B50/60 (give 50% "break score" to diffcore-break, use 60% for diffcore-merge-broken).
- -B/60 (the same as above, since diffcore-break defaults to 50%).
- -B50/60（给 diffcore-break 50% "拆分分数"，给 diffcore-merge-broken 60%）。
- -B/60（与上面相同，因为 diffcore-break 默认为 50%）。

Note that earlier implementation left a broken pair as a separate creation and deletion patches. This was an unnecessary hack and the latest implementation always merges all the broken pairs back into modifications, but the resulting patch output is formatted differently for easier review in case of such a complete rewrite by showing the entire contents of old version prefixed with *-*, followed by the entire contents of new version prefixed with *+*.

​	注意，早期的实现将拆分的文件对作为单独的创建和删除补丁。这是一种不必要的修补，最新的实现始终将所有拆分的文件对合并回 "修改"，但是对于这种完全重写，生成的补丁输出格式有所不同，以便更容易查看，它显示旧版本的整个内容，并在前面加上 *-*，接着显示新版本的整个内容，并在前面加上 *+*。

## diffcore-pickaxe：用于检测指定字符串的添加/删除

This transformation limits the set of filepairs to those that change specified strings between the preimage and the postimage in a certain way. -S<block of text> and -G<regular expression> options are used to specify different ways these strings are sought.

​	该转换将文件对的集合限制为在原始图像和后图像之间以某种方式更改指定字符串的文件对。使用 -S<文本块> 和 -G<正则表达式> 选项来指定这些字符串被寻找的不同方式。

"-S<block of text>" detects filepairs whose preimage and postimage have different number of occurrences of the specified block of text. By definition, it will not detect in-file moves. Also, when a changeset moves a file wholesale without affecting the interesting string, diffcore-rename kicks in as usual, and `-S` omits the filepair (since the number of occurrences of that string didn’t change in that rename-detected filepair). When used with `--pickaxe-regex`, treat the <block of text> as an extended POSIX regular expression to match, instead of a literal string.

​	"-S<文本块>" 检测其原始图像和后图像中具有不同指定文本块的文件对。根据定义，它不会检测文件内部移动。另外，当一个更改集整体上移动了一个文件而不影响感兴趣的字符串时，diffcore-rename 会像往常一样启动，而 `-S` 会忽略该文件对（因为在重命名检测的文件对中，该字符串的出现次数没有改变）。当与 `--pickaxe-regex` 一起使用时，将 <文本块> 视为扩展的 POSIX 正则表达式来匹配，而不是字面字符串。

"-G<regular expression>" (mnemonic: grep) detects filepairs whose textual diff has an added or a deleted line that matches the given regular expression. This means that it will detect in-file (or what rename-detection considers the same file) moves, which is noise. The implementation runs diff twice and greps, and this can be quite expensive. To speed things up binary files without textconv filters will be ignored.

​	"-G<正则表达式>"（记忆口诀：grep）检测文本 diff 中具有与给定正则表达式匹配的添加或删除行的文件对。这意味着它将检测文件内部移动（或者重命名检测认为是相同文件的移动），这是干扰项。实现会运行两次 diff，并进行 grep，这可能非常昂贵。为加快速度，将忽略没有文本转换筛选器的二进制文件。

When `-S` or `-G` are used without `--pickaxe-all`, only filepairs that match their respective criterion are kept in the output. When `--pickaxe-all` is used, if even one filepair matches their respective criterion in a changeset, the entire changeset is kept. This behavior is designed to make reviewing changes in the context of the whole changeset easier.

​	当 `-S` 或 `-G` 与 `--pickaxe-all` 一起使用时，仅在输出中保留与各自条件匹配的文件对。当使用 `--pickaxe-all` 时，如果在一个更改集中至少有一个文件对与各自条件匹配，则保留整个更改集。此行为设计为使在整个更改集的上下文中查看更改更容易。

## diffcore-order：用于根据文件名对输出进行排序

This is used to reorder the filepairs according to the user’s (or project’s) taste, and is controlled by the -O option to the *git diff-** commands.

​	该转换用于根据用户（或项目）的喜好重新排序文件对，并由 *git diff-* 命令的 -O 选项控制。

This takes a text file each of whose lines is a shell glob pattern. Filepairs that match a glob pattern on an earlier line in the file are output before ones that match a later line, and filepairs that do not match any glob pattern are output last.

​	它接受一个文本文件，每行都是一个 shell glob 模式。与文件的 glob 模式匹配的文件对在后面的行之前输出，并且不匹配任何 glob 模式的文件对在最后输出。

As an example, a typical orderfile for the core Git probably would look like this:

​	例如，对于核心 Git 的典型顺序文件可能如下：

```
README
Makefile
Documentation
*.h
*.c
t
```

## diffcore-rotate：用于更改输出路径开始的位置

This transformation takes one pathname, and rotates the set of filepairs so that the filepair for the given pathname comes first, optionally discarding the paths that come before it. This is used to implement the `--skip-to` and the `--rotate-to` options. It is an error when the specified pathname is not in the set of filepairs, but it is not useful to error out when used with "git log" family of commands, because it is unreasonable to expect that a given path would be modified by each and every commit shown by the "git log" command. For this reason, when used with "git log", the filepair that sorts the same as, or the first one that sorts after, the given pathname is where the output starts.

​	该转换接受一个路径名，并旋转文件对集合，以便给定路径名的文件对首先出现，可选地丢弃在它之前的路径。这用于实现 `--skip-to` 和 `--rotate-to` 选项。如果指定的路径名不在文件对集合中，则出现错误，但是在与 "git log" 系列命令一起使用时，出现错误并不实用，因为不合理地期望 "git log" 命令显示的每个提交都修改了给定路径。因此，当与 "git log" 一起使用时，与给定路径排序相同或首先出现的文件对是输出开始的位置。

Use of this transformation combined with diffcore-order will produce unexpected results, as the input to this transformation is likely not sorted when diffcore-order is in effect.

​	使用此转换与 diffcore-order 结合使用将产生意外的结果，因为 diffcore-order 生效时输入可能未排序。



## 另请参阅

[git-diff[1]](../../1/git-diff), [git-diff-files[1]](../../1/git-diff-files), [git-diff-index[1]](../../1/git-diff-index), [git-diff-tree[1]](../../1/git-diff-tree), [git-format-patch[1]](../../1/git-format-patch), [git-log[1]](../../1/git-log), [gitglossary[7]](../gitglossary), [The Git User’s Manual](https://git-scm.com/docs/user-manual)

## GIT

  这是[git[1]](../../Git)工具集中的一部分。