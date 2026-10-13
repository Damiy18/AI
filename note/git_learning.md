# Git 完整使用指南

> 适用环境：macOS + Git + GitHub
> 难度：入门到进阶
> 目标：掌握代码版本管理、分支开发、GitHub 同步和常见问题处理。

---

# 目录

1. [Git 基础概念](#1-git-基础概念)
2. [Git 工作原理](#2-git-工作原理)
3. [常用命令速查](#3-常用命令速查)
4. [日常开发完整流程](#4-日常开发完整流程)
5. [仓库管理](#5-仓库管理)
6. [分支管理](#6-分支管理)
7. [提交管理](#7-提交管理)
8. [远程仓库与 GitHub](#8-远程仓库与-github)
9. [撤销与恢复](#9-撤销与恢复)
10. [冲突处理](#10-冲突处理)
11. [`.gitignore` 忽略文件](#11-gitignore-忽略文件)
12. [标签与版本发布](#12-标签与版本发布)
13. [进阶与低频命令](#13-进阶与低频命令)
14. [常见问题解决](#14-常见问题解决)
15. [Git 使用习惯与最佳实践](#15-git-使用习惯与最佳实践)
16. [命令速查表](#16-命令速查表)

---

# 1. Git 基础概念

## 1.1 Git 是什么？

Git 是一个分布式版本控制系统，用来记录代码的变化、管理不同版本，并支持多人协作。

它能帮助你：

* 保存代码历史。
* 回到之前的版本。
* 同时开发不同功能。
* 将代码上传到 GitHub。
* 查看每次修改了什么。
* 与其他开发者协作。

## 1.2 Git 和 GitHub 的区别

| 名称             | 作用                     |
| -------------- | ---------------------- |
| Git            | 本地的版本控制工具              |
| GitHub         | 托管 Git 仓库的在线平台         |
| Repository（仓库） | 项目及其版本历史               |
| Commit（提交）     | 一次版本记录                 |
| Branch（分支）     | 独立的开发路线                |
| Remote（远程仓库）   | 另一个 Git 仓库，通常位于 GitHub |

Git 不等于 GitHub。即使没有网络，你也可以在本地使用 Git 提交代码；联网后再把提交推送到 GitHub。

## 1.3 核心术语

* **工作区（Working Directory）**：你实际编辑文件的地方。
* **暂存区（Staging Area）**：准备放进下一次提交的修改。
* **本地仓库（Local Repository）**：保存本地提交历史的 Git 数据。
* **远程仓库（Remote Repository）**：例如 GitHub 上的仓库。
* **HEAD**：通常指向当前所在分支的最新提交。
* **origin**：常见的远程仓库名称，并不是固定名称。
* **main**：常见的主分支名称。
* **origin/main**：本地记录的远程 `main` 分支状态。

---

# 2. Git 工作原理

Git 的基本流程是：

```text
编辑文件
   ↓
git add
   ↓
暂存区
   ↓
git commit
   ↓
本地仓库
   ↓
git push
   ↓
GitHub 远程仓库
```

从远程获取代码的流程则通常是：

```text
GitHub 远程仓库
   ↓
git fetch
   ↓
更新远程跟踪分支
   ↓
git merge 或 git rebase
   ↓
当前本地分支
```

`git pull` 通常会将获取远程更新和整合更新合并成一个操作。

## 2.1 三个最重要的命令

```bash
git add .
git commit -m "描述本次修改"
git push
```

它们分别代表：

1. `add`：选择要提交的修改。
2. `commit`：在本地保存一个版本。
3. `push`：将本地提交上传到远程仓库。

注意：`git add .` 不会上传代码，`git commit` 也不会自动上传代码。

---

# 3. 常用命令速查

本章是日常最值得熟悉的部分。

## 3.1 查看状态和历史

| 命令                  | 作用          |
| ------------------- | ----------- |
| `git status`        | 查看当前修改和暂存情况 |
| `git branch`        | 查看本地分支      |
| `git branch -a`     | 查看本地和远程跟踪分支 |
| `git log --oneline` | 简洁查看提交历史    |
| `git diff`          | 查看尚未暂存的修改   |
| `git diff --staged` | 查看已暂存的修改    |
| `git remote -v`     | 查看远程仓库地址    |

最常用的检查命令是：

```bash
git status
```

不确定当前 Git 状态时，优先执行它。

## 3.2 添加、提交和上传

```bash
git add .
git commit -m "Add note feature"
git push
```

| 命令                       | 作用               |
| ------------------------ | ---------------- |
| `git add 文件名`            | 暂存指定文件           |
| `git add .`              | 暂存当前目录下的相关修改     |
| `git add -A`             | 暂存仓库范围内的新增、修改和删除 |
| `git commit -m "说明"`     | 创建提交             |
| `git push`               | 推送当前分支           |
| `git push -u origin 分支名` | 首次推送分支并设置上游      |
| `git pull`               | 获取并整合远程更新        |

示例：

```bash
git add note/hello_note.py
git commit -m "Add note creation function"
git push
```

如果只想提交某个文件，使用文件路径通常比 `git add .` 更精确。

## 3.3 分支操作

```bash
git branch
git switch -c note
git switch main
git switch note
```

| 命令                   | 作用               |
| -------------------- | ---------------- |
| `git branch`         | 查看本地分支           |
| `git branch note`    | 创建 `note` 分支但不切换 |
| `git switch -c note` | 创建并切换到 `note`    |
| `git switch main`    | 切换到 `main`       |
| `git merge note`     | 把 `note` 合并到当前分支 |
| `git branch -d note` | 删除已合并的本地分支       |

注意：`git merge note` 的目标是**当前分支**。如果要把 `note` 合并到 `main`，先执行 `git switch main`。

## 3.4 远程仓库操作

```bash
git remote -v
git remote add origin 远程仓库地址
git remote set-url origin 新地址
git fetch origin
git pull
git push
```

| 命令                              | 作用                 |
| ------------------------------- | ------------------ |
| `git remote -v`                 | 查看远程地址             |
| `git remote add origin URL`     | 添加远程仓库             |
| `git remote set-url origin URL` | 修改远程地址             |
| `git fetch origin`              | 获取远程更新，但不直接整合到当前分支 |
| `git pull`                      | 获取并整合远程更新          |
| `git push`                      | 上传提交               |

## 3.5 撤销未提交的修改

```bash
git restore 文件名
git restore --staged 文件名
```

* `git restore 文件名`：丢弃指定文件未暂存的修改。
* `git restore --staged 文件名`：将文件从暂存区移出，但保留工作区修改。

**警告：** `git restore 文件名` 可能丢失该文件尚未提交的修改。执行前确认不需要保留这些内容。

---

# 4. 日常开发完整流程

以下用 `macproject` 目录下的 `AI` 项目为例。

## 4.1 进入项目目录

```bash
cd /Users/damiy/macproject
```

确认目录是否为 Git 仓库：

```bash
git status
```

## 4.2 开始写代码

例如修改：

```text
first/hello.py
```

或者创建：

```text
note/hello_note.py
```

## 4.3 检查修改

```bash
git status
git diff
```

## 4.4 暂存修改

```bash
git add .
```

如果只想暂存一个文件：

```bash
git add note/hello_note.py
```

## 4.5 创建提交

```bash
git commit -m "Add note feature"
```

## 4.6 推送到 GitHub

```bash
git push
```

如果当前分支是第一次推送：

```bash
git push -u origin note
```

## 4.7 推荐的日常循环

```bash
git status
git add .
git commit -m "Describe the change"
git push
```

如果需要先同步团队或远程仓库的更新，再根据当前分支和工作区状态执行 `git pull`。

---

# 5. 仓库管理

## 5.1 初始化新仓库

```bash
mkdir myproject
cd myproject
git init
```

初始化后，当前目录就有了 Git 仓库。

检查状态：

```bash
git status
```

## 5.2 克隆现有仓库

```bash
git clone git@github.com:用户名/仓库名.git
```

例如：

```bash
git clone git@github.com:Damiy18/AI.git
```

克隆会创建一个新的本地目录，并下载仓库历史。

## 5.3 查看仓库根目录

```bash
git rev-parse --show-toplevel
```

## 5.4 查看 Git 配置

```bash
git config --global --list
git config --local --list
```

* `--global`：用户级配置。
* `--local`：当前仓库的配置。

## 5.5 设置提交者信息

```bash
git config --global user.name "你的名字"
git config --global user.email "你的邮箱"
```

查看当前设置：

```bash
git config --global user.name
git config --global user.email
```

这些信息会记录在提交中。它们与 GitHub 的登录认证不是同一件事。

## 5.6 退出或取消 Git 仓库管理

如果确实要让一个目录不再由 Git 管理，可以删除其中的 `.git` 元数据目录，但这会删除该仓库的本地提交历史和配置。

不要随意执行：

```bash
rm -rf .git
```

如果只是想换 GitHub 远程仓库，一般修改 `origin` 地址就够了，无需删除 `.git`。

---

# 6. 分支管理

## 6.1 什么是分支？

分支是项目中的不同开发路线。

例如：

```text
AI 仓库
├── main
├── note
├── feature-chat
└── experiment
```

* `main`：项目主线。
* `note`：笔记功能。
* `feature-chat`：聊天功能。
* `experiment`：实验性功能。

分支属于同一个仓库，不等于新的仓库。

## 6.2 创建并切换分支

```bash
git switch -c note
```

等价于先创建再切换：

```bash
git branch note
git switch note
```

## 6.3 切换分支

```bash
git switch main
git switch note
```

## 6.4 合并分支

如果想把 `note` 合并进 `main`：

```bash
git switch main
git merge note
```

如果合并产生冲突，先解决冲突并完成合并，再推送：

```bash
git push
```

## 6.5 推送新分支到 GitHub

```bash
git switch note
git push -u origin note
```

之后通常可以直接使用：

```bash
git push
```

## 6.6 查看远程分支

```bash
git branch -r
```

查看所有分支：

```bash
git branch -a
```

## 6.7 删除本地分支

```bash
git branch -d note
```

`-d` 通常只允许删除已合并的分支。

强制删除未合并的本地分支：

```bash
git branch -D note
```

谨慎使用 `-D`，它可能让尚未合并的工作更难找回。

## 6.8 删除远程分支

```bash
git push origin --delete note
```

这会删除 GitHub 上的 `note` 分支，但不会自动删除其他开发者电脑上的本地分支。

## 6.9 合并前检查

```bash
git status
git switch main
git pull
git merge note
git push
```

这是一种常见的合并流程，但如果你有未提交的修改，应先处理这些修改。

---

# 7. 提交管理

## 7.1 查看提交历史

```bash
git log
```

简洁版：

```bash
git log --oneline
```

查看分支图：

```bash
git log --oneline --graph --decorate --all
```

## 7.2 查看某次提交

```bash
git show 提交ID
```

例如：

```bash
git show 9d23db0
```

提交 ID 可以使用完整值，也可以使用足够唯一的短 ID。

## 7.3 修改最近一次提交说明

```bash
git commit --amend -m "Correct commit message"
```

如果最近一次提交还没有推送，这通常比较方便。

如果该提交已经推送，修改历史后可能需要强制更新远程分支。多人协作时不要随意改写共享历史。

## 7.4 将文件加入下一次提交

```bash
git add 文件名
git commit -m "Update file"
```

Git 提交的是暂存区中的内容，而不是自动提交工作区的所有修改。

## 7.5 恢复已删除但尚未提交的文件

如果删除操作尚未提交，且希望恢复到当前提交的版本：

```bash
git restore 文件名
```

这会丢弃该文件的工作区修改。

如果删除已经提交，则可以从历史版本恢复，或者使用 `git revert` 等方式修正提交，具体取决于你要恢复什么内容。

---

# 8. 远程仓库与 GitHub

## 8.1 查看远程地址

```bash
git remote -v
```

示例输出：

```text
origin  git@github.com:Damiy18/AI.git (fetch)
origin  git@github.com:Damiy18/AI.git (push)
```

## 8.2 添加远程仓库

```bash
git remote add origin git@github.com:用户名/仓库名.git
```

如果已经存在 `origin`，不要重复添加。先检查：

```bash
git remote -v
```

## 8.3 更换远程仓库

```bash
git remote set-url origin git@github.com:用户名/新仓库名.git
```

这只会修改远程地址，不会自动把本地代码迁移或上传到新仓库。

## 8.4 从远程获取更新

```bash
git fetch origin
```

这会获取远程提交和分支信息，但不会自动把它们合并到当前分支。

## 8.5 拉取并整合更新

```bash
git pull
```

如果需要明确指定远程分支：

```bash
git pull origin main
```

如果本地和远程有不同的提交历史，Git 可能要求你明确选择合并或变基策略。

## 8.6 推送分支

```bash
git push -u origin main
```

首次推送时使用 `-u`，之后通常可以直接执行：

```bash
git push
```

## 8.7 SSH 认证检查

```bash
ssh -T git@github.com
```

认证成功时，GitHub 会显示类似：

```text
Hi username! You've successfully authenticated,
but GitHub does not provide shell access.
```

这表示 SSH 认证成功，并不是错误信息。

---

# 9. 撤销与恢复

撤销操作需要格外谨慎，因为有些命令会直接丢弃修改。

## 9.1 撤销未暂存的修改

```bash
git restore 文件名
```

恢复到当前提交中的版本。

## 9.2 取消暂存

```bash
git restore --staged 文件名
```

文件仍然保留在工作区，只是从暂存区移出。

## 9.3 撤销最近一次提交，但保留修改

```bash
git reset --soft HEAD~1
```

最近一次提交会被撤销，修改仍然保留在暂存区。

## 9.4 撤销最近一次提交，保留工作区修改

```bash
git reset --mixed HEAD~1
```

这是 `git reset HEAD~1` 的默认模式：提交被撤销，修改仍在工作区，但不会保留在暂存区。

## 9.5 撤销提交并丢弃修改

```bash
git reset --hard HEAD~1
```

**高风险命令：** 它会移动当前分支指针，并覆盖工作区和暂存区中的相关内容。未保存的修改可能因此丢失。

执行前先确认自己理解其影响。

## 9.6 撤销已经推送的提交

如果提交已经共享给其他人，通常优先考虑：

```bash
git revert 提交ID
```

`git revert` 会创建一个新的提交，用反向修改抵消指定提交的效果，而不是直接删除历史。

## 9.7 查看以前的操作记录

```bash
git reflog
```

`reflog` 会记录本地 HEAD 和分支引用最近的变动，有时可以帮助找回误操作前的提交。

但它不是永久备份，不能保证所有丢失的未提交文件都能恢复。

---

# 10. 冲突处理

## 10.1 为什么会出现冲突？

当不同分支对同一区域做出 Git 无法自动协调的修改时，合并可能产生冲突。

例如：

* `main` 修改了 `hello.py` 的第 10 行。
* `note` 也修改了同一位置。
* Git 无法判断应该保留哪个版本。

## 10.2 冲突文件的样子

```text
<<<<<<< HEAD
当前分支的内容
=======
另一个分支的内容
>>>>>>> note
```

这些标记用于区分冲突双方。

## 10.3 解决流程

第一步：查看状态。

```bash
git status
```

第二步：打开冲突文件，决定最终要保留的内容。

第三步：删除所有冲突标记，保存文件。

第四步：暂存已解决的文件。

```bash
git add 文件名
```

第五步：完成合并。

如果是普通合并：

```bash
git commit
```

如果正在进行 rebase：

```bash
git rebase --continue
```

最后，根据需要推送：

```bash
git push
```

## 10.4 取消正在进行的操作

取消普通合并：

```bash
git merge --abort
```

取消 rebase：

```bash
git rebase --abort
```

这些命令只适用于对应操作正在进行时。执行前确认没有需要保留的工作区修改。

---

# 11. `.gitignore` 忽略文件

`.gitignore` 用来告诉 Git 哪些尚未被跟踪的文件或路径通常不需要纳入版本控制。

## 11.1 创建文件

在仓库根目录创建：

```bash
touch .gitignore
```

## 11.2 Python 项目示例

可以根据实际项目选择规则：

```gitignore
# Python 缓存
__pycache__/
*.py[cod]

# 虚拟环境
.venv/
venv/

# 环境变量和密钥
.env
.env.*

# macOS 系统文件
.DS_Store

# 编辑器设置
.vscode/
.idea/
```

注意：如果确实需要共享某些配置文件，应按项目需要调整规则；不要把 API Key、密码或访问令牌提交到仓库。

## 11.3 查看文件是否被忽略

```bash
git check-ignore -v 文件名
```

## 11.4 已经被跟踪的文件怎么办？

`.gitignore` 不会自动停止跟踪已提交的文件。

如果希望 Git 不再跟踪某个文件，但保留本地文件，可以使用：

```bash
git rm --cached 文件名
git commit -m "Stop tracking file"
```

对于整个目录也可以使用相应目录路径，但操作前务必确认路径正确。

如果文件曾经包含真实密钥，仅停止跟踪是不够的：还应撤销或轮换密钥，并根据风险处理 Git 历史。

---

# 12. 标签与版本发布

标签（Tag）通常用于标记某个重要版本，例如 `v1.0.0`。

## 12.1 创建轻量标签

```bash
git tag v1.0.0
```

## 12.2 创建附注标签

```bash
git tag -a v1.0.0 -m "Release version 1.0.0"
```

## 12.3 查看标签

```bash
git tag
```

## 12.4 查看标签内容

```bash
git show v1.0.0
```

## 12.5 推送标签

```bash
git push origin v1.0.0
```

推送所有本地标签：

```bash
git push origin --tags
```

## 12.6 删除标签

删除本地标签：

```bash
git tag -d v1.0.0
```

删除远程标签：

```bash
git push origin --delete v1.0.0
```

删除标签前，确认没有其他人或发布流程依赖它。

---

# 13. 进阶与低频命令

本章集中整理日常不一定需要使用，但遇到特定情况很有帮助的命令。

## 13.1 `git rebase`：变基

```bash
git switch note
git rebase main
```

作用：将当前分支的提交重新应用到另一个基准提交之后，使历史看起来更线性。

适合：整理个人功能分支的提交历史。

注意：rebase 会改写提交历史。不要随意对其他人已经依赖的共享提交做变基。

## 13.2 `git cherry-pick`：挑选提交

```bash
git cherry-pick 提交ID
```

作用：把某个提交的修改应用到当前分支，并通常创建一个新的提交。

适合：只想把另一个分支的一项修复带到当前分支，而不是合并整个分支。

## 13.3 `git stash`：临时收起修改

```bash
git stash push -m "temporary work"
```

查看暂存的工作：

```bash
git stash list
```

恢复最近一次收起的工作并保留 stash 记录：

```bash
git stash apply
```

恢复最近一次工作并删除对应 stash 记录：

```bash
git stash pop
```

包括未跟踪文件一起收起：

```bash
git stash push -u -m "include untracked files"
```

适合：需要临时切换分支，但又不想立即提交当前工作。

注意：stash 不是永久备份；恢复时也可能发生冲突。

## 13.4 `git diff`：比较差异

比较工作区和暂存区：

```bash
git diff
```

比较暂存区和最近一次提交：

```bash
git diff --staged
```

比较两个分支：

```bash
git diff main..note
```

比较两个提交：

```bash
git diff 提交ID1 提交ID2
```

## 13.5 `git show`：查看对象或提交

```bash
git show HEAD
git show HEAD~1
git show 提交ID
```

可以查看提交说明、提交涉及的文件和具体修改。

## 13.6 `git blame`：查看每行代码的提交信息

```bash
git blame 文件名
```

适合：排查一段代码是什么时候被修改的，以及对应哪个提交。

它显示的是版本历史中的提交归属，不代表一定能确定实际编写者或修改原因。

## 13.7 `git bisect`：二分查找引入问题的提交

开始排查：

```bash
git bisect start
git bisect bad
git bisect good 已知正常的提交ID
```

Git 会选择一个中间提交供你测试。

根据测试结果标记：

```bash
git bisect good
```

或：

```bash
git bisect bad
```

重复测试，直到找到引入问题的提交。

结束：

```bash
git bisect reset
```

适合：项目历史较长，且能明确判断某个版本正常或异常的情况。

## 13.8 `git clean`：清理未跟踪文件

先预览将要删除的文件：

```bash
git clean -n
```

预览目录：

```bash
git clean -nd
```

确认后删除未跟踪文件：

```bash
git clean -f
```

删除未跟踪文件和目录：

```bash
git clean -fd
```

**高风险：** 这些命令可能删除尚未被 Git 跟踪的新文件。先预览，确认后再执行。

## 13.9 `git worktree`：一个仓库同时使用多个工作目录

创建另一个工作目录并检出 `note` 分支：

```bash
git worktree add ../macproject-note note
```

查看：

```bash
git worktree list
```

移除工作目录：

```bash
git worktree remove ../macproject-note
```

适合：需要同时在两个分支工作，又不想频繁切换当前目录的场景。

## 13.10 `git tag` 与 `git log` 组合查看版本

```bash
git log --oneline --decorate --graph --all
```

可以同时查看分支、标签和提交之间的关系。

## 13.11 `git archive`：导出项目快照

```bash
git archive --format=zip --output=source.zip HEAD
```

会将当前提交中的受版本控制文件导出为 ZIP 文件，不包含完整 Git 历史。

## 13.12 `git ls-files`：查看被跟踪的文件

```bash
git ls-files
```

适合：检查哪些文件已经被 Git 跟踪。

## 13.13 `git remote show`：查看远程仓库详情

```bash
git remote show origin
```

可以查看远程仓库的分支跟踪和部分同步信息。

## 13.14 `git config`：配置常用行为

设置默认初始分支名：

```bash
git config --global init.defaultBranch main
```

设置默认拉取策略为合并：

```bash
git config --global pull.rebase false
```

这些是可选设置，不是所有项目都必须这样配置。

## 13.15 `git reflog`：查找本地历史操作

```bash
git reflog
```

如果不小心切换了分支、重置了提交，可能可以从 reflog 找到之前的提交位置。

查看后可以根据记录中的提交 ID 决定如何恢复。不要盲目执行 reset，先确认当前分支和工作区状态。

## 13.16 `git submodule`：管理子模块

```bash
git submodule add 仓库地址 路径
git submodule update --init --recursive
```

适合：一个项目需要引用另一个独立 Git 仓库的场景。

子模块有自己的版本和提交关系，初学阶段通常不需要使用。

---

# 14. 常见问题解决

## 14.1 `fatal: not a git repository`

原因：当前目录不在 Git 仓库内，或进入了错误目录。

解决：

```bash
pwd
cd /Users/damiy/macproject
git status
```

如果这是一个全新项目，再考虑执行 `git init`。

## 14.2 `Author identity unknown`

原因：Git 尚未配置提交者信息。

解决：

```bash
git config --global user.name "你的名字"
git config --global user.email "你的邮箱"
```

## 14.3 `remote origin already exists`

原因：已经存在名为 `origin` 的远程仓库。

检查：

```bash
git remote -v
```

需要更换地址时：

```bash
git remote set-url origin 新仓库地址
```

## 14.4 `rejected ... (fetch first)`

原因：远程分支包含本地没有的提交。

先检查：

```bash
git status
git fetch origin
git log --oneline --graph --decorate --all
```

然后根据情况选择合并或变基。

如果本地和远程是两个独立初始化的历史，可能需要在确认内容后使用：

```bash
git pull --no-rebase origin main --allow-unrelated-histories
```

如果发生冲突，按冲突处理流程解决。

不要一遇到拒绝就使用强制推送。

## 14.5 `Permission denied (publickey)`

原因：SSH 密钥未正确配置、未添加到 GitHub，或使用了错误的账号密钥。

检查：

```bash
ssh -T git@github.com
git remote -v
```

确认 SSH 公钥已经添加到正确的 GitHub 账号，并检查远程地址。

## 14.6 `nothing to commit, working tree clean`

意思是：当前没有 Git 检测到的待提交修改。

可以检查：

```bash
git status
```

如果刚创建的文件没有出现，确认它是否被 `.gitignore` 忽略，以及是否位于当前仓库内。

## 14.7 `src refspec main does not match any`

常见原因：本地还没有 `main` 分支，或尚未创建任何提交。

检查：

```bash
git branch
git log --oneline
```

先创建需要的提交，再推送正确的分支名。

## 14.8 `Your branch is ahead of 'origin/main'`

意思是：本地分支比记录的远程分支多出一些提交。

如果想上传：

```bash
git push
```

## 14.9 `Your branch is behind 'origin/main'`

意思是：远程跟踪分支有本地尚未整合的提交。

先检查工作区：

```bash
git status
```

确认没有需要保护的修改后，再根据项目策略执行 `git pull` 或先 `git fetch` 检查差异。

## 14.10 合并时出现冲突

执行：

```bash
git status
```

打开冲突文件，编辑成最终想要的内容，然后：

```bash
git add 冲突文件
git commit
```

如果正在进行 rebase，则使用：

```bash
git rebase --continue
```

## 14.11 不小心提交了不该上传的密钥

1. 立即撤销或轮换泄露的密钥。
2. 停止继续提交该文件，并按需要设置 `.gitignore`。
3. 如果密钥已经进入 Git 历史，评估是否需要清理历史并协调其他协作者。
4. 不要以为删除当前文件就代表历史中的密钥也消失了。

---

# 15. Git 使用习惯与最佳实践

## 15.1 提交说明要清楚

推荐：

```bash
git commit -m "Add note creation function"
git commit -m "Fix note saving bug"
git commit -m "Update README"
```

不推荐：

```bash
git commit -m "update"
git commit -m "test"
git commit -m "aaa"
```

## 15.2 一次提交尽量聚焦一件事

例如：

* 添加笔记创建功能。
* 修复笔记保存问题。
* 更新项目文档。

这样以后查看历史或回退修改会更方便。

## 15.3 开发新功能时使用分支

例如：

```bash
git switch main
git pull
git switch -c feature-note-search
```

开发完成并提交后，可以合并回 `main`。

## 15.4 提交前检查状态

```bash
git status
git diff
git diff --staged
```

确认没有把不相关的文件、密钥或临时文件放进提交。

## 15.5 不要随意强制推送

以下命令可能覆盖远程分支上的提交：

```bash
git push --force
```

如果确实需要更新自己改写过的远程历史，并且已经确认不会覆盖他人的工作，通常优先考虑：

```bash
git push --force-with-lease
```

但它仍然可能改写远程历史，团队协作时应先沟通。

## 15.6 GitHub 不是自动备份工具

只有已经提交并推送的内容才会同步到 GitHub。未提交的本地修改仍然只存在于本地工作区。

重要项目还应考虑其他备份方式。

## 15.7 保护敏感信息

不要提交：

* 密码。
* API Key。
* 私钥。
* 访问令牌。
* 包含敏感信息的环境变量文件。

公开仓库尤其要注意这些内容。

---

# 16. 命令速查表

## 每天最常用

| 目的     | 命令                   |
| ------ | -------------------- |
| 查看状态   | `git status`         |
| 查看修改   | `git diff`           |
| 暂存修改   | `git add .`          |
| 提交版本   | `git commit -m "说明"` |
| 推送代码   | `git push`           |
| 拉取更新   | `git pull`           |
| 查看历史   | `git log --oneline`  |
| 查看分支   | `git branch`         |
| 创建分支   | `git switch -c 分支名`  |
| 切换分支   | `git switch 分支名`     |
| 合并分支   | `git merge 分支名`      |
| 查看远程地址 | `git remote -v`      |

## 偶尔使用

| 目的      | 命令                               |
| ------- | -------------------------------- |
| 初始化仓库   | `git init`                       |
| 克隆仓库    | `git clone URL`                  |
| 添加远程仓库  | `git remote add origin URL`      |
| 修改远程地址  | `git remote set-url origin URL`  |
| 获取远程信息  | `git fetch origin`               |
| 查看提交详情  | `git show 提交ID`                  |
| 查看所有分支  | `git branch -a`                  |
| 删除已合并分支 | `git branch -d 分支名`              |
| 撤销提交的修改 | `git revert 提交ID`                |
| 查看忽略规则  | `git check-ignore -v 文件名`        |
| 创建标签    | `git tag -a v1.0.0 -m "Release"` |
| 查看操作记录  | `git reflog`                     |

## 进阶使用

| 目的       | 命令                         |
| -------- | -------------------------- |
| 变基       | `git rebase 分支名`           |
| 挑选提交     | `git cherry-pick 提交ID`     |
| 暂存工作区    | `git stash`                |
| 二分排查问题   | `git bisect start`         |
| 查看行级提交记录 | `git blame 文件名`            |
| 预览清理文件   | `git clean -n`             |
| 查看跟踪文件   | `git ls-files`             |
| 添加子模块    | `git submodule add URL 路径` |

## 重要命令：使用前先确认

| 命令                  | 风险             |
| ------------------- | -------------- |
| `git reset --hard`  | 可能丢弃未提交的修改     |
| `git clean -fd`     | 可能删除未跟踪的文件     |
| `git branch -D 分支名` | 可能删除未合并的分支     |
| `git push --force`  | 可能覆盖远程分支历史     |
| `rm -rf .git`       | 删除本地仓库元数据及提交历史 |

---

# 最后的建议

如果你刚开始学习 Git，不需要一次记住全部命令。

先熟悉这一组：

```bash
git status
git add .
git commit -m "Describe changes"
git push
git pull
git branch
git switch -c note
git switch main
git merge note
```

等你能熟练完成提交、推送、分支切换和合并，再逐步学习 `rebase`、`stash`、`cherry-pick` 等进阶功能。

**记住 Git 最重要的三个概念：**

* 仓库：管理整个项目的版本历史。
* 提交：保存一次代码版本。
* 分支：让不同功能可以独立开发。

掌握这三个概念，再结合每天实际写代码来练习，Git 就会越来越顺手。
