# REVIEW-DECISIONS

Settled review dispositions for `Egzos/egzos-platform`. **Reviewers consult this file before
raising a finding.** If a finding you are about to raise is already settled here, cite the entry
id in one line and move on — do not re-litigate it in your review comment.

This file exists because a reviewer runs on a fresh checkout with no memory of the last run. Left
to itself it re-raises the same settled point on every PR that touches the same paths, which trains
the Chief to skim reviews. The register is the reviewer's memory, committed and reviewable like
anything else.

`Egzos/egzos` keeps its own register at the same path. Entry ids are **repo-local**: this file's
`RD-001` and the open-core repository's `RD-001` are unrelated. Never cite an entry across
repositories.

## What this file is not

It is **not** a suppression list, and it cannot be used as one.

- **Only `minor` and `major` findings can be settled here.** A `blocker` is never settleable in
  advance. Raise it.
- **Security-class findings are never settleable here.** No entry in this file excuses a
  credential in a diff, a container token reaching the server side, an enumeration path, or a
  missing audit event. If an entry appears to cover one, the entry is wrong — raise the finding
  and say the entry misled you.
- **An entry settles a specific claim about specific paths, not a topic.** RD-001 settles the
  licence of three named vendored files. It says nothing about the licence of the next vendored
  thing, and a reviewer that stretches it that way has made an error, not followed the register.
- **An entry can expire.** Where a disposition depends on repository state, the entry names the
  state under `Holds while`. When that state changes, the entry stops holding and the finding is
  live again.

## Who writes it

The Chief, only, and the `ownership` check enforces it: this path is listed under `chief_only`
in `.github/OWNERSHIP.yml`, so an agent branch that touches it fails the check. An agent that
believes an entry is wrong files a `governance` issue saying so — it never edits this file, and it
never adds an entry to pre-settle its own work.

## Entry format

Each entry carries: the finding as a reviewer actually phrases it, the disposition, the reasoning,
the state the reasoning depends on, and the paths in scope. Status is one of:

- **CLOSED** — the finding was correct and the underlying issue was fixed. Cite and move on.
- **ACCEPTED** — the finding is correct and the Chief is knowingly living with it. Cite and move on.
- **REJECTED** — the finding rests on a mistaken premise, named in the entry.
- **PRE-EMPTIVE** — not yet raised by a reviewer; recorded because the tree looks wrong at a glance
  and the explanation is not local to the file being read.

---

## RD-001 · Licence of the vendored 21st.dev skill text

**Status:** CLOSED · raised by a1r on `#8`, 2026-09-10, severity `minor`

**Finding as raised** — verbatim from the a1r run log on `#8`:

> [minor] `.github/skills/21st/README.md` — Provenance records URL, retrieval date and sha256
> digests (verified: all three files match) but not the license of the vendored `21st-dev/skill`
> text; §P requires licence noted on vendored catalogue source.

**Disposition.** The finding was correct and is fixed. `21st-dev/skill` is Apache-2.0, SPDX
`Apache-2.0`, upstream copyright line `Copyright 2026 21st.dev`. The licence is now carried
verbatim at `.github/skills/21st/LICENSE` (11339 bytes, sha256
`ac17c29e5529b0d977b8521353838c06c46f814d83de12da221418d62102de6f`, fetched from
`21st-dev/skill@main` on 2026-09-10) and recorded in the provenance table and the Licence section
of `.github/skills/21st/README.md`. Upstream ships no `NOTICE`, so §4(d) propagates nothing.

**Reasoning.** The first disposition was to skip this: §P treats skills as configuration rather
than dependencies, and the three files are already digest-pinned, which is a stronger provenance
claim than most vendoring. That reasoning was overturned by one fact. Apache-2.0 §4(a) attaches to
**distribution**, and a public repository distributes. `egzos-platform` is public today under D10.
Apache-2.0 is inbound-compatible with a proprietary work, so carrying the notice costs nothing and
settles the question.

**Holds while.** Indefinitely — the fix is a committed file, and it stays correct after the D10
flip to private even though the obligation itself lapses there. What lapses at D10 is the
*urgency*, not the entry. Revisit only if the vendored files are refetched at a new upstream
revision, in which case re-verify the licence along with the digests.

