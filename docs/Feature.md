---
layout: default
title: LC-HRMS Feature Detection Algorithms
nav_order: 5
has_children: false
---

# LC-HRMS Feature Detection Algorithms 🧪

This page provides a quick reference for popular open-source software packages and libraries commonly used for **Liquid Chromatography–High Resolution Mass Spectrometry (LC-HRMS) feature detection** in metabolomics and proteomics workflows.

| Algorithm/Software | Git Repository & Developer | Primary Language | Minimal Description |
| :--- | :--- | :--- | :--- |
| **1. XCMS** | **[GitHub: `sneumann/xcms`](https://github.com/sneumann/xcms)**<br>Developer: Steffen Neumann, Colin Smith, etc. (Original) | **R** | A widely-used R package for **metabolomics data processing**. It includes algorithms for peak detection (like **centWave**), retention time alignment, and feature grouping. |
| **2. MZmine 3** | **[GitHub: `mzmine/mzmine3`](https://github.com/mzmine/mzmine3)**<br>Developer: Tomáš Pluskal (Lead Developer) & Community | **Java** | A comprehensive, modular, **cross-platform application** with a graphical user interface (GUI). It offers various algorithms for mass detection, chromatogram building, and feature resolving. |
| **3. MS-DIAL** | **[GitHub: `MS-DIAL/MS-DIAL`](https://github.com/MS-DIAL/MS-DIAL)**<br>Developer: Tsugawa, H. (Lead Developer) & Laboratory | **C# / .NET** | Software designed for multiple omics, including **LC-HRMS metabolomics and lipidomics**. It includes feature detection and **data-independent acquisition (DIA) MS/MS deconvolution** algorithms. |
| **4. OpenMS (and TOPP)** | **[GitHub: `OpenMS/OpenMS`](https://github.com/OpenMS/OpenMS)**<br>Developer: The OpenMS Initiative (Lead: Oliver Kohlbacher) | **C++ (with Python bindings: `pyOpenMS`)** | A powerful open-source **framework and library** containing a wealth of algorithms (many feature detection and peak-picking tools) used in proteomics and metabolomics. |
| **5. MAVEN** | **[GitHub: `MAVEN-Mass-Spec-Visualization/MAVEN`](https://github.com/MAVEN-Mass-Spec-Visualization/MAVEN)**<br>Developer: Phillip Seitzer, Eugene Melamud (Original) & Community | **Java** | Initially designed for **isotopic tracing metabolomics** data, MAVEN provides algorithms and a GUI for efficient visualization, feature detection, and peak area quantitation. |
| **6. PF$\Delta$Screen** | **[GitHub: (Associated via `pyOpenMS` to `OpenMS/OpenMS`)**<br>Developer: Jonathan Zweigle, Christian Zwiener, et al. | **Python** | A specialized **Python-based tool** for non-target HRMS screening, focusing on **PFAS (Per- and Polyfluoroalkyl Substances) feature prioritization** using a workflow that includes feature detection via pyOpenMS. |