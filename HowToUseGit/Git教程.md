# Git 使用教程

本教程将带你从零开始学习 Git 版本控制系统的使用。

## 📋 目录

- [什么是 Git](#什么是-git)
- [安装 Git](#安装-git)
  - [Windows 系统](#windows-系统)
  - [Ubuntu/Linux 系统](#ubuntulinux-系统)
  - [macOS 系统](#macos-系统)
- [Git 基础配置](#git-基础配置)
- [Git 基本概念](#git-基本概念)
- [Git 基本操作](#git-基本操作)
- [常用命令速查](#常用命令速查)
- [常见问题](#常见问题)
- [进阶技巧](#进阶技巧)

## 什么是 Git

Git 是一个分布式版本控制系统，由 Linus Torvalds 在 2005 年创建，最初用于 Linux 内核开发。它可以帮助你：

- **记录代码的每一次修改** - 追踪文件的完整历史
- **轻松回退到之前的版本** - 随时恢复到任何历史状态
- **与团队成员协作开发** - 多人同时开发不同功能
- **创建分支进行实验** - 在不影响主代码的情况下尝试新想法
- **解决代码冲突** - 当多人修改同一文件时，Git 可以帮助合并

### Git 与其他版本控制系统的区别

- **分布式** - 每个开发者都有完整的代码仓库副本
- **快速** - 大部分操作在本地完成，速度很快
- **强大的分支管理** - 创建和合并分支非常高效

## 安装 Git

### Windows 系统

#### 方法一：使用官方安装包（推荐）

1. **下载安装包**
   - 访问 Git 官网：https://git-scm.com/download/win
   - 下载最新版本的安装包（自动检测 32 位或 64 位）

2. **安装步骤**
   - 双击下载的 `.exe` 文件
   - 一路点击 "Next"，使用默认设置即可
   - 重要选项说明：
     - **Select Components**：保持默认勾选
     - **Choosing the default editor**：建议选择 Vim 或你熟悉的编辑器
     - **Adjusting your PATH environment**：选择 "Git from the command line and also from 3rd-party software"（推荐）
     - **Choosing HTTPS transport backend**：选择 "Use the OpenSSL library"
     - **Configuring the line ending conversions**：选择 "Checkout Windows-style, commit Unix-style line endings"

3. **验证安装**
   打开命令提示符（CMD）或 PowerShell，输入：
   ```bash
   git --version
   ```
   如果显示版本号（如 `git version 2.42.0.windows.1`），说明安装成功。

#### 方法二：使用包管理器

使用 Chocolatey（需要先安装 Chocolatey）：
```bash
choco install git
```

### Ubuntu/Linux 系统

#### Ubuntu/Debian 系统

1. **更新包管理器**
   ```bash
   sudo apt update
   ```

2. **安装 Git**
   ```bash
   sudo apt install git -y
   ```

3. **验证安装**
   ```bash
   git --version
   ```

#### CentOS/RHEL/Fedora 系统

**CentOS/RHEL：**

```bash
sudo yum install git -y
```

**Fedora：**

```bash
sudo dnf install git -y
```

#### 从源码安装（适用于所有 Linux 发行版）

```bash
# 安装依赖
sudo apt-get install libcurl4-gnutls-dev libexpat1-dev gettext libz-dev libssl-dev

# 下载源码
cd /tmp
wget https://github.com/git/git/archive/v2.42.0.tar.gz

# 解压并编译
tar -zxf v2.42.0.tar.gz
cd git-2.42.0
make prefix=/usr/local all
sudo make prefix=/usr/local install
```

### macOS 系统

#### 方法一：使用 Xcode Command Line Tools
```bash
xcode-select --install
```

#### 方法二：使用 Homebrew（推荐）
```bash
brew install git
```

## Git 基础配置

安装完成后，需要配置用户信息：

```bash
# 配置用户名
git config --global user.name "你的姓名"

# 配置邮箱
git config --global user.email "your.email@example.com"

# 查看配置
git config --list
```

**可选配置：**

```bash
# 设置默认分支名为 main（推荐）
git config --global init.defaultBranch main

# 启用颜色显示
git config --global color.ui auto

# 设置默认编辑器（可选）
git config --global core.editor "vim"

# 配置换行符（Windows 用户推荐）
git config --global core.autocrlf true
```

**查看特定配置：**

```bash
# 查看用户名
git config user.name

# 查看邮箱
git config user.email
```

## Git 基本概念

### 工作区域

Git 有三个主要工作区域：

1. **工作目录（Working Directory）**
   - 你实际编辑文件的地方
   - 包含项目的实际文件

2. **暂存区（Staging Area / Index）**
   - 临时存储准备提交的修改
   - 介于工作目录和本地仓库之间

3. **本地仓库（Local Repository）**
   - 存储项目的完整历史记录
   - `.git` 目录中的内容

### 文件状态

- **未跟踪（Untracked）**：新建的文件，Git 还未管理
- **已修改（Modified）**：文件被修改但未暂存
- **已暂存（Staged）**：文件已添加到暂存区
- **已提交（Committed）**：文件已保存到本地仓库

### 分支（Branch）

分支允许你在不影响主代码的情况下开发新功能。

- **main/master** - 主分支，通常是稳定的生产代码
- **feature 分支** - 用于开发新功能
- **bugfix 分支** - 用于修复 bug
- **hotfix 分支** - 用于紧急修复

## Git 基本操作

### 1. 初始化仓库

```bash
# 在现有目录中初始化 Git 仓库
git init

# 这会创建一个 .git 目录，包含所有必要的仓库文件
```

### 2. 克隆项目（Clone）

从远程仓库获取现有项目：

```bash
# 使用 HTTPS
git clone https://github.com/用户名/仓库名.git

# 使用 SSH（需要先配置 SSH 密钥）
git clone git@github.com:用户名/仓库名.git

# 克隆到指定目录
git clone https://github.com/用户名/仓库名.git 目录名

# 进入项目目录
cd 仓库名
```

### 3. 查看状态

```bash
# 查看当前状态
git status

# 查看具体修改内容
git diff

# 查看暂存区的修改
git diff --staged

# 查看工作区和最新提交的差异
git diff HEAD
```

### 4. 添加到暂存区

```bash
# 添加单个文件
git add 文件名

# 添加多个文件
git add 文件1 文件2 文件3

# 添加所有修改的文件
git add .

# 添加所有 .js 文件
git add *.js

# 交互式添加（可以选择部分修改）
git add -p
```

### 5. 提交修改（Commit）

```bash
# 提交暂存区的修改
git commit -m "提交信息：描述你做了什么"

# 添加并提交（跳过 git add）
git commit -am "提交信息"

# 修改上一次提交
git commit --amend

# 修改上一次提交信息
git commit --amend -m "新的提交信息"
```

**提交信息编写规范：**

```
类型: 简短描述（不超过 50 字符）

详细描述（可选，换行后描述）

常用类型：
- feat: 新功能
- fix: 修复 bug
- docs: 文档更新
- style: 代码格式调整（不影响代码逻辑）
- refactor: 代码重构
- test: 测试相关
- chore: 构建/工具变动
```

**示例：**
```bash
git commit -m "feat: 添加用户登录功能"
git commit -m "fix: 修复首页加载缓慢的问题"
git commit -m "docs: 更新 README 安装说明"
```

### 6. 创建和管理分支（Branch）

```bash
# 查看当前分支
git branch

# 查看所有分支（包括远程分支）
git branch -a

# 创建新分支
git branch feature-分支名

# 切换到新分支
git checkout feature-分支名

# 创建并切换到新分支（推荐）
git checkout -b feature-分支名

# 或使用新命令（Git 2.23+）
git switch -c feature-分支名

# 切换回主分支
git checkout main
```

**分支命名建议：**
- `feature/功能名` - 新功能开发
- `bugfix/问题描述` - 修复 bug
- `hotfix/紧急修复` - 紧急修复
- `docs/文档更新` - 文档更新

### 7. 合并分支（Merge）

```bash
# 切换到目标分支（通常是 main）
git checkout main

# 合并其他分支到当前分支
git merge feature-分支名

# 如果有冲突，解决后：
git add .
git commit -m "解决合并冲突"
```

### 8. 删除分支

```bash
# 删除本地分支（已合并）
git branch -d 分支名

# 强制删除本地分支（未合并）
git branch -D 分支名

# 删除远程分支
git push origin --delete 分支名
```

### 9. 查看提交历史

```bash
# 查看提交历史
git log

# 简洁查看历史
git log --oneline

# 图形化查看分支
git log --graph --oneline --all

# 查看最近 5 次提交
git log -5

# 查看某个文件的修改历史
git log -- 文件名

# 查看某次提交的详情
git show <commit-id>
```

### 10. 撤销操作

```bash
# 撤销工作区的修改（未暂存）
git checkout -- 文件名
# 或使用新命令
git restore 文件名

# 取消暂存（文件回到工作区）
git reset HEAD 文件名
# 或使用新命令
git restore --staged 文件名

# 撤销最后一次提交（保留修改）
git reset --soft HEAD~1

# 撤销最后一次提交（丢弃修改）
git reset --hard HEAD~1

# 反向提交（推荐，不改变历史）
git revert <commit-id>
```

### 11. 远程仓库操作

```bash
# 查看远程仓库
git remote -v

# 添加远程仓库
git remote add origin <url>

# 删除远程仓库
git remote remove origin

# 重命名远程仓库
git remote rename origin new-origin

# 获取远程更新（不合并）
git fetch

# 获取远程更新并合并
git pull

# 推送到远程仓库
git push

# 第一次推送新分支
git push -u origin 分支名

# 推送所有分支
git push --all

# 强制推送（谨慎使用）
git push -f origin 分支名
```

### 12. 保存工作进度（Stash）

当需要临时切换分支但不想提交当前修改：

```bash
# 保存工作进度
git stash

# 保存工作进度并添加说明
git stash save "工作进度说明"

# 查看保存的工作进度
git stash list

# 恢复最近的工作进度（保留 stash）
git stash apply

# 恢复最近的工作进度（删除 stash）
git stash pop

# 恢复指定的工作进度
git stash apply stash@{0}

# 删除工作进度
git stash drop stash@{0}

# 清空所有工作进度
git stash clear
```

### 13. 标签（Tag）

标签用于标记重要的提交点，如版本发布：

```bash
# 查看标签
git tag

# 创建轻量标签
git tag v1.0.0

# 创建带注释的标签（推荐）
git tag -a v1.0.0 -m "版本 1.0.0 发布"

# 为历史提交创建标签
git tag -a v0.9.0 <commit-id>

# 查看标签信息
git show v1.0.0

# 推送标签到远程
git push origin v1.0.0

# 推送所有标签
git push origin --tags

# 删除本地标签
git tag -d v1.0.0

# 删除远程标签
git push origin --delete v1.0.0
```

### 14. 使用 .gitignore

创建 `.gitignore` 文件，忽略不需要版本控制的文件：

```
# Node.js
node_modules/
npm-debug.log
package-lock.json

# Python
__pycache__/
*.pyc
*.pyo
.env
venv/

# Java
*.class
*.jar
target/

# IDE
.vscode/
.idea/
*.swp
*.swo

# 操作系统
.DS_Store
Thumbs.db

# 编译文件
*.o
*.exe
*.dll

# 日志文件
*.log

# 临时文件
*.tmp
*.bak
```

## 常用命令速查

### 基础命令

```bash
git init                    # 初始化仓库
git clone <url>             # 克隆仓库
git status                  # 查看状态
git add <file>              # 添加文件到暂存区
git add .                   # 添加所有修改
git commit -m "msg"         # 提交修改
git push                    # 推送到远程
git pull                    # 拉取远程更新
```

### 分支操作

```bash
git branch                  # 查看分支
git branch <name>           # 创建分支
git checkout <name>         # 切换分支
git checkout -b <name>      # 创建并切换分支
git switch <name>           # 切换分支（新命令）（不一定能用，看Git版本）
git switch -c <name>        # 创建并切换分支（新命令）
git merge <name>            # 合并分支
git branch -d <name>        # 删除分支
git branch -D <name>        # 强制删除分支
```

### 查看历史

```bash
git log                     # 查看提交历史
git log --oneline           # 简洁查看历史
git log --graph             # 图形化查看分支
git log -n 5                # 查看最近 5 次提交
git show <commit>           # 查看某次提交详情
git diff                    # 查看工作区修改
git diff --staged           # 查看暂存区修改
git diff HEAD               # 查看工作区与最新提交的差异
```

### 撤销操作

```bash
git checkout -- <file>      # 撤销工作区修改
git restore <file>          # 撤销工作区修改（新命令）
git reset HEAD <file>       # 取消暂存
git restore --staged <file> # 取消暂存（新命令）
git reset --soft HEAD~1     # 撤销提交（保留修改）
git reset --hard HEAD~1     # 撤销提交（丢弃修改）
git revert <commit>         # 反向提交
```

### 远程仓库

```bash
git remote -v               # 查看远程仓库
git remote add <name> <url> # 添加远程仓库
git remote remove <name>    # 删除远程仓库
git fetch                   # 获取远程更新
git pull                    # 拉取并合并
git push                    # 推送到远程
git push -u origin <branch> # 推送并设置上游分支
```

### 暂存工作进度

```bash
git stash                   # 保存工作进度
git stash list              # 查看保存的工作进度
git stash pop               # 恢复并删除工作进度
git stash apply             # 恢复工作进度（保留）
git stash drop              # 删除工作进度
git stash clear             # 清空所有工作进度
```

### 标签

```bash
git tag                     # 查看标签
git tag v1.0.0              # 创建标签
git tag -a v1.0.0 -m "msg"  # 创建带注释的标签
git push origin v1.0.0      # 推送标签
git push origin --tags      # 推送所有标签
git tag -d v1.0.0           # 删除本地标签
```

## 常见问题

### 1. 忘记创建分支，在 main 分支上修改了代码

```bash
# 创建新分支（会带上你的修改）
git checkout -b feature-新分支

# 提交修改
git add .
git commit -m "提交信息"
git push -u origin feature-新分支
```

### 2. 提交信息写错了

```bash
# 修改最后一次提交信息
git commit --amend -m "新的提交信息"

# 如果已经推送，需要强制推送（谨慎使用）
git push -f origin 分支名
```

### 3. 误提交了敏感信息

```bash
# 撤销最后一次提交
git reset --soft HEAD~1

# 删除敏感文件或修改
rm 敏感文件

# 重新提交
git add .
git commit -m "移除敏感信息"
```

### 4. 合并冲突怎么解决

当执行 `git merge` 或 `git pull` 时出现冲突：

1. **查看冲突文件**
   ```bash
   git status
   ```

2. **打开冲突文件，会看到冲突标记**
   ```
   <<<<<<< HEAD
   你的修改
   =======
   别人的修改
   >>>>>>> branch-name
   ```

3. **手动编辑**，保留需要的内容，删除冲突标记

4. **标记为已解决**
   ```bash
   git add 文件名
   ```

5. **完成合并**
   ```bash
   git commit -m "解决合并冲突"
   ```

### 5. 如何回退到某个版本

```bash
# 查看提交历史
git log --oneline

# 回退到指定版本（保留修改）
git reset --soft <commit-id>

# 回退到指定版本（丢弃修改）
git reset --hard <commit-id>

# 创建一个反向提交（推荐，不改变历史）
git revert <commit-id>
```

### 6. 查看某个文件的历史版本

```bash
# 查看文件的修改历史
git log -- 文件名

# 查看文件在某次提交时的内容
git show <commit-id>:文件名

# 恢复文件到某个版本
git checkout <commit-id> -- 文件名
```

### 7. 误删除了文件

```bash
# 如果只是删除了工作区的文件（未提交）
git checkout -- 文件名

# 如果已经提交了删除操作
git revert <commit-id>
```

### 8. 如何比较两个分支的差异

```bash
# 查看两个分支的差异
git diff branch1..branch2

# 查看某个文件在两个分支的差异
git diff branch1..branch2 -- 文件名

# 查看分支包含哪些提交
git log branch1..branch2
```

### 9. 大文件处理

Git 不适合存储大文件，如需管理大文件，使用 Git LFS：

```bash
# 安装 Git LFS
git lfs install

# 跟踪大文件类型
git lfs track "*.psd"
git lfs track "*.mp4"

# 提交 .gitattributes
git add .gitattributes
git commit -m "添加 Git LFS 支持"

# 之后正常使用 git add、commit、push
```

### 10. 配置代理

*@Tiger：我没搞过，我一般直接开电脑的代理*

如果需要通过代理访问远程仓库：

```bash
# HTTP 代理
git config --global http.proxy http://127.0.0.1:7890
git config --global https.proxy http://127.0.0.1:7890

# SOCKS5 代理
git config --global http.proxy socks5://127.0.0.1:7890
git config --global https.proxy socks5://127.0.0.1:7890

# 取消代理
git config --global --unset http.proxy
git config --global --unset https.proxy

# 只对 GitHub 使用代理
git config --global http.https://github.com.proxy http://127.0.0.1:7890
```

## 进阶技巧

### 1. 使用 Git Alias 提高效率

*@Tiger：就是命令简化*

```bash
# 设置别名
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.unstage 'reset HEAD --'
git config --global alias.last 'log -1 HEAD'
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"

# 使用别名
git st         # 等同于 git status
git co main    # 等同于 git checkout main
git lg         # 美化的 log 输出
```

### 2. 交互式 Rebase

整理提交历史：

```bash
# 整理最近 3 次提交
git rebase -i HEAD~3

# 在编辑器中可以选择：
# pick: 保留提交
# reword: 修改提交信息
# squash: 合并到前一个提交
# fixup: 合并到前一个提交（不保留提交信息）
# drop: 删除提交
```

**示例场景：**
```bash
# 假设你有 3 次提交：
# commit3: 修复拼写错误
# commit2: 添加功能
# commit1: 初始提交

# 使用 rebase 将 commit3 合并到 commit2
git rebase -i HEAD~2

# 在编辑器中：
pick commit2
squash commit3

# 保存后会提示编辑合并后的提交信息
```

### 3. Cherry-pick

将某个提交应用到当前分支：

```bash
# 将指定提交应用到当前分支
git cherry-pick <commit-id>

# 应用多个提交
git cherry-pick <commit-id1> <commit-id2>

# 应用一个范围的提交
git cherry-pick <commit-id1>..<commit-id2>
```

**使用场景：**
- 将某个 bug 修复从一个分支复制到另一个分支
- 只需要某个分支的部分提交

### 4. Rebase vs Merge

**Merge（合并）：**
- 保留完整的提交历史
- 会创建一个新的合并提交
- 历史记录是真实的

```bash
git checkout main
git merge feature-branch
```

**Rebase（变基）：**
- 使提交历史呈线性
- 不会创建额外的合并提交
- 使历史记录更清晰

```bash
git checkout feature-branch
git rebase main
```

**何时使用：**
- **Merge**：合并公共分支（如将 feature 合并到 main）
- **Rebase**：更新自己的功能分支（将 main 的更新应用到自己的分支）

### 5. 查找引入 Bug 的提交（git bisect）

使用二分查找定位引入 bug 的提交：

```bash
# 开始二分查找
git bisect start

# 标记当前版本为坏的
git bisect bad

# 标记一个已知的好版本
git bisect good <commit-id>

# Git 会自动切换到中间版本
# 测试后标记：
git bisect good  # 如果这个版本是好的
git bisect bad   # 如果这个版本是坏的

# 重复直到找到第一个坏的提交

# 结束二分查找
git bisect reset
```

### 6. 子模块（Submodules）

在项目中包含其他 Git 仓库：

```bash
# 添加子模块
git submodule add <仓库URL> <路径>

# 克隆包含子模块的项目
git clone <项目URL>
git submodule init
git submodule update

# 或一步到位
git clone --recursive <项目URL>

# 更新子模块
git submodule update --remote

# 删除子模块
git submodule deinit <路径>
git rm <路径>
```

### 7. 工作树（Worktree）

同时在不同分支上工作：

```bash
# 创建新的工作树
git worktree add <路径> <分支名>

# 查看所有工作树
git worktree list

# 删除工作树
git worktree remove <路径>
```

### 8. 查看每行代码的修改记录（git blame）

```bash
# 查看文件每行的最后修改信息
git blame 文件名

# 查看指定行范围
git blame -L 10,20 文件名

# 查看某次提交之前的 blame
git blame <commit-id> -- 文件名
```

### 9. 搜索提交历史

```bash
# 在提交信息中搜索
git log --grep="关键词"

# 在代码中搜索
git log -S "代码片段"

# 查找添加或删除某个函数的提交
git log -G "函数名"

# 查找某个作者的提交
git log --author="作者名"
```

## 学习资源

- 📖 [Pro Git 中文版](https://git-scm.com/book/zh/v2)（官方免费电子书，强烈推荐）
- 🎮 [Learn Git Branching](https://learngitbranching.js.org/?locale=zh_CN)（交互式学习，非常直观）
- 📺 [Git 教学视频](https://www.bilibili.com/video/BV1pX4y1S7Dq/)（Bilibili）
- 📝 [Git 官方文档](https://git-scm.com/doc)
- 🔧 [Git 速查表](https://training.github.com/downloads/zh_CN/github-git-cheat-sheet/)

## 总结

Git 的学习曲线可能比较陡峭，但掌握基本命令后会极大提高工作效率。

**日常使用的核心命令：**
1. `git clone` - 克隆项目
2. `git status` - 查看状态
3. `git add .` - 添加修改
4. `git commit -m` - 提交修改
5. `git push` - 推送到远程
6. `git pull` - 拉取更新
7. `git branch` / `git checkout` - 分支操作

**建议的学习路径：**
1. **第一阶段**：熟悉基本命令（clone, status, add, commit, push, pull）
2. **第二阶段**：掌握分支操作（branch, checkout, merge）
3. **第三阶段**：学习撤销和回退（reset, revert, checkout）
4. **第四阶段**：了解工作进度保存（stash）
5. **第五阶段**：逐步了解进阶功能（rebase, cherry-pick, bisect）

**最佳实践：**
- 经常提交，保持提交粒度适中
- 写清晰的提交信息
- 在开发新功能前创建新分支
- 定期同步远程仓库的更新
- 使用 `.gitignore` 忽略不必要的文件
- 不要提交大文件和敏感信息

---

💡 **提示**：Git 是一个强大的工具，刚开始可能会感觉复杂，但随着使用会越来越熟练。建议从基础命令开始，逐步深入学习。

Happy Coding! 🚀

