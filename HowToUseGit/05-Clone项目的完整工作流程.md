# 05 - Clone 项目的完整工作流程

本教程将详细介绍 clone 一个项目后的完整开发工作流程，从创建分支到 PR 合并后的操作。

## 📋 目录

- [场景说明](#场景说明)
- [完整工作流程](#完整工作流程)
- [PR 合并后怎么办](#pr-合并后怎么办)
- [常见问题](#常见问题)
- [最佳实践](#最佳实践)

---

## 场景说明

**适用场景：**
- 你是团队项目的成员，有直接推送权限
- 项目仓库在你的组织或团队账号下
- 你需要为项目贡献新功能或修复 bug

**不适用场景：**
- 参与开源项目（应该使用 Fork，见[06 - Fork 项目的完整工作流程](./06-Fork项目的完整工作流程.md)）
- 你没有推送权限的项目

---

## 完整工作流程

### 第 1 步：Clone 项目到本地

```bash
# 使用 HTTPS 克隆
git clone https://github.com/组织名/项目名.git

# 或使用 SSH（推荐，需要先配置 SSH 密钥）
git clone git@github.com:组织名/项目名.git

# 进入项目目录
cd 项目名
```

**验证克隆成功：**
```bash
# 查看远程仓库
git remote -v

# 输出应该显示：
# origin  git@github.com:组织名/项目名.git (fetch)
# origin  git@github.com:组织名/项目名.git (push)
```

---

### 第 2 步：查看当前状态

```bash
# 查看当前分支
git branch

# 查看所有分支（包括远程）
git branch -a

# 查看当前状态
git status
```

**通常你会在 `main` 或 `master` 分支上。**

---

### 第 3 步：创建新分支

**⚠️ 重要：永远不要直接在 main 分支上开发！**

```bash
# 确保在 main 分支
git checkout main

# 拉取最新代码
git pull origin main

# 创建并切换到新分支
git checkout -b feature/你的功能描述

# 或分两步
git branch feature/你的功能描述
git checkout feature/你的功能描述
```

**分支命名建议：**
```bash
# 新功能
git checkout -b feature/用户登录
git checkout -b feature/add-shopping-cart

# Bug 修复
git checkout -b bugfix/修复登录错误
git checkout -b bugfix/fix-header-layout

# 紧急修复
git checkout -b hotfix/修复支付bug

# 文档更新
git checkout -b docs/更新README
```

---

### 第 4 步：进行开发

**工作流程：**

```bash
# 1. 编写代码
# ... 使用你喜欢的编辑器开发 ...

# 2. 查看修改
git status                    # 查看哪些文件被修改
git diff                      # 查看具体修改内容

# 3. 添加到暂存区
git add .                     # 添加所有修改
# 或
git add 文件名                # 添加指定文件

# 4. 提交修改
git commit -m "feat: 添加用户登录功能"

# 5. 继续开发，重复 1-4
```

**提交信息规范：**
```bash
git commit -m "feat: 添加新功能"
git commit -m "fix: 修复 bug"
git commit -m "docs: 更新文档"
git commit -m "style: 调整代码格式"
git commit -m "refactor: 重构代码"
git commit -m "test: 添加测试"
git commit -m "chore: 更新依赖"
```

---

### 第 5 步：推送分支到远程

```bash
# 第一次推送新分支（建立追踪关系）
git push -u origin feature/你的功能描述

# 后续推送
git push
```

**说明：**
- `-u` 参数建立本地分支和远程分支的追踪关系
- 之后只需要 `git push` 就可以推送到对应的远程分支

---

### 第 6 步：保持分支更新（重要！）

在开发过程中，主分支可能有新的提交，需要及时同步：

#### 方法一：使用 Merge（推荐新手）

```bash
# 1. 提交当前工作
git add .
git commit -m "feat: 完成某功能"

# 2. 切换到 main 分支
git checkout main

# 3. 拉取最新代码
git pull origin main

# 4. 切换回功能分支
git checkout feature/你的功能描述

# 5. 合并 main 的更新
git merge main

# 6. 如果有冲突，解决冲突后：
git add .
git commit -m "merge: 合并 main 分支的更新"

# 7. 推送到远程
git push
```

---

#### 方法二：使用 Rebase（推荐有经验者）

```bash
# 1. 提交当前工作
git add .
git commit -m "feat: 完成某功能"

# 2. 拉取 main 的最新代码并 rebase
git fetch origin
git rebase origin/main

# 3. 如果有冲突，解决冲突后：
git add .
git rebase --continue

# 4. 推送到远程（需要强制推送）
git push -f origin feature/你的功能描述
```

**Merge vs Rebase 的选择：**
- **Merge**：保留完整历史，安全，适合新手
- **Rebase**：保持线性历史，更清晰，但需要强制推送

---

### 第 7 步：创建 Pull Request (PR)

**在 GitHub 上操作：**

1. **访问项目仓库**
   - GitHub 通常会自动显示 "Compare & pull request" 按钮
   - 或点击 **Pull requests** -> **New pull request**

2. **选择分支**
   - **base**：目标分支（通常是 `main`）
   - **compare**：你的功能分支

3. **填写 PR 信息**

```markdown
## 📝 变更描述
简要描述这个 PR 做了什么

## 🎯 变更类型
- [x] 新功能（feature）
- [ ] Bug 修复（bugfix）
- [ ] 文档更新（docs）
- [ ] 代码重构（refactor）
- [ ] 性能优化（performance）

## 💡 实现方案
详细说明：
- 做了什么修改
- 为什么要做这些修改
- 如何实现的

## ✅ 测试
- [x] 本地测试通过
- [x] 添加了单元测试
- [x] 更新了文档

## 📷 截图（如果适用）
（添加截图或 GIF）

## 🔗 相关 Issue
Closes #123
```

4. **分配审查者**
   - 在右侧 **Reviewers** 中选择审查人员
   - 通常是你的 team leader 或同事

5. **添加标签**
   - 选择合适的 Labels（如 `enhancement`、`bug`）

6. **点击 "Create pull request"**

---

### 第 8 步：代码审查和修改

#### 响应审查意见

审查者会审查你的代码并提出意见：

**修改代码并更新 PR：**

```bash
# 1. 根据反馈修改代码
# ... 修改文件 ...

# 2. 提交修改
git add .
git commit -m "fix: 根据审查意见修改登录逻辑"

# 3. 推送（PR 会自动更新）
git push
```

**在 GitHub 上回复评论：**
- 点击 "Resolve conversation" 标记已解决
- 如有疑问，在评论中讨论

---

#### 如果主分支有新提交

在 PR 审查期间，主分支可能有新的提交，需要更新你的分支：

```bash
# 方法一：Merge
git checkout feature/你的功能描述
git pull origin main
git push

# 方法二：Rebase
git checkout feature/你的功能描述
git fetch origin
git rebase origin/main
git push -f
```

---

### 第 9 步：PR 被合并

**合并方式（由维护者选择）：**

1. **Merge commit** - 保留所有提交历史
2. **Squash and merge** - 将所有提交压缩为一个（推荐）
3. **Rebase and merge** - 保持线性历史

**合并后，GitHub 会提示删除分支。**

---

## PR 合并后怎么办

### 场景 A：继续在当前分支开发（不推荐）❌

**不推荐原因：**
- 分支已经合并，应该视为"完成"
- 继续使用可能造成混淆
- 不符合 Git Flow 最佳实践

**如果确实要继续：**
```bash
# 1. 更新本地分支
git checkout feature/已合并的分支
git pull origin main

# 2. 继续开发
# ... 修改代码 ...

# 3. 创建新的 PR
```

---

### 场景 B：切换回 main，删除分支，开新分支（推荐）✅

**推荐原因：**
- 保持分支简洁
- 符合 Git Flow 最佳实践
- 每个分支对应一个功能或修复

**操作步骤：**

```bash
# 1. 切换回 main 分支
git checkout main

# 2. 拉取最新代码（包含刚刚合并的内容）
git pull origin main

# 3. 删除本地分支
git branch -d feature/已完成的功能

# 4. 删除远程分支（如果还没删除）
git push origin --delete feature/已完成的功能

# 5. 开始新功能，创建新分支
git checkout -b feature/新功能
```

---

### 一键清理脚本

**创建 alias 简化操作：**

```bash
# 添加到 .bashrc 或 .zshrc
alias git-pr-done='git checkout main && git pull origin main && git fetch -p'

# 使用
git-pr-done
git branch -d feature/旧分支
git checkout -b feature/新功能
```

---

## 常见问题

### 1. 忘记创建分支，直接在 main 上修改了代码

```bash
# 创建新分支（会带上你的修改）
git checkout -b feature/新分支

# 提交修改
git add .
git commit -m "feat: 添加功能"
git push -u origin feature/新分支

# main 分支还原
git checkout main
git reset --hard origin/main
```

---

### 2. PR 审查时发现提交太多太乱

**使用交互式 rebase 清理提交：**

```bash
# 1. 查看最近的提交
git log --oneline -10

# 2. 交互式 rebase（假设要整理最近 5 次提交）
git rebase -i HEAD~5

# 3. 在编辑器中，将多个 pick 改为 squash
pick a1b2c3d feat: 添加功能
squash b2c3d4e fix: 修复bug
squash c3d4e5f fix: 修复拼写
squash d4e5f6g refactor: 优化代码

# 4. 保存退出，编辑合并后的提交信息

# 5. 强制推送
git push -f origin feature/你的分支
```

---

### 3. PR 有冲突怎么办

**方法一：在本地解决**

```bash
# 1. 拉取 main 的最新代码
git checkout feature/你的分支
git pull origin main

# 2. 解决冲突
# ... 编辑冲突文件 ...

# 3. 标记为已解决
git add .
git commit -m "merge: 解决与 main 的冲突"

# 4. 推送
git push
```

**方法二：在 GitHub 上解决**

GitHub 提供了网页版冲突解决工具：
1. 在 PR 页面点击 "Resolve conflicts"
2. 编辑文件，保留需要的内容
3. 点击 "Mark as resolved"
4. 点击 "Commit merge"

---

### 4. PR 被拒绝了怎么办

**原因可能是：**
- 代码质量不符合要求
- 不符合项目规范
- 功能不需要或设计不合理

**应对方法：**
1. 查看审查意见，理解被拒绝的原因
2. 与审查者沟通讨论
3. 根据反馈修改代码
4. 或关闭 PR，重新设计方案

```bash
# 如果决定放弃这个 PR
git checkout main
git branch -D feature/被拒绝的功能
git push origin --delete feature/被拒绝的功能
```

---

### 5. 推送时提示需要先 pull

```bash
# 原因：远程分支有新的提交

# 解决方法一：拉取并合并
git pull origin feature/你的分支

# 解决方法二：拉取并 rebase
git pull --rebase origin feature/你的分支

# 如果确定要覆盖远程（谨慎）
git push -f origin feature/你的分支
```

---

### 6. 多个人在同一个分支开发

**不推荐，但如果必须：**

```bash
# 开发前，先拉取
git pull origin feature/共享分支

# 提交前，再次拉取
git pull origin feature/共享分支

# 如果有冲突，解决后再推送
git push origin feature/共享分支
```

**更好的方法：**
- 每人创建自己的分支
- 分别提交 PR
- 避免直接在共享分支上开发

---

## 最佳实践

### 1. 工作流程总结

```
Clone -> 创建分支 -> 开发 -> 提交 -> 推送 -> 创建 PR -> 审查 -> 合并 -> 回到 main -> 删除旧分支 -> 创建新分支 -> 循环
```

**每次开始新功能：**
```bash
# 1. 确保在最新的 main 上
git checkout main
git pull origin main

# 2. 创建新分支
git checkout -b feature/新功能

# 3. 开发、提交、推送
git add .
git commit -m "feat: ..."
git push -u origin feature/新功能

# 4. 创建 PR，等待审查和合并

# 5. PR 合并后，回到 main
git checkout main
git pull origin main
git branch -d feature/新功能

# 6. 重复
```

---

### 2. 提交频率建议

- ✅ **经常提交**：每完成一个小功能就提交
- ✅ **保持提交粒度适中**：每个提交是一个独立的改动
- ❌ **避免大提交**：不要一次提交几百行代码
- ❌ **避免提交半成品**：确保每次提交代码是可运行的

---

### 3. 分支管理建议

- ✅ **一个分支一个功能**：不要在一个分支里做多个不相关的事
- ✅ **分支生命周期短**：尽快完成功能，合并后删除
- ✅ **及时同步 main**：定期将 main 的更新合并到功能分支
- ❌ **不要在 main 上直接开发**
- ❌ **不要保留太多本地分支**：及时清理已合并的分支

---

### 4. PR 建议

- ✅ **PR 应该小而专注**：一个 PR 只做一件事
- ✅ **描述清晰**：说明做了什么、为什么、怎么做
- ✅ **及时响应审查意见**
- ✅ **测试充分**：确保代码在本地测试通过
- ❌ **不要创建巨大的 PR**（超过 500 行应该拆分）

---

### 5. 团队协作建议

- ✅ **遵循团队规范**：代码风格、命名约定、提交信息格式
- ✅ **使用 Issue**：在 Issue 中讨论功能设计，再开始开发
- ✅ **代码审查认真**：既审查别人的代码，也欢迎别人审查你的
- ✅ **及时沟通**：遇到问题及时沟通，不要埋头苦干

---

## 完整流程示例

**场景：为项目添加用户登录功能**

```bash
# 1. Clone 项目（首次）
git clone git@github.com:mycompany/myproject.git
cd myproject

# 2. 创建功能分支
git checkout main
git pull origin main
git checkout -b feature/user-login

# 3. 开发功能
# ... 编写登录页面 ...
git add src/pages/Login.jsx
git commit -m "feat: 添加登录页面 UI"

# ... 编写登录 API ...
git add src/api/auth.js
git commit -m "feat: 实现登录 API 调用"

# ... 添加表单验证 ...
git add src/utils/validation.js
git commit -m "feat: 添加登录表单验证"

# 4. 推送到远程
git push -u origin feature/user-login

# 5. 创建 PR（在 GitHub 上）
# - 填写 PR 描述
# - 分配审查者
# - 添加标签

# 6. 审查期间，main 有新提交，更新分支
git fetch origin
git rebase origin/main
git push -f origin feature/user-login

# 7. 根据审查意见修改
# ... 修改代码 ...
git add .
git commit -m "fix: 根据审查意见优化错误处理"
git push

# 8. PR 被批准并合并

# 9. 清理并准备下一个功能
git checkout main
git pull origin main
git branch -d feature/user-login
git push origin --delete feature/user-login

# 10. 开始新功能
git checkout -b feature/user-profile
```

---

## 下一步

学习 Fork 项目的工作流程：

👉 **[06 - Fork 项目的完整工作流程](./06-Fork项目的完整工作流程.md)** - 学习如何参与开源项目

---

**相关教程：**
- 👈 [04 - Git 进阶技巧](./04-Git进阶技巧.md)
- 👉 [06 - Fork 项目的完整工作流程](./06-Fork项目的完整工作流程.md)
- 👈 [返回总览](./README.md)

