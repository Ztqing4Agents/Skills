# Skills

个人长期维护的 Agent Skills 集合：每个子目录是一个自包含的技能 bundle，通过软链挂进 DSH 的用户技能根，供所有会话按需加载。

## 安装

DSH 扫描深度 1，只认 `<root>/<name>/SKILL.md`，所以每个技能都要单独建软链：

```bash
ln -s ~/Documents/Agents/Skills/dev-docs-standards ~/.dsh/skills/dev-docs-standards
```

改完某个技能的 `SKILL.md` 后，下一次会话自动读到新版本，不必重启。

## 有什么

| 技能 | 干什么 |
|---|---|
| `dev-docs-standards`<br>Dev文档规范 | 开发者文档体系：README / AGENTS / 契约 / 参考 / CHANGELOG / 看板与 WIP 各装什么、谁保证不腐化；附可填模板与门禁脚本 |

版本号、作者、许可证写在各技能自己的 `SKILL.md` 里；变更见 [CHANGELOG.md](CHANGELOG.md)，本仓库不另记一份。

## 改这个仓库

新增一个技能要动的地方、以及必须守的硬约束（bundle 里放什么、slug 与版本怎么走、发布视图怎么维护），见 [AGENTS.md](AGENTS.md) 与 [spec.md](spec.md)。

## 许可证

MIT，见 [LICENSE](LICENSE)。
