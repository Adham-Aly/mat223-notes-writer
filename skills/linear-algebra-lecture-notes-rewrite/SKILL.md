---
name: "linear-algebra-lecture-notes-rewrite"
description: "Turn one chapter (or a run of sections) of the linear algebra course's lecture-notes textbook PDF into an original, intuitive, lean, LaTeX-typeset PDF. Not a summary or a paraphrase: a recon pass lists what the chapter covers by name, records every definition and theorem verbatim, marks the user's key items and the key theorems whose proofs are testable, and the document is then written from scratch without the textbook ever being looked at again. Use when the user names a chapter of these lecture notes and gives the list of key definitions/theorems and which of those theorems have testable proofs."
---

# Lecture-notes chapter -> original, intuitive, lean typeset notes

## The idea

Source material is rarely the best explanation of its own content. Textbooks are thorough but long, and written for every reader at once. This skill produces the document the reader actually wants: the same mathematics, explained the way an excellent tutor would explain it starting from a blank page, with ideas arriving one at a time and each one understood before the next.

**This is not a condenser.** The output is not a summary, a paraphrase, or a tightened edit of the textbook. The textbook is used for exactly two things: finding out *what is covered*, as a bare list of names, and recording the formal statements (and the proofs the user marks as testable) verbatim, for the two narrow uses described in section 1. Everything else, every explanation, proof, example and picture, is written from your own knowledge of the mathematics, as though the textbook did not exist.

The reason is quality, not style. A paraphrase inherits the textbook's structure, its gaps and its rhythm. A condensation strips out the connecting reasoning that made the original followable. Both are worse to read than the textbook itself. Rebuilding from the ideas up is the only way the result ends up clearer than what it came from.

| Comes from the textbook | Never comes from the textbook |
|---|---|
| Which topics, definitions, theorems, formulas and methods are covered (by name) | The wording of explanations, motivation, transitions, and of every proof the user has not marked as testable |
| The verbatim statement of each definition/theorem/proposition/lemma/corollary (recorded in the recon file, numbering stripped) | Its actual worked examples and figures (you make your own) |
| The verbatim proof of each key theorem whose proof the user marks as testable (printed in the output) | Its numbering and headings: theorem, definition, example, section and page numbers |
| How much explanation each concept received (a level, not the content), and which concepts received worked examples and of what kind | Any sample, quote or paraphrase of those explanations |
| The names it uses for things | Its paragraph structure and its order of explanation within a topic |
| The broad order of topics, and the level | |

The target reader is a student in this course who has not read the chapter. After reading, they should understand not just *what* each result says but *why* it's true and *how* it fits with the others, without the document being any longer than it needs to be.

Promises, in priority order:
1. **Nothing lost.** Everything on the recon list is in the output.
2. **Written from scratch.** Guaranteed by the wall described below, which is never crossed.
3. **Key items and testable proofs marked.** Every item on the user's key list carries a Key badge in the output; every key theorem whose proof is testable also carries a Testable proof badge and its verbatim proof.
4. **Intuitive.** Clearly easier to follow than a typical treatment of the same material. Depth tracks the textbook's emphasis: concepts it explained at length get the fullest treatment.
5. **Lean.** Every sentence must earn its place by removing a real confusion. No introductions, recaps, padding, motivational fluff, or explaining things the reader already gets.
6. **Nothing big added.** No theorems or techniques beyond what is on the recon list.

## The workflow

Two phases with a wall between them.

```
LOCATE    a subagent finds the PDF pages of the requested chapter (numbers only)
RECON     subagents look at those pages  ->  NAME-recon.md: list of names + verbatim statements + verbatim testable proofs
=====     THE WALL: the textbook is never looked at again, by anyone
WRITING   the main session plans, writes, typesets and checks, from the recon file alone
```

