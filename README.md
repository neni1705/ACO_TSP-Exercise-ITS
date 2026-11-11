# Ant Colony Optimization for the Traveling Salesman Problem

**Course:** SD234503 — Metode Optimasi Metaheuristik  
**Language:** English  
**Environment:** Jupyter Notebook / Google Colab

## Learning objectives
1. Formulate symmetric TSP and calculate a route length.
2. Implement Ant System (AS): probabilistic transitions, pheromone evaporation, and pheromone deposition.
3. Compare ACO with nearest-neighbor and a known optimum on the TSPLIB `berlin52` benchmark.
4. Design a reproducible parameter-sensitivity experiment on `eil76` (independent student assignment).

## Repository layout
```text
ACO-TSP-Optimization/
├── README.md
├── ASSIGNMENT.md
├── requirements.txt
├── datasets/
│   └── README.md
├── notebooks/
│   ├── ACO_TSP_Guided_Berlin52.ipynb
│   └── ACO_TSP_Assignment_Eil76.ipynb
└── results/
    └── .gitkeep
```

## Run in Colab
Upload the notebooks to Google Colab, or open them from GitHub. Each notebook installs its own Python dependencies. Run cells from top to bottom. The first execution downloads the TSPLIB data, so internet access is needed. Alternatively, download the `.tsp` files from the official TSPLIB site and upload them to the notebook working directory.

- **Guided lab:** `notebooks/ACO_TSP_Guided_Berlin52.ipynb` (~90–120 min)
- **Independent assignment:** `notebooks/ACO_TSP_Assignment_Eil76.ipynb` (suggested one week)

## Mathematical formulation
Given distances $d_{ij}$, find a permutation $\pi$ minimizing

$$L(\pi)=\sum_{k=1}^{n-1}d_{\pi_k,\pi_{k+1}}+d_{\pi_n,\pi_1}.$$

For an ant at city $i$, the probability of visiting unvisited city $j$ is

$$p_{ij}=\frac{\tau_{ij}^{\alpha}(1/d_{ij})^{\beta}}{\sum_{k\in A_i}\tau_{ik}^{\alpha}(1/d_{ik})^{\beta}},\quad j\in A_i.$$

After all ants construct tours, pheromone is updated by

$$\tau_{ij}\leftarrow(1-\rho)\tau_{ij}+\sum_a\Delta\tau_{ij}^{(a)},\qquad\Delta\tau_{ij}^{(a)}=Q/L_a$$

for each edge used by ant $a$ (zero otherwise).

## Benchmarks
| Instance | Cities | Published optimal length | Metric |
|---|---:|---:|---|
| berlin52 | 52 | 7542 | TSPLIB EUC_2D |
| eil76 | 76 | 538 | TSPLIB EUC_2D |

**Important:** Use `problem.get_weight(i,j)` for TSPLIB distances. Straight-line floating-point distances may not reproduce published optimum values. The notebook uses official integer edge weights. ACO is stochastic and is not guaranteed to reach the optimum.

## Data attribution
TSPLIB95, G. Reinelt, University of Heidelberg: https://comopt.ifi.uni-heidelberg.de/software/TSPLIB95/

## Assignment
Read [ASSIGNMENT.md](ASSIGNMENT.md) for tasks, required figures, reporting format, and rubric.
