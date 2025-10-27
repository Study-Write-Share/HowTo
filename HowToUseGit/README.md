# Git 和 GitHub 使用教程

本教程将带你从零开始学习 Git 版本控制系统和 GitHub 平台的使用。

## 📋 目录

- [什么是 Git 和 GitHub](#什么是-git-和-github)
- [安装 Git](#安装-git)
  - [Windows 系统](#windows-系统)
  - [Ubuntu/Linux 系统](#ubuntulinux-系统)
  - [macOS 系统](#macos-系统)
- [Git 基础配置](#git-基础配置)
- [Git 基本概念](#git-基本概念)
- [基本操作流程](#基本操作流程)
- [GitHub 使用指南](#github-使用指南)
- [常用命令速查](#常用命令速查)
- [常见问题](#常见问题)

## 什么是 Git 和 GitHub

### Git
Git 是一个分布式版本控制系统，用于跟踪代码的变化历史。它可以帮助你：
- 记录代码的每一次修改
- 轻松回退到之前的版本
- 与团队成员协作开发

### GitHub
GitHub 是一个基于 Git 的代码托管平台，提供：
- 远程代码仓库存储
- 团队协作工具
- 代码审查功能（Pull Request）
- 项目管理工具

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

# 配置邮箱（建议使用 GitHub 注册邮箱）
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

## Git 基本概念

### 工作区域

Git 有三个主要工作区域：

1. **工作目录（Working Directory）**
   - 你实际编辑文件的地方

2. **暂存区（Staging Area）**
   - 临时存储准备提交的修改

3. **本地仓库（Local Repository）**
   - 存储项目的完整历史记录

### 文件状态

- **未跟踪（Untracked）**：新建的文件，Git 还未管理
- **已修改（Modified）**：文件被修改但未暂存
- **已暂存（Staged）**：文件已添加到暂存区
- **已提交（Committed）**：文件已保存到本地仓库

### 分支（Branch）

分支允许你在不影响主代码的情况下开发新功能。

## 基本操作流程

### 1. 克隆项目（Clone）

从 GitHub 获取现有项目：

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

### 2. 创建分支（Branch）

在开始新功能开发前，创建一个新分支：

```bash
# 查看当前分支
git branch

# 创建新分支
git branch feature-分支名

# 切换到新分支
git checkout feature-分支名

# 创建并切换到新分支（推荐）
git checkout -b feature-分支名

# 或使用新命令（Git 2.23+）
git switch -c feature-分支名
```

**分支命名建议：**
- `feature/功能名` - 新功能开发
- `bugfix/问题描述` - 修复 bug
- `hotfix/紧急修复` - 紧急修复
- `docs/文档更新` - 文档更新

### 3. 修改代码

使用你喜欢的编辑器修改代码文件。

### 4. 查看状态

```bash
# 查看当前状态
git status

# 查看具体修改内容
git diff

# 查看暂存区的修改
git diff --staged
```

### 5. 添加到暂存区

```bash
# 添加单个文件
git add 文件名

# 添加多个文件
git add 文件1 文件2 文件3

# 添加所有修改的文件
git add .

# 添加所有 .js 文件
git add *.js

# 交互式添加
git add -p
```

### 6. 提交修改（Commit）

```bash
# 提交暂存区的修改
git commit -m "提交信息：描述你做了什么"

# 添加并提交（跳过 git add）
git commit -am "提交信息"

# 修改上一次提交
git commit --amend
```

**提交信息编写规范：**

```
类型: 简短描述（不超过 50 字符）

详细描述（可选，换行后描述）

常用类型：
- feat: 新功能
- fix: 修复 bug
- docs: 文档更新
- style: 代码格式调整
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

### 7. 推送到远程仓库（Push）

```bash
# 第一次推送新分支
git push -u origin 分支名

# 后续推送
git push

# 推送所有分支
git push --all

# 强制推送（谨慎使用）
git push -f origin 分支名
```

### 8. 拉取远程更新（Pull）

```bash
# 拉取并合并远程分支
git pull

# 拉取指定分支
git pull origin main

# 使用 rebase 方式拉取（保持提交历史整洁）
git pull --rebase
```

### 9. 合并分支（Merge）

```bash
# 切换到主分支
git checkout main

# 合并其他分支
git merge feature-分支名

# 如果有冲突，解决后：
git add .
git commit -m "解决合并冲突"
```

## GitHub 使用指南

### 创建 GitHub 账号

1. 访问 https://github.com
2. 点击 "Sign up" 注册账号
3. 验证邮箱

### 配置 SSH 密钥（推荐）

使用 SSH 可以避免每次都输入密码。

#### 1. 生成 SSH 密钥

```bash
# 生成密钥（使用你的 GitHub 邮箱）
ssh-keygen -t ed25519 -C "your.email@example.com"

# 如果系统不支持 ed25519，使用 RSA
ssh-keygen -t rsa -b 4096 -C "your.email@example.com"

# 一路回车，使用默认设置
```

#### 2. 添加到 SSH agent

**Windows (PowerShell)：**
```bash
# 启动 ssh-agent
Get-Service -Name ssh-agent | Set-Service -StartupType Manual
Start-Service ssh-agent

# 添加密钥
ssh-add ~/.ssh/id_ed25519
```

**Linux/macOS：**
```bash
# 启动 ssh-agent
eval "$(ssh-agent -s)"

# 添加密钥
ssh-add ~/.ssh/id_ed25519
```

#### 3. 添加公钥到 GitHub

```bash
# 复制公钥内容（Windows）
type ~/.ssh/id_ed25519.pub | clip

# Linux
cat ~/.ssh/id_ed25519.pub | xclip -selection clipboard

# macOS
pbcopy < ~/.ssh/id_ed25519.pub
```

然后：
1. 登录 GitHub
2. 点击头像 -> Settings
3. 左侧选择 "SSH and GPG keys"
4. 点击 "New SSH key"
5. 粘贴公钥内容，设置标题
6. 点击 "Add SSH key"

#### 4. 测试连接

```bash
ssh -T git@github.com
```

如果看到 "Hi 用户名! You've successfully authenticated..."，说明配置成功。

### 创建新仓库

#### 在 GitHub 网站创建

1. 登录 GitHub
2. 点击右上角 "+" -> "New repository"
3. 填写仓库名称和描述
4. 选择 Public（公开）或 Private（私有）
5. 可选：添加 README、.gitignore、LICENSE
6. 点击 "Create repository"

#### 推送本地项目到 GitHub

```bash
# 在本地项目目录
git init
git add .
git commit -m "首次提交"

# 添加远程仓库
git remote add origin git@github.com:用户名/仓库名.git

# 推送到 GitHub
git branch -M main
git push -u origin main
```

### Pull Request（PR）工作流程

Pull Request 是 GitHub 的核心协作功能。

#### 标准 PR 流程

1. **Fork 项目（如果是别人的项目）**
   - 在项目页面点击 "Fork"
   - 克隆你的 Fork：`git clone git@github.com:你的用户名/项目名.git`

2. **创建新分支**
   ```bash
   git checkout -b feature-新功能
   ```

3. **进行修改并提交**
   ```bash
   git add .
   git commit -m "feat: 添加新功能"
   ```

4. **推送分支到 GitHub**
   ```bash
   git push origin feature-新功能
   ```

5. **在 GitHub 创建 Pull Request**
   - 访问你的 GitHub 仓库
   - 点击 "Compare & pull request" 按钮
   - 填写 PR 标题和描述：
     - 说明做了什么修改
     - 为什么要做这些修改
     - 如何测试
   - 选择要合并到的分支（通常是 `main` 或 `develop`）
   - 点击 "Create pull request"

6. **代码审查（Code Review）**
   - 团队成员会审查你的代码
   - 根据反馈进行修改
   - 在本地修改后：
     ```bash
     git add .
     git commit -m "fix: 根据审查意见修改"
     git push
     ```
   - PR 会自动更新

7. **合并 PR**
   - 审查通过后，项目维护者会合并你的 PR
   - 可以选择合并方式：
     - **Merge commit**：保留所有提交历史
     - **Squash and merge**：将所有提交压缩为一个（推荐）
     - **Rebase and merge**：重新应用提交

8. **同步主分支**
   ```bash
   git checkout main
   git pull origin main
   git branch -d feature-新功能  # 删除本地分支
   ```

### 项目协作最佳实践

1. **始终从最新的 main 分支创建新分支**
   ```bash
   git checkout main
   git pull origin main
   git checkout -b feature-新功能
   ```

2. **保持提交粒度适中**
   - 一个提交只做一件事
   - 避免一次提交太多修改

3. **经常同步主分支**
   ```bash
   git checkout main
   git pull origin main
   git checkout feature-新功能
   git merge main  # 或使用 git rebase main
   ```

4. **解决冲突**
   - 当出现冲突时，Git 会标记冲突文件
   - 手动编辑解决冲突
   - 标记为已解决：`git add 文件名`
   - 完成合并：`git commit`

5. **使用 .gitignore**
   创建 `.gitignore` 文件，忽略不需要版本控制的文件：
   ```
   # Node.js
   node_modules/
   npm-debug.log
   
   # Python
   __pycache__/
   *.pyc
   .env
   
   # IDE
   .vscode/
   .idea/
   *.swp
   
   # 操作系统
   .DS_Store
   Thumbs.db
   ```

## 常用命令速查

### 基础命令

```bash
git init                    # 初始化仓库
git clone <url>            # 克隆仓库
git status                 # 查看状态
git add <file>             # 添加文件到暂存区
git commit -m "msg"        # 提交修改
git push                   # 推送到远程
git pull                   # 拉取远程更新
```

### 分支操作

```bash
git branch                 # 查看分支
git branch <name>          # 创建分支
git checkout <name>        # 切换分支
git checkout -b <name>     # 创建并切换分支
git merge <name>           # 合并分支
git branch -d <name>       # 删除分支
git branch -D <name>       # 强制删除分支
```

### 查看历史

```bash
git log                    # 查看提交历史
git log --oneline          # 简洁查看历史
git log --graph            # 图形化查看分支
git show <commit>          # 查看某次提交详情
git diff                   # 查看工作区修改
git diff --staged          # 查看暂存区修改
```

### 撤销操作

```bash
git checkout -- <file>     # 撤销工作区修改
git reset HEAD <file>      # 取消暂存
git reset --soft HEAD~1    # 撤销最后一次提交（保留修改）
git reset --hard HEAD~1    # 撤销最后一次提交（丢弃修改）
git revert <commit>        # 反向提交（推荐）
```

### 远程仓库

```bash
git remote -v              # 查看远程仓库
git remote add <name> <url>  # 添加远程仓库
git remote remove <name>   # 删除远程仓库
git fetch                  # 获取远程更新（不合并）
git push -u origin <branch>  # 推送并设置上游分支
```

### 标签

```bash
git tag                    # 查看标签
git tag v1.0.0             # 创建标签
git tag -a v1.0.0 -m "msg" # 创建带注释的标签
git push origin v1.0.0     # 推送标签
git push origin --tags     # 推送所有标签
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

# 如果已经推送，需要强制推送
git push -f origin 分支名
```

### 3. 误提交了敏感信息

```bash
# 撤销最后一次提交
git reset --soft HEAD~1

# 删除敏感文件
rm 敏感文件

# 重新提交
git add .
git commit -m "移除敏感信息"
```

### 4. 如何删除远程分支

```bash
# 删除本地分支
git branch -d 分支名

# 删除远程分支
git push origin --delete 分支名
```

### 5. 合并冲突怎么解决

当执行 `git merge` 或 `git pull` 时出现冲突：

1. 查看冲突文件：`git status`
2. 打开冲突文件，会看到：
   ```
   <<<<<<< HEAD
   你的修改
   =======
   别人的修改
   >>>>>>> branch-name
   ```
3. 手动编辑，保留需要的内容，删除标记
4. 标记为已解决：`git add 文件名`
5. 完成合并：`git commit`

### 6. 如何回退到某个版本

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

### 7. clone 速度太慢

```bash
# 使用浅克隆（只克隆最近的提交）
git clone --depth 1 <url>

# 使用国内镜像
git clone https://gitclone.com/github.com/用户名/仓库名.git
```

### 8. 配置代理

如果需要通过代理访问 GitHub：

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
```

### 10. 保存工作进度

当需要临时切换分支但不想提交当前修改：

```bash
# 保存工作进度
git stash

# 切换分支处理其他事情
git checkout other-branch

# 回到原分支，恢复工作进度
git checkout original-branch
git stash pop
```

## 进阶技巧

### 使用 Git Alias 提高效率

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
git st        # 等同于 git status
git co main   # 等同于 git checkout main
git lg        # 美化的 log 输出
```

### 交互式 Rebase

整理提交历史：

```bash
# 整理最近 3 次提交
git rebase -i HEAD~3

# 在编辑器中：
# pick: 保留提交
# reword: 修改提交信息
# squash: 合并到前一个提交
# drop: 删除提交
```

### Cherry-pick

将某个提交应用到当前分支：

```bash
git cherry-pick <commit-id>
```

## 学习资源

- 📖 [Pro Git 中文版](https://git-scm.com/book/zh/v2)（官方免费电子书）
- 🎮 [Learn Git Branching](https://learngitbranching.js.org/?locale=zh_CN)（交互式学习）
- 📺 [Git 教学视频](https://www.bilibili.com/video/BV1pX4y1S7Dq/)（Bilibili）
- 📝 [GitHub 官方文档](https://docs.github.com/cn)

## 总结

Git 的学习曲线可能比较陡峭，但掌握基本命令后会极大提高工作效率：

**日常使用的核心命令：**
1. `git clone` - 克隆项目
2. `git checkout -b` - 创建分支
3. `git add .` - 添加修改
4. `git commit -m` - 提交修改
5. `git push` - 推送到远程
6. `git pull` - 拉取更新
7. `git merge` - 合并分支

**建议的学习路径：**
1. 先熟悉基本命令（clone, add, commit, push）
2. 掌握分支操作（branch, checkout, merge）
3. 学习协作流程（Pull Request）
4. 逐步了解进阶功能（rebase, stash, cherry-pick）

---

有问题欢迎提 Issue！Happy Coding! 🚀

