# Security Policy

**Internal — this repository is private.**

## Reporting a vulnerability

Report privately via **GitHub Security Advisories** on this repository.

Go to: Security → Report a vulnerability (https://github.com/Egzos/egzos-platform/security/advisories/new)

Do not open a public issue with a reproduction. Contact the Chief directly for urgent findings.

## Disclosure split

Security findings for this repository follow the same split as the open core:

- **Security findings:** private GitHub Security Advisory with the reproduction attached. The
  regression test enters `adversarial/` only in the fix PR (absent → passing). Because this
  repository is private, there is no zero-day risk from an xfail with a repro here — but the
  advisory mechanism is still the correct channel so findings are tracked and credited properly.
- **Non-security findings** (contract gaps, behavior bugs): xfail test plus a public issue in
  `Egzos/egzos` if the gap is in the container contract; a private issue here otherwise. The fix
  PR flips the marker.

## Scope

The following surfaces are in scope:

- The egzos.io flagship web UI (OAuth surface, consent phishing, web outward-drag presence)
- The hosted container delivery surface (relay, metering, thin-server)
- Billing and session paths (`server/egzos_platform/billing/`, `server/egzos_platform/sessions/`)
- Catalogue-content injection via the 21st.dev MCP (a4s-atelier's workflow only)
- Any path where `egzos.io` could inadvertently hold or transmit a container token

## Contact

The Chief reviews all advisories. Use GitHub Security Advisories (link above).
