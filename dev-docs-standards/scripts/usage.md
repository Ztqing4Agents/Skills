# 门禁脚本用法

`check-docs.mjs` 把 [references/gates.md](../references/gates.md) 的 14 道文档闸落成可执行代码。零依赖（只用 Node 18+ 内置模块）、ESM 单文件，可直接复制进任何项目。

**闸的定义、判据、实现取舍与已知边界都写在 `gates.md`**，本文件只讲怎么用。

## 用法

```bash
node scripts/check-docs.mjs                 # 检查当前目录
node scripts/check-docs.mjs --root <dir>    # 检查指定仓库根
node scripts/check-docs.mjs --json          # 机器可读输出
node scripts/check-docs.mjs --selftest      # 只跑内置自测（临时夹具，不碰真实仓库）
node scripts/check-docs.mjs --help          # 用法 + 闸清单
```

退出码：有 `red` → `1`，否则 `0`；参数错误 → `2`。失败不许绕过（`--no-verify`、注掉检查），绕过一次就永久失效。

建议挂成 `check:docs` 脚本（npm / make / CI 均可，口径相同）：

```json
{ "scripts": { "check:docs": "node scripts/check-docs.mjs", "check:docs:selftest": "node scripts/check-docs.mjs --selftest" } }
```

## 输出契约

每个问题一行，路径相对 `--root`：

```
spec.md:5 G3 red 规模上限 应为 300（契约），实为 999
README.md:121 G13 warn README 134 行 > 上限 120 行
- G12 skip `.agents/work/` 不存在（新克隆或未启用看板），工作现场一致性跳过
docs-standards: 1 red, 1 warn, 1 skip
```

- `red` / `warn` 按 `闸号 → 文件 → 行号` 排序，输出稳定可 diff。
- `skip` 表示**这道闸当前无法判定**（既不算通过也不算失败），单独一行，计入末行汇总。
- `--json` 输出 `{red, warn, skip}`，元素恒为 `{file, line, gate, message}`；skip 条目的 `file` 为空串、`line` 为 `0`。
- `G0` = 环境与不可判定：`skip`（git 不可用）不计入 red/warn、不影响退出码；`red`（文件读不到，如 EACCES）按普通红处理，行号固定为 `0`（无法定位到具体行）。

## 配置

仓库根读 `.docs-standards.json`，字段全部可选：

```json
{
  "whitelist": ["README.md", "AGENTS.md", "CHANGELOG.md", "spec.md", "docs/**"],
  "exclude": [],
  "idPrefixes": ["U", "M", "V", "D"],
  "limits": {},
  "descriptionSource": "package.json"
}
```

| 字段 | 缺省 | 说明 |
|---|---|---|
| `whitelist` | 无 | **只有写了才判 G1**；没写则 G1 记 `skip`，绝不误报红。支持 `*`（单段）、`**`（跨目录）、`?` |
| `exclude` | 无 | 命中的文件**不进任何闸**（G1–G14 都不判）。放「仓库里夹带、但不属于本规范管辖」的东西，例如随仓库携带的技能包 `"exclude": ["skills/**"]`。别用 `whitelist` 代替——进白名单仍要过 G2–G14 |
| `idPrefixes` | `U/M/V/D` | 看板 ID 前缀词表（G12） |
| `limits` | 脚本 `LIMITS` | 行数类阈值（G10/G12/G13），只写要覆盖的键；取值只在[档位表](../references/layout.md)里定义 |
| `descriptionSource` | `package.json` | G13 取 `description` 的来源文件（Python / Go 项目换成自己的清单） |

## 自测

```bash
node scripts/check-docs.mjs --selftest
```

每个用例在自己的 `os.tmpdir()` 夹具仓库里跑全部闸（尽量 `git init` + `git add`，git 不可用时降级为文件系统遍历）。逐条打印 `PASS <编号> <用例名>`；结果三态——`PASS` / `SKIP`（没跑，例如脚本被单独拷走、旁边没有 `assets/`）/ `FAIL`。有 `FAIL` 则退出码 1；`SKIP` 不改变退出码，但末行会把跳过数单列并提示「这次自测没覆盖全部判据」。

**自测只证明「按作者的理解是对的」**：夹具与实现出自同一人时会共享同一个误解。改门禁的人还要在独立工作树里对真实仓库跑一遍，并让另一个人按规格重写一组用例。

## 实现结构

单文件五段：**常量**（`LIMITS` / `GATES` / `ENUMS` / `DEFAULT_CONFIG`）→ **解析**（CLI、配置、git 清单、头注、表格）→ **各闸**（`gateG1`…`gateG14` + `GATE_IMPLS` + `runChecks`）→ **报告**（排序、文本 / JSON、`main`）→ **自测**（夹具 + `runSelftest`）。

加一道新闸：`GATES` 加条目 → 写 `gateGXX` → 注册进 `GATE_IMPLS` → 在 `selftestCases()` 加至少一个「会让它红」的用例。
