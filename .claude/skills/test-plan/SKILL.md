---
name: test-plan
description: Turn requirements or a phase plan into a concrete test plan — every requirement mapped to test cases with level, data and expected result, before or alongside implementation. Use for "test plan", "what should we test", after Sora's PLAN.md.
---

# Test Plan

Input: `REQUIREMENTS.md`, a `[phase]-PLAN.md` or a design doc. Output: `.planning/[phase]-TEST-PLAN.md` in the project repo.

## Process

1. **Extract requirements** as numbered, testable statements (R1, R2…). Flag any that cannot be tested as written and ask Sora/Yone to sharpen them.
2. **For each requirement**, list cases across: happy path · boundaries · invalid input · empty · failure of dependencies · retry/idempotency · persistence · security-relevant (hand to Rei if non-trivial).
3. **Pick the cheapest level that proves it:** unit (pure logic) → integration (real boundary) → end-to-end (real system via `/verify` feature map). Do not push everything to e2e.
4. **Specify data and expected result** precisely enough that Haru or a script can implement it without asking.
5. **Mark automation:** automated (which suite) or manual (why, and the evidence to capture).

## Output format

```
# <phase> — Test Plan
| Req | Case | Level | Data / setup | Expected | Auto? |
Untestable as written: <R# — why — question>
Out of scope: <what and why>
```

Then `/talk-to haru "test plan ready: <path>"` and run `/nyquist` once tests exist.
