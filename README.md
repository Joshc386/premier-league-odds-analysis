# Premier League Odds Calibration, 2002/03 – 2025/26

Bookmakers publish prices, not probabilities. Convert the three outcomes of a football match into implied probabilities and they sum to between 102% and 108% rather than 100%, because a margin is built into every price. Remove that margin and a testable question remains: when Bet365 prices an outcome at 30%, does it happen 30% of the time?

This applies a single calibration-testing procedure to 9,120 Premier League fixtures across 24 seasons, and repeats it identically for every outcome and every month of the season.

## Approach

The margin is removed using the power method, which solves for the exponent that brings the three implied probabilities to a sum of 1, rather than the naive reciprocal, which distributes the margin unevenly across outcomes. Probabilities are then grouped into 5-point bins, with each bin's mean implied probability computed from its constituent probabilities rather than taken from the bin midpoint.

Each slice of the data is then put through the same battery:

| Step | Purpose |
|---|---|
| Expected vs observed counts | Raw difference between predicted and actual outcomes per bin |
| Expected vs observed with error ranges | Whether observed counts fall inside the range binning alone would explain |
| Calibration plot | Implied probability against actual frequency, weighted by bin population |
| ME, MAE and RMSE | Direction, typical magnitude, and sensitivity to large individual errors |
| Chi-squared goodness-of-fit | Whether deviations exceed random variation |
| Effect size | Whether a significant deviation is large enough to matter |

That battery is applied to all outcomes combined, then to home wins, draws and away wins separately, then to each month within each of those. Bin counts are checked against the minimum expected-frequency assumption before any chi-squared test is run, and June and July are excluded as they contain only rescheduled Covid-era fixtures.

## Results by outcome

| Outcome | Mean error | MAE | RMSE |
|---|---|---|---|
| All combined | ≈ 0 | 0.381 | 0.437 |
| Home wins | **+0.692** | 2.134 | 2.88 |
| Draws | +0.0023 | 0.37 | 0.77 |
| Away wins | −0.006 | 0.36 | 0.42 |

Chi-squared returns insufficient evidence to reject the null hypothesis in every case: p = 0.979 for all outcomes combined, p = 0.63 for home wins (χ² = 14.45 across 18 bins), and p = 0.87 for draws. On these tests, the prices are consistent with the results.

The mean error for all outcomes combined sits at approximately zero by construction — the three implied probabilities sum to 1 and exactly one outcome occurs — so it functions as a sanity check rather than a result.

The asymmetry between outcomes is the substantive finding. Home wins deviate by 0.692%, two orders of magnitude larger than draws or away wins, and occur more often than their prices imply — roughly 31 fixtures across the period, or 1.3 per season. Three bins (0–5%, 20–25% and 55–60%) contribute close to half of the total deviation. Draws and away wins are priced almost exactly, at well under a single fixture's worth of error each.

## Calibration

<p align="center">
  <img src="images/calibration-all-outcomes.png" width="49%" alt="Calibration, all outcomes combined">
  <img src="images/calibration-home.png" width="49%" alt="Calibration, home wins">
  <img src="images/calibration-draw.png" width="49%" alt="Calibration, draws">
  <img src="images/calibration-away.png" width="49%" alt="Calibration, away wins">
</p>

All outcomes combined (top left) sit on the diagonal almost exactly. The three components do not: home wins (top right) scatter visibly, draws (bottom left) deviate at every point though only one bin carries a large sample, and away wins (bottom right) track the line closely apart from the 60–65% and 80–85% bins.

The combined plot conceals this. Where one outcome is mispriced in a bin, the other two absorb it, bringing each bin's observed count back inside the expected range — so the aggregate looks better calibrated than any of its parts.

## Monthly behaviour

<p align="center">
  <img src="images/monthly-error-home.png" width="49%" alt="Monthly error metrics, home wins">
  <img src="images/monthly-error-draw.png" width="49%" alt="Monthly error metrics, draws">
</p>

Mean error is near zero in every month except March, in opposite directions for home wins and draws — near mirror images. For home wins, March is roughly three times the magnitude of the next largest month; for draws it is the only month producing a statistically significant result, at p = 0.0052 with 46 fewer draws than expected.

Whether this is compensatory, noise, or a genuine signal is not yet resolved. The effect size does not make it exploitable, and it is reported here as an anomaly rather than an edge.

## Scope

The per-outcome and per-month analysis is complete. Per-season breakdowns are built for each outcome but not yet carried through the full battery, and no formal conclusions have been drawn — the findings above are read from the individual tests rather than assembled into a single verdict.

## Data

Historical results and closing odds from [football-data.co.uk](https://www.football-data.co.uk/), using Bet365's 1X2 market.

26 seasons were collected. The first two, 2000/01 and 2001/02, carry no Bet365 odds and were excluded, leaving 24 seasons and 9,120 fixtures from 2002/03 to 2025/26.

## Tools

Python — pandas, NumPy, SciPy, matplotlib, seaborn, ipywidgets.

## The analysis

Full workflow, including the interactive per-season plots and the complete monthly breakdowns, is in [`BetProject.ipynb`](BetProject.ipynb).
