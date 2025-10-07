---
layout: default
title: Prioritization
nav_order: 5
has_children: false
---

# Feature Prioritization

After feature extraction and componentization, **prioritization** represents the strategic approach for filtering and ranking compound features or components of interest, guiding subsequent annotation and identification efforts. Prioritization is applied to Non-Targeted Screening (NTS) pipelines and involves various methods and tools to rank features based on their chemical and structural signatures, statistical significance from the study design, or effect-directed properties such as predicted toxicity.

Prioritization can occur at two main points in the NTS pipeline:

1.  **Online (During Data Acquisition):** Often implemented using inclusion/exclusion lists and Data-Dependent Acquisition (DDA). These approaches rely heavily on user interest and instrument-specific control, but they can reduce the MS2 chemical coverage and are less represented in comprehensive NTS pipelines where Data-Independent Acquisition (DIA) is common.
2.  **Offline (Post Hoc Data Processing):** These strategies involve post hoc analysis of MS1 and MS2 data and are distinguished as **feature-based** or **fragmentation-based**. They leverage both the multiplicity of components in acquired data and the study design structure, supporting a wider range of approaches and better adaptation to open-source strategies.

## Prioritization Methods and Tools 🛠️

Offline strategies—which leverage MS1 (feature) or MS2 (fragmentation) data—offer high flexibility and are typically preferred in open-source NTS pipelines.

| Category | Approach Name | Description | Leveraged Data/Properties |
|:---|:---|:---|:---|
| **Online (DDA-based)** | **Intensity filters/TopN** | Real-time MS2 triggering during a duty cycle, prioritizing the most intense components. | Real-time intensity, DDA trigger |
| **Online (List-based)** | **Inclusion/Exclusion lists** | Use pre-determined $m/z$ or post-hoc generated lists from recorded MS1 data to guide DDA. | Pre-determined $m/z$ lists |
| **Online (Structure-based)** | **Isotopic abundance & ratio** | Real-time determination of isotopic patterns to prioritize chemicals containing specific elements (e.g., Cl, Br). | Isotope pattern, Real-time $m/z$ |
| **Offline (Feature-based)** | **Rule-based filtering** | Simple filters based on measured properties like intensity thresholding (noisy feature removal) or mass defect filters. | Intensity, Mass Defect, $m/z$ |
| **Offline (Feature-based)** | **Differential analysis** | Pattern recognition using unsupervised/supervised multivariate analysis or longitudinal trend analysis based on the study design. | Statistical significance, Study design structure |
| **Offline (Fragmentation-based)** | **Prioritization lists** | Filtering based on structure-based lists of chemicals that share similar functional groups, moieties, or specific elements. | Chemical structure, Elemental composition |
| **Offline (Fragmentation-based)** | **Structural/Molecular Motifs** | Prioritizing features that exhibit characteristic mass differences, often indicative of common metabolic or environmental modifications. | Mass differences, Fragmentation patterns |
| **Offline (Fragmentation-based)** | **Quantitative Structure–Activity Relationship (QSAR)** | Ranking putative identifications based on predicted toxicity, hazard estimates, or other structure-derived properties. | Predicted hazard, Molecular structure |

---

## Current Prioritization Tools and Software 💻

This section provides a quick reference for common software and libraries used for various prioritization approaches.

