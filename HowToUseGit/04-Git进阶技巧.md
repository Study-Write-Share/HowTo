# 04 - Git 进阶技巧

本教程将介绍 Git 的进阶功能和技巧，帮助你更高效地使用 Git。

## 📋 目录

- [Rebase 变基](#rebase-变基)
- [Cherry-pick 精选提交](#cherry-pick-精选提交)
- [交互式 Rebase](#交互式-rebase)
- [查找问题提交](#查找问题提交)
- [子模块和 Worktree](#子模块和-worktree)
- [其他实用技巧](#其他实用技巧)

---

## Rebase 变基

### 1. 什么是 Rebase

Rebase 可以将一系列提交"移动"到新的基础提交上，使提交历史呈线性。

**Merge vs Rebase 对比：**

**Merge（合并）：**
```
main:    A---B---C---F---G
              \         /
feature:       D---E---

历史保留了分支结构
```

**Rebase（变基）：**
```
main:    A---B---C---D'---E'

将 feature 分支的提交"移动"到 main 之后
历史呈线性，没有分支结构
```

---

### 2. 基本用法

```bash
# 将当前分支 rebase 到目标分支
git rebase 目标分支

# 示例：将 feature 分支 rebase 到 main
git checkout feature
git rebase main

# 或一步到位
git rebase main feature
```

---

### 3. Rebase 工作流程

**场景：feature 分支想要获取 main 的最新更新**

```bash
# 1. 切换到 feature 分支
git checkout feature

# 2. rebase 到 main
git rebase main

# 3. 如果有冲突，解决冲突后：
git add .
git rebase --continue

# 4. 如果想放弃 rebase：
git rebase --abort

# 5. rebase 完成后，推送到远程（需要强制推送）
git push -f origin feature
```

---

### 4. 何时使用 Rebase

**✅ 适合使用 Rebase：**
- 更新自己的功能分支（将 main 的更新应用到自己的分支）
- 清理本地提交历史（未推送的提交）
- 保持线性的提交历史

**❌ 不适合使用 Rebase：**
- 已经推送到公共分支的提交
- 多人协作的分支
- 不想改变历史的情况

**⚠️ 黄金法则：不要 rebase 已经推送到公共仓库的提交！**

---

### 5. Rebase vs Merge 的选择

| 场景 | 建议 |
|------|------|
| 更新功能分支 | Rebase（保持线性） |
| 合并功能到主分支 | Merge（保留历史） |
| 公共分支 | Merge |
| 本地提交整理 | Rebase |

---

## Cherry-pick 精选提交

### 1. 什么是 Cherry-pick

Cherry-pick 可以将某个提交应用到当前分支。

**使用场景：**
- 只需要某个分支的部分提交
- 将 bug 修复从一个分支复制到另一个分支
- 错误地在错误的分支提交了代码

---

### 2. 基本用法

```bash
# 将指定提交应用到当前分支
git cherry-pick <commit-id>

# 示例
git cherry-pick a1b2c3d

# 应用多个提交
git cherry-pick <commit-id1> <commit-id2>

# 应用一个范围的提交
git cherry-pick <commit-id1>..<commit-id2>

# 不自动提交（可以修改）
git cherry-pick -n <commit-id>
# 或
git cherry-pick --no-commit <commit-id>
```

---

### 3. Cherry-pick 实例

**场景：bug 修复需要应用到多个分支**

```bash
# 在 hotfix 分支修复了 bug，提交为 a1b2c3d

# 将修复应用到 develop 分支
git checkout develop
git cherry-pick a1b2c3d

# 将修复应用到 release 分支
git checkout release
git cherry-pick a1b2c3d
```

---

### 4. 解决 Cherry-pick 冲突

```bash
# 如果 cherry-pick 产生冲突
# 1. 解决冲突
# 2. 标记为已解决
git add .

# 3. 继续 cherry-pick
git cherry-pick --continue

# 或放弃 cherry-pick
git cherry-pick --abort
```

---

## 交互式 Rebase

### 1. 什么是交互式 Rebase

交互式 Rebase 可以修改、合并、删除、重排提交历史。

**用途：**
- 修改提交信息
- 合并多个提交为一个
- 删除不需要的提交
- 重新排列提交顺序

---

### 2. 启动交互式 Rebase

```bash
# 修改最近 3 次提交
git rebase -i HEAD~3

# 从指定提交开始
git rebase -i <commit-id>

# 修改所有提交（从 root 开始）
git rebase -i --root
```

---

### 3. 交互式操作命令

启动后会打开编辑器，显示提交列表：

```
pick a1b2c3d 第一次提交
pick b2c3d4e 第二次提交
pick c3d4e5f 第三次提交

# Rebase commands:
# p, pick = 保留提交
# r, reword = 保留提交，但修改提交信息
# e, edit = 保留提交，但停下来修改
# s, squash = 合并到前一个提交，保留提交信息
# f, fixup = 合并到前一个提交，丢弃提交信息
# d, drop = 删除提交
```

---

### 4. 常用场景

#### 修改提交信息

```bash
# 1. 启动交互式 rebase
git rebase -i HEAD~3

# 2. 将 pick 改为 reword（或 r）
reword a1b2c3d 第一次提交
pick b2c3d4e 第二次提交
pick c3d4e5f 第三次提交

# 3. 保存退出，会打开编辑器让你修改提交信息
```

---

#### 合并多个提交

```bash
# 1. 启动交互式 rebase
git rebase -i HEAD~3

# 2. 将要合并的提交标记为 squash（或 s）
pick a1b2c3d 第一次提交
squash b2c3d4e 第二次提交
squash c3d4e5f 第三次提交

# 3. 保存退出，会打开编辑器让你编写合并后的提交信息
```

**示例：**
```bash
# 假设你有 3 次提交：
# - commit3: 修复拼写错误
# - commit2: 添加功能
# - commit1: 初始提交

# 使用 rebase 将 commit2 和 commit3 合并
git rebase -i HEAD~2

# 在编辑器中：
pick commit2 添加功能
squash commit3 修复拼写错误

# 保存后会提示编辑合并后的提交信息
```

---

#### 删除提交

```bash
# 1. 启动交互式 rebase
git rebase -i HEAD~3

# 2. 将要删除的提交标记为 drop（或直接删除该行）
pick a1b2c3d 第一次提交
drop b2c3d4e 不需要的提交
pick c3d4e5f 第三次提交

# 3. 保存退出
```

---

#### 重新排列提交

```bash
# 1. 启动交互式 rebase
git rebase -i HEAD~3

# 2. 直接调整提交顺序
pick c3d4e5f 第三次提交
pick a1b2c3d 第一次提交
pick b2c3d4e 第二次提交

# 3. 保存退出
```

---

## 查找问题提交

### 1. Git Blame

查看文件每一行是谁修改的。

```bash
# 查看文件每行的最后修改信息
git blame 文件名

# 查看指定行范围
git blame -L 10,20 文件名

# 查看某次提交之前的 blame
git blame <commit-id> -- 文件名

# 只显示作者和时间
git blame -s 文件名
```

**输出示例：**
```
a1b2c3d (张三 2024-01-15 10:30:00 +0800 1) const name = "张三";
b2c3d4e (李四 2024-01-20 14:00:00 +0800 2) const age = 25;
```

---

### 2. Git Bisect

使用二分查找定位引入 bug 的提交。

```bash
# 1. 开始二分查找
git bisect start

# 2. 标记当前版本为坏的
git bisect bad

# 3. 标记一个已知的好版本
git bisect good <commit-id>

# 4. Git 会自动切换到中间版本，测试后标记：
git bisect good  # 如果这个版本是好的
git bisect bad   # 如果这个版本是坏的

# 5. 重复步骤 4，直到找到第一个坏的提交

# 6. 结束二分查找
git bisect reset
```

**自动化 bisect：**
```bash
# 使用脚本自动测试
git bisect start HEAD v1.0
git bisect run npm test

# Git 会自动运行测试，找到第一个失败的提交
```

---

### 3. 搜索提交历史

```bash
# 在提交信息中搜索
git log --grep="关键词"

# 在代码中搜索（找到添加或删除某段代码的提交）
git log -S "代码片段"

# 查找添加或删除某个函数的提交
git log -G "函数名"

# 查找某个作者的提交
git log --author="作者名"

# 查找某个时间段的提交
git log --since="2024-01-01" --until="2024-12-31"

# 组合搜索
git log --author="张三" --grep="bug" --since="1 week ago"
```

---

## 子模块和 Worktree

### 1. 子模块（Submodules）

在项目中包含其他 Git 仓库。

```bash
# 添加子模块
git submodule add <仓库URL> <路径>

# 示例
git submodule add https://github.com/user/lib.git libs/my-lib

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
rm -rf .git/modules/<路径>
```

---

### 2. 工作树（Worktree）

同时在不同分支上工作，无需切换分支。

```bash
# 创建新的工作树
git worktree add <路径> <分支名>

# 示例：在 ../feature-branch 目录创建 feature 分支的工作树
git worktree add ../feature-branch feature

# 查看所有工作树
git worktree list

# 删除工作树
git worktree remove <路径>

# 清理工作树
git worktree prune
```

**使用场景：**
- 需要同时在多个分支工作
- 一边开发新功能，一边修复 bug
- 避免频繁切换分支

---

## 其他实用技巧

### 1. 使用 Git Alias（别名）

简化常用命令：

```bash
# 设置别名
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

### 2. 查看某个文件的历史版本

```bash
# 查看文件的修改历史
git log -- 文件名

# 查看文件在某次提交时的内容
git show <commit-id>:文件名

# 恢复文件到某个版本
git checkout <commit-id> -- 文件名
```

---

### 3. 查看某次提交的详细信息

```bash
# 查看提交详情
git show <commit-id>

# 只查看修改的文件列表
git show <commit-id> --name-only

# 查看修改的统计信息
git show <commit-id> --stat
```

---

### 4. 临时切换到某个提交

```bash
# 切换到某个历史提交（detached HEAD）
git checkout <commit-id>

# 查看代码后，返回当前分支
git checkout main
```

---

### 5. 清理未跟踪的文件

```bash
# 查看会删除哪些文件（不实际删除）
git clean -n

# 删除未跟踪的文件
git clean -f

# 删除未跟踪的文件和目录
git clean -fd

# 删除包括 .gitignore 中的文件
git clean -fdx
```

---

### 6. 查看文件的变更统计

```bash
# 查看每次提交的统计信息
git log --stat

# 查看每个作者的代码贡献量
git shortlog -sn

# 查看某个文件的修改次数和行数
git log --follow --oneline -- 文件名 | wc -l
```

---

### 7. 恢复误删的分支

```bash
# 查看所有操作记录（包括删除的分支）
git reflog

# 找到删除前的 commit id，然后恢复分支
git branch 分支名 <commit-id>
```

---

### 8. 配置 Git 代理

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

---

### 9. 大文件处理（Git LFS）

Git 不适合存储大文件，使用 Git LFS 管理大文件：

```bash
# 安装 Git LFS
git lfs install

# 跟踪大文件类型
git lfs track "*.psd"
git lfs track "*.mp4"
git lfs track "*.zip"

# 提交 .gitattributes
git add .gitattributes
git commit -m "添加 Git LFS 支持"

# 之后正常使用 git add、commit、push
git add large-file.mp4
git commit -m "添加视频文件"
git push
```

---

## 学习资源

- 📖 [Pro Git 中文版](https://git-scm.com/book/zh/v2)（官方免费电子书，强烈推荐）
- 🎮 [Learn Git Branching](https://learngitbranching.js.org/?locale=zh_CN)（交互式学习，非常直观）
- 📺 [Git 教学视频](https://www.bilibili.com/video/BV1pX4y1S7Dq/)（Bilibili）
- 📝 [Git 官方文档](https://git-scm.com/doc)
- 🔧 [Git 速查表](https://training.github.com/downloads/zh_CN/github-git-cheat-sheet/)

---

## 下一步

掌握进阶技巧后，继续学习实际工作流程：

👉 **[05 - Clone 项目的完整工作流程](./05-Clone项目的完整工作流程.md)** - 学习完整的开发工作流程

👉 **[06 - Fork 项目的完整工作流程](./06-Fork项目的完整工作流程.md)** - 学习如何参与开源项目

---

## 💡 最佳实践总结

1. **Rebase**：
   - 用于更新自己的功能分支
   - 不要 rebase 已推送的公共分支

2. **Cherry-pick**：
   - 只需要某个分支的部分提交时使用
   - 适合将 bug 修复应用到多个分支

3. **交互式 Rebase**：
   - 清理提交历史
   - 合并多个小提交为一个
   - 修改提交信息

4. **Git Bisect**：
   - 快速定位引入 bug 的提交
   - 可以自动化测试

5. **别名**：
   - 提高效率
   - 简化常用命令

---

**相关教程：**
- 👈 [03 - Git 分支管理](./03-Git分支管理.md)
- 👉 [05 - Clone 项目的完整工作流程](./05-Clone项目的完整工作流程.md)
- 👈 [返回总览](./README.md)

