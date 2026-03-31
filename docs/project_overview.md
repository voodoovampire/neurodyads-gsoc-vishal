# NeuroDyads GSoC 2026 – Project Overview

This repository contains my work for the Machine Learning for Science (ML4SCI) **NeuroDyads** project as part of my Google Summer of Code 2026 application.

NeuroDyads studies **brain-to-brain decoding** from EEG hyperscanning data recorded from interacting dyads (Speaker and Listener). The goal is to learn low-dimensional neural embeddings that capture meaningful aspects of story listening and social interaction.

This project builds an end-to-end analysis pipeline for one NeuroDyads EEG dyad, with the following components:

- Preprocessing of raw EDF files for Speaker and Listener
- DIN1-based event segmentation and trial selection
- Artifact reduction using ICA, with power spectrum quality checks
- Construction of a joint 128-channel dyad representation
- Training of a 3D **CEBRA** embedding using task labels
- Visualization of embeddings and temporal trajectories
- Shuffled-label control analysis and simple quantitative evaluation

The pipeline is currently implemented in a single main notebook (`notebooks/NeuroDyads_MVP.ipynb`) plus supporting results stored under `results/`. If selected for GSoC, this work will be extended into a more modular and reusable codebase and integrated more closely with the upstream ML4SCI NeuroDyads project.