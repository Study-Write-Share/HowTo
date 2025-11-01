# 02 - Git 基本概念与操作

本教程将介绍 Git 的核心概念和日常使用的基本操作。

## 📋 目录

- [Git 基本概念](#git-基本概念)
- [Git 基本操作](#git-基本操作)
- [常用命令速查](#常用命令速查)
- [使用 .gitignore](#使用-gitignore)

---

## Git 基本概念（不是完整工作流程 )

### 工作区域

Git 有三个主要工作区域：

#### 1. 工作目录（Working Directory）
   - 你实际编辑文件的地方
   - 包含项目的实际文件
   - 可以看到和修改的文件

#### 2. 暂存区（Staging Area / Index）
   - 临时存储准备提交的修改
   - 介于工作目录和本地仓库之间
   - 使用 `git add` 将文件添加到暂存区

#### 3. 本地仓库（Local Repository）
   - 存储项目的完整历史记录
   - `.git` 目录中的内容
   - 使用 `git commit` 将暂存区内容提交到仓库

**工作流程示意图：**
```
工作目录 ---git add---> 暂存区 ---git commit---> 本地仓库 ---git push---> 远程仓库
          <--git checkout--     <--git reset--      <--git pull--
```

---

### 文件状态

Git 中的文件有四种状态：

- **未跟踪（Untracked）**：新建的文件，Git 还未管理
- **已修改（Modified）**：文件被修改但未暂存
- **已暂存（Staged）**：文件已添加到暂存区，等待提交
- **已提交（Committed）**：文件已保存到本地仓库

**状态转换：**
```
未跟踪 ---git add---> 已暂存
已修改 ---git add---> 已暂存
已暂存 ---git commit---> 已提交
已提交 ---修改文件---> 已修改
```

---

## Git 基本操作

### 1. 初始化仓库

**在现有目录中初始化 Git 仓库：**

```bash
# 进入项目目录
cd /path/to/your/project

# 初始化 Git 仓库
git init

# 这会创建一个 .git 目录，包含所有必要的仓库文件
```

**说明：**
- `.git` 目录包含 Git 的所有元数据和对象数据库
- 不要手动修改 `.git` 目录中的内容
- 删除 `.git` 目录会删除所有版本历史

---

### 2. 克隆项目（Clone）

从远程仓库获取现有项目：

```bash
# 使用 HTTPS 克隆
git clone https://github.com/用户名/仓库名.git

# 使用 SSH 克隆（需要先配置 SSH 密钥）
git clone git@github.com:用户名/仓库名.git

# 克隆到指定目录
git clone https://github.com/用户名/仓库名.git 自定义目录名

# 浅克隆（只克隆最近的提交历史，速度快）
git clone --depth 1 https://github.com/用户名/仓库名.git

# 进入项目目录
cd 仓库名
```

**HTTPS vs SSH：**
- **HTTPS**：
  - ✅ 无需配置，直接使用
  - ❌ 每次 push 需要输入密码（或使用 token）
  - ✅ 适合临时克隆

- **SSH**：
  - ✅ 配置后无需输入密码
  - ❌ 需要先配置 SSH 密钥
  - ✅ 适合长期使用

---

### 3. 查看状态

```bash
# 查看当前状态
git status

# 简洁输出
git status -s

# 查看具体修改内容
git diff

# 查看暂存区的修改
git diff --staged
# 或
git diff --cached

# 查看工作区和最新提交的差异
git diff HEAD
```

**`git status` 输出说明：**

```bash
On branch main                    # 当前分支
Your branch is up to date with 'origin/main'.  # 与远程分支同步

Changes to be committed:          # 已暂存的修改（绿色）
  (use "git restore --staged <file>..." to unstage)
        new file:   file1.txt
        modified:   file2.txt

Changes not staged for commit:   # 已修改但未暂存（红色）
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   file3.txt

Untracked files:                  # 未跟踪的文件（红色）
  (use "git add <file>..." to include in what will be committed)
        file4.txt
```

---

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

# 添加某个目录下的所有文件
git add src/

# 交互式添加（可以选择部分修改）
git add -p
# 或
git add --patch
```

**`git add -p` 交互式添加：**
- `y` - 暂存这个块
- `n` - 不暂存这个块
- `s` - 将这个块分割成更小的块
- `e` - 手动编辑这个块
- `q` - 退出
- `?` - 查看帮助

---

### 5. 提交修改（Commit）

```bash
# 提交暂存区的修改
git commit -m "提交信息：描述你做了什么"

# 添加并提交（跳过 git add）
git commit -am "提交信息"
# 注意：只对已跟踪的文件有效，新文件仍需 git add

# 打开编辑器编写详细提交信息
git commit

# 修改上一次提交
git commit --amend

# 修改上一次提交信息
git commit --amend -m "新的提交信息"

# 修改上一次提交，不改消息
git commit --amend --no-edit
```

---

#### 提交信息编写规范

使用 Conventional Commits 规范：

**格式：**
```
<type>(<scope>): <subject>

<body>

<footer>
```

**类型（type）：**
- `feat` - 新功能
- `fix` - Bug 修复
- `docs` - 文档更新
- `style` - 代码格式调整（不影响代码逻辑）
- `refactor` - 代码重构
- `perf` - 性能优化
- `test` - 测试相关
- `chore` - 构建/工具变动
- `ci` - CI 配置

**示例：**

```bash
# 简单提交
git commit -m "feat: 添加用户登录功能"
git commit -m "fix: 修复首页加载缓慢的问题"
git commit -m "docs: 更新 README 安装说明"

# 带作用域
git commit -m "feat(auth): 添加 JWT 验证"
git commit -m "fix(ui): 修复按钮样式错乱"

# 详细提交（打开编辑器）
feat: 添加用户登录功能

- 实现登录 API
- 添加 JWT token 验证
- 更新用户界面

Closes #123
```

---

### 6. 查看提交历史

```bash
# 查看提交历史
git log

# 简洁查看历史（每个提交一行）
git log --oneline

# 图形化查看分支
git log --graph --oneline --all

# 查看最近 5 次提交
git log -5
# 或
git log -n 5

# 查看某个文件的修改历史
git log -- 文件名

# 查看某个作者的提交
git log --author="作者名"

# 查看某个时间段的提交
git log --since="2024-01-01"
git log --since="2 weeks ago"

# 查看包含某个关键词的提交
git log --grep="关键词"

# 查看某次提交的详情
git show <commit-id>

# 查看某次提交的文件变更
git show <commit-id> --stat
```

**`git log` 输出示例：**
```
commit a1b2c3d4e5f6g7h8i9j0 (HEAD -> main, origin/main)
Author: 张三 <zhangsan@example.com>
Date:   Mon Oct 28 10:00:00 2024 +0800

    feat: 添加用户登录功能
    
    - 实现登录 API
    - 添加 JWT 验证
```

---

### 7. 撤销操作

#### 撤销工作区的修改（未暂存）

```bash
# 撤销单个文件的修改
git checkout -- 文件名

# 或使用新命令（Git 2.23+）
git restore 文件名

# 撤销所有修改
git checkout -- .
# 或
git restore .
```

**⚠️ 警告**：这会永久丢失工作区的修改！

---

#### 取消暂存（文件回到工作区）

```bash
# 取消单个文件的暂存
git reset HEAD 文件名

# 或使用新命令（Git 2.23+）
git restore --staged 文件名

# 取消所有文件的暂存
git reset HEAD
# 或
git restore --staged .
```

**说明**：文件修改仍在，只是从暂存区移除。

---

#### 撤销提交

```bash
# 撤销最后一次提交（保留修改在工作区）
git reset --soft HEAD~1

# 撤销最后一次提交（保留修改在暂存区）
git reset --mixed HEAD~1
# 或
git reset HEAD~1

# 撤销最后一次提交（丢弃所有修改）⚠️ 危险
git reset --hard HEAD~1

# 撤销最近 3 次提交
git reset --soft HEAD~3
```

**`HEAD~1` 说明：**
- `HEAD` - 当前提交
- `HEAD~1` - 上一次提交
- `HEAD~2` - 上上次提交
- 或使用 commit id：`git reset --soft a1b2c3d`

---

#### 反向提交（推荐，不改变历史）

```bash
# 创建一个新提交来撤销某次提交
git revert <commit-id>

# 撤销最近一次提交
git revert HEAD

# 撤销多个提交
git revert HEAD~3..HEAD
```

**`reset` vs `revert`：**
- **reset**：直接删除提交历史（危险，不推荐用于已推送的提交）
- **revert**：创建新提交来撤销（安全，推荐）

---

### 8. 远程仓库操作

#### 查看远程仓库

```bash
# 查看远程仓库
git remote

# 查看详细信息（包含 URL）
git remote -v

# 查看某个远程仓库的详细信息
git remote show origin
```

---

#### 添加远程仓库

```bash
# 添加远程仓库
git remote add <远程仓库名> <url>

# 通常使用 origin 作为远程仓库名
git remote add origin https://github.com/用户名/仓库名.git
```

---

#### 修改远程仓库

```bash
# 修改远程仓库 URL
git remote set-url origin <new-url>

# 重命名远程仓库
git remote rename origin new-origin

# 删除远程仓库
git remote remove origin
```

---

#### 获取远程更新

```bash
# 获取远程更新（不合并）
git fetch
# 或指定远程仓库
git fetch origin

# 获取远程更新并合并到当前分支
git pull
# 等同于
git fetch + git merge

# 使用 rebase 方式拉取（保持线性历史）
git pull --rebase
```

**`fetch` vs `pull`：**
- **fetch**：只下载远程更新，不合并（安全）
- **pull**：下载并自动合并（可能产生冲突）

---

#### 推送到远程仓库

```bash
# 推送到远程仓库
git push

# 第一次推送新分支（建立追踪关系）
git push -u origin 分支名
# 或
git push --set-upstream origin 分支名

# 推送所有分支
git push --all

# 推送标签
git push --tags

# 强制推送（⚠️ 谨慎使用）
git push -f origin 分支名
# 或
git push --force origin 分支名

# 更安全的强制推送（检查远程是否有新提交）
git push --force-with-lease origin 分支名
```

**⚠️ 强制推送警告**：
- 会覆盖远程仓库的历史
- 可能导致其他人的提交丢失
- 仅在确认安全的情况下使用

---

### 9. 保存工作进度（Stash）

当需要临时切换分支但不想提交当前修改时：

```bash
# 保存工作进度
git stash

# 保存工作进度并添加说明
git stash save "工作进度说明"

# 包含未跟踪的文件
git stash -u
# 或
git stash --include-untracked

# 查看保存的工作进度
git stash list

# 恢复最近的工作进度（保留 stash）
git stash apply

# 恢复最近的工作进度（删除 stash）
git stash pop

# 恢复指定的工作进度
git stash apply stash@{0}
git stash pop stash@{1}

# 查看 stash 的内容
git stash show
git stash show -p  # 显示详细差异

# 删除工作进度
git stash drop stash@{0}

# 清空所有工作进度
git stash clear
```

**使用场景：**
1. 临时切换分支处理紧急问题
2. 拉取远程更新前保存当前工作
3. 想要重新开始但不想丢失当前修改

---

### 10. 标签（Tag）

标签用于标记重要的提交点，如版本发布。

```bash
# 查看标签
git tag

# 查看标签详细信息
git show v1.0.0

# 创建轻量标签
git tag v1.0.0

# 创建带注释的标签（推荐）
git tag -a v1.0.0 -m "版本 1.0.0 发布"

# 为历史提交创建标签
git tag -a v0.9.0 <commit-id> -m "版本 0.9.0"

# 推送标签到远程
git push origin v1.0.0

# 推送所有标签
git push origin --tags

# 删除本地标签
git tag -d v1.0.0

# 删除远程标签
git push origin --delete v1.0.0
# 或
git push origin :refs/tags/v1.0.0

# 检出标签（创建新分支）
git checkout -b branch-name v1.0.0
```

**标签命名规范：**
- 语义化版本：`v主版本号.次版本号.修订号`
- 示例：`v1.0.0`, `v2.1.3`, `v1.0.0-beta`

---

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

---

## 使用 .gitignore

创建 `.gitignore` 文件，忽略不需要版本控制的文件。

### 基本语法

```gitignore
# 这是注释

# 忽略所有 .log 文件
*.log

# 忽略 node_modules 目录
node_modules/

# 忽略 dist 目录下的所有文件
dist/

# 但不忽略 dist/index.html
!dist/index.html

# 只忽略当前目录下的 TODO 文件
/TODO

# 忽略 doc/ 目录下的所有 .txt 文件
doc/**/*.txt
```

### 常用 .gitignore 模板

#### Node.js 项目

```gitignore
# Node.js
node_modules/
npm-debug.log
yarn-error.log
package-lock.json
.env

# Build
dist/
build/

# IDE
.vscode/
.idea/
*.swp
*.swo

# OS
.DS_Store
Thumbs.db
```

#### Python 项目

```gitignore
# Python
__pycache__/
*.pyc
*.pyo
*.pyd
.Python
*.so
*.egg
*.egg-info/
dist/
build/

# Virtual Environment
venv/
env/
ENV/

# IDE
.vscode/
.idea/
*.swp

# OS
.DS_Store

# Environment
.env
```

#### Java 项目

```gitignore
# Java
*.class
*.jar
*.war
*.ear
target/

# IDE
.idea/
*.iml
.eclipse/
.settings/

# OS
.DS_Store
Thumbs.db
```

### .gitignore 不生效？

如果文件已经被 Git 跟踪，`.gitignore` 不会生效。需要先取消跟踪：

```bash
# 从 Git 中删除文件（但保留本地文件）
git rm --cached 文件名

# 删除整个目录
git rm -r --cached 目录名

# 提交更改
git commit -m "chore: 更新 .gitignore"
```

---

## 下一步

掌握基本概念和操作后，继续学习：

👉 **[03 - Git 分支管理](./03-Git分支管理.md)** - 学习 Git 分支的创建、合并和管理

---

## 💡 实用小贴士

1. **提交频率**：
   - 经常提交，保持提交粒度适中
   - 每个提交应该是一个独立的功能或修复
   - 不要把多个不相关的修改放在一个提交中

2. **提交信息**：
   - 第一行不超过 50 字符（简短描述）
   - 详细描述应该说明"为什么"而不是"是什么"
   - 使用祈使语气："添加"而不是"添加了"

3. **工作流程**：
   - 养成 `git status` 习惯，随时了解状态
   - 提交前使用 `git diff` 检查修改
   - 推送前使用 `git pull` 同步远程更新

4. **安全建议**：
   - 不要提交敏感信息（密码、密钥）
   - 使用 `.gitignore` 忽略敏感文件
   - 如果不小心提交了敏感信息，立即修改密码/密钥

---

**相关教程：**
- 👈 [01 - Git 安装与配置](./01-Git安装与配置.md)
- 👉 [03 - Git 分支管理](./03-Git分支管理.md)
- 👈 [返回总览](./README.md)

