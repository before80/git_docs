+++
title = "gitworkflows——中英对照版"
weight = 30
type = "docs"
date = 2023-07-30T15:39:23+08:00
description = ""
isCJKLanguage = true
draft = false
+++

# gitworkflows 



https://git-scm.com/docs/gitworkflows

version 2.41.0

## 名称

gitworkflows - An overview of recommended workflows with Git

​	gitworkflows - Git 推荐工作流程概述

## 概述

```
git *
```

## 描述

This document attempts to write down and motivate some of the workflow elements used for `git.git` itself. Many ideas apply in general, though the full workflow is rarely required for smaller projects with fewer people involved.

​	本文档试图记录并推动一些用于 `git.git` 本身的工作流程要素。虽然许多想法通常适用于一般情况，但对于涉及较少人员的较小项目，完整的工作流程很少是必需的。

We formulate a set of *rules* for quick reference, while the prose tries to motivate each of them. Do not always take them literally; you should value good reasons for your actions higher than manpages such as this one.

​	我们提出了一组*规则*供快速参考，而文本则试图为每个规则提供动机。不要将它们完全当真；您应该将您的行为合理性价值放在比这个手册（比如本手册）更高的位置。

## 分割更改

As a general rule, you should try to split your changes into small logical steps, and commit each of them. They should be consistent, working independently of any later commits, pass the test suite, etc. This makes the review process much easier, and the history much more useful for later inspection and analysis, for example with [git-blame[1]](../../1/git-blame) and [git-bisect[1]](../../1/git-bisect).

​	通常情况下，您应该尝试将更改分成小的逻辑步骤，并将每个步骤提交。它们应该一致，与后续提交无关，通过测试套件等。这使得审查过程更容易，历史记录对以后的检查和分析更有用，例如使用 [git-blame[1]](../../1/git-blame) 和 [git-bisect[1]](../../1/git-bisect)。

To achieve this, try to split your work into small steps from the very beginning. It is always easier to squash a few commits together than to split one big commit into several. Don’t be afraid of making too small or imperfect steps along the way. You can always go back later and edit the commits with `git rebase --interactive` before you publish them. You can use `git stash push --keep-index` to run the test suite independent of other uncommitted changes; see the EXAMPLES section of [git-stash[1]](../../1/git-stash).

​	为实现这一点，请尽早将您的工作分解为小步骤。合并几个提交总比将一个大提交拆分为几个提交要容易得多。不要害怕在路上迈出太小或不完美的步伐。您可以随后使用 `git rebase --interactive` 编辑提交，然后再发布它们。您可以使用 `git stash push --keep-index` 在没有其他未提交更改的情况下运行测试套件；请参阅 [git-stash[1]](../../1/git-stash) 的 EXAMPLES 部分。

## 管理分支

There are two main tools that can be used to include changes from one branch on another: [git-merge[1]](../../1/git-merge) and [git-cherry-pick[1]](../../1/git-cherry-pick).

​	有两个主要工具可用于将一个分支上的更改包含到另一个分支上：[git-merge[1]](../../1/git-merge) 和 [git-cherry-pick[1]](../../1/git-cherry-pick)。

Merges have many advantages, so we try to solve as many problems as possible with merges alone. Cherry-picking is still occasionally useful; see "Merging upwards" below for an example.

​	合并具有许多优点，因此我们尽可能多地使用合并来解决问题。樱桃拣选偶尔仍然有用；有关示例，请参阅下面的“向上合并”。

Most importantly, merging works at the branch level, while cherry-picking works at the commit level. This means that a merge can carry over the changes from 1, 10, or 1000 commits with equal ease, which in turn means the workflow scales much better to a large number of contributors (and contributions). Merges are also easier to understand because a merge commit is a "promise" that all changes from all its parents are now included.

​	最重要的是，合并在分支级别上工作，而樱桃拣选在提交级别上工作。这意味着合并可以轻松地携带 1 个、10 个或 1000 个提交的更改，从而使工作流程在大量贡献者（和贡献）时更加适用。合并也更容易理解，因为合并提交是一个“承诺”，即现在包含了所有父提交的更改。

There is a tradeoff of course: merges require a more careful branch management. The following subsections discuss the important points.

​	当然，这是一种权衡：合并需要更仔细的分支管理。以下小节讨论了重要的要点。

### 毕业

As a given feature goes from experimental to stable, it also "graduates" between the corresponding branches of the software. `git.git` uses the following *integration branches*:

