# Modularity–controllability coupling reveals optimal regulation of information diffusion
The codes and data supporting our paper “Modularity–controllability coupling reveals optimal regulation of information diffusion” by Jingfang Fan are publicly available.

All C++ and Python scripts used in the analysis can be also accessed at: https://github.com/fanjingfang/MOCOF


## Overview

The MOCOF framework investigates how network community structure (modularity) interacts with control parameters to govern information diffusion dynamics. The project implements:

- **Theoretical Analysis**: Tree-like (TL) approximation and Mean-field (MF) solutions
- **Numerical Simulations**: Monte Carlo simulations on synthetic and real-world networks
- **Phase Diagram Generation**: Identification of three diffusion regimes
- **Optimal Controllability Analysis**: Cost function optimization

## Repository Structure

```
MOCOF_Source_code/
├── README.md
├── ER-ER-ER/              # Erdos-Renyi modular networks
├── SF-SF-SF/              # Scale-Free modular networks
├── ER-SF-ER/              # Hybrid ER-SF modular networks
├── REAL_WORLD_SIM/        # Real-world network simulations
└── append_S9S10/          # Supplementary analysis (Appendix S9 & S10)
```

## Detailed Structure

### ER-ER-ER/ (Erdos-Renyi Networks)

- `scr/Analysis Solution (TL & MF)/` - Analytical solutions (Python)
  - `custom_functions.py` - TL & MF calculation functions
  - `main.ipynb` - Main analysis notebook
- `scr/Numerical simulation/ER/` - Monte Carlo simulations (C++)
  - `network.cpp` - Main simulation code
  - `network.h` - Header with parameters
  - `mt19937ar.*` - Mersenne Twister RNG
- `data/` - Simulation output data
- `document/` - Generated figures and notebooks

### SF-SF-SF/ (Scale-Free Networks)

- `TL solution/` - Analytical solutions for SF networks
  - `custom_functions2.py` - TL solutions with power-law degree
  - `SF.py` - SF network utilities
- `Numerical Simulation/` - Monte Carlo simulations
  - `network.cpp`, `network.h` - Simulation code
  - `makefile` - Build configuration
  - `MPI.sh` - Job submission script
- `data/`, `SimulationData/`, `AnalysisData/` - Data files
- `sfsfsf_pd.ipynb` - Phase diagram notebook

### ER-SF-ER/ (Hybrid Networks)

- `TL solutions/` - Analytical solutions
- `Numerical Simulation/` - Monte Carlo simulations
- `data/`, `SimulationData/`, `AnalysisData/` - Data files
- `cost.ipynb` - Cost function analysis

### REAL_WORLD_SIM/ (Real-World Networks)

- `src/` - Source code
  - `Biased_community.py` - Biased community assignment (main method)
  - `Random_community.py` - Random community assignment (baseline)
  - `remap_community_edge.py` - Edge remapping utility
  - `Friendster/`, `YouTube/`, `Orkut/` - Network-specific simulations
- `data/` - Simulation results for each network
- `append_distribution/` - Degree distribution analysis (Appendix S9)
- `Figures/` - Generated figures
- `Plot_Notebook.ipynb` - Main plotting notebook

### append_S9S10/ (Supplementary Analysis)

- `main_append_S9S10.ipynb` - Analysis for Appendix S9 & S10
- `custom_functions2.py`, `SF.py` - Utility functions
- `random-regular/` - Random regular network analysis

## Requirements

### Python
```bash
pip install numpy scipy matplotlib h5py scikit-learn tqdm jupyter
```

### C++
- C++ compiler with C++11 support
- MPI library (OpenMPI or MPICH)

## Quick Start

### 1. Analytical Solutions (Python)

```python
from custom_functions import TL

params = {
    'theta': 0.2,    # Adoption threshold
    'mu': 0.2,       # Modularity parameter
    'w1': 1.0,       # Intra-community transmissibility
    'w2': 0.5        # Inter-community transmissibility
}

rho, iterations = TL(params)
print(f"Final density: rho_A = {rho[0]:.4f}, rho_B = {rho[1]:.4f}")
```

