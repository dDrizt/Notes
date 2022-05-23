# Git 学习笔记

## Git版本库创建

- 通过`git init`命令可以把当前目录变为Git可以管理的仓库：

```bash
$ git init
Initialized empty Git repository in /Users/michael/learngit/.git/
```

- 仓库创建完成后，当前目录下会多出一个`.git`目录，`ls -ah`命令可以看见当前目录下的所有文件，包括隐藏文件。

## 仓库添加文件步骤

- 我们编写一个`README.md`文件，内容如下：

```markdown
Git is a version control system.
Git is a free software.
```

- 添加文件一共需要两步：

  - 1）用`git add`添加到仓库：

  ```bash
  $ git add README.md
  ```

  - 2）用`git commit`命令将文件提交到仓库：

  ```bash
  $ git commit -m "wrote a README file"
  [master (root-commit) eaadf4e] wrote a readme file
   1 file changed, 2 insertions(+)
   create mode 100644 README.md
  ```

- `commit`一次可以提交多个文件，所以可以`add`多次，最后一并提交。

- 小结：

  - 初始化Git仓库，使用`git init`命令。
  - 添加文件到Git仓库，分两步：
    - 1）使用`git add <file>`，这个命令可以反复多次使用，添加多个文件；
    - 2）使用命令`git commit -m <message>`，完成。

## 时光穿梭机

- 我们先将`README.md`文件改为一下内容：

```bash
Git is a ditributed version control system.
Git is free software.
```

- 运行`git status`命令查看结果：

```bash
// git status命令可以让我们时刻掌握仓库当前的状态
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
```

- 运行`git diff`查看difference

```bash
$ git diff README.md
diff --git a/README.md b/README.md
index 46d49bf..9247db6 100644
--- a/README.md
+++ b/README.md
@@ -1,2 +1,2 @@
-Git is a version control system.
+Git is a distributed version control system.
 Git is free software.
```

- 小结：
  - 使用`git status`命令。
  - 使用`git diff`可以查看修改内容。

### 版本回退

- 我们将文件`README.md`再次修改:

```bash
Git is a distributed version control system.
Git is free software distributed under the GPL.
```

- 然后尝试提交：

```bash
$ git add README.md
$ git commit -m "append GPL"
[master 1094adb] append GPL
 1 file changed, 1 insertion(+), 1 deletion(-)
```

- 当我们把文件修改到一定程度后，可以“保存一个快照”，这个快照在Git中被称为`commit`。如果文件被误删或修改，可以从最近的一个`commit`恢复。

- 现在一共有三个版本被提交Git仓库中：

  - 版本1：wrote a README file

  ```bash
  Git is a version control system.
  Git is free software.
  ```

  - 版本2：add distributed

  ```bash
  Git is a distributed version control system.
  Git is free software.
  ```

  - 版本3：append GPL

  ```bash
  Git is a distributed version control system.
  Git is free software distributed under the GPL.
  ```

- `git log`命令可以显示从最近到最远的提交日志：

```bash
$ git log
commit 1094adb7b9b3807259d8cb349e7df1d4d6477073 (HEAD -> master)
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:06:15 2018 +0800

    append GPL

commit e475afc93c209a690c39c13a46716e8fa000c366
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:03:36 2018 +0800

    add distributed

commit eaadf4e385e865d25c48e7ca9c8395c3f7dfaef0
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 20:59:18 2018 +0800

    wrote a README file
```

- 加上`--pretty=oneline`参数可以简化日志：

```bash
// 前面的一大串数字是commit id
$ git log --pretty=oneline
1094adb7b9b3807259d8cb349e7df1d4d6477073 (HEAD -> master) append GPL
e475afc93c209a690c39c13a46716e8fa000c366 add distributed
eaadf4e385e865d25c48e7ca9c8395c3f7dfaef0 wrote a README file
```

- 使用`git reset`可以回退历史版本，在Git中，用`HEAD`表示当前版本，上一个版本就是`HEAD^`,往上100个版本可以简写成`HEAD~100`。

```bash
$ git reset --hard HEAD^
HEAD is now at e475afc add distribute
```

- 退回历史版本后，如果想要返回就需要使用到`commit id`。

```bash
$ git reset --hard 1094a
HEAD is now at 83b0afe append GPL
```

- `git reflog`可以用来记录每一次命令

```bash
$ git reflog
e475afc HEAD@{1}: reset: moving to HEAD^
1094adb (HEAD -> master) HEAD@{2}: commit: append GPL
e475afc HEAD@{3}: commit: add distributed
eaadf4e HEAD@{4}: commit (initial): wrote a readme file
```

- 小结：
  - 在Git中可以使用`git reset --hard commit_id`命令在各个历史版本之间穿梭。
  - 使用`git log`可以查看提交历史，以便确定要回退到哪个版本。
  - 用`git reflog`查看命令历史，以便回到未来的哪个版本。