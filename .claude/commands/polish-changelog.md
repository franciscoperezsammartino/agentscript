Polish the draft CHANGELOG entry for the VS Code extension.

> **Note:** Run this command before `/rebuild-vsix`. The `prepackage` script runs
> `check-changelog.mjs`, which requires the new version to have a CHANGELOG entry.

> **All shell commands must be run from the repository root.**

## Steps

1. Read `packages/vscode/CHANGELOG.md`.

2. Locate the **topmost versioned section** — the first heading that matches the pattern
   `## [X.Y.Z]` (the `- DATE` suffix is optional). This is the draft entry added by the
   automated release workflow. Do NOT touch any other sections.

3. Rewrite every entry under that section into clear, user-facing language:
   - Write from the perspective of the end user ("You can now…", "Fixed an issue where…", "The X option now…").
   - Expand abbreviations and jargon into plain English.
   - Do NOT copy raw commit subject lines verbatim.

4. Group the rewritten entries under exactly these subsections (omit any subsection that has no entries):
   - `### Added` — new features and capabilities visible to users
   - `### Fixed` — bug fixes
   - `### Changed` — behaviour changes, renames, removals, or dependency bumps that affect users

   If the draft contains multiple `### Changed` blocks (or multiple blocks of any subsection),
   merge them into a single deduplicated block under that heading before writing back.

5. Remove entries that are **purely internal** and have no user-facing impact. Entries to remove include:
   - Version bump commits (`chore: bump version`, `chore(release): …`)
   - CI / tooling changes (`ci:`, `build:`)
   - Internal refactors (`refactor:`) unless the refactor changes observable behaviour
   - Test-only changes (`test:`)
   - Dependency upgrades that do not change any user-visible behaviour (e.g. `Updated dependencies: @agentscript/lsp-server@X.Y.Z` with no further detail)

6. Write the updated content back to `packages/vscode/CHANGELOG.md`, preserving the rest of the file exactly as-is (the `[Unreleased]` section, older version sections, and the header).

7. Stage the file — run:
   ```
   git add packages/vscode/CHANGELOG.md
   ```
   Do **NOT** commit.

8. Print a concise summary of what was changed: how many entries were kept, removed, and rewritten, and which subsections are now present.
