# 01 - Git 安装与配置

本教程将详细介绍如何在不同操作系统上安装和配置 Git。

## 📋 目录

- [什么是 Git](#什么是-git)
- [安装 Git](#安装-git)
- [Git 基础配置](#git-基础配置)

---

## 什么是 Git

Git 是一个分布式版本控制系统,由 Linus Torvalds 在 2005 年创建,最初用于 Linux 内核开发。它可以帮助你：

- **记录代码的每一次修改** - 追踪文件的完整历史
- **轻松回退到之前的版本** - 随时恢复到任何历史状态
- **与团队成员协作开发** - 多人同时开发不同功能
- **创建分支进行实验** - 在不影响主代码的情况下尝试新想法
- **解决代码冲突** - 当多人修改同一文件时，Git 可以帮助合并

### Git 与其他版本控制系统的区别

- **分布式** - 每个开发者都有完整的代码仓库副本
- **快速** - 大部分操作在本地完成，速度很快
- **强大的分支管理** - 创建和合并分支非常高效

---

## 安装 Git

### Windows 系统

#### 方法一：使用官方安装包（推荐）

**1. 下载安装包**
   - 访问 Git 官网：https://git-scm.com/download/win
   - 下载最新版本的安装包（自动检测 32 位或 64 位）

**2. 安装步骤**
   - 双击下载的 `.exe` 文件
   - 一路点击 "Next"，使用默认设置即可
   
**3. 重要选项说明：**
   - **Select Components**：保持默认勾选
   - **Choosing the default editor**：建议选择 Vim 或你熟悉的编辑器
   - **Adjusting your PATH environment**：选择 "Git from the command line and also from 3rd-party software"（推荐）
   - **Choosing HTTPS transport backend**：选择 "Use the OpenSSL library"
   - **Configuring the line ending conversions**：选择 "Checkout Windows-style, commit Unix-style line endings"

**4. 验证安装**
   
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

---

### Ubuntu/Linux 系统

#### Ubuntu/Debian 系统

**1. 更新包管理器**
```bash
sudo apt update
```

**2. 安装 Git**
```bash
sudo apt install git -y
```

**3. 验证安装**
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

---

### macOS 系统

#### 方法一：使用 Xcode Command Line Tools
```bash
xcode-select --install
```

#### 方法二：使用 Homebrew（推荐）
```bash
brew install git
```

---

## Git 基础配置

安装完成后，需要配置用户信息。这些信息会出现在你的每次提交记录中。

### 必需配置

```bash
# 配置用户名
git config --global user.name "你的姓名"

# 配置邮箱
git config --global user.email "your.email@example.com"

# 查看配置
git config --list
```

**说明：**
- `--global` 表示全局配置，对所有仓库生效
- 不使用 `--global` 则只对当前仓库生效
- 姓名和邮箱可以是任意值，但建议使用真实信息或 GitHub 账号信息

---

### 可选配置

#### 1. 设置默认分支名

```bash
# 设置默认分支名为 main（推荐，GitHub 默认）
git config --global init.defaultBranch main
```

**说明：**
- 旧版本 Git 默认分支名为 `master`
- 现在推荐使用 `main` 作为默认分支名
- GitHub、GitLab 等平台也改用 `main` 作为默认

---

#### 2. 启用颜色显示

```bash
# 启用彩色输出，更易读
git config --global color.ui auto
```

---

#### 3. 设置默认编辑器

```bash
# 设置 Vim 为默认编辑器
git config --global core.editor "vim"

# 或设置 VS Code
git config --global core.editor "code --wait"

# 或设置 Notepad++（Windows）
git config --global core.editor "'C:/Program Files/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"
```

**说明：**
- 编辑器用于编写提交信息、解决冲突等
- 如果不设置，Git 会使用系统默认编辑器

---

#### 4. 配置换行符（重要！）

**Windows 用户：**
```bash
# 检出时转换为 CRLF，提交时转换为 LF
git config --global core.autocrlf true
```

**macOS/Linux 用户：**
```bash
# 检出时不转换，提交时转换为 LF
git config --global core.autocrlf input
```

**说明：**
- Windows 使用 CRLF（`\r\n`）作为换行符
- Unix/Linux/macOS 使用 LF（`\n`）作为换行符
- 这个设置可以避免跨平台协作时的换行符问题

---

#### 5. 设置默认推送行为

```bash
# 只推送当前分支到远程同名分支
git config --global push.default simple
```

---

#### 6. 启用凭证缓存（避免重复输入密码）

**Windows：**
```bash
# 永久存储凭证
git config --global credential.helper wincred
```

**macOS：**
```bash
# 使用 Keychain 存储凭证
git config --global credential.helper osxkeychain
```

**Linux：**
```bash
# 缓存凭证 1 小时
git config --global credential.helper cache

# 或永久存储（使用纯文本，不太安全）
git config --global credential.helper store
```

---

### 查看和修改配置

#### 查看所有配置

```bash
# 查看所有配置
git config --list

# 查看全局配置
git config --global --list

# 查看当前仓库配置
git config --local --list
```

---

#### 查看特定配置

```bash
# 查看用户名
git config user.name

# 查看邮箱
git config user.email

# 查看默认编辑器
git config core.editor
```

---

#### 修改配置

```bash
# 修改用户名
git config --global user.name "新的用户名"

# 修改邮箱
git config --global user.email "new.email@example.com"
```

---

#### 删除配置

```bash
# 删除特定配置项
git config --global --unset user.name

# 删除某个配置段（如代理）
git config --global --unset http.proxy
git config --global --unset https.proxy
```

---

### 配置文件位置

Git 配置存储在以下位置：

**全局配置（`--global`）：**
- **Windows**：`C:\Users\用户名\.gitconfig`
- **macOS/Linux**：`~/.gitconfig`

**系统配置（`--system`）：**
- **Windows**：`C:\Program Files\Git\etc\gitconfig`
- **macOS/Linux**：`/etc/gitconfig`

**仓库配置（`--local`）：**
- 项目目录下的 `.git/config`

**优先级**：local > global > system

---

### 常用配置示例

**完整的初始配置：**

```bash
# 用户信息
git config --global user.name "张三"
git config --global user.email "zhangsan@example.com"

# 默认分支
git config --global init.defaultBranch main

# 颜色显示
git config --global color.ui auto

# 编辑器
git config --global core.editor "vim"

# 换行符（Windows）
git config --global core.autocrlf true

# 凭证缓存（Windows）
git config --global credential.helper wincred

# 推送设置
git config --global push.default simple

# 启用长路径支持（Windows，避免路径过长问题）
git config --global core.longpaths true
```

---

### 别名配置（提高效率）

可以为常用命令设置别名：

```bash
# 常用命令别名
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.unstage 'reset HEAD --'
git config --global alias.last 'log -1 HEAD'

# 美化的 log
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"

# 使用别名
git st         # 等同于 git status
git co main    # 等同于 git checkout main
git lg         # 美化的 log 输出
```

---

## 下一步

安装和配置完成后，继续学习：

👉 **[02 - Git 基本概念与操作](./02-Git基本概念与操作.md)** - 了解 Git 的核心概念和基本操作

---

## 💡 小提示

1. **首次使用前必须配置**：用户名和邮箱是必需的
2. **配置文件可直接编辑**：也可以直接编辑 `.gitconfig` 文件
3. **不同项目可用不同配置**：在项目目录下使用 `git config --local` 设置
4. **安全建议**：不要在公共电脑上使用 `credential.helper store`（明文存储密码）

---

**相关教程：**
- 👈 [返回总览](./README.md)
- 👉 [02 - Git 基本概念与操作](./02-Git基本概念与操作.md)

