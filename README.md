# [Delayed threshold and spatial diffusion in k-core percolation induced by long-range connectivity](https://www.nature.com/articles/s42005-025-02171-5)
Xin-Ya Zhang, Yi Yao, Zhiyu Han and Gang Yan. Communications Physics, 2025.

## Description
This repository contains Python code to simulate and analyze k-core percolation in spatially embedded networks, focusing on the impact of different long-range connectivity models on network robustness. This work investigates how geometric constraints influence both structural integrity (phase transitions, percolation thresholds) and spatial organization (diffusion, clustering) during the percolation process.

The primary goal is to compare two spatial connectivity models:

- **Geometric Scaling Law Model**: Connection probability decays purely as a power law with distance, i.e., $$p_{ij} \propto d_{ij}^{-\alpha}$$.
- **Exponential Decay Model**: Connection probability follows a power law combined with an exponential cutoff, i.e., $$p_{ij} \propto d_{ij}^{-\alpha} e^{-\lambda d_{ij}}$$ $\text{ (with } \alpha=1 \text{ in the paper/code)}$

Simulations are conducted on 2D grids with periodic boundary conditions, and analysis includes tracking the giant $k$-core component, determining percolation thresholds ($p_c$), measuring spatial spread ($\tilde{\sigma}$), and examining spatial cluster distributions.

## Environment Setup
It is recommended to use conda or venv to create an independent Python environment. The code was tested using Python >= 3.0.

Create and activate environment:
```bash
# Using conda (example)
conda create -n kcore_env python=3.9
conda activate kcore_env

# Or using venv (example)
python -m venv venv
source venv/bin/activate  
```

Install dependencies:
```bash
pip install numpy matplotlib networkx joblib numba scipy pickle jupyter
```


## Reproduction Workflow

### Step 1: Generate Simulation Data
This step runs the core simulations to generate network evolution data under different parameters.

**Scripts:**
- `scripts/kcore-percolation/powerlaw.py`: Simulates the Geometric Scaling Law model ($p_{ij} \propto d_{ij}^{-\alpha}$).
- `scripts/kcore-percolation/exponential_decay.py`: Simulates the Exponential Decay model ($p_{ij} \propto d_{ij}^{-1} e^{-\lambda d_{ij}}$).

**Configuration:**
Parameters inside the scripts must be modified before running, for example:
- `L`: Grid side length.
- `avg_degree`: Network average degree.
- `zetalist`: List of $\alpha$ values (for `powerlaw.py`) or $\lambda$ values (for `exponential_decay.py`) to simulate.
- `runs`: Number of independent runs per parameter combination.
- `ini_pp`: Initial node occupation probability $p$ at the start of each run.
- `end_p`: Ending node occupation probability $p$; the simulation loop stops when $p$ falls below this value.
- `step_p`: Step size by which the node occupation probability $p$ is decreased in each iteration.
**Execution:**
```bash
python scripts/kcore-percolation/powerlaw.py
python scripts/kcore-percolation/exponential_decay.py
```

**Output:**
Generates network topology files and simulation result data in the specified `save_path`.

### Step 2 (Optional): Calculate Spatial Spread Metric
This step calculates the k-core spatial spread metric $\tilde{\sigma}$ near the percolation threshold $p_c$.

**Script:**
- `scripts/kcore-percolation/spread_metric.py`

**Prerequisite:** Requires the network topology files generated in Step 1.

**Execution:**
```bash
python scripts/kcore-percolation/spread_metric.py
```

**Output:** 
Generates files containing the spatial spread metric results in the specified `save_path`.

### Step 3: Demo and Figures


The demo is available in [`scripts/demo/demo-plot.ipynb`](scripts/demo/demo-plot.ipynb).

This notebook performs the following tasks:

- Loads input data from the `./example` directory.
- Utilizes [`scripts/area-estimation/area_calculator.py`](scripts/area-estimation/area_calculator.py) for percolation area calculations, which are required for generating Figures 3 and 4.
- Reproduces Figures 1, 3, and 4 from the paper.

To run the demo, simply open the notebook and execute all cells. 
