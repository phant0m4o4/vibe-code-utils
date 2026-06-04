# vibe-code-utils

可复用的 AI 编码工作流工具集。跨 AI 助手通用,基于 [`AGENTS.md`](https://agents.md/) 开放约定。

不绑定具体工具——Claude Code / Cursor / Codex CLI / Aider 等任何认 `AGENTS.md` 约定的助手都能用。

## 这是什么

两类东西:

- **怎么干活(行为约束)**:[`AGENTS.md`](AGENTS.md) —— 给 AI 代理的统一行为约束。前半段是通用五原则(吸收自 Karpathy 对 LLM 写码常见缺陷的观察:想清再写 / 先求简 / 外科手术式改动 / 目标驱动 / 说人话),后半段是本仓库特定规则。
- **干什么(工作流)**:[`docs/workflow.md`](docs/workflow.md) —— 从点子到产品的 7 阶段工作流(锁需求 → MVP → 设计 → 搭骨架 → 迭代 → 验证加固 → 发布反馈),外加贯穿全程的协作原则(确认 gate、不瞎补默认值、决策权与执行权分离、卡住升级求助、状态可接力)。

## 怎么用

把 [`AGENTS.md`](AGENTS.md) 放进你的项目根目录,支持该约定的 AI 助手会自动读取并据此约束自己的行为;想跑完整的产品流程,就让助手照着 [`docs/workflow.md`](docs/workflow.md) 一阶段一阶段走。

## 文档导航

完整文档清单、阅读顺序与维护规则见 [`docs/index.md`](docs/index.md)。

## License

[MIT](LICENSE)
