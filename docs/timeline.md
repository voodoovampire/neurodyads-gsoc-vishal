# Planned Timeline (350-hour GSoC Project)

If selected for GSoC 2026 with ML4SCI, the work in this repository will be extended over a 12-week, 350-hour full-time project.

## Community bonding period

- Reproduce the existing `NeuroDyads_MVP.ipynb` pipeline from a clean environment.
- Discuss expectations and coding style with mentors.
- Agree on the final repository layout and priority extensions.

## Weeks 1–3: Preprocessing refactor and QC

- Move data loading, event extraction, filtering, and ICA into reusable Python modules.
- Add configuration options for key parameters (filters, window lengths, channel lists).
- Extend PSD and QC plots to multiple dyads (where data are available).
- Document all preprocessing decisions clearly in `docs/methodology.md`.

**Milestone:** reusable preprocessing pipeline tested on several examples, with systematic QC figures.

## Weeks 4–6: CEBRA pipeline and experiment suite

- Implement a configurable CEBRA training pipeline (scripts/config files).
- Run a controlled set of experiments varying:
  - Embedding dimension (e.g., 2D, 3D, 10D)
  - Key hyperparameters (learning rate, batch size, label schemes)
- Store embeddings, metrics, and configuration metadata in `results/metrics/`.
- Standardize plotting functions for embeddings and trajectories.

**Milestone:** small but meaningful suite of CEBRA experiments with logged metrics and plots.

## Weeks 7–9: Controls, robustness, and baselines

- Extend shuffled-label controls across multiple random seeds.
- Compare true vs shuffled embeddings quantitatively using KNN and simple cluster metrics.
- Explore at least one robustness dimension (e.g., changing time windows or training on a subset of participants and testing on others).
- If time permits, implement a basic baseline such as PCA or simple autoencoder for comparison.

**Milestone:** clear, repeatable control and robustness analyses with written summaries.

## Weeks 10–12: Documentation, integration, and final report

- Clean all notebooks and scripts for readability and reproducibility.
- Improve `README.md` and `docs/` with usage examples and conceptual diagrams as needed
