# Bundle Template

Use this template when generating an `aa-*` bundle skill.

This file intentionally uses ASCII-only placeholders. Replace placeholders with natural Chinese when writing the generated bundle.

```markdown
---
name: aa-example-skills
description: Chinese-friendly bundle entry for a related group of installed Codex skills. Use when the user asks which skill in this bundle to use, asks to use this bundle, asks to see Chinese descriptions for this bundle, or wants help calling one of these bundled skills.
---

# AA Example Skills

[Chinese overview: explain that this is an automatically generated bundle entry. It does not replace the original skills; it helps users choose and call this group of skills more easily.]

[Chinese note: UI titles and original skill names are kept unchanged by default. Chinese aliases and Chinese descriptions can be edited to match user or team habits. Future updates should preserve user edits when possible.]

## [Chinese heading: Common Entry Points]

- `child-skill-a` ([Chinese display name]): [One-sentence Chinese summary.]
- `child-skill-b` ([Chinese display name]): [One-sentence Chinese summary.]

## [Chinese heading: Skill List]

### `child-skill-a` ([Chinese display name])

- [Chinese label: Purpose]: [Chinese description.]
- [Chinese label: Best for]: [Typical scenarios.]
- [Chinese label: Trigger phrases]: [Chinese trigger phrases, English trigger phrases.]
- [Chinese label: Source]: [Original skill path or source note.]

### `child-skill-b` ([Chinese display name])

- [Chinese label: Purpose]: [Chinese description.]
- [Chinese label: Best for]: [Typical scenarios.]
- [Chinese label: Trigger phrases]: [Chinese trigger phrases, English trigger phrases.]
- [Chinese label: Source]: [Original skill path or source note.]

## [Chinese heading: Routing Rules]

[Chinese routing text: choose the most relevant original skill from the list above based on the user's request. After choosing, follow the original skill's own instructions. Do not treat this bundle as a replacement for the original skill.]

[Chinese fallback text: if no original skill matches, say this bundle does not contain a good match, then continue with normal Codex behavior.]

## [Chinese heading: Maintenance Notes]

- [Chinese note: users may manually edit Chinese aliases, Chinese descriptions, trigger phrases, and order.]
- [Chinese note: updates should preserve user-edited Chinese content.]
- [Chinese note: do not rename original skills or localize UI titles unless the user explicitly asks.]
- [Chinese note: do not copy full original skill bodies into this bundle.]
```

## Notes

The generated bundle should usually contain only `SKILL.md`.

Do not create `scripts/`, `references/`, or `assets/` inside generated `aa-*` bundle skills unless the user explicitly asks.

If adding `agents/openai.yaml` for a generated bundle, keep `display_name` in the original/title style and put Chinese text in `short_description`.
