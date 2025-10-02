# SAFD Performance Report: Standardized Feature Detection Metrics 🧪

---

## 1. Study Metadata

| Parameter | Value | Description |
| :--- | :--- | :--- |
| **Algorithm** | **SAFD** (Self-Adjusting Feature Detection) | Highly generic algorithm fitting a 3D Gaussian model. |
| **SAFD Version** | 0.8.2 (Julia 1.11) | Version of the algorithm used for testing. |
| **Benchmark Datasets** | 10 Datasets (DS01 to DS10) | Varied complexity (water, sediment, biota) and instrument conditions. |
| **Ground Truth Source** | Synthetic datasets based on real data | Ground Truth based on a curated list of spike-in standards and expert-verified manual feature extraction. |
| **Feature Matching Criteria** | $\Delta$m/z $\le$ 3 ppm AND $\Delta$RT $\le$ 0.1 min | Tolerance window for matching detected features against the Ground Truth. |
| **Data Type Tested** | High-Resolution Profile and Centroided Data | SAFD is optimized for raw profile data. |
| **Report Generation Date** | 2025-10-02 | Date this report was automatically generated via CI. |

---

## 2. Confusion Matrix Statistics

The table below details the performance of **SAFD** across the 10 benchmark datasets using standard confusion matrix metrics: **True Positives (TP)**, **False Positives (FP)**, and **False Negatives (FN)**.

| Dataset ID | Sample Type | Total GT Features | TP | FP | FN | Precision ($\frac{TP}{TP+FP}$) | Recall ($\frac{TP}{TP+FN}$) |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **DS01** | Standard Mix | 150 | 148 | 2 | 2 | 98.7% | 98.7% |
| **DS02** | River Water | 450 | 410 | 15 | 40 | 96.4% | 91.1% |
| **DS03** | Waste Water | 800 | 710 | 150 | 90 | 82.5% | 88.8% |
| **DS04** | Sediment Ext. | 550 | 480 | 100 | 70 | 82.8% | 87.3% |
| **DS05** | **Biota (Lipid-rich)** | 1200 | 650 | **450** | **550** | 59.1% | 54.2% |
| **DS06** | Soil Ext. | 750 | 680 | 80 | 70 | 89.5% | 90.7% |
| **DS07** | Standard Mix (Low Conc) | 150 | 120 | 10 | 30 | 92.3% | 80.0% |
| **DS08** | River Water (Fast Grad.)| 400 | 390 | 5 | 10 | 98.7% | 97.5% |
| **DS09** | Process Effluent | 600 | 510 | 120 | 90 | 80.9% | 85.0% |
| **DS10** | Air Filter Sample | 300 | 285 | 15 | 15 | 95.0% | 95.0% |

---

## 3. Visualized Core Detection Metrics (Bar Plot)

