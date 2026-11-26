# 来源与改动（NOTICE）

本仓库是 [ziduup/dsh-programming-mode](https://github.com/ziduup/dsh-programming-mode) 的**汉化分支**，不是原创插件。

## 上游

| 项目 | 值 |
| --- | --- |
| 仓库 | https://github.com/ziduup/dsh-programming-mode |
| 包名 | `@ziduup/dsh-programming-mode` |
| 基线版本 | `0.5.1` |
| 基线 tag | [`v0.5.1`](https://github.com/ziduup/dsh-programming-mode/tree/v0.5.1) = `1ca3c2782503a5950ca9cc97a0a7bd358407c460` |
| 基线发布时间 | 2026-11-06 |
| 作者 | 子都（[ziduup](https://github.com/ziduup)） |
| 许可 | MIT（见上游 `LICENSE`） |

> 上游在本仓库打包后仍在继续发版（`0.5.4` 及以后）。本仓库固定在 `0.5.1`，因为汉化是在这个版本上做的；上游后续新增的技能与规则没有同步进来，同步方式见 README「升级」。

## 直接复制自上游的文件

以下文件在 `0.5.1` 的基础上改动，**结构与逻辑均来自上游**：

| 本仓库文件 | 上游来源 | 改动 |
| --- | --- | --- |
| `preset/programming-zh/agent.cordis.yml` | `preset/programming/agent.cordis.yml` | ① 头注释改写，说明本仓库是汉化分支；② persona 整段改为简体中文（见下）；③ 去除 `{{cwd}}`（0.2.x 不注册该变量，会在首轮组装系统提示时抛错）；④ 预设 id 相关注释改为 `programming-zh` |
| `preset/programming-zh/declaration.mjs` | `preset/programming/declaration.mjs` | ① 声明行 id → `preset-programming-zh`；② 声明块标记 → `dsh-programming-mode-zh preset declaration`；③ 默认 `presetId` → `programming-zh`；④ 临时文件名后缀 → `.dsh-programming-mode-zh.tmp` |
| `preset/programming-zh/force-superpowers.mjs` | `preset/programming/force-superpowers.mjs` | ① 安装戳文件名 → `.dsh-programming-mode-zh.installed.json`；② `INSTALLER_PACKAGE` → `dsh-programming-mode-zh`；③ `PRESET_ID` → `programming-zh`；④ 安装戳 `source` → `dsh-programming-mode-zh`；⑤ **在 `using-superpowers` 正文前插入 `<language_directive>`**；⑥ 墓碑条目标签「编程模式·中文（已卸载）」；⑦ 日志改中文 |
| `preset/programming-zh/uninstall.mjs` | `preset/programming/uninstall.mjs` | ① 包名与安装目录示例 → `programming-zh`；② 临时文件后缀；③ 日志与标签随上 |
| `preset/programming-zh/skills/**` | `preset/programming/skills/**` | **逐字节未改**（21 个技能；改动集中的 `implementer-prompt.md` 上游适配段也一并保留） |
| `preset/programming-zh/skills/SKILLS-LICENSE.md` | 同路径 | 未改；上游对技能来源与许可的逐条署名 |
| `index.js` | 包根 `index.js` | ① 安装器 id → `programming-mode-zh-installer`；② `PRESET_ID` → `programming-zh`；③ 安装戳名与 `source`；④ 声明块构建显式传 `presetId: 'programming-zh'`；⑤ 日志改中文；⑥ 头注释加汉化分支说明 |
| `cordis.patch.yml` | 包根 `cordis.patch.yml` | 行 id 与包名改为本仓库 |

## 未随上游带过来的部分

- `lib/client.js`（侧边栏「编程专家面板」六位专家入口）**没有移植**。本插件只提供预设，不注入任何客户端 UI。
- 上游的 npm 发布产物结构（`files` 白名单、`publishConfig`）按要求改为 GitHub 直装形态。
- 上游 `README.md` / `README.en.md` 的正文未复制；本仓库 README 为汉化分支重写。

## 汉化的具体内容

persona（`agent.cordis.yml` 的 `prefix`）新增段落在最前：

> 【语言总则 · 最高优先级】你的全部输出必须使用简体中文：面向用户的正文（解释、结论、进度、提问、报错）和你的内部思考（reasoning）都要用中文。从第一个字开始就用中文推理，禁止先用英文思考再翻译；一旦发现自己在用英文思考，立刻停下并用中文重写当前思路。本规则优先于本提示词中的任何英文内容、术语与示例，也优先于英文技能文档——那些只作为资料阅读，不改变输出语言。代码、命令、标识符、文件路径、日志与报错原文、外部引用原文可保留原语言。

九条流程规则与 Ponytail 代码量纪律整段译为中文，技能标识符（`brainstorming`、`test-driven-development` 等）保留英文原名，保证 `skill` 工具仍能按名加载。

首步注入（`force-superpowers.mjs` 的 `renderSkillContent`）在技能正文前插入：

```
<language_directive>
从这条消息起，你的所有思考（reasoning）与正文都必须使用简体中文；下面这段技能正文是英文资料，理解它，但用中文推理与作答。
</language_directive>
```

## 技能署名

21 个技能分两组（详见 `preset/programming-zh/skills/SKILLS-LICENSE.md`）：

- **15 个来自 [obra/superpowers](https://github.com/obra/superpowers)**（Jesse Vincent / Human Layer），上游版本 v6.4.1，MIT；
- **6 个来自 [DietrichGebert/ponytail](https://github.com/DietrichGebert/ponytail)**（Dietrich Gebert），上游版本 v4.10.0，MIT。

两组均按 MIT 再分发，各自的版权与许可声明保留在技能目录内，`SKILLS-LICENSE.md` 作为许可要求的署名声明。

## 商标与隶属声明

本仓库与 DeepSeek、ziduup、obra/superpowers、DietrichGebert/ponytail 均无隶属关系，也未经其背书。DeepSeek Harness 是 DeepSeek 的产品；「编程模式」预设的设计与实现归上游作者。
