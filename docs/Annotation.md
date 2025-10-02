---
layout: default
title: Componentization
nav_order: 6
has_children: false
---

# Feature Detection Overview

**LC-HRMS feature detection** algorithms transform the complex raw mass spectrometry data (a map of mass-to-charge (*m/z*), retention time (RT), and intensity) into a list of quantifiable, discrete **features** (representing chemical compounds). The typical processing steps include:

1.  **Mass Detection/Peak Picking:** Identifying individual ions in each spectrum.
2.  **Chromatogram Construction:** Generating Extracted Ion Chromatograms (EICs).
3.  **Chromatographic Peak Detection:** Identifying peaks within the EICs.


## Data Types in Feature Detection

The data type is a critical parameter for selecting a feature detection algorithm:

* **Profile Data:** The raw output where a single ion is represented by a distribution of data points (the "peak shape"). Algorithms like **SAFD** and **OpenMS's IsotopeWavelet** are designed to use this information.
* **Centroided Data:** Simplified data where the ion's $m/z$ distribution is reduced to a single representative value and its intensity. This is the common input for algorithms like **XCMS's centWave** and **KPIC**.


# LC-HRMS Feature Detection Algorithms 🧪

This page provides a quick reference for popular open-source software packages and libraries commonly used for **Liquid Chromatography–High Resolution Mass Spectrometry (LC-HRMS) feature detection** in metabolomics and proteomics workflows.

| Algorithm/Software | Primary Language | Minimal Description | Version (Latest Stable) | Open-Access Paper | License | Data Type | 
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **1. XCMS (centWave)** | **R** | A widely-used R package for **metabolomics data processing**. **CentWave** is its most popular algorithm for peak detection, using a **cent**er-weighted **wave**let approach to find features. | `xcms 4.0` (Bioconductor) | **2008 (Original)**: *XCMS: Processing Mass Spectrometry Data...* [Anal. Chem.] **2008 (centWave)**: *Highly sensitive feature detection...* [Bioinfor.] | **Open-source** (Bioconductor/R) | Optimized for **Centroided** data. |
| **2. MZmine/ mzio** | **Java** | A comprehensive, modular, **cross-platform application** with a GUI. It offers multiple algorithms for mass detection, chromatogram building, and **chromatogram deconvolution/feature resolving**. | Actively updated (Check official website for latest release) | **2023 (MZmine 3)**: *Integrative analysis of multimodal mass spectrometry data in MZmine 3* [Nat. Biotechnol.] | **Open-source** | Works with both **Profile** and **Centroided** data. |
| **3. MS-DIAL** | **C# / .NET** | Software designed for multiple omics. It utilizes algorithms based on **differential calculus and peak spotting** across mass and retention time dimensions for feature detection and deconvolution. | Actively updated (Check official website for latest release) | **2015 (Original)**: *MS-DIAL: Data Independent MS/MS Deconvolution...* [Anal. Chem.] | **Open-source** | Handles both **Profile** and **Centroided** data. |
| **4. OpenMS (FeatureFinders)** | **C++ / Python** | A powerful open-source **framework and library**. It provides multiple FeatureFinder tools like **`FeatureFinderCentroided`** and **`FeatureFinderIsotopeWavelet`** for proteomics and metabolomics. | OpenMS 3.x (Check project release page) | **2024 (OpenMS 3)**: *OpenMS 3 enables reproducible analysis...* [Nat. Methods] | **3-Clause BSD** | Supports **Centroided** (`Centroided`) and **Profile** (`IsotopeWavelet`) data via different modules. |
| **5. SAFD** | **Julia** | **Self-Adjusting Feature Detection.** A highly generic algorithm that performs feature detection by fitting a **three-dimensional Gaussian** model to the raw signal, avoiding pre-centroiding or binning. | Actively updated (Check project repository) | **2019**: *A Self Adjusting Algorithm for the Non-targeted Feature Detection...* [Anal. Chem.] | MIT | Specifically designed for raw **Profile** data but can handle also centroided data. |
| **6. KPIC** | **R** | **Kinetic Peak Isolation and Comparison.** An R package algorithm focused on **robust isolation and deconvolution** of co-eluting peaks using patterns derived from the kinetic/shape characteristics of the chromatogram. | Not consistently versioned (Check CRAN/GitHub) | **2016**: *Kinetic peak isolation and comparison...* [Bioinformatics] | **Open-source** (Likely GPL/LGPL) | Primarily designed for **Centroided** data. |

---


# Additional Resources:

## Defintions: 

The **Allotrope Foundation Ontology (AFO)** and the **Allotrope Simple Model (ASM)** define a standardized, semantic vocabulary for analytical data, ensuring interoperability between software tools. The key input and output parameters used in feature detection are formally defined within this framework. It should be noted that these Property are by no means exhustive and we invite the developers to to contribute to this list.

| Property | Semantic Term (Allotrope Context) | Property Role | Units | Description | 
| :--- | :--- | :--- |
| ID | **Mass to Charge Ratio at the Start** | **Additional** output | Da | The m/z value at the strat of the detected and integrated peak. |
| mz | **Mass to Charge Ratio** | **Essential** output | Da | The measured value of the mass of an ion divided by its charge measured either at the top of mass peak or an aneraged value over the full feature. |
| rt | **Retention Time** | **Essential** output | s (seconds) | The time interval from the start of the chromatographic run to the point of maximum peak intensity of the detected feature. |
| Int | **Peak Intensity** | **Essential** output | Counts | The peak intensity in the time domain recorded either at the maximum of measured EIC or at the top of fitted peak. |
| Area | **Peak Area** | **Essential** output | NA | The integrated area under the chromatographic curve (EIC) or the fitted peak for the feature. |
| mz_s | **Mass to Charge Ratio at the Start** | **Additional** output | Da | The m/z value at the strat of the detected and integrated peak. |
| mz_e | **Mass to Charge Ratio at the End** | **Additional** output | Da | The m/z value at the end of the detected and integrated peak. |
| rt_s | **Retention time at the Start** | **Additional** output | s | The retention time value at the start of the detected and integrated peak. |
| rt_e | **Retention time at the End** | **Additional** output | s | The retention time value at the end of the detected and integrated peak. |
| PWMD | **Peak Width in Mass Domain** | **Additional** output | Da | The m/z window associated with the detected and integrated peak, providing information on the measurement uncertainty. |
| PWTD | **Peak Width in Time Domain** | **Additional** output | s | The retention window associated with the detected and integrated peak, providing information on the quality of separation. |
| Q | **Quality** | **Additional** output | NA | A measure of quality of the detected peak e.g. R$^{2}$ fit or QPeak parameter etc. |


## Example Files

Here you can find example outputs files in [JSON](https://github.com/EMCMS/PINTS/blob/main/assets/example_files/json_examp.json) and [CSV](https://github.com/EMCMS/PINTS/blob/main/assets/example_files/csv_examp.csv) formats. These files are meant to be used for the developers to generate their output files. 

![JSON File](https://github.com/EMCMS/PINTS/blob/main/assets/images/JSON.png?raw=true)

![CSV File](https://github.com/EMCMS/PINTS/blob/main/assets/images/CSV.png?raw=true)

