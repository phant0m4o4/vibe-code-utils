# vibe-code-utils

可复用的 AI 编码工作流工具集。跨 AI 助手通用,基于 [`AGENTS.md`](https://agents.md/) 开放约定。

## 内容

- [`prompts/`](prompts/) — 提示词模板
  - [`new-project.md`](prompts/new-project.md) — 从零启动(或接入已有)项目的完整托管模板,按 **脚本 / 小工具 / 单人产品 / 团队产品** 四档分级。覆盖领域文档生成、治理文档(AGENTS.md / PLAN.md)、ADR、CI/CD、迭代实施等全流程,内置走样防护(SoT、骨架优先、并行准入、反幻觉补全、求助退路、回滚约定),并吸收 [Karpathy 编码原则](https://github.com/forrestchang/andrej-karpathy-skills) + 边界与耦合约束作为代码级行为约束。

## 使用方法

### 1. 准备

- **AI 编码助手**:Claude Code / Cursor / Codex CLI / Aider 等任意支持 `AGENTS.md` 约定的工具
- **项目目录**:空目录(新建)或已有代码目录(自动进入接入模式)
- 在对应目录打开 AI 助手

### 2. 启动

打开 [`prompts/new-project.md`](prompts/new-project.md),把**分隔线下方**(`---` 之后的)所有内容复制,作为新对话**首条消息**发给 AI 助手。

### 3. 走流程

模板会驱动 agent 走三个阶段,每阶段都有用户确认 gate:

| 阶段 | 你做什么 | agent 做什么 |
|---|---|---|
| **0 前置 + 提问** | 回答 4 问:项目一句话定位 / 规模(脚本 / 小工具 / 单人产品 / 团队产品)/ 有无 UI / 是否对外 API | 检查目录、识别新建 vs 接入、列出对应规模档的文档清单请你确认 |
| **1 领域文档** | 审查每波文档生成后的 `[假设]` / `TBD` 列表,逐条确认或补全 | **默认串行**生成 `requirements.md` → `tech-stack.md` → `architecture.md` → 其他;关键字段未明示时停下追问,**不**自动用"行业通识"补全 |
| **2 治理文档** | 审查并确认 | 生成 `AGENTS.md`(项目宪法) + `PLAN.md`(进度跟踪) |
| **3 迭代实施** | 给出业务方向、回答求助、决定何时 `git push` | 按骨架优先推进:先 CI/CD 骨架 → 端到端最简路径 → 分模块加深;每步自动 commit、归档 PLAN.md,**push 永远手动** |

### 4. 后续

- 新 agent 接手时:让它读 `AGENTS.md` → `PLAN.md Current Step` → 关联 SoT 文档,几秒内对齐进度
- 重大决策变更:写 ADR (`docs/adr/YYYY-MM-DD-NN-标题.md`)
- 步骤卡 3 次:agent 自动升级"求助步骤",停下等你介入

## 使用效果

### 生成的项目骨架(档 B 示例)

```
project-root/
├── AGENTS.md                   # 治理宪法(≤80 行,索引 + 铁律 + 启动检查表)
├── PLAN.md                     # 进度跟踪(Changelog + Current Step + Next Step + 技术债)
├── docs/
│   ├── plans/                  # SoT 领域文档(唯一权威)
│   │   ├── README.md           # 单一索引入口
│   │   ├── requirements.md
│   │   ├── tech-stack.md
│   │   ├── architecture.md
│   │   ├── usage.md
│   │   ├── cicd.md
│   │   └── api.md              # 视情况
│   └── adr/                    # 决策记录(可选)
├── .gitignore
└── <你的代码 / 配置 / 测试...>
```

档 C(正式产品)额外增加 `feature-flags.md` / `observability.md` / `docs/snapshots/<tag>/` 等。

### 你会看到的行为差异

相比直接对 AI 说"帮我做个 X 项目",这套模板带来:

- 🚫 **不再瞎补默认值**:agent 不会自动填 JWT、80% 覆盖率、默认目录、典型字段——未明示就是 `TBD` 或 `> [假设] ...`,生成后集中列给你逐条审查
- 🧭 **新 agent 接力零摩擦**:`AGENTS.md` 控在百行内,`PLAN.md` 主体永远只显示当前一步,新会话读 3 个文件就跟上进度
- 🔁 **状态原子**:每步代码 + 文档 + `PLAN.md` 一次性提交,git 历史可读
- 🛡️ **push 是不可逆动作,永远手动**:agent 自动 commit 但**绝不**自动 push
- 🔍 **diff 可追溯**:每行代码改动能直接对应到 step 编号和用户请求(K3 精准修改)
- 🧱 **架构不腐化**:边界和耦合先想清楚,设计模式只在真需要时用(K5)
- 🆘 **不死磕**:同一步连续 3 次同类卡点自动升级"求助步骤",写明卡点等你介入,不烧 token
- ⚙️ **可对接已有项目**:已有代码目录自动进入接入模式,先盘点 `inventory.md` 再补齐缺失文档,不假装从零

### 适用规模

| 档 | 何时选 | 文档量 | 优化纳入 |
|---|---|---|---|
| **A 脚本** | 单人 < 1 周一次性活儿 | 2 份 | 无 |
| **B 小工具** | 长期维护、不上严肃生产 | 7 份基础 | A1 / B1-B3 |
| **C 单人产品** | 单人 / 小工作室 + 上生产 + 长期 | 10+ 份 + 强制字段 | 全部(A1-A4 / B1-B5 / C1-C5 / D1-D5) |
| **D 团队产品** | 多人 + 上生产 + 长期演进 | 档 C + 协作文档(`contributing.md` / `CODEOWNERS`) | 档 C 全部 + 协作约束(分支保护 / PR review / CODEOWNERS) |

## License

[MIT](LICENSE)
