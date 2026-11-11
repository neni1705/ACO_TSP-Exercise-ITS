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


## Dataset Description: Berlin52

### 1. About the Dataset

This exercise uses **berlin52.tsp**, a benchmark dataset from the
TSPLIB95 collection for studying the Traveling Salesman Problem (TSP).

The dataset represents 52 locations in Berlin, Germany. Each location
is described by two-dimensional coordinates (X, Y).

The objective is to find the shortest possible route that visits
all 52 locations exactly once and returns to the starting location.

**Dataset source:**
[Berlin52 – TSPLIB95](https://github.com/pdrozdowski/TSPLib.Net/blob/master/TSPLIB95/tsp/berlin52.tsp)

### 2. Dataset Characteristics

| Property | Description |
|---|---|
| Dataset | berlin52.tsp |
| Source | TSPLIB95 |
| Problem type | Symmetric Traveling Salesman Problem |
| Number of locations | 52 |
| Data representation | Two-dimensional coordinates (X, Y) |
| Distance type | EUC_2D (rounded Euclidean distance) |
| Known optimal tour length | 7,542 |
| Optimization objective | Minimize the total tour distance |

The dataset is symmetric, meaning that the distance from location
i to location j is equal to the distance from j to i.

**Important:** The dataset does not contain actual road distances,
traffic conditions, or travel times. The coordinates are used
to calculate mathematical distances between locations.

### 3. Dataset Structure

The TSPLIB file contains general information about the problem,
followed by the coordinates of each location.

Example:

```text
NAME: berlin52
TYPE: TSP
COMMENT: 52 locations in Berlin (Groetschel)
DIMENSION: 52
EDGE_WEIGHT_TYPE: EUC_2D
NODE_COORD_SECTION
1 565.0 575.0
2 25.0 185.0
3 345.0 750.0
4 945.0 685.0
5 845.0 655.0
...
EOF
```

Each row in `NODE_COORD_SECTION` contains:

- **Node ID:** Unique identifier for a location.
- **X-coordinate:** Horizontal position.
- **Y-coordinate:** Vertical position.



### 4. Distance Calculation

The distance between two locations is calculated using
two-dimensional Euclidean distance, rounded to the nearest integer:

$$
d_{ij} = \mathrm{round}\left(
\sqrt{(x_i-x_j)^2 + (y_i-y_j)^2}
\right)
$$

where:

- $d_{ij}$: Distance between locations $i$ and $j$.
- $(x_i,y_i)$: Coordinates of location $i$.
- $(x_j,y_j)$: Coordinates of location $j$.
- $\mathrm{round}$: Rounding to the nearest integer.

For example:

- Location 1: $(565,575)$
- Location 2: $(25,185)$

The Euclidean distance is:

$$
d_{12} = \sqrt{(565-25)^2 + (575-185)^2}
$$

$$
d_{12} = \sqrt{540^2 + 390^2}
\approx 666.1
$$

Therefore, the rounded distance is:

$$
d_{12} = 666
$$

**Note:** Distances are measured in coordinate units,
not necessarily kilometers.


### 5. Optimization Problem

The objective is to minimize the total distance of a
closed tour visiting all 52 locations exactly once.

$$
\min_{\pi} L(\pi) =
\sum_{k=1}^{n}
d_{\pi_k,\pi_{k+1}}
$$

subject to:

$$
\pi_{n+1} = \pi_1
$$

where:

- $n=52$: Number of locations.
- $\pi$: A permutation representing the visiting order.
- $d_{\pi_k,\pi_{k+1}}$: Distance between consecutive locations.
- $\pi_{n+1}=\pi_1$: The tour returns to its starting location.

### 7. Performance Evaluation

The published optimal tour length for Berlin52 is **7,542**.

The optimality gap is calculated as:

$$
\text{Optimality Gap (\%)} =
\frac{L_{\text{algorithm}}-7542}{7542}
\times 100
$$

where $L_{\text{algorithm}}$ is the total route distance
produced by Nearest-Neighbor or ACO.

**Interpretation:**

- **0% gap:** The algorithm reaches the known optimal distance.
- **Small positive gap:** The solution is close to the optimum.
- **Large positive gap:** The solution is relatively far from the optimum.

Because ACO is stochastic, different random seeds or parameter
settings may produce different solutions.

The goal is not only to find a short route but also to understand
how pheromone-based search improves optimization performance.


**Important:** Use `problem.get_weight(i,j)` for TSPLIB distances. Straight-line floating-point distances may not reproduce published optimum values. The notebook uses official integer edge weights. ACO is stochastic and is not guaranteed to reach the optimum.

## Data attribution
TSPLIB95, G. Reinelt, University of Heidelberg: https://comopt.ifi.uni-heidelberg.de/software/TSPLIB95/

## Assignment
Read [ASSIGNMENT.md](ASSIGNMENT.md) for tasks, required figures, reporting format, and rubric.
