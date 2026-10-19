---
name: pre-push-checks
description: Use before pushing, force-pushing, marking ready for review, or claiming checks pass on a branch in this repository, to select the smallest tests and checks that cover the outgoing or just-published diff without reflexively running the full repository suite.
---

# Pre-Push Checks

Use this skill to run relevant local evidence once before a push. CI owns exhaustive coverage and the platform matrix.

## Inspect the outgoing change

1. Confirm the checkout and branch.

```sh
git status --short --branch
git rev-parse --show-toplevel
```

2. Verify the live PR base, fetch that ref, and inspect the complete scope against it.

```sh
git fetch origin <base>
git diff --name-only <base>...
git status --short
```

Never guess or fetch a base; supply the ref verified from current remote or PR state. The report records committed paths relative to the resolved merge base, while staged, unstaged, and untracked paths describe the current worktree. After merging a changed base, rerun the report, reassess which behavior the combined scope can affect, and rerun only checks invalidated by the merge.

## Select relevant evidence

There is no universal local baseline. Every behavior change needs the narrowest available test or purpose-built check that would fail for its regression; add broader checks only for surfaces the diff actually reaches.

- **Documentation and Agent Notes:** run `npm run verify`.

Do not manually repeat a passing check merely because commit or push follows.

## Full local rehearsal

Run the complete local approximation only when the user explicitly requests it, while diagnosing a CI failure, or when the change spans the repository so broadly that no narrower set is credible. Use the current workflow and package scripts as the inventory.

## Protect history-rewriting pushes

Rebase is allowed for standalone PR branches, including after review. Before a standalone history rewrite, fetch the current remote branch and record its exact OID; publish with `--force-with-lease=<branch>:<observed-oid>` so a concurrent update aborts the push. Raw `--force` is never allowed.

After any rewritten push, fetch the live heads again and re-audit unresolved review threads, approvals, mergeability, and checks. Commit hashes and inline-comment anchors from before the rewrite are not current evidence.

## Handle failures

If a relevant check fails before an ordinary push, stop and fix or explain the blocker. Do not push and hope CI differs.

If a failure looks environment-specific, prove it:

- Record the exact command, failing test, and platform-specific mismatch.
- Confirm the relevant non-platform evidence.
- Prefer fixing cross-platform nondeterminism when the check is required.
- Bypass a local hook only when the user explicitly asks or agrees, and report exactly what failed and why CI is expected to differ.

## Push procedure

For ordinary and standalone rebase pushes:

1. Run the selected relevant checks once.
2. Commit normally and inspect any files changed by the pre-commit fixer before continuing.
3. Push normally, or use the exact lease for an authorized rewritten branch.
4. Verify the remote ref matches local `HEAD`.

```sh
git rev-parse HEAD origin/$(git branch --show-current)
```

For GitHub PRs, inspect remote CI after the push:

```sh
gh pr checks
```

Report pending checks as pending. Inspect failures before attributing them to the branch or the environment.

When `gh pr checks` reports "no checks reported" and `/actions/runs?head_sha=<sha>` returns `total_count: 0`, read mergeability before suspecting the push or a dropped GitHub event:

```sh
gh pr view <number> --json mergeable,mergeStateStatus
```

GitHub creates no `pull_request` workflow runs while a PR is `CONFLICTING`/`DIRTY`, so the absent signal is the conflict, not infrastructure. Resolving the conflict is the only fix; empty commits, `--allow-empty` pushes, draft/ready toggles, and revert-and-restore bounces all leave `total_count` at zero and add junk history. Confirm the conflicting paths with `git merge-tree --write-tree HEAD origin/<base>` when the branch cannot be merged locally yet.
