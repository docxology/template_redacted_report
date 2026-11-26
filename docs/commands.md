# Analysis and development proof commands

Run these scripts from the repository root after [installing the locked dependencies](rendering.md). The monorepo Stage 03/04
render/validate stages and the drift checker live in
[docxology/template](https://github.com/docxology/template); this standalone
repository ships the audit engine and the development visual-proof scripts.

## Canonical Stage 02 analysis

`docs/manuscript/config.yaml` allowlists the two normal analysis entrypoints:

```bash
uv run python scripts/01_generate_release_artifacts.py
uv run python scripts/02_build_figures.py
```

`01_generate_release_artifacts.py` delegates to
`redacted_report.write_release_artifacts()`. It reads
`data/example_segments.json` and writes only
`output/reports/redaction_audit.json` and
`output/data/release_ledger.json`. Both files are canonical JSON and exclude
source text. The script accepts `--input` and `--output-root` for fixture
testing. `02_build_figures.py` writes the two PNG/SVG pairs and
`output/figures/figure_registry.json`.

## Development visual matrix

The development proofs cover blackout, whiteout, grayout, and blur treatments
on white, gray, black, and blurred backgrounds. The blur background subdues
non-redacted context while leaving spans treated by the selected redaction
style. Proof generation is an explicit opt-in separate from Stage 02.

`generate_dev_variants.py` creates the development proof matrix for every redaction style and PDF background combination. By default it also runs the template steganography/provenance post-processor on every proof PDF and writes `output/dev/redaction_variants/variant_matrix.json`; pass `--no-steganography` when the `infrastructure` package from a [docxology/template](https://github.com/docxology/template) checkout is not importable.

```bash
PYTHONPATH=/path/to/template-renderer uv run python scripts/generate_dev_variants.py
```

Kmyth TPM sidecar sealing is optional and disabled by default. Use `--with-kmyth` to request sealing with tools on `PATH`, `--kmyth-binary-dir <path-to-kmyth-bin>` to pin a specific build, or `--require-kmyth` when missing tools or failed sealing should block generation.

Verify the full provenance-enabled matrix after generation. The smoke option requires Poppler
`pdftoppm` on PATH.

```bash
uv run python scripts/verify_dev_variants.py --render-smoke
```

The verifier enforces the stable `redaction_on_background.pdf`, `redaction_on_background_steganography.pdf`, and `redaction_on_background.hashes.json` filenames for all 16 visual combinations, checks matrix hashes, opens every PDF, and optionally rasterizes page 1 of every PDF with Poppler. For Kmyth-enabled runs, add `--require-kmyth-sidecars` to require both `.ski` sidecars for every variant.
