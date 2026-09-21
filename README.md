# Premier League Odds Calibration, 2002/03 – 2025/26

Bookmakers publish prices, not probabilities. There are three outcomes of a football match that when turned into implied probabilities they sum to between 102% and 108% rather than 100%, because a margin is built into every price, known as the overround. Remove that margin and a testable question remains: when Bet365 prices an outcome at 30%, does it happen 30% of the time?

This applies one calibration procedure to 9,120 Premier League fixtures across 24 seasons, then repeats it for each outcome and each month of the season.

## Method

The margin is removed with the **power method**, which solves for the exponent `k` such that the three quoted probabilities raised to `k` sum to 1. This is the preferred method compared to more simplistic methods such as the additive or the multiplicative methods that are poor at producing implied probabilities for underdogs and large favourites relative to their actual rate of occurrence. So the power method should produce the most accurate underlying probabilities and therefore if there is miscalibration evident here it is more likely to be 'true' as a result of this methodology. In order to optimise this de-vigging method I needed to bin the data due to the fact that looking at the number of data points per percentage point results in a very small number of data points in many probabilities so binning the data appropriately ensured large enough sample sizes in most cases to perform proper analysis.

Probabilities are grouped into 5-percentage-point bins. For each bin several metrics were calculated in order to properly visualise the calibration:

- **Expected events** = Σ*p* over the fixtures in the bin
- **Variance** = Σ*p*(1−*p*), since the variances of independent 0/1 outcomes add
- **Error bars** = ±1.96 √variance, a 95% interval, drawn only where at least 5 successes and 5 failures are expected

Significance is tested with a **chi-squared goodness-of-fit** statistic using both the calculated expected events and the number of observed events to compute the chi-squared statistics properly. Adjacent bins are merged until each group holds at least 5 expected successes *and* 5 expected failures before the test is applied. This is due to the fact that at very low counts the distribution differs significantly from the chi-squared distribution as count data is obviously discrete but the chi-squared distribution is continuous. So at very low sample sizes the distribution is 'steppy' and diverges far from the chi-squared distribution.

Calibration quality was measured mostly visually but also quantitatively using the Brier Score for each result individually. The predominant visuals used were calibration plots whereby perfect calibration was plotted as y=x then the implied mean probability for each probability bin was plotted against the actual frequency/rate of occurrence of events that were within each probability bin. Perfect calibration represents events occurring at the rate they are said to occur at and deviation from this line represents a miscalibration whereby Bet365 either **overestimates** how often an event occurs or **underestimates** how often an event occurs. Both are miscalibrations but represent different outcomes for the bookmaker. The **Brier score** is compared against a base-rate benchmark: a forecaster who ignores the fixture and always quotes the long-run rate of that outcome to assess whether the bookmaker's models were actually 'good'/calibrated or whether they just followed the base rate.

## Results

### Calibration is good, but not as good as the aggregate suggests

<p align="center">
  <img src="images/overall_calibration.png" width="49%" alt="Calibration, all outcomes combined">
  <img src="images/calibration-home.png" width="49%" alt="Calibration, home wins">
  <img src="images/calibration-draw.png" width="49%" alt="Calibration, draws">
  <img src="images/calibration-away.png" width="49%" alt="Calibration, away wins">
</p>

Pooled across all three outcomes, bins sit a mean of **0.93 percentage points** from perfect calibration, 17.61 points in total across 19 populated bins. Close, but not uniform: one bin, **30–35%**, falls outside its 95% interval, with 574 observed against 623.2 expected — a gap of 49.2 against a bar of ±40.2. A lot of the miscalibration on show is evident in the red-coloured data points which represents bins with far lower data points. But, the 30-35% bin is unlikely noise due to its large sample size so there is potential evidence for miscalibration here overall.

<p align="center">
  <img src="images/total_obs_v_exp.png" width="49%" alt="Calibration, all outcomes combined">
  <img src="images/home_obs_v_exp.png" width="49%" alt="Calibration, home wins">
  <img src="images/draw_obs_v_exp.png" width="49%" alt="Calibration, draws">
  <img src="images/away_obs_v_exp.png" width="49%" alt="Calibration, away wins">
</p>

