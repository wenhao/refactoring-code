# refactoring-code

[English](README.md) | [中文](README.zh.md)

A code-refactoring **skill** for ZCode (AI coding agent), distilled chapter-by-chapter from the complete Chinese translation of *Refactoring: Improving the Design of Existing Code (2nd Edition)* by Martin Fowler.

The skill encodes the book's methodology — small safe steps, test-first safety nets, bad-smell diagnosis, and the full catalog of refactorings — as on-demand instructions an AI agent loads when you ask it to refactor code.

> **Note:** the skill's instructions are written in Chinese, since they were distilled from the Chinese translation of the book.

## Repository structure

```
refactoring-code/
├── skill/                      # The skill itself
│   ├── SKILL.md                # Entry point: 5-step workflow + principle cheat-sheet + index
│   └── references/             # Loaded on demand (progressive disclosure)
│       ├── principles.md       # Mindset & principles (ch1, 2, 5)
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
    └── book-refactoring2/      # Source book (Chinese translation), study reference
```

That's **61 cataloged refactorings** and **24 bad smells**, each entry keeping the book's five-part shape (name → sketch → motivation → mechanics → example reference) with page-number anchors back to the original chapters in `knowledgebase/`.

## How the skill works

`SKILL.md` defines a five-step workflow:

1. **Establish intent & baseline** — two hats (never mix refactoring with new features); build a test safety net first (characterization tests for legacy code).
2. **Diagnose bad smells** — match the code against `smells.md`, attack the smell that hurts comprehension the most, one at a time.
3. **Consult the catalog** — load the matching `catalog-*.md` for mechanics; first-time use of a technique sends you to the original chapter for the full example.
4. **Small-step loop** — apply one tiny change → compile → test → commit. Roll back to the last green state rather than debugging forward.
5. **Wrap up** — update callers/docs, report which smells were fixed, which techniques were applied, and why behavior is unchanged.

## Installation

ZCode discovers skills from these directories (highest priority first):

- `<project>/.zcode/skills/<name>/`
- `<project>/.agents/skills/<name>/`
- `~/.zcode/skills/<name>/`
- `~/.agents/skills/<name>/`

Enable it by symlinking (or copying) `skill/` as `refactoring-code` under one of them:

```bash
# Enable globally (all projects)
ln -s /path/to/refactoring-code/skill ~/.agents/skills/refactoring-code

# Or for the current project only
ln -s /path/to/refactoring-code/skill .agents/skills/refactoring-code
```

## Usage

Once installed, the skill triggers automatically when you ask things like "重构这段代码", "improve this code's design", "clean up this tech debt" — even if you never say the word "refactor". You can also invoke it explicitly with `/refactoring-code`.

## Maintenance conventions

The skill was built by reading all 12 chapters and iterating after each one. When improving it:

- Keep each entry's "motivation + mechanics" structure and its page-number anchor into the book.
- If a distilled note conflicts with the book, **the book wins** — the anchor makes it easy to verify.
- The knowledgebase keeps the skill self-contained: chapter references in catalog entries resolve to `knowledgebase/book-refactoring2/docs/ch*.md`.

## Copyright

- `skill/` is a methodology digest in note form, for personal study.
- `knowledgebase/book-refactoring2/` comes from [MwumLi/book-refactoring2](https://github.com/MwumLi/book-refactoring2), the Chinese translation of *Refactoring, 2nd Edition*. All rights belong to the original author and translators; it is included for personal study only — do not redistribute commercially.
