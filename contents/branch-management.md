<!-- omit in toc -->
# Branch Management

<!-- omit in toc -->
## Table of Contents

- [What's Branch](#whats-branch)
- [Command List](#command-list)
- [Branch Workflow](#branch-workflow)
  - [Mini Project](#mini-project)
  - [Git Flow](#git-flow)
- [Summary](#summary)

## What's Branch

Git 中的分支可以联系生活的分支理解，不同的开发者从同一个地方出发，开发不同的功能；等到开发完毕，在重新汇聚起来。

## Command List

之前已经介绍本地分支的相关指令，还有一些远程分支相关的指令需要学习。

- 查看远程分支

    ```shell
    git branch -r
    ```

- 推送分支到远程仓库

    ```shell
    git push origin <branch>
    # 如果想对远程分支取名，可以使用（一般不建议）
    git push origin <branch>:<branchname>
    ```

- 删除远程分支

    ```shell
    git push origin :<branch>
    ```

- 将远程分支迁移到本地

    ```shell
    git branch <branchname> origin/<branch>
    # 同样，迁移并切换分支
    git checkout -b <branchname> origin/<branch>
    ```

## Branch Workflow

### Mini Project

一般来说，如果是一人开发的小项目，可能只需要 master、develop 两个分支就 ok，平时开发在 develop 分支进行，开发完成之后，发布之前合并到 master 分支即可。

### Git Flow

如果是一个小团队进行相当大小的项目开发，就需要一定的分支管理规范。特别常见的是，团队正在开发时，突然发现线上有一个严重的 bug，不得不停下手头的工作优先处理 bug，而且很多时候多人协作下如果没有一个规范，很容易出问题。

因此，有人提出了 [Git Flow][1]，一种比较成熟的分支管理流程。

Git Flow 将分支分成五类：

1. master

    master 分支存放项目的稳定版本，一个项目只有一个 master 分支，是发布产品 (production-ready) 的所在分支。

    这个分支只能从其他分支合并进来，严格禁止在master分支修改代码。

2. develop

    develop 分支是我们是我们的主开发分支。每一次的需求迭代开发都是从这个 develop 分支上来拉一个新的 feature 分支来进行开发，记录产品最新的开发状态。

    同样，一个项目也只有一个 develop 分支，一般也是不允许直接对 develop 分支进行代码的修改。

3. feature

    feature 分支基于 develop 分支，主要是用来开发一个新的功能。当有产品需求出现时，就创建一个 feture 分支。

    一旦开发完成，release 分支将合并到 develop 分支，然后删除分支。

4. release

    release 分支是准备发布版本的分支。当 develop 完成新的需求时且认为足够稳定时，就会从 develop 拉出 release 分支。在该分支上，进行上线的最后测试，发现 bug 即在本分支修复 bug。

    完成 release 分支后，要将 release 分支合并回 develop 分支 和 master 分支。而且，一般 release 分支合并到 master 分支上后，我们会在 master 分支上打相应的版本号 tag。

    完成所有工作后，会删除 release 分支。

5. hotfix

    hotfix 分支是用于紧急修复 master 分支上的 bug 所创建的分支。

    完成 bug 的修复工作后，会合并回 master 分支 和 develop 分支，并且和 release 分支一样，会在 master 分支上打相应的版本号 tag，随即删除 hotfix 分支。

结合下图，再重新理解 Git Flow 流程，应该不是难事。

![git flow](/images/git-flow.jpg)

## Summary

分支管理在团队协作中非常重要。使用 Git Flow 能极大避免开发中遇到的生产问题，而且 Git Flow 有相应的脚本插件，还有 GUI 程序，十分方便使用。

当然，Git Flow 只是一种使用较为广泛的 Git 工作流程，也有其不适合使用的场景。还有很多其他的工作流程，如：Github Flow、Gitlab Flow 等。使用哪种工作流程还要看具体的场景需求。

[1]: https://github.com/nvie/gitflow
