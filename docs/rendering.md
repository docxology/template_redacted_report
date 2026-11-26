# Render and validate the report

Install uv, Git, Pandoc 3.14, and XeLaTeX. The slide renderer uses the
Pandoc Lua caption API. The pinned template provides
`scripts/shell/setup-system-deps.sh --check` to check the system tools. Run
these commands from the root of a clean clone of this project. The native step
uses the project's pinned Python and locked dependencies.

```bash
uv sync --locked --extra dev
uv run --no-sync python scripts/01_generate_release_artifacts.py
uv run --no-sync python scripts/02_build_figures.py
```

The first command after setup generates
`output/reports/redaction_audit.json` and `output/data/release_ledger.json`.
The second generates the two figures and their registry in `output/figures/`.
The only report source is [the manuscript directory](manuscript/README.md).
To render a fresh copy without prior edition artifacts, use a disposable
clone and clear its tracked `output/` directory before running the two
generators. Tracked outputs are snapshots of previous runs.

Register the indexed checkout as a working project in a separate
`docxology/template` clone. The project directory should be named
`template_redacted_report`.

```bash
report_root="$(pwd -P)"
git clone https://github.com/docxology/template.git ../template-renderer
cd ../template-renderer
git checkout --detach 87704feae0a4490ef7dad9c931d268e8c30273c2
uv sync --locked --no-dev
mkdir -p projects/working
ln -s "$report_root" projects/working/template_redacted_report
uv run --no-sync python scripts/pipeline/stage_03_render.py --project working/template_redacted_report
uv run --no-sync python scripts/pipeline/stage_04_validate.py --project working/template_redacted_report
```

Stage 03 writes to the report checkout. Check the combined
`output/pdf/template_redacted_report_combined.pdf` and `output/web/index.html`
for the abstract, methods, results, discussion, bibliography, and both
generated figures. Section slide PDFs are in `output/slides/`. Stage 04 writes
`output/reports/validation_report.json` and
`output/reports/rendered_provenance.json`. The provenance report identifies the
numbered sections under `docs/manuscript/` that the renderer consumed.

See the pinned [project integration guide](https://github.com/docxology/template/blob/87704feae0a4490ef7dad9c931d268e8c30273c2/projects/README.md#L208-L223),
[manuscript resolver](https://github.com/docxology/template/blob/87704feae0a4490ef7dad9c931d268e8c30273c2/infrastructure/core/project_paths.py#L59-L75),
and [stage commands](https://github.com/docxology/template/blob/87704feae0a4490ef7dad9c931d268e8c30273c2/docs/RUN_GUIDE.md#L563-L574).
