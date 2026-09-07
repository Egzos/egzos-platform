# adversarial/

A6-adversary's targets for egzos-platform. Owner: a6-adversary (exclusive).

## Standing targets in this repository

Per the build plan (A6 charter), the egzos-platform-specific adversarial surface includes:

- **OAuth surface:** PKCE downgrade attempts against the container AS; redirect-URI allowlist
  bypass; token interception at the relay layer.
- **Consent phishing:** spoofed or misleading consent screens on the flagship; UI that obscures
  the grant being authorized.
- **Web outward-drag presence:** the drag-drop gate and its interaction with the container's
  trust model; attempts to extract data via drag events outside the intended surface.
- **Catalogue-content injection:** poisoned descriptions or previews from the 21st.dev MCP
  flowing into an agent with authority (must not reach a1r, a6, or Herald).

## Disclosure split

- **Security findings:** private GitHub Security Advisory on this repo, reproduction attached
  there. Regression test enters `adversarial/` only in the fix PR (absent → passing).
- **Non-security findings:** xfail test + issue. Fix PR flips the marker.

Treat this repository and its Actions logs as public whatever the visibility setting says (D10): an
xfail with a repro is a disclosure here exactly as in the open core, and the advisory is the only place
a reproduction lives before the fix.

## Status

No adversarial tests yet. A6 builds the suite as the surfaces land (Phase 6+).
