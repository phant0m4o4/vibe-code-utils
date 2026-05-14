# 示例

3 个端到端示例(不同档/形态)+ 生成文档样例。

---

## 示例 1:档 A 脚本 — bash 家目录备份

最小化场景,展示形态识别 + 完成下限触发 + 档 A 简化流程。

### 阶段 0

**用户首条消息**(在 `~/projects/home-backup/` 空目录):
> 把 `prompts/new-project.md` 分隔线下方内容粘贴 + "我要写个 bash 备份家目录的脚本"

**agent 检查目录**:空 → 新建流程

**agent 问 4 问**(一次问完):
1. 项目一句话定位?
2. 规模:脚本 / 小工具 / 单人产品 / 团队产品?
3. 是否有 UI/前端?
4. 是否对外提供 API?

**用户答**:`bash 备份家目录脚本` / `脚本` / `无` / `无`

**agent**:"我判断形态为**脚本**,如不对请纠正"
**用户**:"对"

**agent 列档 A 清单**:
- `docs/plans/README.md`(索引)
- `docs/plans/requirements.md`(20-50 行,含使用说明)
- `AGENTS.md` ≤30 行
- `PLAN.md` 可选
- 优化:**全不纳入**

**用户**:"确认"

### 阶段 1 波次 1:锁定 requirements.md

`git init` + `.gitignore` + zero-commit。

**agent 生成草稿**(脚本形态完成下限关键字段:触发方式 + 输入位置/格式 + 输出位置/格式):

```markdown
# Requirements

## 项目
bash 备份家目录脚本。

## 功能
- 备份家目录
  - TBD: 备份到哪(本地目录 / 外接磁盘 / 远程主机 / 云对象存储)
  - TBD: 触发方式(命令行手动 / cron 定时)
  - TBD: 排除规则(`.cache` / `node_modules` 等是否跳过)
```

**完成下限触发**(关键字段全 TBD,**停下追问**):

agent:"以下关键字段未明示,请补全后才能进下一步:
1. 备份到哪?
2. 触发方式?
3. 输入范围:整个 `~/` 还是排除某些目录?"

**用户答**:"备份到外接磁盘 `/Volumes/Backup/`;命令行手动调用;排除 `.cache`、`Library/Caches`、`node_modules`"

agent 更新 requirements,关键字段填实。档 A **跳过核对 gate**(脚本太简)。

### 阶段 2 治理文档

生成 `AGENTS.md`(≤30 行,只放定位 + 索引 + 全档通用铁律):

```markdown
# AGENTS.md

## 项目
bash 备份家目录脚本。

## 索引
- [requirements](docs/plans/requirements.md)
- [PLAN.md](PLAN.md)(若有)

## 铁律(全档通用)
- **push 永远手动**:禁自动 `git push`,禁 `git reset --hard` + force push
- **反幻觉**:未明示标 `TBD` / `> [假设]`;禁"主流""推荐""通常"等措辞
```

`PLAN.md` 用户选择不要(几小时活儿)。

### 阶段 3 迭代实施

档 A 简化(实施 → 自测 → commit),无 walking skeleton:

- Step 1:写 `backup.sh`(`rsync -a --exclude=...`)
- Step 2:`--dry-run` 验证
- Step 3:跑一次,看 `/Volumes/Backup/` 内容

agent 每步自动 commit,push 等用户明示。

---

## 示例 2:档 C 单人产品 — 个人记账 webapp(关键节选)

展示**形态识别 Web 全栈** + **完成下限多 TBD** + **核对 gate 5 步** + **冲突自检**。

### 阶段 0

**用户**:"做个人记账 webapp,收入支出分类,月报年报,多设备同步,要能离线"

agent 问 4 问 → 用户答:`单人产品` / 有 UI / 有 API

**agent**:"我判断形态为 **Web 全栈**(因为'多设备同步'需后端,'要能离线'又需前端 PWA),如不对请纠正"
**用户**:对

agent 列档 C 清单 → 用户确认 → 进阶段 1。

### 阶段 1 波次 1:锁定 requirements.md

#### 完成下限触发(Web 全栈关键字段:endpoint 列表 + 数据模型字段 + 页面列表 + 内容组织)

