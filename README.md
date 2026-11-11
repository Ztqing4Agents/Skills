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

本仓库是集合。真要单独对外发布某个技能时用 subtree 拆历史，别复制目录重建仓库：

```bash
git subtree split -P dev-docs-standards -b skill/dev-docs-standards
```

## 许可证

MIT，见仓库根 [LICENSE](LICENSE)。`git subtree split` 拆出来的技能只带走它自己目录，**发布前记得把根 `LICENSE` 复制进该 bundle**。