**The wall.** This is the most important rule in the skill. Once the recon file is written, the textbook is off limits for good, to the main session and to every subagent, for the rest of the task and for any later revisions. Do not open, view, rasterize, search, extract text from, skim or quote it, not even to check a single statement. The textbook is inspected only during locate and recon, which happen separately from and independently of the writing. The main session never opens the textbook at any point, before or after the wall: locating pages and recon are both done by subagents.

Why so strict: any contact with the textbook while writing pulls the writing toward it. The structure creeps in, then the examples, then the sentences. The only reliable protection is that the writer has nothing of the textbook to lean on beyond what the recon file deliberately carries.

The user's extra recon requirements (verbatim statements, key tags, testable proofs) are carried out entirely during recon by the recon subagents. They do not open the wall, change the names-only nature of the rest of the recon file, or change how the writing phase works. If the user later asks for more from the textbook, that is a fresh recon pass by a fresh recon subagent, which adds to the recon file; it is never done by the main session.

If the recon file turns out not to be enough (an ambiguous bullet, say), settle it from the neighbouring bullets, the verbatim statements and the level of the material. If that fails, ask the user what was meant. The textbook is not an option.

**Subagents.**
- At most **4** subagents at a time. The user can raise or lower this number; use theirs if they give one.
- Subagents run on the **same model as the main session**. Spawn them with no model override.
- Subagents do the locate step and the recon, and nothing else. The main session writes the whole document itself unless the user asks for the writing to be split among subagents (section 3).

## 0. Intake (main session)

**The source is only ever the course's lecture-notes textbook PDF.** No other format (images, slides, videos, transcripts, web pages, pasted text) is accepted; if the user offers one, say this skill works only on the lecture-notes PDF. The PDF is `textbook.pdf` in the workspace unless the user gives another path. **Do not open or view it yourself.** The only mechanical fact you collect is the page count (`pdfinfo FILE | grep Pages`).

**Required from the user's prompt.** Do not proceed until all three are present:
1. **Scope:** which chapter, or which sections of a chapter, to cover (e.g. "chapter 4: 4.1–4.4"). End-of-chapter exercises/questions are always excluded; chapter summaries get no bullets of their own (see recon).
2. **Key list:** which definitions, theorems, propositions, lemmas and corollaries are *key*, identified by the textbook's numbering and name, e.g. `Definition 4.2: matrix transformation`, `Theorem 4.8: Defining Matrix Theorem`. Only these items are marked Key; everything else is not, however important it looks.
3. **Testable-proof list:** which of the key theorems/propositions/lemmas have a proof that is *testable* (the user may write it inline, e.g. "Theorem 4.8 ... (testable)"). "Testable" is a property of the proof, not of the theorem: the theorem is key, and its proof is testable. A theorem with a testable proof is therefore always on the key list; if the user marks a proof testable without listing its theorem as key, treat the theorem as key too.

If the key list or the testable-proof list is missing, ask for it before starting the locate step; an explicit "none" from the user is a valid answer. A key or testable-proof list from an earlier chapter never carries over. The numbering in these lists is used only to find the items in the textbook; it never appears in the recon file or the output.

**Output directory.** Use the one the user gives. If none is given, use the workspace (the directory you are running in). The final `.pdf` and the `.html` it was built from both go there, with the same descriptive kebab-case name (e.g. `linear-transformations.pdf`, `linear-transformations.html`). Don't overwrite a file you didn't create; pick another name. Scratch files go in a temp directory.

**User instructions.** Everything in this skill is the default. The user may override any part of it: aim for a different length, change the number of subagents, split the writing among subagents, include the exercises. Apply an override to precisely what it names and keep the defaults everywhere else. An override that needs more from the textbook than this skill already extracts is carried out during recon, by passing the instruction to the recon subagents. It does not open the wall.

**Don't interview the user.** Ask only for the three required items above, or when the scope can't be determined. Otherwise proceed.

## 1. Locate and recon (subagents)

### Locate

