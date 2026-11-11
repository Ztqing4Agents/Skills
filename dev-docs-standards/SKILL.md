---
name: dev-docs-standards
description: 规定开发项目的文档体系：README / AGENTS / 契约 / 参考 / CHANGELOG / 看板与 WIP / MEMORY 各自装什么、长什么样、谁保证不腐化。用于新建或接管项目、决定某段内容该写进哪份文件、整理 docs、写 README / AGENTS / CHANGELOG、配置文档门禁，或发现 agent 记忆堆了本该进文档的东西。
slug: dev-docs-standards
displayName: 开发者文档体系规范
summary: 一套可执行的文档规范——AGENTS / 契约 / 参考 / CHANGELOG / 看板与 WIP 各归其位，附 7 份可填模板与 14 道可跑门禁。
version: "1.2.0"
author: Ztqing
license: MIT
homepage: https://github.com/Ztqing4Agents/Skills
tags: [documentation, agents, standards, lint]
---

# 开发者文档体系（写给开发者与 AI agent）

> **同一种信息只能有一个容器。** 容器缺位时它不会消失，只会掉进不被 review 的 agent 记忆。
> 读者：`README.md` 只对外（还没装的人）；`AGENTS.md`、契约、参考、`CHANGELOG.md` 对内。

## 什么时候不用

- 一次性脚本、没有第二个读者的个人仓库：一份 README 就够，不建 `docs/`、不建看板。
- 已有文档体系仍在维护：只按下面的归位表对账，不重排目录。
- 只写代码注释 / API 文档：不在本规范范围。

## 按任务开文件

**还没开始写 → 看这张表找细则；东西已经存在、只是想不好放哪 → 看下面那张归位表。**

| 要做的事 | 打开 |
|---|---|
| 盘点现有 `.md`、决定接管顺序 | 本文「接管一个仓库」 |
| 写 / 改 `README.md`（对外介绍） | [writing-readme.md](references/writing-readme.md) |
| 写 / 改 `AGENTS.md`（给 agent 的开工地图） | [writing-agents.md](references/writing-agents.md) |
| 发版、写 `CHANGELOG`、判断改动归哪类 | [writing-changelog.md](references/writing-changelog.md) |
| 写契约（现在必须成立的事实、类型与头注） | [writing-contracts.md](references/writing-contracts.md) |
| 写参考（外部结构：API / DOM / 第三方行为） | [writing-reference.md](references/writing-reference.md) |
| 决定目录怎么建、每份文档能多大 | [layout.md](references/layout.md) |
| 配门禁、判红还是告警、证明门禁不恒绿 | [gates.md](references/gates.md) |
| 多任务并行、挂起再回来、需要看板 | [board-and-wip.md](references/board-and-wip.md) |
| MEMORY 膨胀 | [memory-decoupling.md](references/memory-decoupling.md) |
| 收尾审计：文档有没有在悄悄变假 | [doc-rot-signals.md](references/doc-rot-signals.md) |
| 要一份能直接填的骨架 | [assets/](assets/)：7 份模板（README / AGENTS / CHANGELOG / spec / reference / BOARD / WIP） |
| 要跑门禁 | [scripts/check-docs.mjs](scripts/check-docs.mjs)：`node scripts/check-docs.mjs --root .`；用法见 [scripts/usage.md](scripts/usage.md) |

## 内容归位表（唯一判据）

| 手上的东西 | 容器 | 何时消失 |
|---|---|---|
| 现在必须成立的事实 | 契约：`spec.md`（1 份）/ `docs/contracts/`（≥ 2 份） | 改代码时同批改；不再成立就删 |
| 为什么这么定、放弃了什么 | commit message；产品级「不做」→ 契约的决策登记 | 不消失（`git log -S` 可查） |
| 还没做，一句话说得清 | `.agents/work/BOARD.md`「待办」表 | 勾掉 = 换表 |
| 做完了，要能一眼查到 | 看板「已完成」表，溢出进 `BOARD-archive.md` | 只增不删；细节归 commit message |
| 指路：结论在哪、依据是什么 | 就写在那一行里（`docs/x.md §3.2` / `AGENTS.md 硬约束 N` / 短 sha / 用户原话(日期)） | 坐标失效即红；不写「见上文」 |
| 做到一半，跨会话还要读 | `.agents/work/wip/<ID>-<slug>.md` | 蒸馏后删 |
| 方案 / 草稿 / 分析 / 报告 | `.agents/work/scratch/` | 随时 `rm -rf` |
| 对升级者的影响 | `CHANGELOG.md` | 追加，不改历史 |
| 这东西是什么、值不值得用 | `README.md` | 功能变了就跟 |
| 外部长什么样 | 参考：1 份放根 `reference.md`，≥ 2 份放 `docs/reference/` | 重采时整刷 |
| 怎么干活的上下文 | `AGENTS.md` | 变胖时拆到契约 |
| 我们试过什么 | 不落盘；属于某个挂起任务时写进该 WIP 的 `## 决策` | — |

两档判据：**三天后回来还要读它吗？** 要 → `wip/`（删前必须蒸馏）；不要 → `scratch/`。

## 接管一个仓库

- [ ] 1 盘点：`git ls-files '*.md'` + `git ls-files -o --exclude-standard '*.md'`，给每份标出读者
- [ ] 2 归位：按上表塞进唯一容器，删重复（两份都留 = 门禁全绿而文档长期打架）
- [ ] 3 整形：每份文件按自己的档位收身，契约与参考补齐头注——做完应能一眼看出每份「给谁读」
- [ ] 4 门禁：跑入口表里那条门禁命令，从红跑到绿（白名单 / 头注 / 指路坐标 / 引用 / 零行号）
- [ ] 5 看板：待办与完成分表，溢出进归档区——做完看板能回答「还剩什么没做」
- [ ] 6 审计：跑一遍腐化信号，把过程叙述移出仓库——做完库里只剩「现在成立的事实」

## 新建一个仓库：最小起点

只建三样：`README.md`、`AGENTS.md`、`.agents/work/BOARD.md`。`docs/` 等同类文件 ≥ 2 份再建——空目录会邀请人往里塞东西。

## 七条铁律

1. **同一个事实只留一个真源**：发现两处都写着，按「谁被 review」留正文，另一处改成指针。
2. **过程不进仓库**：要落盘就进 `scratch/`；唯一必须落盘的过程是挂起的 WIP。
3. **AGENTS.md 是地图，不是领土**：超过档位行数 = 有人在塞本该属于契约的内容。
4. **CHANGELOG 面向升级者**：记「你升级时要注意什么」，不记「我改了哪些文件」。
5. **MEMORY 装的是不进 review 的东西**：一旦定型、会被别人读、改错要负责，就该离开。
6. **完成的 WIP 必须删**：价值蒸馏进 CHANGELOG / commit / 契约 / AGENTS，原件消失。
7. **指路的坐标必须能被门禁 grep 到**：会变的数不手写，没主体的将来时不写。

## 交付前自检

- [ ] 新建的文件归得上归位表哪一行？归不上 → 不建
- [ ] 新开一份文档，只因为「读者不同」或「原文件超档位」？其余应并入已有文件
- [ ] 用户可见变化进 CHANGELOG 了？破坏性变更单独列了？
- [ ] 挂起的任务：WIP 的「下一步」能直接执行？看板「依据」能 grep 到？
- [ ] 完成的任务：三问蒸馏 + 删 WIP + 换表不换号（见 [board-and-wip.md](references/board-and-wip.md)）
- [ ] 新句子里有没有「会变的数」或「没主体的将来时」（见 [doc-rot-signals.md](references/doc-rot-signals.md)）
