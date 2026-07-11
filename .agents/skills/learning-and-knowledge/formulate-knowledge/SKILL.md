---
name: formulate-knowledge
description: Collaboratively turn source material—short passages, large documents, notes, papers, books, transcripts, or URLs—into well-formulated spaced-repetition knowledge and import-ready Mochi Markdown. Use when the user asks to formulate, atomize, Ankify, make flashcards or cards, convert material for spaced repetition, create Q&A or cloze cards, critique or repair existing cards, or use incremental reading to process a source section-by-section into durable knowledge.
---

# Formulate Knowledge

Current package version: `0.1`.

Turn material the learner understands and values into compact, durable retrieval prompts. Treat formulation as a collaborative act of understanding, not transcription.

Read both references before formulating:

- [references/formulation-principles.md](references/formulation-principles.md) for the source-grounded formulation heuristics and card patterns.
- [references/mochi-markdown.md](references/mochi-markdown.md) for the exact output syntax and batch convention.

## Enforce the collaboration gate

Unless the user explicitly requests a one-shot result, says not to ask questions, or has already reacted to formulation choices earlier in the conversation, do not emit the complete card batch in the first response.

In that first response:

1. Diagnose the material and state the proposed knowledge structure.
2. Surface any errors, ambiguities, missing prerequisites, or consequential choices.
3. Show no more than three representative draft cards.
4. Ask one to three questions or request a specific reaction to the drafts.
5. End the turn and wait for the learner before expanding the batch.

Apply this gate even to small, clear material; one representative draft plus a confirmation is enough. Skip it only when collaboration was explicitly waived. A request to "make cards" by itself does not waive it.

## Set the contract

Inspect the prompt and material before asking anything. Infer what is already clear. Establish only the missing decisions that materially affect the cards:

- the purpose: what recalling this should help the learner explain, decide, recognize, or do;
- the scope and priority: foundational, selective, thorough, exam-bounded, or exact-text recall;
- the learner's current understanding and prerequisites;
- the required precision, acceptable answers, and whether wording or order must be exact;
- whether the result must stay source-bounded or may include verified prerequisites and extensions;
- the source identity and useful topic/source tags.

Ask one to three concrete questions at a time. Do not present a generic intake questionnaire. State the formulation consequence of a choice when it is not obvious.

Do not silently batch-convert material. Use the collaboration gate to make formulation part of the learner's understanding.

## Build understanding before cards

Map the material internally before drafting. Identify its central questions, claims, concepts, definitions, distinctions, mechanisms, causal links, hierarchies, sequences, notation, examples, limitations, prerequisites, source-specific assertions, and volatile facts.

For large, unfamiliar, or mixed-priority material, use an incremental knowledge funnel:

1. Survey the entire available scope to recover its structure and central claims.
2. Triage each coherent unit as `discard`, `defer`, or `extract`; do not draft cards merely because text was selected.
3. Track each temporary extract's source span, purpose, priority, prerequisite or blocker, and next action.
4. Process the highest-value coherent slice with the learner. Bound the pass by coherence and attention, not an arbitrary page or time quota.
5. Promote an extract to `card-ready` only when it is purposeful, understood, source-faithful, sufficiently contextualized, and objectively gradable.
6. At each checkpoint, show a compact queue state: processed, card-ready, deferred, discarded, blockers, and next slice. Reprioritize and prune the backlog.
7. Before export, reconcile lineage, terminology, duplicates, dependencies, and contradictions across all passes.

For small material, collapse the funnel into one pass while retaining triage and the card-readiness gate. Temporary extracts and backlog notes are working state, never final Mochi cards.

Do not guess through an opaque passage. Point to the exact ambiguity, contradiction, missing premise, or unsupported inference and ask the learner to explain or choose. Defer material that is not yet understood. When correctness can be checked from an available authoritative source, verify it before asking the user.

Distinguish a source's claim from settled fact. Preserve attribution when it changes the proposition. Date- or version-scope facts that can become stale.