Spawn one subagent to find the PDF pages. It may use the table of contents, the PDF outline, or rasterize pages and view them. Its reply must contain page numbers only, no section titles, topics or wording. Prompt shape:

```
Mechanical lookup only. /abs/path/textbook.pdf (N pages) is a linear algebra textbook. Find the
1-based PDF page indices where chapter C begins, where each of sections C.1, ..., C.k begins,
the last page of the chapter's content before its exercises, and the page where the end-of-chapter
exercises start. Delete any temp files. Reply ONLY with the page numbers, nothing about the content.
```

### Recon

Recon produces one file: `NAME-recon.md`, in the workspace. It has two kinds of content, kept in separate sections:

- **The coverage list.** A dot-jot list of what the chapter covers, each item given by name ("matrix-vector product", "Defining Matrix Theorem"), never by statement. It contains no mathematical content and no explanations. Key items carry the tag `**[KEY]**`; key theorems whose proof is testable carry `**[KEY]** **[TESTABLE]**`. It ends with two fixed subsections: `### Explanation coverage`, one entry per concept giving the level of explanation the textbook gave it (`extensive | moderate | brief | none (statement only)`), and `### Examples`, one entry per concept that received a worked example, of the shape "<concept> received an example showing <problem type>", with "(several)" when there were more than one. These carry a level and a type only, never what the explanation or the examples contained, so that the new document gives each concept a similar share.
- **The verbatim record.** Every formally stated definition, theorem, proposition, lemma and corollary in scope, copied word for word in LaTeX, with the numbering stripped (`Definition (Matrix transformation)`, never `Definition 4.2`) and any back-reference to a numbered item replaced, drop-in, by that item's name. Key and testable tags are repeated here. Then, in its own section, the verbatim proof of each theorem whose proof is testable, with the same back-reference replacement. These are the only textbook wording the recon file may carry.

The two narrow uses of the verbatim record during writing: (a) the verbatim testable proofs are printed in the output exactly as recorded; (b) the verbatim statements let the writer check that each statement it writes carries the same hypotheses, conclusion, names and notation conventions as the course. The writer still writes every statement in its own words from its own understanding; the record is not a text to copy from.

**Split.** Divide the chapter's pages into contiguous parts, one per recon subagent: about one subagent per 6 pages, up to the cap. A small chapter still gets at least one recon subagent, so that the main session never sees the textbook at all. Past the cap, give each subagent a longer range rather than adding subagents. If a part boundary may cut through a testable proof, tell both neighbours to read across the boundary to capture the whole proof.

