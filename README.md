# Codex Skill Bundler

[![skills.sh](https://skills.sh/b/tomhuaijbig/codex-skill-bundler)](https://skills.sh/tomhuaijbig/codex-skill-bundler)

一个用于整理 Codex 技能的技能。它会把一批相关技能整理成更容易调用的 `aa-*` 技能包入口，同时把技能列表里的说明翻译成中文，但默认不改技能标题。

这个仓库里的核心技能是 [`skill-bundler`](./skills/skill-bundler/SKILL.md)。

## Quickstart

如果你的 Codex 支持从 GitHub 安装技能，可以直接让 Codex 安装这个仓库里的技能：

```text
安装 https://github.com/tomhuaijbig/codex-skill-bundler 里的 skill-bundler 技能
```

也可以手动复制：

```powershell
Copy-Item -Recurse .\skills\skill-bundler "$env:USERPROFILE\.codex\skills\skill-bundler"
```

安装后重启或刷新 Codex，让技能列表重新加载。

## Why This Skill Exists

同时安装很多技能后，技能列表会变长，用户需要记住每个技能的名字、英文说明和触发场景。`skill-bundler` 的目标是把这件事变成一次整理动作。

它不会替代原技能，也不会把原技能合并成一个大文件。它只生成更靠前、更容易点选的 `aa-*` 入口技能，并在入口里说明每个子技能适合什么场景。真正执行任务时，仍然回到原技能自己的触发逻辑和说明。

## What It Does

- 自动扫描已安装的 Codex 技能。
- 根据安装来源、安装时间、目录结构和功能相似性，把相关技能分组。
- 为每组生成或更新 `aa-*` 技能包入口，让它们在技能列表里排得更靠前。
- 把技能说明、短描述和插件描述翻译成中文。
- 默认保留原技能标题、文件夹名、`name`、`display_name` 和 Markdown 标题，不做标题汉化。
- 支持整理刚安装的新技能，也支持回头整理以前装过的技能。
- 更新已有技能包时尽量保留用户自己改过的中文别名和说明。

## When To Use It

适合在这些时候调用：

```text
整理一下我的技能
```

```text
安装完帮我打包技能
```

```text
把这些技能做成更方便调用的技能包
```

```text
把技能说明汉化，标题不要汉化
```

```text
刷新 AA 技能包
```

不适合在普通开发任务里调用。比如“修这个 bug”“做一个网页”“分析这个数据”这类任务，应直接使用对应的开发、浏览器、数据分析或文档技能。

## Behavior

`skill-bundler` 有三种主要模式：

- **Post-install mode**：刚安装完一批技能后，根据新增文件、安装时间和来源推断这一批技能，并生成对应技能包。
- **Backfill mode**：整理以前已经安装过的技能，给历史技能补上技能包入口。
- **Localization mode**：只做汉化和翻译，重点更新技能说明，不改标题。

默认生成的 `aa-*` 技能包通常只有一个 `SKILL.md`。除非用户明确要求，不会额外创建 `scripts/`、`references/` 或 `assets/`。

## Reference

- **[skill-bundler](./skills/skill-bundler/SKILL.md)** - 自动整理已安装的 Codex 技能，生成更容易调用的技能包入口，并汉化技能说明但默认不汉化标题。
- **[bundle-template](./skills/skill-bundler/references/bundle-template.md)** - 生成 `aa-*` 技能包时使用的结构模板。
- **[chinese-copy-rules](./skills/skill-bundler/references/chinese-copy-rules.md)** - 中文说明、别名、触发短语和 UI 元数据的写法规则。
- **[grouping-rules](./skills/skill-bundler/references/grouping-rules.md)** - 判断哪些技能应该被打包到同一个入口里的分组规则。

## Safety Rules

- 不删除原技能。
- 不重命名原技能。
- 不默认汉化标题。
- 不复制完整原技能正文进技能包。
- 修改后会验证 YAML、JSON、引用路径和子技能名称。

## License

MIT
