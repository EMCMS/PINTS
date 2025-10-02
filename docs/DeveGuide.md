---
layout: default
title: Developer Guidelines
nav_order: 8
has_children: false
---

# Benchmark Datasets 

Here we will have a short description of the benchmark datasets with a Zenodo link for download.

# 🛠️ PINTS Developer Quick-Start Guide: Tool Registration

This guide outlines the four mandatory steps required to register a new feature detection algorithm with the **PINTS Hub (Pipelines for Non-Target Screening)**. Following these instructions ensures your tool adheres to our standardized output, passes automated tests, and can be successfully integrated into the central performance dashboard.

---

## Step 1: Standardize Your Tool Output

The first and most critical step is ensuring your tool's final feature list output adheres to the **PINTS Standard Feature Schema (JSON)**. This enables machine-readable performance comparisons.

### A. Core Requirements

Your tool **must** generate a single output file, ideally named `pints_output.json`, following the structure and semantics of PINTS Hub. For each step, please take a look at the specific page in the website and make sure that your output files at least include the listed `essential` properties for that step (e.g. [Feature Detection](https://emcms.github.io/PINTS/docs/Feature.html)). 



### B. Translation Script

If your tool's native output (e.g., `.tsv`, `.csv`, or a different JSON structure) doesn't match this schema, you **must** include a translation script (e.g., Python or R) in your registration repository to convert the native output into `pints_output.json`. 

---

## Step 2: Set up the Tool Registration Repository

### A. Repository Structure

Your repository **must** contain the following essential files to be parsed by the central PINTS CI:
![essential files](https://github.com/EMCMS/PINTS/blob/main/assets/images/Tool_addedfiles.png?raw=true)

Examples of such files to be adapted to your tool can be found [here](https://github.com/EMCMS/PINTS/tree/main/assets/example_files).

### B. Download the Benchmark Datasets

The PINTS benchmark dataset s standardized LC-HRMS set of raw data file with must be used for all testing.

1. Download the PINTS Benchmark Set from the [Zenodo folder of PINTS Hub](https://zenodo.org/).

2. Place these files in your local local_test/ directory.

### C. Configure Tool Metadata (pints-config.json)
Populate this file with accurate metadata about your tool. This information will populate the PINTS dashboard.

```json
{
  "tool_name": "MyFeatureDetectorName",
  "github_url": "https://github.com/YourOrg/YourTool",
  "version": "1.0.0",
  "language": "Python 3.13.7",
  "description": "A brief, one-sentence description of the algorithm.",
  "manuscript" : "paper where your tools is published (preprints are more than welcome)",
  "License": "the license type",
  "output_schema_version": "1.0"
}
``` 
## Step 3: Run Local Testing and Generate Reports

Use the provided PINTS Local Runner Script (available from the main PINTS Hub documentation) to execute your tool and generate the standardized performance report.

1. Modify tool_scripts/run_tool.sh: Ensure this script correctly calls your tool and produces pints_output.json from the benchmark data in local_test/.

2. Run the Local Runner: Execute the PINTS Local Runner (e.g., pints-local-runner.py) against your repository.

3. Check Output: The runner will generate reports associate with the benchmark dataset and will place those reports in the local_test/.

Success Condition: You must ensure the generated reports look correct and resoanable.

## Step 4: Submit Pull Request (PR) for Registration

This final step submits your tool to the central PINTS Hub for automated validation and final registration.

### A. Submit Files
Commit and Push the following generated files to your repository:

* The fully configured pints-config.json.

* The final, locally generated report file.

* The tool_scripts/ directory with the execution scripts.

### B. Create Pull Request

1. Navigate to the main PINTS Hub Repository.

2. Create a new Pull Request (PR) that proposes adding your entire `Tool-Registration-Repo/` (or a sub-folder pointing to it) to the designated algorithms/ directory.

3. CI Validation: Your PR will automatically trigger the full **PINTS CI Workflow** (using the .github/workflows/ci-pints-test.yml from the Hub). This CI compares your generated reports with the `ground truth` and will generate the **report.md** files that will be assoaciated with your algorithm.

4. Acceptance: Once the CI workflow passes your tool will be registered and displayed on the PINTS dashboard along with the generated report.

It should be noted that the acceptance of a tool is not dependent on a performance metrics. As long as your tools complies with the provided quidelines, it has a place in **PINTS Hub**. 
