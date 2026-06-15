# Zebra Finch Vocalization Analysis

Repository detailing Lynn Chien's research and analytical pipelines for classifying DAY 1 & DAY 2 zebra finch calls at the Theunissen Lab (UC Berkeley), Fall 2025 - Spring 2026.

This project focuses on tracking day-over-day structural and proportional shifts in subject vocalizations. By processing raw spectrograms and Modulation Power Spectra (MPS), the pipeline establishes a behavioral baseline using unsupervised clustering, and projects temporal data into that latent space to measure behavioral drift.

## Presentations
Detailed overviews of the methodology, progress, and findings throughout the academic year:
* **[Fall 2025 Presentation](https://docs.google.com/presentation/d/14s5xW7QCCVfc-qabbiSgQt082fPBMSqixBlAzVfwyuI/edit?usp=sharing)**
* **[Spring 2026 Presentation](https://docs.google.com/presentation/d/1rD0VuWIoYquHEYsU9aKseATSwKqPg13rO-QcozErnj4/edit?usp=sharing)**

---

## Working Files

The development and execution of the classification pipeline are contained within the following notebooks:

### 1. `TableZeeBeeDAY2.ipynb`
* Handles the initial data ingestion, preprocessing, and exploratory formatting of the DAY 2 experiment datasets. 

### 2. `DAY2AnalysisFINAL.ipynb`
*The primary analytical pipeline.* This notebook contains the loop that evaluates vocalizations per bird. It is designed to be robust against ambient noise and boundary-case calls using a **Tiered Acceptance Algorithm**. 
* **Dimensionality Reduction:** Uses Z-score normalized spectrograms to map calls into a shared latent space.
* **Unsupervised Baseline:** Applies `HDBSCAN` density-based clustering to discover natural vocalization archetypes in the Day 1 baseline.
* **Supervised Mapping:** Utilizes a distance-weighted `KNN` classifier with adaptive strict/relaxed confidence thresholds to map Day 2 calls to the baseline.
* **Structural Drift Analysis:** Models syllable clusters as multivariate Gaussian distributions (GMM) and uses Mahalanobis distance, Trace Ratios, and Linear Discriminant Analysis (LDA) to quantify internal acoustic shifts.
* **Statistical Validation:** Computes Fisher's Exact Tests (with Bonferroni correction) to evaluate proportional shifts in syllable usage.

---

## Notes

When adapting the `DAY2AnalysisFINAL.ipynb` pipeline for a new dataset, pay special attention to the `BIRD_PARAMS` dictionary in the Setup block. The distance fence multipliers (`pctile`, `mult`, `relaxed_frac`) must be tuned iteratively for specific subjects using the diagnostic outputs at the end of the notebook. Remember to change the file paths as well.