**Spawn** them in parallel, each with a prompt of this shape (the brief lives in this skill's directory):

```
Read SKILL_DIR/references/recon-brief.md and follow it exactly.
Source: /abs/path/textbook.pdf, PDF pages 71-75 of 187 (first part of the chapter; another agent covers 76-81).
Exercises start on page 82; stop before them.
Key items (identified by the textbook's numbering so you can find them; never write the numbers in your output):
  Definition 4.1 (matrix-vector product); Definition 4.2 (matrix transformation); ...
Key theorems whose proof is testable (proof to be transcribed verbatim): Theorem 4.8 (Defining Matrix Theorem)
Other user instructions affecting recon: none.
```

Pass the complete key and testable-proof lists to every recon subagent, since any item may fall on any page. Pass on any other user instruction that affects what recon should record.

**Assemble.** Each subagent replies with its coverage list, its verbatim statements and any verbatim proofs. Put them together in source order and write `NAME-recon.md`:

```markdown
# Coverage: matrix-vector products, matrix transformations and linear maps (first course in linear algebra)

## Matrix-vector product
- definition: matrix-vector product (as a linear combination of the columns) **[KEY]**
- example type: computing matrix-vector products
...
- theorem: Defining Matrix Theorem (with proof, by expanding in the standard basis) **[KEY]** **[TESTABLE]**
...

## Explanation coverage
- coverage: matrix-vector product — brief
- coverage: Defining Matrix Theorem — moderate
...

## Examples
- example: matrix-vector product received an example showing a routine numeric computation (several)
...

## Verbatim statements

**Definition (Matrix-vector product) [KEY].** Let $A$ be an $m\times n$ matrix ...

**Theorem (Defining Matrix Theorem) [KEY] [TESTABLE].** Every linear transformation is ...

## Verbatim testable proofs

### Defining Matrix Theorem
**Proof.** Suppose that ...
```

While assembling:
- Merge duplicate bullets, statements and proofs where two parts met mid-topic (a proof that crosses a boundary comes back from both neighbours; keep one copy).
- Make the headings and style uniform, and add the one-line title naming the subject and level.
- Enforce the names-only rule on the coverage list yourself: if a subagent returned a statement, a formula, an explanation, or any theorem, section or page number in a bullet, cut the bullet back to the name before it goes in the file. Notes on how the textbook presents something ("set as an activity", "proof required") are not coverage; cut them. Merge the subagents' `### Explanation coverage` and `### Examples` subsections into one `## Explanation coverage` and one `## Examples` section after the list; cut anything in them beyond a level or a problem type (a sample of an explanation, an example's matrix, numbers or solution).
- A chapter summary or recap gets no bullets of its own; fold any fact it alone states into the topic it belongs to and drop the rest.
- Check that exactly the user's key items are tagged, that each theorem on the testable-proof list is tagged and has a verbatim proof, and that no numbering survives anywhere. If a key item or a testable proof is missing from every subagent's reply, send a recon subagent back for it; do not look yourself.
- Fix mechanical damage in the LaTeX (e.g. `&amp;` for `&`).

**Then the wall comes down.** From this point on, nobody looks at the textbook.

If no subagent mechanism is available, or the user set the number of subagents to zero, do the locate step and recon yourself by following `references/recon-brief.md`, write the file, and then hold yourself to the wall exactly the same: write from the recon file, not from your memory of the pages.

## 2. Find the through-line and plan the path (silently)

Work from `NAME-recon.md` and your own knowledge of the subject.

**Through-line.** Find the single idea that ties the chapter together, usually one picture or one sentence. This becomes a 1-3 sentence opening, and later sections refer back to it. It's the biggest intuition win and costs almost no space.

**Path.** Design the shortest chain of ideas from what the reader already holds to where the chapter ends, each link small enough to explain in a few sentences. Follow the list's broad order of topics so the document lines up with the course, but the sectioning, and how each idea is introduced, motivated and sequenced, are your own design: picture first, then plan, then the formal statement; move an item earlier if later material leans on it.

**Depth from coverage.** The coverage levels set the weight of each concept. `extensive` means the textbook considered it the hard part: give it the fullest intuition, a picture if spatial, and your own worked example. `brief` or `none (statement only)` means a statement with a one- or two-sentence lead-in is enough. Never skip a concept because its coverage is low, and never let a key or testable tag change a concept's weight: the tags mark, the levels weigh.

**Notation.** Choose standard notation for the level and keep it consistent throughout. Use the names the list uses for things. The verbatim testable proofs are printed as recorded, so choose the document's notation to agree with theirs (vector arrows, names like $T_A$, $\vec e_i$, $M_F$), so that the proof reads as part of the document rather than a quotation.

**Gaps.** For each bullet, find what a first-time reader will need beyond the bare result:
- **Missing prerequisite.** A term from earlier chapters. One-sentence Recall where first needed.
- **Purpose.** Why is this result here? One sentence before the statement, only if it isn't obvious.
- **Unexplained choices in proofs.** A specific substitution, case split or construction: say why it works. For a testable proof, this goes in a short note after the proof, never inside it.
- **Hidden hypotheses.** State the precise version directly.

## 3. Write

The main session writes the whole document, from the recon file and its own understanding of the mathematics. The tools, each used only where it removes a confusion:

