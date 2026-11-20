# Online Learning for Cloud Load Balancing — Experiments & Analysis

**Hedge (exponential weights) portfolio evaluation over 9 load-balancer experts using CloudAnalyst/CloudSim.**

This repository contains the **online learning experimental code, runner, and results** from the companion framework **[throttled-esce-hybrid-lb](https://github.com/kopolart/throttled-esce-hybrid-lb)**.

## Contents

| Folder | Description |
|--------|-------------|
| `analysis/` | Python Hedge portfolio implementation (`hedge_experiment.py`), dependencies, generated figures/tables |
| `scripts/run-online-experiment.sh` | Headless driver: 9 experts × 5 seeds × 96 rounds (~30 min) |
| `src/cloudsim/ext/experiment/` | Java headless runner (`HeadlessSimulationRunner`, `ExperimentSettings`, `ExperimentMetrics`) |
| `results/online/` | Raw CSV logs for all 9 experts × 5 seeds (rounds.csv, summary.csv, run.properties) |
| `docs/` | Experiment methodology (`online-learning-experiment.md`), report (`.docx`), slides (`.pptx`) |

## Experts Evaluated

1. **Round Robin** (static baseline)
2. **Throttled** (state-aware)
3. **ESCE** (load-spreading)
4. **Honey Bee** (meta-heuristic)
5. **Ant Colony (ACO)** (pheromone-based)
6. **PSO** (particle swarm)
7. **Min-Min** (heuristic)
8. **Max-Min** (heuristic)
9. **Proposed Hybrid** (3 presets: balanced, cost-lean, latency-lean)

## Protocol

- **Workload**: Seeded 24-hour trace, diurnal + bursty patterns
- **Rounds**: 96 decision epochs (15-min intervals)
- **Seeds**: 5 paired seeds for statistical rigor
- **Metric**: Cumulative SLA loss (response time + cost penalty)
- **Learner**: Hedge with η = √(ln K / T), K=9, T=96

## Quick Start

### Prerequisites
- Java 8+ (for headless runner)
- Python 3.8+ (for analysis)
- CloudSim/CloudAnalyst jars (from throttled-esce-hybrid-lb/jars/)

### Run Experiment
```bash
# From throttled-esce-hybrid-lb root (needs jars and compiled classes):
./scripts/run-online-experiment.sh
# Resumes automatically via summary.csv checkpoints
# Subset: EXPERTS="rr hybrid" SEEDS="1 2 3" ./scripts/run-online-experiment.sh
```

### Analyze Results
```bash
cd analysis
pip install -r requirements.txt
python hedge_experiment.py
```
Outputs: `analysis/output/figures/*.png`, `analysis/output/tables/*.md`, `analysis/output/RESULTS.md`, `summary.json`

## Key Results

| Metric | Value |
|--------|-------|
| Hedge cumulative SLA loss | 72.61 |
| Best fixed expert (hindsight) | 72.43 |
| Empirical regret | 0.88% of theoretical bound |
| Paired-seed wins vs all 8 experts | 5/5 |

## Reproducibility

Full methodology, equations, and setup in `docs/online-learning-experiment.md`.  
Framework source (algorithms, CloudAnalyst integration) in **[throttled-esce-hybrid-lb](https://github.com/kopolart/throttled-esce-hybrid-lb)**.

## License

MIT — see framework repo LICENSE.

## Citation

```bibtex
@software{online_learning_loadbalancer_experiments,
  title = {Online Learning for Cloud Load Balancing — Hedge Portfolio Experiments},
  author = {Yousefi},
  url = {https://github.com/kopolart/online-learning-loadbalancer}
}
```