# vibe-code-utils

可复用的 AI 编码工作流工具集。跨 AI 助手通用,基于 [`AGENTS.md`](https://agents.md/) 开放约定。

不绑定具体工具——Claude Code / Cursor / Codex CLI / Aider 等任何认 `AGENTS.md` 约定的助手都能用。

## 这是什么

两类东西:

- **怎么干活(行为约束)**:[`AGENTS.md`](AGENTS.md) —— 给 AI 代理的统一行为约束。前半段是通用五原则(吸收自 Karpathy 对 LLM 写码常见缺陷的观察:想清再写 / 先求简 / 外科手术式改动 / 目标驱动 / 说人话),后半段是本仓库特定规则。
- **干什么(工作流)**:[`product-playbook`](.claude/skills/product-playbook/) skill —— 从一个想法到规模化运营的 18 阶段产品全流程陪跑。一次推进一步,每步落地产物、过关卡再继续,按项目规模裁剪。

## 怎么用

把 [`AGENTS.md`](AGENTS.md) 放进你的项目根目录,支持该约定的 AI 助手会自动读取并据此约束行为。

`product-playbook` 是一个 [Claude Code skill](https://docs.claude.com/en/docs/claude-code/skills):放在 `.claude/skills/` 下,聊产品相关的事时会自动触发。想在所有项目里用,把 `.claude/skills/product-playbook/` 拷到 `~/.claude/skills/`。用法详见 skill 自带的 [README](.claude/skills/product-playbook/README.md)。

## License

[MIT](LICENSE)
