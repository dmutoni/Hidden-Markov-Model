# Hidden Markov Model Activity Recognition

Human Activity Recognition (HAR) project using smartphone inertial sensors and a Hidden Markov Model (HMM).

## Team
- Oreste Abizera
- Mutoni Uwingeneye Denyse

## Project Goal
Infer hidden activity states (`Standing`, `Walking`, `Jumping`, `Still`) from noisy accelerometer and gyroscope recordings.

## Repository Structure
- `hmm_activity_recognition.ipynb`: Main end-to-end notebook (data audit, preprocessing, feature extraction, HMM training, unseen evaluation, plots).
- `sensor-data/`: Raw recordings grouped by activity/session.
- `requirements.txt`: Python dependencies.

## Method Summary
1. Data quality audit on recordings, device metadata, and duration checks.
2. Accelerometer and gyroscope merge by timestamp, then resampling to 50 Hz.
3. Sliding-window feature extraction (time + frequency domain).
4. Gaussian HMM training with Baum-Welch and sequence decoding with Viterbi.
5. Unseen file-level evaluation with confusion matrix and per-class metrics.

## Current Final Results (Unseen Data)
- Overall Accuracy: `0.9091`
- Macro F1-score: `0.9058`

Class-wise sensitivity:
- Standing: `1.0000`
- Walking: `0.9167`
- Jumping: `1.0000`
- Still: `0.7000`

## Collaboration Workflow
Work was split using feature branches and merge commits to preserve traceable contributions by both teammates.

## Run Instructions
1. Create and activate a virtual environment.
2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
3. Open and run:
   - `hmm_activity_recognition.ipynb`
