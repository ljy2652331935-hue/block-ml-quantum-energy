# Block-ML: Local Quantum Physics for Energy Surrogates

**Jiayi Liu | Independent research project | Research in progress**

Public overview prepared on 23 October 2026. Numerical results below refer to saved experiments and analysis through 14 September 2026, not new October runs.

## The Question

Can inexpensive calculations on small quantum blocks, combined with a learned correction for interactions between them, estimate the ground-state energy of larger systems on modest hardware?

The project began with the two-dimensional transverse-field Ising model (TFIM) and developed into a study of Rydberg-array energy surrogates, block-state features, graph neural networks and geometry-aware partitioning. The aim is an efficient energy estimator, not an exact many-body solver or a replacement for a full quantum state.

## The Method

```text
Geometry and Hamiltonian parameters
                  |
       Partition into small blocks
                  |
    Local quantum calculations / cluster mean field
                  |
  Block states + boundary geometry + interactions
                  |
          Residual correction
                  |
        Predicted total energy
```

The shared principle is **E_pred = E_physics + residual correction**. The physics baseline has evolved from a sum of isolated-block energies to cluster mean field (CMF), which includes a self-consistent mean environment. Corrections must be defined relative to the chosen baseline to avoid double counting.

The research includes tabular residual models and a PyTorch block-graph message-passing network. Recent partition experiments deliberately used a fixed, simple correction so that changes could be attributed to the partition and local calculation, rather than retraining a neural network.

![Actual 25- and 30-site partitions, including L-shaped remainder blocks](block-partitions.png)

*Actual partitions from the two-edge pilot. Colours identify blocks; numbers in the headings are atom counts per block. The local exact-solver budget was at most 16 atoms. Empty space is not filled with fictitious atoms.*

## What Has Been Done

| Stage | Completed work |
|---|---|
| TFIM foundation | Exact local blocks; contact and orientation features; Ridge and tree-based residual models; size/geometry validation studies. |
| Rydberg extension | Long-range interactions; local quantum-state descriptors; block-graph GNN experiments; active-label and validation-selected blend studies. |
| Larger blocks and boundaries | Blocks up to 16 atoms; self-consistent CMF; large-block-first, connected-remainder and cost-aware partition comparisons. |
| Reference-data analysis | Audited an external energy workbook containing 423 configurations, including 212 workbook-qualified approximate MPS labels. |

## Representative Results

**A useful improvement on strips.** On 18 development configurations with N = 28, 29, 30, 31, 37 and 38, grouping the remainder into a connected block reduced total-energy MAE from **0.026513 to 0.014737**, a **44.4%** reduction. The correction coefficient and five starting states were fixed. Four of the 18 cases nevertheless became worse.

**An important limit on new shapes.** On six 5x5/5x6 configurations, the same idea improved the uncorrected CMF baseline MAE by **32.0%**, but the corrected prediction MAE increased from **0.019158 to 0.022597**. Better local physics therefore did not automatically produce a better calibrated energy estimator.

Both comparisons use approximate MPS references and total energies in units with Omega = 1. They concern different panels and are not a single model leaderboard. The strip panel reused development references; the two-edge panel was a prospective development pilot, not the original final test.

**A clue from larger reference arrays.** Four-row N50-100 data show almost constant energy increments when a complete column of four atoms is added, while single-atom increments follow the end-cap geometry. This motivates a cheap finite-size/boundary baseline against which more expressive ML must be compared. It is an exploratory observation, not a universal scaling law.

The external reference workbook reaches **150 atoms only in selected qualified geometries**, including 5x30 at Delta = 0.5. This is reference coverage, **not a claim that Block-ML has been validated on arbitrary 150-atom systems**.

[Detailed results and evidence limits](RESULTS.md) | [Method and research scope](METHOD.md)

## What This Suggests

The most promising research question is whether **local quantum-state and boundary information adds transferable value beyond geometry alone**, at a matched reference-data and computational budget. Existing fragment-based and graph-based methods mean that simply combining blocks and a GNN is not, by itself, a sufficient novelty claim.

## Next Steps

1. Audit the external reference-generation code and selected saved MPS states before integrating that data into training.
2. Compare cheap size/boundary baselines, geometry-only models and state-aware block models under the same protocol.
3. Freeze candidates, then test new boundary shapes, positional disorder and non-uniform parameters, grouping all partitions of one Hamiltonian into the same data split.

**Discussion sought:** the strongest physical interpretation, the most decisive next validation experiment, and an appropriate publication scope. This is a progress summary, not a peer-reviewed publication or a claim of state-of-the-art performance.

## About This Repository

This repository intentionally contains only a concise research overview, selected aggregate results and figures. Full source code, raw datasets, model checkpoints, manuscript drafts and protected final-test material are not part of this public release. It is not a standalone reproducibility package.
