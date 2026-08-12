# Contributing to Decision-Making Research

Thanks for your interest! This repository is part of a consistent set of
`*-research` corpora, so contributions should stay within the shared house
style (standard pipeline, `papers.yaml` as source of truth).

## Workflow

1. Fork and branch off `main`.
2. Add or update entries in `papers.yaml`. Follow the [taxonomy guide](docs/taxonomy.md) for classification conventions.
3. Regenerate derived artifacts when you change the corpus:
   ```bash
   python3 scripts/standard_stats.py
   python3 scripts/analysis/generate_reports.py
   python3 tools/landscape_analyzer.py --write-doc
   python3 scripts/export_bibtex.py
   ```
4. Open a PR describing the change and your validation output.

## Paper Format

Each entry in `papers.yaml` requires at minimum `title`, `date`, `url`, `category`, and `subcategory`. See [docs/taxonomy.md](docs/taxonomy.md) for the classification system and [docs/pipeline.md](docs/pipeline.md) for the full data flow.

## CI

The CI pipeline (`.github/workflows/validate.yml`) automatically syntax-checks all scripts and regenerates `papers.json`, `statistics.json`, and the report docs on every push and PR.
