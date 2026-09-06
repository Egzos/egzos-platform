# spec/design/

Flagship screen specs. Private — this repository is private throughout.

Per R10, flagship screen specs live here; the shared design system (tokens, principles, sources,
lifeboat spec, step-up tap + pending-approval specs) lives in the public sibling
`Egzos/egzos/spec/design`.

## How specs arrive

A2 (studio mode on Hyperagent) runs direction sessions with the Chief → produces direction boards
→ Chief picks → A2 writes the binding spec → Chief commits it here. The commit is the approval.
Agents build only from committed specs. A missing spec is a `design-gap` issue; agents never
improvise design decisions.

## Screens in build order

Each entry becomes a spec file committed by the Chief. `TODO(a2)` marks screens not yet specced.

- `TODO(a2)`: search / list screen
- `TODO(a2)`: permissions dashboard
- `TODO(a2)`: pending review with previews
- `TODO(a2)`: onion graph
- `TODO(a2)`: drag-drop gate
- `TODO(a2)`: triage flow
- `TODO(a2)`: permissions matrix
- `TODO(a2)`: step-up integration screen

Each spec names every component as a registry item (with license) or `bespoke`, and provides the
component picks for A4 to install via the shadcn CLI (vendored into the components directory and
committed, never fetched at build time).

Security-surface screens (pending review, step-up integration) are bespoke and A6-reviewed even
when assembled from catalogue primitives.
