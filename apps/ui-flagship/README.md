# apps/ui-flagship/

The egzos.io flagship web UI.

## Stack (DECIDED, R3)

React + TypeScript + Vite + Tailwind + shadcn/ui.

The Vite app has not been scaffolded yet. `TODO(a4s)`: scaffold the Vite app per the first
approved spec committed by the Chief (Phase 3+). Do not scaffold before a spec exists.

## Design tokens dependency

The tokens file lives in `Egzos/egzos/spec/design/` (public). This application consumes it as a
dependency. The tokens are the identity layer: every catalogue component is re-themed to these
tokens before it enters the flagship. Inspiration flows through the tokens, never around them.

## Component sourcing rules

- **A2 decides, A4 installs.** Every component in a screen spec is named as a registry item
  (license noted) or `bespoke`. A4 never improvises a pick; a missing pick is a `design-gap`
  issue and the agent takes the next item.
- **Catalogue for chrome:** nav, tables, dialogs, forms, command palette, empty states, toasts.
  Source: shadcn/ui + 21st.dev picks (a4s-atelier carries the 21st CLI with `API_KEY_21ST`; skills vendored under `.github/skills/21st/`).
- **Bespoke for the differentiators:** onion graph, drag-drop gate, triage flow, permissions
  matrix. Owner: a4g-atelier.
- **Vendored via PR:** a4s installs picks with the shadcn CLI into `src/components/` and COMMITS
  the source, re-themed to the egzos tokens. Never fetched at build time.
- **Provenance:** every pick is recorded in `Egzos/egzos/spec/design/DESIGN-SOURCES.md`.
- **Security surfaces** (pending review, step-up tap, consent) are bespoke and A6-reviewed even
  when assembled from catalogue primitives.

## Path ownership

| Path | Owner |
|---|---|
| `apps/ui-flagship/**` (excluding bespoke/) | a4s-atelier |
| `apps/ui-flagship/src/bespoke/**` | a4g-atelier (exclusive) |

## Status

Pre-alpha. No code yet. Agents are queue-driven from Phase 6.
