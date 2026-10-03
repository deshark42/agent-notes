# AGENTS.md

This repository carries the Agent Notes governance contract and its gates, not the Agent Notes themselves: an Agent Note is an agent-written RFC recording the rationale and the rejected alternatives behind a durable decision.

Read [.agents/notes/README.md](.agents/notes/README.md) for the contract; work under `.agents/notes/` also follows [.agents/notes/AGENTS.md](.agents/notes/AGENTS.md).

## Repository layout

```
.agents/notes/README.md                                  the Agent Note contract
.agents/notes/{proposed,implemented,rejected}/<class>/   active notes
.agents/notes/archived/<class>/                          sealed artifacts + manifest.json
.agents/skills/dsh-archive-agent-notes/                  archive workflow
scripts/                                                 gates
```

## Commands

```sh
node scripts/verify-agent-note-classification.ts   # path and class
node scripts/verify-agent-note-format.ts           # header and required sections
node scripts/verify-archived-agent-notes.ts        # sealed archive; --write appends new seals
```

Run all three after touching `.agents/notes/` or `scripts/`.

## Vendoring

`source` holds a filtered snapshot of upstream DSH governance files; every local change belongs on `master`, merged with `git merge source`.

- Upstream's `.agents/notes/README.zh.md` maps to `.agents/notes/README.md` here. Upstream's English `README.md`, `README.i18n.yaml`, and the note corpus are not carried.
- On merge, drop content specific to DSH: note titles, the bilingual pairing model, and per-note seal exceptions.
- `README.md` is Chinese; this file, the vendored `AGENTS.md` files, and `SKILL.md` are English.
