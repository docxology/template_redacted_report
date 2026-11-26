# Native verification

Tests cover release-audit behavior, redaction edge cases, residual-risk detection, source-safe ledgers, hash manifests, reviewer gates, typed fixture loading, canonical two-run JSON equality, real CLI execution, malformed/missing input failures, and unique-canary proof that source spans never reach public artifacts.

From the repository root, install the locked development dependencies and run
the coverage gate.

```bash
uv sync --locked --extra dev
uv run --no-sync pytest tests --cov=src --cov-fail-under=90
```

The manuscript binding tests also check the [results](manuscript/02_results.md)
against a fresh canonical audit, verify both figure paths against the figure
registry and disk, and confirm that the Stage 02 allowlist names scripts that
exist.
