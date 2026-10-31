# The Invisible Half — Rwanda Vital Statistics 2019–2025

Entry for the **NISR 2026 Infographic Competition** (Mathematics for Machine Learning, Cohort 3).

Rwanda records almost every birth (**92.9%** in 2025) but only about half of its deaths (**50.5%**).
In 2025, **38,521 deaths went unrecorded**, about 106 a day.

## Deliverables

| File | What it is |
|---|---|
| [figures/poster_invisible_half.png](figures/poster_invisible_half.png) / [.pdf](figures/poster_invisible_half.pdf) | Static infographic (2400 × 3600 px) |
| [figures/dynamic_invisible_half.mp4](figures/dynamic_invisible_half.mp4) | Dynamic infographic (submitted): 41-second HD video, 1280 × 720 |
| [figures/dynamic_invisible_half.html](figures/dynamic_invisible_half.html) | Interactive version of the same animation (Plotly), works offline in any browser |
| [notebooks/analysis.ipynb](notebooks/analysis.ipynb) | All data loading, checks, maths and figures |
| [figures/key_numbers.json](figures/key_numbers.json) | Every number quoted on the poster |

## Main findings

- **The gap is closing.** Death completeness fell to 26.2% in 2021, then rose every year to 50.5% in 2025.
  A straight line (normal equation) gives +3.8 pts/yr on all years but +6.4 pts/yr on 2021–2025.
- **Projection.** If the 2021–25 pace continues, 90% of deaths would be recorded by about
  **2031 (straight line) to 2033 (logistic S-curve fitted by gradient descent)**.
- **Fastest point now.** The S-curve's rate dy/dt = k·y(1 − y/100) peaks at 50% (6.8 pts/yr); Rwanda is there,
  so later gains will come more slowly.
- **What changed.** Verbal autopsies rose from 1,889 (2022) to 18,921 (2025), about ×10. This *coincided* with
  the rise; the data cannot show causation.
- **Women are counted less.** 57.6% of men's deaths vs 43.7% of women's were recorded (z ≈ 39). By Bayes' theorem,
  men are 49% of deaths but 56% of recorded deaths.
- **Where.** 55.9% of registered deaths occur outside health facilities (community), from 32.3% in Kicukiro to
  67.2% in Muhanga; rural 61.3% vs urban 41.4%.

## Methods

1. Linear regression via the normal equation, θ = (XᵀX)⁻¹Xᵀy
2. Logistic S-curve y = 100 / (1 + e^(−k(t − t₀))) fitted by gradient descent from scratch (MSE loss, chain-rule gradients),
   started from a naive guess and cross-checked against a logit-transform start
3. Derivative of the logistic curve and its maximum at 50%
4. Train/test validation (train ≤ 2023, test 2024–2025) of line vs S-curve
5. Probability: completeness as a binomial maximum-likelihood estimate, standard error, conditional probability by sex,
   Bayes' theorem, expected unrecorded deaths

With only 7 yearly values, projections are scenarios ("if current trends continue"), not forecasts.

## Data handling

The source has internal inconsistencies; the notebook (section 1.3) checks and documents each one.
**Where tables differ, Table 12 values are used.** In particular: deaths 2025 = 39,355; deaths 2022 = 25,536;
female completeness 2025 = 43.7% (recomputed from Table 12 counts; the table prints 43.6). Under-5 mortality and
usable community causes of death are not used because the source tables disagree. Table 14's "community" means
outside a health facility.

## Run it

```bash
python -m pip install -r requirements.txt
```
```bash
python -m jupyter nbconvert --to notebook --execute --inplace notebooks/analysis.ipynb
```

## Source

National Institute of Statistics of Rwanda (NISR), *Rwanda Vital Statistics Report 2025* and
*Final Tables_CRVS_2025.xlsx* (CRVS system, 2025).
<https://statistics.gov.rw/statistical-publications/vital-statistics/rwanda-vital-statistics-report-2025>
