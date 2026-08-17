<h1 align="center">
  <strong>Decision-Making Research Corpus</strong>
</h1>
<h3 align="center">Reinforcement learning, planning, optimization under uncertainty, and human–AI decision processes</h3>

### 🔗 Links

- **GitHub**: https://github.com/tobias-weiss-ai-xr/dm-research
- **License**: https://github.com/tobias-weiss-ai-xr/dm-research/blob/main/LICENSE
- **CI**: https://github.com/tobias-weiss-ai-xr/dm-research/actions/workflows/validate.yml
- **Bayesian Stats**: https://github.com/tobias-weiss-ai-xr/bayesian-statistics-research
- **Robotics**: https://github.com/tobias-weiss-ai-xr/robotics-research
- **C2-AI**: https://github.com/tobias-weiss-ai-xr/c2-ai-research


> 🎲 **Decision-making research corpus:** reinforcement learning, sequential
> decision processes, planning under uncertainty, multi-agent coordination,
> Bayesian optimization, and human–AI collaborative decision-making — part of
> the family of `*-research` corpora.

<p align="center">
  <img src="https://raw.githubusercontent.com/tobias-weiss-ai-xr/dm-research/main/assets/visualizations/category_distribution.png" alt="Teaser" width="600" />
</p>

---

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

## 📊 Corpus Statistics

**1,924 papers** across **10 categories**.  
Sources: **arXiv** 82 (4%) · **DOI** 1,832 (95%) · **Other** 10 (0%).  
Full paper list: [GitHub Pages site](https://tobias-weiss-ai-xr.github.io/dm-research).

### Top categories

| Category | Papers | Recent | |
|----------|--------|--------|-|
| rl | **259** | 0 | ████████████ |
| bayesian-optimization | **195** | 0 | █████████░░░ |
| decision-theory | **192** | 0 | ████████░░░░ |
| human-ai-decision | **188** | 0 | ████████░░░░ |
| multi-agent-decision | **186** | 0 | ████████░░░░ |
| planning | **185** | 0 | ████████░░░░ |
| mdp | **181** | 0 | ████████░░░░ |
| risk-decision | **180** | 0 | ████████░░░░ |
| sequential-decision | **180** | 0 | ████████░░░░ |
| optimization | **178** | 0 | ████████░░░░ |


### By year

| Year | Papers | |
|------|--------|-|
| 2024 | 396 | ████████░░░░ |
| 2025 | 273 | █████░░░░░░░ |
| 2026 | 34 | ░░░░░░░░░░░░ |


### Momentum (hottest categories)

| Category | Total | Rate | Recent | Score |
|----------|-------|------|--------|-------|
| Multi Agent Decision | 186 | 2.5/mo | 16% | -29 |
| Human Ai Decision | 188 | 2.5/mo | 16% | -34 |
| Optimization | 178 | 0.5/mo | 3% | -57 |
| Risk Decision | 180 | 0.6/mo | 4% | -59 |
| Decision Theory | 192 | 0.6/mo | 4% | -70 |


### Trending keywords

| Keyword | Papers | Burst |
|---------|--------|-------|
| agentic | 11 | 11.55 |
| language model | 27 | 4.71 |
| dataset | 8 | 4.54 |
| multi-agent | 193 | 2.92 |
| causal | 7 | 2.59 |
| agent | 261 | 2.50 |
| human | 230 | 2.29 |
| teaming | 8 | 2.27 |


### Top venues

| Venue | Papers |
|-------|--------|
| arXiv (Cornell University) | 38 |
| Applied Energy | 34 |
| IEEE Transactions on Intelligent Transportation Systems | 30 |
| Expert Systems with Applications | 27 |
| IEEE Access | 23 |
| Computers & Industrial Engineering | 21 |
| European Journal of Operational Research | 20 |
| Applied Sciences | 18 |


### Research gaps (thinnest cells)

| Cell | Papers |
|------|--------|
| `bayesian-optimization/review` | 1 |
| `rl/evaluation` | 1 |
| `optimization/evaluation` | 1 |
| `multi-agent-decision/review` | 1 |
| `bayesian-optimization/evaluation` | 1 |



*Generated 2026-08 by `scripts/standard_stats.py`.*

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
