# AAA Skill Bundler

[![skills.sh](https://skills.sh/b/tomhuaijbig/aaa-skill-bundler)](https://skills.sh/tomhuaijbig/aaa-skill-bundler)

把已安装的技能整理成清爽的 `aa-*` 入口包。兼容 Codex 和 Claude Code。

## 痛点

装技能简单，用技能难。装了一堆之后，技能列表变成一堵英文墙。你忘了 `metric-diagnostics` 是干嘛的，分不清 `figma-use` 和 `figma-generate-design` 该用哪个。最后不是放弃，就是每次翻半天。

## 解法

`aaa-skill-bundler` 扫描所有已安装的技能，按来源和功能分组，生成轻量的 `aa-*` 入口技能。这些入口排在列表顶部，用中文告诉你每个子技能做什么、什么时候用、有哪些触发词。

真正的执行仍然走原技能。入口只负责让你找得到。

运行时会先问你**要不要汉化**。要的话，把说明翻成中文，但标题、文件夹名、`name` 保留原样。不要的话，生成纯英文入口。不改名、不删除、不出意外。

## 快速开始

### skills.sh（推荐）

```bash
npx skills@latest add tomhuaijbig/aaa-skill-bundler
```

选你需要的技能和目标代理（Codex 或 Claude Code），就装好了。

### 手动安装

```powershell
# Codex
Copy-Item -Recurse .\skills\aaa-skill-bundler "$env:USERPROFILE\.codex\skills\aaa-skill-bundler"

# Claude Code
Copy-Item -Recurse .\skills\aaa-skill-bundler "$env:USERPROFILE\.agents\skills\aaa-skill-bundler"
```

### 触发

```text
整理一下我的技能
```

```text
bundle my skills
```

---

## 技能清单

### `aaa-skill-bundler` — 整理器

主技能。扫描、分组、生成 `aa-*` 入口包。

**文件**: [`skills/aaa-skill-bundler/SKILL.md`](./skills/aaa-skill-bundler/SKILL.md)

**功能**:

- 扫描 `~/.codex/skills/`、插件缓存和代理目录
- 按安装来源、时间和功能分组
- 每组生成一个 `aa-*` 入口技能
- 每次修改后验证 YAML、JSON、路径和引用
- 更新已有包时保留用户自己改过的中文内容

**模式**:

| 模式 | 场景 | 触发词 |
|------|------|--------|
| 新装整理 | 刚装了新技能 | `刚安装完技能`、`打包技能` |
| 历史整理 | 以前装过的技能 | `整理一下我的技能` |
| 纯汉化 | 只要翻译 | `汉化技能说明`、`翻译技能` |

**安全规则**:

- 汉化前先询问
- 不删除、不重命名原技能
- 不把原技能正文复制进入口包
- 默认不汉化标题

### `aaa-trial-hello` — 试用测试

最小演示技能，用来验证技能管道是否正常。

**文件**: [`skills/aaa-trial-hello/SKILL.md`](./skills/aaa-trial-hello/SKILL.md)

**触发词**: `试用技能` / `trial skill` / `skill demo`

**用途**: 确认安装成功、学习技能结构、测试跨平台兼容性。

**平台**: Codex :check_mark: | Claude Code :check_mark:

---

## 平台支持

| 平台 | 技能目录 | 备注 |
|------|----------|------|
| Codex | `~/.codex/skills/` | 完整触发支持，所有元数据字段 |
| Claude Code | `~/.agents/skills/` | `SKILL.md` 约定；`agents/openai.yaml` 被忽略 |

---

## 参考文件

| 文件 | 说明 |
|------|------|
| [aaa-skill-bundler SKILL.md](./skills/aaa-skill-bundler/SKILL.md) | 整理器技能定义 |
| [aaa-trial-hello SKILL.md](./skills/aaa-trial-hello/SKILL.md) | 试用演示技能 |
| [bundle-template.md](./skills/aaa-skill-bundler/references/bundle-template.md) | 生成包的结构模板 |
| [chinese-copy-rules.md](./skills/aaa-skill-bundler/references/chinese-copy-rules.md) | 中文说明写法规则 |
| [grouping-rules.md](./skills/aaa-skill-bundler/references/grouping-rules.md) | 技能分组规则 |

---

## 设计理念

> 反馈速度就是你的上限。

好工具找不到等于没有。`aaa-skill-bundler` 做的事很简单：把一堵英文墙变成一个清爽的、可扫读的中文索引。没有黑魔法，没有自动改名。就是分组、路由、可选中文化。

## License

MIT
