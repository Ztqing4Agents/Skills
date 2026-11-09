---
name: dev-docs-standards
description: 开发者文档体系规范：规定 AGENTS.md / CHANGELOG.md / 契约 / 参考 / archive / MEMORY / WIP 工作现场各自装什么、长什么样、谁保证不腐化。当要新建或接管开发项目、决定某份内容该写进哪个文件、整理 docs、写 README / AGENTS.md / CHANGELOG、或发现 agent 记忆堆了大量本该在文档里的东西时使用。README 按 standard-readme 的形状写，其余文档写给仓库内部读者。
---

# 开发者文档体系（面向开发者与 AI agent）

**范围**：仓库的文档体系。面向内部读者的有 `AGENTS.md` / 契约 / 参考 / `CHANGELOG.md`；面向外部读者的只有 `README.md` 一份。其余对外文档（安装指南、营销页）仍不在此列——它们另有节奏，混进来会互相拖累。

**命题**：文档混乱不是「写得不够」，而是**同一种信息被写进多个容器**，或**某个容器不存在，于是全挤进了 agent 记忆**。

## 信息归位表（唯一路由入口）

| 你手上的东西 | 去哪 | 何时消失 |
|---|---|---|
| 现在必须成立的事实 | 契约：`spec.md`（1 份）/ `docs/contracts/`（≥2 份） | 改代码时同步改；不再成立就删 |
| 为什么这么定、放弃了什么 | commit message | 不消失（git 承载，`git log -S` 可查） |
| 还没做，且一句话说得清 | 任务板「待办」表 | 勾掉 = 换表 |
| **做完了，要能一眼查到** | 任务板「已完成」表（ID / 完成于 / 一件事 / commit） | 只增不删；细节归 commit message |
| **指路：结论在哪、依据是什么** | 就写在那条记录里（`docs/x.md §3.2` / `AGENTS.md 硬约束 N` / 短 sha / 用户原话(日期)） | 锚点失效即红；不写「见上文」 |
| **做到一半，跨会话还要读** | **`.agents/work/wip/<ID>-<slug>.md`** | **蒸馏后删** |
| **方案 / 草稿 / 分析 / 报告** | **`.agents/work/scratch/`** | **随时 `rm -rf`** |
| 对升级者的影响 | `CHANGELOG.md` | 追加，不改历史 |
| 这东西**是什么、值不值得用** | `README.md`（≤120 行，只写给使用者） | 功能变了就跟 |
| 外部长什么样 | 参考：`docs/reference/`（≥2 份放目录，1 份放根） | 重采时整刷 |
| 怎么干活的上下文 | `AGENTS.md`（≤100 行） | 变胖时拆到契约 |
| 我们试过什么 | 不落盘（**例外**：属于某个挂起任务时，写进那份 WIP 的 `## 决策`） | — |

**`work/` 两档的判据**：三天后回来继续这件事要读它吗？要 → `wip/`（删前必须蒸馏）；不要 → `scratch/`（读完全弃）。

**同一个事实只能有一个真源**：发现两处都写着，按「谁被 review」留——契约文档留正文，另一处改成指针。两份都留着，门禁全绿而文档长期打架。

## 阅读路由

| 你要做的事 | 打开 |
|---|---|
| 写 / 改 `README.md`（对外介绍） | [references/writing-readme.md](references/writing-readme.md) |
| 写 / 改 `AGENTS.md`（给 agent 的开工地图） | [references/writing-agents-md.md](references/writing-agents-md.md) |
| 发版、写 `CHANGELOG`、判断改动归哪一类 | [references/writing-changelog.md](references/writing-changelog.md) |
| 写契约 / 参考文档，决定建不建 `archive/` | [references/writing-contracts.md](references/writing-contracts.md) |
| 收尾时发现 MEMORY 膨胀 | [references/memory-decoupling.md](references/memory-decoupling.md) |
| 配门禁、定目录、判红还是告警 | [references/gates-and-layout.md](references/gates-and-layout.md) |
| 多任务并行、要挂起再回来、需要看板 | [references/parallel-work.md](references/parallel-work.md) |

## 六条铁律

1. **四类信息不许进同一个容器。** 过程不许留在**仓库**里——要落盘就进 `scratch/`；唯一必须落盘的过程是挂起的 WIP。
2. **AGENTS.md 是地图，不是领土。** 超过 100 行 = 有人在塞本该属于契约的内容。
3. **CHANGELOG 面向升级者，不是作者日记。** 记「你升级时要注意什么」，不记「我改了哪些文件」。
4. **MEMORY 装的是「不进 review 的东西」。** 一旦定型、会被别人读、改错要负责，它就该离开。
5. **完成的 WIP 必须删。** 不归档——价值蒸馏进 CHANGELOG / commit / 契约 / AGENTS，原件消失。唯一例外：改版/迁移前留一份**可弃快照**（放 `scratch/`、文件名带日期、注明确认后即删）——目的是证明迁移没丢内容，不是收藏。
6. **指路的东西必须能被门禁 grep 到。** 任务板的「依据」、文档地图、契约里的用例编号都是坐标不是散文；坐标能机械校验，散文不能——这是看板不腐化的唯一手段。

## 交付前自检

- [ ] 新建的文件归得上归位表里的哪一行？（归不上 → 不建）
- [ ] 新开一份文档，是否只因为「读者不同」或「原文件超档位」？其余情况应并入已有文件
- [ ] 用户可见变化进 CHANGELOG 了？破坏性变更单独列了？
- [ ] 有任务挂起吗？那份 WIP 的「下一步」具体到能直接执行吗？看板那条的「依据」能 grep 到吗？
- [ ] 有任务完成吗？三问蒸馏（用户可感知 / 新不变量 / 新坑）做了吗？WIP 删了吗？行换进「已完成」表、ID 没换号吗？
