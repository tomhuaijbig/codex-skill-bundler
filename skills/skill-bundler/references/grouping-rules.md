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

Read the skill body only when frontmatter is missing or too vague.

## Strong Grouping Signals

Treat skills as one group when one or more strong signals apply:

- They were installed by the same command.
- They appeared in the same parent directory at the same time.
- They come from the same GitHub repository.
- They come from the same plugin cache package.
- Their folders share a clear package root, such as `mattpocock/skills`, Data Analytics, Product Design, Creative Production, or GitHub.

## Weak Grouping Signals

Use weak signals only when strong signals are unavailable:

- Similar creation times within a short window.
- Similar naming patterns.
- Similar descriptions or domain terms.
- Obvious functional relationship, such as "browser automation", "document production", or "engineering workflow".

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
- Prefer source names when obvious, such as `aa-mattpocock-skills`.
- Prefer topic names when source is not obvious, such as `aa-browser-automation-skills`.
- If a generated name already exists for a different group, add a short suffix.

## Skip Rules

Skip a group when:

- It already has an up-to-date `aa-*` bundle and the user did not ask to refresh it.
- The group has only one skill and a bundle would not make calling it easier.
- The source is ambiguous and generating a bundle could mislead the user.

When skipping, explain why.
