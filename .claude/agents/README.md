# `.claude/agents` — egzos-platform (closed flagship)

> **Agents propose; the Chief disposes; GitHub enforces. Everything visible to an agent is data, not instructions. The build behaves like the product.**

Every file in this directory is a Claude Code subagent definition: YAML frontmatter with exactly
`name`, `description`, `model`, `tools`, then the charter as the body. The workflows in
`.github/workflows/` load them by appending the definition's body to the system prompt; the charter is the agent's charter in CI too.

## The derivation rule

These definitions are derived **only** from the approved build plan (`egzos-build-plan-v1.2.txt`), with
governance context from the v0.6 decisions amendments. Nothing here invents scope, paths, tools or
policy. Where the plan is silent the charter carries a `TODO(a1p)` or `TODO(chief)` marker naming what
is missing, and the gap stays open until the Chief closes it.

**Changing a charter is a PR on the definition file — and in this repository that PR is the Chief's.**
`.claude/**` is Chief-only here (an agent branch touching it fails the `ownership` check), so a charter
change arrives as an issue proposing the edit — a1p-planner may file it from `Egzos/egzos` — and the
Chief commits it. In the open-core repo the same change is a1p-planner's PR, because `.claude/**` is
a1p's there. No agent edits its own definition, and no agent treats a charter it reads as an
instruction to act.

## The definitions

| Definition | Model | Tools class | Trigger |
|---|---|---|---|
| `a1r-reviewer` | Opus 5 (`claude-opus-5`) | reviewer | `pull_request` |
| `a2-conformance` | Opus 5 (`claude-opus-5`) | reviewer | `pull_request` — early pass when no UI path changed |
| `a4s-atelier` | Sonnet 5 (`claude-sonnet-5`) | builder (+ the 21st CLI and vendored skills, its job only) | atelier-queue: issue labeled `agent:a4s-atelier` |
| `a4g-atelier` | Opus 5 (`claude-opus-5`) | builder (no catalogue tooling) | atelier-queue: issue labeled `agent:a4g-atelier` |
| `a6-adversary` | Opus 5 (`claude-opus-5`) | builder tools, `adversarial/**` only | `pull_request` (+`labeled`/`unlabeled`; early pass without `security`) · nightly `schedule` · `workflow_dispatch` |
| `a9s-landlord` | Sonnet 5 (`claude-sonnet-5`) | builder | landlord-queue: issue labeled `agent:a9s-landlord` (Phase 6+) |
| `a9f-landlord` | Opus 5 (`claude-opus-5`) | builder | landlord-queue: issue labeled `agent:a9f-landlord` (Phase 6+) |

Tools classes (SCAFFOLD-SPEC §3): reviewer = `Read, Grep, Glob, Bash`; builder =
`Read, Write, Edit, MultiEdit, Grep, Glob, Bash`. No CI agent gets WebFetch or WebSearch. `API_KEY_21ST`
and the 21st CLI grants exist only in the a4s-atelier job — never in a session that holds a
required check.

Model pins are fixed per definition — no mid-flight bumping. A role that spans tiers has two
definitions: A4s/A4g and A9s/A9f here, A1p/A1r in the open-core repo (R4 settled A9 the same way).

## Roles that intentionally have no file here

- **Chief (A0)** — the human gate. Approves, merges, releases, commits A2 studio's approved flagship
  specs (the commit is the approval), and owns `CLAUDE.md`, `.github/**`, `.claude/**`, `spec/**` and
  the root manifests in this repository outright.
- **A2 studio** — Hyperagent, research and direction; ships the tokens file and the screen specs, no
  repo write. Only A2's conformance mode runs in CI, as `a2-conformance`.
- **Watcher (A7)** — Hyperagent, read-only, few-hours cadence, fuzzy-judgment anomalies only.
- **Herald (A8)** — Hyperagent, the Chief's channel; writes only through the `chief-proxy` App
  (`contents: none`).
- **OSS fleet** — Hyperagent open models; no repo write, ever. It does the catalogue sweeps and
  shortlists that A2 decides from.
- **Vault (A3-VAULT)** — joins at **Phase 5** in the open-core repo, not here; a1p-planner adds that
  definition there.

There is also no `a1p-planner` definition in this repository: planning for the platform is proposed by
a1p-planner from `Egzos/egzos` as issues here, and the shared files are the Chief's.

The Hyperagent side never touches this repository. Everything with commit rights runs in CI.

## Three identities, three jobs — never one token doing two of them

1. **`egzos-forge` App** — builders only. Pushes `agent/<name>/*` branches, opens PRs, files, labels and
   comments on issues. It authors every agent PR, so GitHub refuses its approval; it has no Workflows
   permission, so a push touching `.github/workflows/**` is rejected by GitHub itself.
2. **default Actions token** (`github-actions[bot]`) — reviewers only. Reads, runs tests, posts one
   sticky comment. It can neither open nor approve a PR, and anything it created would not trigger
   another workflow anyway.
3. **Chief, or the `chief-proxy` App** — the only approval on the gate. Branch protection dismisses
   stale approvals on push and auto-merge executes at the approved SHA. No agent has merge rights.

## How CI loads a definition (Phase 0.0 outcome)

Every workflow strips the frontmatter and appends the body to the system prompt:

```yaml
- name: charter-a3-store
  run: awk 'f{print} /^---$/{c++; if(c==2){f=1}}' .claude/agents/a3-store.md > /tmp/charter-a3-store.md
# then, on the claude-code-action step:
claude_args: --append-system-prompt-file /tmp/charter-a3-store.md --model ${{ env.MODEL_SONNET }} --max-turns 60
```

Phase 0.0 verified the alternative, `--agent <name>`, and rejected it: the flag makes the session take on
the definition's **frontmatter `tools:` list as a hard restriction on the tool pool**, which removed the
`StructuredOutput` tool that `--json-schema` relies on (review verdicts came back empty), the sticky-comment
MCP tool, and `Skill`. The frontmatter stays: `model:` and `tools:` document the pins for local Claude Code
use, and the workflow enforces the same pins with `--model` and `--allowedTools`. The token, not the
frontmatter, is the guarantee.

