# Feature map template

Create `.claude/skills/verify-<project>/` in the project repo:

```
verify-<project>/
  SKILL.md                    # launch, doctor, drive, evidence, cleanup for THIS project
  references/features/
    README.md                 # index (one line per feature), baseline preconditions, full-sweep order
    <feature>.md              # one file per feature area
```

## SKILL.md must answer
- How to start / reach the system (local, staging, prod read-only)
- Doctor: how to prove the instance is fresh and correct (versions, config, env, account)
- Command surface to drive it (CLI, curl, UI driver), with examples
- Evidence capture commands
- Cleanup

## Every `<feature>.md` uses exactly these four H2s

```markdown
# <Feature>

<one paragraph: what it is>

## Sub-features
- name: one line each

## How to get to it (user POV)
Trigger(s) a real user or system uses.

## Driving it
Exact commands; expected observable end state for success / error / empty / persistence.

## Gotchas
What lies: misleading signals, flaky waits, state that must be reset.
```

Keep entries behavior-level and short enough to run without reading source. Split a file when a section needs its own preconditions.
