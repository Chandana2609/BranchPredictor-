# README - Custom Branch Predictor Project (ChampSim)

#Problem Statement : Global History branch predictor uses a pattern history table(PHT) to predict the appropriate action (branch taken or not taken). In some cases two or more branches may index into the same entry in the PHT, which can lead to one branch overriding the learned pattern of another branch. Create a new branch predictor that solves this problem without using a substantial amount of space. Compare it to already existing global history branch predictors. Justify your findings.

# Project Overview

This project involved implementing custom global branch predictors inside the ChampSim simulator. I worked on two custom predictors:

- mybranchpred: A basic custom predictor using global history.
- mybranchpred2: An improved version using XOR-based indexing and longer global history to reduce PHT aliasing.

These were compared against standard predictors like bimodal, gshare, and binomial.

# Trace Files Used

We ran simulations on the following benchmark traces from the DPC-3 trace set:

- `403.gcc-16B.champsimtrace.xz`
- `445.gobmk-17B.champsimtrace.xz`

Each predictor was tested across 10M, 20M, and 140M instruction windows to evaluate how they perform at different execution lengths.

# How to Build and Run

To build ChampSim with a custom predictor:

```bash
./build_champsim.sh mybranchpred no no no no lru 1
./build_champsim.sh mybranchpred2 no no no no lru 1
```

To run simulations:

```bash
./run_champsim.sh ./bin/champsim_mybranchpred 10M 403.gcc-16B.champsimtrace.xz
```

Repeat the above command with different traces and instruction window lengths (10M, 20M, 140M).

---

# Results Summary

# **gcc trace**

| Predictor     | Accuracy (20M) | MPKI (20M) | ROB (20M) | Accuracy (140M) | MPKI (140M) | ROB (140M) |
| ------------- | -------------- | ---------- | --------- | --------------- | ----------- | ---------- |
| bimodal       | 99.627         | 0.73225    | 44.5555   | 99.65           | 0.684       | 45.38      |
| gshare        | 99.7443        | 0.502      | 56.564    | 99.78           | 0.42        | 63.09      |
| mybranchpred  | 99.7298        | 0.53035    | 53.9266   | 99.78           | 0.431       | 62.77      |
| mybranchpred2 | 99.8087        | 0.3756     | 65.9054   | 99.89           | 0.203       | 114.955    |

# **gobmk trace**

| Predictor     | Accuracy (20M) | MPKI (20M) | ROB (20M) | Accuracy (140M) | MPKI (140M) | ROB (140M) |
| ------------- | -------------- | ---------- | --------- | --------------- | ----------- | ---------- |
| binomial      | 86.18          | 24.74      | 18.5      | 86.22           | 24.44       | 18.01      |
| gshare        | 84.32          | 28.06      | 14.79     | 85.59           | 25.55       | 16.07      |
| mybranchpred  | 84.189         | 28.31      | 14.69     | 85.53           | 25.67       | 15.98      |
| mybranchpred2 | 91.607         | 15.031     | 29.06     | 94.47           | 9.8         | 42.85      |