### 2. Numerical Simulations (C++)

```bash
# Compile
cd SF-SF-SF/Numerical\ Simulation/
make

# Run with MPI
mpirun -np 20 ./network
```

### 3. Real-World Network Analysis

```bash
# Preprocess community labels
cd REAL_WORLD_SIM/src/
python Biased_community.py

# Run simulations and generate figures
jupyter notebook ../Plot_Notebook.ipynb
```

## Model Parameters

| Parameter | Symbol | Description | Default |
|-----------|--------|-------------|---------|
| Network size | N | Total number of nodes | 200,000 |
| Average degree | z | Mean degree per node | 20 |
| Initial density | rho_0 | Initial adopter fraction | 0.17 |
| Threshold | theta | Adoption threshold | 0.1 |
| Modularity | mu | Inter-community link fraction | Variable |
| Intra-transmissibility | omega_1 | Intra-community influence weight | Variable |
| Inter-transmissibility | omega_2 | Inter-community influence weight | Variable |
| SF exponent | lambda | Power-law degree exponent | 2.5, 3.0, 3.5 |

## Data Formats

### Simulation Output (.out files)
```
# Each line: rho_A rho_B
0.8234567890 0.1234567890
...
```

### HDF5 Files (.h5)
```python
import h5py
f = h5py.File('data.h5', 'r')
w1_list = f['w1'][()]
w2_list = f['w2'][()]
rho_inf = f['rho'][()]
f.close()
```

## Reproducing Paper Figures

| Figure | Location | Notebook |
|--------|----------|----------|
| Fig. 2-3 | `ER-ER-ER/document/` | `figs_on_manuscript.ipynb` |
| Fig. 4 | `REAL_WORLD_SIM/Figures/` | `Plot_Notebook.ipynb` |
| Fig. S3-S4 | `SF-SF-SF/` | `sfsfsf_pd.ipynb` |
| Fig. S5-S6 | `ER-SF-ER/` | `cost.ipynb` |
| Fig. S9 | `REAL_WORLD_SIM/append_distribution/` | `Network_mu_and_Degree_Distribution.ipynb` |
| Fig. S10 | `append_S9S10/` | `main_append_S9S10.ipynb` |

## Real-World Datasets

Data sourced from [Stanford SNAP](https://snap.stanford.edu/data/):
- **Friendster**: Social network with ground-truth communities
- **YouTube**: Video sharing social network
- **Orkut**: Online social network

### Community Assignment Strategies
1. **Biased Assignment**: Overlapping nodes assigned to smaller community (main method)
2. **Random Assignment**: Overlapping nodes randomly assigned (baseline)

## Authors

- Xiaojie Chen (Beijing Normal University; Hubei University)
- Meiling Xie (Beijing Normal University)
- Jun Meng (Chinese Academy of Sciences)
- Sheng Fang (Beijing Normal University)
- Xiaosong Chen (Zhejiang University; Beijing Normal University)
- Jürgen Kurths (Potsdam Institute for Climate Impact Research; Humboldt University)
- Jan Nagler (Frankfurt School of Finance & Management)
- Jingfang Fan (Beijing Normal University; Potsdam Institute for Climate Impact Research)

## Citation

```bibtex
@article{mocof2025,
  title={Modularity-controllability coupling reveals optimal regulation of information diffusion},
  author={Chen, Xiaojie and Xie, Meiling and Meng, Jun and Fang, Sheng and Chen, Xiaosong and Kurths, J{\"u}rgen and Nagler, Jan and Fan, Jingfang},
  journal={},
  year={2025}
}
```

## License

MIT License

## Contact

For questions or collaborations, please contact:

**Jingfang Fan** - jingfang@bnu.edu.cn

School of Systems Science, Beijing Normal University