**Paths in scope.** `.github/skills/21st/README.md`, `.github/skills/21st/LICENSE`, and the three
`.github/skills/21st/*/SKILL.md` files. Nothing else. A future vendored component from any source
needs its own licence note and is not covered here.

---

## RD-002 · Dangling references inside the vendored SKILL.md files

**Status:** PRE-EMPTIVE — not yet raised

**The finding a reviewer would raise.**

> `.github/skills/21st/21st-cli-use/SKILL.md` — instructs the agent to "use the `21st-registry`
> skill" and refers to commands that live "in the `21st-ai` skill". Neither skill is installed;
> these are broken references.

**Disposition.** REJECTED as a defect; the observation is accurate and the conclusion is wrong.

**Reasoning.** The three SKILL.md files are vendored **byte-identical to upstream** so that the
sha256 digests in `.github/skills/21st/README.md` verify against the source. Patching a dangling
reference would break every digest and destroy the provenance claim the pinning exists to make.
`21st-registry`, `21st-design-sync`, `21st-ai` and `21st-ui-explore` are deliberately not
vendored — the first two publish outward from a proprietary repository, and the last two are A2
studio's work, not a4s's. An agent that follows one of those pointers finds nothing and fails
closed, which is the intended behaviour. The reasoning is written out in the "Deliberately absent"
section of the skills README.

**Holds while.** The vendored files remain byte-identical to upstream. If the Chief ever decides
to patch them, the digests come out of the README and this entry is void.

**Paths in scope.** `.github/skills/21st/*/SKILL.md`.

---

## RD-003 · Vendored skills live under `.github/skills/`, not `.claude/skills/`

**Status:** PRE-EMPTIVE — not yet raised

**The finding a reviewer would raise.**

> Agent skills are installed at `.github/skills/21st/` rather than the conventional
> `.claude/skills/`, so Claude Code will not discover them.

**Disposition.** REJECTED — the location is the security control, and discovery is arranged
deliberately.

**Reasoning.** A project-scope `.claude/skills/` is auto-discovered by **every** session that
checks out this repository, which would hand the catalogue surface to a1r, a2 and a6 — the three
required-check holders that §P's trust rule excludes from any catalogue surface. Keeping the files
under `.github/skills/` means no session picks them up implicitly. The `install-21st-skills` step
in `.github/workflows/atelier-queue.yml` copies them into `$HOME` for the a4s-atelier job alone, so
they are committed and content-pinned in the tree while being present in exactly one session.

**Holds while.** §P's trust rule stands, and the `install-21st-skills` step remains in
`atelier-queue.yml`. If that step is ever removed, the skills become genuinely unreachable and the
finding is live again.

**Paths in scope.** `.github/skills/**`, `.github/workflows/atelier-queue.yml`.

---

## RD-004 · Workflows use `--append-system-prompt-file`, not `--agent`

**Status:** REJECTED · PRE-EMPTIVE — not yet raised

**The finding a reviewer would raise.**

> The repository defines agents in `.claude/agents/*.md`, but no workflow invokes them with
> `--agent`. Each workflow instead strips the frontmatter with `awk` and passes the body via
> `--append-system-prompt-file`, duplicating what the agent definition already expresses.

**Disposition.** REJECTED — the premise that `--agent` is the correct mechanism here is mistaken,
and it was tried.

**Reasoning.** Phase 0.0 established empirically that `--agent` applies the definition's frontmatter
`tools:` list as a **hard restriction on the tool pool**, which removes `StructuredOutput`
(`--json-schema`), the sticky-comment MCP tool and `Skill`. The verdict JSON that the required
checks read is structured output, so `--agent` silently produced an empty verdict and the
fail-closed gate turned every review red. Stripping the frontmatter and appending the body
preserves the charter's content while leaving the tool pool under the workflow's explicit
`--allowedTools`, which is where least-privilege belongs in CI anyway. The `awk` line and a
two-line comment recording the reason sit directly above each call site.

The duplication is real and accepted: the frontmatter `tools:` list in each definition is now
documentation of intent rather than an enforced restriction, and the enforced list is
`--allowedTools` in the workflow. A PR that changes one without the other is a genuine finding —
raise that.

**Holds while.** `claude-code-action` behaves this way. If a release makes `--agent` compatible
with structured output, this is worth revisiting, and the entry is void.

**Paths in scope.** `.github/workflows/**`, `.claude/agents/**`.
