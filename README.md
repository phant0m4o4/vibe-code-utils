# vibe-code-utils

可复用的 AI 编码工作流工具集。跨 AI 助手通用,基于 [`AGENTS.md`](https://agents.md/) 开放约定。

## 内容

- [`prompts/`](prompts/) — 提示词模板
  - [`new-project.md`](prompts/new-project.md) — 从零启动(或接入已有)项目的完整托管模板,按 **脚本 / 小工具 / 单人产品 / 团队产品** 四档分级。覆盖领域文档生成、治理文档(AGENTS.md / PLAN.md)、ADR、CI/CD、迭代实施等全流程,内置走样防护(SoT、骨架优先、并行准入、反幻觉补全、求助退路、回滚约定),并吸收 [Karpathy 编码原则](https://github.com/forrestchang/andrej-karpathy-skills) + 边界与耦合约束作为代码级行为约束。

详细**使用方法 + 使用效果**见 [`docs/README.md`](docs/README.md)。

## License

[MIT](LICENSE)
