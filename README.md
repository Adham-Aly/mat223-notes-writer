# mat223-textbook-rewriter

An agent skill that turns one chapter of a linear algebra course's lecture-notes textbook (PDF) into an original, intuitive, lean, LaTeX-typeset PDF of notes.

It is not a summariser. A recon pass by subagents records what the chapter covers (by name), every formal definition and theorem verbatim, which items the user marks as *key*, and the verbatim proof of each key theorem whose proof the user marks as *testable*. The notes are then written from scratch from that recon file alone. The textbook is never looked at again.

## Install

```bash
npx skills add Adham-Aly/mat223-textbook-rewriter
```

Or copy `skills/mat223-textbook-rewriter/` into your agent's skills directory (for Claude Code, `~/.claude/skills/`).

## Use

Invoke the skill with the chapter to cover, the key list and the testable-proof list. For example:

```
/mat223-textbook-rewriter
source: textbook.pdf
cover: all of chapter 4 (4.1, 4.2, 4.3, 4.4), ignoring end-of-chapter exercises.

key items:
Definition 4.1: matrix-vector product
Definition 4.2: matrix transformation
Proposition 4.6: Linearity of Matrix Transformations
Theorem 4.8: Defining Matrix Theorem (testable)
Theorem 4.13: Pivot Characterization Theorem
```

The skill will not start without the scope, the key list and the testable-proof list (an explicit "none" is a valid list). "Testable" describes a proof, not a theorem; a theorem with a testable proof is always also key.

Output: `NAME.pdf`, the `NAME.html` it was typeset from, and `NAME-recon.md`, the recon file.

## Requirements

- `pdftoppm` and `pdfinfo` (poppler) for rasterising pages
- `node` and `npm` for KaTeX and headless Chromium (installed on first run by `scripts/setup.sh` into `~/.cache/mat223-textbook-rewriter`; set `MAT223_DEPS=/other/dir` to install somewhere else)
- An agent harness with subagents (the skill's locate and recon steps run in subagents so the main session never sees the textbook)

## Layout

```
skills/mat223-textbook-rewriter/
  SKILL.md                   the workflow: intake, locate, recon, the wall, write, typeset, check, deliver
  references/recon-brief.md  what recon subagents record, and how
  assets/template.html       HTML/CSS building blocks for the typeset document (boxes, badges, figures)
  scripts/setup.sh           installs KaTeX + Chromium (idempotent)
  scripts/render.mjs         HTML -> PDF, reports anything KaTeX could not typeset
```

See `AGENTS.md` for the rules an agent must keep when working on this skill.

## License

MIT
