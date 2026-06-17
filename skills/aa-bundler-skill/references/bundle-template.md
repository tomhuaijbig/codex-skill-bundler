# Bundle Template

Use this template when generating an `aa-*` bundle skill.

This file intentionally uses ASCII-only placeholders. Replace placeholders with natural Chinese when writing the generated bundle (if localization is enabled). Otherwise, replace with English.

```markdown
---
name: aa-example-skills
description: <One-sentence domain description ending with "...or help choose which skill in this category to use.">
---

# AA Example Skills

<Brief overview: explain that this is an auto-generated bundle entry. It does not replace the original skills; it helps users choose and call this group of skills more easily.>

<Optional note when localization is enabled: UI titles and original skill names are kept unchanged by default. Chinese aliases and descriptions can be edited to match user or team habits. Future updates should preserve user edits when possible.>

## Common Entry Points

- `<child-skill-a>` (<Chinese alias or English name>): <One-sentence summary of what it does.>
- `<child-skill-b>` (<Chinese alias or English name>): <One-sentence summary of what it does.>

## Usage Constraints

<Only include this section if any child skill has a prerequisite, dependency, or required tool. If none, omit this section entirely.>

Some skills in this bundle have prerequisites. Check before using:

| Skill | Requires | Notes |
|-------|----------|-------|
| `<skill-name>` | `<prerequisite skill or tool>` | <Why it is needed and what happens without it.> |

<Example entries:>
| `figma-generate-design` | `figma-create-new-file` (or existing `fileKey`) | Without a fileKey, the import has no target file. |
| `openai-api-troubleshooting` | `openai-platform-api-key` | Credentials must be configured before troubleshooting can proceed. |
| `openai-platform-api-key` | OpenAI Platform account | A valid Platform account with billing is required. |

## Skill List

### `<child-skill-a>` (<Chinese alias or English name>)

- Purpose: <What the skill does.>
- Best for: <One or two typical scenarios.>
- Trigger phrases: <Chinese trigger phrases, English trigger phrases.>
- Source: <Original skill path or source note.>

### `<child-skill-b>` (<Chinese alias or English name>)

- Purpose: <What the skill does.>
- Best for: <One or two typical scenarios.>
- Trigger phrases: <Chinese trigger phrases, English trigger phrases.>
- Source: <Original skill path or source note.>

## Routing Rules

<Routing text: choose the most relevant original skill from the list above based on the user's request. After choosing, follow the original skill's own instructions. Do not treat this bundle as a replacement for the original skill.>

<Fallback text: if no original skill matches, say this bundle does not contain a good match, then continue with normal Codex behavior.>

## Maintenance Notes

- <Note: users may manually edit Chinese aliases, Chinese descriptions, trigger phrases, and order.>
- <Note: updates should preserve user-edited Chinese content.>
- <Note: do not rename original skills or localize UI titles unless the user explicitly asks.>
- <Note: do not copy full original skill bodies into this bundle.>
```

## Notes

The generated bundle should usually contain only `SKILL.md`.

Do not create `scripts/`, `references/`, or `assets/` inside generated `aa-*` bundle skills unless the user explicitly asks.

When generating constraint/dependency information, only scan the child skill's SKILL.md body for prerequisite mentions. Do not add speculative constraints — every constraint must be traceable to a concrete statement in the original skill's documentation.

If adding `agents/openai.yaml` for a generated bundle, keep `display_name` in the original/title style and put Chinese text in `short_description`.
