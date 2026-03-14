<div align="center">

<br />


# Swarm Intelligence Research Sandbox

**A serious, research-grade multi-agent simulation platform built entirely in the browser.**

[![Live Demo](https://img.shields.io/badge/LIVE%20DEMO-ma--mar--itan.github.io-4d9fff?style=for-the-badge&logoColor=white)](https://ma-mar-itan.github.io/Swarm-Intelligence-Playground/)
[![Version](https://img.shields.io/badge/version-3.0-00c4b4?style=for-the-badge)](https://github.com/Ma-Mar-Itan/Swarm-Intelligence-Playground)
[![License](https://img.shields.io/badge/license-MIT-9b7fff?style=for-the-badge)](LICENSE)
[![No Dependencies](https://img.shields.io/badge/dependencies-zero-2dd68c?style=for-the-badge)](https://ma-mar-itan.github.io/Swarm-Intelligence-Playground/)

<br />

> *Explore how simple local interaction rules generate complex collective intelligence —*
> *entirely in your browser, with no setup, no backend, no dependencies.*

<br />

---

</div>

## What Is This?

The **Swarm Intelligence Research Sandbox** (SIRS) is a computational laboratory for decentralized multi-agent systems. It is designed not as a toy visualization but as a proper experimentation environment for researchers, engineers, and students working with:

- Swarm robotics and autonomous systems
- Complex adaptive systems and emergent behavior
- Multi-agent algorithm design and benchmarking
- Distributed coordination and consensus protocols

The simulator runs **100–800+ agents** in real time, supports a full **algorithm plugin API**, batch **parameter sweep experiments**, quantitative **metrics export**, and a layered **environment model** — all in a single `.html` file.

---

## Live Application

**→ [https://ma-mar-itan.github.io/Swarm-Intelligence-Playground/](https://ma-mar-itan.github.io/Swarm-Intelligence-Playground/)**

Open it. No installation. No login. No dependencies.

---

## Feature Overview

### Algorithm Engine

| Algorithm | Tag | Description |
|---|---|---|
| Reynolds Boids | `REYNOLDS` | Classic separation · alignment · cohesion |
| Vicsek Model | `VICSEK` | Metric interaction, noise-driven phase transitions |
| Formation Control | `GEO` | Decentralized convergence to geometric targets |
| Distributed Exploration | `FRONTIER` | Coverage-gradient frontier seeking |
| Consensus Dynamics | `ORDER-Φ` | Heading consensus, order parameter measurement |
| Leader–Follower | `HIER` | Hierarchical swarm with dynamic leader routing |
| Artificial Potential Fields | `APF` | Repulsion/attraction field superposition |
| PSO Dynamics | `PSO` | Personal best + global best particle convergence |
| ACO Exploration | `ACO` | Pheromone grid deposition and gradient following |
| Resource Foraging | `FORAGE` | Collect-and-return with nest homing |
| Split & Merge | `FISSION` | Periodic fission–fusion group dynamics |

### Algorithm Plugin API

Register any custom behavior at runtime — no page reload required:

```javascript
// The full plugin interface
registerBehavior('my_algorithm', function(agent, neighbors, env) {
  let fx = 0, fy = 0;

  for (const b of neighbors) {
    const dx = agent.x - b.x;
    const dy = agent.y - b.y;
    const d  = Math.hypot(dx, dy) || 0.001;

    // your local interaction rule here
    fx += (dx / d) * env.params.separation;
    fy += (dy / d) * env.params.separation;
  }

  return { fx, fy };
});
```

The `env` object exposes: `env.W`, `env.H`, `env.frame`, `env.params`, `env.obstacles`, `env.wind`.
The registered algorithm immediately appears in the algorithm selector and comparison tools.

---

### Experiment Framework

**Parameter Sweep** — define ranges over any combination of parameters and automatically generate the full Cartesian product of configurations:

| Parameter | Example Range |
|---|---|
| `senseRadius` | 20 → 120, 5 steps |
| `alignment` | 0.5 → 2.5, 4 steps |
| `noise` | 0.01 → 0.5, 6 steps |
| `agentCount` | 50 → 400, 4 steps |
| `commFail` (packet loss) | 0 → 0.5, 3 steps |

**Batch Runner** — run all configurations non-blocking, streaming results live:

```
Configurations: 120  ×  Runs/config: 5  =  600 total runs
```

**Monte Carlo mode** — each run draws a fresh random seed for statistical robustness.

**Export formats:**

```bash
swarm_experiment_1720000000000.csv   # one row per run, all params + metrics
swarm_experiment_1720000000000.json  # full structured results with metadata
swarm_snapshot_1720000000000.csv     # live simulation state snapshot
swarm_state_1720000000000.json       # full agent state vectors
```

---

### Metrics Dashboard

All metrics update live at ~8 Hz and are logged continuously:

| Metric | Symbol | Description |
|---|---|---|
| Avg Inter-Agent Distance | *d̄* | Mean Euclidean distance between neighboring pairs |
| Detected Clusters | *K* | Connected-component cluster count via BFS |
| Spatial Coverage | *C* | Fraction of arena cells occupied (20×14 grid) |
| Mean Velocity | *v̄* | Average agent speed in units/frame |
| Consensus Order Parameter | *Φ* | Vicsek order parameter: `‖Σ v̂ᵢ‖ / N` ∈ [0, 1] |
| Swarm Entropy | *H* | Shannon entropy of heading distribution (bits) |
| Formation Error | *ε* | Mean displacement from formation targets (px) |

Sparkline charts for *C*, *Φ*, and *d̄* over time are displayed in the right panel alongside a live agent density heatmap.

---

### Environment Model

| Feature | Description |
|---|---|
| Static obstacles | Placed by slider or by clicking the canvas |
| Moving obstacles | Bouncing dynamic obstacles |
| Wind field | Directional force with configurable turbulence |
| Packet loss | Per-communication-event drop probability |
| Communication delay | Configurable latency in simulation frames |
| Sensor noise | Gaussian position error on agent's own readings |
| GPS error σ | Corrupts perceived neighbor positions |
| Spawn modes | Random · Clustered · Center · Edge |

---

### Task Scenarios

| Task | Behavior | Objective |
|---|---|---|
| Search & Rescue | Exploration | Locate all hidden targets |
| Area Coverage | Exploration | Maximize arena coverage |
| Resource Foraging | Foraging | Collect & return all resources to nest |
| Target Tracking | Consensus | Surround and follow a moving target |
| Task Allocation | Leader–Follower | Self-organize distributed task assignment |
| Obstacle Navigation | Boids | Navigate through a dense obstacle field |

---

### Reproducibility

Every simulation run is seeded with a deterministic PRNG (Mulberry32). The seed is:

- Displayed in the HUD and topbar at all times
- Embedded in all exported configs and results
- Regeneratable with the **New Seed** button
- Fully deterministic: same seed + same params = identical run

Save and load complete configuration snapshots as JSON to reproduce any experiment exactly.

---

## Interface

```
┌─────────────────────────────────────────────────────────────────────────┐
│  SIRS  Swarm Intelligence Research Sandbox v3.0          [Controls]     │
├──────────────────────┬──────────────────────────────────┬───────────────┤
│  [Simulation]        │                                  │  Live Metrics │
│  [Algorithm Lab]     │                                  │               │
│  [Experiment]        │         Canvas                   │  Sparklines   │
│  [Environment]       │                                  │               │
│  [Tasks]             │    Sim | Density | Traj | Cov    │  Density Map  │
│  [Presets]           │                                  │               │
│                      │                                  │  Inspector    │
│  ─ Parameters ─      │                                  │               │
│  ─ Visualization ─   │                                  │  Sim Log      │
├──────────────────────┴──────────────────────────────────┴───────────────┤
│  Status · Algorithm · Task · Obstacles · Wind · Packet Loss · v3.0      │
└─────────────────────────────────────────────────────────────────────────┘
```

**Left panel tabs:**

- **Simulation** — algorithm selector, swarm parameters, visualization toggles
- **Algorithm Lab** — register custom behaviors, validate code, run A/B comparisons
- **Experiment** — parameter sweep builder, batch runner, results table
- **Environment** — obstacles, wind, sensor noise, GPS error, scenario save/load
- **Tasks** — named task scenarios with live progress tracking
- **Presets** — curated experiment presets (Murmuration, Vicsek Phase Transition, PSO, etc.)

**Canvas view tabs:**

- **Sim** — live agent rendering with trails, vectors, comm links
- **Density** — real-time spatial density overlay
- **Trajectories** — persistent path accumulation
- **Coverage** — explored area heatmap

---

## Canvas Interactions

| Action | Effect |
|---|---|
| `Click` empty space | Place a static obstacle |
| `Right-Click` near obstacle | Remove nearest obstacle |
| `Shift + Click` | Spawn predator agent at cursor |
| `Click` an agent | Select and inspect state vector |

---

## Experiment Presets

| Preset | Algorithm | Key Property |
|---|---|---|
| Starling Murmuration | Boids | High alignment, 300 agents |
| Vicsek Phase Transition | Vicsek | Noise-controlled order parameter |
| PSO Optimization | PSO | Global best convergence |
| Tactical Drone Swarm | Formation (Grid) | Decentralized lattice formation |
| Army Ant Foraging | ACO/Foraging | Stigmergic collection |
| Fish Schooling | Boids | High-density separation |
| Search & Rescue | Exploration | Frontier coverage, 10% packet loss |
| Predator Evasion | Boids | Disturbance agent response |
| Consensus Demo | Consensus | Order parameter convergence from disorder |
| Scatter & Reform | Split & Merge | Fission–fusion group dynamics |

---

## Running Locally

```bash
# Clone the repository
git clone https://github.com/Ma-Mar-Itan/Swarm-Intelligence-Playground.git

# Open directly in browser — no server needed
open index.html
# or
start index.html     # Windows
xdg-open index.html  # Linux
```

No `npm install`. No `pip install`. No build step. One file.

---

## Architecture

The simulator is a single self-contained HTML file structured as modular JavaScript:

```
index.html
│
├── PRNG           Mulberry32 seeded random number generator
├── Registry       Algorithm plugin system (registerBehavior API)
├── Spatial Hash   O(1) neighbor lookup via cell grid
├── Agent          Class with full kinematic + task state
├── Behaviors      11 built-in algorithms + unlimited custom plugins
├── Environment    Wind · obstacles · sensor noise · GPS error
├── Task System    6 named scenarios with progress tracking
├── Metrics        7 live metrics + sparklines + density map
├── Experiment     Sweep generation · headless batch runner · export
├── Renderer       Canvas 2D with trails · overlays · HUD
└── UI             6-tab left panel · right metrics panel · footer bar
```

The headless batch runner executes simulations off the main render loop using `async/await` with `setTimeout(0)` yields, keeping the UI responsive during long experiments.

---

## Research Use

This platform is suitable for:

- **Algorithm benchmarking** — compare any two registered behaviors over identical conditions with the built-in A/B comparison tool
- **Parameter sensitivity analysis** — sweep any parameter range and export full result matrices as CSV/JSON for analysis in Python, R, or MATLAB
- **Reproducible experiments** — deterministic seeding ensures any published result can be exactly reproduced
- **Emergent behavior study** — phase transitions (Vicsek noise threshold), cluster formation, coverage saturation, and consensus convergence are all measurable in real time
- **Education** — interactive demonstration of decentralized coordination, self-organization, and local-rule emergence

### Typical Python analysis workflow

```python
import pandas as pd
import matplotlib.pyplot as plt

# Load exported batch results
df = pd.read_csv('swarm_experiment_1720000000000.csv')

# Plot consensus Φ vs noise level
pivot = df.groupby('noise')['phi'].mean()
pivot.plot(title='Consensus Order Parameter vs Noise (Vicsek)')
plt.xlabel('Noise σ')
plt.ylabel('Order Parameter Φ')
plt.show()
```

---

## License

MIT © 2025 Ma-Mar-Itan

Permission is hereby granted, free of charge, to any person obtaining a copy of this software to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the software, subject to the standard MIT License conditions.

---

<div align="center">

**[Open the live app →](https://ma-mar-itan.github.io/Swarm-Intelligence-Playground/)**

*Built as a single HTML file. Zero dependencies. Runs everywhere.*

</div>
