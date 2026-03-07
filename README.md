# Hidden Markov Model for Human Activity Recognition

## Overview
This project models human activities from smartphone inertial signals using a Hidden Markov Model (HMM).  
The activities are treated as hidden states, while extracted sensor features are the observations.

The full pipeline is implemented in a single notebook for submission readiness and traceability.

## Team
- Oreste Abizera
- Mutoni Uwingeneye Denyse

## Use Case
We target lightweight home rehabilitation monitoring, where clinicians need interpretable evidence of whether a patient is standing, walking, jumping (exercise bursts), or still. The HMM is useful because it captures temporal continuity between adjacent motion windows instead of classifying each window independently.

## Dataset Summary
Source: smartphone recordings collected by the team with accelerometer and gyroscope streams.

Activities:
- Standing
- Walking
- Jumping
- Still

Current audited dataset (from notebook quality checks):
- Total recordings: `51`
- Files per class:
   - Jumping: `12`
   - Standing: `13`
   - Still: `13`
   - Walking: `13`
- Total duration per class (seconds):
   - Jumping: `91.095`
   - Standing: `125.274`
   - Still: `101.415`
   - Walking: `122.640`
- Device context:
   - iPhone 15 (Jumping, Still)
   - SM-S918U1 (Standing, Walking)
- Sampling metadata indicates near 100 Hz logging; all processing is harmonized to **50 Hz**.

## Repository Structure
- `hmm_activity_recognition.ipynb`: End-to-end implementation and analysis.
- `sensor-data/`: Raw session folders and CSV sensor streams.
- `requirements.txt`: Python dependencies.
- `README.md`: Project documentation.

## Methodology
1. Data collection audit and validation
- Verify recordings, labels, durations, and device metadata.
- Confirm minimum activity duration targets are met.

2. Sensor merge and harmonization
- Align accelerometer and gyroscope by nearest timestamps.
- Resample merged streams to 50 Hz for a consistent time base.

3. Windowing and feature extraction
- Window size: 2 seconds (100 samples at 50 Hz).
- Overlap: 50%.
- Time-domain features: mean, std, variance, RMS, SMA, axis correlations.
- Frequency-domain features: dominant frequency and spectral energy (0.5-3.0 Hz).
- Stillness-aware features were added to improve low-motion separation:
   - magnitude range
   - jerk RMS
   - low-motion ratio

4. HMM training and decoding
- Gaussian HMM, diagonal covariance.
- Baum-Welch for parameter estimation.
- Viterbi for most likely state sequence decoding.
- Final model uses 6 latent states to better separate subtle behavior sub-patterns.

5. Evaluation protocol
- File-level unseen split (no window leakage across train/test files).
- Confusion matrix and per-class sensitivity/specificity.
- Transition and emission visualizations for interpretability.

## Final Unseen-Test Results
- Overall Accuracy: `0.9091` (90.91%)
- Macro F1-score: `0.9058`

Per-class sensitivity (recall):
- Standing: `1.0000`
- Walking: `0.9167`
- Jumping: `1.0000`
- Still: `0.7000`

Key interpretation:
- Jumping and Standing are highly separable.
- Remaining challenge is Still vs Standing under very low motion.
- Post-decoding low-motion calibration improved class balance over earlier runs.

## How to Run
1. Create and activate a virtual environment.
2. Install dependencies:

```bash
pip install -r requirements.txt
```

3. Open and run notebook top-to-bottom:
- `hmm_activity_recognition.ipynb`

## Reproducibility Notes
- Seed is fixed in the notebook (`RANDOM_SEED = 42`).
- Feature scaling is fit on train only to avoid leakage.
- Unseen evaluation is based on file-level split, not random window split.

## Collaboration and Git Workflow
The project uses branch-based collaboration with merge commits to keep contributions attributable and auditable in Git history.

## Submission Checklist Mapping
This repository contains:
- Well-labeled raw CSV dataset folder (`sensor-data/`)
- HMM implementation notebook (`hmm_activity_recognition.ipynb`)
- Documentation and results summary (`README.md`)

For final submission export:
- Re-run all notebook cells
- Export report PDF with figures/tables from the notebook
- Push final `main` branch state to GitHub
