# DPAC

Cohort assembly for NHANES diabetes research: raw survey files from seven
two-year cycles, converted to CSV and merged into one analysis table.

This is the data-preparation stage that feeds
[dp-diabetes-prediction](https://github.com/sha256rma/dp-diabetes-prediction).
It contains no modelling code.

## What is here

Seven cycles of [NHANES](https://www.cdc.gov/nchs/nhanes/), 2005-2006 through
2017-2018, covering 14 years. Each cycle directory holds the original SAS
transport files under `XPT/` and their CSV conversions alongside.

Eight NHANES components are pulled per cycle:

| Prefix | Component |
|--------|-----------|
| DEMO | Demographics |
| BMX | Body measures |
| BPX | Blood pressure examination |
| BPQ | Blood pressure and cholesterol questionnaire |
| DIQ | Diabetes questionnaire |
| GLU | Plasma fasting glucose |
| TCHOL | Total cholesterol |
| MCQ | Medical conditions questionnaire |

## Output

`merged_data.csv`: 8,290 participants, 14 columns, joined across components on
the NHANES respondent sequence number `SEQN`.

```
SEQN, BMXWT, BMXBMI, BMXWAIST, BPQ020, BPQ080, BPQ090D, RIAGENDR,
RIDAGEYR, DIQ010, LBDGLUSI, MCQ300C, LBDTCSI, BPXSY_avg
```

`DIQ010` (self-reported diabetes diagnosis) is the prediction target downstream.

## How it works

- `2005-2006/merge.ipynb` and `2007-2008/merge.ipynb`: per-cycle merges.
- `script.ipynb`: walks every cycle directory, drops rows with missing values,
  casts `SEQN` to int, and concatenates the cycles into `merged_data.csv`.

Run `script.ipynb` from the repository root. It discovers cycle directories by
globbing for `*-*`, so it picks up new cycles without code changes.

## Data

NHANES is public, de-identified survey data released by the US CDC National
Center for Health Statistics. No access request is needed and nothing here is
personally identifying. Source: https://www.cdc.gov/nchs/nhanes/