​	随着给定功能从实验性变为稳定性，它还会在软件的相应分支之间“毕业”。`git.git` 使用以下*集成分支*：

- *maint* tracks the commits that should go into the next "maintenance release", i.e., update of the last released stable version;
- *maint* 跟踪应该进入下一个“维护发布”的提交，即最近发布的稳定版本的更新；
- *master* tracks the commits that should go into the next release;
- *master* 跟踪应该进入下一个发布的提交；
- *next* is intended as a testing branch for topics being tested for stability for master.
- *next* 旨在作为正在测试的主题的测试分支，以获得对主分支稳定性的测试。

There is a fourth official branch that is used slightly differently:

​	还有一个使用略有不同的官方分支：

- *seen* (patches seen by the maintainer) is an integration branch for things that are not quite ready for inclusion yet (see "Integration Branches" below).
- *seen*（维护者已查看的补丁）是一个集成分支，用于尚不完全准备好包含的内容（请参阅下面的“集成分支”）。

Each of the four branches is usually a direct descendant of the one above it.

​	四个分支中的每一个通常都是其上方分支的直接后代。

Conceptually, the feature enters at an unstable branch (usually *next* or *seen*), and "graduates" to *master* for the next release once it is considered stable enough.

​	从概念上讲，该功能进入不稳定分支（通常是 *next* 或 *seen*），并且一旦被认为足够稳定，它就会“毕业”到 *master*，以进行下一个发布。

### 向上合并

The "downwards graduation" discussed above cannot be done by actually merging downwards, however, since that would merge *all* changes on the unstable branch into the stable one. Hence the following:

​	上面讨论的“向下毕业”不能通过实际向下合并来完成，因为那样会将不稳定分支上的*所有*更改合并到稳定分支中。因此有以下内容：

#### 规则：向上合并

Always commit your fixes to the oldest supported branch that requires them. Then (periodically) merge the integration branches upwards into each other.

​	始终将您的修复提交到需要它们的最旧支持分支。然后（定期地）将集成分支向上合并到彼此之间。

This gives a very controlled flow of fixes. If you notice that you have applied a fix to e.g. *master* that is also required in *maint*, you will need to cherry-pick it (using [git-cherry-pick[1]](../../1/git-cherry-pick)) downwards. This will happen a few times and is nothing to worry about unless you do it very frequently.

​	这提供了一个非常受控制的修复流程。如果注意到您已经将修复应用到了 *master* 中，而在 *maint* 中也需要它，则需要将其向下樱桃拣选（使用 [git-cherry-pick[1]](../../1/git-cherry-pick)）。这会发生几次，不过除非您非常频繁地执行此操作，否则不用担心。

### 主题分支

Any nontrivial feature will require several patches to implement, and may get extra bugfixes or improvements during its lifetime.

​	任何非平凡的功能都需要多个补丁来实现，并且在其生命周期内可能会获得额外的错误修复或改进。

Committing everything directly on the integration branches leads to many problems: Bad commits cannot be undone, so they must be reverted one by one, which creates confusing histories and further error potential when you forget to revert part of a group of changes. Working in parallel mixes up the changes, creating further confusion.

​	直接在集成分支上提交所有内容会引起许多问题：无法撤消不良提交，因此必须逐个还原，这会创建混乱的历史记录，并在您忘记还原一组更改的一部分时产生更多错误。并行工作混淆了更改，进一步导致混淆。

Use of "topic branches" solves these problems. The name is pretty self explanatory, with a caveat that comes from the "merge upwards" rule above:

​	使用“主题分支”解决了这些问题。该名称相当自说明，但有一个来自上面的“向上合并”规则的警告：

#### 规则：主题分支

Make a side branch for every topic (feature, bugfix, …). Fork it off at the oldest integration branch that you will eventually want to merge it into.

​	为每个主题（功能、错误修复等）创建一个侧分支。在您最终希望将其合并到的最旧集成分支上分叉它。

Many things can then be done very naturally:

​	然后可以非常自然地执行许多操作：

