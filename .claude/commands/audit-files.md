---
description: Classify every file in the project by provenance and propose safe deletions
---

Audit every file in this project and report a single table:

| File | Class | Produced by | Verdict |

Skip `.git/` and editor/language noise (`.venv/`, `__pycache__/`, `.Rhistory`, `.RData`,
`.DS_Store`); say how many files you skipped. **Do not skip `scratch/` or `analysis/outputs/`
just because `.gitignore` lists them** — those are the two directories this audit exists to look at.

Classes:

- **source** — shipped input data. Never delete. Everything in `data/`.
- **script** — a producer: it writes other files. Name what it writes.
- **regenerable** — a script in `analysis/scripts/` writes it. Name that script.
- **record** — written by hand and not regenerable: `analysis/decisions.md`,
  `analysis_notebook.md`, `analysis/report.md`. Never propose deleting these.
- **scratch** — lives in `scratch/`. Deletable without ceremony.
- **orphan** — nothing reads it, and no script lists it as an output.
- **config** — tooling the project needs: `.gitignore`, `CLAUDE.md`, `.claude/`.
- **unknown** — you cannot determine what produced it or what reads it.

To build the graph, read each script's header block (the `Question:` / `Inputs:` / `Outputs:` /
`Decisions:` comment lines). Fall back to reading the code only where a header is missing, and say
which scripts those were.

Rules:

- Propose deletions. Do not delete anything — wait for me to confirm.
- For every **unknown**, say what you checked. An unknown file means its provenance is already
  lost; that is the finding, not the file.
- Check that every file in `analysis/outputs/` is named after a script in `analysis/scripts/`.
- Flag any file written by more than one script.
- Flag any script in `analysis/` that reads from `scratch/`. That boundary is supposed to hold in
  one direction only.
- Flag any script missing a header, and any header whose `Inputs:`/`Outputs:` lines disagree with
  what the code actually reads and writes. A stale header is worse than a missing one, because the
  rest of this audit trusts it.
- Flag absolute paths (`/Users/...`, `/n/`, `C:\`) anywhere in `analysis/`.
- Say whether the project is under version control and whether the tree is clean. A committed file
  is safe to delete; an uncommitted one is not. Note that `scratch/` and `analysis/outputs/` are
  gitignored, so files there are never "safe because they're committed" — they're safe because a
  script regenerates them, or they're not safe at all.
