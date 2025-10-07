---
layout: default
title: Annotation
nav_order: 6
has_children: false
---

# Annotation

**Annotation** is the process of assigning a putative chemical identity to a **feature** or component based on individual or combined experimental evidence (e.g., retention time, mass defect, fragmentation pattern) from all steps of the Non-Targeted Screening (NTS) pipeline, ultimately leading to identification at an appropriate confidence level.

While early approaches relied heavily on chromatographic data and MS1 accurate mass matching, today, **fragmentation spectra (MS/MS)** are central to compound identification and confirmation.

Typical annotation strategies include:

1.  **Spectral Library Matching:** Comparing experimental MS/MS spectra to curated databases (e.g., using a modified cosine score).
2.  **In Silico Fragmentation:** Predicting spectra from candidate chemical structures for identification.
3.  **De Novo Annotation:** Strategies for identifying unknowns that lack database entries.
4.  **Molecular Networking (MN):** Grouping features by spectral similarity to infer structural relationships.

---

# NTS Annotation Algorithms & Workflows 🧪

This page provides a quick reference for open-source software packages and libraries registered in PINTS Hub for **Annotation** in Non-Targeted Screening (NTS) workflows.

| Algorithm | Primary Language | Minimal Description | Discipline | Version (Latest Stable) | Open-Access Paper | License | Data Type |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **1. FluoroMatch Workflow** | R | **Automated workflow** for PFAS annotation that uses blank filtering, KMD plots, retention time pattern analysis, and exact mass matching for identification. | Metabolomics/Exposomics | 5.6 | [FluoroMatch 2.0](https://link.springer.com/article/10.1007/s00216-021-03392-7) | Unclear | Raw vendor formats |
| **2. Universal Library Search Algorithm (ULSA)** | Julia | Performs spectral library search using a multiple parameter scoring. | Exposomics | 0.6 | [Combining...](https://pubs.acs.org/doi/10.1021/acs.est.8b00259) | MIT | components or prioritized components |
| **3. MetFrag** | Java-R | Identifies small molecules from tandem MS measurements **without spectral reference data** by matching Predicted Fragments with Mass Spectra. | Metabolomics | 2.6.8 | [MetFrag...](https://jcheminf.biomedcentral.com/articles/10.1186/s13321-016-0115-9) | GNU | components or prioritized components |
| **4. MS2LDA** | Python | Finds **substructures/molecular topics** within a dataset of spectra and can automatically annotate them using topic modeling. | Metabolomics | 2.0 | [Large-scale...](https://www.biorxiv.org/content/10.1101/2025.06.19.659491v2) | MIT | components or prioritized components | 


# Additional Resources:

## Definitions: 

The annotation step requires specific inputs, primarily the output from the prioritization step, which contains both feature/component information. The output must report the assigned chemical identity and key metrics for confidence.


| Property | Semantic Term (Allotrope Context) | Property Role | Description | 
| :--- | :--- | :--- | :--- | :--- |
| annotation | **a unique chemical identifier** (e.g. SMILES) | **Essential** output | The chemical identity. |
| annotation_score | **annotation score** | **Essential** output | A similarity score either via cosine similarity or other approaches. |
| annotation_type | **annotation algorithm** | **Essential** output | The used algorithm and the output type of the annotation (e.g. molecular formula vs library match). |
| annotation_certainty | **annotation certainty** | **Additional** output | Either a probability or confidence level for the annotation. |
| matched_frags | **number of matched fragments** | **Additional** output | The number of matched fragments in the annotated spectrum. |


---


## Example Files

Here you can find an example JSON file structure that aligns with the described inputs and outputs. This structure is intended to be the final output file from an annotation tool.

```json
{
  "file_description": "LC-HRMS Feature Detection & Annotation Output (from ms2query)",
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
  
  "annotation_info": {
    "software": ["SIRIUS", "ULSA"],
    "version": ["4.0", "0.6.8"],
    "parameters": {
      "database": ["PubChem", ["GNPS", "MassBank"]],
      "ionization_mode": "positive",
      "model": "",
      "mass_tolerance_mDa": 5,
      "threshold": 0.5
    }
  },
  
  "schema": {
    "$schema": "[http://json-schema.org/draft-07/schema#](http://json-schema.org/draft-07/schema#)",
    "$id": "[http://example.com/ms_feature_annotation_array.json](http://example.com/ms_feature_annotation_array.json)",
    "title": "LC-HRMS Feature Annotation Output (Allotrope Semantic Basis)",
    "description": "An array of detected and annotated LC-HRMS features.",
    "type": "array",
    "items": {
      "type": "object",
      "required": ["ID", "mz", "rt", "Int", "Area", "Annotation", "Category of annotation", "Minimum required fragments", "Annotation score"],
      "properties": {
        "ID": {
          "type": "integer",
          "description": "Unique identifier for the detected feature (Peak ID).",
          "minimum": 1
        },
        "mz": {
          "type": "number",
          "description": "Mass to Charge Ratio (Essential Output from Feature Detection). Unit: Da.",
          "minimum": 0
        },
        "rt": {
          "type": "number",
          "description": "Retention Time (Essential Output from Feature Detection). Unit: s.",
          "minimum": 0
        },
        "Int": {
          "type": "integer",
          "description": "Peak Intensity (Essential Output from Feature Detection). Unit: Counts.",
          "minimum": 0
        },
        "Area": {
          "type": "integer",
          "description": "Peak Area (Essential Output from Feature Detection). Unit: NA.",
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
        },
        "annotation": {
          "type": "string",
          "description": "The assigned chemical identity (SMILES or InChIKey).",
          "pattern": "^(SMILES|InChIKey):.*"
        },
        "annotation_type": {
          "type": "string",
          "description": "The strategy used for annotation (e.g., Library Matching, In Silico, De Novo)."
        },
        "matched_frags": {
          "type": "integer",
          "description": "The number of reliable fragments supporting the annotation.",
          "minimum": 0
        },
        "annotation_score": {
          "type": "number",
          "description": "The similarity/matching score (e.g., Cosine Score, MS2Query Score).",
          "minimum": 0
        },
        "annotation_certainty": {
          "type": "number",
          "description": "Confidence score for the annotation, if provided by the tool.",
          "minimum": 0,
          "maximum": 1
        }
      },
      "additionalProperties": true 
    }
  },
  
  "data": [
    {
      "ID": 1,
      "mz": 301.0705,
      "rt": 4.52,
      "Int": 85000,
      "Area": 1250000,
      "prio": 1,
      "Q_p": 0.95,
      "annotation": "InChIKey=HEDGSPUNRUDNRO-UHFFFAOYSA-N",
      "annotation_type": "Library Matching (ULSA)",
      "matched_frags": 4,
      "annotation_score": 0.98,
      "annotation_certainty": 0.85
    },
    {
      "ID": 2,
      "mz": 450.1234,
      "rt": 8.11,
      "Int": 150000,
      "Area": 2800000,
      "prio": 0,
      "Q_p": 0.30,
      "annotation": "SMILES=C1(C=CC(=C(C1)O)C)O",
      "annotation_type": "In Silico (SIRIUS)",
      "matched_frags": 3,
      "Annotation score": 0.92,
      "Annotation certainty": 0.70
    },
    {
      "ID": 3,
      "mz": 155.0501,
      "rt": 2.90,
      "Int": 25000,
      "Area": 300000,
      "prio": 1,
      "Q_p": 0.88,
      "annotation": "",
      "annotation_type": "De Novo (SIRIUS Formula)",
      "matched_frags": 0,
      "Annotation score": 0.55,
      "Annotation certainty": 0.50
    }
  ]
}

```