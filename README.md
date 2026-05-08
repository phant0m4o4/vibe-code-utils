# vibe-code-utils

可复用的 AI 编码工作流工具集。跨 AI 助手通用,基于 [`AGENTS.md`](https://agents.md/) 开放约定。

## 内容

- [`prompts/`](prompts/) — 提示词模板
  - [`new-project.md`](prompts/new-project.md) — 从零启动(或接入已有)项目的完整托管模板,按 **脚本 / 小工具 / 正式产品** 三档分级。覆盖领域文档生成、治理文档(AGENTS.md / PLAN.md)、ADR、CI/CD、迭代实施等全流程,内置走样防护(SoT、walking skeleton、并行准入、求助退路、回滚约定)。

## 使用方式

打开你想用的 AI 编码助手(Claude Code / Cursor / Codex CLI / Aider 等),把对应模板**分隔线下方**的内容作为新对话的首条消息粘贴进去即可。

## License

[MIT](LICENSE)
