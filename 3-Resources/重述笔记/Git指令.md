---
Project: 
Status: 
tags:
  - Resources
  - 软件开发
Deadline: 
CreateTime: 
Connected:
---

## 指令
### 1. **安装 Git**
   - **Windows**：可以从 [Git 官网](https://git-scm.com/) 下载并安装 Git。
   - **macOS**：可以通过 Homebrew 安装，运行命令：`brew install git`。
   - **Linux**：大多数 Linux 发行版可以通过包管理器安装 Git，例如在 Ubuntu 上运行：`sudo apt-get install git`。

### 2. **配置 Git**
   在安装完 Git 之后，需要进行一些基本的配置：

   ```bash
   git config --global user.name "你的名字"
   git config --global user.email "你的邮箱"
   ```

   这些配置用于标识提交记录的作者信息。

### 3. **创建和初始化 Git 仓库**
   - **新项目**：在一个新项目文件夹中，运行以下命令初始化一个 Git 仓库：

     ```bash
     mkdir my_project
     cd my_project
     git init
     ```

   - **已有项目**：如果已经有一个项目文件夹，只需进入该文件夹并运行 `git init`。

### 4. **基本操作：添加、提交和查看状态**
   - **查看状态**：使用以下命令查看当前仓库的状态，包括哪些文件被修改或未跟踪：

     ```bash
     git status
     ```

   - **添加文件到暂存区**：在提交更改之前，需要将文件添加到暂存区：

     ```bash
     git add 文件名      # 添加单个文件
     git add .           # 添加当前目录下的所有文件
     ```

   - **提交更改**：将暂存区的文件提交到仓库：

     ```bash
     git commit -m "提交信息"
     ```

### 5. **查看提交历史**
   使用以下命令查看仓库的提交历史记录：

   ```bash
   git log
   ```

   可以使用 `git log --oneline` 查看简洁的日志信息。

### 6. **远程仓库：克隆、拉取和推送**
   - **克隆远程仓库**：从远程仓库创建本地副本：

     ```bash
     git clone 仓库地址
     ```

   - **拉取最新更改**：从远程仓库获取最新更改并合并到本地分支：

     ```bash
     git pull
     ```

   - **推送更改**：将本地的更改推送到远程仓库：

     ```bash
     git push
     ```

### 7. **分支管理**
   - **创建新分支**：

     ```bash
     git branch 分支名
     ```

   - **切换到指定分支**：

     ```bash
     git checkout 分支名
     ```

   - **创建并切换到新分支**：

     ```bash
     git checkout -b 分支名
     ```

   - **合并分支**：将某个分支的更改合并到当前分支：

     ```bash
     git merge 分支名
     ```
在 Git 中，`switch` 和 `restore` 是较新的命令，它们引入的目的是简化一些常用的 Git 操作，使其更容易理解和使用。这些命令在 Git 2.23.0 版本中被引入，用于替代一些原本需要使用 `checkout` 的场景。以下是它们的详细用法：

- `git switch` 命令

`git switch` 主要用于在不同的分支之间切换。相比于 `git checkout`，`git switch` 更加直观和明确，不涉及文件的更改。以下是一些常见的用法：

- **切换到一个已有的分支：**

    ```bash
    git switch 分支名
    ```

    例如，要切换到 `feature` 分支，可以运行：

    ```bash
    git switch feature
    ```

- **创建并切换到一个新分支：**

    ```bash
    git switch -c 新分支名
    ```

    例如，创建一个名为 `new-feature` 的新分支并切换到该分支：

    ```bash
    git switch -c new-feature
    ```

- **切换到之前的分支：**

    ```bash
    git switch -
    ```

    这相当于在不同的分支之间来回切换（例如从当前分支切换回之前的分支）。

-  `git restore` 命令

`git restore` 主要用于恢复文件的状态，替代一些需要 `checkout` 的场景，尤其是在处理文件的更改时。它可以用于撤销对文件的修改或从历史记录中恢复文件。以下是一些常见的用法：

- **恢复工作目录中的文件到上一个提交的状态（即撤销未暂存的更改）：**

    ```bash
    git restore 文件名
    ```

    例如，要恢复 `file.txt` 文件，可以运行：

    ```bash
    git restore file.txt
    ```

- **恢复暂存区中的文件到上一个提交的状态（即撤销已经暂存的更改）：**

    ```bash
    git restore --staged 文件名
    ```

    例如，要取消对暂存区中 `file.txt` 的修改，可以运行：

    ```bash
    git restore --staged file.txt
    ```

- **从特定的提交中恢复文件：**

    ```bash
    git restore --source=提交ID 文件名
    ```

    例如，从特定的提交 `abc123` 中恢复 `file.txt`：

    ```bash
    git restore --source=abc123 file.txt
    ```


### 8. **解决合并冲突**
   在合并分支时可能会遇到冲突，此时 Git 会标记冲突的文件，需要手动编辑解决冲突，然后：

   ```bash
   git add 冲突文件
   git commit -m "解决冲突"
   ```

### 9. **恢复和回滚更改**
   - **恢复暂存区的文件**：

     ```bash
     git reset 文件名
     ```

   - **回滚到特定提交**：

     ```bash
     git checkout 提交ID -- 文件名
     ```

   - **撤销提交**：

     ```bash
     git revert 提交ID
     ```


## 命令参数
好的，Git 中有许多指令和参数，它们可以用于管理项目、版本控制、协作等。以下是一些常用 Git 指令及其重要的命令参数解析：

### 1. `git init`

- **描述**：初始化一个新的 Git 仓库。
- **常用参数**：
  - `--bare`：创建一个裸仓库，通常用于中央仓库或远程仓库，不包含工作目录。
    
    ```bash
    git init --bare
    ```

### 2. `git clone`

- **描述**：克隆一个远程仓库到本地。
- **常用参数**：
  - `--depth <depth>`：仅克隆指定深度的历史记录，有效减少克隆时的数据量。
    
    ```bash
    git clone --depth 1 仓库地址
    ```
    
  - `--branch <branch>` 或 `-b`：指定克隆某个分支。
    
    ```bash
    git clone -b 分支名 仓库地址
    ```
    
  - `--single-branch`：仅克隆指定分支的历史记录，而不包含其他分支。
    
    ```bash
    git clone --single-branch --branch 分支名 仓库地址
    ```

### 3. `git add`

- **描述**：将文件添加到暂存区（stage）。
- **常用参数**：
  - `-A` 或 `--all`：将所有修改（包括新建、修改和删除）添加到暂存区。
    
    ```bash
    git add -A
    ```
    
  - `.`：添加当前目录及其子目录中的所有更改。
    
    ```bash
    git add .
    ```

### 4. `git commit`

- **描述**：将暂存区的更改提交到本地仓库。
- **常用参数**：
  - `-m "message"`：在命令行中直接添加提交信息。
    
    ```bash
    git commit -m "提交说明"
    ```
    
  - `--amend`：修改上一次的提交，合并新的更改或更新提交信息。
    
    ```bash
    git commit --amend -m "修改后的提交说明"
    ```
    
  - `-a`：自动将所有已跟踪的文件添加到暂存区，然后提交。这不包括新文件。

    ```bash
    git commit -a -m "自动添加已跟踪文件并提交"
    ```

### 5. `git status`

- **描述**：显示当前工作目录和暂存区的状态。
- **常用参数**：
  - `-s` 或 `--short`：简短格式显示状态信息。
    
    ```bash
    git status -s
    ```
    
  - `-b` 或 `--branch`：显示分支信息。
    
    ```bash
    git status -b
    ```

### 6. `git log`

- **描述**：查看提交历史。
- **常用参数**：
  - `--oneline`：以简洁的一行格式显示提交历史。
    
    ```bash
    git log --oneline
    ```
    
  - `--graph`：以 ASCII 图形显示分支和合并历史。
    
    ```bash
    git log --graph
    ```
    
  - `--stat`：显示每次提交更改的文件及行数统计。
    
    ```bash
    git log --stat
    ```
    
  - `-p` 或 `--patch`：显示每个提交的差异内容。
    
    ```bash
    git log -p
    ```

### 7. `git diff`

- **描述**：显示文件之间的差异。
- **常用参数**：
  - `--staged` 或 `--cached`：显示已暂存的文件与上次提交之间的差异。
    
    ```bash
    git diff --staged
    ```
    
  - `--name-only`：仅显示有差异的文件名称。
    
    ```bash
    git diff --name-only
    ```
    
  - `--stat`：显示统计信息，如更改的行数。
    
    ```bash
    git diff --stat
    ```

### 8. `git reset`

- **描述**：重置当前分支的 HEAD 到指定状态。
- **常用参数**：
  - `--soft`：重置 HEAD 到指定提交，保留暂存区和工作目录的更改。
    
    ```bash
    git reset --soft HEAD~1
    ```
    
  - `--mixed`（默认）：重置 HEAD 和暂存区，保留工作目录中的更改。
    
    ```bash
    git reset --mixed HEAD~1
    ```
    
  - `--hard`：重置 HEAD、暂存区和工作目录到指定提交的状态，丢弃所有未提交的更改。
    
    ```bash
    git reset --hard HEAD~1
    ```

### 9. `git merge`

- **描述**：合并两个或多个分支。
- **常用参数**：
  - `--no-ff`：执行非快速合并，即使可以使用快速合并。非快速合并会创建一个新的提交记录，保留分支信息。
    
    ```bash
    git merge --no-ff 分支名
    ```
    
  - `--squash`：将分支上的所有更改压缩成一次提交合并到当前分支。
    
    ```bash
    git merge --squash 分支名
    ```
    
  - `--abort`：取消合并并恢复到合并前的状态。
    
    ```bash
    git merge --abort
    ```

### 10. `git rebase`

- **描述**：将分支的基底更改为另一条分支的基底，保持提交历史的线性。
- **常用参数**：
  - `-i` 或 `--interactive`：交互式 rebase，允许编辑、合并或删除提交。
    
    ```bash
    git rebase -i HEAD~3
    ```
    
  - `--onto`：将分支的基底更改到指定的提交。
    
    ```bash
    git rebase --onto 新基底 提交ID
    ```
    
  - `--continue`：继续已暂停的 rebase 过程。
    
    ```bash
    git rebase --continue
    ```
    
  - `--abort`：取消 rebase 并恢复到 rebase 前的状态。
    
    ```bash
    git rebase --abort
    ```

### 11. `git fetch`

- **描述**：从远程仓库获取最新数据，但不合并到本地分支。
- **常用参数**：
  - `--all`：获取所有远程仓库的数据。
    
    ```bash
    git fetch --all
    ```
    
  - `--prune`：删除本地已被远程仓库删除的分支。
    
    ```bash
    git fetch --prune
    ```
    
  - `--dry-run`：模拟执行 fetch，显示将要获取的内容而不实际执行操作。
    
    ```bash
    git fetch --dry-run
    ```

### 12. `git push`

- **描述**：将本地更改推送到远程仓库。
- **常用参数**：
  - `--set-upstream` 或 `-u`：将当前分支与指定的远程分支关联，以后可以直接使用 `git push` 推送。
    
    ```bash
    git push -u origin 分支名
    ```
    
  - `--force` 或 `-f`：强制推送，即使它会覆盖远程仓库的更改。使用时需谨慎。
    
    ```bash
    git push --force
    ```
    
  - `--tags`：推送本地的所有标签到远程仓库。
    
    ```bash
    git push --tags
    ```

这些命令和参数是 Git 使用中最常见和最有用的工具。掌握它们有助于你有效地管理代码、处理项目协作和维护清晰的提交历史。如果你需要了解更多关于某个特定指令或参数的详细用法，可以继续询问！


当然，这里是对更多 Git 指令及其常用命令参数的解析，以帮助你更深入了解 Git 的使用方法：

### 13. `git pull`

- **描述**：从远程仓库获取最新数据并合并到当前分支。`git pull` 实际上是 `git fetch` 和 `git merge` 的组合。
- **常用参数**：
  - `--rebase`：获取最新更改后，使用 `rebase` 而非 `merge` 来合并，保持提交历史的线性。
    
    ```bash
    git pull --rebase
    ```
    
  - `--no-commit`：拉取更改后不自动创建合并提交，允许你先查看和手动调整。
    
    ```bash
    git pull --no-commit
    ```
    
  - `--ff-only`：只执行快速合并，如果需要非快速合并（即存在冲突或不同分支历史），则操作失败。
    
    ```bash
    git pull --ff-only
    ```

### 14. `git stash`

- **描述**：暂时保存当前工作目录的修改，使工作目录干净以便执行其他操作。之后可以恢复这些修改。
- **常用参数**：
  - `save "message"`：保存工作目录中的更改并添加描述信息。使用时推荐提供描述信息。
    
    ```bash
    git stash save "保存的信息"
    ```
    
  - `apply`：恢复最新保存的更改到工作目录，但不从栈中移除。
    
    ```bash
    git stash apply
    ```
    
  - `pop`：恢复最新保存的更改到工作目录，并从栈中移除。
    
    ```bash
    git stash pop
    ```
    
  - `list`：列出所有保存的更改。
    
    ```bash
    git stash list
    ```
    
  - `drop`：从栈中删除指定的 stash 条目。
    
    ```bash
    git stash drop stash@{0}
    ```

### 15. `git cherry-pick`

- **描述**：将一个或多个特定的提交应用到当前分支。这在从一个分支中选择性地应用更改时很有用。
- **常用参数**：
  - `-n` 或 `--no-commit`：应用更改但不立即提交，允许在应用更改后进行修改。
    
    ```bash
    git cherry-pick -n 提交ID
    ```
    
  - `-x`：在提交信息中添加 `cherry-picked from commit` 的信息，以记录原始提交的来源。
    
    ```bash
    git cherry-pick -x 提交ID
    ```
    
  - `--edit` 或 `-e`：在提交前编辑提交信息。
    
    ```bash
    git cherry-pick -e 提交ID
    ```

### 16. `git tag`

- **描述**：创建标签，用于标记特定的提交，常用于发布版本标记。
- **常用参数**：
  - `-a`：创建带注释的标签，允许提供详细的标签信息。
    
    ```bash
    git tag -a v1.0 -m "版本 1.0 发布"
    ```
    
  - `-l` 或 `--list`：列出所有标签，或匹配指定模式的标签。
    
    ```bash
    git tag -l "v1.*"
    ```
    
  - `-d` 或 `--delete`：删除指定的标签。
    
    ```bash
    git tag -d v1.0
    ```
    
  - `--sign` 或 `-s`：创建带签名的标签，需要使用 GPG 签名。
    
    ```bash
    git tag -s v1.0 -m "版本 1.0 发布"
    ```

### 17. `git remote`

- **描述**：管理远程仓库连接，包括添加、删除和查看远程仓库。
- **常用参数**：
  - `add <name> <url>`：添加一个新的远程仓库。
    
    ```bash
    git remote add origin https://github.com/user/repo.git
    ```
    
  - `remove <name>`：删除指定的远程仓库。
    
    ```bash
    git remote remove origin
    ```
    
  - `-v` 或 `--verbose`：显示所有远程仓库的详细信息，包括获取（fetch）和推送（push）的 URL。
    
    ```bash
    git remote -v
    ```
    
  - `rename <old-name> <new-name>`：重命名一个远程仓库。
    
    ```bash
    git remote rename origin upstream
    ```

### 18. `git rm`

- **描述**：从工作目录和暂存区中删除文件。
- **常用参数**：
  - `-f` 或 `--force`：强制删除，即使文件有未提交的更改。
    
    ```bash
    git rm -f 文件名
    ```
    
  - `--cached`：仅从暂存区删除文件，保留工作目录中的文件。这相当于撤销 `git add` 操作。
    
    ```bash
    git rm --cached 文件名
    ```
    
  - `-r`：递归删除目录及其内容。
    
    ```bash
    git rm -r 目录名
    ```

### 19. `git mv`

- **描述**：重命名文件或移动文件，同时保持版本控制。
- **常用参数**：
  - 无参数使用：指定源文件和目标文件位置即可。

    ```bash
    git mv 原文件名 新文件名
    ```