agent 列 9 个 TBD,**停下追问**。用户逐条补全:

- **endpoint**:`GET/POST/PUT/DELETE /transactions`、`GET /reports/monthly`、`GET /reports/yearly`、`POST /sync`
- **数据模型** Transaction:`id, amount, type(income/expense), category, date, note`
- **数据模型** Category:`id, name, parent_id, color`
- **页面列表**:Login / Dashboard / TransactionList / ReportMonthly / ReportYearly / Settings
- ...

#### [假设]/TBD 审查

agent 扫到 17 个标注 → 列给用户逐条 `确认 / 修改 / 补全`,全清空。

#### 核对 gate 5 步

**Step 1 完整性扫(Web 全栈清单)**:

agent 主动列常被遗漏的功能类,问用户每类需不需要:

| 类别 | 用户答 |
|---|---|
| 认证 / 授权 | 是 (P0) |
| 错误处理 | 是 (P0) |
| 数据导入导出(CSV) | 是 (P1) |
| 国际化 | 否 (单中文) |
| 监控 | 是 (P1) |
| 备份恢复 | 是 (P1) |
| 通知推送 | P2 → **档 C 砍掉** |
| 管理后台 | 否 |
| 离线 | 已确认 P0 |

→ catch 到用户没主动说的:**认证 + 错误处理 + CSV 导入导出 + 备份恢复**

**Step 2 优先级**:agent 把所有已确认功能标 P0/P1。

**Step 3 验收量化**:用户最初说"多设备同步要流畅" → agent **退回重写** → 改成"手机 A 添加交易 → 60 秒内手机 B 看到"。

**Step 4 冲突自检**:agent 扫到 ⚠️:

> **"完全离线" + "多设备同步"** 是经典 CRDT / 冲突解决问题:多设备并发离线编辑同一记录,联网后如何 merge?

候选 + trade-off:
- LWW(last-write-wins):简单 / 偶尔丢更新 / 适用低冲突场景
- CRDT:无丢失 / 实现复杂 / 适用频繁并发
- 手动解决:用户介入 / UX 差 / 适用关键数据

用户选 **LWW**(指出"个人记账冲突场景少")。

**Step 5 核心用户旅程**(交互形态用"旅程"措辞):

1. 注册 → 登录 → 添加首笔 → 看月报
2. 多设备同步:手机 A 添加 → 平板 B 60 秒内看到
3. 离线场景:断网添加 → 联网自动同步(LWW 解决冲突)

用户确认每条旅程 P0 覆盖完整 → **波次 1 完成,锁定 requirements**。

### 阶段 1 波次 2 - 4

- 波次 2:tech-stack(用户选 Vue + FastAPI + PostgreSQL + Redis)+ architecture
- 波次 3:api.md + ui.md + prototype.html + usage.md + cicd.md + feature-flags.md + observability.md(可并行,准入通过)
- 波次 4:docs/plans/README.md 索引

### 阶段 2 + 3

- AGENTS.md(≤120 行)+ PLAN.md 完整四段
- 阶段 3 第一步 CI/CD 骨架 → 第二步 walking skeleton(端到端登录→添加→看列表) → 第三步起在骨架上分模块加深

---

## 示例 3:接入模式 — 已有 Python FastAPI 项目

展示**接入子流程** + **inventory.md** + **已有内容引用而非重写**。

### 已有目录

```
~/projects/api-server/
├── main.py                  (FastAPI app,~500 行)
├── models.py                (SQLAlchemy 模型)
├── requirements.txt         (fastapi==0.110, sqlalchemy==2.0, pydantic==2.5)
├── README.md                (部署 + 简介,~80 行)
├── tests/                   (pytest)
├── .github/workflows/ci.yml (lint + pytest)
└── .git/
```

### 阶段 0 前置

agent 检查目录 → **已有代码** → **进接入子流程**。

agent 扫目录、读 `requirements.txt`、读 `README.md`、`git log -20`,产出 `docs/plans/inventory.md`:

