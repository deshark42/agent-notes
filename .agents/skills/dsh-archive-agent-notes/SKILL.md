---
name: dsh-archive-agent-notes
description: Use when adding, auditing, pruning, archiving, restoring, or reviewing Agent Notes in deepseek-harness; checks every new note for superseded active records, deletes small UI and purely mechanical records, classifies other implemented notes by future decision value, deletes rejected notes that no longer prevent a tempting fallacy, and applies the frozen archived/{kind} and manifest rules.
---

# Archive DeepSeek Harness Agent Notes

Reduce the active decision corpus without erasing history that can still guide work. Judge every note semantically; word count and age are discovery aids, never archive criteria.

## Read the contracts

Read [the Agent Note rules](../../notes/README.md), [the archive instructions](../../notes/archived/AGENTS.md), and the applicable active lifecycle instructions before classifying. Use current code, configuration, package docs, generated catalogs, newer Agent Notes, and inbound links to establish whether a rationale still owns or constrains anything.

## Check supersession when adding a note

Before drafting, apply the [creation criteria](../../notes/README.md#when-to-write-one). Every new Agent Note triggers a scoped audit of active notes covering the same decision, mechanism, or rejected alternative. Classify each full or partial supersession while writing the new note: archive qualifying implemented notes in the same PR, retain and cross-link partial supersessions or independently useful rationale, reject obsolete proposals, and delete rejected notes that no longer prevent a plausible mistake. Apply the Agent Note consolidation rule when the new owner absorbs every unique proposition; do not defer a known match to a later corpus audit.

## Classify by future value

Apply these lifecycle-specific outcomes:

- **Implemented — delete:** delete notes that only describe small UI adjustments or purely mechanical changes, and repair or remove inbound links. Local bug fixes, performance changes, new capabilities, and substantive behavior or ownership decisions do not qualify merely because they are implemented. Apply this criterion before keep/archive classification.
- **Implemented — keep active:** retain a note when its rationale, alternatives, negative guarantees, durable/wire semantics, ownership boundary, security rule, or reintroduction condition is likely to guide a future change. Classify the decision, not the mechanics of its implementation: a mechanical rename or type extraction can still record lasting naming, compatibility, or ownership rules. Length does not matter.
- **Implemented — archive:** archive a substantive historical decision when it is complete and unlikely to guide future work, but its historical rationale still warrants preservation. Do not archive records that meet the direct-deletion criterion.
- **Proposed — never archive:** keep a live proposal active; if it is no longer worth pursuing, reject it with an honest reason and satisfy the rejected lifecycle format.
- **Rejected — keep only as a guardrail:** retain a rejection only when the losing proposal remains a tempting, meaningful mistake and the note explains why it loses.
- **Rejected — delete:** delete the note when the rejected idea is obsolete, superseded, no longer plausible, or unlikely to prevent re-litigation. Repair or delete inbound links.

Do not archive toward a quota. Inspect every note in scope, classify analogous groups under one principle, use best judgment for close cases, and record genuinely borderline decisions for the handoff.

## Calibrated examples

These examples set the bar; the word counts demonstrate that size is not the test.

Delete implemented notes such as:

- a collapsed UI control — 533 words: closed, minor UI behavior;
- a module ownership move — completed relocation and import rewiring with unchanged behavior.

Keep implemented notes such as:

- an event-sourced session store — 248 words: foundational authority and durability boundary;
- a single configuration-root resolver — 596 words: a cross-product ownership rule;
- project-scoped data directories — 628 words: durable storage and identity policy;
- parallel pre-push gates — 400 words: borderline, but still guides gate scheduling and resource tuning;
- a dropped content block — 334 words: keep until multimodal support lands, because it states the coordinated reintroduction condition.

For rejected notes:

- keep folding a package split — 426 words: the temptation to merge them remains meaningful;
- delete streaming progress through tool calls — 972 words: its protocol premise is obsolete;
- delete dropping terminal metadata — 362 words: a later, narrower decision resolved the question.

## Archive an implemented Agent Note

1. Move `foo.md` from `implemented/<kind>/` to `archived/<kind>/`; `implemented` is deliberately absent from the archive path.
2. Make no body edits. Insert only `Archived: YYYY-MM-DD` immediately below `Status: implemented`, using the archival date.
3. Search for inbound links from active prose. Redirect them to current authority, retarget them to the archived path only when the historical snapshot is intentionally cited, or delete them. Never verify or repair links out of the archived note.
4. Run `node scripts/verify-archived-agent-notes.ts --write`. Its append-only mode first proves every existing seal still matches, then adds only the new artifact hashes. Run the normal verifier afterward.

After the note is sealed, never edit, move, reformat, or delete it. Archived notes remain valid inbound-link targets but are historical snapshots, not authority for current behavior.

## Validate and report

Run `node scripts/verify-archived-agent-notes.ts`, `node scripts/verify-agent-note-format.ts`, `node scripts/verify-agent-note-classification.ts`, and `git diff --check`.

Report active implemented notes kept, implemented notes deleted or archived, rejected notes kept/deleted, proposed notes rejected if any, and every genuinely borderline case with its word count and chosen outcome. Do not claim archived outbound links are valid: the archive verifier intentionally never checks them.