Here we can see the observed vs. expected counts for every bin for each result as well as overall. The error bar represents $\pm$ 95% (or 1.96 standard deviations) of the expected value and therefore any observed value for any bin outside of this range is considered 'significant'. Assessing the overall plot here we can see that pretty much every bin has the observed count within the error bars range except for the 30-35% bin whereby the observed count was 574 and the expected count was 623.2$\pm$40.2. This is unlikely due to noise in my opinion and is likely a systematic miscalibration being highlighted because of the large sample size. There are several occasions in this analysis where calibration appears present but is unlikely significant/true due to the low sample size and therefore high noise, but this is the largest sample size where there is a of-interest result. This bin represents ~6% of all the data present and is one of the most populated bins. Now this alone does not imply miscalibration for-sure but it is a big indicator that could indicate miscalibration.
Assessing the individual results has to be interpreted differently due to the fact that the sample sizes are much smaller and roughly 3 times smaller on average than the same corresponding bins for the overall assessment. Looking at the home plot first we can see that there are 2 bins that have an observed count outside of the expected count: 20-25% and 55-60% with both of them having observed counts above the expected ranges, indicative of a potential underestimation by Bet365 which is the worst form of miscalibration as if events occur at a higher rate than anticipated that means the price a bettor is getting is a better price than the probabilities imply and produces positive expected value for the bettor in the long-run and losses for Bet365 in these incidents. the 20-25% bin for home results only has ~120 counts so it is a less significant finding but the 55-60% bin has ~350 counts which is 3 times larger and is the 4th most populated bin. Using this information and the calibration plot we can see that this is probably the largest sign of miscalibration present that is significant: sample size is large, calibration plot shows clear deviation from the perfect calibration line and the observed count is outside of the error bars of the expected count.

No combined chi-squared test is reported. Every fixture contributes three rows to the pooled table and exactly one of them succeeds, so those rows are perfectly dependent and the independence assumption the test requires does not hold.

### Per outcome

Looking at the per-result calibration plots we can see that for the home results the 55-60% bin shows signs of miscalibration. The observed number of events falls outside of the error bars and there is significant deviation from perfect calibration line. This bin also has over 300 counts present so it is not due to the low sample size. The 20-25% bin also shows evidence of miscalibration as there is a somewhat decent sample size of just over 100 data points and the observed count falls outside of the error bars plus the calibration plot data point for this bin is distinctly above perfect calibration.
Then the draw plots there is a lot fewer bins with data and only 1 bin with a significant sample size to comment on at all which is the 25-30% bin. The observed count is within the error bars and the point on the calibration plot deviates very slightly from perfect calibration. This is somewhat surprising to me due to the nature of draws in football and the fact that they are notoriously hard to predict so this is evidence of Bet365' models to be very good here. Looking at lower sample size bins though we can see very poor calibration, almost entirely due to the small sample sizes.
Finally, looking at the away plots we see that all the observed counts fall within the error bars and all the bins with a large sample size are very close to the perfect calibration line which is representative of very good calibration.

| | Mean error | as fixtures | χ² | bins | p |
|---|---|---|---|---|---|
| Home wins | +0.367% | +33.5 | 25.24 | 17 | **0.066** |
| Draws | +0.232% | +21.2 | 2.92 | 6 | 0.712 |
| Away wins | −0.600% | −54.7 | 9.94 | 17 | 0.870 |

No outcome is significantly miscalibrated at the 5% level. Home wins come closest: they occur about 33 times more often across the period than their prices imply, and the home p-value is an order of magnitude lower than the other two.

Mean error across all outcomes combined is ~10⁻¹⁴, which is an identity rather than a result — the three probabilities sum to 1 and exactly one outcome occurs — so it serves as a check on the margin removal.

### The draw market is well calibrated and nearly uninformative

| | Base rate | Brier | Brier, base rate only | Skill |
|---|---|---|---|---|
| Home wins | 45.6% | 0.2113 | 0.2481 | **14.8%** |
| Draws | 24.7% | 0.1831 | 0.1858 | **1.4%** |
| Away wins | 29.7% | 0.1773 | 0.2088 | **15.1%** |

