# 写 AGENTS.md

遵循 [agents.md](https://agents.md/)：一个可预测的、专门给编码 agent 的上下文位置。

**它没有必填字段**——官方 FAQ 原话「Are there required fields? No. AGENTS.md is just standard Markdown」。以下小节是官方举例的**常见选择，全部可选**。

## 与 README 的分工

README 给人：quick start、项目描述。AGENTS.md 给 agent：那些「会让 README 变臃肿、但对 agent 必须」的东西。

**一旦 AGENTS.md 开始描述功能，它就会随功能腐化**，而 agent 每次都得读完。

## 官方举例的常见小节（择需）

```markdown
## Project overview          项目是什么（3 行以内）
## Build and test commands
## Code style                 agent 会照做的那几条，不是完整风格指南
## Testing instructions       跑哪个、怎么只跑一个、绿的标准
## Security considerations
```

官方补充：**commit message / PR 规范、安全陷阱、大数据集、部署步骤**——「任何你会告诉新同事的东西」。

## 本规范补的两节（官方未列，但 agent 必须知道）

```markdown
## 文档地图
| 想知道什么 | 看哪份文件 | 注意 |
|---|---|---|
| 现在必须成立的事实 | spec.md | 随代码变，改代码要同步 |
| 外部结构长什么样 | docs/reference/xxx.md | 有采集日期，过期要重采 |
| 过去是什么 | git log | 没有 archive 目录 |

## 硬约束（不可协商）
- <不能做的事 + 为什么>
- <必须做的事 + 判据>
```

**「文档地图」是关键增量**：agent 每次都是新会话，它不缺命令（缺了会报错），缺的是「真相在哪几个文件」。有这张表，它就不必盲读 300 行 spec。地图里列的路径必须真实存在——门禁第 8 道查这个。

## 铁律

- **每条都要可执行**：「代码要整洁」不可执行；「提交前跑 `npm run check`」可执行。
- **超过 100 行 = 在塞领土。** 该做的是把内容送进契约文档，把指针留在文档地图里。
- **不写**：实现细节、架构长文、变更历史、待办列表、教程。

## 嵌套与优先级（官方规则，monorepo 必读）

- 子项目目录可放**嵌套 AGENTS.md**；agent 读**目录树中最近的**那份；
- 冲突时：**离被编辑文件最近的胜**；用户对话里的明确指令覆盖一切；
- 单仓库不要预建嵌套文件——没有子项目就不建。

## 反面样例

```markdown
## 简介
本项目是一个浏览器端插件，提供设置页来隐藏宿主 UI 的各个角落，支持侧边栏、
右侧边栏、回答操作、账户菜单……（接下来 40 行功能描述）
```

这是 README 的内容。agent 读完 40 行，仍不知道「跑测试用哪条命令」「哪些文件不能动」——信息密度低，且随功能腐化。
