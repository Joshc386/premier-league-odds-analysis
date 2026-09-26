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

Here we can see the observed vs. expected counts for every bin for each result as well as overall. The error bar represents $\pm$95% (or 1.96 standard deviations) of the expected value and therefore any observed value for any bin outside of this range is considered 'significant'. Assessing the overall plot here we can see that pretty much every bin has the observed count within the error bars range except for the 30-35% bin whereby the observed count was 574 and the expected count was 623.2$\pm$40.2. This is unlikely due to noise in my opinion and is likely a systematic miscalibration being highlighted because of the large sample size. There are several occasions in this analysis where calibration appears present but is unlikely significant/true due to the low sample size and therefore high noise, but this is the largest sample size where there is a of-interest result. This bin represents ~6% of all the data present and is one of the most populated bins. Now this alone does not imply miscalibration for-sure but it is a big indicator that could indicate miscalibration.
Assessing the individual results has to be interpreted differently due to the fact that the sample sizes are much smaller and roughly 3 times smaller on average than the same corresponding bins for the overall assessment. Looking at the home plot first we can see that there are 2 bins that have an observed count outside of the expected count: 20-25% and 55-60% with both of them having observed counts above the expected ranges, indicative of a potential underestimation by Bet365 which is the worst form of miscalibration as if events occur at a higher rate than anticipated that means the price a bettor is getting is a better price than the probabilities imply and produces positive expected value for the bettor in the long-run and losses for Bet365 in these incidents. the 20-25% bin for home results only has ~120 counts so it is a less significant finding but the 55-60% bin has ~350 counts which is 3 times larger and is the 4th most populated bin. Using this information and the calibration plot we can see that this is probably the largest sign of miscalibration present that is significant: sample size is large, calibration plot shows clear deviation from the perfect calibration line and the observed count is outside of the error bars of the expected count. Now looking at draws and the away versions we see that there are no bins with the observed count outside of the expected count range, even the bins with a small sample size. Draws only has 6 bins with data present with 2 of them having very large sample sizes so if these did show a count outside the expected range then it would be the strongest sign of a miscalibration here. The draws appear well-calibrated from just looking at this plot alone. The away plot also shows Bet365 to be well-calibrated here due to no observed counts being outside of the expected ranges.

### Per outcome data

| | Mean error | Observed - expected | χ² | bins | p |
|---|---|---|---|---|---|
| Home wins | +0.367% | +33.5 | 25.24 | 17 | **0.066** |
| Draws | +0.232% | +21.2 | 2.92 | 6 | 0.712 |
| Away wins | −0.600% | −54.7 | 9.94 | 17 | 0.870 |

No outcome is significantly mis-calibrated at the 5% level. There was some bin combining here in order to ensure the chi-squared values were accurate and calculated on bins that followed the correct assumptions. The p-values here continue the same story described above that it appears that the home results are likely mis-calibrated but the p-value is not significant here. It is still much lower though showing that the observations still have a low probability of happening by purely by chance. 

Mean error across all outcomes combined is ~10⁻¹⁴, which is an identity rather than a result — the three probabilities sum to 1 and exactly one outcome occurs — so it serves as a check on the margin removal.

### Brier Scores

| | Base rate | Brier | Brier, base rate only | Skill |
|---|---|---|---|---|
| Home wins | 45.6% | 0.2113 | 0.2481 | **14.8%** |
| Draws | 24.7% | 0.1831 | 0.1858 | **1.4%** |
| Away wins | 29.7% | 0.1773 | 0.2088 | **15.1%** |

The Brier score here essentially measures the accuracy of the predictions made by the bookmaker, ie, do events with an implied probability of X% occur X% of the time? A Brier score of 0 represents perfect accuracy with no difference between predicted/expected and actual/observed results. Then a Brier score of 1 represents fully inaccurate predictions, ie, all predictions are wrong. A baseline is required though to compare against to give it 'context'. Why it is required is because if an event happens say 10% of the time and a model that simply guesses 10% for every single prediction would get quite a low Brier score but purely by chance rather than predictive ability. So the base rate represents the historical rate of occurrence of each result. A Brier score lower than the base rate score represents the models Bet365 to be using to have a predictive edge and therefore resemble a form of calibration. For home and away wins we can see that the base rate differs quite a lot from the actual Brier score, showing the models Bet365 use to set odds does have predictive power but for draws the difference is only 1.4%. I believe this is because of the nature of draws that I have mentioned before whereby it is not a desired result by either team in most cases and it is never usually aimed for over a win before a fixture commences, making them inherently harder to predict and forecast. This also changes the image of how well-calibrated draws actually are as the 2 plots above show them to be well-calibrated but looking at the Brier score now this may just be due to chance.

### Monthly
I then proceeded to assess how calibration changes month-to-month as this gives an idea of how calibration changes throughout a season on average. Fixtures that were played in June and July were excluded though due to the fact that there were very few fixtures played in these months and all were during the 19/20 season due to the pandemic. A monthly view shows seasonal calibration and if there are variations worth noting.
The monthly calibration plots and observed vs. expected plots are very noisy due to the small sample sizes so I will not include them here but I will look at the errors associated with each month, specifically the mean error and the root mean squared error.

<p align="center">
  <img src="images/monthly-error-home.png" width="32%" alt="Monthly error, home wins">
  <img src="images/monthly-error-draw.png" width="32%" alt="Monthly error, draws">
  <img src="images/monthly-error-away.png" width="32%" alt="Monthly error, away wins">
</p>

Mean error is near zero in every month except March, where home wins and draws move in opposite directions and roughly mirror one another. Other than that there is not much else of interest from these plots except for the spike in March for home results and the dip in March for draws. The positive home ME for March shows home results to have occurred more often than predicted and draws being negative show them to occur less than predicted so across the dataset these plots show that there is dependence on the number of results in each category which makes sense of course and it is unclear exactly why there were more home wins and less draws than expected specifically in March by a noticeable amount in comparison to the other months.

