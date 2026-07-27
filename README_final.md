# Air Quality in Bangladesh (2022–2025)

**A spatiotemporal analysis of hourly air quality across 27 cities**

![R](https://img.shields.io/badge/R-4.2%2B-276DC3?logo=r&logoColor=white)
![Analysis](https://img.shields.io/badge/analysis-R%20Markdown-75AADB)
![License](https://img.shields.io/badge/license-MIT-green)

---

## Overview

This project characterises the level, spatial pattern and temporal behaviour of
air pollution across 27 cities in Bangladesh, and benchmarks observed
concentrations against the WHO 2021 Global Air Quality Guidelines.

The analysis addresses five questions:

1. How polluted is the air, and how does it compare to WHO guidelines?
2. Which cities carry the highest burden?
3. How does pollution vary across the day, the week and the year?
4. Is air quality improving or deteriorating over the study period?
5. How are the different pollutants related to one another?

**[View the rendered report](https://nabedist29.github.io/bangladesh-air-quality-2022-2025/)**

---

## Data

| Property | Value |
|---|---|
| Records | 782,784 hourly observations |
| Cities | 27 |
| Period | 4 August 2022 – 23 November 2025 |
| Temporal resolution | Hourly |
| Pollutants | PM2.5, PM10, CO, NO2, SO2, O3, AQI |
| Panel structure | Balanced (28,992 records per city) |

Carbon dioxide is present in the raw file but is dropped during cleaning, as
approximately 67% of its values are missing.

---

## Repository structure

```
.
├── AQI_Bangladesh_Analysis.Rmd     # main analysis document
├── AQI_Bangladesh_Analysis.html    # rendered report
├── data/
│   └── AQI_Bangladesh.csv          # raw dataset
├── figures/                        # exported figures
└── README.md
```

---

## Reproducing the analysis

**Requirements:** R 4.2 or later.

```r
install.packages(c(
  "tidyverse", "lubridate", "scales", "viridis", "patchwork",
  "corrplot", "trend", "knitr", "maps", "ggrepel", "rmarkdown"
))
```

**Steps:**

1. Clone the repository.
2. Place `AQI_Bangladesh.csv` in a `data/` folder at the project root.
3. Render the report:

```r
rmarkdown::render("AQI_Bangladesh_Analysis.Rmd")
```

Chunk caching is enabled, so the first render is slower and subsequent renders
are fast.

---

## Methods

**Data preparation.** Hour strings are parsed into numeric hours and combined
with date fields into a datetime column. Small negative values in NO2 and O3,
which arise from instrument rounding near zero, are clipped to zero. Seasons
follow the standard Bangladesh calendar: Winter (Dec–Feb), Pre-monsoon
(Mar–May), Monsoon (Jun–Sep), Post-monsoon (Oct–Nov).

**Descriptive analysis.** Distributions are summarised by mean, standard
deviation and selected percentiles. Because all pollutant distributions are
strongly right-skewed, medians and percentiles are reported alongside means.

**AQI classification.** Hourly AQI values are assigned to the six standard US EPA
categories, expressing pollution as time spent in each health-risk band.

**Guideline exceedance.** Hourly values are aggregated to city-day means and
compared against WHO 2021 24-hour guidelines: PM2.5 = 15, PM10 = 45, NO2 = 25,
SO2 = 40 µg/m³.

**Correlation.** Spearman rank correlation is used rather than Pearson, since
relationships between pollutants are monotonic but not linear.

**Trend testing.** The Mann-Kendall test assesses monotonic trend in city-level
monthly mean PM2.5, with Sen's slope estimating magnitude. Both are
non-parametric and robust to outliers.

---

## Key findings

**Concentrations are far above health guidelines.** All 27 cities exceed the WHO
annual PM2.5 guideline of 5 µg/m³ by a wide margin, and the 24-hour guideline of
15 µg/m³ is breached on the majority of days almost everywhere.

**A clear geographic gradient exists.** Northern and central cities — Bheramara,
Bogra, Dinajpur — carry the highest burden. Coastal and hill cities in the
southeast, including Bandarban, Chittagong and Cox's Bazar, are markedly cleaner.

**Season dominates all other variation.** The winter-to-monsoon contrast exceeds
the contrast between the most and least polluted city, reflecting wet deposition
and improved atmospheric mixing during the monsoon.

**Diurnal cycles point to traffic and photochemistry.** Particulates and NO2 peak
twice daily at traffic hours, while ozone peaks in the early afternoon,
consistent with sunlight-driven formation.

**Trend estimates are weak over this record.** The series spans just over three
years and does not begin at a year boundary. Given the magnitude of the seasonal
signal, trend estimates are sensitive to start and end points and are reported as
descriptive rather than conclusive.

---

## Limitations

1. Coverage is limited to 27 cities and does not constitute a national sample.
2. The record begins in August 2022 and ends in November 2025, leaving two
   incomplete calendar years. Year-on-year comparison is restricted to 2023–2024.
3. Carbon dioxide is excluded due to extensive missingness.
4. Values are used as reported, without validation against reference-grade ground
   monitors.
5. Inter-pollutant correlation indicates shared sources but does not establish
   causation.

---

## Next steps

- Link concentrations to health outcome data to estimate attributable disease
  burden.
- Add meteorological covariates (temperature, wind speed, boundary layer height)
  to separate emission changes from weather-driven variability.
- Apply source apportionment to the PM2.5/PM10 ratio and trace gas profile to
  identify dominant emission sectors.
- Extend to distributed lag non-linear models once a health outcome series is
  available, since pollution effects on health are both delayed and non-linear.

---

## Related work

- **Spatiotemporal Epidemiology of Dengue Fever Across All 64 Districts of
  Bangladesh** — *BMC Public Health*, in press
- Additional projects at [nabedist29.github.io](https://nabedist29.github.io)

---

## Author

**Istiaque Ahamed Nabed**
Department of Disaster Science and Climate Resilience
University of Dhaka

---

## License

Released under the MIT License. The underlying dataset remains subject to its
original terms of use.