- **Statements from your own knowledge, stated exactly.** Each definition and theorem on the list is written in its precise standard form for the level, with every hypothesis, in your own words. Then compare against the verbatim record: same hypotheses, same conclusion, same names, same notation conventions. Fix a mismatch by correcting the content, not by pasting the textbook's sentence.
- **Key badge.** Every item on the key list gets the `Key` badge (template: `<span class="tag">Key</span>`) right after its title in its Definition/Theorem box. No other item gets one.
- **Theorems with a testable proof.** The box also gets the `Testable proof` badge (`<span class="tag test">Testable proof</span>`). Its proof is the verbatim proof from the recon file, printed in full inside the proof block, unaltered: same sentences, same equations, same line breaks between displayed steps. Only typesetting adapts: the textbook's closing square is dropped because the proof block adds its own end mark, and the LaTeX is rendered with the document's KaTeX setup. A one-line Plan goes before it, and a short note after it may explain a step the reader might not follow (which linearity rule is used where, say). Never rewrite, shorten, "improve" or annotate inside the verbatim proof.
- **One new idea per paragraph or box.** If it needs "and also", split it.
- **Concrete before abstract.** An actual matrix or a pair of vectors, then the symbols.
- **A picture where the idea is spatial.** Every `figure` bullet gets a picture of that thing, of your own design. Beyond those, anything that moves a grid, a line or a region usually deserves a small clean SVG. Skip pictures that just decorate.
- **Intuition before formal.** One or two plain sentences saying what the statement means, then the formal statement.
- **Proof plan before every non-trivial proof.** One line naming the strategy. Then the proof, with the reasoning filled in as short steps. Prove what the list marks `with proof`, using the named technique if one is given, in your own words (testable proofs excepted, as above). For results marked `stated without proof`, don't add a full proof; a one-line reason is fine.
- **Your own examples.** For each `example type` bullet and each `example` entry, invent a clean instance of that problem type and work it, saying why each step is the natural one. "(several)" means two or three short instances. Check every computation.
- **Non-examples where a hypothesis matters.** For each `caution` bullet, and wherever a hypothesis can't be dropped, give a small counterexample right after the result and say which hypothesis it shows is needed.
- **Jargon explained in passing**, inside the sentence.
- **Quick checks, sparingly.** At most one per major section, a question that makes the reader use the idea. Answers in a few lines at the end.
- **Confident textbook voice.** No hedging. The document stands alone: never mention the textbook or the course materials ("the notes say", "in lecture").
- **No borrowed numbering.** Results are labelled by name: "Theorem (Defining Matrix Theorem)", "Definition (Matrix transformation)". The document's own plain section numbers are fine.

Style distinction: formal statements and proofs look formal (Definition/Theorem boxes, Proof ... end mark); intuition sits in lighter Idea and Recall boxes or in plain lead-in sentences.

**Leanness rules.**
- Length is set by the recon list: each bullet gets the space a first-time reader needs to understand it, weighted by its coverage level, and no more. As a guide, a definition with its intuition takes a few lines; a theorem with its plan and proof, a third to half a page. The levels set the ratios between concepts, not the absolute length; the rules here still set that.
- Each explanation is as short as it can be while still working. Usually one or two sentences; three only if a proof step needs it.
- Say each thing once. No restating a theorem, no end recap, no "in this section we will".
- Don't explain what the reader already understands.
- If it runs long, cut the weakest explanations first. Never cut something that is on the list, and never cut from a verbatim proof.

**Only if the user asks for the writing to be split among subagents.** Do section 2 yourself first, then hand each writing subagent (same cap, same model) everything you are working from:
- the path to this `SKILL.md`, with the instruction to follow "The wall" and section 3;
- the complete `NAME-recon.md`, not just their part of it;
- your full plan: through-line, section outline with the bullets assigned to each section, notation, any user overrides;
- which section(s) they are to write, and the building blocks in `assets/template.html`. They return an HTML fragment for the body only.