- To get the feature/bugfix into an integration branch, simply merge it. If the topic has evolved further in the meantime, merge again. (Note that you do not necessarily have to merge it to the oldest integration branch first. For example, you can first merge a bugfix to *next*, give it some testing time, and merge to *maint* when you know it is stable.)
- 要将功能/错误修复合并到集成分支中，只需合并它。如果该主题在此期间进一步发展，请再次合并。（请注意，您不一定要先将其合并到最旧的集成分支中。例如，您可以先将错误修复合并到 *next*，然后在确定其稳定后再合并到 *maint*。）
- If you find you need new features from the branch *other* to continue working on your topic, merge *other* to *topic*. (However, do not do this "just habitually", see below.)
- 如果发现需要从分支 *other* 获取新功能以继续处理主题，请将 *other* 合并到 *topic*。（但是，不要“习惯性地”这样做，请参阅下面的说明。）
- If you find you forked off the wrong branch and want to move it "back in time", use [git-rebase[1]](../../1/git-rebase).
- 如果发现您当初分叉错误的分支并希望将其“回溯”，请使用 [git-rebase[1]](../../1/git-rebase)。

Note that the last point clashes with the other two: a topic that has been merged elsewhere should not be rebased. See the section on RECOVERING FROM UPSTREAM REBASE in [git-rebase[1]](../../1/git-rebase).

​	请注意，最后一点与前两点冲突：已在其他地方合并的主题不应该进行变基。请参阅 [git-rebase[1]](../../1/git-rebase) 中有关“从上游重新获得”的部分。

We should point out that "habitually" (regularly for no real reason) merging an integration branch into your topics — and by extension, merging anything upstream into anything downstream on a regular basis — is frowned upon:

​	我们应该指出，“习惯性地”（经常没有真正原因）将集成分支合并到您的主题中，而且顺便说一下，定期将上游的任何内容合并到下游的任何内容也是不受欢迎的：

#### 规则：仅在明确定义的时刻向下游合并

Do not merge to downstream except with a good reason: upstream API changes affect your branch; your branch no longer merges to upstream cleanly; etc.

​	除非有充分的理由，否则不要向下游合并：上游 API 的更改影响了您的分支；您的分支无法干净地合并到上游；等等。

Otherwise, the topic that was merged to suddenly contains more than a single (well-separated) change. The many resulting small merges will greatly clutter up history. Anyone who later investigates the history of a file will have to find out whether that merge affected the topic in development. An upstream might even inadvertently be merged into a "more stable" branch. And so on.

​	否则，被合并的主题将突然包含不止一个（间隔较大的）更改。许多由此产生的小型合并会极大地混乱历史记录。后来调查文件历史记录的任何人都必须弄清楚该合并是否影响了正在开发的主题。上游甚至可能无意中合并到“更稳定”的分支。等等。

### 丢弃集成

If you followed the last paragraph, you will now have many small topic branches, and occasionally wonder how they interact. Perhaps the result of merging them does not even work? But on the other hand, we want to avoid merging them anywhere "stable" because such merges cannot easily be undone.

​	如果您按照上面的最后一段进行操作，现在您将有许多小的主题分支，并且偶尔会想知道它们是如何交互的。也许合并它们的结果甚至无法正常工作？但另一方面，我们又不希望将它们合并到任何“稳定”位置，因为这样的合并无法轻松撤消。

The solution, of course, is to make a merge that we can undo: merge into a throw-away branch.

​	当然，解决方法是进行可以撤消的合并：合并到丢弃分支。

#### 规则：丢弃集成分支

To test the interaction of several topics, merge them into a throw-away branch. You must never base any work on such a branch!

​	为了测试几个主题之间的交互作用，请将它们合并到一个丢弃分支中。您绝不能将任何工作基于此类分支！

If you make it (very) clear that this branch is going to be deleted right after the testing, you can even publish this branch, for example to give the testers a chance to work with it, or other developers a chance to see if their in-progress work will be compatible. `git.git` has such an official throw-away integration branch called *seen*.

​	如果您明确指出此分支在测试后将被删除，甚至可以发布此分支，例如为测试人员提供使用它的机会，或者为其他开发人员提供查看其进行中工作是否兼容的机会。`git.git` 有一个名为 *seen* 的官方丢弃集成分支。

### 版本发布后的分支管理

Assuming you are using the merge approach discussed above, when you are releasing your project you will need to do some additional branch management work.

​	假设您正在使用上面讨论的合并方法，当您发布项目时，您需要进行一些额外的分支管理工作。

A feature release is created from the *master* branch, since *master* tracks the commits that should go into the next feature release.

​	功能发布是从 *master* 分支创建的，因为 *master* 跟踪应该进入下一个功能发布的提交。

