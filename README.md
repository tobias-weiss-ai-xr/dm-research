# DM Research

Research corpus and analysis pipeline for decision-making — WIP.

Standard `*-research` layout: `papers.yaml` is the source of truth;
`scripts/standard_stats.py` builds `papers.json` + `statistics.json`;
`scripts/analysis/generate_reports.py` and `tools/*` produce reports and
topic/trend documents; BibTeX via `scripts/export_bibtex.py`.

## Usage

```bash
python3 scripts/standard_stats.py
python3 scripts/analysis/generate_reports.py
python3 tools/landscape_analyzer.py --write-doc
```
