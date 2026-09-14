# server/

The egzos-platform server side. Built by a9s-landlord and a9f-landlord, queue-driven from Phase 6.

## What A9s and A9f build

**a9s-landlord (Sonnet 5):** hosted containers, preview pipeline, relay, metering plumbing,
thin-server delivery, hosted ambient intelligence (scheduling core intelligence on hosted
containers; continuous baselining; nightly suggest).

**a9f-landlord (Opus 5):** billing and session-security paths exclusively. Firebase-as-identity
behind the abstraction, subscription proof, session checks. Owner of `billing/` and `sessions/`
(exclusive paths; no other agent writes there).

## Stack

`TODO(chief)`: server stack chosen with A9's proposal when Phase 6 opens. Do not pick a stack
before A9f's proposal is reviewed and the Chief decides.

## The invariant that must never be violated

**egzos.io holds no container token, ever.**

The egzos.io session (Firebase-as-identity behind the abstraction) proves subscription. The
container token proves authorization and is obtained by the browser via PKCE against the user's
own container, held browser-side. BYOC intelligence runs in-container. The data plane never
touches egzos servers without an asterisk.

Every server path that could receive, cache, or forward a container token is a security finding.
A6 reviews billing and session paths (label `security` on those PRs).

## Layout

```
server/
  egzos_platform/
    billing/     — subscription proof, billing stub (a9f-landlord, exclusive)
    sessions/    — session checks, Firebase abstraction (a9f-landlord, exclusive)
```

## Status

Pre-alpha. No code yet. Phase 6.1.