```markdown
# Inventory

## 已有
- Python 3.11 + FastAPI 0.110 项目
- SQLAlchemy 2.0 + PostgreSQL(env var: `DATABASE_URL`)
- pytest 测试套件(覆盖率约 60%)

## 已有治理文档
- `README.md`(根目录) — 简介 + 部署说明(可在 usage.md 引用)

## 已有 CI/CD 配置
- `.github/workflows/ci.yml` — lint(ruff) + pytest

## 缺失(对照档 C 目标清单)
- 治理:`AGENTS.md` / `PLAN.md` / `docs/adr/`
- SoT:`requirements.md` / `tech-stack.md` / `architecture.md` / `usage.md` / `cicd.md`
- 档 C 强制:`feature-flags.md` / `observability.md` / `docs/snapshots/`

## 当前状态
- main 分支稳定
- feature/email-verify 分支 WIP
- 最近 commit:[2026-05-09] feat: 用户注册 endpoint
```

### 阶段 0 提问

用户选**档 C / 形态 Web 后端 API**。agent 列出"将**只生成 inventory 中标缺失**的文档,**不重写** `README.md`"→ 用户确认。

### 阶段 1 波次 1:锁定 requirements.md

agent 基于 **inventory 扫到的事实**填充(按接入模式规则,扫到的事实等同明示):

```markdown
# Requirements

## 项目定位
FastAPI 后端 API(从 main.py 推断,见 inventory)。

## 已有 endpoint(来源:main.py:42-180)
- POST /auth/register(用户注册)
- POST /auth/login(用户登录)
- GET /users/me(获取当前用户)
- ...

## 已有数据模型(来源:models.py)
- User(id, email, password_hash, created_at)
- ...

## 新增功能(用户口头补:邮箱验证)
- TBD: 邮箱验证流程细节
- TBD: 验证链接有效期
```

完成下限对**新增功能**触发停下追问。用户补全细节。

### 阶段 3 Step 1:补齐 CI/CD

对照 `cicd.md` 档 C 强制字段,**已有 ci.yml 缺**:
- smoke test post-deploy
- 安全扫描(SAST + 依赖漏洞)
- 覆盖率门槛(核心 ≥80%)
- 数据迁移强约束

agent 在 Step 1 增补这些到 `.github/workflows/`。

---

## 生成文档样例

### requirements.md(档 B,markdown→PDF CLI 工具节选)

```markdown
# Requirements

> 相关:[README](./README.md) · [tech-stack](./tech-stack.md) · [architecture](./architecture.md)

## 项目定位
markdown → PDF 的 CLI 工具。

## 子命令
- `md2pdf input.md` — 默认输出 `input.pdf`
  - **验收**:跑 `examples/sample.md`,输出文件存在且能用 PDF 查看器打开
- `md2pdf --help` — 显示帮助
  - **验收**:列出所有子命令 + 参数

## 参数 / Flag
- `-o, --output <path>` — 指定输出
- `-q, --quiet` — 静默(只输出错误)
- `--theme <name>` — **候选,等用户选**:`github` / `minimal` / 自定义 CSS

## 输入输出格式
- **输入**:CommonMark `.md` 文件
- **输出**:PDF v1.7
- **退出码**:`0` 成功 / `1` 输入文件不存在 / `2` 转换失败

## 边界(不做什么)
- 不支持 HTML 嵌入
- 不支持自定义字体
```

### AGENTS.md(档 C,~100 行)

