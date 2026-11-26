# dsh-programming-mode-zh

> 编程模式 · 中文 —— 把 [ziduup/dsh-programming-mode](https://github.com/ziduup/dsh-programming-mode) 的「编程模式」预设汉化后独立打包的 DeepSeek Harness 插件。装上后模式选择器里多一个 **「编程模式 · 中文」**，规则不变，语言变成简体中文。
>
> A self-contained Chinese localization of the DSH *programming mode* preset: same Superpowers discipline, Chinese persona, Chinese reasoning.

[中文](#中文说明) · [English](#english-overview)

---

## 中文说明

### 这是什么

「编程模式」= 部署自带 `standard` 模式的全部工具与能力 + 一套强制执行的 Superpowers 工程纪律。本仓库把它的 **persona（人设提示词）整段改成简体中文**，并在首步技能注入里加了一条语言指令，让模型的思考与正文都默认用中文，其余流程规则、技能、工具面完全保留：

1. 行动前先查技能（`using-superpowers`）
2. 创作前先头脑风暴（`brainstorming`），设计批准后才动手
3. 多步任务先写计划（`writing-plans` → `executing-plans`）
4. 实现走 TDD 红绿重构（`test-driven-development`）
5. 调试先找根因（`systematic-debugging`）
6. 完成前必须跑真实验证（`verification-before-completion`）
7. 重要工作请求代码审查（`requesting` / `receiving-code-review`）
8. 需求文档与开发计划一律用中文撰写
9. 常驻 Ponytail 代码量纪律（7 级阶梯：YAGNI → 复用 → stdlib → 原生 → 已有依赖 → 一行 → 最少可用）
10. 首条消息强制注入 `using-superpowers` 技能全文，纪律从第一句就生效

### 前身与改动

本插件不是从零写的：**预设组合（`agent.cordis.yml`）、安装器（`index.js`）、声明块生成（`declaration.mjs`）、首步注入（`force-superpowers.mjs`）、21 个技能**全部来自上游 [ziduup/dsh-programming-mode](https://github.com/ziduup/dsh-programming-mode) `0.5.1`（MIT，© 2026 子都）。改动只有下面这些，逐条可对照：

| # | 项目 | 上游 `@ziduup/dsh-programming-mode@0.5.1` | 本仓库 `dsh-programming-mode-zh@1.0.0` |
| --- | --- | --- | --- |
| 1 | 分发形态 | npm 包，需 `dsh plugin add @ziduup/dsh-programming-mode` | 独立仓库，`dsh plugin add github:CYF736222347/dsh-programming-mode-zh`，不依赖上游包 |
| 2 | 预设 id | `programming` | `programming-zh`（**与上游不冲突**，两者可同时安装） |
| 3 | 安装目录 | `$DSH_HOME/.agent-presets/programming` | `$DSH_HOME/.agent-presets/programming-zh` |
| 4 | 声明块标记 | `# >>> dsh-programming-mode preset declaration` | `# >>> dsh-programming-mode-zh preset declaration` |
| 5 | 声明行 id | `preset-programming` | `preset-programming-zh` |
| 6 | 安装戳 | `.dsh-programming-mode.installed.json` | `.dsh-programming-mode-zh.installed.json` |
| 7 | **persona 语言** | 约 90% 英文，规则 9 单行较长 | **整段简体中文**，开头新增「语言总则 · 最高优先级」，明确要求 reasoning 也用中文 |
| 8 | **首步技能注入** | 只注入 `using-superpowers` 正文 | 正文前插入 `<language_directive>`：技能正文是英文资料，理解它，但用中文推理与作答 |
| 9 | **本地化兼容修复** | persona 含 `{{cwd}}`（0.2.x 不注册该变量，首轮会在调用模型前直接失败） | 已移除，persona 只保留 `{{model}}` |
| 10 | 安装器日志 | 英文 | 中文 |
| 11 | 客户端「编程专家面板」 | 有（`lib/client.js`，侧边栏六位专家入口） | **未包含**，本插件只做预设，不注入任何客户端 UI |
| 12 | 技能内容 | 21 个技能（15 superpowers + 6 ponytail），英文原文 | **原样保留，未翻译**（技能正文是资料，改语言的是 persona 与注入指令） |
| 13 | 卸载自我清理 | 有（墓碑 / 目录自删） | 沿用上游逻辑，只改标识符与提示语 |

为什么改这几处：模型的「思考语言」跟 persona 的语言强相关——原版 persona 九成是英文，结尾追加一条中文要求压不住首轮的英文惯性；把 persona 整段换成中文、再在首步注入里给出明确的语言指令，才真正改变了推理语言。

`{{cwd}}` 这条不是风格问题：DSH 0.2.x 不注册 `cwd` 提示变量，persona 里留着它会让模式的第一轮在组装系统提示时就抛错、零 token 结束。上游在声明块生成阶段做文本替换来规避，本仓库直接在源头去掉。

### 安装

```bash
# web profile
dsh plugin --profile web add github:CYF736222347/dsh-programming-mode-zh

# 桌面端（先完全退出 DSH NEXT 再执行；桌面端跑的是 desktop profile）
dsh plugin --profile desktop add github:CYF736222347/dsh-programming-mode-zh
```

也可以在插件管理器里用 `github:CYF736222347/dsh-programming-mode-zh` 添加。

装完**重启 DSH**，然后**新建会话**（旧会话不会换预设），在输入框上方的模式选择器里选「编程模式 · 中文」。会话头里的 `agentPreset` 应为 `programming-zh`——这是判断它真的生效了的最快方式。

要求：DSH `>= 0.2.1-rc.5`（与上游一致）。

### 和上游同时装会怎样

不会互相覆盖：预设 id、安装目录、声明块标记、安装戳都不同，技能目录也是各自一份。想两个都试，装完后模式选择器里会同时出现「编程模式」和「编程模式 · 中文」。

代价是磁盘上多一份约 550 KB 的技能副本，以及两个预设各自占用一份工具面。

### 卸载

```bash
dsh plugin --profile web remove dsh-programming-mode-zh
# 立即清理（不等重启）
node ~/.dsh/.agent-presets/programming-zh/uninstall.mjs
```

卸载后如果还有历史会话记录在这个预设下，插件会保留一个「编程模式 · 中文（已卸载）」的墓碑条目，让那些会话仍能打开（行为等同标准模式）；没有历史会话时整个目录会被删掉。

### 升级

本插件是**自包含**的：预设与技能都打包在仓库里，不跟随上游 npm 版本自动更新。上游发新版（新技能、新流程规则）时需要重新同步一次；同步方式与改动范围见 [CHANGELOG.md](CHANGELOG.md) 与 `NOTICE.md`。

### 目录结构

```
dsh-programming-mode-zh/
├── cordis.patch.yml                 # bundle patch：插入一行安装器插件
├── index.js                         # 安装器：植入预设目录 + 写声明块（0.2.x）
├── package.json                     # dsh.bundle.patch 指向 cordis.patch.yml
└── preset/programming-zh/
    ├── preset.yml                   # 模式名与描述（模式选择器里显示的文案）
    ├── agent.cordis.yml             # 预设组合：中文 persona + 全部工具行
    ├── declaration.mjs              # 0.2.x 声明块生成/替换/删除
    ├── force-superpowers.mjs        # 首步强制注入 using-superpowers（含语言指令）
    ├── uninstall.mjs                # 手动清理脚本
    └── skills/                      # 21 个技能（superpowers 15 + ponytail 6）
        └── SKILLS-LICENSE.md        # 技能署名与许可
```

### 已知限制

- **技能正文仍是英文**：这是有意的——技能是参考资料，翻译会让后续上游同步变成人工重写。语言由 persona 和注入指令保证。
- **不含客户端专家面板**：上游的侧边栏六位专家入口没有移植，本插件不注入客户端代码。
- **不跨 profile 共享**：和上游一样，声明块写在安装它的那个 profile 的 `cordis.patch.yml` 里；装到 `web` 不会出现在 `desktop`。
- **升级会覆盖手改**：若你在安装目录里手改过 persona，插件升级或重装会覆盖它（安装戳按版本比对）。

### 许可与署名

MIT。上游预设、安装器与技能打包：© 2026 子都（[ziduup](https://github.com/ziduup)）。技能：15 个来自 [obra/superpowers](https://github.com/obra/superpowers)（MIT），6 个来自 [DietrichGebert/ponytail](https://github.com/DietrichGebert/ponytail)（MIT），逐条署名见 `preset/programming-zh/skills/SKILLS-LICENSE.md`。汉化改动：© 2026 CYF736222347。完整说明见 [NOTICE.md](NOTICE.md)。

---

## English overview

A self-contained DSH plugin that installs a **Chinese localization** of the *programming mode* agent preset: the `standard` tool surface plus the Superpowers engineering discipline (skills-first, brainstorm, plan, TDD, systematic debugging, verification, code review), with a fully Chinese persona and a `<language_directive>` prepended to the first-step `using-superpowers` injection so the model reasons and answers in Chinese.

- **Upstream:** [ziduup/dsh-programming-mode](https://github.com/ziduup/dsh-programming-mode) `0.5.1` (MIT, © 子都). Composition, installer, declaration writer, injection hook and the 21 bundled skills are upstream's; the localization is the diff.
- **Preset id is `programming-zh`**, so this plugin installs alongside upstream's `programming` without either overwriting the other.
- **No client UI, no runtime dependency on the upstream package.** Skills stay in English on purpose: they are reference material, and translating them would turn every upstream sync into a manual rewrite.
- **Requires** DSH `>= 0.2.1-rc.5`. After installing, restart DSH, open a **new** session, and pick 「编程模式 · 中文」 in the mode picker.

```bash
dsh plugin --profile web add github:CYF736222347/dsh-programming-mode-zh
```

See [NOTICE.md](NOTICE.md) for the full provenance and the exact list of changes.
