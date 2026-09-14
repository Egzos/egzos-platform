# docs/

Documentation for egzos-platform.

Build governance documents (decisions log, build plan) are published in the open-core sibling
`Egzos/egzos/docs/build/` and are not duplicated here.

The one exception is `docs/build/REVIEW-DECISIONS.md`, the review register: settled review
dispositions that a1r-reviewer and a2-conformance consult **before** raising a finding, so a
settled point is cited rather than re-litigated on every PR that touches the same paths. It is
per-repository by necessity — a reviewer session only ever checks out the repository it is
reviewing, so a single shared copy would be unreachable from here. Chief-only (`chief_only` in
`.github/OWNERSHIP.yml`). It can never settle a `blocker` or a security-class finding. Entry ids
are repo-local and are never cited across repositories.

Internal platform documentation (deployment guides, runbooks, operator notes) will be added here
as the platform matures. `TODO(chief)`: documentation structure to be decided when Phase 6 opens.
