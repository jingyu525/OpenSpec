# OpenSpec 使用指南

完整的 OpenSpec 使用教程，帮助你快速上手规范驱动开发。

## 目录

- [安装](#安装)
- [初始化项目](#初始化项目)
- [核心工作流程](#核心工作流程)
- [常用命令](#常用命令)
- [目录结构](#目录结构)
- [规范文件格式](#规范文件格式)
- [实际使用示例](#实际使用示例)
- [团队协作](#团队协作)
- [最佳实践](#最佳实践)
- [故障排除](#故障排除)

## 安装

### 前置条件

- **Node.js >= 20.19.0** - 使用 `node --version` 检查你的版本

### 全局安装 CLI

```bash
npm install -g @fission-ai/openspec@latest
```

验证安装：
```bash
openspec --version
```

## 初始化项目

### 步骤 1：进入项目目录

```bash
cd 你的项目路径
```

### 步骤 2：初始化 OpenSpec

```bash
openspec init
```

**初始化时会发生什么：**
- 提示你选择使用的 AI 工具（Cursor、Claude Code、Qoder 等）
- 自动配置对应工具的斜杠命令
- 创建 `openspec/` 目录结构
- 生成 `AGENTS.md` 指令文件
- 在项目根目录创建托管的 AI 指令文件

**初始化后：**
- 运行 `openspec list` 验证设置
- 重启 AI 工具以加载新的斜杠命令
- （可选）填充 `openspec/project.md` 定义项目约定

## 核心工作流程

OpenSpec 采用 **提案 → 审查 → 实现 → 归档** 的四阶段流程：

```
┌────────────────────┐
│ 1. 起草变更提案    │
└────────┬───────────┘
         │
         ▼
┌────────────────────┐
│ 2. 审查与对齐      │◄──── 反馈循环 ──────┐
└────────┬───────────┘                      │
         │                                  │
         ▼                                  │
┌────────────────────┐                      │
│ 3. 实现任务        │──────────────────────┘
└────────┬───────────┘
         │
         ▼
┌────────────────────┐
│ 4. 归档并更新规范  │
└────────────────────┘
```

### 阶段 1：创建变更提案

当你需要添加新功能或修改现有功能时，让 AI 助手创建提案：

**使用自然语言：**
```text
你对 AI 说：创建一个 OpenSpec 提案，添加用户登录功能
```

**使用快捷命令（支持的 AI 工具）：**
```text
/openspec:proposal 添加用户登录功能
```

AI 会自动创建以下结构：

```
openspec/changes/add-user-login/
├── proposal.md      # 为什么做、做什么、影响范围
├── tasks.md         # 实现任务清单
└── specs/           # 规范增量（新增/修改的需求）
    └── auth/
        └── spec.md
```

**何时创建提案：**
- ✅ 添加新功能或特性
- ✅ 进行破坏性变更（API、架构）
- ✅ 更改架构或设计模式
- ✅ 性能优化（改变行为）
- ✅ 更新安全模式

**何时跳过提案：**
- ❌ 错误修复（恢复预期行为）
- ❌ 拼写、格式、注释修改
- ❌ 非破坏性依赖更新
- ❌ 配置变更
- ❌ 为已有行为编写测试

### 阶段 2：验证和审查

检查提案是否正确创建并审查内容：

```bash
# 查看所有活动变更
openspec list

# 验证规范格式
openspec validate add-user-login

# 严格模式验证（推荐）
openspec validate add-user-login --strict

# 查看提案详情
openspec show add-user-login
```

**迭代完善规范：**

如果需要修改，继续与 AI 沟通：
```text
你对 AI 说：给登录功能添加双因素认证的场景
你对 AI 说：更新任务清单，添加单元测试任务
```

### 阶段 3：实现变更

提案确认后，开始实现：

**使用自然语言：**
```text
你对 AI 说：规范看起来不错，开始实现这个变更
```

**使用快捷命令：**
```text
/openspec:apply add-user-login
```

AI 会：
1. 读取 `openspec/changes/add-user-login/proposal.md` 了解目标
2. 查看 `tasks.md` 中的任务清单
3. 按顺序实现每个任务
4. 完成后标记 `- [x]` 完成状态

**实现过程中：**
- AI 会参考 `specs/` 中的现有规范
- 遵循 `openspec/project.md` 中定义的项目约定
- 实现完成后更新 `tasks.md` 中的复选框

### 阶段 4：归档变更

功能完成、测试通过并部署后，归档变更：

**命令行方式：**
```bash
openspec archive add-user-login --yes
```

**AI 助手方式：**
```text
你对 AI 说：归档 add-user-login 变更

# 或使用快捷命令
/openspec:archive add-user-login
```

**归档会做什么：**
- 将 `changes/add-user-login/` 移动到 `changes/archive/YYYY-MM-DD-add-user-login/`
- 将规范增量合并到 `specs/` 中的对应规范文件
- 更新 `specs/` 成为新的真实来源
- 运行验证确保归档后规范仍然有效

**特殊情况：**

仅工具变更（不涉及规范）：
```bash
openspec archive <change-id> --skip-specs --yes
```

## 常用命令

### 查看和列表

```bash
# 查看所有活动变更
openspec list

# 查看所有规范
openspec list --specs

# 长格式显示（更多详情）
openspec spec list --long

# JSON 格式输出（用于脚本）
openspec list --json
```

### 显示详情

```bash
# 显示变更详情
openspec show <变更名称>

# 显示规范详情
openspec show <规范名称> --type spec

# JSON 格式输出
openspec show <变更名称> --json

# 仅显示增量
openspec show <变更名称> --json --deltas-only
```

### 验证

```bash
# 验证特定变更
openspec validate <变更名称>

# 严格模式验证（推荐）
openspec validate <变更名称> --strict

# 批量验证（交互式）
openspec validate

# 验证特定规范
openspec validate <规范名称> --type spec
```

### 交互式工具

```bash
# 交互式仪表板
openspec view

# 交互式选择（show 命令）
openspec show

# 交互式选择（validate 命令）
openspec validate
```

### 项目管理

```bash
# 初始化 OpenSpec
openspec init [路径]

# 更新 AI 指令文件
openspec update [路径]

# 归档已完成的变更
openspec archive <变更名称> [--yes|-y]

# 归档时跳过规范更新
openspec archive <变更名称> --skip-specs --yes
```

### 命令标志

- `--json` - 机器可读的 JSON 输出
- `--type change|spec` - 消除项目类型歧义
- `--strict` - 全面验证（推荐）
- `--no-interactive` - 禁用交互式提示
- `--skip-specs` - 归档时不更新规范
- `--yes` / `-y` - 跳过确认提示（非交互式）
- `--long` - 显示更多详细信息

## 目录结构

```
你的项目/
├── openspec/
│   ├── project.md              # 项目约定、技术栈、架构模式
│   ├── AGENTS.md               # AI 助手的工作流程指令
│   ├── specs/                  # 当前真实规范（已实现的功能）
│   │   └── [capability]/       # 单一功能领域
│   │       ├── spec.md         # 需求和场景
│   │       └── design.md       # 技术模式（可选）
│   └── changes/                # 变更提案（待实现或进行中）
│       ├── [change-name]/      # 活动变更
│       │   ├── proposal.md     # 为什么、是什么、影响
│       │   ├── tasks.md        # 实现清单
│       │   ├── design.md       # 技术决策（可选）
│       │   └── specs/          # 增量变更
│       │       └── [capability]/
│       │           └── spec.md # ADDED/MODIFIED/REMOVED
│       └── archive/            # 已完成的历史变更
│           └── YYYY-MM-DD-[change-name]/
└── AGENTS.md                   # 根目录托管的 AI 指令（自动生成）
```

**关键概念：**
- **`specs/`** = 已经构建的内容（真实来源）
- **`changes/`** = 提议要改变的内容（待审批）
- **`archive/`** = 已完成的历史变更
- **每个 capability** = 单一聚焦的功能领域

## 规范文件格式

### 基本规范结构

```markdown
# 功能名称规范

## 目的
简要描述此功能的用途。

## 需求

### Requirement: 需求名称
系统必须/应该提供的功能描述。

#### Scenario: 场景名称
- **WHEN** 触发条件
- **THEN** 期望结果

#### Scenario: 另一个场景
- **WHEN** 其他条件
- **THEN** 其他结果
```

### 增量格式（用于变更）

增量是显示规范如何更改的"补丁"：

```markdown
## ADDED Requirements
### Requirement: 新功能
系统必须提供新的功能。

#### Scenario: 成功场景
- **WHEN** 用户执行操作
- **THEN** 系统返回预期结果

## MODIFIED Requirements
### Requirement: 现有功能名称
[完整的修改后需求内容，包括所有场景]

#### Scenario: 更新的场景
- **WHEN** 新的触发条件
- **THEN** 新的期望结果

## REMOVED Requirements
### Requirement: 废弃功能
**Reason**: 为什么移除此功能
**Migration**: 用户如何迁移到新方案

## RENAMED Requirements
- FROM: `### Requirement: 旧名称`
- TO: `### Requirement: 新名称`
```

### 格式要求

**必须遵守：**
- ✅ 使用 `### Requirement: <名称>` 作为需求标题（3 个 #）
- ✅ 使用 `#### Scenario: <名称>` 作为场景标题（4 个 #）
- ✅ 每个需求至少有一个场景
- ✅ 在需求文本中使用 SHALL/MUST 表示强制性
- ✅ 场景使用 `**WHEN**` 和 `**THEN**` 格式

**错误示例：**
```markdown
- **Scenario: 场景名称**  ❌ (不要用项目符号)
**Scenario**: 场景名称     ❌ (不要用粗体冒号)
### Scenario: 场景名称      ❌ (只能用 4 个 #)
```

### ADDED vs MODIFIED 的使用时机

**使用 ADDED：**
- 引入全新的、独立的功能
- 添加正交的（不影响现有功能的）新需求
- 例如：添加"双因素认证"到认证系统

**使用 MODIFIED：**
- 改变现有需求的行为、范围或验收标准
- 必须粘贴完整的、更新后的需求内容（包括标题和所有场景）
- 归档器会用你提供的内容替换整个需求
- 例如：更新"用户登录"需求以支持新的认证方式

**常见陷阱：**
使用 MODIFIED 添加新关注点但不包含之前的文本，这会导致归档时丢失详细信息。如果你不是明确改变现有需求，应该在 ADDED 下添加新需求。

**正确编写 MODIFIED 需求：**
1. 在 `openspec/specs/<capability>/spec.md` 中定位现有需求
2. 复制整个需求块（从 `### Requirement: ...` 到所有场景）
3. 粘贴到 `## MODIFIED Requirements` 下并编辑以反映新行为
4. 确保标题文本完全匹配（忽略空格）并保留至少一个 `#### Scenario:`

## 实际使用示例

### 示例 1：添加新功能

**场景：** 为电商系统添加商品搜索功能

```text
# 第 1 步：创建提案
你: /openspec:proposal 添加商品搜索功能

AI: 我将创建商品搜索的 OpenSpec 提案。
    [创建 openspec/changes/add-product-search/]

# 第 2 步：验证
你: openspec validate add-product-search --strict
你: openspec show add-product-search

# 第 3 步：完善（如果需要）
你: 添加按价格范围筛选的场景

AI: [更新规范增量]

# 第 4 步：实现
你: 规范确认，开始实现

AI: [实现 tasks.md 中的任务]
    ✓ 1.1 创建搜索 API 端点
    ✓ 1.2 实现全文搜索
    ✓ 1.3 添加价格筛选
    ✓ 1.4 编写单元测试

# 第 5 步：归档
你: openspec archive add-product-search --yes
```

### 示例 2：修改现有功能

**场景：** 更新用户权限检查逻辑

```text
# 第 1 步：创建修改提案
你: /openspec:proposal 更新用户权限检查逻辑

AI: [在 specs/auth/spec.md 下创建 MODIFIED Requirements]

# 第 2 步：审查修改内容
你: openspec show update-auth-permissions

# 第 3 步：实现
你: /openspec:apply update-auth-permissions

AI: [修改权限检查代码，更新测试]

# 第 4 步：归档
你: /openspec:archive update-auth-permissions
```

### 示例 3：跨多个规范的变更

**场景：** 添加双因素认证，影响认证和通知两个领域

```text
你: /openspec:proposal 添加双因素认证

AI: [创建以下结构]
    openspec/changes/add-2fa/
    ├── proposal.md
    ├── tasks.md
    └── specs/
        ├── auth/
        │   └── spec.md      # ADDED: 双因素认证需求
        └── notifications/
            └── spec.md      # ADDED: OTP 邮件通知需求

你: openspec validate add-2fa --strict
你: 看起来不错，开始实现

AI: [实现跨两个领域的功能]

你: openspec archive add-2fa --yes
    [归档器会更新 specs/auth/ 和 specs/notifications/]
```

## 团队协作

### 1. 初始化团队项目

```bash
# 项目维护者
cd team-project
openspec init
git add openspec/ AGENTS.md
git commit -m "chore: initialize OpenSpec"
git push
```

### 2. 团队成员加入

```bash
# 团队成员
git pull
openspec update  # 刷新 AI 指令

# 选择自己使用的 AI 工具配置
# 工具配置文件会自动生成
```

### 3. 统一规范，多样工具

**优势：**
- 团队成员可以使用不同的 AI 工具（Cursor、Claude Code、Qoder 等）
- 所有人共享同一套规范（`openspec/specs/`）
- 变更提案统一管理（`openspec/changes/`）

**工作流：**
```
开发者 A (使用 Cursor)
  ↓ 创建提案
openspec/changes/add-feature-x/
  ↓ 团队审查
开发者 B (使用 Claude Code)
  ↓ 实现功能
归档到 specs/
```

### 4. 代码审查最佳实践

审查时检查：
- ✅ `proposal.md` 清晰说明了变更原因和影响
- ✅ `tasks.md` 中的任务已全部完成并标记
- ✅ 规范增量格式正确（通过 `openspec validate --strict`）
- ✅ 每个需求都有足够的场景覆盖
- ✅ 代码实现符合规范要求

### 5. 切换 AI 工具

当团队成员切换工具时：

```bash
# 更新 AI 指令和斜杠命令
openspec update

# 重启 AI 工具以加载新配置
```

## 最佳实践

### ✅ 提案先行

- **重大功能先创建提案**：不要直接编码，先获得规范批准
- **明确影响范围**：在 `proposal.md` 中列出所有受影响的规范
- **早期反馈**：在实现前与团队讨论提案

### ✅ 验证习惯

```bash
# 每次修改规范后验证
openspec validate <change> --strict

# 归档前最后验证
openspec validate --strict
```

### ✅ 及时归档

- **功能部署后立即归档**：保持 `specs/` 是最新状态
- **定期清理**：不要让太多活动变更堆积在 `changes/`
- **归档即文档**：归档历史是系统演进的完整记录

### ✅ 清晰任务

`tasks.md` 要求：
- 具体可执行的任务描述
- 合理的任务粒度（不要太大也不要太小）
- 明确的完成标准
- 包含测试任务

示例：
```markdown
## 1. 数据库设置
- [ ] 1.1 向 users 表添加 otp_secret 列（VARCHAR(32)）
- [ ] 1.2 创建 otp_logs 表（user_id, timestamp, success）

## 2. 后端实现
- [ ] 2.1 实现 OTP 生成端点 POST /api/auth/otp/generate
- [ ] 2.2 修改登录流程，登录成功后要求 OTP 验证
- [ ] 2.3 实现 OTP 验证端点 POST /api/auth/otp/verify

## 3. 测试
- [ ] 3.1 单元测试：OTP 生成和验证逻辑
- [ ] 3.2 集成测试：完整登录 + OTP 流程
```

### ✅ 完整场景

每个需求至少包含：
- **正常场景**：功能按预期工作
- **异常场景**：错误处理、边界情况
- **边缘情况**：特殊条件下的行为

示例：
```markdown
### Requirement: 用户登录
系统必须支持用户通过邮箱和密码登录。

#### Scenario: 成功登录
- **WHEN** 用户提供有效的邮箱和密码
- **THEN** 返回 JWT 令牌和用户信息

#### Scenario: 无效凭证
- **WHEN** 用户提供无效的邮箱或密码
- **THEN** 返回 401 错误和错误消息

#### Scenario: 账户被锁定
- **WHEN** 用户账户已被管理员锁定
- **THEN** 返回 403 错误和账户锁定提示
```

### ✅ 简单优先

OpenSpec 倡导简单性：
- 默认 <100 行新代码的变更
- 单文件实现，直到证明不足
- 避免无明确理由的框架
- 选择成熟、经过验证的模式

**复杂性触发条件**（仅在以下情况添加复杂性）：
- 性能数据显示当前方案太慢
- 具体的规模需求（>1000 用户、>100MB 数据）
- 多个经过验证的用例需要抽象

### ✅ 清晰引用

- 使用 `file.ts:42` 格式标注代码位置
- 引用规范为 `specs/auth/spec.md`
- 链接相关变更和 PR

### ✅ 能力命名

- 使用动词-名词格式：`user-auth`、`payment-capture`
- 每个能力单一目的
- 10 分钟可理解原则
- 如果描述需要 "AND" 则拆分

### ✅ Change ID 命名

- 使用 kebab-case，简短且有描述性：`add-two-factor-auth`
- 优先使用动词前缀：`add-`、`update-`、`remove-`、`refactor-`
- 确保唯一性；如果已占用，追加 `-2`、`-3` 等

## 故障排除

### 问题：AI 不认识 OpenSpec 命令

**症状：**
- AI 说"我不知道 /openspec:proposal 是什么"
- 斜杠命令没有自动补全

**解决方案：**
1. **重启 AI 工具**：斜杠命令在启动时加载
2. **检查配置文件**：
   ```bash
   # 确认对应工具的配置文件存在
   ls -la .cursor/  # Cursor
   ls -la .qoder/   # Qoder
   ls -la .windsurf/ # Windsurf
   ```
3. **重新初始化**：
   ```bash
   openspec update
   ```
4. **使用自然语言替代**：
   ```text
   创建一个 OpenSpec 变更提案用于...
   ```

### 问题：验证失败 "Requirement must have at least one scenario"

**症状：**
```
Error: Requirement "用户登录" must have at least one scenario
```

**原因：**
场景格式不正确

**解决方案：**
确保使用正确的格式：
```markdown
✅ 正确：
#### Scenario: 场景名称

❌ 错误：
- **Scenario: 场景名称**
**Scenario**: 场景名称
### Scenario: 场景名称
```

### 问题：验证失败 "Change must have at least one delta"

**症状：**
```
Error: Change must have at least one delta
```

**原因：**
变更文件夹缺少规范增量文件

**解决方案：**
1. 检查 `changes/<名称>/specs/` 目录是否存在
2. 确保其中有 `.md` 文件
3. 验证文件有操作前缀（`## ADDED Requirements` 等）

示例：
```bash
# 检查结构
ls -R openspec/changes/add-feature/

# 应该看到：
openspec/changes/add-feature/
├── proposal.md
├── tasks.md
└── specs/
    └── feature-name/
        └── spec.md  # 必须存在
```

### 问题：场景解析静默失败

**症状：**
- `openspec show` 显示需求但没有场景
- 验证通过但场景缺失

**调试：**
```bash
# 使用 JSON 输出检查增量解析
openspec show <change> --json --deltas-only

# 检查特定需求
openspec show <spec> --json -r 1
```

**解决方案：**
确保精确格式：`#### Scenario: 名称`（4 个 # + 空格 + "Scenario:" + 空格 + 名称）

### 问题：归档后规范丢失内容

**症状：**
归档后 `specs/` 中的需求缺少之前的场景

**原因：**
使用 MODIFIED 但只粘贴了部分内容

**解决方案：**
编写 MODIFIED 需求时：
1. 从 `specs/<capability>/spec.md` 复制完整需求
2. 粘贴到变更的 `## MODIFIED Requirements` 下
3. 编辑内容以反映变更
4. 确保包含所有需要保留的场景

### 问题：多个变更冲突

**症状：**
两个变更修改同一个规范的同一个需求

**解决方案：**
1. 运行 `openspec list` 查看所有活动变更
2. 检查是否有重叠的规范
3. 与变更所有者协调
4. 考虑合并提案或调整变更顺序

### 问题：无法找到规范或变更

**症状：**
```
Error: Item not found
```

**调试：**
```bash
# 列出所有可用的
openspec list
openspec list --specs

# 检查拼写和命名
ls openspec/changes/
ls openspec/specs/
```

**解决方案：**
- 使用确切的名称（区分大小写）
- 使用 `--type change` 或 `--type spec` 消除歧义
- 对于归档的变更，包含日期前缀

---

## 快速参考卡

### 工作流速查

```
提案 → 验证 → 实现 → 归档

/openspec:proposal <功能>
openspec validate <change> --strict
/openspec:apply <change>
openspec archive <change> --yes
```

### 文件目的

- `proposal.md` - 为什么和是什么
- `tasks.md` - 实现步骤
- `design.md` - 技术决策
- `spec.md` - 需求和行为

### 阶段指示器

- `changes/` - 提议的，尚未构建
- `specs/` - 已构建和部署
- `archive/` - 已完成的变更

---

## 下一步

1. **尝试第一个变更**：在测试项目中创建一个简单的提案
2. **阅读示例**：查看 `openspec/changes/archive/` 中的已归档变更
3. **加入社区**：[OpenSpec Discord](https://discord.gg/YctCnvvshC) 获取帮助
4. **探索高级功能**：查看 `openspec/AGENTS.md` 了解更多工作流程细节

记住：**规范是真相。变更是提案。保持它们同步。**
