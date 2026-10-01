# TRF EEG Analysis: Reproducing Simon et al. (2024) Using Python/Eelbrain

This repository contains Jupyter notebooks reproducing the TRF (Temporal Response Function) analysis from:

> Simon, A., Bech, S., Loquet, G., & Østergaard, J. (2024). Cortical linear encoding and decoding of sounds: Similarities and differences between naturalistic speech and music listening. *European Journal of Neuroscience*, 59(8), 2059–2074. https://doi.org/10.1111/ejn.16265

The original analysis was implemented in MATLAB using the mTRF toolbox. This project reproduces it in Python using the [Eelbrain](https://eelbrain.readthedocs.io/) toolbox, and extends it by running per-participant analyses in addition to the condition-level approach used in the paper.

---

## Dataset

The preprocessed EEG data and audio envelopes are publicly available on Zenodo:

> **DOI:** https://doi.org/10.5281/zenodo.7500806

Download the following files from Zenodo and place them in a `Processed/` folder:
- `Data_Music_Cello.mat`
- `Data_Music_Danish.mat`
- `Data_Music_Finnish.mat`
- `Data_Speech_Danish.mat`
- `Data_Speech_Finnish.mat`

The electrode positions file (`electrodes_locations.ced`) is included in this repository.

---

## Notebooks

### `trf_condition_analysis.ipynb`
Condition-level TRF analysis — reproduces the main analysis from the paper.

- Loads all five conditions into Eelbrain Datasets
- Estimates TRFs using the boosting algorithm (k=4, k=10, k=20)
- Plots TRF shapes, topographic arrays, predictive power, and mean r per condition
- Compares results with the paper (Figure 4 and Figure 5)

### `trf_participant_analysis.ipynb`
Per-participant TRF analysis — extends the paper by running boosting on each participant individually.

- Runs boosting per participant per condition (whole trials and segmented trials)
- Segments each 60-second trial into 4 × 15-second segments to increase cross-validation data
- Builds a chance model (envelope shifted by 5 seconds) to verify predictions are not due to chance
- Statistical analyses: one-sample t-test, paired t-test (real vs chance), repeated measures ANOVA, speech vs music comparison

---

## Installation

**1. Create a conda environment with Python 3.12:**
```bash
conda create -n eelbrain python=3.12
conda activate eelbrain
```

**2. Install Eelbrain:**
```bash
conda install -c conda-forge eelbrain
```

**3. Install remaining dependencies:**
```bash
pip install -r requirements.txt
```

**4. Launch Jupyter:**
```bash
jupyter notebook
```

---

## File Structure

```
TRF-EEG-Analysis/
├── README.md
├── requirements.txt
├── electrodes_locations.ced        ← electrode positions (64 channels)
├── trf_condition_analysis.ipynb    ← condition-level analysis (k=4, 10, 20)
├── trf_participant_analysis.ipynb  ← per-participant analysis + statistics
├── Processed/                      ← place .mat files here (download from Zenodo)
│   ├── Data_Music_Cello.mat
│   ├── Data_Music_Danish.mat
│   ├── Data_Music_Finnish.mat
│   ├── Data_Speech_Danish.mat
│   └── Data_Speech_Finnish.mat
├── figures/                        ← condition-level output figures (auto-created)
└── participants_figures/           ← per-participant output figures (auto-created)
```

---

## Key Parameters

These match the original paper (Section 2.6):

| Parameter | Value | Description |
|---|---|---|
| τmin | −100 ms | TRF start time |
| τmax | 750 ms | TRF end time |
| basis | 0.050 | 50ms Hamming window smoothing |
| SFREQ | 128 Hz | EEG sampling rate |
| N_TIMES | 7680 | Time points per trial (60 seconds) |

> **Note:** The paper used ridge regression (MATLAB mTRF toolbox). This implementation uses Eelbrain's boosting algorithm (coordinate descent with cross-validation-based early stopping). Results are comparable (mean r difference < 0.002).

---

## Adapting for a New Dataset

To use these notebooks with a different dataset:

1. Update the file paths at the top of each notebook (`DATA_ROOT`, `figures_path`)
2. Update `SFREQ` and `N_TIMES` to match your sampling rate and trial length
3. Replace `electrodes_locations.ced` with your electrode positions file
4. Adjust the condition file names and keys in the `conditions` dictionary

---

## Dependencies

- [Eelbrain](https://eelbrain.readthedocs.io/) — EEG/TRF analysis toolbox
- [NumPy](https://numpy.org/)
- [Pandas](https://pandas.pydata.org/)
- [SciPy](https://scipy.org/)
- [Matplotlib](https://matplotlib.org/)

---

## References

- Simon, A., Bech, S., Loquet, G., & Østergaard, J. (2024). Cortical linear encoding and decoding of sounds. *European Journal of Neuroscience*, 59(8), 2059–2074.
- Brodbeck, C., Das, P., Hong, L. E., & Simon, J. Z. (2021). Eelbrain: A Python toolkit for time-continuous analysis with temporal response functions. *eLife*, 10, e85012.

---

## Author

**Chan Myae** | UCL Neuroscience | Supervisor: Dr Adele Simon | 2025
