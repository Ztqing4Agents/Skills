# 写 CHANGELOG.md

遵循 [Keep a Changelog 1.1.0](https://keepachangelog.com/en/1.1.0/)。**官方没有强制格式**，以下是它推荐的做法 + 本规范的三条增补（会标注）。

## 结构

```markdown
# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [1.1.0] - 2024-09-27

### Added

- 用户能感知到什么（不是改了哪些文件）。
```

**其余五类在有内容时再加；没有内容的类别整段不写**（照抄上面的模板去补空标题，正好违反下文的 Bad Practices 4）。

## 六类语义（官方，不要自创）

| 类别 | 用于 |
|---|---|
| `Added` | 新功能 |
| `Changed` | 既有功能的变化 |
| `Deprecated` | 即将移除的功能 |
| `Removed` | 已移除的功能 |
| `Fixed` | bug 修复 |
| `Security` | 安全相关问题 |

## 官方七条 Guiding Principles

1. Changelog 是给人看的，不是给机器。
2. **每个版本都要有条目。**
3. **同类变更分组。**
4. **版本与 section 可链接。**
5. **最新版本在最上面。**
6. **每个版本显示发布日。**
7. **声明是否遵循 SemVer。**

## 官方 Bad Practices（四条，最常踩）

1. **不要把 commit log / diff 倒进来**——merge commit、晦涩标题、文档改动全是噪音。
2. **不要忽略 deprecation**——官方原话：「别的都不做，至少列出 deprecations、removals 和 breaking changes。」
3. **日期用 ISO 8601**（`2024-09-27`），跨区域无歧义。
4. **不要不一致**——只记一部分变更「**和**没有 changelog **一样危险**」（官方原话 *can be as dangerous as*）——读者会误以为它是真相源。

**关于空 section**：官方 0.0.4 版删掉空 section，理由是「缺失的 section 会被认为**是有意省略**」，而留着空标题只会制造噪音。所以：**不写空类别**。

**撤销的版本**：`## [0.0.5] - 2014-12-13 [YANKED]`——方括号让它在正文里显眼，也便于解析。

## 本规范增补四条

1. **CHANGELOG 是真相源，tag message 只写一句指针**（如「见 CHANGELOG.md」）。官方没规定 tag 内容，这是本规范的选择——理由是两份独立维护必然分叉。
2. **文件名必须叫 `CHANGELOG.md`**（不要 `HISTORY` / `NEWS` / `RELEASES`）：读者找不到就等于没有。
3. **项目还没有任何版本号 / tag 时不要建它**——git log 已经够，而这个文件会变成没人维护的摆设。
4. **哪些变更不该记**（这是 "notable" 的定义；缺了它，上面的 Bad Practices 4 会被读成「所有变更都要记」）：
   - **只有迫使使用者改变行为或预期的才记**：新增 / 删除 / 改名 / 行为变化 / 需要迁移的配置。门禁数量、用例条数、内部重构、目录搬迁、文档补写对使用者**不可见**，归 commit message 与契约的现状描述。
   - **「不显著的不必记」不解除 Bad Practices 4**：它对 notable changes 这个集合成立——**集合内不许挑着记，集合外一条都不记**。
   - **单条主句 ≤ 60 字**，写「什么功能 + 变成什么样 + 我要做什么」；迁移 / 破坏性条目可在动作上多一句，整条不超两行。
   - **60 字的真正作用不是「短」，是一道防抄契约的物理闸**：一条契约条款的最小完整表述（值 + 判据 + 结果）必然超过 60 字，压不进去通常说明你在复述规范而不是报告变化。

## 反例

```markdown
## [1.2.0] - 2024-09-27
### Changed
- 修改 client.js、test/smoke.mjs、README.md
```

这是 `git log --stat` 的内容。升级者想知道的是「我要不要改配置」「有没有行为变化」——文件清单对他零信息。
