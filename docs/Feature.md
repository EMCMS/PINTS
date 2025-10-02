---
layout: default
title: LC-HRMS Feature Detection Algorithms
nav_order: 3
has_children: false
---

# Feature Detection Overview

**LC-HRMS feature detection** algorithms transform the complex raw mass spectrometry data (a map of mass-to-charge ($m/z$), retention time (RT), and intensity) into a list of quantifiable, discrete **features** (representing chemical compounds). The typical processing steps include:

1.  **Mass Detection/Peak Picking:** Identifying individual ions in each spectrum.
2.  **Chromatogram Construction:** Generating Extracted Ion Chromatograms (EICs).
3.  **Chromatographic Peak Detection:** Identifying peaks within the EICs.
4.  **Feature Grouping:** Combining related chromatographic peaks (isotopes, adducts) across $m/z$ and RT dimensions into a single "feature."
5.  **Alignment:** Correcting for minor RT shifts between samples.

## Data Types in Feature Detection

The data type is a critical parameter for selecting a feature detection algorithm:

* **Profile Data:** The raw output where a single ion is represented by a distribution of data points (the "peak shape"). Algorithms like **SAFD** and **OpenMS's IsotopeWavelet** are designed to use this information.
* **Centroided Data:** Simplified data where the ion's $m/z$ distribution is reduced to a single representative value and its intensity. This is the common input for algorithms like **XCMS's centWave** and **KPIC**.


# LC-HRMS Feature Detection Algorithms 🧪

This page provides a quick reference for popular open-source software packages and libraries commonly used for **Liquid Chromatography–High Resolution Mass Spectrometry (LC-HRMS) feature detection** in metabolomics and proteomics workflows.

| Algorithm/Software | Primary Language | Minimal Description | Version (Latest Stable) | Open-Access Paper | License | Data Type | 
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **1. XCMS (centWave)** | **R** | A widely-used R package for **metabolomics data processing**. **CentWave** is its most popular algorithm for peak detection, using a **cent**er-weighted **wave**let approach to find features. | `xcms 4.0` (Bioconductor) | **2008 (Original)**: *XCMS: Processing Mass Spectrometry Data...* [Anal. Chem.] **2008 (centWave)**: *Highly sensitive feature detection...* [Anal. Chem.] | **Open-source** (Bioconductor/R) | Optimized for **Centroided** data. |
| **2. MZmine 3** | **Java** | A comprehensive, modular, **cross-platform application** with a GUI. It offers multiple algorithms for mass detection, chromatogram building, and **chromatogram deconvolution/feature resolving**. | 3.2.0 (August 2022) | **2023 (MZmine 3)**: *Integrative analysis of multimodal mass spectrometry data in MZmine 3* [Nat. Biotechnol.] | **Open-source** | Works with both **Profile** and **Centroided** data. |
| **3. MS-DIAL** | **C# / .NET** | Software designed for multiple omics. It utilizes algorithms based on **differential calculus and peak spotting** across mass and retention time dimensions for feature detection and deconvolution. | Actively updated (Check official website for latest release) | **2015 (Original)**: *MS-DIAL: Data Independent MS/MS Deconvolution...* [Anal. Chem.] | **Open-source** | Handles both **Profile** and **Centroided** data. |
| **4. OpenMS (FeatureFinders)** | **C++ / Python** | A powerful open-source **framework and library**. It provides multiple FeatureFinder tools like **`FeatureFinderCentroided`** and **`FeatureFinderIsotopeWavelet`** for proteomics and metabolomics. | OpenMS 3.x (Check project release page) | **2024 (OpenMS 3)**: *OpenMS 3 enables reproducible analysis...* [Nat. Methods] | **3-Clause BSD** | Supports **Centroided** (`Centroided`) and **Profile** (`IsotopeWavelet`) data via different modules. |
| **5. SAFD** | **MATLAB/Julia** | **Self-Adjusting Feature Detection.** A highly generic algorithm that performs feature detection by fitting a **three-dimensional Gaussian** model to the raw signal, avoiding pre-centroiding or binning. | Actively updated (Check project repository) | **2019**: *A Self Adjusting Algorithm for the Non-targeted Feature Detection...* [Anal. Chem.] | MIT | Specifically designed for raw **Profile** data but can handle also centroided data. |
| **6. KPIC** | **R** | **Kinetic Peak Isolation and Comparison.** An R package algorithm focused on **robust isolation and deconvolution** of co-eluting peaks using patterns derived from the kinetic/shape characteristics of the chromatogram. | Not consistently versioned (Check CRAN/GitHub) | **2016**: *Kinetic peak isolation and comparison...* [Bioinformatics] | **Open-source** (Likely GPL/LGPL) | Primarily designed for **Centroided** data. |

---