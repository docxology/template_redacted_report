# template_redacted_report - AGENTS.md

## Ground truth

Configuration lives in `manuscript/config.yaml`; invented public redaction fixtures live in `data/`; reusable release-audit logic lives in `src/redacted_report/`.

## Commands

```bash
uv run pytest tests --cov=src --cov-fail-under=90
uv run python scripts/01_generate_release_artifacts.py
uv run python scripts/02_build_figures.py
```

Standalone repository: the render/validate stages and the `infrastructure`
steganography tooling live in the [docxology/template](https://github.com/docxology/template)
monorepo, not here.

## Contracts and boundaries

Keep this exemplar limited to lawful redaction, declassification support, public-records release review, taxonomy normalization, source-safe ledgers, reviewer approval gates, sanitized packet export, and source-protection auditing. Do not add targeting, collection, evasion, or surveillance operational guidance. All committed examples must be invented fixtures.

Stage 02 runs the scripts declared by `manuscript/config.yaml` →
`analysis.scripts`, in order: `scripts/01_generate_release_artifacts.py`, then
`scripts/02_build_figures.py`. The first script delegates loading, policy
application, source-safe projection, and canonical JSON writing to
`src/redacted_report/artifacts.py`. Public audit artifacts must remain text-free:
they may expose sanitized metadata and cryptographic hashes, never source spans,
review rationales, or the in-memory narrative packet. The second script renders
the deterministic exemplar figures via `src/redacted_report/figures.py`.

Decision memory and verifier hardening follow the monorepo's `docs/rules/memory_and_decision_records.md` (not shipped in this standalone repository): use nearby `WHY:` comments only for surprising local choices, keep volatile numbers generated (not transcribed into prose), and pair every verifier-like gate (hash manifests, PDF proof verification, redaction ledgers) with a negative control — as the known-bad crafted-file cases in `tests/test_visuals_coverage.py` do.
