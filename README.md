# sdtm-dataset-generator

A practical and reproducible R project that demonstrates how to transform raw clinical trial data into **CDISC SDTM-compliant datasets**.

## Overview

This repository provides a modular framework for building the most commonly used SDTM domains from raw clinical data while documenting every transformation step.

### Included Domains

- DM — Demographics
- LB — Laboratory Test Results
- VS — Vital Signs
- AE — Adverse Events

## Features

- Modular SDTM builders
- Reusable R utilities
- Simulated raw datasets
- ISO 8601 date handling
- Automatic USUBJID generation
- Automatic --SEQ generation
- Basic CDISC-oriented validation
- CSV export
- Optional SAS XPT export
- Unit test structure

## Repository Structure

```text
R/
data/
output/
sdtm-dm/
sdtm-lb/
sdtm-vs/
sdtm-ae/
docs/
tests/
```

## Prerequisites

- R 4.3+
- Packages listed in requirements.txt

```r
source('install_packages.R')
```

## Quick Start

```r
source('run_all.R')
```

Outputs:

- output/dm.csv
- output/lb.csv
- output/vs.csv
- output/ae.csv

Enable XPT export:

```r
options(sdtm.write_xpt = TRUE)
source('run_all.R')
```

## Validation

The project includes educational validation routines for required variables, duplicate keys, subject consistency, ISO-8601 dates, controlled terminology examples, and start/end date consistency. These checks are not a replacement for regulatory validation tools.

## Extending the Repository

1. Create a new domain folder.
2. Add a builder script.
3. Add raw data.
4. Reuse utilities.
5. Register the builder in run_all.R.
6. Add tests.

## References

- https://www.cdisc.org/standards/foundational/sdtm
- https://www.cdisc.org/standards/foundational/sdtmig
- https://www.cdisc.org/core
