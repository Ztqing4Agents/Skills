# Skills

本机长期维护的个人 Agent Skills 集合。每个子目录是一个自包含的技能 bundle，通过软链挂进 DSH 的用户技能根，供所有会话按需加载。

## 结构

```
Skills/
  dev-docs-standards/     # 开发者文档体系规范
    SKILL.md              # 技能本体：frontmatter + 正文
    references/           # 按需加载的细则
```

bundle 里只放技能内容：不放 `.git`、不放 `README.md`、不放 `.DS_Store`（本仓库根是唯一的仓库与 README 归处）。

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

| 技能 | 干什么 |
|---|---|
| dev-docs-standards | 开发者文档体系：AGENTS.md / CHANGELOG / 契约 / 参考 / archive / MEMORY / WIP 各自装什么、谁保证不腐化 |

## 单独发布某个技能

本仓库是集合。真要单独对外发布某个技能时用 subtree 拆历史，别复制目录重建仓库：

```bash
git subtree split -P dev-docs-standards -b skill/dev-docs-standards
```
