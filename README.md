<div align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="assets/logo-dark.svg" />
    <img src="assets/logo.svg" alt="refactoring-code logo" width="160" />
  </picture>
</div>

# refactoring-code

Refactor code with small, safe, test-backed steps. A skill distilled from *Refactoring: Improving the Design of Existing Code (2nd Edition)*, chapter by chapter.

[English](README.md) | [中文](README.zh.md)

## Overview

- **Bad-smell diagnosis**: 24 named smells (long function, duplicated code, feature envy…) mapped to first-choice refactorings, so the agent knows *what* to look for and *which* technique to reach for.
- **Health score**: diagnosis condenses into a 5-point score (one decimal) across 6 refactoring dimensions — naming, functions, data, module boundaries, abstraction, duplication — each dimension backed by file-level evidence.
- **61 cataloged refactorings**: every technique from the book's catalog — Extract Function, Encapsulate Variable, Replace Conditional with Polymorphism, Replace Subclass with Delegate… — as small-step mechanics the agent follows one change at a time.
- **Human in the loop**: the agent never edits a line before presenting a plan (findings → suggested techniques → risk assessment) and getting your approval on scope and depth.
- **Incremental by default**: scans only your uncommitted and unpushed changes, and reminds you a full-repo scan is available.

> The skill's instructions are written in Chinese, distilled from the book's Chinese translation.

## Features

- **Evidence-based catalog** — distilled by reading all 12 chapters; every entry keeps the book's five-part shape (name → sketch → motivation → mechanics) with page-number citations for human traceability.
- **Evidence-based health scoring** — 6 dimensions covering all 24 smells; scoped to what was actually scanned, with unrated dimensions called out and a weakest-link warning when any dimension drops to 2 or below; the score never bypasses the decision gate; re-scored at wrap-up for a before/after comparison.
- **Self-contained install** — the `skill/` directory is everything an agent needs; no paths into this repo, no external dependencies.
- **Universal SKILL.md format** — works with ZCode, Claude Code, and Codex (and any agent that discovers skills from a `SKILL.md`).
- **Safety-net first** — characterization tests for legacy code before touching it; green-bar discipline throughout (test failed → roll back, don't debug forward).
- **Two hats enforced** — refactoring commits never mix in new features.
- **Decision gate** — read-only diagnosis, then a plan you approve before any edit; mid-flight discoveries are recorded for the next decision round, never fixed on the fly.
- **Progressive disclosure** — a lean `SKILL.md` workflow; the heavy catalogs load only when a matching smell is diagnosed.

## Quick Start

### 1. Install

Symlink (or copy) `skill/` as `refactoring-code` into your agent's skills directory:

| Agent | Project-level | Global |
|-------|---------------|--------|
| ZCode | `<project>/.zcode/skills/` or `<project>/.agents/skills/` | `~/.zcode/skills/` or `~/.agents/skills/` |
| Claude Code | `<project>/.claude/skills/` | `~/.claude/skills/` |
| Codex | `<project>/.codex/skills/` | `~/.codex/skills/` |

```bash
# Global install for Claude Code
ln -s /path/to/refactoring-code/skill ~/.claude/skills/refactoring-code

# Project-level install for Codex
ln -s /path/to/refactoring-code/skill .codex/skills/refactoring-code
```

Replace `/path/to/refactoring-code` with your local clone path. For global installs, copy instead of symlink if your clone may move.

### 2. Trigger it

Just ask, in your own words — the skill triggers even without the word "refactor":

```text
Refactor the code from my current changes.
Improve the design of the code I'm working on.
This code is a mess — help me clean up the tech debt.
```

Or invoke it explicitly where the agent supports it (e.g. `/refactoring-code` in ZCode).

### 3. Review the plan, then approve

The agent will scan your incremental changes, report the bad smells it found, and propose techniques with a risk assessment. You decide the scope (first item only / low-risk items / everything), the depth (local tidy-ups vs. structural changes), and any do-not-touch constraints. Only then does it edit — one small change, one test run, one commit at a time.

## How it works

```text
1. Scope        git diff @{u} + untracked files → report scope → remind about full-repo scan
2. Baseline     intent (two hats) + test safety net (characterization tests if none)
3. Diagnose     match code against 24 bad smells, condense into a 5-point health score — read-only
4. DECIDE       scorecard + plan (findings / techniques / risks) → wait for user approval
5. Execute      approved items only, following catalog small-step mechanics
6. Loop         tiny change → compile → test → commit; roll back on red
7. Wrap up      report per approved item, incl. declined ones and new findings; re-score (before → after)
```

## Project structure

```text
refactoring-code/
├── skill/                      # The skill (self-contained; this is all you install)
│   ├── SKILL.md                # Entry point: 7-step workflow + principle cheat-sheet + catalog index
│   └── references/
│       ├── principles.md       # Mindset & principles (ch1-2): two hats, when to refactor, YAGNI
│       ├── testing.md          # Test safety net (ch4): characterization tests, red/green discipline
│       ├── smells.md           # 24 bad smells → first-choice refactorings (ch3)
│       ├── scoring.md          # Health score: 6-dimension, 5-point scorecard
│       ├── catalog-basic.md            # Extract/inline, rename, split phase… (ch6)
│       ├── catalog-encapsulation.md    # Encapsulate record/collection, extract class… (ch7)
│       ├── catalog-moving-features.md  # Move function/field, split loop, remove dead code… (ch8)
│       ├── catalog-data.md             # Split variable, derived variable → query… (ch9)
│       ├── catalog-conditionals.md     # Guard clauses, polymorphism, special case… (ch10)
│       ├── catalog-api.md              # Separate query/modifier, remove flag argument… (ch11)
│       └── catalog-inheritance.md      # Pull up/push down, replace type code, delegate… (ch12)
├── knowledgebase/
│   └── book-refactoring2/      # Source book (Chinese translation) — development-time
│                               # verification only; the installed skill does not depend on it
├── assets/                     # Logo (light/dark theme variants)
├── README.md                   # This file
└── README.zh.md                # Chinese version
```

## Maintenance conventions

The skill was built by reading all 12 chapters and iterating after each one. When improving it:

- Keep each entry's "motivation + mechanics" structure and its page-number anchor.
- If a distilled note conflicts with the book, **the book wins** — `knowledgebase/` has the full text for verification.
- **Portability rule**: never reference repo files (such as `knowledgebase/`) from inside the skill — installed users only take the `skill/` directory. Chapter and page numbers remain as citations only.

## Copyright

- `skill/` is a methodology digest in note form, for personal study.
- `knowledgebase/book-refactoring2/` comes from [MwumLi/book-refactoring2](https://github.com/MwumLi/book-refactoring2), the Chinese translation of *Refactoring, 2nd Edition*. All rights belong to the original author and translators; it is included for personal study only — do not redistribute commercially.
