# Discrete-Event Simulation of a Tool Rental Service

A discrete-event simulation (DES) of a tool rental shop, written in Python from scratch (no simulation library). It models customers arriving, renting tools, using and returning them, and staff doing maintenance and cleaning. The goal is to compare **three staffing policies** by customer waiting time and staff workload.

> B.Sc. course project (Simulation).

## Highlights

- **Event-scheduling engine**: a future event list (FEL) built on a priority queue (`heapq`), with handlers for arrival, rental, return, maintenance and cleaning events
- **Custom random number generator**: a Linear Congruential Generator (LCG) with `a = 1664525`, `c = 1013904223`, `m = 2^32`
- **Random variate generation**: normal service times made with the Box–Muller transform (truncated at 0.1 min)
- **Statistical design**: 30 independent replications per scenario, with a different seed for each, and results averaged across runs
- **KPIs**: average customer wait, share of customers waiting more than 5 minutes, average queue length, mean rental, maintenance and cleaning times, and task counts per worker

## Model

| Parameter | Distribution (minutes) |
|---|---|
| Customer inter-arrival time | Normal(μ = 30, σ² = 900) |
| Rental service time, worker K | Normal(14, 16) |
| Rental service time, workers M / R | Normal(10, 25) |
| Tool usage duration | Normal(60, 3600) |
| Tool return handling | 2 (constant) |
| Maintenance time | Normal(6, 16) |
| Cleaning time | Normal(10, 36) |

Customers arrive until **t = 600 min**, and the shop processes remaining events until **t = 660 min**. Returns get the highest priority, then rentals, then maintenance and cleaning.

## Scenarios

| Scenario | Staffing policy |
|---|---|
| **1** (`part1.py`) | Two workers (K, M). Both handle returns and rentals; K also does maintenance. |
| **2** (`part2.py`) | Two workers. K is the first choice for rentals; M rents when free and also does maintenance. |
| **3** (`part3.py`) | Three workers. M only rents; K does maintenance and cleaning; a floating worker R helps with rentals first, then with maintenance and cleaning. |

## Results

Mean over 30 replications (you get these numbers by running the scripts):

| Scenario | Avg wait (min) | Customers waiting > 5 min | Avg queue length |
|---|---|---|---|
| 1 | 1.01 | 7.3% | 0.39 |
| 2 | 1.21 | 9.1% | 0.51 |
| 3 | **0.61** | **3.8%** | **0.15** |

Adding the floating worker (Scenario 3) cuts the average wait by about 40% and halves the share of long waits compared with Scenario 1.

## How to run

```bash
pip install -r requirements.txt
python part1.py   # writes results_part1.csv
python part2.py   # writes results_part2.csv
python part3.py   # writes results_part3.csv
```

Each script prints a table with all 30 runs plus a summary row, and saves it as CSV.

## Repository structure

```
part1.py, part2.py, part3.py                  # Simulation for scenarios 1–3
partone.ipynb, partsec.ipynb, partthird.ipynb  # Notebook versions of the same scenarios
requirements.txt
```

## Authors

- **[@panizotaghi](https://github.com/panizotaghi)**
- **[@Hussmart](https://github.com/Hussmart)**

This is a joint project. It was first published at [Hussmart/Discrete-event-queue-simulator](https://github.com/Hussmart/Discrete-event-queue-simulator).
