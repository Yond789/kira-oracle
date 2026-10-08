---
name: nyquist
description: Nyquist validation — check that every behavior has an automated test that runs on every change, so regressions are sampled often enough to be caught. Use for "nyquist", "coverage gaps", "is every requirement tested".
---

# Nyquist Validation

Principle: if a behavior exists, an automated check for it must exist **and run at least as often as the code changes** (every commit / CI run). A test that only runs manually is under-sampling.

## Process

1. **Behaviors:** list them from REQUIREMENTS / PLAN / TEST-PLAN and from the code's public surface (CLI commands, endpoints, scheduled jobs, UI flows).
2. **Tests:** list automated tests (`grep` test names, read test files). Map each test to the behavior(s) it actually asserts. Read assertions; a test that runs code without asserting the behavior does not count.
3. **Sampling:** for each test, where does it run? (pre-commit, CI on every PR, nightly, manual). Check CI config.
4. **Gaps:**
   - Behavior with no test → **gap**
   - Test that does not assert the behavior → **false coverage**
   - Test not in CI → **under-sampled**
   - Test with no matching behavior → **orphan** (candidate to archive)

## Report

```
## Nyquist — <project> — <n>/<m> behaviors covered
| Behavior | Test (file:line) | Asserts? | Runs in | Status |
Gaps (priority order): ...
```

Kira may write the missing tests (test files only). Send implementation-side issues to Haru.
