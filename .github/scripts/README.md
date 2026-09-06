# .github/scripts — egzos-platform CI scripts

## check_ownership.py

Enforces path ownership on every pull request.  Reads `.github/OWNERSHIP.yml`
and exits 0 (pass) or 1 (fail).

### How it works

Same logic as the egzos open-core checker, adapted for the platform schema:

1. **Branch classification** — branches starting with `agent/` are agent
   branches; all others are human (Chief) branches.
2. **Human branches** — print "human branch — Chief owns everything" and pass.
   The size cap still applies.
3. **Agent branches** — derive the agent name from the second path segment
   (`agent/<name>/<slug>`).  The agent must exist in `OWNERSHIP.yml`.
4. **Chief-only paths** (`chief_only:` key) — `CLAUDE.md`, `.github/**`,
   `.claude/**`, `spec/**`, and root manifests.  Any agent branch touching
   these paths fails immediately.
5. **File ownership** — each changed file must match at least one of the
   agent's `paths:` globs.
6. **Exclusive paths** — if the file matches another agent's `exclusive:` glob,
   the check fails.  E.g. `apps/ui-flagship/src/bespoke/**` is exclusive to
   a4g-atelier; a4s-atelier must not touch those paths.
7. **Size cap** — 600 lines / 30 files (lockfiles and tests/fixtures/** excluded).
   Waived by the `size-exception` label.

### Local run

```bash
pip install pyyaml

python3 .github/scripts/check_ownership.py \
    --base origin/main \
    --head HEAD \
    --branch "$(git rev-parse --abbrev-ref HEAD)" \
    --labels ""

# Testing with canned data:
python3 .github/scripts/check_ownership.py \
    --base unused \
    --head unused \
    --branch "agent/a4g-atelier/issue-7" \
    --labels "" \
    --changed-files /tmp/files.txt \
    --numstat /tmp/numstat.txt
```
