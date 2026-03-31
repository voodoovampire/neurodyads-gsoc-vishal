# Methodology

This document describes the analysis pipeline implemented in this repository for the NeuroDyads EEG data.

## 1. Data and task

The dataset consists of EEG recordings from a dyad performing a story listening task:

- **Speaker**: tells the story.
- **Listener**: listens to the story.

EEG is recorded simultaneously from both participants, with event markers (DIN1) indicating stimulus boundaries and task structure. Raw data are stored as EDF files (`Speaker.edf`, `Listener.edf`) under `data/raw/`.

## 2. Loading and segmentation

The notebook uses Python and MNE to:

1. Load the raw EDF files for Speaker and Listener.
2. Extract event information from the DIN1 channel.
3. Define time windows around relevant story events.
4. Segment the continuous data into trials aligned across Speaker and Listener.

The segmentation logic is kept explicit so that window length and event selection can be changed later if mentors want to explore other definitions.

## 3. Preprocessing and artifact handling

For each participant, the following preprocessing steps are applied:

1. **Channel handling**  
   - Remove the VREF channel and any clearly broken or flat channels.
   - Optionally re-reference to a chosen reference scheme if needed by NeuroDyads.

2. **Filtering**  
   - Band-pass filtering appropriate for EEG (e.g., 1–40 Hz) to remove slow drifts and high-frequency noise.

3. **Independent Component Analysis (ICA)**  
   - Fit ICA to the continuous data.
   - Identify components corresponding to common artifacts (e.g., eye blinks, muscle activity) using their topographies and time courses.
   - Remove selected components and reconstruct the cleaned signal.

Quality-control plots are generated to compare **power spectra before and after ICA** for selected channels and participants. These figures are saved in `results/figures/`.

## 4. Joint dyad representation

To perform brain-to-brain decoding, the Speaker and Listener signals must be represented jointly:

1. For each segmented trial, the Speaker and Listener data are time-aligned using the event markers.
2. The channel data from both participants are concatenated into a single 128-channel matrix (assuming 64 channels per participant).
3. These matrices are used to construct the input to the representation learning model.

This simple concatenation can later be replaced or complemented by more sophisticated features (e.g., inter-brain coupling measures), but it provides a clear baseline for CEBRA.

## 5. CEBRA embedding

CEBRA is used as a representation learning method for the joint dyad data. The notebook:

1. Formats the joint EEG segments and labels into the input expected by CEBRA.
2. Trains a **3D embedding** where each point represents a time sample or window in the story.
3. Saves the learned embeddings and metadata in `results/metrics/`.
4. Visualizes the embedding as 3D scatter plots colored by task-relevant labels (e.g., positive vs negative conditions, story segments).

These plots are stored in `results/figures/` and are used in the written interpretation.

## 6. Control analysis

To check whether the observed structure in the embedding depends on the true labels, a shuffled-label control is implemented:

1. The labels are randomly permuted.
2. The CEBRA model is retrained using the shuffled labels.
3. The resulting embedding is visualized and compared to the true-label embedding.
4. Simple quantitative evaluation (e.g., KNN classification on the embedding) is used to compare true vs shuffled performance.

If embeddings trained on real labels show stronger structure and better metrics than those trained on shuffled labels, this supports the claim that the representation is capturing meaningful information rather than random noise.

## 7. Interpretation and limitations

The notebook includes written sections discussing:

- The geometry of the learned embeddings and how labels cluster.
- How the shuffled-label control affects embedding structure and performance.
- Limitations of the current implementation (single dyad, limited set of parameters).
- Planned extensions (more participants, more thorough controls, better modularity) if selected for GSoC.

This documentation is meant to make the reasoning and design decisions transparent for mentors and future contributors.