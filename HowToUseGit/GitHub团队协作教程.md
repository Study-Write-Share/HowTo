# GitHub 团队协作教程

本教程将详细介绍如何使用 GitHub 进行团队协作开发，包括 Pull Request、Issue、Discussion 等核心功能。

## 📋 目录

- [什么是 GitHub](#什么是-github)
- [创建 GitHub 账号](#创建-github-账号)
- [配置 SSH 密钥](#配置-ssh-密钥)
- [创建和管理仓库](#创建和管理仓库)
- [Pull Request 工作流程](#pull-request-工作流程)
- [Issue 问题管理](#issue-问题管理)
- [Discussion 讨论区](#discussion-讨论区)
- [代码审查（Code Review）](#代码审查code-review)
- [项目管理工具](#项目管理工具)
- [团队协作最佳实践](#团队协作最佳实践)
- [GitHub Actions](#github-actions)
- [常见问题](#常见问题)

## 什么是 GitHub

GitHub 是全球最大的代码托管平台和开发者社区，基于 Git 版本控制系统。它提供：

- **代码托管** - 免费的公开和私有仓库
- **协作工具** - Pull Request、Issue、Discussion
- **代码审查** - 内置的代码审查功能
- **项目管理** - Projects、Milestones、Labels
- **自动化** - GitHub Actions CI/CD
- **文档** - Wiki、GitHub Pages
- **社交** - Follow、Star、Fork

### GitHub vs GitLab vs Bitbucket

| 特性 | GitHub | GitLab | Bitbucket |
|------|--------|--------|-----------|
| 免费私有仓库 | ✅ | ✅ | ✅ |
| 社区规模 | 最大 | 中等 | 较小 |
| CI/CD | GitHub Actions | 内置 GitLab CI | Bitbucket Pipelines |
| 适合场景 | 开源、团队协作 | DevOps、企业 | 小团队、Jira 集成 |

## 创建 GitHub 账号

### 1. 注册账号

1. 访问 https://github.com
2. 点击 "Sign up" 注册账号
3. 输入邮箱、密码、用户名
4. 完成人机验证
5. 验证邮箱

### 2. 完善个人资料

登录后，完善个人资料：

1. 点击头像 -> Settings
2. 填写个人信息：（做不做都行）
   - **Name**：真实姓名或昵称
   - **Bio**：个人简介
   - **Company**：公司/组织
   - **Location**：所在地
   - **Website**：个人网站
   - **Profile Picture**：上传头像

### 3. 设置双因素认证（推荐）

为了账号安全，建议启用双因素认证：

1. Settings -> Password and authentication
2. 点击 "Enable two-factor authentication"
3. 选择认证方式：
   - **App**：使用 Authenticator 等 APP（手机应用商店一般有这个软件，就叫“Authenticator”）
   - **SMS**：短信验证码
4. 保存恢复码（重要！）

## 配置 SSH 密钥（做不做都行，不做的话就是需要输密码）

使用 SSH 可以避免每次 push 都输入密码。

### 1. 检查是否已有 SSH 密钥

```bash
# Windows (PowerShell)
ls ~/.ssh

# 查找 id_rsa.pub、id_ed25519.pub 等文件
```

### 2. 生成 SSH 密钥

如果没有密钥，生成新的：

```bash
# 使用你的 GitHub 邮箱
ssh-keygen -t ed25519 -C "your.email@example.com"

# 如果系统不支持 ed25519，使用 RSA
ssh-keygen -t rsa -b 4096 -C "your.email@example.com"

# 提示输入文件名时，直接回车使用默认
# 提示输入密码时，可以设置密码或直接回车
```

### 3. 添加到 SSH agent

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

### 4. 添加公钥到 GitHub

```bash
# 复制公钥内容（Windows）
type ~/.ssh/id_ed25519.pub | clip

# Linux（需要 xclip）
cat ~/.ssh/id_ed25519.pub | xclip -selection clipboard

# macOS
pbcopy < ~/.ssh/id_ed25519.pub
```

然后在 GitHub 上添加：

1. 登录 GitHub
2. 点击头像 -> **Settings**
3. 左侧选择 **SSH and GPG keys**
4. 点击 **New SSH key**
5. **Title**：给密钥起个名字（如 "我的电脑"）
6. **Key**：粘贴公钥内容
7. 点击 **Add SSH key**

### 5. 测试连接

```bash
ssh -T git@github.com

# 如果看到：
# Hi 用户名! You've successfully authenticated, but GitHub does not provide shell access.
# 说明配置成功
```

## 创建和管理仓库

### 1. 在 GitHub 创建新仓库

1. 登录 GitHub
2. 点击右上角 **+** -> **New repository**
3. 填写仓库信息：
   - **Repository name**：仓库名称（必填）
   - **Description**：仓库描述（可选）
   - **Public/Private**：公开或私有
   - **Initialize with**：
     - ☑ Add a README file（推荐）
     - ☑ Add .gitignore（选择项目类型）
     - ☑ Choose a license（开源项目推荐）
4. 点击 **Create repository**

### 2. 克隆仓库到本地

```bash
# 使用 HTTPS
git clone https://github.com/用户名/仓库名.git

# 使用 SSH（推荐，需要先配置 SSH）
git clone git@github.com:用户名/仓库名.git

# 进入项目目录
cd 仓库名
```

**注意**：组织中的某些项目可能没有配置 SSH，这种情况下只能使用 HTTPS 方式克隆。

### 3. 推送本地项目到 GitHub

如果你已有本地项目，想上传到 GitHub：

```bash
# 在本地项目目录
cd 你的项目目录

# 初始化 Git 仓库
git init

# 添加所有文件
git add .

# 首次提交
git commit -m "首次提交"

# 添加远程仓库（先在 GitHub 创建空仓库）
git remote add origin git@github.com:用户名/仓库名.git

# 设置主分支名为 main
git branch -M main

# 推送到 GitHub
git push -u origin main
```

### 4. Fork 仓库

Fork 是复制别人的仓库到自己账号下：

1. 访问目标仓库
2. 点击右上角 **Fork** 按钮
3. 选择 Fork 到的账号/组织
4. 等待 Fork 完成

**Fork 后的操作：**

```bash
# 克隆你的 Fork
git clone git@github.com:你的用户名/仓库名.git

# 添加上游仓库（原始仓库）
git remote add upstream git@github.com:原作者/仓库名.git

# 查看远程仓库
git remote -v
# origin    你的 Fork
# upstream  原始仓库
```

### 5. 同步 Fork

保持你的 Fork 与原始仓库同步：

```bash
# 获取上游仓库的更新
git fetch upstream

# 切换到主分支
git checkout main

# 合并上游的更新
git merge upstream/main

# 推送到你的 Fork
git push origin main
```

## Pull Request 工作流程

Pull Request（PR）是 GitHub 协作的核心功能，用于向项目提交代码。

### 标准 PR 工作流程

#### 1. 准备工作

```bash
# 如果是别人的项目，先 Fork
# 然后克隆你的 Fork
git clone git@github.com:你的用户名/项目名.git
cd 项目名

# 如果是团队项目，直接克隆
git clone git@github.com:组织名/项目名.git
cd 项目名
```

#### 2. 创建新分支

```bash
# 确保在最新的 main 分支
git checkout main
git pull origin main

# 创建并切换到新分支
git checkout -b feature/添加用户登录功能

# 分支命名建议：
# feature/功能描述 - 新功能
# bugfix/问题描述 - 修复 bug
# hotfix/紧急修复 - 紧急修复
# docs/文档描述 - 文档更新
```

#### 3. 进行开发

```bash
# 编写代码...

# 查看修改
git status
git diff

# 添加修改
git add .

# 提交（遵循提交规范）
git commit -m "feat: 添加用户登录功能"
```

#### 4. 推送分支

```bash
# 第一次推送新分支
git push -u origin feature/添加用户登录功能

# 后续推送
git push
```

#### 5. 创建 Pull Request

在 GitHub 上创建 PR：

1. **访问仓库**，GitHub 会自动提示 "Compare & pull request"
2. 或手动：点击 **Pull requests** -> **New pull request**
3. 选择分支：
   - **base**：目标分支（通常是 `main` 或 `develop`）
   - **compare**：你的功能分支
4. **填写 PR 信息**：

```markdown
## 标题
简洁描述这个 PR 做了什么

## 描述
详细说明：
- 做了什么修改
- 为什么要做这些修改
- 如何测试

## 变更类型
- [ ] 新功能（feature）
- [ ] Bug 修复（bugfix）
- [ ] 文档更新（docs）
- [ ] 代码重构（refactor）
- [ ] 性能优化（performance）

## 测试
- [ ] 本地测试通过
- [ ] 添加了单元测试
- [ ] 更新了文档

## 截图（如果适用）
（添加截图或 GIF）

## 相关 Issue
Closes #123
```

5. **分配审查者**：在右侧 Reviewers 中选择审查人员
6. **添加标签**：选择合适的 Labels（如 `enhancement`、`bug`）
7. **关联项目**：如果有项目面板，关联到对应项目
8. **关联里程碑**：如果适用，关联到 Milestone
9. 点击 **Create pull request**

#### 6. 代码审查和修改

审查者会审查你的代码并提出意见：

**响应审查意见：**

```bash
# 根据反馈修改代码
# ... 修改文件 ...

# 提交修改
git add .
git commit -m "fix: 根据审查意见修改登录逻辑"

# 推送（PR 会自动更新）
git push
```

**处理审查评论：**
- 在 PR 页面回复评论
- 解决问题后，点击 "Resolve conversation"
- 如果需要更多讨论，继续在评论中交流

#### 7. 保持分支更新

如果主分支有新的提交：

```bash
# 方法一：Merge（保留完整历史）
git checkout main
git pull origin main
git checkout feature/你的分支
git merge main
git push

# 方法二：Rebase（保持线性历史，推荐）
git checkout feature/你的分支
git fetch origin
git rebase origin/main
git push -f  # 需要强制推送
```

#### 8. 合并 PR

审查通过后，维护者会合并 PR：

**合并方式：**

1. **Merge commit**
   - 保留所有提交历史
   - 创建一个合并提交
   - 适合：重要的功能分支

2. **Squash and merge**（推荐）
   - 将所有提交压缩为一个
   - 保持主分支历史清晰
   - 适合：多次小提交的功能

3. **Rebase and merge**
   - 将提交应用到基础分支
   - 保持线性历史
   - 适合：已经整理好的提交

#### 9. 清理分支

PR 合并后：

```bash
# 切换到主分支
git checkout main

# 拉取最新代码
git pull origin main

# 删除本地分支
git branch -d feature/你的分支

# 删除远程分支（如果没有自动删除）
git push origin --delete feature/你的分支
```

### Draft Pull Request

如果 PR 还未完成，可以创建草稿 PR：

1. 创建 PR 时，点击 **Create draft pull request**
2. 草稿 PR 不会请求审查
3. 完成后，点击 **Ready for review**

### PR 模板

创建 `.github/pull_request_template.md` 文件：

```markdown
## 变更描述
<!-- 简要描述此 PR 的目的 -->

## 变更类型
- [ ] 新功能（feature）
- [ ] Bug 修复（bug fix）
- [ ] 文档更新（documentation）
- [ ] 代码重构（refactoring）
- [ ] 性能优化（performance improvement）
- [ ] 测试（test）
- [ ] 构建/CI（build/ci）

## 相关 Issue
<!-- 关联相关的 Issue，如：Closes #123 -->

## 测试情况
- [ ] 本地测试通过
- [ ] 添加了新的测试用例
- [ ] 所有现有测试通过

## 检查清单
- [ ] 代码遵循项目规范
- [ ] 进行了自我审查
- [ ] 添加了必要的注释
- [ ] 更新了相关文档
- [ ] 没有产生新的警告

## 截图/录屏
<!-- 如果涉及 UI 变更，请添加截图或录屏 -->

## 额外说明
<!-- 其他需要说明的内容 -->
```

## Issue 问题管理

Issue 是 GitHub 的问题跟踪系统，用于管理 bug、功能请求、任务等。

### 1. 创建 Issue

1. 进入仓库，点击 **Issues** 标签
2. 点击 **New issue**
3. 填写 Issue 信息：

```markdown
## 标题
简洁描述问题或需求

## 问题描述
详细描述遇到的问题

## 复现步骤
1. 进入登录页面
2. 输入错误的密码
3. 点击登录按钮
4. 观察错误

## 期望行为
应该显示"密码错误"提示

## 实际行为
页面卡死无响应

## 环境信息
- 操作系统：Windows 11
- 浏览器：Chrome 120
- 版本：v1.2.3

## 截图
（如果适用，添加截图）

## 可能的解决方案
（如果有想法，可以提出）
```

4. 添加 **Labels**（标签）
5. 添加 **Assignees**（负责人）
6. 添加 **Projects**（项目面板）
7. 添加 **Milestone**（里程碑）
8. 点击 **Submit new issue**

### 2. Issue 标签（Labels）

使用标签对 Issue 进行分类：

**常用标签：**
- `bug` - Bug 报告
- `enhancement` - 功能增强
- `documentation` - 文档相关
- `question` - 问题咨询
- `help wanted` - 需要帮助
- `good first issue` - 适合新手
- `wontfix` - 不会修复
- `duplicate` - 重复问题
- `invalid` - 无效问题

**创建自定义标签：**

1. Issues -> Labels -> New label
2. 设置名称、描述、颜色
3. 点击 Create label

**标签最佳实践：**
- **类型**：`bug`, `feature`, `docs`
- **优先级**：`priority: high`, `priority: medium`, `priority: low`
- **状态**：`in progress`, `blocked`, `ready`
- **难度**：`difficulty: easy`, `difficulty: medium`, `difficulty: hard`

### 3. Issue 模板

创建 `.github/ISSUE_TEMPLATE/bug_report.md`：

```markdown
---
name: Bug 报告
about: 报告一个 bug 帮助我们改进
title: '[BUG] '
labels: bug
assignees: ''
---

## Bug 描述
简洁明了地描述这个 bug

## 复现步骤
1. 进入 '...'
2. 点击 '...'
3. 滚动到 '...'
4. 看到错误

## 期望行为
清楚简洁地描述你期望发生什么

## 实际行为
清楚简洁地描述实际发生了什么

## 截图
如果适用，添加截图帮助解释问题

## 环境信息
- 操作系统：[如 Windows 10]
- 浏览器：[如 Chrome 120]
- 版本：[如 v1.0.0]

## 附加信息
添加关于问题的任何其他信息
```

创建 `.github/ISSUE_TEMPLATE/feature_request.md`：

```markdown
---
name: 功能请求
about: 为项目提出新功能建议
title: '[FEATURE] '
labels: enhancement
assignees: ''
---

## 功能描述
清楚简洁地描述你想要的功能

## 问题背景
这个功能解决了什么问题？为什么需要？

## 建议的解决方案
描述你希望的实现方式

## 替代方案
描述你考虑过的其他解决方案

## 附加信息
添加关于功能请求的任何其他信息或截图
```

### 4. 管理 Issue

**分配 Issue：**
```markdown
<!-- 在 Issue 评论中 -->
@用户名 可以帮忙看一下这个问题吗？

<!-- 或在右侧 Assignees 中分配 -->
```

**关联 Issue：**
```markdown
<!-- 在 PR 或 commit 中 -->
Closes #123
Fixes #456
Resolves #789

<!-- 多个 Issue -->
Closes #123, closes #456
```

**引用 Issue：**
```markdown
<!-- 在评论或 PR 中引用 -->
参见 #123
和 #456 相关
```

**关闭 Issue：**
- 手动点击 "Close issue"
- 在 commit 中使用关键词（如 `Closes #123`）
- PR 合并时自动关闭

### 5. Issue 工作流程示例

**Bug 处理流程：**

1. **用户报告** Bug（创建 Issue）
2. **维护者确认**，添加 `bug` 标签
3. **评估优先级**，添加 `priority: high`
4. **分配开发者**，设置 Assignee
5. **开发者修复**，创建 PR，关联 Issue
6. **代码审查通过**，合并 PR
7. **Issue 自动关闭**

**功能请求流程：**

1. **用户提出**功能请求（创建 Issue）
2. **团队讨论**可行性
3. **决定实现**，添加 `enhancement` 标签
4. **规划里程碑**，添加到 Milestone
5. **开发实现**，创建 PR
6. **测试验证**
7. **发布新版本**，关闭 Issue

## Discussion 讨论区

Discussion 是 GitHub 的社区讨论功能，用于非正式交流。

### 1. 启用 Discussions

1. 进入仓库设置（Settings）
2. 找到 **Features** 部分
3. 勾选 **Discussions**
4. 点击 **Set up discussions**

### 2. Discussions 分类

**默认分类：**
- **Announcements** - 公告（只有维护者可以发布）
- **General** - 一般讨论
- **Ideas** - 想法和建议
- **Q&A** - 问答（可以标记最佳答案）
- **Show and tell** - 展示和分享

**创建自定义分类：**

1. Discussions -> Categories（右上角齿轮图标）
2. New category
3. 设置名称、描述、表情符号
4. 选择格式：
   - **Open-ended discussion** - 开放式讨论
   - **Question / Answer** - 问答（可选最佳答案）
   - **Announcement** - 公告
5. 创建分类

### 3. 创建 Discussion

1. 点击 **Discussions** 标签
2. 点击 **New discussion**
3. 选择分类
4. 填写标题和内容：

```markdown
## 关于新版本 UI 设计的讨论

大家好！我们正在规划下一个版本的 UI 改进，想听听大家的意见。

### 目前的问题
- 导航栏不够直观
- 颜色对比度不足
- 移动端适配不好

### 改进方向
1. 重新设计导航栏
2. 采用更现代的配色方案
3. 优化移动端体验

### 大家有什么建议吗？
请在下方留言分享你的想法！🎨
```

5. 点击 **Start discussion**

### 4. Discussion vs Issue 的区别

| 特性 | Discussion | Issue |
|------|-----------|-------|
| **用途** | 开放式讨论、交流想法 | 跟踪具体问题和任务 |
| **状态** | 无明确状态 | Open/Closed |
| **最佳答案** | 可以标记（Q&A 类型） | 无 |
| **分类** | 多种分类 | 使用标签 |
| **适用场景** | 功能建议讨论、技术交流 | Bug 报告、确定的任务 |

**何时使用：**
- **Discussion**：
  - 征求意见和反馈
  - 技术问题讨论
  - 功能提案的早期讨论
  - 社区交流
  
- **Issue**：
  - 明确的 Bug 报告
  - 已确定的功能需求
  - 具体的任务追踪
  - 需要关闭的事项

### 5. Discussion 最佳实践

**对于维护者：**
1. 定期发布公告（Announcements）
2. 及时回复讨论
3. 将有价值的讨论转化为 Issue
4. 标记有帮助的答案为最佳答案
5. 引导用户选择合适的分类

**对于贡献者：**
1. 在正确的分类发起讨论
2. 搜索是否有类似讨论
3. 描述清晰，提供足够上下文
4. 尊重社区准则
5. 如果问题已解决，标记最佳答案

## 代码审查（Code Review）

代码审查是保证代码质量的重要环节。

### 1. 审查 Pull Request

作为审查者：

1. **查看 PR 描述**，了解变更目的
2. **查看文件变更**：
   - 点击 **Files changed** 标签
   - 绿色（+）：新增代码
   - 红色（-）：删除代码

3. **逐行审查代码**：
   - 点击行号左侧的 **+** 添加评论
   - 可以选择多行评论
   - 提出具体的改进建议

4. **添加审查意见**：

```javascript
<!-- 建议性评论 -->
💡 这里可以使用更简洁的写法：
return items.filter(item => item.active);

<!-- 指出问题 -->
⚠️ 这里可能会导致空指针异常，建议添加空值检查

<!-- 表扬好的做法 -->
👍 这个抽象很好，提高了代码可读性
```

5. **提交审查**：
   - 点击 **Review changes**
   - 选择审查类型：
     - **Comment** - 仅评论
     - **Approve** - 批准合并
     - **Request changes** - 需要修改
   - 添加总结评论
   - 点击 **Submit review**

### 2. 审查清单

**代码质量：**
- [ ] 代码符合项目规范
- [ ] 命名清晰，易于理解
- [ ] 没有重复代码
- [ ] 适当的代码注释
- [ ] 没有遗留的调试代码

**功能：**
- [ ] 实现了预期功能
- [ ] 边界情况处理正确
- [ ] 错误处理完善
- [ ] 没有引入新的 bug

**测试：**
- [ ] 添加了相应的测试
- [ ] 所有测试通过
- [ ] 测试覆盖率足够

**性能：**
- [ ] 没有明显的性能问题
- [ ] 数据库查询优化
- [ ] 没有不必要的计算

**安全：**
- [ ] 没有安全漏洞
- [ ] 输入验证充分
- [ ] 敏感信息已保护

**文档：**
- [ ] 更新了相关文档
- [ ] API 文档完整
- [ ] 代码注释清晰

### 3. 审查建议

**给出建设性反馈：**

❌ **不好的反馈：**
```markdown
这代码写得太烂了
```

✅ **好的反馈：**

```markdown
这个函数有点长，建议拆分成几个小函数，每个函数只做一件事。
这样可以提高可读性和可测试性。

例如，可以将验证逻辑提取到 `validateInput()` 函数中。
```

**使用表情符号：**
- 👍 赞同、做得好
- 💡 建议
- ⚠️ 警告、需要注意
- ❓ 疑问
- 🔧 需要修复
- 📝 文档相关

**批量评论：**
- 如果有多处类似问题，先评论一处
- 在总结中说明"类似问题在其他地方也存在"
- 避免重复评论造成冗余

### 4. 响应审查意见

作为 PR 创建者：

1. **认真对待每条评论**
2. **及时回复和修改**
3. **讨论有争议的地方**
4. **修改后推送代码**（PR 自动更新）
5. **解决评论后标记**（Resolve conversation）
6. **感谢审查者的时间**

**回复示例：**

```markdown
<!-- 接受建议 -->
好的，我会按你说的重构这部分代码。

<!-- 讨论不同意见 -->
我理解你的担忧，但是这里使用 X 方案是因为 Y 原因。
你觉得这样是否合理？

<!-- 说明无法修改的原因 -->
这部分代码是为了兼容旧版本 API，暂时无法修改。
我在代码中添加了注释说明。
```

## 项目管理工具

### 1. Projects（项目面板）

GitHub Projects 提供看板式项目管理。

**创建项目：**

1. 点击仓库的 **Projects** 标签
2. 点击 **New project**
3. 选择模板：
   - **Board** - 看板视图（类似 Trello）
   - **Table** - 表格视图
   - **Roadmap** - 路线图视图
4. 填写项目名称和描述
5. 点击 **Create project**

**管理项目：**

1. **添加列**（Board 视图）：
   - Todo（待办）
   - In Progress（进行中）
   - In Review（审查中）
   - Done（完成）

2. **添加卡片**：
   - 手动创建：点击列底部的 **+ Add item**
   - 关联 Issue：拖拽 Issue 到项目
   - 关联 PR：拖拽 PR 到项目

3. **移动卡片**：
   - 拖拽卡片在列之间移动
   - 反映工作进展

**自动化：**
- Issue 创建时自动添加到项目
- PR 合并时自动移动到 Done
- Issue 关闭时自动归档

### 2. Milestones（里程碑）

里程碑用于跟踪特定版本或时间节点的进度。

**创建里程碑：**

1. Issues -> Milestones -> New milestone
2. 填写：
   - **Title**：如 "v1.0.0 发布"
   - **Due date**：截止日期
   - **Description**：里程碑描述
3. 点击 **Create milestone**

**使用里程碑：**

1. 在 Issue 或 PR 右侧选择 Milestone
2. 查看里程碑进度（完成百分比）
3. 跟踪哪些任务已完成，哪些待处理

### 3. Wiki

Wiki 用于编写项目文档。

**启用 Wiki：**

1. Settings -> Features
2. 勾选 **Wikis**

**创建 Wiki 页面：**

1. 点击 **Wiki** 标签
2. 点击 **Create the first page**
3. 编写内容（支持 Markdown）
4. 点击 **Save page**

**Wiki 结构示例：**
- Home - 项目介绍
- Installation - 安装指南
- Configuration - 配置说明
- API Reference - API 文档
- FAQ - 常见问题
- Contributing - 贡献指南

## 团队协作最佳实践

### 1. 分支策略

**Git Flow：**

- **main** - 生产环境代码
- **develop** - 开发分支
- **feature/** - 功能分支
- **release/** - 发布分支
- **hotfix/** - 紧急修复分支

**GitHub Flow（推荐，更简单）：**

- **main** - 生产环境代码（随时可部署）
- **feature/** - 功能分支（从 main 创建，合并回 main）

```bash
# GitHub Flow 工作流程
git checkout main
git pull origin main
git checkout -b feature/新功能
# ... 开发 ...
git add .
git commit -m "feat: 添加新功能"
git push -u origin feature/新功能
# 创建 PR，审查，合并
```

### 2. 提交规范

使用 Conventional Commits：

```bash
<type>(<scope>): <subject>

<body>

<footer>
```

**类型（type）：**
- `feat` - 新功能
- `fix` - Bug 修复
- `docs` - 文档更新
- `style` - 代码格式（不影响代码逻辑）
- `refactor` - 代码重构
- `perf` - 性能优化
- `test` - 测试相关
- `chore` - 构建/工具变动
- `ci` - CI 配置

**示例：**

```bash
feat(auth): 添加用户登录功能

- 实现登录 API
- 添加 JWT token 验证
- 更新用户界面

Closes #123
```

### 3. 代码审查规范

**审查频率：**
- 所有代码必须经过审查
- 至少需要 1 人批准（可在 Settings 配置）
- 重要功能需要 2 人以上审查

**审查时限：**
- 小型 PR（<200 行）：24 小时内
- 中型 PR（200-500 行）：48 小时内
- 大型 PR（>500 行）：应该拆分

**审查重点：**
1. 功能正确性
2. 代码质量
3. 测试覆盖
4. 文档完整性
5. 性能影响

### 4. 保护分支

保护重要分支不被误操作：

1. Settings -> Branches
2. Add rule
3. 选择分支（如 `main`）
4. 设置保护规则：
   - ☑ **Require a pull request before merging**
     - ☑ Require approvals（需要 N 个批准）
   - ☑ **Require status checks to pass**（需要 CI 通过）
   - ☑ **Require branches to be up to date**（需要最新代码）
   - ☑ **Require conversation resolution**（需要解决所有评论）
   - ☑ **Do not allow bypassing**（管理员也不能绕过）
5. Create

### 5. 团队协作流程

**日常开发流程：**

```bash
# 1. 同步最新代码
git checkout main
git pull origin main

# 2. 创建功能分支
git checkout -b feature/用户导出功能

# 3. 开发 + 提交
git add .
git commit -m "feat: 添加用户数据导出功能"

# 4. 推送分支
git push -u origin feature/用户导出功能

# 5. 创建 PR
# 在 GitHub 上创建 PR

# 6. 代码审查
# 等待团队审查，根据反馈修改

# 7. 合并代码
# PR 被批准后合并

# 8. 清理分支
git checkout main
git pull origin main
git branch -d feature/用户导出功能
```

**处理冲突：**

```bash
# 当 PR 显示有冲突时
git checkout feature/你的分支
git fetch origin
git merge origin/main

# 或使用 rebase（保持线性历史）
git rebase origin/main

# 解决冲突后
git add .
git rebase --continue  # 如果是 rebase
git commit            # 如果是 merge
git push -f           # rebase 需要强制推送
```

## GitHub Actions

GitHub Actions 是 GitHub 的 CI/CD 自动化工具。

### 1. 基本概念

- **Workflow** - 工作流程（一个 YAML 文件）
- **Job** - 作业（一组步骤）
- **Step** - 步骤（单个命令或动作）
- **Action** - 动作（可重用的步骤）
- **Runner** - 运行器（执行环境）

### 2. 创建 Workflow

创建 `.github/workflows/ci.yml`：

```yaml
name: CI

# 触发条件
on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

# 作业
jobs:
  test:
    runs-on: ubuntu-latest
    
    steps:
      # 检出代码
      - uses: actions/checkout@v3
      
      # 设置 Node.js
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
      
      # 安装依赖
      - name: Install dependencies
        run: npm ci
      
      # 运行测试
      - name: Run tests
        run: npm test
      
      # 运行 Lint
      - name: Run lint
        run: npm run lint
```

### 3. 常用 Workflows

**自动化测试：**

```yaml
name: Test

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [16, 18, 20]
    
    steps:
      - uses: actions/checkout@v3
      - name: Use Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v3
        with:
          node-version: ${{ matrix.node-version }}
      - run: npm ci
      - run: npm test
```

**自动部署：**

```yaml
name: Deploy

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Deploy to production
        run: |
          # 部署命令
          echo "Deploying..."
```

### 4. PR 状态检查

在 Actions 中配置检查后，PR 页面会显示状态：
- ✅ 所有检查通过 - 可以合并
- ❌ 检查失败 - 需要修复
- 🟡 检查进行中 - 等待完成

## 常见问题

### 1. clone 速度太慢

```bash
# 方法一：使用浅克隆
git clone --depth 1 https://github.com/用户名/仓库名.git

# 方法二：使用国内镜像（镜像网址可能会变动）
git clone https://gitclone.com/github.com/用户名/仓库名.git

# 方法三：直接在电脑上开代理（推荐）
```

### 2. push 时需要输入密码

**原因**：使用 HTTPS 方式，GitHub 已不支持密码验证。

**解决方法**：

1. **切换到 SSH**：
   ```bash
   # 查看当前远程仓库
   git remote -v
   
   # 修改为 SSH
   git remote set-url origin git@github.com:用户名/仓库名.git
   ```

2. **使用 Personal Access Token**：
   - GitHub Settings -> Developer settings -> Personal access tokens
   - Generate new token
   - 使用 token 代替密码

### 3. PR 显示大量无关文件变更

**原因**：换行符不一致（CRLF vs LF）

**解决方法**：

```bash
# 配置 Git
git config --global core.autocrlf true  # Windows
git config --global core.autocrlf input # macOS/Linux

# 在 .gitattributes 中统一规则
* text=auto
*.js text eol=lf
*.md text eol=lf
```

### 4. 如何处理大文件

使用 Git LFS（Large File Storage）：

```bash
# 安装 Git LFS
git lfs install

# 跟踪大文件
git lfs track "*.psd"
git lfs track "*.mp4"

# 提交 .gitattributes
git add .gitattributes
git commit -m "Add Git LFS configuration"

# 正常使用 git
git add 大文件.mp4
git commit -m "添加视频文件"
git push
```

### 5. 如何回滚已合并的 PR

```bash
# 方法一：使用 revert
git revert -m 1 <merge-commit-hash>
git push origin main

# 方法二：在 GitHub 上操作
# PR 页面 -> Revert -> Create pull request
```

### 6. 如何批量关闭 Issues

在 commit 或 PR 描述中：

```markdown
Closes #1, closes #2, closes #3
```

或使用脚本批量操作。

## 学习资源

- 📖 [GitHub 官方文档](https://docs.github.com/cn)（中文）
- 📺 [GitHub Skills](https://skills.github.com/)（交互式学习）
- 💡 [GitHub 最佳实践](https://github.com/github/platform-samples)
- 🎓 [GitHub 认证](https://examregistration.github.com/)（官方认证）
- 📝 [GitHub 博客](https://github.blog/)
- 👥 [GitHub Community](https://github.community/)（社区论坛）

## 总结

GitHub 是强大的团队协作平台，掌握以下核心功能可以极大提高团队效率：

**核心功能：**
1. **Pull Request** - 代码审查和合并工作流
2. **Issue** - 问题跟踪和任务管理
3. **Discussion** - 社区讨论和交流
4. **Projects** - 项目管理看板
5. **Actions** - 自动化 CI/CD

**协作要点：**
- 使用分支进行功能开发
- 通过 PR 提交代码
- 认真进行代码审查
- 及时处理 Issue
- 保持良好的沟通

**最佳实践：**
- 遵循提交规范
- 编写清晰的 PR 描述
- 及时响应审查意见
- 保护重要分支
- 使用自动化工具

---

🚀 **现在开始你的 GitHub 协作之旅吧！**

如有问题，欢迎在 Discussion 中讨论，或提交 Issue。Happy Collaborating! 🎉

