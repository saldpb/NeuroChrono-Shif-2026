# NeuroChrono-Shif-trial
# NeuroChrono: Sub-Perceptual Threat Gating BCI Pipeline

[![Open In Colab](https://google.com)](https://google.com)
![Python](https://shields.io)
![MNE-Python](https://shields.io)
![PyRiemann](https://shields.io)

An end-to-end **Brain-Computer Interface (BCI)** signal processing, Riemannian geometry decoding, and real-time **Lab Streaming Layer (LSL)** inference pipeline designed for low-latency (`< 250 ms`) neural state gating on a **6-channel posterior wearable montage** (`P3, Pz, P4, O1, Oz, O2`).

---

## Architecture & 4-Week Progression

| Stage | Module | Key Operations & Methods |
| :--- | :--- | :--- |
| **Week 1** | **Hardware-Emulated Dataset Audit** | Maps posterior channels (`P3, Pz, P4, O1, Oz, O2`), clears bad-channel flags, audits `STI 014` stimulus triggers, and computes unfiltered Power Spectral Density (PSD) up to 100 Hz. |
| **Week 2** | **Signal Conditioning & ICA** | Applies `60 Hz` FIR notch and `0.1–30.0 Hz` bandpass filters, isolates ocular artifacts via FastICA (`EOG 061`), resamples to `250 Hz`, and extracts baseline-corrected ERP epochs (`-100` to `700 ms`). |
| **Week 3** | **Riemannian Manifold Decoding** | Evaluates leak-free 5-fold Stratified CV comparing **ERP-MDM** and **Tangent Space + Logistic Regression** using Ledoit-Wolf shrinkage (`lwf`) super-trial covariances (`AUC > 0.88` target). |
| **Week 4** | **Real-Time LSL Geodesic Gating** | Streams 250 Hz EEG chunks over LSL (`pylsl`), computes live Riemannian geodesic distances (`distance_riemann`) against exported class centroids, and benchmarks end-to-end latency (`< 250 ms` ceiling). |

---

## Key Visual Results

### Offline Riemannian Classification (5-Fold Leak-Free CV)
![Milestone 3 ROC-AUC Curves](results/milestone3_roc_auc_curves.png)

### Closed-Loop LSL Execution Latency (< 250 ms Hard Ceiling)
![Milestone 3 Latency Distribution](results/milestone3_latency_distribution.png)

---

## Repository Structure

```text
├── README.md
├── requirements.txt
├── .gitignore
├── notebooks/
│   └── NeuroChrono_BCI_Pipeline.ipynb   # Full Colab notebook with inline outputs
├── src/
│   └── neurochrono_pipeline.py          # Unified standalone Python pipeline
├── models/
│   ├── mdm_centroids.npy                # Exported Riemannian SPD class centroids
│   ├── mdm_classes.npy                  # Class label mapping
│   └── erp_templates.npy                # Grand-average evoked prototypes
└── results/
    ├── dataset_markers_audit.csv
    ├── week1_psd_and_events.png
    ├── week2_erp_waveforms.png
    ├── milestone3_roc_auc_curves.png
    └── milestone3_latency_distribution.png
```

---

## Quickstart (Local Execution)

```bash
# 1. Clone repository & install dependencies
git clone https://github.com
cd YOUR_REPO
pip install -r requirements.txt

# 2. Run the complete 4-stage pipeline
python src/neurochrono_pipeline.py
```