The *master* branch is supposed to be a superset of *maint*. If this condition does not hold, then *maint* contains some commits that are not included on *master*. The fixes represented by those commits will therefore not be included in your feature release.

​	*master* 分支应该是 *maint* 的超集。如果不满足此条件，则 *maint* 包含一些未包含在 *master* 上的提交。因此，这些提交代表的修复将不会包含在您的功能发布中。

To verify that *master* is indeed a superset of *maint*, use git log:

​	要验证 *master* 是否确实是 *maint* 的超集，请使用 git log：

#### 步骤：验证 *master* 是 *maint* 的超集

```
git log master..maint
```

This command should not list any commits. Otherwise, check out *master* and merge *maint* into it.

​	此命令不应列出任何提交。否则，切换到 *master* 并将 *maint* 合并到它。

Now you can proceed with the creation of the feature release. Apply a tag to the tip of *master* indicating the release version:

​	现在，您可以继续创建功能发布。对 *master* 的顶部应用一个表示版本发布的标签：

#### 步骤：版本标记

```
git tag -s -m "Git X.Y.Z" vX.Y.Z master
```

You need to push the new tag to a public Git server (see "DISTRIBUTED WORKFLOWS" below). This makes the tag available to others tracking your project. The push could also trigger a post-update hook to perform release-related items such as building release tarballs and preformatted documentation pages.

​	您需要将新标签推送到公共的 Git 服务器（请参阅下面的“分布式工作流程”）。这将使标签对其他跟踪您的项目的人员可用。此推送还可能触发 post-update 钩子，执行与发布相关的任务，如构建发布 tar 包和预格式化的文档页。

Similarly, for a maintenance release, *maint* is tracking the commits to be released. Therefore, in the steps above simply tag and push *maint* rather than *master*.

​	同样，对于维护发布，*maint* 跟踪将要发布的提交。因此，在上述步骤中只需为 *maint* 而不是 *master* 打标签并推送。

### 功能发布后的维护分支管理

After a feature release, you need to manage your maintenance branches.

​	在进行功能发布后，您需要管理维护分支。

First, if you wish to continue to release maintenance fixes for the feature release made before the recent one, then you must create another branch to track commits for that previous release.

​	首先，如果您希望在最近一次功能发布之前继续发布功能发布的维护修复，那么必须创建另一个分支来跟踪之前发布的先前版本的提交。

To do this, the current maintenance branch is copied to another branch named with the previous release version number (e.g. maint-X.Y.(Z-1) where X.Y.Z is the current release).

​	为此，将当前维护分支复制到以先前版本号命名的另一个分支中（例如，maint-X.Y.(Z-1)，其中 X.Y.Z 是当前版本）。

#### 步骤：复制 maint

```
git branch maint-X.Y.(Z-1) maint
```

The *maint* branch should now be fast-forwarded to the newly released code so that maintenance fixes can be tracked for the current release:

​	*maint* 分支现在应该被快进到新发布的代码，以便可以为当前版本跟踪维护修复：

#### 步骤：更新 maint 到新版本

- `git checkout maint`
- `git merge --ff-only master`

If the merge fails because it is not a fast-forward, then it is possible some fixes on *maint* were missed in the feature release. This will not happen if the content of the branches was verified as described in the previous section.

​	如果合并失败，因为它不是快进，则可能会错过在功能发布中的 *maint* 的一些修复。如果已经按照上一节中描述的方法验证了分支的内容，这种情况不会发生。

### 功能发布后的 *next* 和 *seen* 分支管理

After a feature release, the integration branch *next* may optionally be rewound and rebuilt from the tip of *master* using the surviving topics on *next*:

​	功能发布后，集成分支 *next* 可以选择回退并从 *master* 的末尾重新构建，使用 *next* 中的活动主题：

#### 步骤：回退和重新构建 next

- `git switch -C next master`
- `git merge ai/topic_in_next1`
- `git merge ai/topic_in_next2`
- …

The advantage of doing this is that the history of *next* will be clean. For example, some topics merged into *next* may have initially looked promising, but were later found to be undesirable or premature. In such a case, the topic is reverted out of *next* but the fact remains in the history that it was once merged and reverted. By recreating *next*, you give another incarnation of such topics a clean slate to retry, and a feature release is a good point in history to do so.

​	这样做的好处是 *next* 的历史记录将会很干净。例如，合并到 *next* 中的一些主题可能最初看起来很有希望，但后来发现它们是不可取的或不成熟的。在这种情况下，将该主题从 *next* 中撤消，但是它曾经合并并撤消的事实仍然在历史记录中。通过重新创建 *next*，您为此类主题提供了另一种尝试的干净机会，功能发布是历史上的一个好时机。