```markdown
# AGENTS.md

> 个人记账 webapp 的治理宪法。

## 项目定位
个人记账 webapp,收入支出分类 + 月报年报 + 多设备同步 + 离线。

## 文档索引
- SoT 文档:[docs/plans/README.md](docs/plans/README.md)
- 进度:[PLAN.md](PLAN.md)
- 决策记录:[docs/adr/](docs/adr/)
- 里程碑快照:[docs/snapshots/](docs/snapshots/)

## 铁律

### 全档通用
- **push 永远手动**:禁自动 `git push`,禁 `git reset --hard` + force push
- **反幻觉**:未明示标 `TBD` / `> [假设] ...`;禁自动补全;禁"主流""推荐""通常"等措辞

### 档 C 必含
- **密钥不进代码 / 日志 / commit / LLM 上下文**,统一 secret store
- **依赖白名单**:`tech-stack.md` 外的依赖须先写 ADR
- **禁止编造**:SoT 外信息必须问用户,禁"通常""一般""我假设"

## 启动检查表(新 agent 必走,A1)
1. `git status` 看当前状态
2. `git log --oneline -20` 看最近进度
3. 读本文(AGENTS.md)
4. 读 [PLAN.md Current Step](PLAN.md)
5. 读 Current Step 关联的 SoT 文档
6. 若 AGENTS.md/PLAN.md 不存在(项目阶段 1 中途 abort):扫 `docs/plans/` 现有对比清单从中断处续

## commit 消息格式
`<type>(<scope>): <summary> [step-NN]`
例:`feat(api): 用户登录 [step-04]`

## 风险分级 review
- **跳过询问**:纯新增 / 文案 / 配置
- **必问**:核心模块 / 破坏性 / 数据迁移 / 安全
- **建议问**:跨模块 / 新依赖

## 测试策略
- 数据模型 → 单测
- API → 集成测试
- UI → 人工或快照
- 纯文档 → 无需

## 决策变更走 ADR
- 命名:`docs/adr/YYYY-MM-DD-NN-标题.md`
- 内容:从 A 改到 B 的理由 + 影响范围
```

### PLAN.md(档 C 当前正在 Step 05)

```markdown
# PLAN.md

## Changelog
- [2026-05-10] Step 01: 初始化 + CI/CD 骨架 ✅
- [2026-05-11] Step 02: walking skeleton 端到端最简路径(登录→添加→列表) ✅
- [2026-05-11] Step 03: 用户注册 + 邮箱验证 ✅
- [2026-05-12] Step 04: JWT 登录 + 多设备 token 管理 ✅

## Current Step
- 编号:Step 05
- 标题:交易增删查改 + 分类管理
- 描述:实现 `POST/GET/PUT/DELETE /transactions` 完整 CRUD,含分类树
- 任务粒度:最小可工作切片 —— 先做 `Transaction` 模型 + POST + GET 两个 endpoint,PUT/DELETE 留 Step 06
- 测试策略:单测(model 验证)+ 集成(API 端到端)
- 关键文件:`models/transaction.py`、`api/transactions.py`、`tests/test_transactions.py`
- 已知风险:多设备同步冲突解决留到 Step 09(LWW 策略),本步只单设备 CRUD
- 验证产物:`tests/output/step-05.txt`(pytest 输出)+ CI run #142 + 截图 `docs/snapshots/step-05-list.png`

## Next Step
- 编号:Step 06
- 标题:交易 PUT/DELETE + 软删除

## 遗留事项 / 技术债
- [2026-05-11] [medium] 多设备同步 LWW 策略简单,无 P99 延迟实测 — `services/sync.py` — Step 09 之前压测
- [2026-05-12] [low] JWT 过期时间硬编码 7 天 — `api/auth.py:23` — Step 08 抽配置
```

### ADR 样例(`docs/adr/2026-05-11-01-改用 SQLite-WAL.md`)

```markdown
# 2026-05-11-01:多设备同步从 PostgreSQL 改用 SQLite + WAL

## 背景
最初 tech-stack 选 PostgreSQL,但用户作为 indie 单人产品,运维 Postgres 成本高,且用户基数小(<1k MAU 预期)。

## 决策
本地缓存 SQLite + WAL 模式,服务端用 Cloudflare D1(基于 SQLite)做主存。

## 影响
- 影响 `architecture.md` 数据存储部分 —— 同步更新
- 影响 `cicd.md` 数据迁移流程 —— 同步更新
- 不影响 endpoint 接口
- 减少运维成本 ~80%
- 限制:写并发不如 Postgres,但单人产品场景不会触及上限

## 回滚路径
若用户量超过 D1 限制(理论 100k 用户/库),迁回 Postgres,迁移脚本备份在 `migrations/sqlite-to-pg.sql`。
```

---

更多场景:阶段 0 形态识别困难?用户中途加需求?求助步骤升级?**模板都有处理流程**——参阅 [`../prompts/new-project.md`](../prompts/new-project.md) 完整规则。
