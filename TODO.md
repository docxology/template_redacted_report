# template_redacted_report TODO

This backlog is future-only. Completed validation and dated review evidence are preserved in
the [docxology/template](https://github.com/docxology/template) monorepo's
`docs/maintenance/exemplar-backlog-history.md` (not shipped in this standalone repository)
or in source-owned generated receipts. Each active row must retain a stable ID, size, dependency,
next action, proving artifact, acceptance command, and negative control; absence of an owner or external receipt
keeps a capability blocked rather than silently promoting it.

## Backlog operating rules

- Keep deterministic and offline defaults unchanged unless an upcoming row explicitly scopes an opt-in.
- Do not close a row until its producer, artifact, consumer, gate, and failing negative control are present.
- Treat unavailable network, LLM, container, formal-tool, and publication paths as explicit skips
  or blockers.
- Re-derive counts and receipts from live source data; never copy measurements into this planning file.

## Integrity and template-status gaps

- Keep rendered sanitized report outputs and development visual-proof outputs regenerated after style, steganography, or Kmyth build changes.

## Configurable-surface gaps

- Organization-specific marking taxonomies and review-role policies are intentionally limited to cleared, invented fixtures; a real policy needs a separately scoped owner-approved row.

## Documentation and signposting gaps

- Keep public safety boundaries visible in README, AGENTS, and manuscript prose.

## Current test and validator contract

- Bind manuscript tables to the canonical audit JSON only if rendering can preserve the text-free projection and fails closed when the audit schema changes.
- Pixel-level visual regression remains the explicit blocked-tool row below until the repo adopts stable screenshot/PDF raster tooling for exemplar outputs.
- Keep year-stable ISO-date residual detection and complete collection-platform
  span coverage under negative controls; contextual labels in explicitly public
  explanatory prose must remain distinct from source-segment residuals.

## Minor upcoming

| ID | Status | Size | Dependency | Next action / unblock condition | Proving artifact | Acceptance command | Negative control |
| --- | --- | --- | --- | --- | --- | --- | --- |
No active rows are currently scoped at this size.

## Medium upcoming

| ID | Status | Size | Dependency | Next action / unblock condition | Proving artifact | Acceptance command | Negative control |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `REDACTED-VISUAL-1` | blocked-tool | Medium | Stable raster toolchain | Install or pin the required tool, or record its unavailable status; run the explicit visual gate only when tooling is pinned and attach a pixel regression manifest. | pixel regression manifest | `uv run pytest tests -q --no-cov` | missing raster tool must report unavailable, not pass |

## Major upcoming

| ID | Status | Size | Dependency | Next action / unblock condition | Proving artifact | Acceptance command | Negative control |
| --- | --- | --- | --- | --- | --- | --- | --- |
No active rows are currently scoped at this size.

## Backlog status

Rows remain active until the acceptance command and negative control pass in the same source revision.
A blocked row is a deliberate boundary, not a skipped success.

## Workflowz review — 2026-11-10

| ID | Status | Size | Dependency | Next action / unblock condition | Proving artifact | Acceptance command | Negative control |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `WFZ-RR-001` | ready | Minor | None; keep deterministic defaults and the fixed proof-matrix byte contract unchanged | Make the fallback byline explicit rather than silently replacing a present but unreadable or malformed manuscript configuration. `publication_meta.publication_author_and_date` catches every `Exception` and returns `("Template Author", "")` at `src/redacted_report/publication_meta.py:27-28`, even though its own purpose is to prevent a scaffold byline from being stamped into tracked proof PDFs (`src/redacted_report/publication_meta.py:3-7`). Preserve an explicit neutral-byline path only when the configuration is intentionally absent; for a present config with invalid YAML or missing/invalid author fields, raise a clear configuration error so a proof run cannot silently lose its provenance. Add a behavioral test for a present malformed config and a readable valid config, and document any intentional standalone fallback. | Focused regression test plus regenerated proof metadata showing either the configured byline or a clear failure for invalid config | From the repository root: `uv run python -c 'from pathlib import Path; import tempfile; import redacted_report.publication_meta as p; d=tempfile.TemporaryDirectory(); q=Path(d.name)/"config.yaml"; q.write_text("authors: [broken\n", encoding="utf-8"); p._CONFIG_PATH=q; exec("try:\n p.publication_author_and_date()\nexcept Exception as e:\n print(type(e).__name__, str(e))\nelse:\n raise SystemExit(\"invalid config silently produced a neutral byline\")")'` and run the existing rendering checks with `uv run pytest tests/test_visuals_proofs.py -q --no-cov`. | With the same isolated config-path setup, write `authors: [{name: "Fixture Author"}]\npaper: {date: "2026-01-01"}\n`; `publication_author_and_date()` must return `("Fixture Author", "2026-01-01")`. A missing config may use the documented standalone fallback, but malformed present YAML must not pass by returning the neutral byline. |
