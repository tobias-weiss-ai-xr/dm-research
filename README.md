<h1 align="center">
  <strong>Decision-Making Research</strong>
</h1>
<h3 align="center">A data-driven survey corpus for Decision-Making — reinforcement learning, planning, optimization under uncertainty, and human–AI decision processes</h3>

<div align="center">

[![GitHub](https://img.shields.io/badge/GitHub-tobias--weiss--ai--xr/dm--research-181717.svg?logo=github)](https://github.com/tobias-weiss-ai-xr/dm-research)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg?)](LICENSE)
[![CI](https://img.shields.io/github/actions/workflow/status/tobias-weiss-ai-xr/dm-research/validate.yml?label=CI&logo=github)](https://github.com/tobias-weiss-ai-xr/dm-research/actions/workflows/validate.yml)
[![Agent Memory Survey](https://img.shields.io/badge/Agent_Memory_Survey-b31b1b.svg?logo=arxiv)](https://github.com/tobias-weiss-ai-xr/agent-memory-research)
[![Agentic VR Survey](https://img.shields.io/badge/Agentic_VR_Survey-004D40.svg?logo=github)](https://github.com/tobias-weiss-ai-xr/agentic-vr-research)
[![Skill Survey](https://img.shields.io/badge/Skill_Survey-004D40.svg?logo=github)](https://github.com/tobias-weiss-ai-xr/agent-skill-research)
[![Learning Survey](https://img.shields.io/badge/Learning_Survey-004D40.svg?logo=github)](https://github.com/tobias-weiss-ai-xr/agent-learning-research)

</div>

## 📋 Table of Contents

- [About](#about)
- [Taxonomy](#taxonomy)
- [Pipeline](#pipeline)
- [Generated Reports](#generated-reports)
- [Paper List](#paper-list)
- [Related Projects](#related-projects)
- [Contributing](#contributing)
- [Citation](#citation)

---

## About

This repository is a structured research corpus for **decision-making** — spanning reinforcement learning, sequential decision processes, planning under uncertainty, multi-agent coordination, Bayesian optimization, and human–AI collaborative decision-making. It is part of a family of consistent `*-research` corpora maintained under the same pipeline conventions.

**Current status:** scaffold — pipeline and tools are operational; the corpus (`papers.yaml`) awaits population.

### What this repo provides

| Artifact | Description |
|----------|-------------|
| `papers.yaml` | Source of truth — structured paper metadata (title, date, URL, authors, abstract, venue, category, subcategory) |
| `papers.json` | Derived flat list (newest first), consumed by front-ends |
| `statistics.json` | Full corpus analytics: category/subcategory/year counts, momentum, keyword bursts, venue mix, gaps, top authors |
| `assets/graph_analysis.json` | D3-style visualization data |
| `paper/references.bib` | BibTeX export for downstream writing |
| `docs/research/literature_review.md` | Auto-generated literature synthesis with category insights and momentum tables |
| `docs/research/trends.md` | 12-month keyword burst and fastest-growing cell report |
| `docs/research/landscape_report.md` | Detailed landscape: YoY growth, emerging themes, hot/thin cells |

---

## Taxonomy

Papers are classified along two orthogonal dimensions:

| Dimension | Levels |
|-----------|--------|
| **Category** | `reinforcement-learning` — value-based, policy-gradient, model-based RL<br>`planning` — task decomposition, search, world models, hierarchical planning<br>`multi-agent` — multi-agent RL, social dilemmas, negotiation, teaming<br>`bayesian-decision` — Bayesian optimization, probabilistic inference, expected utility<br>`human-decision` — human decision-making, bounded rationality, heuristics, cognitive models<br>`decision-support` — decision support systems, recommender decision interfaces, explainability<br>`optimization` — combinatorial optimization, bandits, sequential optimization |
| **Subcategory** | `model-free` — model-free RL, policy search, value iteration<br>`model-based` — dynamics models, world models, simulation-based planning<br>`offline` — offline RL, batch decision-making from logged data<br>`inverse` — inverse RL, preference learning, reward modeling<br>`hierarchical` — options, feudal RL, macro-actions<br>`single-agent` — individual decision-making<br>`adversarial` — game theory, opponent modeling, minimax<br>`cooperative` — coordination, communication, shared reward<br>`uncertainty` — robust decision-making, risk-sensitive, ambiguity<br>`transfer` — transfer in decision-making, domain adaptation<br>`human-in-the-loop` — interactive decision-making, preference elicitation<br>`benchmark` — decision-making benchmarks, datasets, environments<br>`survey` — survey papers, systematic reviews<br>`theoretical` — theoretical foundations, convergence, sample complexity |

> [!NOTE]
> The taxonomy is derived dynamically from the papers in `papers.yaml`. The levels above are the *intended* classification scheme — new categories and subcategories emerge as the corpus grows. See [docs/taxonomy.md](docs/taxonomy.md) for the full guide.

---

## Pipeline

The analysis pipeline follows the standard `*-research` convention shared across all sibling corpora:

```
papers.yaml
    │
    ├─▶ scripts/standard_stats.py  ──▶  statistics.json
    │                                 ──▶  papers.json
    │                                 ──▶  assets/graph_analysis.json
    │
    ├─▶ scripts/analysis/generate_reports.py  ──▶  docs/research/literature_review.md
    │                                              docs/research/trends.md
    │
    ├─▶ tools/landscape_analyzer.py  ──▶  docs/research/landscape_report.md
    │
    ├─▶ tools/trend_scanner.py       ──▶  terminal / JSON burst report
    │
    ├─▶ tools/topic_planner.py       ──▶  docs/topics/ARTICLE_TOPICS.md
    │
    ├─▶ tools/brief_generator.py      ──▶  docs/topics/brief_<topic>.md
    │
    └─▶ scripts/export_bibtex.py      ──▶  paper/references.bib
```

### Quick start

```bash
# Install dependencies
pip install -r requirements.txt

# Run the full pipeline
python3 scripts/standard_stats.py              # statistics.json + papers.json + graph_analysis.json
python3 scripts/analysis/generate_reports.py    # literature_review.md + trends.md
python3 tools/landscape_analyzer.py --write-doc # landscape_report.md
python3 scripts/export_bibtex.py                 # paper/references.bib

# Optional visualizations (requires matplotlib)
python3 scripts/visualize_statistics.py          # ASCII chart + assets/visualizations/category_distribution.png
```

### Tools

| Tool | Command | Output |
|------|---------|--------|
| Brief generator | `python3 tools/brief_generator.py "bayesian optimization" --papers 5` | Topic-focused article brief |
| Brief → doc | `python3 tools/brief_generator.py "multi-agent rl" --as-doc` | `docs/topics/brief_multi_agent_rl.md` |
| Topic planner | `python3 tools/topic_planner.py --top 10` | Evidence-ranked topic list + `docs/topics/ARTICLE_TOPICS.md` |
| Landscape | `python3 tools/landscape_analyzer.py` | Terminal report (YoY, venues, themes, gaps) |
| Landscape (doc) | `python3 tools/landscape_analyzer.py --write-doc` | `docs/research/landscape_report.md` |
| Trend scanner | `python3 tools/trend_scanner.py --months 12` | Keyword bursts + growing cells |
| Trend scanner (JSON) | `python3 tools/trend_scanner.py --months 12 --json` | Machine-readable JSON |
| Statistics viz | `python3 scripts/visualize_statistics.py` | ASCII bar chart + optional PNG |

### CI

The [validate workflow](.github/workflows/validate.yml) runs on every push and PR:
1. Installs dependencies from `requirements.txt`
2. Syntax-checks all pipeline scripts with `py_compile`
3. Regenerates `papers.json` and `statistics.json`
4. Regenerates report docs

---

## Generated Reports

| Report | Path | Regenerate |
|--------|------|-----------|
| Literature review | [`docs/research/literature_review.md`](docs/research/literature_review.md) | `python3 scripts/analysis/generate_reports.py` |
| Trends (12-month) | [`docs/research/trends.md`](docs/research/trends.md) | `python3 scripts/analysis/generate_reports.py` |
| Landscape report | [`docs/research/landscape_report.md`](docs/research/landscape_report.md) | `python3 tools/landscape_analyzer.py --write-doc` |
| Article topics | [`docs/topics/ARTICLE_TOPICS.md`](docs/topics/ARTICLE_TOPICS.md) | `python3 tools/topic_planner.py` |

---

## Paper List

<!-- PAPER_LIST_START -->

> The corpus is empty. Add papers to `papers.yaml` and re-run the pipeline to populate this section.

<!-- PAPER_LIST_END -->

---

## Related Projects

This repository is part of a family of research corpora sharing the same pipeline conventions:

| Repository | Focus |
|------------|-------|
| [agent-memory-research](https://github.com/tobias-weiss-ai-xr/agent-memory-research) | Agent memory systems |
| [agentic-vr-research](https://github.com/tobias-weiss-ai-xr/agentic-vr-research) | Agentic AI in VR/XR |
| [agent-skill-research](https://github.com/tobias-weiss-ai-xr/agent-skill-research) | Agent skill acquisition |
| [agent-learning-research](https://github.com/tobias-weiss-ai-xr/agent-learning-research) | Agent learning strategies |
| [learning-research](https://github.com/tobias-weiss-ai-xr/learning-research) | Machine learning research |
| [graph-research](https://github.com/tobias-weiss-ai-xr/graph-research) | Graph neural networks |
| [c2-ai-research](https://github.com/tobias-weiss-ai-xr/c2-ai-research) | Command & control AI |
| [devops-research](https://github.com/tobias-weiss-ai-xr/devops-research) | DevOps & infrastructure AI |

---

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for the contribution workflow. In short:

1. Fork and branch off `main`.
2. Add or update entries in `papers.yaml` (see [docs/taxonomy.md](docs/taxonomy.md) for classification guidance).
3. Regenerate derived artifacts:
   ```bash
   python3 scripts/standard_stats.py
   python3 scripts/analysis/generate_reports.py
   ```
4. Open a PR describing the change.

---

## Citation

If you use this research repository, please cite:

```bibtex
@software{dm_research_2026,
  author    = {Weiß, Tobias},
  title     = {Decision-Making Research},
  year      = {2026},
  license   = {MIT},
  url       = {https://github.com/tobias-weiss-ai-xr/dm-research}
}
```

See [CITATION.cff](CITATION.cff) for the machine-readable citation file.

---

## License

[MIT](LICENSE) © 2026 Tobias Weiß
