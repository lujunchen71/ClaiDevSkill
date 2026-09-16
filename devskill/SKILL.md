---
name: dev-flow
description: 需求→方案→定版→计划→执行→日志→评审→清理的开发流程规范。任何项目通用。AI 读完即知开发节奏与目录约定；纪律与日志协议在 DISCIPLINE.md / WORKLOG-PROTOCOL.md（随 skill 分发，逐字拷入项目，禁止现场改写）。用于新需求立项、跨机/跨人/跨模型接手、多版本文档治理。
---

# 开发流程规范（dev-flow）

> 本文件只说**流程与结构**：先做什么后做什么、什么目录放什么、文件从哪来。
> 具体纪律条款在 `DISCIPLINE.md`——它是独立文件，**逐字拷入项目**生效，禁止在 skill 上或项目里现场改写。

## 1. 全流程总览（严格按序）

```
① 需求与方案 → ② 多轮人工评审(v1..vN) → ③ 定版开工(拷纪律+写 plan.md) → ④ 执行+worklog → ⑤ 阶段评审(review/) → ⑥ 稳定后清理
```

| 阶段 | 动作 | 产物 |
|---|---|---|
| ① 需求 | 写需求与方案文档：现象、目标、方案与取舍、不做什么、验收口径 | `v1/NN-*.md` |
| ② 评审 | 人工多轮评审，每轮定稿升一版；最新版=唯一基线 | `v2/ … vN/` |
| ③ 定版 | 从本 skill **逐字拷贝** `DISCIPLINE.md` 与 `WORKLOG-PROTOCOL.md` 进项目；需求拆成可执行计划 | `vN/DISCIPLINE.md`、`vN/WORKLOG-PROTOCOL.md`、`vN/plan.md` |
| ④ 执行 | 按 plan 执行，做一个勾一个，每步记 worklog | 代码 + `vN/worklog/` |
| ⑤ 评审 | 阶段成果 review 入专门目录；结论回灌走 ①→③→④ | `vN/review/` |
| ⑥ 清理 | 功能稳定后删旧版计划（用户确认）；需求文档/worklog/边界/评审永留 | — |

**plan.md 存在才开始记 worklog。**

## 2. 目录结构约定

```
docs/<领域>/
  v1/ … vN/                  — 版本化需求线目录；定稿一版开一版，AI 只读最新版
  vN/
    00-README.md             — 总览：自包含背景、目标、文档地图、红线索引
    NN-<主题>.md             — 需求/设计文档（编号排序；不管执行项）
    DISCIPLINE.md            — 纪律（本 skill 逐字拷贝，禁止改写）
    WORKLOG-PROTOCOL.md      — 工作日志协议（本 skill 逐字拷贝，禁止改写）
    plan.md                  — 唯一执行计划：PX 执行单元（P0..PN）+ 逐项 checkbox
    worklog/                 — 工作日志：<平台>-<机器名>-<写作者>.md，每人/机/模型一个文件
    review/                  — 评审记录：谁评审、结论、证据
    BOUNDARY.md              — 边界文件：能做/不能做/反复修改史
```

多领域并行按 `<领域>/` 分列（如 `docs/pi/v3/`、`docs/architecture/v4/`）。

## 3. 三个流转规则

- **新需求进入**：先写需求文档并评审定版 → 查当前 plan：相关→插入现有 PX；无关→新起 PX → 进 plan 才允许写码。
- **评审结论回灌**：review 结论 → 先整理为需求/现象文档 → 拆成 plan 条目 → 执行。禁止从 review 直接改码。
- **边界登记**：用户明确允许/禁止、同一问题改多次、反复纠正的偏好 → 当日登记 `BOUNDARY.md`；任何人/机/模型接手先读它再行动。
- **施工权唯一（并行防线）**：多会话/多机并行同一 worktree 时，一个板块同一时刻只许一条施工线——开工前在 plan.md 顶部插旗（施工者/分支/时间，随提交入库），收工拔旗；已有旗的板块禁止并行双写，先确认归属（细则见 DISCIPLINE.md §3-5）。

## 4. 接手协议（新机器/新人/新模型）

严格按序，读完再动手：
1. `git pull`（真值只在 git，未提交内容视为不存在）。
2. `BOUNDARY.md` → `00-README.md` → `WORKLOG-PROTOCOL.md` §3 恢复流程（锚定进行中任务 → checkbox↔worklog 对账 → 核验 commit 可达+重跑验证命令）→ AI 先复述再动手。

## 5. 采用方法（新项目接入）

1. 建 `docs/<领域>/v1/`，写第一批需求文档。
2. **逐字拷贝** `DISCIPLINE.md` 与 `WORKLOG-PROTOCOL.md` 到 `vN/`；项目特有规则写进项目自己的 `BOUNDARY.md`，禁止改进这两个文件。
3. **逐字拷贝** `templates/` 五个文件建对应文档（需求文档、`plan.md`、`worklog/<平台>-<机器名>-<写作者>.md`、`review/`、`BOUNDARY.md`）。**AI 禁止现场发挥新写这些文件**——格式漂移即流程失效。
4. **同一 commit 在 `docs/REGISTRY.md` 登记一行**（路径：干什么，≤100 字）；板块范围显著扩大时同 commit 修订该行。漏登/漏改 = 违规（AGENTS.md 红线引用）。
5. 进入 §1 阶段④循环。

## 6. 临界规则

- skill 只说流程；纪律只看 `DISCIPLINE.md`；日志规则只看 `WORKLOG-PROTOCOL.md`；模板只逐字拷贝。
- 禁止无需求文档开工、无 plan 项写码、做完补勾、review 散落、边界不登记、日志只留本地。
