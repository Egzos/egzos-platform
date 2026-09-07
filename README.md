# egzos-platform

**Private. Proprietary. See LICENSE.**

egzos-platform is the closed, paid side of egzos: the flagship web UI (egzos.io), hosted containers,
previews, relay, metering, billing, and sessions.

## What this repository is

The egzos-platform is split from the open core (`Egzos/egzos`) on the **container contract**. This
repository is that contract's first external client. It consumes the container only over the wire,
exactly as any fork's UI would. A gap in the contract is an escalation to `Egzos/egzos` (label
`contract-change`), never a private shortcut — that is the point of the split.

**egzos.io holds no container token, ever.** The egzos.io session (Firebase-as-identity behind the
abstraction) proves subscription. The container token proves authorization and is obtained by the
browser via PKCE against the user's own container, held browser-side. Those are two separate
authorities, deliberately. BYOC intelligence runs in-container; the data plane never touches
egzos servers.

## Three identities, three jobs

Agent PRs are opened under the `egzos-forge` App. Approvals come only from the Chief or the
`chief-proxy` App. The default Actions token comments and nothing more.

## Stack

**Flagship UI (DECIDED, R3):** React + TypeScript + Vite + Tailwind + shadcn/ui.
**Server side:** `TODO(chief)` — stack chosen with A9's proposal when Phase 6 opens.

## Where specs live (R10)

- Flagship screen specs: `spec/design/` here (closed, private).
- Design system, tokens file, DESIGN-PRINCIPLES.md, DESIGN-SOURCES.md, lifeboat spec, step-up tap
  + pending-approval specs: `Egzos/egzos/spec/design` (public open-core surfaces).

The tokens file is published in `Egzos/egzos/spec/design`; this repo consumes it as a dependency,
never forks it.

## Layout

```
apps/
  ui-flagship/         — the flagship React + TypeScript web UI
server/
  egzos_platform/
    billing/           — subscription proof, billing stub (a9f-landlord, exclusive)
    sessions/          — session checks, Firebase abstraction (a9f-landlord, exclusive)
spec/
  design/              — flagship screen specs (closed; committed by the Chief)
adversarial/           — a6-adversary's targets for this repo
docs/                  — build and user documentation
```

## Status

Pre-alpha. No product code has landed. Platform agents (a4s-atelier, a4g-atelier, a9s-landlord,
a9f-landlord) are queue-driven from Phase 6. Nothing here is installable or usable as a product.

## Security

See `SECURITY.md`. This repository is private; findings go to private advisories on this repo.
Contact the Chief directly. Do not open a public issue with a reproduction.