Draws have the *lowest* Brier score of the three outcomes, which naively reads as the best-priced market. They are not — Brier is automatically low for rare events. Measured against a forecaster who quotes 24.7% on every fixture and never looks at the teams, Bet365's draw prices are only **1.4% better**, against roughly 15% for home and away wins.

So "Bet365 prices draws well" needs stating carefully: the draw prices are extremely well calibrated (χ² = 2.92 across 6 bins) and carry almost no information about *which* fixtures will be drawn. Pricing everything near the base rate is an easy way to be well calibrated and an uninformative way to be right. That reflects something real about football rather than a failing of the bookmaker.

### March

<p align="center">
  <img src="images/monthly-error-home.png" width="32%" alt="Monthly error, home wins">
  <img src="images/monthly-error-draw.png" width="32%" alt="Monthly error, draws">
  <img src="images/monthly-error-away.png" width="32%" alt="Monthly error, away wins">
</p>

Mean error is near zero in every month except March, where home wins and draws move in opposite directions — roughly mirror images.

> The notebook reports the raw monthly p-values. The Bonferroni-adjusted column below is computed here rather than in the notebook: it is simply `p × 30`, the 30 being 3 outcomes × 10 months (June and July are excluded as they contain only rescheduled Covid-era fixtures).

| | p | Bonferroni-adjusted |
|---|---|---|
| **March, draws** | **0.00046** | **0.014** |
| March, home wins | 0.032 | 0.96 |
| May, draws | 0.032 | 0.96 |

March draws produce 150 observed against 196.7 expected — **46.7 fewer than priced**, effect size *w* = 0.19. It is the only monthly result to survive correction for multiple comparisons, and it does so comfortably. March home wins move in the opposite direction but do not survive correction.

Whether this is a genuine seasonal effect or an artefact of fixture rescheduling is not resolved here.

### Would any of it have made money?

Flat-staking every fixture on a single outcome, at Bet365's own quoted prices, across all 24 seasons:

| | Total return | Mean ROI per season | Best season | Worst season |
|---|---|---|---|---|
| Home wins | −£2,693.59 | −2.95% | +9.48% | −18.41% |
| Draws | −£6,378.95 | −6.99% | +7.69% | −28.21% |
| Away wins | −£9,000.39 | −9.87% | +28.62% | −31.42% |

All three lose, which is what a 5.4% average margin buys the bookmaker. Calibration being close to correct and betting being unprofitable are entirely compatible: the margin sits on top of the prices, so even perfectly calibrated odds return less than stake over time.

## Limitations

- **These are pre-match odds, not closing odds.** football-data.co.uk publishes Bet365's closing line separately (`B365CH`/`CD`/`CA`), but only for 2,660 of the 9,120 fixtures, beginning around 2019/20. That count comes from the source CSVs directly — the notebook drops those columns during cleaning, so it does not appear in the analysis. The closing line is the efficient one, so testing openers is a different — and arguably more interesting — question, but it is a different question.
- **Matches are treated as independent.** They are not: injuries, managerial changes and league position all carry across fixtures. The chi-squared test assumes independence, and the p-values should be read with that in mind.
- **Binning discards information.** Five-point bins give usable sample sizes at the cost of resolution within each bin.
- **The March result is one finding among 30 tests.** It survives Bonferroni correction, but it was found by looking rather than predicted in advance.

## Data

Historical results and pre-match odds from [football-data.co.uk](https://www.football-data.co.uk/), Bet365's 1X2 market.

26 seasons were collected. The first two, 2000/01 and 2001/02, carry no Bet365 odds and are excluded, leaving 24 seasons and 9,120 fixtures from 2002/03 to 2025/26. One row with no date — a stray line in the source data rather than a fixture — is dropped.

## Running it

```bash
git clone https://github.com/Joshc386/premier-league-odds-analysis.git
cd premier-league-odds-analysis
jupyter lab BetProject.ipynb
```

The CSVs are in `PL_Data/` and are read by relative path, so Restart & Run All works from a clean clone. The figures in this README are written by the notebook itself, so they cannot drift from the analysis.

Python — pandas, NumPy, SciPy, matplotlib, seaborn, ipywidgets.

## The analysis

Full workflow, including the interactive per-season plots and the complete monthly breakdowns, is in [`BetProject.ipynb`](BetProject.ipynb).
