# Monte Carlo Simulation for Warranty Penalty Time

Official Python repository for the Monte Carlo simulation framework evaluating the penalty time, as presented in the paper **"Moving Beyond Simulation: Analytical Derivation of the Warranty Penalty"**.

## Overview
This repository contains the computational baseline used to estimate the expected value of the penalty time. The cumulative penalty time ($P_{Time}$) is rigorously characterized as a **Compound Poisson Process**, where failures follow a homogeneous Poisson process and each jump corresponds to the excess repair time beyond a fixed threshold ($\tau$).

This simulation leverages vectorized array operations in `NumPy` to process extreme-scale scenarios (e.g., $M = 50,000$ iterations) efficiently, empirically validating the exact analytical derivations proposed in the study.

## Repository Contents
* `PENALTY_TIME_SIMULATION_ING.ipynb`: The main Jupyter Notebook containing the Monte Carlo simulation loop and data export pipeline.
* `requirements.txt`: List of Python dependencies and library versions required to reproduce the environment.
* `README.md`: Project documentation.

## Computational Mapping
To ensure transparency, the following table maps the stochastic mathematical components from the paper to their respective Python/NumPy implementations:

| Algorithm Step | Mathematical Component | Python/NumPy Implementation | Computational Role |
| :--- | :--- | :--- | :--- |
| **1. Arrival Process** | $N(L) \sim \text{Poisson}(\lambda L)$ | `np.random.poisson(lambda_model * l_residual)` | Generates the discrete number of product failures over the coverage period. |
| **2. Repair Times** | $Y_i \sim \text{Exponential}(\mu)$ | `np.random.exponential(scale=(1 / mi_model), size=n_failures)` | Generates a vectorized array of independent repair times. |
| **3. Penalty Magnitude** | $Z_i = \max(0, Y_i - \tau)$ | `np.maximum(0, repair_times - tau)` | Evaluates each repair duration against the threshold ($\tau$), isolating positive excess times. |
| **4. Cumulative Penalty** | $P_{Time} = \sum_{i=1}^{N(L)} Z_i$ | `np.sum(...)` | Aggregates the individual penalty magnitudes to compute the total penalty. |

*Note: The pseudo-random number generator is initialized with a fixed seed (`seed=42`) to guarantee strict scientific reproducibility.*

## How to Run
To reproduce the simulation and generate the datasets used in the study, follow these steps:

1. **Clone the repository:**
   ```bash
   git clone [https://github.com/santoshenrique2021/penalty-time-simulation.git](https://github.com/santoshenrique2021/penalty-time-simulation.git)
   ```


2. **Install the dependencies:**
Ensure you have Python installed, then install the required packages:

   ```bash
   pip install -r requirements.txt
   ```

3. **Execute the Notebook:**   
Open the PENALTY_TIME_SIMULATION_ING.ipynb file using Jupyter Notebook or JupyterLab.

4. **Run Sequentially:**
Run the cells from top to bottom. The script will automatically iterate through the predefined Monte Carlo sample sizes ($M \in \{500, 1000, 2000, 5000, 10000, 50000\}$) and generate the corresponding .csv output files in the root directory.