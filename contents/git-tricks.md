# Tricks in Git

exclude files in `git diff`: `git diff ':!<filenames>'`

## Git commit corresponding to a specific line

`git blame`: show what revision and author last modified each line of a file

`git tag --contains <commit>`: show tags that contain the commit

`git log --follow <file>`: show the history of a file, including renames

`git log --pretty=oneline commit0..commit1`: show the commit messages between two commits

## Git Hook

[git hook](https://www.git-scm.com/book/en/v2/Customizing-Git-Git-Hooks) 能让 git 执行特定操作时触发的自定义脚本，可以用于实现 ci、维护不同分支文件、规定代码风格/提交留言格式、邮件提醒等一系列骚操作。

> 需求用例场景：两个分支维护各自不同的文件，可以在 `post-checkout` 中执行 `git clean -x` 清除从上一个分支遗留下来的文件。

钩子被存在 `.git/hooks` 目录下，执行 `git init` 指令时会生成一系列默认以 `sample` 作为后缀名的 git hook 脚本，如：`pre-commit.sample` 会检查提交的代码，如果代码行末存在空格会返回提示并终止 commit 操作。

git 提供的 sample 文件默认不会执行，执行相应的 hook 文件需要将后缀名去掉。hook 文件像 shell 脚本一样开头加上 `#!/path-to-interpreter`，指明使用什么解释器执行，只要可以直接执行的脚本都可以用作 hook 执行脚本，常见使用的语言：bash、python、ruby、perl。

> 自己编写的 hook 文件记得执行 `chmod +x` 加上可执行的权限，没有可执行的人先会提示 `not set as executable`