If you do this, then you should make a public announcement indicating that *next* was rewound and rebuilt.

​	如果您这样做了，那么您应该发布一条公告，指示 *next* 已回退和重新构建。

The same rewind and rebuild process may be followed for *seen*. A public announcement is not necessary since *seen* is a throw-away branch, as described above.

​	可以对 *seen* 也执行相同的回退和重新构建过程。不需要公告，因为 *seen* 是一个丢弃分支，如上面所述。

## 分布式工作流程

After the last section, you should know how to manage topics. In general, you will not be the only person working on the project, so you will have to share your work.

​	在上一节，您应该了解如何管理主题。一般来说，您不会是唯一在项目上工作的人，因此需要共享您的工作。

Roughly speaking, there are two important workflows: merge and patch. The important difference is that the merge workflow can propagate full history, including merges, while patches cannot. Both workflows can be used in parallel: in `git.git`, only subsystem maintainers use the merge workflow, while everyone else sends patches.

​	大致上，有两个重要的工作流程：合并和补丁。重要的区别在于合并工作流程可以传播完整的历史记录，包括合并，而补丁则不行。两种工作流程可以并行使用：在 `git.git` 中，只有子系统维护者使用合并工作流程，而其他所有人都会发送补丁。

Note that the maintainer(s) may impose restrictions, such as "Signed-off-by" requirements, that all commits/patches submitted for inclusion must adhere to. Consult your project’s documentation for more information.

​	请注意，项目维护者可能会施加一些限制，例如 “Signed-off-by” 要求，所有提交/补丁必须遵循此要求。有关更多信息，请查阅项目的文档。

### 合并工作流程

The merge workflow works by copying branches between upstream and downstream. Upstream can merge contributions into the official history; downstream base their work on the official history.

​	合并工作流程通过在上游和下游之间复制分支来工作。上游可以将贡献合并到官方历史记录中；下游的工作基于官方历史记录。

There are three main tools that can be used for this:

​	这方面有三个主要工具可以使用：

- [git-push[1]](../../1/git-push) copies your branches to a remote repository, usually to one that can be read by all involved parties;
- [git-push[1]](../../1/git-push) 将您的分支复制到远程存储库，通常是可由所有相关方读取的存储库；
- [git-fetch[1]](../../1/git-fetch) that copies remote branches to your repository; and
- [git-fetch[1]](../../1/git-fetch) 将远程分支复制到您的存储库；以及
- [git-pull[1]](../../1/git-pull) that does fetch and merge in one go.
- [git-pull[1]](../../1/git-pull) 将获取和合并合并为一体。

Note the last point. Do *not* use *git pull* unless you actually want to merge the remote branch.

​	请注意最后一点。除非您真的想合并远程分支，否则不要使用 *git pull*。

Getting changes out is easy:

​	发布更改很容易：

#### 步骤：推送/拉取：发布分支/主题

`git push <remote> <branch>` and tell everyone where they can fetch from.

​	`git push <remote> <branch>` 并告诉每个人从哪里获取。

You will still have to tell people by other means, such as mail. (Git provides the [git-request-pull[1]](../../1/git-request-pull) to send preformatted pull requests to upstream maintainers to simplify this task.)

​	您仍然需要通过其他方式告诉其他人，例如邮件。（Git 提供了 [git-request-pull[1]](../../1/git-request-pull) 以向上游维护者发送预格式化的拉取请求，以简化此任务。）

If you just want to get the newest copies of the integration branches, staying up to date is easy too:

​	如果您只想获取集成分支的最新副本，保持最新状态也很容易：

#### 步骤：推送/拉取：保持最新状态

Use `git fetch <remote>` or `git remote update` to stay up to date.

​	使用 `git fetch <remote>` 或 `git remote update` 保持最新状态。

Then simply fork your topic branches from the stable remotes as explained earlier.

​	然后只需按照前面解释的方式从稳定的远程仓库派生主题分支。

If you are a maintainer and would like to merge other people’s topic branches to the integration branches, they will typically send a request to do so by mail. Such a request looks like

​	如果您是项目维护者，并且想要将其他人的主题分支合并到集成分支中，他们通常会通过邮件发送请求这样做。此类请求的格式如下：

```
Please pull from
    <URL> <branch>
```

