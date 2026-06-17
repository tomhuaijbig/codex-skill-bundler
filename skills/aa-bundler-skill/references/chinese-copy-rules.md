# Chinese Copy Rules

Use these rules when generating Chinese descriptions, aliases, trigger phrases, and UI metadata for bundled skills.

This file intentionally uses ASCII-only prose so it remains stable across Windows terminals and patch tools. The generated bundle content should still be written in natural Chinese.

## Priority

When updating existing generated bundle content, use this priority:

```text
user-edited Chinese content > existing generated Chinese content > newly generated Chinese content > original English description
```

Do not overwrite user edits unless the user explicitly asks to regenerate everything.

If unsure whether text was user-edited, preserve it or ask before overwriting.

## Interactive Localization Preference

Before generating bundle content, the caller (`aaa-skill-bundler`) will ask the user whether they want Chinese localization.

- If the user says yes: apply the rules below (translate descriptions, keep titles).
- If the user says no: skip all Chinese translation, use English descriptions throughout.
- If the user explicitly asks for localization mode (e.g. "汉化技能说明"): skip the ask, localization is implied.

This is controlled by the skill workflow, not by this reference file. This file only defines HOW to write Chinese copy once the decision to localize has been made.

## Title Policy

Do not localize UI titles by default.

Keep these fields in their original language unless the user explicitly asks to rename titles:

- skill folder names
- `SKILL.md` `name`
- Markdown headings such as `# PDF Skill`
- `agents/openai.yaml` `display_name`
- plugin `.codex-plugin/plugin.json` `interface.displayName`

For generated `aa-*` bundles, keep the technical skill name with the `aa-` prefix. This is for sorting and discovery. Put Chinese explanation in `description`, `short_description`, and the body.

When useful, include a Chinese alias in body text, not as the primary UI title. Example style:

- `diagnose`: Chinese name meaning "diagnosis/debug diagnosis".
- `tdd`: Chinese name meaning "test-driven development".
- `gh-fix-ci`: Chinese name meaning "fix GitHub CI".
- `image-to-code`: Chinese name meaning "image to code".
- `skill-installer`: Chinese name meaning "skill installer".

Keep the original skill name visible next to the Chinese name.

## Chinese Description

Each description should answer:

- What does this skill do?
- When should a user choose it?
- What important limit or source difference should they know?

Keep descriptions concise but useful. One or two sentences is usually enough.

Prefer practical Chinese that says the job and trigger context. Avoid marketing language.

For UI localization, translate these description fields when present:

- `SKILL.md` `description`
- `SKILL.md` `metadata.short-description`
- `agents/openai.yaml` `short_description`
- plugin `.codex-plugin/plugin.json` `description`
- plugin `.codex-plugin/plugin.json` `interface.shortDescription`
- plugin `.codex-plugin/plugin.json` `interface.longDescription`

Do not translate `default_prompt` unless the user asks; it is often an invocation hint and may intentionally contain skill names or English examples.

## Trigger Phrases

Generate both Chinese and English trigger phrases when useful.

Examples of intent categories:

- "fix this bug"
- "diagnose why this failed"
- "use TDD"
- "split this into issues"
- "create a PRD"
- "fix CI"

The generated bundle should translate these into natural Chinese trigger phrases, while keeping useful English trigger phrases too.

Trigger phrases should be examples, not rigid commands.

## User Editable Text

Generated bundle skills should clearly say that Chinese aliases and Chinese descriptions can be edited later.

The generated Chinese note should communicate:

- Chinese aliases can be changed to match team habits.
- Chinese descriptions can be edited by the user.
- Future bundle updates should preserve those edits when possible.

## Validation Rules

After localization, check:

- YAML frontmatter still parses.
- `agents/openai.yaml` still parses.
- plugin JSON still parses.
- UI title fields do not contain Chinese unless title localization was explicitly requested.
- Description fields requested for localization contain Chinese.
- Generated `aa-*` bundles reference existing child skill names.

## Tone

Use direct, practical Chinese. Avoid slogans, vague praise, or marketing copy.

Prefer:

- "Use this before implementation when requirements are fuzzy or architecture may be affected."
- "Use this when tests fail and the cause is not clear."

Avoid:

- "This powerful skill empowers every scenario with intelligent excellence."