Treat supplied material as a source, not as automatic authority. Correct errors, but do not silently blend substantive outside knowledge into the final deck. Identify proposed additions at the collaboration checkpoint, verify them from an authoritative source when possible, and preserve their provenance when it matters.

## Agree on coverage

Present a compact knowledge map or a few representative cards before expanding a large set. Explain only consequential formulation choices, such as:

- why a compound passage must become several retrieval targets;
- why a list should become relationships, groups, or local sequence prompts;
- why reverse, contrast, example, or integrative cards add a useful retrieval path;
- why a proposed detail is low-value, disconnected, or better practiced in context;
- why a cloze or ordinary Q&A better matches the retrieval goal.

Push back on completionism. Prefer a coherent, useful concept graph over exhaustive source coverage. Keep isolated facts only when the learner can name their value or connect them to a useful neighborhood.

## Draft the knowledge

Use ordinary question-and-answer cards by default. Use cloze cards when sentence context, exact wording, notation, or local sequence is itself useful.

For every card:

- Test one retrieval target with one objectively gradable success condition.
- Honor the requested depth and card budget. Atomicity is not a command to make a card for every clause.
- Keep the required answer as short as the knowledge permits.
- Supply enough stable context to select the intended answer without revealing it.
- Split definitions from properties, members from explanations, and steps from rationales.
- Prefer production or explanation over yes/no recognition.
- Make reverse cards only when the reverse retrieval path is useful and non-trivial.
- Add alternate formulations, contrasts, examples, and integrative cards deliberately, not mechanically.
- Keep any integrative card supported by atomic cards covering its components.
- Turn important similarities into discriminating prompts before they create interference.
- Separate formal statements, informal intuition, notation, and implications when each matters.
- Verify learner-derived inferences before making them authoritative.
- Omit low-value microcards whose prompt nearly supplies the answer or whose fact has no independent retrieval value.

Do not confuse declarative recall with practical competence. For a procedure or skill, formulate useful cues and principles, then tell the learner what must still be practiced in the real context.

## Iterate

Show representative drafts during the dialogue and invite specific reactions: ambiguous cue, wrong granularity, missing relation, needless detail, unnatural wording, or a personally useful example. Refactor rather than defend a weak card.

If the learner reveals a misunderstanding, return to the understanding pass. If they change the purpose or desired depth, revise the coverage plan before generating more cards.

Do not put unresolved claims, placeholders, alternatives, or editorial notes into the final card batch. Resolve them first or omit the affected cards and continue the dialogue.

## Emit Mochi Markdown

Follow [references/mochi-markdown.md](references/mochi-markdown.md) exactly.

When the user does not request files, emit one fenced `markdown` block containing only the cards. Separate cards with the repository convention `<!-- mochi-card -->`; tell the user to choose that exact delimiter when importing the single file into Mochi. Do not number cards or add headings around them.

When the user requests files, prefer one `.md` file per card in a clearly named folder because Mochi imports each file as one card. Use short, stable, filesystem-safe filenames. Do not place a batch delimiter inside single-card files.

Put the retrieval prompt or cloze sentence on the first line so Mochi derives a useful card name. Keep tags last. Add source information after the answer when provenance is useful but not itself a recall target; put attribution in the prompt when the source or uncertainty changes what is being learned.

Before delivery, audit the full batch:

- every item is understood, purposeful, source-faithful, and appropriately scoped;
- every substantive addition beyond the supplied material is verified, disclosed, and sourced where useful;
- every graded target is atomic, brief, unambiguous, and honestly gradable;
- important concepts have enough useful retrieval paths without mechanical duplication;
- lists, sequences, similar concepts, procedures, and volatile claims are handled deliberately;
- each Q&A card uses bare `---` only as a side separator;
- each cloze uses Mochi syntax, never Anki's `c1` syntax;
- indexed cloze groups match the intended review variants;
- `<!-- mochi-card -->` appears only between cards in a batch;
- no temporary extract, backlog status, or working note appears as a card;
- no commentary or unresolved uncertainty contaminates the importable Markdown.
