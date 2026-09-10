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
| `21st-ui-explore` | 2672 | `1b94d8d00e7ae87c3737e37c2019cbdc680393ef17a96388e9845f541367f1ec` |
| `21st-ui-review` | 2206 | `78939e830bceb64b8e2d32f831b80472008d5d2e50b31c8a8f38af5e5a015088` |

To update: refetch, diff, verify the new digest, commit as the Chief. Never automate this.

## Deliberately absent

`21st-registry` and `21st-design-sync` publish components and the project's design
tokens **out** to 21st.dev. No CI identity holds an outbound path from a proprietary
repository. `21st-ai` generates components; a4s installs A2's picks, and whether the
generation surface belongs to A2 is A2's decision, not a4s's.

`21st-cli-use` still *references* all three (lines 18-20, 103, 110): "use the
`21st-registry` skill", "lives in the `21st-ai` skill". Those pointers are dead ends —
the skills are not installed, so an attempt to follow one fails closed. The files are left
byte-identical rather than patched, so the digests above verify against upstream; the
dangling references are expected, not an oversight.
