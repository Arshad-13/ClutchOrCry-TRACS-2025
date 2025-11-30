# ClutchOrCry-TRACS-2025
Official code for the "Clutch or Cry" team's hybrid stacking ensemble solution for imbalanced astrophysical document classification at the TRACS @ WASP 2025 shared task.

[![Paper](https://img.shields.io/badge/paper-ACL_Anthology-blue.svg)](https://link-to-your-paper-once-available)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

This repository contains the official code and models for the **"Clutch or Cry"** team's 4th place entry in the [TRACS @ WASP 2025 shared task](https://www.kaggle.com/competitions/tracs-wasp-2025).

Our system is a hybrid ensemble for classifying astrophysical documents, specifically designed to handle the severe class imbalance and long document contexts present in the dataset.

For a detailed description of our methodology and findings, please refer to our paper:
**["Clutch or Cry" Team at TRACS @ WASP 2025: A Hybrid Stacking Ensemble for Astrophysical Document Classification](https://drive.google.com/file/d/1XF2nmSg34A0-X_xcRxb3luJMccrycz71/view?usp=drivesdk)**

## System Architecture

Our solution employs distinct architectures tailored to the specific requirements of each subtask.

### Task 1: Telescope Identification

* **Goal:** A multi-class classification problem to identify the primary telescope (`CHANDRA`, `HST`, `JWST`, or `None`).
* **Method:** A `RandomForestClassifier` trained on extensive feature-engineering. The most dominant feature was a rule-based extraction of the document ID suffix (bibcode), which provided a high-precision signal.

### Task 2: Document Role Classification

* **Goal:** A multi-label classification task to determine the telescope's role (`science`, `instrumentation`, `mention`, or `not_telescope`).
* **Challenge:** This task was characterized by severe class imbalance; for example, the `instrumentation` class had a positive-to-negative ratio of approximately 1:91.
* **Method:** A two-level stacking ensemble (see Figure 1 in the paper).
    * **Level-0 (Base Models):** We combined a fast, symbolic **Rule-Based Keyword Classifier** (using a curated dictionary of >1,000 terms) with a deep semantic model (a fine-tuned **`astroBERT`** transformer).
    * **Level-1 (Meta-Learner):** We trained four independent **XGBoost classifiers**—one for each label. This per-label design was critical, as it allowed us to apply targeted optimization strategies (like SMOTE, aggressive class weighting, and custom decision thresholds) only to the specific minority classes that needed them.

## Results

* **Final Rank:** 4th Place.
* **Score:** 0.82 combined Macro F1-score on the official leaderboard.
* **Key Finding:** Our targeted optimization strategy for Task 2 was highly effective, dramatically improving the F1-score for the extreme-minority `instrumentation` class from **0.510** (baseline) to **0.782** (final).

## Setup and Usage

1.  **Clone the repository:**
    ```bash
    git clone [https://github.com/your-username/ClutchOrCry-TRACS-2025.git](https://github.com/your-username/ClutchOrCry-TRACS-2025.git)
    cd ClutchOrCry-TRACS-2025
    ```

2.  **Create a virtual environment and install dependencies:**
    ```bash
    python -m venv venv
    source venv/bin/activate  # On Windows, use `venv\Scripts\activate`
    pip install -r requirements.txt
    ```

3.  **Download data and models:**
    * DATASET - https://www.kaggle.com/competitions/tracs-wasp-2025/data
    * ASTROBERT - https://huggingface.co/adsabs/astroBERT

## Citation

If you find our work useful in your research, please cite our paper:

```bibtex
@inproceedings{clutch-et-al-2025,
    title = {"{C}lutch or {C}ry" {T}eam at {TRACS} @ {WASP} 2025: {A} {H}ybrid {S}tacking {E}nsemble for {A}strophysical {D}ocument {C}lassification},
    author = "Khatib, Arshad  and
      Prasad, Aayush  and
      Trivedi, Rudra  and
      Malviya, Shrikant",
    booktitle = "Proceedings of the Third Workshop for Artificial Intelligence for Scientific Publications (WASP 2025)",
    month = "...",
    year = "2025",
    address = "...",
    publisher = "Association for Computational Linguistics",
    pages = "..."
}
