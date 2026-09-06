# server/egzos_platform/sessions/

Session-security paths for egzos-platform.

**Owner:** a9f-landlord (exclusive). No other agent writes to this directory.

## Invariants

- **Firebase-as-identity behind the abstraction.** Firebase (or any configured IdP) is an
  implementation detail. The rest of the platform sees a verified session claim; it does not
  interact with Firebase directly.
- **Session checks.** Every server-rendered or API response that requires an authenticated session
  passes through this layer. Session validity is checked here, not inline.
- **egzos.io holds no container token, ever.** The session layer proves subscription only. It
  never holds, inspects, or forwards a container token. BYOC intelligence runs in-container.
  Any session path that touches a container token is a security finding.

## Phase

Phase 6.1. No code yet. `TODO(a9f)`: session layer to be scaffolded per A9f's proposal, reviewed
and approved by the Chief when Phase 6 opens.
