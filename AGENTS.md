# AGENTS

在这个仓库干活的 agent：开工前读 [spec.md](spec.md) 的硬规则，再动文件。

## 项目概览

- 这里是**技能集合**：`<name>/` 是自包含的 Agent Skills bundle，仓库根只放集合级文档。
- **产品内容与项目文档是两套东西**。`dev-docs-standards/` 是商品本身，不受本仓库的文档规范管辖——它在 [.docs-standards.json](.docs-standards.json) 的 `exclude` 里，所有门禁对它隐形。别给它加「类型/读者」头注。

## 命令

| 要做的事 | 命令 |
|---|---|
| 把技能挂进 DSH | `ln -s ~/Documents/Agents/Skills/<name> ~/.dsh/skills/<name>` |
| 跑文档门禁 | `node dev-docs-standards/scripts/check-docs.mjs --root .` |
| 门禁自测 | `node dev-docs-standards/scripts/check-docs.mjs --selftest` |
| 发布前本地预检 | `skillhub publish dev-docs-standards --dry-run` |
| 发布到 SkillHub | `skillhub publish dev-docs-standards --changelog "<这版改了什么>"` |
| 刷新发布视图 | `git subtree split -P dev-docs-standards -b skill/dev-docs-standards` |
| 发版打 tag | 主线打 `dev-docs-standards/vX.Y.Z`，发布视图头打 `vX.Y.Z` |

## 硬约束

1. **bundle 里只放技能内容**：不放 `README.md`、`.git`、`.DS_Store`——仓库根是唯一的 README 归处。
2. **bundle 里不放无扩展名文件**：SkillHub 打包直接 400（`LICENSE` 因此改名为 `LICENSE.md`）。
3. **`slug` 全网唯一且不随版本变，`version` 只增**：改 slug 等于发新技能；版本倒退会被平台拒。
4. **新增技能必须同批改两处**：[README.md](README.md) 的清单、[.docs-standards.json](.docs-standards.json) 的白名单（若该技能目录要纳入文档规范）。
5. **不在 `skill/<name>` 分支上直接改**：那是 `git subtree split` 生成的只读发布视图，要改回 `main` 改、再重跑 split。

## 文档地图

| 文件 | 回答什么 |
|---|---|
| [README.md](README.md) | 对外：这是什么、怎么装、有哪些技能 |
| [AGENTS.md](AGENTS.md) | 在这仓库怎么干活：命令、硬约束、去哪找东西 |
| [spec.md](spec.md) | 必须成立的事实：bundle 形态、软链命名、版本与发布规则 |
| [CHANGELOG.md](CHANGELOG.md) | 升级者要知道的变更 |
