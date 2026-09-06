# apps/ui-flagship/src/bespoke/

Bespoke components — the differentiators of the egzos flagship UI.

**Owner:** a4g-atelier (exclusive path; no other agent writes here without the Chief's exception).

## What is bespoke and why

These components are the product. They require judgment, precision, and deep familiarity with the
egzos trust and data model. They are never sourced from a catalogue.

| Component | Why bespoke |
|---|---|
| Onion graph | The visual representation of egzos rings of trust; the data model and interaction are novel |
| Drag-drop gate | The primary trust-assignment surface; drag physics + trust semantics require bespoke implementation |
| Triage flow | The inbox and proposal review surface; close coupling to the audit and approval model |
| Permissions matrix | The permissions overview; custom data shape and interaction |
| Step-up integration | A security surface; the step-up tap + consent flow must be bespoke and A6-reviewed |

Security-surface components (the step-up integration, consent screen, pending-review flow) are
bespoke and A6-reviewed even when assembled from catalogue primitives. A stock dialog wrapping the
step-up flow is still a security surface.

## A6-reviewed

Every commit to this directory is reviewed by a6-adversary (the `security` label on PRs triggers
the A6 check). Do not add components here without that label.

## Status

No code yet. a4g-atelier is queue-driven from Phase 6, after A2's approved bespoke-component specs
are committed by the Chief.
