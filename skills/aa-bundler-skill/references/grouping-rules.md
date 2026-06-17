# Grouping Rules

Use these rules to decide which installed skills should become one generated `aa-*` bundle.

## Inputs To Inspect

For each candidate skill, inspect:

- skill folder path
- folder creation time
- folder last write time
- `SKILL.md` frontmatter `name`
- `SKILL.md` frontmatter `description`
- parent directory
- repository or plugin source when inferable from the path

Read the skill body only when frontmatter is missing, too vague, or when semantic grouping is active and deeper content analysis is needed.

## Grouping Strategies

The user chooses one strategy at the start of each run:

### Strategy A: Semantic Grouping (Recommended)

Analyze each skill's `description` field and body content to infer its functional domain. Group skills by what they do, not where they came from.

**Keyword-led inference**: Scan `description` and the first 100 lines of body text for domain-signaling terms. Common domain signals include:

| Domain | Signal keywords |
|--------|----------------|
| Engineering Discipline | test, debug, diagnose, TDD, bug, refactor, architecture, code quality, lint |
| Git & Version Control | git, commit, branch, PR, push, merge, worktree, rebase, hook |
| Documentation & Teaching | teach, document, handoff, explain, zoom, context, tutorial |
| Browser & UI Automation | browser, chrome, playwright, screenshot, click, page, DOM, CSS |
| Data & Analytics | data, metric, KPI, dashboard, chart, report, SQL, visualization, analytics |
| Creative & Design | design, Figma, layout, component, style, icon, logo, moodboard, mockup |
| Project Management | issue, PRD, plan, triage, spec, project, milestone, task |
| Video & Media | video, animation, render, frame, GSAP, caption, subtitle, audio |
| AI & API Development | API, SDK, agent, app, key, token, deployment, endpoint |
| Skill Management | skill, install, create, bundle, plugin, organize, manifest |

**Fuzzy matching**: When a skill's description does not contain any matching keyword, apply a broader word-similarity check against the skill's `name` and body. Compare against known skill names in each domain.

**Conflict resolution**: When a skill could belong to multiple domains (e.g., a skill with both "browser" and "test" keywords), assign it to the domain with the strongest keyword weight. Weight = (exact keyword matches in description) * 2 + (keyword matches in body).

**Edge case — mixed-source groups**: Semantic grouping may produce groups that mix skills from different plugin packages or local directories. This is intentional and desired. The bundle's source note should mention "Mixed sources" with a brief list.

### Strategy B: Source-Time Grouping (Legacy)

Group skills by their install origin and timestamp. This is the legacy behavior.

## Strong Grouping Signals (Strategy B)

Treat skills as one group when one or more strong signals apply:

- They were installed by the same command.
- They appeared in the same parent directory at the same time.
- They come from the same GitHub repository.
- They come from the same plugin cache package.
- Their folders share a clear package root, such as `mattpocock/skills`, Data Analytics, Product Design, Creative Production, or GitHub.

## Weak Grouping Signals (Strategy B)

Use weak signals only when strong signals are unavailable:

- Similar creation times within a short window.
- Similar naming patterns.
- Similar descriptions or domain terms.
- Obvious functional relationship, such as "browser automation", "document production", or "engineering workflow".

## Minimum Bundle Threshold

A group must contain **at least 3 skills** to become a bundle.

- **Default threshold**: 3
- **User override**: The user may set a different threshold (e.g., 2 or 5) at the start of the run. Record the chosen value.
- **Orphan skills**: Skills that belong to groups smaller than the threshold are not bundled. They are reported in the final output as "Not bundled (group size N < threshold T)".
- **Exception — explicit user request**: If the user explicitly names a group of 1-2 skills and asks for a bundle, override the threshold for that group only.

## Deduplication

Merge obvious duplicates:

- Same `name` and same or near-identical description from multiple cache locations.
- Superpowers skills copied into both plugin cache and local skill directories.

Do not merge same-name skills when descriptions or sources differ. Instead, include source notes. Example: one `prototype` skill may belong to Product Design, while another may belong to an engineering workflow package.

## Bundle Size

Prefer meaningful bundles over huge catch-all bundles.

Good bundle sizes:

- 3-25 skills from one source or domain.
- One generated bundle per plugin package.
- One generated bundle per GitHub repository install.

Avoid creating a single bundle for every installed skill unless the user explicitly asks for a master index.

## Bundle Naming

Generate folder names with:

```text
aa-<short-source-or-topic>-skills
```

Rules:

- Use lowercase letters, digits, and hyphens only.
- Start generated bundle names with `aa-`.
- Keep names short.
- When using semantic grouping, prefer topic names: `aa-engineering-discipline-skills`, `aa-git-version-control-skills`.
- When using source-time grouping, prefer source names: `aa-mattpocock-skills`, `aa-creative-production-skills`.
- If a generated name already exists for a different group, add a short suffix.

## Skip Rules

Skip a group when:

- It already has an up-to-date `aa-*` bundle and the user did not ask to refresh it.
- The group size is below the minimum threshold and no explicit override was given.
- The source is ambiguous and generating a bundle could mislead the user.
- In incremental mode, the group has no new or modified members since the last scan.

When skipping, explain why.
