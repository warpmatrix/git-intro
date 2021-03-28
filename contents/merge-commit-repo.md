<!-- omit in toc -->
# Merge Commit and Repo

<!-- omit in toc -->
## Table of Contents

- [Commit Merging](#commit-merging)
- [Repository Merging](#repository-merging)
- [Merge 的一些参数](#merge-的一些参数)

## Commit Merging

合并一个仓库中的两个 commit 通常使用指令：

`git rebase -i [startpoint] [endpoint]`

- `-i` 参数表示使用交互式界面完成合并操作
- `[startpoint]` 表示起始位置，通常使用 `HEAD^n` 或 `HEAD~n` 表示在 HEAD 之前的第 n 次提交
- `[endpoint]` 表示结束位置，默认为当前分支的 HEAD

使用 `git rebase -i` 指令，可以让我们修改提交历史，执行该指令后有不同的子指令根据需要执行：

- pick：保留该commit（缩写:p）
- reword：保留该commit，但我需要修改该commit的注释（缩写:r）
- edit：保留该commit, 但我要停下来修改该提交(不仅仅修改注释)（缩写:e）
- squash：将该commit和前一个commit合并（缩写:s）
- fixup：将该commit和前一个commit合并，但我不要保留该提交的注释信息（缩写:f）
- exec：执行shell命令（缩写:x）
- drop：丢弃该commit（缩写:d）

`git rebase` 还有其它的参数进行不同的操作：

- `git rebase [startpoint] [endpoint] --onto [branch-name]`：将提交复制到对应的分支上。
  - 需要注意的是，复制以后的提交是游离状态，还需要使用指令将 master 指向对应提交的 id
- `git rebase --abort` 放弃当前 rebase

```shell
git checkout master
git reset --hard [commit-id]
```

## Repository Merging

合并两个 git 仓库，实际上是对 git 指令的综合应用。这里的合并仓库，可以是和本地仓库的合并，也可以和远程仓库合并。

1. 我们要把被合并的子仓库添加到主仓库的远程仓库列表中，本地仓库使用对应的绝对路径或相对路径添加，远程仓库使用对应的 url 添加即可：

    ```shell
    git remote add [remote-repo-name] [merged-repo-path]
    ```

2. 将子仓库的内容拉到主仓库中。执行指令后，运行 `git branch` 可以看到新增加的远程仓库分支信息。

    ```shell
    git fetch [remote-repo-name] [merged-repo-branch]:[new-branch-name]
    ```

3. 接下来就可以使用 merge 或 rebase 指令进行分支合并。其中，被合并仓库中可能新增之前没有跟踪过的文件，根据需要加上 `--allow-unrelated-histories` 参数。

    ```shell
    git checkout [merging-branch]
    git merge [new-branch-name] --allow-unrelated-histories
    ```

4. 最后，删除对应的远程仓库和分支即可。

    ```shell
    git remote remove [remote-repo-name]
    git branch -d [new-branch-name]
    ```

## Merge 的一些参数

- `no-ff`：不进行 fast-forward 操作，保留原分支的修改信息。可以查看被合并分支的具体情况
- `no-commit`：取消自动 commit，提交前可以进一步修改。利用这个参数用户可以在 commit 之前确定合并的结果没有错误
