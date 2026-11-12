# CHANGELOG

面向装这些技能的人：只记升级时要注意什么，不记改了哪些文件。

## [Unreleased]

### Fixed

- 删掉一句错断言：原文说 `.agents/work/` 已被 `.agents/*` 规则覆盖。多数仓库只忽略了 `.agents/memory/`，照做会让看板与 WIP 直接进 git。现在明确要求自己加一行 `.agents/work/`，并说明为什么不能假设。

### Changed

- 「什么时候才建 WIP」补一条：`wip/` 是逐个任务的现场文件，不是队列——一个任务一个文件，完成即删，不留积压。

## [1.2.5] - 2026-11-11

### Changed

- 更新技能图标（网页端上传，包内容不变）。

## [1.2.4] - 2026-11-11

### Changed

- 展示名由「开发者文档体系规范」改为 **Dev文档规范**，`slug` 不变（改名不影响已装环境）。

## [1.2.3] - 2026-11-11

### Added

- 门禁支持 `exclude`：`.docs-standards.json` 里命中的路径不进任何闸，用来放仓库里夹带、但不归本规范管辖的产品内容（如技能包）。`whitelist` 仍然只回答「这份文档允许存在」。
- 自测补两个用例：`exclude` 生效时内容不被判红；未配 `exclude` 时同一份内容仍报「无头注」——证明它不是恒真。

## [1.2.2] - 2026-11-11

### Fixed

- bundle 内许可证文件改名为 `LICENSE.md`。SkillHub 打包只收带扩展名的文件，裸 `LICENSE` 会被 400 拒收；仓库根的 `LICENSE` 不受影响。

## [1.2.1] - 2026-11-11

### Added

- 补齐发布到 SkillHub 所需的 frontmatter：`slug`、`displayName`、`summary`、`homepage`、`tags`。
- bundle 内自带一份许可证副本，供独立发布时随包带走。

## [1.2.0] - 2026-11-11

### Added

- 首个版本：`references/` 重整为「怎么写某类文档」与「跨文件机制」两族，新增 `writing-reference.md`。
- 提供可直接填写的模板：README / AGENTS / CHANGELOG / spec / reference / BOARD / WIP。
- 提供 G1–G14 门禁脚本 `check-docs.mjs`（零依赖、含 `--selftest`）与 `exclude`、`limits` 等配置。
