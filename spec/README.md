# spec/

`spec/` holds the platform's design specs and planning artifacts. This directory is private
(the repository is private throughout).

## Layout

```
spec/
  design/    — flagship screen specs; committed by the Chief after A2 direction sessions
```

## design/

Per R10 (decisions log §N), flagship screen specs live here — closed product, closed specs.

Each screen spec names every component as a registry item (license noted) or `bespoke`. The Chief
commits each approved spec; the commit is the approval act. Agents build from committed specs only.

The shared design system (tokens file, DESIGN-PRINCIPLES.md, DESIGN-SOURCES.md, lifeboat spec,
step-up tap + pending-approval specs) lives in the public sibling `Egzos/egzos/spec/design`.
The platform depends on the tokens file across repos; it consumes it, never forks it.

## What does not live here

- Container contracts: `Egzos/egzos/spec/contracts/` (public; frozen after Phase 0.3)
- Design system, principles, sources, lifeboat spec, step-up tap spec: `Egzos/egzos/spec/design/`
- Agent charters: `.claude/agents/` in each repo
