# Mochi Markdown Contract

Follow the syntax documented in Mochi's [basic formatting](https://mochi.cards/docs/markdown/basic-formatting/) and [advanced formatting](https://mochi.cards/docs/markdown/advanced-formatting/). Mochi's [card overview](https://mochi.cards/docs/cards/) and [Markdown import guide](https://mochi.cards/docs/import-and-export/importing/) clarify multi-side cards and batch imports.

## Contents

- [Ordinary cards](#ordinary-cards)
- [Cloze cards](#cloze-cards)
- [Tags and supporting Markdown](#tags-and-supporting-markdown)
- [Multiple-card output](#multiple-card-output)
- [Final syntax audit](#final-syntax-audit)

## Ordinary cards

Use a line containing exactly three dashes to separate card sides:

```markdown
What process converts light energy into chemical energy?
---
Photosynthesis.
```

Bare `---` separates sides, never cards. Repeating it creates three or more sides. Prefer two sides unless successive reveal is intentional.

Use four or more dashes for an ordinary horizontal rule:

```markdown
----
```

Each side may contain multiple lines of Markdown. Keep the first line meaningful because Mochi derives an untemplated card's name from it.

## Cloze cards

Use double braces for a simple deletion:

```markdown
The process that converts light energy into chemical energy is {{photosynthesis}}.
```

All simple deletions on one card are hidden together.

Use numeric groups when one card needs independently scheduled variants:

```markdown
{{1::ATP}} supplies readily usable energy, while {{2::DNA}} stores hereditary information.
```

Spans with the same number hide together:

```markdown
The {{1::left atrium}} receives {{2::oxygenated blood}} from the {{2::pulmonary veins}}.
```

Use Mochi's `{{1::answer}}` form. Never emit Anki's `{{c1::answer}}` form. Mochi's cited documentation does not define cloze hints, so do not invent hint syntax.

A cloze card does not need a `---` side separator.

## Tags and supporting Markdown

Inline tags become Mochi metadata:

```markdown
#biology #source/campbell/chapter-10
```

Use `/` for hierarchical tags. A heading contains whitespace after `#`; a tag does not. Put tags after the answer or at the end of a cloze card, not on the first line.

Use standard Markdown for emphasis, lists, code, links, images, math, and footnotes. Avoid extra formatting that does not improve the retrieval cue or revealed explanation.

The cited Mochi pages do not document escaping for its special tokens. Do not claim that a backslash or code fence suppresses Mochi parsing. Avoid a literal bare `---` line or literal `{{...}}` construct unless it is intended Mochi syntax; flag source material that genuinely requires either literal form for user verification.

## Multiple-card output

Mochi has no fixed inter-card Markdown delimiter. It can import either:

- a folder of Markdown files, with one file per card; or
- one Markdown file split on a delimiter chosen during import.

Use this repository convention for a single-file batch:

```markdown
What is the capital of Japan?
---
Tokyo.
#geography

<!-- mochi-card -->

Which city is Japan's capital?
---
Tokyo.
#geography
```

Tell the user to enter `<!-- mochi-card -->` as Mochi's import delimiter. This comment is a batch convention, not native Mochi card syntax. Emit it only between cards, with a blank line on both sides. Never use `---` as an inter-card divider.

For one-file-per-card output, omit `<!-- mochi-card -->` entirely.

## Final syntax audit

- Start each card with its prompt or cloze sentence.
- Use exactly one intentional bare `---` in an ordinary two-sided card.
- Use no bare `---` in a normal cloze card.
- Use only Mochi cloze syntax.
- Give independent cloze targets different indices; use a shared index only when they should hide together.
- Put inline tags last.
- Put the batch delimiter only between complete cards.
- Ensure the delimiter string does not occur in source content.
- Keep fences around the overall batch outside the content that will be imported.
