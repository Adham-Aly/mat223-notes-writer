# Recon brief

You are doing the recon pass for a rewrite of one chapter of a linear algebra course's lecture-notes textbook. Look through the PDF pages you were assigned and return three things, in this order:

1. **The coverage list:** a bare list of **what the pages cover**, by name, followed by two fixed subsections, `### Explanation coverage` and `### Examples`, recording how much the textbook gave each concept (a level and a type, never content).
2. **The verbatim statements:** every formally stated definition, theorem, proposition, lemma and corollary on your pages, copied word for word.
3. **The verbatim proofs:** the proof of each theorem whose proof your task marks *testable*, copied word for word. "Testable" describes the proof, not the theorem; every such theorem is also a key item.

Someone else writes the new document from your reply alone. They will never see the textbook, on purpose: the new document has to be written from scratch out of the writer's own knowledge of the mathematics, and any wording, example or structure that leaks through from the textbook pulls the writing toward a paraphrase of it. So apart from parts 2 and 3, your reply must carry names and nothing more. It is not a summary, not a condensed version, not notes on the material. Think of the index at the back of a book, arranged in the order things appear.

Two things follow:
- **Too much on the list spoils the document.** A formula or an explanation in the coverage list becomes the textbook's voice in the output.
- **Anything missing is lost for good.** Nobody goes back to the textbook after you. Completeness is your responsibility.

## Looking at the pages

Look only at the pages assigned to you, except where a testable proof or a back-reference lookup requires otherwise (below). Stop before the end-of-chapter exercises; they are never covered.

**Look, never extract.** Do not use OCR, `pdftotext`, the PDF's text layer, or any other text extraction. Extraction mangles formulas and silently drops diagrams, tables, margin notes and colour. Rasterize into a temp directory of your own and view every page, one at a time, as an image:

```bash
WORK=$(mktemp -d)
pdftoppm -r 150 -f FIRST -l LAST -png INPUT.pdf "$WORK/pg"
```

View each `pg-*.png` in order with your image-viewing tool; skip nothing except blank pages. If small print is hard to read, re-render that page at `-r 300`. For a long range, work in batches: view a run of pages, add to the list, continue.

## Part 1: the coverage list

Markdown bullets under plain topic headings, in the order the pages cover things. One bullet per thing covered, each starting with its kind:

```
## Matrix-vector product
- definition: matrix-vector product (as a linear combination of the columns) **[KEY]**
- example type: computing matrix-vector products
- example type: deciding when a matrix-vector product is defined (size compatibility)
- remark: sizes for an m×n matrix (input in R^n, output in R^m)

## Linearity
- definition: linear function between Euclidean spaces (additivity and homogeneity)
- figure: a line in R^2 given by a point vector and a direction vector
- theorem: Linearity of Matrix Transformations (with proof, by direct computation) **[KEY]**
- theorem: Defining Matrix Theorem (with proof, by expanding in the standard basis) **[KEY]** **[TESTABLE]**

### Explanation coverage
- coverage: matrix-vector product — brief
- coverage: linear function — extensive
- coverage: Linearity of Matrix Transformations — none (statement only)
- coverage: Defining Matrix Theorem — moderate

### Examples
- example: matrix-vector product received an example showing a routine numeric computation (several)
- example: linear function received an example showing a map checked against the definition (several)
- example: Defining Matrix Theorem received an example showing a geometrically described map's matrix found from the images of the standard basis
```

Kinds: `definition`, `theorem` (also for propositions, lemmas, corollaries), `formula`, `method`, `example type`, `figure`, `caution`, `remark`.

