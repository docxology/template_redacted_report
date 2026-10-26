---
name: template-redacted-report
description: Redacted release-report exemplar for classification ceilings, source protection, source-safe ledgers, reviewer gates, and mosaic-risk checks.
version: 0.1.0
author: docxology
license: MIT
tags: [exemplar, redaction, disclosure-control, release-review]
---

# template-redacted-report

Load this skill when working inside the `template_redacted_report` repository
(standalone clone of the `template_redacted_report` exemplar).

## When to Use

- Creating or reviewing a sanitized release-report workflow.
- Auditing redaction ledgers, source-control coverage, reviewer approvals, residual-risk patterns, and mosaic risk.
- Forking a disclosure-control scaffold.

## Quick Reference

```bash
uv run pytest tests --cov=src --cov-fail-under=90
uv run python scripts/01_generate_release_artifacts.py
uv run python scripts/02_build_figures.py
```

Run from the repository root. Renderer and monorepo Stage 03/04 stages live in
[docxology/template](https://github.com/docxology/template).

## Pitfalls

- Use invented or cleared public fixture text only.
- Keep source-safe ledgers hash-based; do not expose original redacted spans in public packets.
- Treat `redaction_audit.json` and `release_ledger.json` as text-free public
  evidence surfaces; the comprehensive narrative packet remains in memory.
- Keep `01_generate_release_artifacts.py` thin and the normal Stage 02 allowlist
  separate from opt-in development PDF generation.
- Do not add operational targeting, collection, evasion, or surveillance guidance.
