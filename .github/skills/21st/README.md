# Vendored agent skills

Skills are **configuration**, not dependencies (v0.8 §P). They are vendored here by the
Chief, pinned by content, reviewed like any other committed file, and never fetched at
run time. A skill fetched live would be an unpinned instruction from a vendor, arriving
in the one session that holds the forge token and `API_KEY_21ST`.

## Provenance

Source: https://21st.dev/api/skills/<name> — the live mirror of `21st-dev/skill`.
Retrieved: 2026-09-09. Files are byte-identical to source so these digests verify upstream.

| skill | bytes | sha256 |
|---|---|---|
| `21st-cli-use` | 5010 | `88ff054abb4334a5095462dd20b5c4ec214c9b99f3256eae60dadb384d136bca` |
| `21st-ui-build` | 3002 | `151dbc3e97289a4ba945438f6552c0f597a40d8b5b1dc5d917dd9a5b65389641` |
| `21st-ui-review` | 2206 | `78939e830bceb64b8e2d32f831b80472008d5d2e50b31c8a8f38af5e5a015088` |

To update: refetch, diff, verify the new digest, commit as the Chief. Never automate this.

## Deliberately absent

`21st-registry` and `21st-design-sync` publish components and the project's design
tokens **out** to 21st.dev. No CI identity holds an outbound path from a proprietary
repository. `21st-ai` generates components; a4s installs A2's picks, and whether the
generation surface belongs to A2 is A2's decision, not a4s's.

`21st-ui-explore` is not vendored either. It defines visual directions, recommends one and records
the decision — design direction is human-only. That work has a seat already: A2 studio, on
Hyperagent, in a direction session with the Chief (`spec/design/README.md`). The catalogue browsing
this repo's agents must not do, A2 does there, with the Chief present to pick; the pick arrives here
as a committed spec. `21st-ui-build` is vendored, but its design-context steps are inert for a4s:
no `21st init --design-context`, no writes under `.21st/` (an unowned root path), no durable visual
choice. a4s reads `spec/design/` and builds what is already decided.

`21st-cli-use` still *references* the absent skills (lines 18-20, 103, 110): "use the
`21st-registry` skill", "lives in the `21st-ai` skill". Those pointers are dead ends —
the skills are not installed, so an attempt to follow one fails closed. The files are left
byte-identical rather than patched, so the digests above verify against upstream; the
dangling references are expected, not an oversight.

## Inert instructions inside the vendored text

The SKILL.md files are byte-identical to source, so they still describe commands a4s is not
granted. Each is inert — the grant list in `atelier-queue.yml` allows only `search`, `get`,
`add`, `theme`, `logo` and `usage`:

| vendored instruction | where | why it cannot run |
|---|---|---|
| `--api-key <key>` on the command line | `21st-cli-use` L24-25 | key comes from `API_KEY_21ST` in env; a CLI arg would land in a process list and a public Actions log |
| `21st generate` / `21st iterate` | `21st-cli-use` L50-57, L103, L110 | generation is A2's, not a4s's; `generate` is ungranted |
| `21st install-skill` | `21st-cli-use` L111 | run-time skill fetch; skills are vendored here and reviewed |
| `21st init --client claude --write` | `21st-cli-use` L126 | writes MCP config into the session |
| `21st publish*`, `21st login` | `21st-registry` (not vendored) | no outbound path from this repository |
| `21st init --design-context`, `.21st/` writes | `21st-ui-build` | design direction is A2 studio's, with the Chief |

If a grant is widened, revisit this table first.

## Location

These live under `.github/skills/`, **not** `.claude/skills/`. A project-scope `.claude/skills/`
is auto-discovered by every session that checks out this repository — including a1r, a2 and a6,
the required-check holders §P excludes from any catalogue surface. The `install-21st-skills` step
in `atelier-queue.yml` copies them into `$HOME` for the a4s job alone: committed and content-pinned
in the tree, present in exactly one session.
