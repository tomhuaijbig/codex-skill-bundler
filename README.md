# AAA Skill Bundler

[![skills.sh](https://skills.sh/b/tomhuaijbig/aa-bundler-skill)](https://skills.sh/tomhuaijbig/aa-bundler-skill)

将已安装的 Codex / Claude Code 技能自动分类并生成 `aa-*` 入口索引，可选中文描述。

## 背景

同时安装多个技能后，技能列表迅速膨胀。用户需记忆每个技能的英文名称、功能描述与触发场景，查找效率随技能数量线性下降。`aa-bundler-skill` 将这一维护成本压缩为单次整理操作。

## 原理

该技能扫描所有已安装技能（`~/.codex/skills/`、插件缓存、代理目录），按安装来源、目录结构及功能相似度分组，为每个有意义的组生成一个轻量 `aa-*` 入口技能。入口排在列表顶部，用中文说明每个子技能的用途、适用场景和触发短语。

实际执行仍由原技能负责，入口仅承担索引与路由功能。

运行前会询问是否需要**中文描述**。若选择是，则翻译 `description`、`short_description` 等描述字段，同时保留 `name`、文件夹名、标题等字段不变。若选择否，生成英文入口。无论哪种模式，均不修改或删除任何原技能。

## 安装

### skills.sh（推荐）

```bash
npx skills@latest add tomhuaijbig/aa-bundler-skill
```

### 手动

```powershell
# Codex
Copy-Item -Recurse .\skills\aa-bundler-skill "$env:USERPROFILE\.codex\skills\aa-bundler-skill"

# Claude Code
Copy-Item -Recurse .\skills\aa-bundler-skill "$env:USERPROFILE\.agents\skills\aa-bundler-skill"
```

### 使用

```text
整理一下我的技能
```

```text
bundle my skills
```

---

## 包含技能

### `aa-bundler-skill` — 整理器

**路径**: [`skills/aa-bundler-skill/SKILL.md`](./skills/aa-bundler-skill/SKILL.md)

**功能**：

- 扫描全部已安装技能
- 按来源、时间和功能分组
- 每组生成一个 `aa-*` 入口
- 修改后自动校验 YAML、JSON、路径与引用完整性
- 更新已有入口时保留用户编辑的中文内容

**运行模式**：

| 模式 | 适用场景 |
|------|----------|
| 新装整理 | 刚安装一批新技能后 |
| 历史整理 | 对已安装的历史技能补建入口 |
| 描述汉化 | 仅翻译描述字段，不改标题 |

**安全约束**：

- 汉化前先询问用户
- 不删除、不重命名任何原技能
- 不将原技能正文复制到入口包
- 默认不汉化标题（`name`、文件夹名、标题均保留原样）

### `aaa-trial-hello` — 管道测试

**路径**: [`skills/aaa-trial-hello/SKILL.md`](./skills/aaa-trial-hello/SKILL.md)

**触发**: `试用技能` / `trial skill` / `skill demo`

用于验证技能安装管道是否正常工作，同时作为技能结构参考。

**兼容性**: Codex :check_mark: | Claude Code :check_mark:

---

## 平台兼容

| 平台 | 技能目录 | 说明 |
|------|----------|------|
| Codex | `~/.codex/skills/` | 完整触发支持 |
| Claude Code | `~/.agents/skills/` | `SKILL.md` 约定；忽略 `agents/openai.yaml` |

---

## 参考

| 文件 | 内容 |
|------|------|
| [aa-bundler-skill](./skills/aa-bundler-skill/SKILL.md) | 整理器定义 |
| [aaa-trial-hello](./skills/aaa-trial-hello/SKILL.md) | 管道测试技能 |
| [bundle-template](./skills/aa-bundler-skill/references/bundle-template.md) | 入口包结构模板 |
| [chinese-copy-rules](./skills/aa-bundler-skill/references/chinese-copy-rules.md) | 中文描述编写规范 |
| [grouping-rules](./skills/aa-bundler-skill/references/grouping-rules.md) | 技能分组规则 |

---

## License

MIT

