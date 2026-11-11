# Skills 集合契约

> 类型：契约
> 读者：在本仓库新增或修改技能的人与 agent
> 真源：本仓库目录结构、`.docs-standards.json`、各 bundle 的 `SKILL.md` frontmatter
> 更新触发：改 bundle 形态、软链方式、发布流程或门禁配置
> 规模上限：300

## 范围

管集合级约定与 bundle 的外部形态：目录怎么放、软链怎么命名、bundle 里允许出现什么、版本与发布怎么走。

不管 bundle 内部写什么——那是技能自己的产品内容，见 `dev-docs-standards/`。也不管单个技能的文档质量，那由技能自带的规范约束。

## 条款

### 1 目录与软链

- 仓库根只放集合级文档与技能目录，不放第二个 README。
- 每个技能一个目录，目录名 kebab-case，与该目录 `SKILL.md` 的 `name` 相同。
- DSH 通过软链发现技能：`~/.dsh/skills/<name>` → `~/Documents/Agents/Skills/<name>`。扫描深度 1，只认 `<root>/<name>/SKILL.md`。

### 2 bundle 里允许出现什么

- 允许：`SKILL.md`、`references/`、`assets/`、`scripts/`、`LICENSE.md`。
- 禁止：`README.md`、`.git`、`.DS_Store`、无扩展名的文件（SkillHub 打包只收带扩展名的文件，裸 `LICENSE` 会被 400 拒收）。
- bundle 一律自带一份 `LICENSE.md`，与仓库根的 `LICENSE` 内容一致；根那份覆盖整个集合。

### 3 `SKILL.md` 的 frontmatter

- 本地加载必需：`name`（kebab-case，等于目录名）、`description`（做什么 + 何时用，第三人称）。
- 发布到 SkillHub 还必需：`slug`、`displayName`、`version`（SemVer）、`license`；可选 `summary`、`author`、`homepage`、`tags`。
- 版本、作者、许可证只写在这里，仓库其它地方不重复；`.docs-standards.json` 之类清单里不复制版本号。

### 4 版本与发布

- 版本只增：发布过的版本号不能再发一次，也不能倒退。
- `slug` 全网唯一，发布后不再改动；改 slug 等于发布一个新技能。
- 每个发布过的版本在 `main` 上打 `dev-docs-standards/vX.Y.Z`，在发布视图头打 `vX.Y.Z`。
- 发布走 CLI：先 `--dry-run` 过本地预检，再带 `--changelog` 正式发布。

### 5 发布视图

- `skill/<name>` 分支由 `git subtree split -P <name> -b skill/<name>` 生成，树根即技能内容，可直接推成独立仓库。
- 该分支**只读**：不在其上直接提交；要改回 `main` 改，再重跑 split。
- 重跑是幂等的，旧提交 SHA 不变，已发布的 tag 不受影响。

## 验收

| 条款 | 怎么验 |
|---|---|
| 1 目录与软链 | `ls -l ~/.dsh/skills/<name>` 指向本仓库同名目录；`basename` 与 `SKILL.md` 的 `name` 一致 |
| 2 bundle 内容 | 仓库根跑 `node dev-docs-standards/scripts/check-docs.mjs --root .`；再用 `git ls-files <name>` 逐条比对允许清单 |
| 3 frontmatter | `skillhub publish <name> --dry-run` 能过（缺 `slug`/`displayName`/`version` 会当场报错） |
| 4 版本与发布 | `git tag --list '<name>/*'` 与平台上的版本列表对得上；`git describe --tags main` 等于 `SKILL.md` 的 `version` |
| 5 发布视图 | `git subtree split -P <name> -b skill/<name>` 后 `git ls-tree --name-only skill/<name>` 只列出技能内容 |
