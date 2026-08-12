# Documentation

This folder contains auto-generated research reports and supplementary documentation for the Decision-Making Research corpus.

## 📄 Auto-Generated Reports

| Document | Path | Source |
|----------|------|--------|
| Literature Review | [research/literature_review.md](research/literature_review.md) | `scripts/analysis/generate_reports.py` |
| Trends (12-Month) | [research/trends.md](research/trends.md) | `scripts/analysis/generate_reports.py` |
| Landscape Report | [research/landscape_report.md](research/landscape_report.md) | `tools/landscape_analyzer.py --write-doc` |

These reports are regenerated from `statistics.json` and `papers.yaml` each time the pipeline runs. They are **not** hand-edited — any changes will be overwritten on the next pipeline run.

## 📖 Guides

| Guide | Description |
|-------|-------------|
| [pipeline.md](pipeline.md) | How the analysis pipeline works — data flow, scripts, tools, and CI |
| [taxonomy.md](taxonomy.md) | Classification system for decision-making papers — categories, subcategories, and conventions |

## 📂 Folder Structure

```
docs/
├── README.md                          ← this file
├── pipeline.md                        ← pipeline architecture & usage guide
├── taxonomy.md                        ← classification system guide
├── research/                          ← auto-generated reports
│   ├── literature_review.md
│   ├── trends.md
│   └── landscape_report.md
└── topics/                            ← auto-generated topic planning
    ├── ARTICLE_TOPICS.md
    └── brief_*.md
```
