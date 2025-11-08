<p align="center">
  <a href="https://github.com/Fission-AI/OpenSpec">
    <picture>
      <source srcset="assets/openspec_pixel_dark.svg" media="(prefers-color-scheme: dark)">
      <source srcset="assets/openspec_pixel_light.svg" media="(prefers-color-scheme: light)">
      <img src="assets/openspec_pixel_light.svg" alt="OpenSpec logo" height="64">
    </picture>
  </a>
  
</p>
<p align="center">AI 编程助手的规范驱动开发。</p>
<p align="center">
  <a href="https://github.com/Fission-AI/OpenSpec/actions/workflows/ci.yml"><img alt="CI" src="https://github.com/Fission-AI/OpenSpec/actions/workflows/ci.yml/badge.svg" /></a>
  <a href="https://www.npmjs.com/package/@fission-ai/openspec"><img alt="npm version" src="https://img.shields.io/npm/v/@fission-ai/openspec?style=flat-square" /></a>
  <a href="https://nodejs.org/"><img alt="node version" src="https://img.shields.io/node/v/@fission-ai/openspec?style=flat-square" /></a>
  <a href="./LICENSE"><img alt="License: MIT" src="https://img.shields.io/badge/License-MIT-blue.svg?style=flat-square" /></a>
  <a href="https://conventionalcommits.org"><img alt="Conventional Commits" src="https://img.shields.io/badge/Conventional%20Commits-1.0.0-yellow.svg?style=flat-square" /></a>
  <a href="https://discord.gg/YctCnvvshC"><img alt="Discord" src="https://img.shields.io/badge/Discord-Join%20the%20community-5865F2?logo=discord&logoColor=white&style=flat-square" /></a>
</p>

<p align="center">
  <img src="assets/openspec_dashboard.png" alt="OpenSpec dashboard preview" width="90%">
</p>

<p align="center">
  Follow <a href="https://x.com/0xTab">@0xTab on X</a> for updates · Join the <a href="https://discord.gg/YctCnvvshC">OpenSpec Discord</a> for help and questions.
</p>

# OpenSpec

OpenSpec 通过规范驱动开发使人类和 AI 编程助手保持一致,确保在编写任何代码之前就达成共识。**无需 API 密钥。**

## 为什么选择 OpenSpec?

AI 编程助手功能强大,但当需求存在于聊天历史中时会变得不可预测。OpenSpec 添加了一个轻量级的规范工作流程,在实现之前锁定意图,为你提供确定性、可审查的输出。

主要成果:
- 人类和 AI 利益相关者在工作开始前就规范达成一致。
- 结构化的变更文件夹(提案、任务和规范更新)使范围明确且可审计。
- 对提议的、活跃的或已归档的内容具有共享可见性。
- 与你已经使用的 AI 工具配合使用:在支持的情况下使用自定义斜杠命令,在其他情况下使用上下文规则。

## OpenSpec 的比较优势(一览)

- **轻量级**:简单的工作流程,无需 API 密钥,最少的设置。
- **现有项目优先**:在 0→1 之后表现出色。OpenSpec 将真实来源与提案分开:`openspec/specs/`(当前真相)和 `openspec/changes/`(提议的更新)。这使得跨功能的差异保持明确和可管理。
- **变更跟踪**:提案、任务和规范增量共存;归档将批准的更新合并回规范。
- **与 spec-kit 和 Kiro 相比**:这些工具在全新功能(0→1)方面表现出色。OpenSpec 在修改现有行为(1→n)时也很出色,特别是当更新跨越多个规范时。