Assessing the RMSE across all results we see that the lines are pretty much flat except for a very small spike in November for home results and a small dip in October for away results. But, there is a noticeable trough in the RMSE for March for draws which represents the overall errors for predictions made in March for draws is smaller than for any other month. This does not correlate with the mean error simply due to the same shape in the lines but could be evidence of better accuracy in predictions for the month of March. The only issue with this observation is that the sample size is not exactly large: there were 150 draws observed for the month of March across the whole dataset, which excluding June and July was the lowest out of any month, so it is quite possible this is due to noise looking at it from this angle. There was a ~47 amount difference between expected and observed events (47 less than expected) for March combined with he p-value being less than 5% and the effect size being ~0.19 this could reveal that maybe it is not simply due to sample size that this observation appeared. There is sufficient evidence to reject the null hypothesis and means here the observed counts are not consistent with the expected counts. The effect size is quite small though so this finding is significant but not anything major but it is very strong evidence for miscalibration for the month of March for draws. Exactly why is very difficult to determine and would require more granular data and looking at the models used for the pricing.

| | p | Bonferroni-adjusted |
|---|---|---|
| **March, draws** | **0.00046** | **0.0046** |
| March, home wins | 0.032 | 0.32 |
| May, draws | 0.032 | 0.32 |

So firstly the 3 results picked here were the ones with the smallest p-values that have potentially significant results. There was no other reason for picking these here.
A Bonferroni adjustment is used to reduce the chance of getting a statistically significant result purely by chance when performing multiple hypothesis tests like I did for the months (10 tests in total per result). $\alpha$ is 5% here, meaning I am accepting a 5% probability of a Type I error occurring but as I did 10 tests per result the probability of getting at least one false positive somewhere is **greater** than 5%. With independent tests this means that there is just over a 40% chance of getting at least 1 false positive out of the 10 tests for each result which is far too high $1 - (1-\alpha)^{10}$. This means that even if there was no genuine effect(s) there will more likely be something that appears significant just by chance. There are 2 methods of using the Bonferroni adjustment. One can either divide the significance level by the number of tests, producing $\alpha_{m} = \frac{\alpha}{10}$ or one can multiply the existing p-values by the number of tests using the formula $p_{adjusted} = min(mp,1)$. The methods are identical and is just a matter of preference but I decided to multiply the p-values by the number of tests so the significance level is 5% still. After applying the correction we see that 2 of the 3 results show insufficient evidence to reject null hypothesis but the draws result in March shows sufficient evidence to reject the null hypothesis, suggesting that the observed counts for draws in March are inconsistent with the expected counts. The actual difference is 47 events (rounded) where there was 197 expected draws but only 150 observed and this had an effect size of 0.19 so a pretty small effect size which shows quite a small deviation.

This is not a conclusive answer of seasonality of Bet365's implied probability setting but the result for draws in March is an indicator that there could be seasonality.

### Would any of it have made money?

This is an extra part of the analysis that aims to demonstrate if there was potential money-making opportunities for a bettor using a flat-staking strategy of £10 per bet. The reason for a flat-staking strategy is that if this is profitable it highlights mis-calibration far more than a more technical/discretionary strategy due to the fact these require much more detailed analyses beyond the odds and implied probabilities. Below show the key statistics for each result and the theoretical returns:

| | Total return | Mean ROI per season | Best season | Worst season |
|---|---|---|---|---|
| Home wins | −£2,693.59 | −2.95% | +9.48% | −18.41% |
| Draws | −£6,378.95 | −6.99% | +7.69% | −28.21% |
| Away wins | −£9,000.39 | −9.87% | +28.62% | −31.42% |

Unsurprisingly all lose in the long-term just flat-staking on each result every game. There are several seasons whereby betting on 1 result for a whole season would have been profitable and this is most likely what would have happened with a flat-staking of £10 per game as this is a small enough amount to go 'under the radar' and not affect the odds set. The best season for away results is an anomalous result due to the fact this occurred during the covid season whereby there were no fans allowed in the stadium, meaning that for an away team it was essentially the exact same as playing at home so the home advantage almost evaporated for this season and no other season had this so it is definitely an outlier and cannot be interpreted the same as other results. I believe the main reason for the profitability especially for home results is due to the overround they incorporate in their odds. I would need to investigate this further but I think definitely for the home results if the implied probabilities were used as odds the mean ROI would be much closer to breaking even.

## Limitations

- **These are pre-match odds, not closing odds.** football-data.co.uk publishes Bet365's closing line separately (`B365CH`/`CD`/`CA`), but only for 2,660 of the 9,120 fixtures, beginning around 2019/20. The closing line is the final price of an event before it starts but the odds used are opening odds which are take less data into account as there is not as much money in the market at market open. So this is more reflective actually of what Bet365 anticipate and reflects their implied probability setting capability better than closing odds but in terms of profitability using the opening odds is not the most accurate as it is rare that a bettor would place bets at these prices for every game.
- **Matches are treated as independent.** Each match is treated as independent and this is loosely true but definitely not entirely true. A team's prior result influences how a team approach their next game and other teams may alter their playstyle/tactics depending on other team's results and their own results but using the data here this is very difficult to capture unless I assume that the odds contain all of this information. The chi-squared test assumes independence, and the p-values should be interpreted with this in mind.
- **External Factors are not considered.** Elements such as injuries, manager changes, lineup changes, form, fixture congestion and other factors are not accounted for at all and it is assumed that the implied probabilities/odds account for all of this information which is a naive assumption here as these are opening odds and between the market opening and the match starting a lot of these factors can change.
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
