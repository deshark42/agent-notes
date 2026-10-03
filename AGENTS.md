# AGENTS.md

Agent Notes are agent-written RFCs that record the rationale and the rejected alternatives behind a durable decision. The contract for this repository is [.agents/notes/README.md](.agents/notes/README.md); work under `.agents/notes/` also follows [.agents/notes/AGENTS.md](.agents/notes/AGENTS.md).

## Repository layout

```
.agents/notes/README.md                                  the Agent Note contract
.agents/notes/{proposed,implemented,rejected}/<class>/   active notes
.agents/notes/archived/<class>/                          sealed artifacts + manifest.json
.agents/skills/archive-agent-notes/                      archive workflow
scripts/                                                 gates
```

## Commands

```sh
node scripts/verify-agent-note-classification.ts   # path and class
node scripts/verify-agent-note-format.ts           # header and required sections
node scripts/verify-archived-agent-notes.ts        # sealed archive; --write appends new seals
```

Run all three after touching `.agents/notes/` or `scripts/`.

## Language

`README.md` is Chinese. The other instruction files are English.