Rules:
- **Names only.** Each bullet is a name or a short identifying label of a few words. Never the statement, the formula itself, a proof, a worked solution, or any explanation. "matrix-vector product", not the definition.
- **Coverage, not pedagogy.** List the mathematics being taught, not how the textbook teaches it. Its intuition, analogies, motivation, mental pictures, opening framing and "idea of the proof" comments are its explanation: they get no bullet at all. Do not note how something is presented ("set as an activity", "boxed", "proof required"). A `remark` is a mathematical fact noted in passing, named like any other result. An `example type` is a problem actually worked for the reader (including in-text activities), not an illustrative aside.
- **Qualifiers identify, they don't state.** A qualifier says which result is meant ("for sequences", "epsilon-N"). It does not spell out hypotheses or conclusions.
- **The index test.** Could the bullet sit in a book's index? If it reads like a sentence that teaches something, cut it down to the name.
- **Unnamed results get a label saying what they are about**, not what they say: "uniqueness of the defining matrix", "linear maps send lines to lines or points".
- **Be unambiguous.** A mathematician reading the bullet must know exactly which result, definition or technique is meant. Add a qualifier when the bare name could mean several things.
- **Use the textbook's name for a thing** when it has one ("Defining Matrix Theorem"); otherwise the standard name.
- **Theorems:** add `(with proof)` or `(stated without proof)`. If the proof's technique has a name of its own, add it in a few words.
- **Examples are listed by type**: the kind of problem and the technique it shows. Not the specific problem, its numbers, or its solution. Several examples of the same type make one bullet.
- **Figures are listed by what they depict**, in a phrase. Not a description of how the textbook drew it.
- **Nothing that locates or echoes the textbook.** No page, section, chapter, theorem, definition or example numbers. No copied headings: write your own plain topic headings. No quotations.
- **Chapter summaries and recaps** get no bullets. If the summary states a fact that appears nowhere else on your pages, list that fact once under the topic it belongs to.
- **Leave out what isn't mathematics being taught:** logistics, history asides, digressions, repetition, exercise sets.
- **Key and testable tags.** Your task names the key items, and the key theorems whose proof is testable, by the textbook's numbering, so that you can find them. Tag exactly the key bullets `**[KEY]**`, and those with a testable proof `**[KEY]** **[TESTABLE]**`. Tag nothing else, however important it looks. Never write the numbers in your reply.
- **Miss nothing.** Every definition, theorem, formula, method, type of example, content-carrying figure, caution and remark on your pages gets a bullet.

### Explanation coverage

At the end of your coverage list, under `### Explanation coverage`, one entry per concept, definition, theorem, formula or method, stating only how much explanatory text the textbook gave it, so the new document can give it a similar share:

```
- coverage: <concept name> — extensive | moderate | brief | none (statement only)
```

Never, under any circumstances, include a sample, a quotation or a paraphrase of the explanation, nor say what it says, how it argues, what analogy or picture it uses. The level is the whole entry.

### Examples

Under `### Examples`, one entry per concept that received a worked example or activity, in exactly this shape:

```
- example: <concept> received an example showing <problem type / setup / scenario nature>
```

Add "(several)" if more than one example of that type was given. Never name individual examples, never list them one by one, never give their matrices, numbers, maps or solutions. "showing a map checked against the definition of linearity" is right; the actual map is not. These entries sit alongside the `example type` bullets in the list: the bullets say what is worked, in order; these entries say which concept each example serves and how many there were.

## Part 2: the verbatim statements

Under the heading `## Verbatim statements`, in source order, copy every formally stated definition, theorem, proposition, lemma and corollary on your pages, word for word as the textbook gives it, typeset in LaTeX (`$...$` inline, `$$...$$` display, `\begin{bmatrix}` for matrices, `\vec` where the textbook uses arrows). Plain `&` in LaTeX, never `&amp;`.

- **Strip the numbering.** Head each one `**Definition (Name).**`, `**Theorem (Name).**`, `**Proposition (Name).**`, etc., using the textbook's name for the item if it has one, and a short descriptive name otherwise. Never `Definition 4.2`.
- **Replace back-references.** If the statement refers to another numbered item ("by Definition 3.1", "the basis of Theorem 2.7"), replace the number, drop-in, with that item's name ("by the definition of the standard basis", "the Rank–Nullity Theorem"). You may look up a referenced item elsewhere in the PDF solely to learn its name. Change nothing else.
- **Tags.** Append `[KEY]` and `[TESTABLE]` to the heading of the tagged items, matching Part 1.
- Only formally stated, boxed or labelled results. Not remarks, not in-text observations, not examples.

## Part 3: the verbatim testable proofs

Under the heading `## Verbatim testable proofs`, with a `### Name` subheading per theorem, transcribe each testable proof word for word and line for line as the textbook gives it, in LaTeX, including the displayed equations exactly as displayed (use `aligned` for multi-line derivations, keeping the textbook's line breaks and side comments such as "by linearity of F").

- **Replace back-references** exactly as in Part 2: every "Definition 3.1", "Proposition 4.6", "equation (2)" style reference becomes the name of the thing referenced. Change nothing else.
- **Keep the closing square** or end mark as the textbook gives it.
- If the proof starts before your first page or runs past your last page, read the neighbouring pages as needed to capture the whole proof. A proof is never left incomplete.
- If a theorem whose proof your task marks testable does not appear on your pages (or the neighbouring pages reached by the previous rule), say so in one line at the end of your reply, so it can be retrieved from the other part.

## Before you finish

1. Go through your pages once more against the list and add anything missing.
2. Reread the coverage list against the rules and cut every bullet back to a name. Check that exactly the key items are tagged, that coverage entries carry only a level and example entries only a type, and that no numbering survives anywhere in your reply.
3. Compare each verbatim statement and proof against the page image once more, symbol by symbol.
4. Delete your temp directory (page images).
5. Reply with the three parts and nothing else: no summary of the material, no commentary, no description of the textbook. If your part starts or ends mid-topic, just list what is on your pages; the replies get merged afterwards.
