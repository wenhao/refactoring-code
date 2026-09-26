# refactoring-code

[English](README.md) | [中文](README.zh.md)

A code-refactoring **skill** for AI coding agents, distilled chapter-by-chapter from the complete Chinese translation of *Refactoring: Improving the Design of Existing Code (2nd Edition)* by Martin Fowler.

The skill encodes the book's methodology — small safe steps, test-first safety nets, bad-smell diagnosis, and the full catalog of refactorings — as on-demand instructions an agent loads when you ask it to refactor code. It is **self-contained**: the `skill/` directory is all an installed agent needs.

> **Note:** the skill's instructions are written in Chinese, since they were distilled from the Chinese translation of the book.

## Repository structure

```
refactoring-code/
├── skill/                      # The skill itself (self-contained, installable)
│   ├── SKILL.md                # Entry point: 6-step workflow + principle cheat-sheet + index
│   └── references/             # Loaded on demand (progressive disclosure)
│       ├── principles.md       # Mindset & principles (ch1, 2)
│       ├── testing.md          # Test safety net (ch4)
│       ├── smells.md           # 24 bad smells → first-choice refactorings (ch3)
│       ├── catalog-basic.md            # First set of refactorings (ch6, 11 techniques)
│       ├── catalog-encapsulation.md    # Encapsulation (ch7, 9)
│       ├── catalog-moving-features.md  # Moving features (ch8, 9)
│       ├── catalog-data.md             # Organizing data (ch9, 5)
│       ├── catalog-conditionals.md     # Simplifying conditionals (ch10, 6)
│       ├── catalog-api.md              # Refactoring APIs (ch11, 10)
│       └── catalog-inheritance.md      # Dealing with inheritance (ch12, 11)
└── knowledgebase/
    └── book-refactoring2/      # Source book (Chinese translation) — development-time
                                # reference only; the installed skill does not depend on it
```

That's **61 cataloged refactorings** and **24 bad smells**, each entry keeping the book's five-part shape (name → sketch → motivation → mechanics → example reference). Chapter and page numbers are citations for human traceability, not file paths.

## How the skill works

`SKILL.md` defines a six-step workflow:

1. **Scope the scan — incremental by default** — when no target is specified, scan only uncommitted and committed-but-unpushed changes (`git diff @{u}` plus untracked files), report the scope to the user, and proactively remind them a full-repo scan is available; an explicitly specified target or an explicit full-repo request always overrides the default.
2. **Establish intent & baseline** — two hats (never mix refactoring with new features); build a test safety net first (characterization tests for legacy code).
3. **Diagnose bad smells** — match the code against `smells.md`, attack the smell that hurts comprehension the most, one at a time.
4. **Consult the catalog** — load the matching `catalog-*.md` and follow its small-step mechanics; the skill is self-contained and requires no external files.
5. **Small-step loop** — apply one tiny change → compile → test → commit. Roll back to the last green state rather than debugging forward.
6. **Wrap up** — update callers/docs, report which smells were fixed, which techniques were applied, and why behavior is unchanged.

## Installation

The skill uses the common `SKILL.md` format (a directory with `SKILL.md` in YAML frontmatter carrying `name` and `description`), so it works with any agent that supports skills. Symlink (or copy) the `skill/` directory as `refactoring-code` into your agent's skills directory:

| Agent | Project-level | Global |
|-------|---------------|--------|
| ZCode | `<project>/.zcode/skills/` or `<project>/.agents/skills/` | `~/.zcode/skills/` or `~/.agents/skills/` |
| Claude Code | `<project>/.claude/skills/` | `~/.claude/skills/` |
| Codex | `<project>/.codex/skills/` | `~/.codex/skills/` |

```bash
# Example: enable globally for Claude Code
ln -s /path/to/refactoring-code/skill ~/.claude/skills/refactoring-code

# Example: enable for the current project in Codex
ln -s /path/to/refactoring-code/skill .codex/skills/refactoring-code
```

Replace `/path/to/refactoring-code` with your local clone path. For global installs, copy instead of symlink if your clone may move.

## Usage

Once installed, the skill triggers automatically when you ask things like "重构这段代码", "improve this code's design", "clean up this tech debt" — even if you never say the word "refactor". Agents that support explicit invocation can also call it directly (e.g. `/refactoring-code` in ZCode).

## Maintenance conventions

The skill was built by reading all 12 chapters and iterating after each one. When improving it:

- Keep each entry's "motivation + mechanics" structure and its page-number anchor into the book.
- If a distilled note conflicts with the book, **the book wins** — the knowledgebase in this repo has the full text for verification.
- **Portability rule**: never reference repo files (such as `knowledgebase/`) from inside the skill — installed users only take the `skill/` directory. Chapter and page numbers remain as citations only.

## Copyright

- `skill/` is a methodology digest in note form, for personal study.
- `knowledgebase/book-refactoring2/` comes from [MwumLi/book-refactoring2](https://github.com/MwumLi/book-refactoring2), the Chinese translation of *Refactoring, 2nd Edition*. All rights belong to the original author and translators; it is included for personal study only — do not redistribute commercially.