In that case, *git pull* can do the fetch and merge in one go, as follows.

​	在这种情况下，*git pull* 可以一次完成获取和合并，如下所示。

#### 步骤：推送/拉取：合并远程主题

```
git pull <URL> <branch>
```

Occasionally, the maintainer may get merge conflicts when they try to pull changes from downstream. In this case, they can ask downstream to do the merge and resolve the conflicts themselves (perhaps they will know better how to resolve them). It is one of the rare cases where downstream *should* merge from upstream.

​	偶尔，当维护者尝试从下游拉取更改时，可能会遇到合并冲突。在这种情况下，他们可以要求下游执行合并并自行解决冲突（也许他们更了解如何解决冲突）。这是下游“应该”从上游合并的少数情况之一。

### 补丁工作流程

If you are a contributor that sends changes upstream in the form of emails, you should use topic branches as usual (see above). Then use [git-format-patch[1]](../../1/git-format-patch) to generate the corresponding emails (highly recommended over manually formatting them because it makes the maintainer’s life easier).

​	如果您是通过电子邮件将更改发送给上游的贡献者，则应按照通常的主题分支方式使用（请参阅上面）。然后使用 [git-format-patch[1]](../../1/git-format-patch) 生成相应的电子邮件（强烈推荐比手动格式化它们，因为这样可以使维护者的生活更轻松）。

Recipe: format-patch/am: Publishing branches/topics

步骤：format-patch/am：发布分支/主题

- `git format-patch -M upstream..topic` to turn them into preformatted patch files
- `git format-patch -M upstream..topic` 将它们转换为预格式化的补丁文件
- `git send-email --to=<recipient> <patches>`

See the [git-format-patch[1]](../../1/git-format-patch) and [git-send-email[1]](../../1/git-send-email) manpages for further usage notes.

​	有关更多使用说明，请参阅 [git-format-patch[1]](../../1/git-format-patch) 和 [git-send-email[1]](../../1/git-send-email) 手册。

If the maintainer tells you that your patch no longer applies to the current upstream, you will have to rebase your topic (you cannot use a merge because you cannot format-patch merges):

​	如果维护者告诉您，您的补丁不再适用于当前的上游代码，您将不得不重新基于主题（您不能使用合并，因为您不能格式化合并）：

Recipe: format-patch/am: Keeping topics up to date

#### 步骤：format-patch/am：保持主题最新

```
git pull --rebase <URL> <branch>
```

You can then fix the conflicts during the rebase. Presumably you have not published your topic other than by mail, so rebasing it is not a problem.

​	然后在变基过程中解决冲突。您很可能没有通过邮件发布主题，因此重新基于它不是问题。

If you receive such a patch series (as maintainer, or perhaps as a reader of the mailing list it was sent to), save the mails to files, create a new topic branch and use *git am* to import the commits:

​	如果收到这样的补丁系列（作为维护者，或者作为发送给邮件列表的读者），请将邮件保存到文件中，创建一个新的主题分支，并使用 *git am* 导入提交：

Recipe: format-patch/am: Importing patches

#### 步骤：format-patch/am：导入补丁

```
git am < patch
```

One feature worth pointing out is the three-way merge, which can help if you get conflicts: `git am -3` will use index information contained in patches to figure out the merge base. See [git-am[1]](../../1/git-am) for other options.

​	值得指出的一个功能是三方合并，它可以帮助您解决冲突：`git am -3` 将使用补丁中包含的索引信息来找出合并基准。有关其他选项，请参阅 [git-am[1]](../../1/git-am)。

## 另请参阅

[gittutorial[7]](../../7/gittutorial), [git-push[1]](../../1/git-push), [git-pull[1]](../../1/git-pull), [git-merge[1]](../../1/git-merge), [git-rebase[1]](../../1/git-rebase), [git-format-patch[1]](../../1/git-format-patch), [git-send-email[1]](../../1/git-send-email), [git-am[1]](../../1/git-am)

## 另请参阅

[gittutorial[7]](../gittutorial), [git-push[1]](../../1/git-push), [git-pull[1]](../../1/git-pull), [git-merge[1]](../../1/git-merge), [git-rebase[1]](../../1/git-rebase), [git-format-patch[1]](../../1/git-format-patch), [git-send-email[1]](../../1/git-send-email), [git-am[1]](../../1/git-am)

## GIT

  这是[git[1]](../../Git)工具集中的一部分。