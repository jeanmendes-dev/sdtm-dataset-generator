<div align="center">

# SDTM Dataset Generator

### A practical and extensible R framework for transforming raw clinical trial data into CDISC SDTM datasets

[![R](https://img.shields.io/badge/R-%3E%3D%204.3-276DC3?style=for-the-badge&logo=r&logoColor=white)](https://www.r-project.org/)
[![CDISC](https://img.shields.io/badge/CDISC-SDTM-005A9C?style=for-the-badge)](https://www.cdisc.org/standards/foundational/sdtm)
[![Domains](https://img.shields.io/badge/Domains-DM%20%7C%20LB%20%7C%20VS%20%7C%20AE-success?style=for-the-badge)](#supported-sdtm-domains)
[![License](https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge)](LICENSE)

**Raw clinical data → standardized transformations → validation → SDTM-ready outputs**

</div>

---

## Table of Contents

- [Project Overview](#project-overview)
- [Why This Project Exists](#why-this-project-exists)
- [Supported SDTM Domains](#supported-sdtm-domains)
- [Key Features](#key-features)
- [Architecture](#architecture)
- [Repository Structure](#repository-structure)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Quick Start](#quick-start)
- [Running Individual Domains](#running-individual-domains)
- [Input and Output Examples](#input-and-output-examples)
- [Domain Details](#domain-details)
- [Reusable Utilities](#reusable-utilities)
- [Validation Strategy](#validation-strategy)
- [Controlled Terminology](#controlled-terminology)
- [Date and Time Handling](#date-and-time-handling)
- [Testing](#testing)
- [Adding a New Domain](#adding-a-new-domain)
- [Production Enhancements](#production-enhancements)
- [Roadmap](#roadmap)
- [Limitations](#limitations)
- [References](#references)
- [Contributing](#contributing)
- [License](#license)

---

# Project Overview

`SDTM Dataset Generator` is an educational and executable R repository that demonstrates how raw clinical trial data can be mapped, transformed, validated, and exported into datasets based on the **Clinical Data Interchange Standards Consortium Study Data Tabulation Model**.

The current implementation includes four foundational domains:

- **DM — Demographics**
- **LB — Laboratory Test Results**
- **VS — Vital Signs**
- **AE — Adverse Events**

Each domain includes:

- a dedicated R builder;
- simulated raw input data;
- mapping and derivation logic;
- basic CDISC-oriented validation;
- domain-specific documentation;
- CSV output and optional SAS XPT output.

The repository is designed to support future domains without duplicating common logic.

> **Important:** This project is an educational implementation framework. It does not replace the official SDTM, SDTMIG, Controlled Terminology, conformance rules, Define-XML, annotated CRF, Study Data Reviewer's Guide, or qualified regulatory validation tools.

---

# Why This Project Exists

Clinical data collected from EDC systems, laboratories, safety databases, and external vendors rarely arrives in a submission-ready format.

A raw-to-SDTM process commonly needs to address:

- inconsistent source variable names;
- different date and time formats;
- sponsor-specific coding;
- repeated observations;
- controlled terminology;
- subject identifier reconciliation;
- partial dates;
- original versus standardized results;
- domain-level sequencing;
- traceability;
- validation and quality control.

This repository converts those requirements into transparent and reusable R code.

Main goals:

1. demonstrate reproducible raw-to-SDTM transformations;
2. document mapping decisions clearly;
3. provide reusable programming patterns;
4. offer synthetic data for hands-on practice;
5. establish a maintainable repository structure;
6. support learning and professional portfolio development.

---

# Supported SDTM Domains

| Domain | Label | Class | Record Level | Builder |
|---|---|---|---|---|
| DM | Demographics | Special Purpose | One record per subject | `sdtm-dm/sdtm-dm-builder.R` |
| LB | Laboratory Test Results | Findings | One record per test result | `sdtm-lb/sdtm-lb-builder.R` |
| VS | Vital Signs | Findings | One record per measurement | `sdtm-vs/sdtm-vs-builder.R` |
| AE | Adverse Events | Events | One record per event | `sdtm-ae/sdtm-ae-builder.R` |

Recommended next domains:

- DS — Disposition
- SV — Subject Visits
- EX — Exposure
- MH — Medical History
- CM — Concomitant Medications
- SE — Subject Elements
- SUPPQUAL — Supplemental Qualifiers

---

# Key Features

- Modular domain builders
- Reusable SDTM utility functions
- Simulated raw clinical datasets
- ISO 8601 date and datetime handling
- Automatic `USUBJID` generation
- Deterministic `--SEQ` generation
- Required-variable checks
- Duplicate-key detection
- Cross-domain subject reconciliation
- Controlled-value examples
- Logical date checks
- CSV output
- Optional SAS XPT output
- Basic `testthat` test structure
- Extensible architecture

---

# Architecture

```text
Raw Clinical Data
        │
        ▼
Input Validation
        │
        ▼
Cleaning and Type Normalization
        │
        ▼
Raw-to-SDTM Mapping
        │
        ▼
SDTM Derivations
        │
        ▼
Controlled Terminology
        │
        ▼
Identifier and Sequence Generation
        │
        ▼
Structural and Logical Validation
        │
        ▼
Variable Ordering
        │
        ▼
CSV / XPT Output
```

Execution dependency:

```text
DM
│
├── LB
├── VS
└── AE
```

DM is generated first because all observation domains validate their subjects against DM.

---

# Repository Structure

```text
sdtm-dataset-generator/
├── R/
│   └── utils-sdtm.R
├── data/
│   ├── raw/
│   │   ├── raw_dm.csv
│   │   ├── raw_lb.csv
│   │   ├── raw_vs.csv
│   │   └── raw_ae.csv
│   └── metadata/
│       └── controlled_terminology_demo.csv
├── output/
├── sdtm-dm/
│   ├── README.md
│   └── sdtm-dm-builder.R
├── sdtm-lb/
│   ├── README.md
│   └── sdtm-lb-builder.R
├── sdtm-vs/
│   ├── README.md
│   └── sdtm-vs-builder.R
├── sdtm-ae/
│   ├── README.md
│   └── sdtm-ae-builder.R
├── docs/
│   └── EXTENDING_THE_REPOSITORY.md
├── tests/
│   ├── testthat.R
│   └── testthat/
│       └── test-builders.R
├── STEP_BY_STEP_GUIDE.md
├── run_all.R
├── install_packages.R
├── requirements.txt
├── .gitignore
├── LICENSE
└── README.md
```

---

# Prerequisites

## Software

- R 4.3 or later recommended
- RStudio, Positron, or VS Code
- Git
- A GitHub account for publication

## R packages

```text
dplyr
readr
stringr
lubridate
tibble
haven
testthat
```

Install dependencies with:

```r
source("install_packages.R")
```

---

# Installation

```bash
git clone https://github.com/your-username/sdtm-dataset-generator.git
cd sdtm-dataset-generator
```

Then install the R packages:

```r
source("install_packages.R")
```

---

# Quick Start

Run all domains:

```r
source("run_all.R")
```

Expected CSV output:

```text
output/
├── dm.csv
├── lb.csv
├── vs.csv
└── ae.csv
```

Enable XPT output:

```r
options(sdtm.write_xpt = TRUE)
source("run_all.R")
```

Expected additional files:

```text
output/
├── dm.xpt
├── lb.xpt
├── vs.xpt
└── ae.xpt
```

---

# Running Individual Domains

Generate DM first:

```r
source("sdtm-dm/sdtm-dm-builder.R")
```

Then run the desired observation domain:

```r
source("sdtm-lb/sdtm-lb-builder.R")
source("sdtm-vs/sdtm-vs-builder.R")
source("sdtm-ae/sdtm-ae-builder.R")
```

---

# Input and Output Examples

## Raw DM

```text
study_id,site_id,subject_id,birth_date,sex,race,first_dose_date
STUDY-2026-001,101,0001,1984-02-15,M,WHITE,2026-01-10
```

## SDTM DM

```text
STUDYID,DOMAIN,USUBJID,SUBJID,SITEID,BRTHDTC,AGE,AGEU,SEX,RACE
STUDY-2026-001,DM,STUDY-2026-001-0001,0001,101,1984-02-15,41,YEARS,M,WHITE
```

## Raw LB

```text
study_id,subject_id,lab_test_code,original_result,original_unit
STUDY-2026-001,0001,HGB,14.2,g/dL
```

## SDTM LB

```text
STUDYID,DOMAIN,USUBJID,LBSEQ,LBTESTCD,LBORRES,LBORRESU,LBSTRESN
STUDY-2026-001,LB,STUDY-2026-001-0001,1,HGB,14.2,g/dL,14.2
```

---

# Domain Details

## DM — Demographics

Main transformations:

- create `USUBJID`;
- convert reference dates;
- derive `AGE`;
- set `AGEU`;
- standardize sex, race, and ethnicity;
- map planned and actual treatment arms;
- validate one record per subject.

Key:

```text
USUBJID
```

## LB — Laboratory Test Results

Main transformations:

- map test code and name;
- preserve original results;
- create standardized results;
- map units and reference ranges;
- map specimen and visit;
- generate `LBSEQ`;
- handle `NOT DONE` records.

Key:

```text
USUBJID + LBSEQ
```

## VS — Vital Signs

Main transformations:

- represent each test as a separate record;
- preserve original results;
- standardize values and units;
- map position and location;
- generate `VSSEQ`.

Key:

```text
USUBJID + VSSEQ
```

## AE — Adverse Events

Main transformations:

- preserve the verbatim term;
- map coded term and body system;
- standardize severity and seriousness;
- map relationship, action, and outcome;
- handle ongoing events;
- generate `AESEQ`.

Key:

```text
USUBJID + AESEQ
```

---

# Reusable Utilities

Shared functions are stored in:

```text
R/utils-sdtm.R
```

Main functions:

```r
project_root()
read_raw_csv()
normalize_character()
normalize_upper()
to_iso_date()
to_iso_datetime()
make_usubjid()
derive_age_years()
add_seq()
assert_columns()
assert_no_missing()
assert_unique_key()
assert_constant()
assert_values()
assert_iso8601()
assert_subjects_in_dm()
assert_start_before_end()
write_sdtm()
```

These utilities keep validation and transformation behavior consistent across domains.

---

# Validation Strategy

The repository includes educational checks for:

## Input structure

- expected raw variables exist;
- required source files exist.

## Dataset structure

- required target variables exist;
- identifiers are populated;
- `DOMAIN` has the expected value;
- keys are unique.

## Cross-domain consistency

- LB, VS, and AE subjects must exist in DM.

## Controlled values

Example checks include:

- `SEX`;
- `AESEV`;
- `AESER`;
- `LBSTAT`;
- `VSPOS`.

## Dates

- basic ISO 8601 structure;
- start date not after end date;
- ongoing AE records without completed end date.

## Domain-specific logic

- laboratory `NOT DONE` records require a reason;
- sequence variables must be unique within subject.

These checks do not replace CDISC CORE, Pinnacle 21, sponsor rules, or authority-specific validation.

---

# Controlled Terminology

The repository includes a small demonstration file:

```text
data/metadata/controlled_terminology_demo.csv
```

For production work:

1. identify the applicable terminology release;
2. freeze the release for the delivery;
3. use official terminology where applicable;
4. document sponsor extensions;
5. align values with Define-XML;
6. validate coded values consistently.

---

# Date and Time Handling

Examples:

```text
2026
2026-01
2026-01-15
2026-01-15T08:30:00
```

The project preserves uncertainty instead of silently imputing missing date components.

Analysis-date imputation should normally be documented in the statistical analysis plan and implemented in ADaM rather than embedded silently in SDTM conversion logic.

---

# Testing

Run tests from the repository root:

```r
source("tests/testthat.R")
```

Current tests verify:

- output files are created;
- each dataset has the expected `DOMAIN`;
- DM is unique by `USUBJID`;
- LB, VS, and AE sequence keys are unique.

Recommended future tests:

- variable order;
- variable types and lengths;
- record counts;
- mapping results;
- terminology coverage;
- partial-date handling;
- regression comparisons;
- metadata consistency.

---

# Adding a New Domain

Example: adding CM.

## 1. Create the folder

```text
sdtm-cm/
```

## 2. Add files

```text
sdtm-cm/sdtm-cm-builder.R
sdtm-cm/README.md
data/raw/raw_cm.csv
```

## 3. Reuse utilities

```r
source("R/utils-sdtm.R")
```

## 4. Build the domain

```r
raw_cm <- read_raw_csv("data/raw/raw_cm.csv")
dm <- load_dm_output()

cm <- raw_cm |>
  transmute(
    STUDYID = normalize_upper(study_id),
    DOMAIN = "CM",
    USUBJID = make_usubjid(study_id, subject_id),
    CMTRT = normalize_character(medication_name),
    CMSTDTC = to_iso_date(start_date),
    CMENDTC = to_iso_date(end_date)
  ) |>
  add_seq("CM", c("CMSTDTC", "CMTRT"))
```

## 5. Validate

```r
assert_subjects_in_dm(cm, dm, "CM")
assert_unique_key(cm, c("USUBJID", "CMSEQ"), "CM")
```

## 6. Register and test

Add the builder to `run_all.R` and create unit tests.

---

# Production Enhancements

Recommended enhancements:

- `renv` for dependency locking;
- `targets` for orchestration;
- metadata-driven mappings;
- complete Controlled Terminology management;
- Define-XML metadata preparation;
- GitHub Actions;
- validation reports;
- independent QC;
- traceability matrices;
- enhanced XPT checks;
- release notes;
- issue and decision logs.

---

# Roadmap

## Phase 1 — Foundational Domains

- [x] DM
- [x] LB
- [x] VS
- [x] AE
- [x] Shared utilities
- [x] Synthetic raw data
- [x] Basic tests
- [x] CSV output
- [x] Optional XPT output

## Phase 2 — Conduct and Treatment

- [ ] DS
- [ ] SV
- [ ] SE
- [ ] EX
- [ ] EC

## Phase 3 — Medical History and Medications

- [ ] MH
- [ ] CM
- [ ] PR

## Phase 4 — Metadata and Relationships

- [ ] SUPPQUAL
- [ ] RELREC
- [ ] Trial Design domains
- [ ] Mapping templates
- [ ] Define-XML metadata

## Phase 5 — Automation and Quality

- [ ] `renv`
- [ ] `targets`
- [ ] GitHub Actions
- [ ] Expanded unit tests
- [ ] Automated validation reports
- [ ] Controlled Terminology version management

---

# Limitations

The current project does not include:

- complete SDTMIG metadata;
- every required, expected, and permissible variable;
- complete Controlled Terminology;
- Define-XML generation;
- annotated CRF generation;
- SDRG generation;
- complete regulatory validation;
- medical coding workflows;
- every partial-date scenario;
- sponsor-specific standards;
- real EDC traceability;
- qualification documentation.

All sample data are synthetic and intentionally small.

---

# References

- [CDISC SDTM](https://www.cdisc.org/standards/foundational/sdtm)
- [CDISC SDTMIG](https://www.cdisc.org/standards/foundational/sdtmig)
- [CDISC Controlled Terminology](https://www.cdisc.org/standards/terminology/controlled-terminology)
- [CDISC CORE](https://www.cdisc.org/core)
- [CDISC Standards](https://www.cdisc.org/standards)

Always confirm the applicable versions and regulatory technical specifications before starting a real submission project.

---

# Contributing

Contributions are welcome.

A contribution should include:

1. a clear use case;
2. documented mapping decisions;
3. readable R code;
4. synthetic or nonconfidential data;
5. validation checks;
6. unit tests;
7. updated documentation.

Suggested contributions:

- new domain builders;
- validation improvements;
- metadata templates;
- mapping examples;
- test coverage;
- documentation corrections;
- CI workflows.

---

# License

This project is available under the [MIT License](LICENSE).

CDISC standards and related materials remain subject to their respective terms of use and copyright conditions.

---

<div align="center">

### Built for clinical programmers, statistical programmers, data standards specialists, and R developers

**Reliable clinical data starts with transparent standards and reproducible code.**

</div>
