# Skills

本机长期维护的个人 Agent Skills 集合。每个子目录是一个自包含的技能 bundle，通过软链挂进 DSH 的用户技能根，供所有会话按需加载。

## 结构

```
Skills/
  dev-docs-standards/     # 开发者文档体系规范
    SKILL.md              # 技能本体：frontmatter + 正文
    references/           # 按需加载的细则
    assets/               # 可直接复制的文档骨架
    scripts/              # 门禁参考实现（零依赖）
```

bundle 里只放技能内容：不放 `.git`、不放 `README.md`、不放 `.DS_Store`（本仓库根是唯一的仓库与 README 归处）。

## 元数据放哪

每个技能的版本、作者、许可证写在**它自己 `SKILL.md` 的 frontmatter 顶层**（`version` / `author` / `license`），全仓库只有这一处真源，不在本 README 或其它清单里重复。许可证正文在仓库根的 `LICENSE`（MIT），全集合共用一份。

**不放 `_meta.json`。** 那不是给作者写的文件：本机 `@michengai/dsh-skills-manager` 的 `provenance.js` 会读 `_skillhub_meta.json` / `_meta.json`，但只取其中的 `source` / `sourceType` 用来推「这个技能是从哪个仓库装的」；字段里还是 `ownerId` / `installedAt` 这类安装器状态。手写一份只会造出假溯源，而且下次安装或更新会被覆盖。发布到 SkillHub 时，那份文件由安装器生成，不是交上去的东西。

另注：DSH 的技能管理器只读 frontmatter 的**顶层简单键**（`name` / `description`，以及 `version` / `author` 这类同层键），嵌套的 `metadata:` 块它读不到——所以版本号直接放顶层，别塞进 `metadata:`。

## 怎么被 DSH 发现

DSH 的用户技能根是 `~/.dsh/skills`，扫描深度 1，只认 `<root>/<name>/SKILL.md`。本仓库不在任何扫描根里，所以每新增一个技能都要建软链：

```bash
ln -s ~/Documents/Agents/Skills/<name> ~/.dsh/skills/<name>
```

`dsh-skill-filesystem` 用 `stat`/`realpath` 跟随软链，技能面板同样走 `realpath`，加载、编辑、启停都照常。软链丢了重建一条即可，技能本体在 git 里。改 `SKILL.md` 的 frontmatter（`name` / `description`）会刷新会话目录，只改正文则下次调用即生效——都不需要重启。

## 加一个新技能

1. 建 `<name>/SKILL.md`，frontmatter 必须有 kebab-case 的 `name` 与 `description`
2. 软链到 `~/.dsh/skills/<name>`（见上）
3. 在下面「清单」补一行
4. `git add -A && git commit`

细则放 `<name>/references/*.md`，由 `SKILL.md` 按需指向，不要堆进正文。

## 清单

| 技能 | 版本 | 干什么 |
|---|---|---|
| dev-docs-standards | 见其 `SKILL.md` 的 `version` | 开发者文档体系：AGENTS.md / CHANGELOG / 契约 / 参考 / archive / MEMORY / WIP 各自装什么、谁保证不腐化 |

## 单独发布某个技能

本仓库是集合。要单独对外发布某个技能时用 subtree 拆历史，别复制目录重建仓库：

```bash
git subtree split -P dev-docs-standards -b skill/dev-docs-standards
```

**`skill/<name>` 是只读发布视图**：它的树根就是该技能内容本身（`SKILL.md` + `assets/` + `references/` + `scripts/`），可以直接推成独立仓库。`main` 上每次改动该技能后同步一次：

```bash
git subtree split -P dev-docs-standards -b skill/dev-docs-standards   # 幂等，可反复跑
```

- **先 commit 再 split**：split 只认提交、不认工作树，未提交的改动不会被带过去。
- **增量追加，不重写历史**：split 保留原提交的 author / date / message，所以旧提交 SHA 不变，已发布的 tag 仍指向同一份内容。
- **别在这条分支上直接改**：它是生成物，要改回 `main`；往主线回流要走 `git subtree merge`，容易分叉。
- **`LICENSE` 要放在 bundle 里**：split 只导出技能目录，根 `LICENSE` 不会被带出；要对外发布的技能在自己目录里放一份副本（`dev-docs-standards/LICENSE.md` 即是），split 会一并带出。
- **每次发版三步**：`main` 上打 `dev-docs-standards/vX.Y.Z` → 重跑 split → 在发布视图的新头上打 `vX.Y.Z`。

## 许可证

MIT，见仓库根 [LICENSE](LICENSE)。对外发布的技能在自己的 bundle 内放一份副本（`dev-docs-standards/LICENSE.md`；用 `.md` 后缀，SkillHub 打包只收带扩展名的文件），这样 `git subtree split` 与 SkillHub 打包都能带上它。