| Algorithm/Software | Category | Description | Input Data Type | Output Score | Open-Access Paper | License |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **MS2Tox** | QSAR/Toxicity Prediction | Toxicity prediction based on predicted SIRIUS fingerprint. | SIRIUS fingerprints | LC/EC 50 | [MS2Tox a machine...](https://pubs.acs.org/doi/10.1021/acs.est.2c02536#Abstract) | [Open-source](https://github.com/kruvelab/MS2Tox) |
| **ToxMapPrioritization** | Feature-based Ranking | Predicting toxicity category based on MS1 and MS2 signals. | MS1, MS2, and predicted RI | Toxicity Category | [Prioritization of...](https://pubs.acs.org/doi/full/10.1021/acs.est.4c13026) | [Open-source/MIT](https://bitbucket.org/viktoriiaturkina/toxmap_prioritization.jl/src/main/) |


---

# Additional Resources:

## Definitions: 

Outputs must include a string for the prioritization category and/or boolean value for the prioritization result, ensuring excluded features are preserved. This list of properties is harmonized with the former feature detection steps for an open NTS pipeline.

| Property | Semantic Term (Allotrope Context) | Property Role | Units | Description | 
| :--- | :--- | :--- | :--- | :--- |
| prior | **Prioritization category/prediction** | **Essential** output | NA | Either a value predicted based on a model (e.g. LC50), rank of the components, or category. |
| Qp | **Prioritization quality** | **Additional** output | NA | An uncertainty or prediction quality parameter. |

## Example Files

```json 

{
  "file_description": "LC-HRMS Feature Detection Output from SAFD.",
  "schema_version": "Draft 07",
  
  "processing_info": {
    "software": "SAFD (Self adjusting feature detection algorithm)",
    "version": "0.8.3",
    "parameters": {
      "maximum_iteration": 20000,
      "resolution": 20000,
      "s2n": 2,
      "R_thresh": 0.75,
      "Min_intensity": 1000
    }
  },
  "componentization_info": {
    "software": "CAMERA",
    "version": "3",
    "parameters": {
      "mz_tol_ppm": 5,
      "rt_tol": 0.1
    }
  },
  
  "prioritization_info": {
    "software": "ToxMap_Prioritization",
    "version": "1.0",
    "parameters": {
      "hazard_endpoint": "Toxicity category",
      "prediction_model": "Random Forest",
      "q_p_threshold": 0.70,
      "min_number_of_CNL_to_prioritize": 3
    }
  },
  
  "schema": {
    "$schema": "http://json-schema.org/draft-07/schema#",
    "$id": "http://example.com/ms_feature_array.json",
    "title": "LC-HRMS Feature Detection Output (Allotrope Semantic Basis)",
    "description": "An array of detected LC-HRMS features, structured using parameters aligned with the Allotrope Simple Model (ASM) context.",
    "type": "array",
    "items": {
      "type": "object",
      "required": ["ID", "mz", "rt", "Int", "Area"],
      "properties": {
        "ID": {
          "type": "integer",
          "description": "Unique identifier for the detected feature (Peak ID).",
          "minimum": 1
        },
        "mz": {
          "type": "number",
          "description": "Mass to Charge Ratio (Essential Output). Unit: Da.",
          "minimum": 0
        },
        "rt": {
          "type": "number",
          "description": "Retention Time (Essential Output). Unit: s.",
          "minimum": 0
        },
        "Int": {
          "type": "integer",
          "description": "Peak Intensity (Essential Output). Unit: Counts.",
          "minimum": 0
        },
        "Area": {
          "type": "integer",
          "description": "Peak Area (Essential Output). Unit: NA.",
          "minimum": 0
        },
        "mz_s": {
          "type": "number",
          "description": "Mass to Charge Ratio at the Start (Additional Output). Unit: Da.",
          "minimum": 0
        },
        "mz_e": {
          "type": "number",
          "description": "Mass to Charge Ratio at the End (Additional Output). Unit: Da.",
          "minimum": 0
        },
        "rt_s": {
          "type": "number",
          "description": "Retention time at the Start (Additional Output). Unit: s.",
          "minimum": 0
        },
        "rt_e": {
          "type": "number",
          "description": "Retention time at the End (Additional Output). Unit: s.",
          "minimum": 0
        },
        "PWMD": {
          "type": "number",
          "description": "Peak Width in Mass Domain (Additional Output). Unit: Da.",
          "minimum": 0
        },
        "PWTD": {
          "type": "number",
          "description": "Peak Width in Time Domain (Additional Output). Unit: s.",
          "minimum": 0
        },
        "Q": {
          "type": "number",
          "description": "Quality Measure (Additional Output). Specifically, the R^2 fit of the peak model.",
          "minimum": 0,
          "maximum": 1
        },
        "Rs": {
          "type": "number",
          "description": "Measured Resolution of the chromatographic peak (Additional Output). Unit: NA.",
          "minimum": 0
        },
        "prio": {
          "type": "integer",
          "description": "Prioritization status (0: Not Prioritized, 1: Prioritized).",
          "minimum": 0,
          "maximum": 1
        },
        "Q_p": {
          "type": "number",
          "description": "Quality of Prioritization (Score reflecting certainty/confidence of prioritization).",
          "minimum": 0,
          "maximum": 1
        }
      },
      "additionalProperties": false
    }
  },
  
  "data": [
    {
      "ID": 1,
      "mz": 301.0705,
      "rt": 4.52,
      "Int": 85000,
      "Area": 1250000,
      "mz_s": 301.0699,
      "mz_e": 301.0711,
      "rt_s": 4.45,
      "rt_e": 4.59,
      "PWMD": 0.0012,
      "PWTD": 0.14,
      "Q": 0.98,
      "Rs": 23000,
      "prio": 1,
      "Q_p": 0.95
    },
    {
      "ID": 2,
      "mz": 450.1234,
      "rt": 8.11,
      "Int": 150000,
      "Area": 2800000,
      "mz_s": 450.1228,
      "mz_e": 450.1240,
      "rt_s": 8.02,
      "rt_e": 8.21,
      "PWMD": 0.0012,
      "PWTD": 0.19,
      "Q": 0.95,
      "Rs": 34800,
      "prio": 0,
      "Q_p": 0.21
    },
    {
      "ID": 3,
      "mz": 155.0501,
      "rt": 2.90,
      "Int": 25000,
      "Area": 300000,
      "mz_s": 155.0496,
      "mz_e": 155.0506,
      "rt_s": 2.85,
      "rt_e": 2.95,
      "PWMD": 0.0010,
      "PWTD": 0.10,
      "Q": 0.89,
      "Rs": 29000,
      "prio": 1,
      "Q_p": 0.88
    }
  ]
}


``` 

