---
name: aa-bundler-skill
description: 自动整理已安装的 Codex 技能，生成更容易调用的 aa-* 技能包入口，运行前会询问你是否需要汉化。仅用于安装技能后打包、整理历史技能、翻译说明或维护技能列表。
---

# AAA Skill Bundler

Create lightweight `aa-*` / `aaa-*` bundle entry skills and optionally localize skill descriptions to Chinese, so installed skills are easier to discover and call.

This skill is an installation, organization, and optional localization helper. It should run after skills are installed, when the user asks to organize skills installed earlier, or when the user asks to translate/localize skill descriptions. It must not replace, move, rename, or merge the original skills.

## Core Rules

- `aa-bundler-skill` is the organizer. It uses the `aaa-` prefix to sort at the very top of skill lists.
- Generated bundle skills use the `aa-` prefix so they sort near the top of skill lists.
- Users do not need to say "AA"; the prefix is an implementation detail.
- Original skills keep their own trigger logic and behavior.
- Generated bundle skills act as Chinese-friendly indexes and routers for related skills.
- Generated bundle skills should usually contain only a `SKILL.md`.
- Do not create scripts, assets, or extra files inside generated `aa-*` bundle skills unless the user explicitly asks.
- **Ask the user before localization.** Before generating bundle content or translating descriptions, ask the user whether they want Chinese localization. Do not assume yes.
- Do not localize skill titles by default. Keep `name`, folder names, headings, `display_name`, and `displayName` in their original form unless the user explicitly asks to rename titles.
- Default localization means translating descriptions and explanations: `SKILL.md` `description`, `metadata.short-description`, `agents/openai.yaml` `short_description`, and plugin `shortDescription` / `longDescription`.
- Preserve technical terms such as GitHub, PR, CI, Chrome, Playwright, PDF, PowerPoint, TDD, KPI, URL, and API when that is clearer than translating them.
- **Minimum bundle threshold.** A group must contain at least 3 skills to become a bundle. Groups with 1-2 skills are listed in the final report as unbundled with a reason.
- **Incremental updates.** On backfill runs, compare against the last scan timestamp. Only process skill directories that are new or have been modified since the last run. Skip unchanged groups.

## When To Run

Run this skill when the user asks to:

- install skills and then organize them
- bundle newly installed skills
- organize skills installed earlier
- scan existing skills and create bundles
- generate a Chinese skill index
- translate or localize skill descriptions
- make skill list grey descriptions Chinese while keeping titles unchanged
- make many installed skills easier to call

Do not run this skill just because a normal task might use a skill. For example, do not run it for "fix this bug", "build a page", "analyze this data", or "open the browser" unless the user is asking to organize skills.

## Workflow

### Phase 1: Setup

1. Determine whether this is post-install, backfill, localization, or mixed mode.
2. **Ask the user which grouping strategy to use:**
   - **Semantic grouping (Recommended)**: Analyze each skill's `description` and body keywords to group by functional domain. Skills like `tdd`, `test-driven-development`, `diagnose`, and `systematic-debugging` will be grouped under "Engineering Discipline" even if they came from different install sources.
   - **Source-time grouping**: Group by install source (plugin package, repo, parent directory) and install time. This is the legacy behavior — same-source skills land in one bundle regardless of function.
   - Present this as a clear two-option choice before scanning. If the user does not answer, default to semantic grouping.
3. **Ask the user whether they want Chinese localization.** Present a clear yes/no choice. If the user does not answer within a reasonable time, default to no (English-only bundles).

### Phase 2: Scan & Group

4. Scan installed skills by finding `SKILL.md` files under the relevant skill roots.
5. For incremental backfill runs, compare directory modification times against the last known scan timestamp. Only include new or modified skill directories. Skip directories that are unchanged and already belong to an existing bundle.
6. Read skill frontmatter first: `name`, `description`, and optional `metadata.short-description`.
7. Read `agents/openai.yaml` and plugin `.codex-plugin/plugin.json` only when they exist or when localizing UI metadata.
8. Group skills using the chosen strategy. See `references/grouping-rules.md` for detailed rules per strategy.

### Phase 3: Filter & Generate

9. Deduplicate obvious copies.
10. **Apply minimum bundle threshold.** Discard any group with fewer than 3 skills. These orphan skills are recorded in the final report as "Not bundled (group too small)".
11. For each remaining group, **check for usage constraints**:
    - Scan each child skill's SKILL.md body for prerequisite mentions (other skill names, required tools, required files or tokens).
    - Document these constraints in the generated bundle entry under a dedicated "Constraint" or "Prerequisite" section. For example: "Requires `figma-create-new-file` to have been run first if no existing fileKey is available."
12. Generate or update one `aa-*` bundle skill per meaningful group.
13. Preserve user-edited Chinese copy when updating existing generated bundles or descriptions (only relevant when localization was enabled).

### Phase 4: Validate & Health Check

