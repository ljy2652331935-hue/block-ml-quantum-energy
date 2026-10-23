# Method and Research Scope

[Back to the project overview](README.md)

## Physical Models

The original project studies the transverse-field Ising model, schematically:

```text
H_TFIM = -sum_<ij> J_ij sigma_i^z sigma_j^z - sum_i h_i sigma_i^x
```

The Rydberg extension uses an effective two-level array Hamiltonian of the form:

```text
H_Rydberg = sum_i (Omega_i/2) sigma_i^x
            - sum_i Delta_i n_i
            + sum_i<j V_ij n_i n_j
V_ij = C6 / |r_i-r_j|^6
```

The convention for sigma, the definition of n, units, boundaries and the interaction model must be matched when importing reference data. Different atomic species are not automatically interchangeable: transfer would require a validated common effective Hamiltonian and a suitable parameter range.

## From Local Blocks to a Global Energy Estimate

1. Partition the occupied sites into disjoint blocks under a local computational budget.
2. Compute local energies and selected quantum-state observables.
3. Represent the block layout and physical interactions between blocks.
4. Correct a physics baseline using tabular regression, a graph model or a deliberately simple fixed-coefficient control.

The original isolated-block baseline is a sum of local block ground-state energies. It generally omits cross-block contributions and is not the coupled system's energy. The residual target is defined against a trusted numerical reference, often normalized by cut-bond count or total cut-interaction strength.

In the later CMF branch, each block sees a self-consistent mean environment. Its total energy already includes mean-field cross-block contributions. The learned or empirical correction must therefore be defined relative to CMF rather than adding the same mean-field terms again.

## What Has Been Implemented

- Ridge and tree-based residual models with normalized block, contact, orientation and geometry features.
- A PyTorch block-graph message-passing network using local block observables and inter-block Rydberg interaction statistics.
- Local block-state features, including occupation-related and correlation-related information.
- Larger-module and adaptive-partition experiments, with later exact local blocks capped at 16 atoms.
- Multiple-start CMF, fixed-correction partition controls and numerical-reference convergence checks.

This is a family of research experiments, not one universally best final model. In particular, the existing small-block GNN representation should not be described as an already-validated arbitrary-shape 16-site architecture.

## The Question Still Open

Does a variable-size representation of local quantum states and boundary environments improve accuracy and data efficiency on unseen geometries and parameters, beyond what geometry alone can explain?

The necessary comparisons are:

- A physics baseline with no learned correction.
- Cheap size/finite-size and boundary baselines.
- Tabular and geometry-only graph models.
- State-aware block graph models at matched label and compute budgets.

All partitions and related views of one Hamiltonian should remain in the same data split. Partition-consistency regularization is a possible training aid, not evidence of accuracy by itself.

## Scope of the Claim

The intended application is screening many parameter/geometry choices cheaply, followed by trusted-solver checks of important candidates. The current work does not establish an exact solver, quantum advantage, universal replacement for DMRG/MPS, arbitrary-material transfer or a generally monotonic benefit from using larger blocks.

Novelty must be established against existing fragment-based, local-energy and graph-learning methods. The contribution being tested is the value of local quantum-state and boundary representations under controlled generalization and resource constraints.
