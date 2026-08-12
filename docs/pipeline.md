# Pipeline Architecture

The Decision-Making Research pipeline follows the standard `*-research` convention shared across all sibling corpora. Every artifact is derived from the single source of truth: `papers.yaml`.

## Data Flow

```
papers.yaml ──────────────────────────────────────────────────────────────┐
    │                                                                     │
    ├── scripts/standard_stats.py ──▶ statistics.json                    │
    │                               ──▶ papers.json                       │
    │                               ──▶ assets/graph_analysis.json       │
    │                                                                     │
    ├── scripts/analysis/generate_reports.py ──▶ docs/research/           │
    │       (consumes statistics.json)         literature_review.md        │
    │                                           trends.md                  │
    │                                                                     │
    ├── tools/landscape_analyzer.py ──▶ docs/research/landscape_report.md │
    │                                                                     │
    ├── tools/trend_scanner.py ──▶ terminal output or JSON                │
    │   (also reused by generate_reports.py via import)                   │
    │                                                                     │
    ├── tools/topic_planner.py ──▶ docs/topics/ARTICLE_TOPICS.md          │
    │                                                                     │
    ├── tools/brief_generator.py ──▶ docs/topics/brief_<topic>.md          │
    │                                                                     │
    └── scripts/export_bibtex.py ──▶ paper/references.bib                │
                                                                          │
    scripts/visualize_statistics.py ◀── statistics.json                    │
        (ASCII chart + optional PNG via matplotlib)                      │
```

## Source of Truth

`papers.yaml` is the **only file you edit by hand**. Every other artifact is derived. A single paper entry looks like:

```yaml
- title: "Bayesian Optimization for Robust Decision-Making"
  date: "2025-06"
  url: "https://arxiv.org/abs/2506.xxxxx"
  authors:
    - "Author One"
    - "Author Two"
  abstract: "We propose a Bayesian optimization framework for sequential decision-making under uncertainty..."
  venue: "NeurIPS 2025"
  category: "bayesian-decision"
  subcategory: "uncertainty"
```

### Required fields

| Field | Type | Description |
|-------|------|-------------|
| `title` | string | Paper title |
| `date` | string | Publication date in `YYYY-MM` format |
| `url` | string | arXiv, DOI, or publisher URL |
| `category` | string | Primary classification (see [taxonomy.md](taxonomy.md)) |
| `subcategory` | string | Sub-classification within the category |

### Optional fields

| Field | Type | Description |
|-------|------|-------------|
| `authors` | list\[string\] | Author names |
| `abstract` | string | Abstract text (used for keyword burst analysis) |
| `venue` | string | Publication venue (journal, conference, preprint server) |

## Scripts

### `scripts/standard_stats.py`

Generates the three core derived files from `papers.yaml`:

- **`statistics.json`** — Full analytics: paper counts by category, subcategory, year, and cell; 12-month momentum scores; category trajectories; keyword bursts; source breakdown (arXiv vs DOI); venue distribution; gap analysis (thinnest cells, white space); top authors.
- **`papers.json`** — Flat list of papers sorted newest-first, suitable for front-end rendering.
- **`assets/graph_analysis.json`** — D3-style visualization data with categories, subcategories, timeline, venues, and keyword bursts.

```bash
python3 scripts/standard_stats.py
```

### `scripts/analysis/generate_reports.py`

Consumes `papers.yaml` (directly) and `statistics.json` to produce two auto-generated reports:

- **`docs/research/literature_review.md`** — Corpus overview table, momentum by category, research gaps, and per-category insights with recent papers.
- **`docs/research/trends.md`** — 12-month keyword burst table and fastest-growing cells.

```bash
python3 scripts/analysis/generate_reports.py
```

### `scripts/export_bibtex.py`

Exports all papers from `papers.yaml` to BibTeX format.

```bash
python3 scripts/export_bibtex.py
# → paper/references.bib
```

### `scripts/visualize_statistics.py`

Prints an ASCII bar chart of papers per category to the terminal. If `matplotlib` is installed, also writes a PNG.

```bash
python3 scripts/visualize_statistics.py
# → assets/visualizations/category_distribution.png (optional)
```

## Tools

### `tools/landscape_analyzer.py`

Produces a comprehensive landscape report with year-over-year category growth, research aspects, year trends, emerging themes, venue mix, top authors, and hot/thin cells.

```bash
python3 tools/landscape_analyzer.py                 # terminal report
python3 tools/landscape_analyzer.py --json          # machine-readable JSON
python3 tools/landscape_analyzer.py --write-doc     # docs/research/landscape_report.md
```

### `tools/trend_scanner.py`

Scans for keyword bursts and fastest-growing taxonomy cells within a configurable time window. Provides a reusable `scan()` function imported by `generate_reports.py`.

```bash
python3 tools/trend_scanner.py --months 12          # terminal report
python3 tools/trend_scanner.py --months 12 --json    # machine-readable JSON
```

### `tools/topic_planner.py`

Ranks categories by evidence (paper count + recent activity) and writes a topic planning document.

```bash
python3 tools/topic_planner.py --top 10
# → docs/topics/ARTICLE_TOPICS.md
```

### `tools/brief_generator.py`

Searches the corpus for papers matching a topic query and generates a ready-to-write article brief.

```bash
python3 tools/brief_generator.py "multi-agent reinforcement learning" --papers 5
python3 tools/brief_generator.py "bayesian optimization" --as-doc
# → docs/topics/brief_bayesian_optimization.md
```

## Keyword Lists

Several scripts scan paper titles and abstracts for keywords to detect research trends. The keyword lists are defined inline in the scripts:

| Script | Variable | Purpose |
|--------|----------|---------|
| `scripts/standard_stats.py` | `BURST_KEYWORDS` | Burst detection for statistics generation |
| `tools/landscape_analyzer.py` | `THEME_KEYWORDS` | Emerging theme detection |
| `tools/trend_scanner.py` | `TREND_KEYWORDS` | Trend scanning and burst scoring |

All lists cover: reinforcement learning, deep RL, policy, reward, multi-agent, planning, simulation, world models, decision-making, optimization, Bayesian methods, uncertainty, transfer, imitation, offline RL, memory, retrieval, benchmarks, datasets, surveys, human factors, autonomy, scalability, federated learning, explainability, diffusion models, language models, multimodal, graph methods, skills, tools, embodied AI, causality, and attention.

## CI Pipeline

The GitHub Actions workflow (`.github/workflows/validate.yml`) runs on pushes to `main` and on all PRs:

1. **Checkout** the repository
2. **Set up** Python 3.11
3. **Install** dependencies from `requirements.txt`
4. **Syntax-check** all pipeline scripts (`py_compile`)
5. **Regenerate** `statistics.json` and `papers.json`
6. **Regenerate** report docs (`literature_review.md`, `trends.md`)

This ensures that changes to `papers.yaml` always produce valid, consistent derived artifacts.

## Dependencies

```
PyYAML>=6.0
matplotlib>=3.0  # optional, for visualize_statistics.py PNG output
```

Install with:

```bash
pip install -r requirements.txt
```