14. **Structural validation.** Validate that all touched skill metadata still parses and that original titles were not localized unless explicitly requested.
15. **Bundle health scan.** After generating/updating bundles, scan all existing `aa-*` bundles to detect:
    - **Orphan references**: child skills named in a bundle that no longer exist on disk.
    - **Stale bundles**: bundles whose last child skill has been removed (empty bundle).
    - **Duplicate coverage**: two or more bundles that reference the same child skill.
    - **Constraint violations**: a child skill that declares a prerequisite, but the prerequisite is not present in the same bundle (or anywhere on disk).
    Report these findings separately in the final output.
16. **Save scan timestamp** for future incremental runs.

### Phase 5: Report

17. Report:
    - which `aa-*` bundles were created or updated
    - which original skills were included
    - which groups were skipped and why (too small, already bundled, ambiguous source)
    - which metadata description fields were localized (if localization was enabled)
    - validation counts and any remaining issues
    - health scan results: orphan refs, stale bundles, duplicates, constraint violations
    - whether the user should restart Codex to pick up new skill entries

## Operating Modes

### Post-install mode

Use this when the user just installed one or more skills.

If an install-before snapshot exists in the conversation or workspace, compare it with the current scan. If no snapshot exists, infer the newest batch from directory creation time, common source, and timestamp closeness.

### Backfill mode

Use this when the user asks to organize skills installed earlier.

Scan existing skills and create bundles for meaningful historical groups. Skip groups that already have `aa-*` bundles unless the user asks to refresh them. On subsequent backfill runs, use the incremental scan timestamp to skip unchanged directories.

### Localization mode

Use this when the user explicitly asks to translate, localize, or Chinese-ify skill entries. This mode implies the user already wants localization, so skip the ask step.

Default behavior:

- Keep visible titles unchanged: `display_name`, `displayName`, `name`, headings, and folder names stay original.
- Translate the grey/list descriptions and trigger descriptions into Chinese.
- If `agents/openai.yaml` exists, update only `short_description` by default.
- If a skill has no `agents/openai.yaml`, do not create one just to translate the title. Create one only when the UI needs a translated `short_description` and preserving the title is possible.
- If plugin `.codex-plugin/plugin.json` exists, keep `interface.displayName` unchanged and translate `description`, `interface.shortDescription`, and `interface.longDescription` when they are likely to appear in UI.
- For generated `aa-*` bundles, use the `aa-` prefix in the technical skill name for sorting. Put Chinese explanations in the body and description, not by renaming the title.
- If the user explicitly asks to rename titles, confirm scope before changing titles because it can affect discovery and user muscle memory.

When localizing plugin caches, prefer active plugin cache roots over marketplace or temporary candidate caches. Do not bulk-translate uninstalled marketplace candidates unless the user explicitly asks.

## Validation

After any bundle or localization update, run a fresh structural validation:

1. Parse every touched `SKILL.md` frontmatter as YAML.
2. Confirm required `name` and `description` fields still exist.
3. Parse every touched `agents/openai.yaml` as YAML and confirm `interface.display_name` and `interface.short_description` exist.
4. Confirm titles were not localized by default: `display_name` and plugin `displayName` should not contain Chinese unless the user explicitly requested title localization.
5. Confirm descriptions are Chinese where requested: `description`, `metadata.short-description`, `short_description`, `shortDescription`, and `longDescription`.
6. Parse every touched plugin `.codex-plugin/plugin.json` as JSON.
7. Resolve icon paths relative to the skill folder, not the `agents/` folder.
8. For generated `aa-*` bundles, verify referenced child skill names exist.
9. Report any duplicates separately from localization failures; duplicate installed copies are common when the same skill exists in personal skills, `.agents`, and plugin caches.

## References

Read only the references needed for the current task:

- `references/grouping-rules.md`: use when deciding which skills belong in the same generated bundle. Updated with semantic grouping rules and minimum threshold.
- `references/chinese-copy-rules.md`: use when generating or updating Chinese descriptions, aliases, trigger phrases, or UI metadata.
- `references/bundle-template.md`: use when writing a generated `aa-*` bundle `SKILL.md`. Updated with constraint/dependency section.

## Safety Rules

- Never delete original skills.
- Never rename original skills.
- Never edit original skill files unless the user explicitly asks to organize, bundle, translate, localize, or refresh them.
- Even when editing original skill metadata, change only the requested metadata fields and preserve original instructions, workflows, scripts, assets, and trigger behavior.
- Never localize titles by default. Keep original titles unless the user explicitly asks to rename titles.
- Generated `aa-*` bundles may be created or updated.
- Before overwriting an existing generated bundle, preserve user-edited Chinese text when possible.
- If unsure whether content was user-edited, ask before overwriting.
- Groups smaller than the minimum threshold (default 3) are never bundled. They are reported as unbundled in the final output.

## Final Response

After creating or updating bundles, report:

- which `aa-*` bundles were created or updated
- which original skills were included
- which groups were skipped and why (too small, already bundled, ambiguous source)
- which metadata description fields were localized, if any
- validation counts and any remaining issues
- health scan results: orphan refs, stale bundles, duplicate coverage, constraint violations
- whether the user should restart Codex to pick up new skill entries

