# Biathlon Target Analysis

Eye-tracking analysis of biathlon shooting performance across fatigue conditions.

## Dataset

`Biathlon Metrics.tsv` — 300 rows from 21 participants across 3 conditions (`no_fatigue`, `fatigue1`, `fatigue2`), 5 target positions, recorded in two viewing orders (`L-R`, `R-L`).

## Pipeline

1. **Clean** — Drop participants with `Order = "Other"` (no valid L-R/R-L mapping)
2. **Normalize** — Standardize condition names (`fatigue1`, `fatigue2`, `no_fatigue`)
3. **Flatten** — Columns `.1`–`.5` = physical positions; `Interval` = viewing step. L-R: position = interval; R-L: position = 6 − interval. Flip columns via pair swap (1↔5, 2↔4)
4. **Calculate**
   - `anticipation` — count of TTFF values below 150 ms
   - `total_fix` — sum of fixations across all positions
   - `mean_ttff` — mean TTFF across all positions
   - `mean_ttff_excl1` — mean TTFF positions 2–5 only (position 1 biased by distance from fixation cross)

## Visualizations

| Plot | Description |
|------|-------------|
| `boxplots.png` | Total fixations, mean TTFF, mean TTFF excl. pos1 by condition |
| `anticipation_histogram.png` | Side-by-side bar chart of anticipatory response counts (grayscale) |

## Export

`biathlon_export.xlsx` with two sheets:

- `all_fixations` — full table (17 columns)
- `first_fixation` — Participant, condition, Order, fix_1, ttff_1

## Setup

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install pandas numpy matplotlib jupyter openpyxl
```

## Run

```bash
jupyter notebook analysis.ipynb
```

Execute all cells to regenerate exports.
