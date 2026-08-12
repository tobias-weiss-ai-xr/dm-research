# Taxonomy — Classification System for Decision-Making Research

Papers in this corpus are classified using a two-dimensional taxonomy: **category** (primary research area) and **subcategory** (methodological or thematic focus within that area). The taxonomy is derived dynamically from the entries in `papers.yaml` — the levels listed below are the *intended* classification scheme as the corpus grows.

## How Classification Works

- Each paper in `papers.yaml` has a `category` and `subcategory` field.
- The pipeline scripts discover all unique categories and subcategories automatically.
- Taxonomy cells are the intersection of category × subcategory (e.g., `reinforcement-learning/model-free`).
- The `statistics.json` report tracks saturation: how many cells are filled vs. total possible cells.

## Categories

Categories represent the primary research domain of a paper.

### `reinforcement-learning`
Value-based, policy-gradient, and model-based reinforcement learning. Includes work on reward shaping, exploration strategies, and general RL theory.

**Typical subcategories:** `model-free`, `model-based`, `offline`, `inverse`, `hierarchical`, `theoretical`

### `planning`
Task decomposition, search, world models, and hierarchical planning. Covers both classical planning and learned planning approaches.

**Typical subcategories:** `model-based`, `hierarchical`, `single-agent`, `theoretical`

### `multi-agent`
Multi-agent reinforcement learning, social dilemmas, negotiation protocols, cooperative/competitive dynamics, and team coordination.

**Typical subcategories:** `cooperative`, `adversarial`, `single-agent`, `human-in-the-loop`

### `bayesian-decision`
Bayesian optimization, probabilistic inference for decision-making, expected utility theory, Gaussian processes for sequential decisions, and Thompson sampling.

**Typical subcategories:** `uncertainty`, `single-agent`, `theoretical`

### `human-decision`
Human decision-making research, bounded rationality, heuristics, cognitive biases, decision neuroscience, and behavioral economics models.

**Typical subcategories:** `human-in-the-loop`, `benchmark`, `survey`

### `decision-support`
Decision support systems, recommender decision interfaces, explainable decision-making, clinical decision support, and AI-assisted decision workflows.

**Typical subcategories:** `human-in-the-loop`, `transfer`, `benchmark`

### `optimization`
Combinatorial optimization, bandits (multi-armed and contextual), sequential optimization under constraints, and efficient search.

**Typical subcategories:** `uncertainty`, `theoretical`, `benchmark`

## Subcategories

Subcategories cut across categories to describe the *methodological approach* or *thematic focus*.

| Subcategory | Description |
|-------------|-------------|
| `model-free` | Model-free RL, policy search, value iteration without learned dynamics |
| `model-based` | Dynamics models, world models, simulation-based planning, learned simulators |
| `offline` | Offline RL, batch decision-making from logged data, off-policy evaluation |
| `inverse` | Inverse RL, preference learning, reward modeling from demonstrations |
| `hierarchical` | Options frameworks, feudal RL, macro-actions, temporal abstraction |
| `single-agent` | Individual decision-making (single decision-maker settings) |
| `adversarial` | Game theory, opponent modeling, minimax, robust adversaries |
| `cooperative` | Coordination, communication protocols, shared reward, team learning |
| `uncertainty` | Robust decision-making, risk-sensitive optimization, ambiguity-averse methods |
| `transfer` | Transfer learning in decision-making, domain adaptation, few-shot policy learning |
| `human-in-the-loop` | Interactive decision-making, preference elicitation, human-AI collaboration |
| `benchmark` | Decision-making benchmarks, standardized environments, evaluation datasets |
| `survey` | Survey papers, systematic reviews, meta-analyses |
| `theoretical` | Theoretical foundations, convergence guarantees, sample complexity, regret bounds |

## Naming Conventions

- Use **kebab-case** for both categories and subcategories (e.g., `reinforcement-learning`, `model-free`).
- Categories and subcategories should be **descriptive and broad enough** to group multiple papers, but **specific enough** to be meaningful.
- When in doubt, add a new category or subcategory rather than overloading an existing one — the pipeline handles dynamic taxonomy discovery.
- Avoid abbreviations in taxonomy keys; use full descriptive names.

## Adding Papers

When adding a paper to `papers.yaml`, classify it by the most specific applicable category and subcategory:

```yaml
- title: "Sample Paper Title"
  date: "2025-08"
  url: "https://arxiv.org/abs/XXXX.XXXXX"
  authors: ["Author A", "Author B"]
  abstract: "We propose..."
  venue: "ICML 2025"
  category: "reinforcement-learning"    # primary domain
  subcategory: "offline"                # methodological focus
```

A paper about offline multi-agent RL would be:

```yaml
  category: "multi-agent"
  subcategory: "offline"
```

A survey on Bayesian optimization would be:

```yaml
  category: "bayesian-decision"
  subcategory: "survey"
```

## Evolving the Taxonomy

The taxonomy is **not fixed**. As the corpus grows:

1. New categories and subcategories are added as needed.
2. Existing ones can be split if they become too broad.
3. The `statistics.json` gap analysis highlights underrepresented cells.
4. Run `python3 tools/landscape_analyzer.py --write-doc` to see which cells are thinnest.

The pipeline will automatically pick up any changes to the taxonomy in `papers.yaml` on the next run — no code changes needed.