查看完整比较请参见 [OpenSpec 的比较优势](#openspec-的比较优势详解)。

## 工作原理

```
┌────────────────────┐
│ 起草变更           │
│ 提案               │
└────────┬───────────┘
         │ 与 AI 分享意图
         ▼
┌────────────────────┐
│ 审查与对齐         │
│ (编辑规范/任务)    │◀──── 反馈循环 ──────┐
└────────┬───────────┘                      │
         │ 批准的计划                        │
         ▼                                  │
┌────────────────────┐                      │
│ 实现任务           │──────────────────────┘
│ (AI 编写代码)      │
└────────┬───────────┘
         │ 发布变更
         ▼
┌────────────────────┐
│ 归档并更新         │
│ 规范(源)           │
└────────────────────┘

1. 起草一个捕获你想要的规范更新的变更提案。
2. 与你的 AI 助手一起审查提案,直到每个人都同意。
3. 实现引用已批准规范的任务。
4. 归档变更以将批准的更新合并回真实来源规范。
```

## 入门指南

### 支持的 AI 工具

#### 原生斜杠命令
这些工具具有内置的 OpenSpec 命令。在提示时选择 OpenSpec 集成。

| 工具 | 命令 |
|------|------|
| **Claude Code** | `/openspec:proposal`, `/openspec:apply`, `/openspec:archive` |
| **CodeBuddy Code (CLI)** | `/openspec:proposal`, `/openspec:apply`, `/openspec:archive` (`.codebuddy/commands/`) — 参见[文档](https://www.codebuddy.ai/cli) |
| **CoStrict** | `/openspec-proposal`, `/openspec-apply`, `/openspec-archive` (`.cospec/openspec/commands/`) — 参见[文档](https://costrict.ai)|
| **Cursor** | `/openspec-proposal`, `/openspec-apply`, `/openspec-archive` |
| **Cline** | `.clinerules/` 目录中的规则 (`.clinerules/openspec-*.md`) |
| **Crush** | `/openspec-proposal`, `/openspec-apply`, `/openspec-archive` (`.crush/commands/openspec/`) |
| **Factory Droid** | `/openspec-proposal`, `/openspec-apply`, `/openspec-archive` (`.factory/commands/`) |
| **OpenCode** | `/openspec-proposal`, `/openspec-apply`, `/openspec-archive` |
| **Kilo Code** | `/openspec-proposal.md`, `/openspec-apply.md`, `/openspec-archive.md` (`.kilocode/workflows/`) |
| **Qoder (CLI)** | `/openspec:proposal`, `/openspec:apply`, `/openspec:archive` (`.qoder/commands/openspec/`) — 参见[文档](https://qoder.com/cli) |
| **Windsurf** | `/openspec-proposal`, `/openspec-apply`, `/openspec-archive` (`.windsurf/workflows/`) |
| **Codex** | `/openspec-proposal`, `/openspec-apply`, `/openspec-archive` (全局: `~/.codex/prompts`, 自动安装) |
| **GitHub Copilot** | `/openspec-proposal`, `/openspec-apply`, `/openspec-archive` (`.github/prompts/`) |
| **Amazon Q Developer** | `@openspec-proposal`, `@openspec-apply`, `@openspec-archive` (`.amazonq/prompts/`) |
| **Auggie (Augment CLI)** | `/openspec-proposal`, `/openspec-apply`, `/openspec-archive` (`.augment/commands/`) |
| **Qwen Code** | `/openspec-proposal`, `/openspec-apply`, `/openspec-archive` (`.qwen/commands/`) |


Kilo Code 自动发现团队工作流程。将生成的文件保存在 `.kilocode/workflows/` 下,并通过命令面板使用 `/openspec-proposal.md`、`/openspec-apply.md` 或 `/openspec-archive.md` 触发它们。

#### AGENTS.md 兼容
这些工具自动从 `openspec/AGENTS.md` 读取工作流程指令。如果它们需要提醒,请让它们遵循 OpenSpec 工作流程。了解更多关于 [AGENTS.md 约定](https://agents.md/)。

| 工具 |
|------|
| Amp • Jules • Gemini CLI • 其他 |

### 安装与初始化

#### 前置条件
- **Node.js >= 20.19.0** - 使用 `node --version` 检查你的版本

#### 步骤 1:全局安装 CLI

```bash
npm install -g @fission-ai/openspec@latest
```

验证安装:
```bash
openspec --version
```

#### 步骤 2:在你的项目中初始化 OpenSpec

导航到你的项目目录:
```bash
cd my-project
```

运行初始化:
```bash
openspec init
```

**初始化期间发生的事情:**
- 系统会提示你选择任何原生支持的 AI 工具(Claude Code、CodeBuddy、Cursor、OpenCode、Qoder 等);其他助手总是依赖共享的 `AGENTS.md` 存根
- OpenSpec 自动为你选择的工具配置斜杠命令,并始终在项目根目录写入托管的 `AGENTS.md` 交接文件
- 在你的项目中创建一个新的 `openspec/` 目录结构

**设置后:**
- 主要的 AI 工具可以在没有额外配置的情况下触发 `/openspec` 工作流程
- 运行 `openspec list` 以验证设置并查看任何活动变更
- 如果你的编程助手没有立即显示新的斜杠命令,请重启它。斜杠命令在启动时加载,
  因此全新启动可确保它们出现

### 可选:填充项目上下文

在 `openspec init` 完成后,你将收到一个建议的提示,以帮助填充你的项目上下文:

```text
填充你的项目上下文:
"请阅读 openspec/project.md 并帮我填写有关我的项目、技术栈和约定的详细信息"
```

使用 `openspec/project.md` 定义项目级约定、标准、架构模式以及应在所有变更中遵循的其他指南。

### 创建你的第一个变更

这是一个显示完整 OpenSpec 工作流程的真实示例。这适用于任何 AI 工具。具有原生斜杠命令的工具会自动识别快捷方式。

#### 1. 起草提案
首先要求你的 AI 创建一个变更提案:

```text
你: 创建一个 OpenSpec 变更提案,用于按角色和团队添加个人资料搜索过滤器
    (具有斜杠命令的工具的快捷方式:/openspec:proposal 添加个人资料搜索过滤器)

AI: 我将为个人资料过滤器创建一个 OpenSpec 变更提案。
    *使用 proposal.md、tasks.md、规范增量搭建 openspec/changes/add-profile-filters/。*
```

#### 2. 验证与审查
检查变更是否正确创建并审查提案:

```bash
$ openspec list                             # 确认变更文件夹存在
$ openspec validate add-profile-filters     # 验证规范格式
$ openspec show add-profile-filters         # 审查提案、任务和规范增量
```

#### 3. 完善规范
迭代规范直到它们符合你的需求:

```text
你: 你能为角色和团队过滤器添加验收标准吗?

AI: 我将使用角色和团队过滤器的场景更新规范增量。
    *编辑 openspec/changes/add-profile-filters/specs/profile/spec.md 和 tasks.md。*
```

#### 4. 实现变更
一旦规范看起来不错,就开始实现:

```text
你: 规范看起来不错。让我们实现这个变更。
    (具有斜杠命令的工具的快捷方式:/openspec:apply add-profile-filters)

AI: 我将完成 add-profile-filters 变更中的任务。
    *从 openspec/changes/add-profile-filters/tasks.md 实现任务*
    *标记任务完成:任务 1.1 ✓,任务 1.2 ✓,任务 2.1 ✓...*
```

#### 5. 归档已完成的变更
实现完成后,归档变更:

```text
AI: 所有任务已完成。实现已准备就绪。

你: 请归档变更
    (具有斜杠命令的工具的快捷方式:/openspec:archive add-profile-filters)

AI: 我将归档 add-profile-filters 变更。
    *运行:openspec archive add-profile-filters --yes*
    ✓ 变更已成功归档。规范已更新。准备好进行下一个功能!
```

或者你自己在终端中运行命令:
```bash
$ openspec archive add-profile-filters --yes  # 归档已完成的变更,不提示
```

**注意:** 具有原生斜杠命令的工具(Claude Code、CodeBuddy、Cursor、Codex、Qoder)可以使用所示的快捷方式。所有其他工具使用自然语言请求来"创建 OpenSpec 提案"、"应用 OpenSpec 变更"或"归档变更"。

## 命令参考

```bash
openspec list               # 查看活动变更文件夹
openspec view               # 规范和变更的交互式仪表板
openspec show <change>      # 显示变更详情(提案、任务、规范更新)
openspec validate <change>  # 检查规范格式和结构
openspec archive <change> [--yes|-y]   # 将已完成的变更移动到 archive/(使用 --yes 为非交互式)
```

## 示例:AI 如何创建 OpenSpec 文件

当你要求你的 AI 助手"添加双因素认证"时,它会创建:

```
openspec/
├── specs/
│   └── auth/
│       └── spec.md           # 当前的认证规范(如果存在)
└── changes/
    └── add-2fa/              # AI 创建整个结构
        ├── proposal.md       # 为什么和改变什么
        ├── tasks.md          # 实现清单
        ├── design.md         # 技术决策(可选)
        └── specs/
            └── auth/
                └── spec.md   # 显示添加内容的增量
```

### AI 生成的规范(创建于 `openspec/specs/auth/spec.md`):

```markdown
# 认证规范

## 目的
认证和会话管理。

## 需求
### 需求:用户认证
系统应在成功登录时颁发 JWT。

#### 场景:有效凭证
- 当用户提交有效凭证时
- 则返回 JWT
```

### AI 生成的变更增量(创建于 `openspec/changes/add-2fa/specs/auth/spec.md`):

```markdown
# 认证增量

## ADDED Requirements
### 需求:双因素认证
系统必须在登录期间要求第二因素。

#### 场景:需要 OTP
- 当用户提交有效凭证时
- 则需要 OTP 挑战
```

### AI 生成的任务(创建于 `openspec/changes/add-2fa/tasks.md`):

```markdown
## 1. 数据库设置
- [ ] 1.1 向 users 表添加 OTP 密钥列
- [ ] 1.2 创建 OTP 验证日志表

## 2. 后端实现  
- [ ] 2.1 添加 OTP 生成端点
- [ ] 2.2 修改登录流程以要求 OTP
- [ ] 2.3 添加 OTP 验证端点

## 3. 前端更新
- [ ] 3.1 创建 OTP 输入组件
- [ ] 3.2 更新登录流程 UI
```

**重要:** 你不需要手动创建这些文件。你的 AI 助手会根据你的需求和现有代码库生成它们。

## 理解 OpenSpec 文件

### 增量格式

增量是显示规范如何更改的"补丁":

- **`## ADDED Requirements`** - 新功能
- **`## MODIFIED Requirements`** - 更改的行为(包括完整的更新文本)
- **`## REMOVED Requirements`** - 已弃用的功能

**格式要求:**
- 使用 `### Requirement: <name>` 作为标题
- 每个需求至少需要一个 `#### Scenario:` 块
- 在需求文本中使用 SHALL/MUST

## OpenSpec 的比较优势详解

### 与 spec-kit 的比较
OpenSpec 的双文件夹模型(`openspec/specs/` 用于当前真相,`openspec/changes/` 用于提议的更新)将状态和差异分开。当你修改现有功能或触及多个规范时,这会扩展。spec-kit 在绿地/0→1 方面很强大,但在跨规范更新和不断发展的功能方面提供的结构较少。

### 与 Kiro.dev 的比较
OpenSpec 将功能的每个变更分组在一个文件夹中(`openspec/changes/feature-name/`),使相关的规范、任务和设计易于一起跟踪。Kiro 将更新分散在多个规范文件夹中,这可能使功能跟踪更加困难。

### 与无规范的比较
没有规范,AI 编程助手从模糊的提示生成代码,经常遗漏需求或添加不需要的功能。OpenSpec 通过在编写任何代码之前就所需行为达成一致来带来可预测性。

## 团队采用

1. **初始化 OpenSpec** – 在你的存储库中运行 `openspec init`。
2. **从新功能开始** – 要求你的 AI 将即将开始的工作捕获为变更提案。
3. **逐步增长** – 每个变更都归档到记录你的系统的活规范中。
4. **保持灵活** – 不同的团队成员可以使用 Claude Code、CodeBuddy、Cursor 或任何与 AGENTS.md 兼容的工具,同时共享相同的规范。

每当有人切换工具时运行 `openspec update`,以便你的代理获取最新的指令和斜杠命令绑定。

## 更新 OpenSpec

1. **升级包**
   ```bash
   npm install -g @fission-ai/openspec@latest
   ```
2. **刷新代理指令**
   - 在每个项目中运行 `openspec update` 以重新生成 AI 指导并确保最新的斜杠命令激活。

## 贡献

- 安装依赖:`pnpm install`
- 构建:`pnpm run build`
- 测试:`pnpm test`
- 本地开发 CLI:`pnpm run dev` 或 `pnpm run dev:cli`
- 约定式提交(单行):`type(scope): subject`

## 许可证

MIT
