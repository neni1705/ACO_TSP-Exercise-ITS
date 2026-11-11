# Independent Student Assignment — ACO Parameter Sensitivity on `eil76`

**Course:** SD234503 — Metode Optimasi Metaheuristik  
**Dataset:** TSPLIB95 `eil76` (76 cities, optimum 538)  
**Suggested duration:** 1 week  
**Deliverables:** completed `.ipynb` notebook and a 2–3 page PDF report (or an equivalent Markdown report in the notebook).

## Objective
Investigate how ACO parameters influence solution quality, convergence, and computational cost. Do **not** treat one stochastic run as sufficient evidence.

## Tasks
1. **Data and verification (10 points).** Load `eil76`, verify 76 cities, symmetric distances, zero diagonal, and explain the distance metric.
2. **Baseline (15 points).** Implement or reuse nearest-neighbor; run it from every possible starting city and report the best route length. Plot the best route.
3. **ACO implementation (20 points).** Reuse the Ant System implementation from the guided lab, verify valid Hamiltonian cycles, and plot a representative best tour and convergence curve.
4. **Controlled parameter study (25 points).** Use at least 3 levels each for $\alpha$, $\beta$, and $\rho$. Change one factor at a time around a fixed reference setting, with **at least 5 independent random seeds** per configuration. Keep ant count, iteration budget, and $Q$ fixed. State every setting and seed.
5. **Performance and visualization (20 points).** Report mean, standard deviation, minimum best-tour length, mean optimality gap (%), and mean runtime per configuration. Include a results table and at least two plots (e.g. parameter vs mean gap, convergence curves).
6. **Interpretation and reproducibility (10 points).** Discuss exploration vs exploitation, whether improvements are consistent across seeds, limitations, and how to reproduce your results.

## Suggested reference configuration
`n_ants=20`, `n_iterations=100`, `alpha=1.0`, `beta=3.0`, `rho=0.5`, `Q=100`, `seeds=[11,22,33,44,55]`. These are **experimental starting values**, not proven optimal parameters.

Suggested levels: $\alpha\in\{0.5,1,2\}$, $\beta\in\{1,3,5\}$, $\rho\in\{0.1,0.5,0.9\}$. This is one-factor-at-a-time; explain why interactions are not fully assessed. Optional bonus: a small factorial design or 2-opt local search (+10).

## Report questions
- Which parameter appears most influential, under the chosen experimental budget?
- Does a lower mean distance always correspond to lower run-to-run variability?
- How much better is ACO than nearest-neighbor on this instance?
- Did any run reach the published optimum? What does the optimality gap mean?
- What are the limitations of one instance and five seeds?

## Submission
Submit `StudentID_Name_ACO_Eil76.ipynb` and `StudentID_Name_ACO_Report.pdf` through the course submission channel specified by the instructor. All code cells must run in order from a fresh runtime. Do not submit fabricated or manually altered results.
