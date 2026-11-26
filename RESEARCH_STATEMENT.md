# Auditing Rollout Feedback in Constrained Neural Operators (CU-NO)

**Chun Tang** · Kunpeng Sovereign Lab · [ORCID: 0009-0008-4128-2071](https://orcid.org/0009-0008-4128-2071) · [GitHub: ariz32601-ksl](https://github.com/ariz32601-ksl)

When a learned fluid simulator drifts, does feeding a physics correction back into the next step improve later forecasts more than correcting only the outputs we score? Zhou, Xian, and Yang reported that their constrained neural operator (PCM-NO) achieved lower periodic-flow velocity error than per-step state projection (FinalProj) despite nearly identical divergence at step 50 (Learning Autoregressive Neural Operators on Solver-Defined Physical Constraint Manifolds, [arXiv:2611.19753v1](https://arxiv.org/html/2611.19753v1#S5), §5.2, Table 1). Both of their methods fed corrected states back into the rollout. To isolate the effect of feedback, our **Constrained U-shaped Neural Operator (CU-NO)** pilot on periodic shear flow holds the FNO checkpoint and Helmholtz–Hodge projection fixed. In both projected modes, we score the corrected velocity, but only Mode C feeds it back into the next step.

### Experiment 0 Pilot (Single DGX Spark, `FNO` Baseline)

Although `CU-NO` targets multiscale U-shaped Neural Operators (`U-NO`), Experiment 0 tests feedback first on a Fourier Neural Operator (`FNO`) baseline in PyTorch `neuraloperator`. On one NVIDIA DGX Spark, we will train a compact `FNO` on 2D periodic incompressible shear flow from *The Well* (Ohana et al., [arXiv:2412.00568v3](https://arxiv.org/abs/2412.00568v3)), pinning the **Flatiron `shear_flow` `v2` dataset** in a clean directory and holding out one Reynolds number across all Schmidt values as a pilot rather than a full regime map.

From one frozen, validation-selected checkpoint and identical initial histories, we will compare:

1. **Mode A (Raw rollout, raw scoring):** unprojected state feedback; raw velocity scored.
2. **Mode B (Raw rollout, projected scoring):** unprojected state feedback; Helmholtz-Hodge projection applied only to scored velocity outputs.
3. **Mode C (Projected feedback, projected scoring):** velocity projected in physical units before re-normalizing for next-step feedback; projected velocity scored.

Modes B and C should agree on the first scored forecast. Any differences on later steps isolate the effect of feeding back the correction. We will specify the primary late-horizon velocity error metric before testing, then report divergence and failed rollouts separately. Follow-on work will test `U-NO` backbones and projection-in-the-loop training. We will release the frozen checkpoint, run configurations, and code to reproduce the rollout plot, including forecast error and failed-rollout counts.

### Completed Verification Track Record

This plan follows our process-isolated molecular assembly audit (`assembly-theory==0.6.1`), which separated heuristic estimates from exact solver results, flagged `u32::MAX` overflow sentinels in the production ledger rather than in the released **626,131-record COCONUT 2.0 cohort**, and published a checksummed companion dataset. Paper 3 documents three constitutional isomers with the same molecular formula, the same raw SMILES length (111 bytes), and the same zlib-compressed length (72 bytes), but exact molecular assembly indices of 10, 18, and 19: [Paper 1 v3](https://doi.org/10.26434/chemrxiv.15004164/v3) · [Paper 2 v1](https://doi.org/10.26434/chemrxiv.15012530/v1) · [Paper 3 v1](https://doi.org/10.26434/chemrxiv.15012760/v1) · [Zenodo v1.0.0](https://doi.org/10.5281/zenodo.24544551) · [GitHub](https://github.com/ariz32601-ksl/molecular-assembly-benchmarks).
