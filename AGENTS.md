# AGENTS.md

Agent Notes are agent-written RFCs that record the rationale and the rejected alternatives behind a durable decision. The contract for this repository is [.agents/notes/README.md](.agents/notes/README.md); work under `.agents/notes/` also follows [.agents/notes/AGENTS.md](.agents/notes/AGENTS.md).

## Repository layout

```
.agents/notes/README.md                                  the Agent Note contract
.agents/notes/AGENTS.md                                  rules for writing notes
.agents/notes/{proposed,implemented,rejected}/<class>/   active notes
.agents/notes/archived/<class>/                          sealed artifacts + manifest.json
.agents/skills/archive-agent-notes/                      archive workflow
.agents/skills/pre-push-checks/                          check selection
scripts/                                                 gates
```

## Commands

```sh
npm run verify   # all Agent Note gates: path/class, header/sections, sealed archive
```

The gates execute TypeScript directly, so Node 22.18 or newer is required. `package.json` owns the script inventory.

## Run relevant checks locally

Before pushing, follow [pre-push-checks](.agents/skills/pre-push-checks/SKILL.md); report only the commands run.

- Match evidence to the surface: `npm run verify` for changes under `.agents/notes/` or `scripts/`, the [archive workflow](.agents/skills/archive-agent-notes/SKILL.md) for archiving, and no gate for instruction or metadata changes.
- Never default to the full suite or repeat a passing check for commit or push. CI, once configured, owns exhaustive coverage.
