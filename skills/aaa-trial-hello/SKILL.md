---
name: aaa-trial-hello
description: A trial/demo skill for testing skill installation in both Codex and Claude Code (cloud code). Simple hello-world skill that prints a greeting and explains skill fundamentals. Use when the user wants to test skill installation, learn skill structure, verify Codex/Claude Code skill loading, or confirm cross-platform compatibility.
---

# AAA Trial Hello — Codex & Claude Code Demo

This is a trial skill for demonstrating how skills are structured, installed, and triggered in both **Codex** and **Claude Code (cloud code)**.

It serves as a **smoke test** (can you install and trigger a skill successfully?) and a **learning reference** (what sections does a real skill have?).

This skill does not perform complex operations. It is intentionally minimal so new users or developers can verify their skill pipeline end to end.

## Expected Behavior

When triggered, this skill will:

1. Confirm that it was loaded correctly by the current platform (Codex or Claude Code).
2. List the key parts of its own structure so you can use it as a reference.
3. Suggest next steps for learning about skills (such as reading `skill-installer`, `skill-creator`, or `write-a-skill`).

## Platform Compatibility

This skill is designed to work on both platforms:

| Platform   | Support | Notes |
|------------|---------|-------|
| Codex      | Full    | Native support with all trigger features |
| Claude Code (cloud code) | Compatible | Works via `SKILL.md` convention; some UI metadata fields may not render |

### Claude Code (Cloud Code) Notes

- Claude Code recognizes skills placed under `~/.codex/skills/` or `~/.agents/skills/`
- The `name`, `description`, and body of `SKILL.md` are the primary fields recognized
- `disable-model-invocation` is Codex-specific and ignored by Claude Code
- Trigger phrases in the body help Claude Code recognize when to invoke this skill
- For Claude Code installation, copy the skill folder to the skills directory and restart

## Skill Structure Reference

Every compatible skill has:

- **Skill folder**: a directory under `~/.codex/skills/` (or `~/.agents/skills/` for Claude Code)
- **SKILL.md**: the main file containing frontmatter (`name`, `description`) and body (trigger logic, workflow, rules)
- **References** (optional): `references/` folder with supporting docs
- **Scripts** (optional): `scripts/` folder with executable helpers
- **Agent config** (optional): `agents/openai.yaml` for UI metadata (Codex-only)

## Trigger Phrases

- 试用技能 / trial skill / hello skill
- 测试技能加载 / test skill loading
- skill demo / 技能演示
- Codex/Claude 技能结构 / skill structure
- cloud code skill test

## Installation

### For Codex

```text
安装 https://github.com/tomhuaijbig/aa-bundler-skill 里的 aaa-trial-hello 技能
```

### For Claude Code (Cloud Code)

```powershell
Copy-Item -Recurse .\skills\aaa-trial-hello "$env:USERPROFILE\.agents\skills\aaa-trial-hello"
```

Or clone the entire repo and copy the skill folder to your Claude Code skills directory.

### Manual (both platforms)

```powershell
Copy-Item -Recurse .\skills\aaa-trial-hello "$env:USERPROFILE\.codex\skills\aaa-trial-hello"
```

## Source

This is an example skill included for demonstration and learning. Feel free to modify it, fork it, or delete it after testing.

## Notes

- This skill does not have side effects. It only reads its own structure and reports back.
- You can rename, copy, or extend this skill to create your own custom skills.
- For Claude Code specific features, check Claude Code documentation at docs.anthropic.com.

