# server/egzos_platform/billing/

Billing paths for egzos-platform.

**Owner:** a9f-landlord (exclusive). No other agent writes to this directory.

## Invariants

- **Subscription proof only.** The billing surface checks subscription status and records billing
  events. It does not hold, forward, or inspect container tokens.
- **Firebase-as-identity behind the abstraction.** The identity provider is an implementation
  detail hidden behind the session abstraction. The billing layer sees a verified subscription
  claim, not raw Firebase tokens.
- **egzos.io holds no container token, ever.** Any billing path that receives a container token
  is a security finding. A6 reviews every commit to this directory.

## Phase

Phase 6.1. No code yet. `TODO(a9f)`: billing stub to be scaffolded per A9f's proposal, reviewed
and approved by the Chief when Phase 6 opens.
