# Drivers of Smallholder Farm Yield and Profitability in Ghana

**Evidence from the Ghana Agricultural Household Survey (GAHS)**

Margaret Naa Dei Neequaye · Fred Achi · Korang Pearl Antwi · Gilbert Woasieworm Adjei · Lydia Boatemaa Oduro
DSCD 602 — Data Science Capstone, University of Ghana, 2026

---

## Overview

This project investigates what farm-level, behavioural, and market-access factors
are associated with crop yield and profitability among smallholder farmers in
Ghana, and whether farmers can be usefully segmented into distinct typologies
based on their production and market-access profiles.

The analysis uses the **Ghana Agricultural Household Survey (GAHS)** — a
simulated but realistically structured dataset of 2,000 farm households linked
to 120 local markets across all 16 regions of Ghana — and works through the
full pipeline from raw joins to a final model and an unsupervised segmentation.

## Repository contents

```
.
├── final_project.Rmd      # Full analysis, source of the PDF/DOCX report
├── final_project.pdf      # Rendered report
├── final_project.docx     # Rendered report (Word)
├── gahs_farm.csv          # Farm-level survey data (2,000 rows x 43 columns)
└── gahs_market.csv        # Market-level survey data (120 rows x 18 columns)
```

`final_project.Rmd` expects both CSVs in the same directory as itself (the
default R Markdown working directory), so no path changes are needed to knit
it as-is.

## What's in the analysis

| Task | What it covers |
|---|---|
| 1 | Import, many-to-many join resolution (market aggregated to Region x Survey_Year before joining), missing-value audit |
| 2 | Wrangling pipeline: `pivot_longer()` reshape, string manipulation, filtering/mutating, group-aware missing-value imputation |
| 3 | Exploratory visualisation: yield distribution, education, fertiliser-by-profitability, a region x practice adoption heatmap, and a regional choropleth |
| 4 | Descriptive summary table (`gtsummary`) and three inferential tests: t-test (yield by improved seed use), Kruskal-Wallis (yield by soil quality), Fisher's exact test (credit access vs. profitability) |
| 5 | Multiple linear regression on yield, with LINE assumption checks, VIF, and `stepAIC()` variable selection |
| 6 | K-means and hierarchical clustering (k=4, selected via elbow + silhouette) to segment farmers into production/market-access typologies, with a cluster choropleth |

## Key results

- Resolving the farm-market join required aggregating the market file to one
  row per Region x Survey_Year before joining — a naive join on those keys
  inflates 2,000 farm rows to 4,889 via a many-to-many match.
- Improved seed use is associated with significantly higher yield (2.82 vs.
  2.39 t/ha, p < .001, Cohen's d = -0.47); soil quality and credit access show
  no significant association with yield or profitability in this sample.
- The final regression (R-squared = 0.145) retains farm size, fertiliser use,
  and extension visits as significant positive predictors of yield; region and
  land tenure add little once those are controlled for.
- K-means clustering (k=4) separates an input-intensive/extension-connected
  group, a larger-farm group, a low-input smallholder majority, and a
  distance-from-market group — graded tendencies (silhouette ~ 0.24) rather
  than sharply separated types.

## Reproducing the report

Requires R with: `dplyr`, `tidyr`, `readr`, `stringr`, `forcats`, `ggplot2`,
`patchwork`, `viridis`, `sf`, `leaflet`, `rstatix`, `broom`, `car`, `vcd`,
`cluster`, `factoextra`, `flextable`, `officer`, `gtsummary`, and (for the
choropleth) `geodata` with a fallback to `rnaturalearth` if no network access
is available at knit time.

```r
rmarkdown::render("final_project.Rmd")
```
