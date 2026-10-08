# Working on this repository

This repo holds one agent skill, `skills/linear-algebra-lecture-notes-rewrite/`. It rewrites a chapter of a linear algebra course's lecture-notes textbook PDF into original, intuitive, lean typeset notes. Read this file before changing anything in the skill.

## What the skill is

- A deliberately narrow skill. It accepts **only the course's lecture-notes textbook PDF** and one chapter or run of sections of it. Do not add support for other sources or formats (images, slides, videos, transcripts, pasted text).
- Concepts are weighted by explanation-coverage levels and per-concept example entries recorded during recon. Proofs are decided by `with proof` / `stated without proof` bullets plus the user's testable-proof list.
- The three files that carry behaviour are `SKILL.md` (the workflow), `references/recon-brief.md` (what recon subagents record) and `assets/template.html` (the document's building blocks). `scripts/` installs the typesetting toolchain into `~/.cache/la-notes-rewriter` and renders HTML to PDF; leave it alone unless the typesetting itself is the task.

## The rules the skill lives by

These came from the owner, one at a time, and each is deliberate. Keep every one of them when editing.

1. **The wall.** Once the recon file is written, nobody opens the textbook again, for anything, including later revisions. The main session never opens it at any point: a locator subagent finds the chapter's pages (replying with numbers only) and recon subagents read them. Extra things the user wants out of the textbook are done during recon by recon subagents; they never open the wall.
2. **The recon file is names-only**, apart from exactly these additions: every formal definition/theorem/proposition/lemma/corollary verbatim; the verbatim proof of each theorem on the testable-proof list; `[KEY]` and `[TESTABLE]` tags; an explanation-coverage level per concept; a per-concept "received an example showing <type>" entry; and `figure` bullets that describe in words what each picture depicts. No explanations, no sample of the textbook's prose, no numbering anywhere, no notes on how the textbook presents things ("set as an activity").
3. **Verbatim items drop the textbook's numbering** ("Definition (Matrix transformation)", never "Definition 4.2"), and every back-reference by number inside them is replaced, drop-in, by the name of the thing referenced. Recon subagents may look elsewhere in the PDF solely to learn such a name.
4. **The user supplies the key list and the testable-proof list in the initial prompt**, identified by the textbook's numbering. The skill does not start without them; an explicit "none" is valid. Only listed items are marked key, however important others look. Lists never carry over between chapters.
5. **"Testable" is a property of a proof, not a theorem.** The theorem is key; its proof is testable. A theorem with a testable proof is always on the key list. The output badge reads "Testable proof".
6. **In the output, statements are in the writer's own words**, from its own knowledge, checked against the verbatim record for hypotheses, conclusion, names and notation. The verbatim record is not a text to copy from. The one piece of textbook text printed in the output is each testable proof, unaltered (only its closing square is dropped, since the proof block adds an end mark), with a Plan line before and an optional short note after.
7. **Depth tracks the textbook's emphasis.** Coverage levels (`extensive | moderate | brief | none`) and example entries set each concept's share of the notes; they record a level and a type only, never content. Key/testable tags mark, they do not weigh.
8. **Exercises are always excluded; chapter summaries get no bullets of their own.**
9. **Leakage matters to the owner.** Beyond the deliberate verbatim record, no textbook prose should reach the main session, not even in subagent replies. The delivery note discloses any statement that ended up close to the textbook's wording.

## How to make changes

- The owner asks for small, precise edits and wants the skill to stay laser-focused. Make exactly the change asked for, keep every default elsewhere, and do not expand scope. A request that adds one recording rule to recon is one paragraph in the brief and a matching sentence or two in `SKILL.md`, not a redesign.
- When a change touches both what recon records and how the writer uses it, edit the brief and `SKILL.md` together: the brief's sample list, the rule itself, the "before you finish" checklist, `SKILL.md`'s recon-file description, its sample recon file, the assembly rules, the planning or leanness rule that consumes the new information, and the check step.
- The brief's sample list and `SKILL.md`'s sample recon file should always show the current format, so subagents copy it.
- After editing, tell the owner exactly what changed, file by file.
- When asked to take inspiration from another skill, read it only; never edit it.
- Do not commit generated outputs (`.pdf`, `.html` other than the template, `*-recon.md`); `.gitignore` already excludes them.

## Testing a change

Run the skill on a chapter with a real key/testable list and check: the recon file has the names-only list, verbatim statements with numbering stripped, verbatim testable proofs, coverage and example sections; the PDF renders with no KaTeX errors (`scripts/render.mjs` reports them); every key item and only those carry the Key badge; each testable proof matches the recon file word for word; and no one opened the textbook after the recon file was written.
