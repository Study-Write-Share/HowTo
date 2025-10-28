# 03 - Git 分支管理

本教程将详细介绍 Git 分支的创建、切换、合并等操作。

## 📋 目录

- [什么是分支](#什么是分支)
- [分支操作](#分支操作)
- [合并分支](#合并分支)
- [解决冲突](#解决冲突)
- [分支策略](#分支策略)

---

## 什么是分支

### 分支的概念

分支允许你在不影响主代码的情况下开发新功能、修复 bug 或进行实验。

**理解分支：**
- 分支就像平行宇宙，可以在独立的分支上开发
- 主分支（main/master）通常是稳定的生产代码
- 功能分支用于开发新功能
- 完成后可以将分支合并回主分支

**示意图：**
```
main:     A---B---C---F---G
               \       /
feature:        D---E
```

---

### 常见分支类型

- **main/master** - 主分支，稳定的生产代码
- **develop** - 开发分支，集成所有功能
- **feature/** - 功能分支，开发新功能
- **bugfix/** - Bug 修复分支
- **hotfix/** - 紧急修复分支
- **release/** - 发布分支

---

## 分支操作

### 1. 查看分支

```bash
# 查看本地分支
git branch

# 查看所有分支（包括远程分支）
git branch -a

# 查看远程分支
git branch -r

# 查看分支及最后一次提交
git branch -v

# 查看已合并到当前分支的分支
git branch --merged

# 查看未合并到当前分支的分支
git branch --no-merged
```

**输出示例：**
```
* main              # * 表示当前分支
  feature-login
  bugfix-header
  remotes/origin/main
  remotes/origin/develop
```

---

### 2. 创建分支

```bash
# 创建新分支（不切换）
git branch 分支名

# 创建新分支并切换
git checkout -b 分支名

# 或使用新命令（Git 2.23+）
git switch -c 分支名

# 从指定提交创建分支
git branch 分支名 <commit-id>

# 从远程分支创建本地分支
git checkout -b 本地分支名 origin/远程分支名
```

---

### 3. 切换分支

```bash
# 切换分支
git checkout 分支名

# 或使用新命令（Git 2.23+）
git switch 分支名

# 切换到上一个分支
git checkout -
# 或
git switch -

# 强制切换（丢弃当前修改）⚠️
git checkout -f 分支名
```

**⚠️ 注意**：切换分支前，确保当前分支没有未提交的修改，否则：
- 提交修改：`git commit`
- 或暂存修改：`git stash`

---

### 4. 分支命名建议

使用有意义的分支名：

```bash
# 功能分支
git checkout -b feature/用户登录
git checkout -b feature/添加购物车
git checkout -b feature/user-profile

# Bug 修复
git checkout -b bugfix/修复登录错误
git checkout -b bugfix/fix-header-layout

# 热修复（紧急修复）
git checkout -b hotfix/修复支付bug
git checkout -b hotfix/security-patch

# 文档更新
git checkout -b docs/更新README
git checkout -b docs/api-documentation
```

**命名规范：**
- 使用小写字母
- 使用连字符 `-` 或下划线 `_` 分隔单词
- 使用类型前缀（feature/, bugfix/, hotfix/）
- 简洁但有描述性

---

### 5. 删除分支

```bash
# 删除本地分支（已合并）
git branch -d 分支名

# 强制删除本地分支（未合并）
git branch -D 分支名

# 删除远程分支
git push origin --delete 分支名
# 或
git push origin :分支名
```

**示例：**
```bash
# 删除已合并的分支
git branch -d feature-login

# 强制删除未合并的分支
git branch -D feature-abandoned

# 删除远程分支
git push origin --delete feature-login
```

---

### 6. 重命名分支

```bash
# 重命名当前分支
git branch -m 新分支名

# 重命名其他分支
git branch -m 旧分支名 新分支名

# 重命名并推送到远程
git branch -m 旧名 新名
git push origin --delete 旧名
git push origin 新名
git push origin -u 新名
```

---

## 合并分支

### 1. Merge 合并

将一个分支的修改合并到另一个分支。

```bash
# 切换到目标分支（通常是 main）
git checkout main

# 合并其他分支到当前分支
git merge 分支名

# 示例：将 feature-login 合并到 main
git checkout main
git pull origin main          # 先更新 main
git merge feature-login       # 合并 feature
git push origin main          # 推送到远程
```

---

### 2. 合并策略

#### Fast-forward 合并（快进合并）

当目标分支没有新提交时，Git 会进行快进合并：

```
合并前：
main:    A---B---C
              \
feature:       D---E

合并后：
main:    A---B---C---D---E
```

```bash
# 默认会尝试快进合并
git merge feature-branch

# 禁止快进合并（总是创建合并提交）
git merge --no-ff feature-branch
```

---

#### 三方合并（Three-way merge）

当两个分支都有新提交时，Git 会创建一个合并提交：

```
合并前：
main:    A---B---C---F
              \
feature:       D---E

合并后：
main:    A---B---C---F---G
              \         /
feature:       D---E---
```

```bash
# 三方合并会自动创建合并提交
git merge feature-branch
```

---

#### Squash 合并（压缩合并）

将所有提交压缩成一个提交再合并：

```bash
# Squash 合并
git merge --squash feature-branch
git commit -m "feat: 添加用户登录功能（包含所有修改）"
```

**使用场景：**
- 功能分支有很多小提交
- 希望保持主分支历史清晰
- 不想在主分支保留功能分支的所有提交

---

### 3. 合并选项

```bash
# 不自动提交合并结果（可以手动检查）
git merge --no-commit feature-branch

# 禁止快进合并
git merge --no-ff feature-branch

# Squash 合并
git merge --squash feature-branch

# 仅合并特定文件
git checkout feature-branch -- 文件名
```

---

## 解决冲突

### 1. 什么是冲突

当两个分支修改了同一文件的同一部分时，Git 无法自动合并，就会产生冲突。

---

### 2. 冲突的产生

```bash
# 尝试合并
git merge feature-branch

# 如果有冲突，会看到：
Auto-merging file.txt
CONFLICT (content): Merge conflict in file.txt
Automatic merge failed; fix conflicts and then commit the result.
```

---

### 3. 查看冲突文件

```bash
# 查看冲突文件
git status

# 输出示例：
Unmerged paths:
  (use "git add <file>..." to mark resolution)
        both modified:   file.txt
```

---

### 4. 冲突标记

打开冲突文件，会看到冲突标记：

```
<<<<<<< HEAD
你的修改（当前分支的内容）
=======
别人的修改（要合并的分支的内容）
>>>>>>> feature-branch
```

---

### 5. 解决冲突

**步骤：**

**1. 打开冲突文件**

```
<<<<<<< HEAD
const name = "张三";
=======
const name = "李四";
>>>>>>> feature-branch
```

**2. 手动编辑，保留需要的内容**

```javascript
// 选择一个
const name = "张三";

// 或选择另一个
const name = "李四";

// 或合并两者
const name = "张三和李四";
```

**3. 删除冲突标记**

删除 `<<<<<<<`, `=======`, `>>>>>>>` 这些标记。

**4. 标记为已解决**

```bash
git add file.txt
```

**5. 完成合并**

```bash
git commit -m "解决合并冲突"
```

---

### 6. 使用工具解决冲突

#### 使用 VS Code

VS Code 提供了可视化的冲突解决界面：

- `Accept Current Change` - 接受当前分支的修改
- `Accept Incoming Change` - 接受要合并的分支的修改
- `Accept Both Changes` - 保留两者
- `Compare Changes` - 比较差异

---

#### 使用 Git 内置工具

```bash
# 启动合并工具
git mergetool

# 使用 vimdiff（需要配置）
git mergetool --tool=vimdiff
```

---

### 7. 取消合并

```bash
# 取消合并，回到合并前的状态
git merge --abort
```

---

### 8. 冲突解决最佳实践

1. **经常同步主分支**
   ```bash
   # 在功能分支上定期合并 main 的更新
   git checkout feature-branch
   git merge main
   ```

2. **小步提交**
   - 频繁提交，每次提交改动小
   - 减少冲突的可能性
   - 即使有冲突也容易解决

3. **沟通协作**
   - 团队成员之间及时沟通
   - 避免多人同时修改同一文件

4. **使用分支保护**
   - 在 GitHub 上设置分支保护规则
   - 要求 PR 合并前必须解决冲突

---

## 分支策略

### 1. Git Flow

经典的分支管理模型，适合版本发布型项目。

```
main (生产环境)
  |
develop (开发分支)
  |
  |-- feature/功能A
  |-- feature/功能B
  |-- release/v1.0
  |-- hotfix/紧急修复
```

**分支说明：**
- **main**：生产环境代码，只能从 release 或 hotfix 合并
- **develop**：开发分支，集成所有功能
- **feature/**：功能分支，从 develop 创建，完成后合并回 develop
- **release/**：发布分支，准备发布时从 develop 创建
- **hotfix/**：紧急修复，从 main 创建，修复后合并到 main 和 develop

**工作流程：**
```bash
# 1. 从 develop 创建功能分支
git checkout develop
git checkout -b feature/用户登录

# 2. 开发功能
git add .
git commit -m "feat: 添加登录功能"

# 3. 合并回 develop
git checkout develop
git merge feature/用户登录

# 4. 准备发布
git checkout -b release/v1.0 develop

# 5. 发布到 main
git checkout main
git merge release/v1.0
git tag v1.0.0

# 6. 合并回 develop
git checkout develop
git merge release/v1.0
```

---

### 2. GitHub Flow（推荐，更简单）

适合持续部署的项目。

```
main (生产环境，随时可部署)
  |
  |-- feature/功能A
  |-- feature/功能B
  |-- bugfix/修复C
```

**特点：**
- 只有一个长期分支：main
- 所有功能分支从 main 创建
- 完成后通过 PR 合并回 main
- main 分支随时可部署

**工作流程：**
```bash
# 1. 从 main 创建功能分支
git checkout main
git pull origin main
git checkout -b feature/用户登录

# 2. 开发并提交
git add .
git commit -m "feat: 添加登录功能"

# 3. 推送到远程
git push -u origin feature/用户登录

# 4. 创建 Pull Request（在 GitHub 上）

# 5. 代码审查通过后合并到 main

# 6. 删除功能分支
git branch -d feature/用户登录
git push origin --delete feature/用户登录
```

---

### 3. Trunk-Based Development

主干开发模式，适合快速迭代的项目。

**特点：**
- 所有开发在 main 分支进行
- 功能分支生命周期很短（1-2天）
- 使用特性开关（Feature Toggle）控制功能发布

---

## 实用技巧

### 1. 比较分支差异

```bash
# 查看两个分支的差异
git diff main feature-branch

# 查看分支包含哪些提交
git log main..feature-branch

# 查看某个文件在两个分支的差异
git diff main feature-branch -- 文件名
```

---

### 2. 追踪远程分支

```bash
# 创建并追踪远程分支
git checkout -b 本地分支名 origin/远程分支名

# 或
git checkout --track origin/远程分支名

# 为现有分支设置上游分支
git branch --set-upstream-to=origin/远程分支名
# 或
git branch -u origin/远程分支名
```

---

### 3. 整理分支

```bash
# 删除所有已合并的本地分支
git branch --merged | grep -v "\*" | xargs -n 1 git branch -d

# 清理远程已删除的分支引用
git fetch --prune
# 或
git fetch -p
```

---

### 4. 查看分支图

```bash
# 图形化查看分支历史
git log --graph --oneline --all

# 更详细的图形
git log --graph --all --decorate --oneline
```

---

## 常见问题

### 1. 忘记创建分支，在 main 上修改了代码

```bash
# 创建新分支（会带上你的修改）
git checkout -b feature-新分支

# 提交修改
git add .
git commit -m "提交信息"
git push -u origin feature-新分支

# main 分支还原
git checkout main
git reset --hard origin/main
```

---

### 2. 切换分支时提示有未提交的修改

```bash
# 方法 1：提交修改
git add .
git commit -m "临时提交"

# 方法 2：暂存修改
git stash
git checkout 其他分支
# 回来后恢复
git checkout 原分支
git stash pop
```

---

### 3. 合并后想撤销

```bash
# 如果还没有推送
git reset --hard HEAD~1

# 如果已经推送
git revert -m 1 HEAD
git push
```

---

## 下一步

掌握分支管理后，继续学习：

👉 **[04 - Git 进阶技巧](./04-Git进阶技巧.md)** - 学习 rebase、cherry-pick 等进阶技巧

👉 **[05 - Clone 项目的完整工作流程](./05-Clone项目的完整工作流程.md)** - 学习完整的开发工作流程

---

## 💡 最佳实践

1. **分支命名**：使用有意义的名称，包含类型前缀
2. **短期分支**：功能分支应该短期存在，完成后及时合并和删除
3. **定期同步**：经常从主分支合并更新，减少冲突
4. **代码审查**：使用 Pull Request 进行代码审查
5. **保护主分支**：设置分支保护规则，防止直接推送到主分支

---

**相关教程：**
- 👈 [02 - Git 基本概念与操作](./02-Git基本概念与操作.md)
- 👉 [04 - Git 进阶技巧](./04-Git进阶技巧.md)
- 👈 [返回总览](./README.md)

