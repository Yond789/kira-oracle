---
name: verify
description: Kira's QA gate. Prove a change works by driving the real system against the project's feature map, with evidence, then report pass/fail with file:line. Use for "verify", "QA this", "does it work", after Haru ships.
---

# Verify

Adapted from poteto/verification-skill-example. **Proof bar:** a skeptical reviewer must accept the evidence. "It compiles", "tests pass" and "look, it opens" are not proof on their own.
Kira never modifies implementation code: read, run, write tests, report.

## 1. Load the project's verification kit

In the target repo look for, in order:
1. `.claude/skills/verify-<project>/SKILL.md`: how to launch, check health ("doctor") and drive this project
2. `.claude/skills/verify-<project>/references/features/README.md`: the feature map (one file per feature area)

If either is missing, **stop and propose creating it** with `references/feature-map-template.md`. Verifying without a map means guessing the coverage set.

## 2. Scope

- What changed: `git diff <base>...HEAD --stat`, the PR, or Haru's message.
- Requirements: the PLAN / REQUIREMENTS / design doc the change implements (goal-backward: check against intent, not just "it runs").
- Match the change to feature files. Those files define the **coverage set**.

## 3. Freshness ("doctor") before anything

Run the project's health check. Evidence from a stale build, the wrong environment or the wrong account is not evidence.

## 4. Drive the real paths

For each feature file touched, exercise every reachable entry point and the paths the change can affect:
**success · cancel · error · empty · persistence (reload / re-run) · retry / idempotency**.

- Use the production path (real CLI, real endpoint, real UI). Use inspection (`eval`, DB reads, logs) only to check state *after* the user path ran.
- Mocks count only behind the same boundary a real outage would use.
- Wait on observable end states, not fixed sleeps.

## 5. Evidence

Per path: the command or action, and the observed end state: output, file contents, DB row, screenshot, log line, a reload round-trip. Verify side effects, not just exit codes.

## 6. Skips

Name every path not covered, why (account, OS, cost, external system), and the closest real path you did cover. A silent skip counts as a fail.

## 7. Report

```
## Verify — <project> — <change> — PASS | FAIL | PARTIAL
Requirements checked: <doc>
| Feature / path | Result | Evidence | file:line (on fail) |
Skipped: <path — reason — nearest covered path>
Map drift: <feature files that no longer match reality>
```

- PASS → `/talk-to yumi "cc: QA passed — <change>"`
- FAIL → `/talk-to haru "QA issues: <list with file:line>"`
- Map drift → fix the feature file in the same pass (it is test code, not implementation).