Do not give them the textbook or tell them where it is; the wall binds them too. When the fragments come back, combine them in order and consolidate the document as a whole: one voice, one notation, no repeated Recalls or duplicated explanations, cross-references and call-backs to the through-line in place, trimmed to the leanness rules. Then typeset and check it as a single document.

## 4. Typeset (LaTeX look via HTML + KaTeX -> PDF)

The document is authored as a single HTML file and printed to PDF with headless Chromium. Not Markdown, not LaTeX.

Setup (idempotent; fast after the first run). Paths are relative to this skill's directory:

```bash
bash scripts/setup.sh        # installs KaTeX + Chromium into ~/.cache/la-notes-rewriter, prints the KaTeX URL
```

Start the HTML from `assets/template.html`: read it, replace `KATEX_DIST` (three places) with the URL that `setup.sh` printed, and write the document into the body using the template's building blocks (including the `tag` badges). Write it straight to its final path in the output directory. The CSS uses `KaTeX_Main`, a Computer Modern lookalike, for body text so prose and maths match. Maths goes in `$...$` (inline) and `$$...$$` (display).

Diagrams: small inline SVGs, thin black strokes, one accent colour for the key region. Around 120-160px tall. KaTeX does not typeset inside SVG, so write labels in plain Unicode (θ, x², ≤) or SVG `tspan` subscripts, never `$...$`, and place them clear of the curves. For images of grids, lines and regions under a map, compute the points with a few lines of code and paste the resulting paths; an eyeballed picture that is subtly wrong teaches the wrong thing. Give each figure its own `<figure>` so page breaks fall between figures, not inside one.

Render:

```bash
node scripts/render.mjs OUTDIR/name.html OUTDIR/name.pdf
```

It prints the page count, and exits non-zero listing anything that failed to load and every formula KaTeX couldn't parse or left as raw text. Fix those and re-render until it is clean.

If `setup.sh` can't install (no npm or no network), use `https://cdn.jsdelivr.net/npm/katex@latest/dist` as `KATEX_DIST` and any headless Chromium for printing. If neither works, say so plainly rather than delivering a PDF with raw TeX in it.

## 5. Check

The textbook stays closed. Every check is against the recon file or your own mathematics.

- **Coverage.** Walk the coverage list; every bullet and every `example` entry is in the output, and each concept's share of explanation matches its coverage level. Nothing substantial is in the output that isn't on the list.
- **Key and testable.** Every key item has its badge and no other item does. Every theorem on the testable-proof list has both badges, and its proof matches the recon file's verbatim proof word for word.
- **Statements.** Each statement you wrote agrees with its verbatim record in hypotheses, conclusion, names and notation.
- **Maths.** Re-derive every formula and proof step you wrote. Check each worked example's arithmetic. A wrong intuitive rewrite is worse than no rewrite.
- **Figures.** Each picture is true to the map or object it claims to show: shape, labels, which lines go where.
- **First-time-reader pass.** Reread the draft top to bottom as a student new to the material. Wherever a step jumps (a symbol used before it's explained, a "clearly" that isn't clear, two new ideas in one sentence), fix it. Then reread as a student who *did* follow: cut anything that explains the obvious. Both passes matter; the second keeps it lean.
- **Look at it.** `pdftoppm -r 70 -png OUTDIR/name.pdf "$(mktemp -d)/out"`, view every page: maths typeset, boxes not split, diagrams correct and their labels clear of the drawing, tables not wrapping awkwardly, nothing overflowing the margins, no large blank gaps caused by an unbreakable figure.

## 6. Deliver

Report in a few lines: the paths of the PDF and its HTML, the page count, the path of the recon file, which items were marked Key and which proofs Testable, and any user override that was applied. If any statement in the output ended up close to the textbook's wording because of the verbatim record, say which. Nothing else unless something couldn't be done.
