# oEFG Zone Value Decomposition, 24-26 ALL RB SE a2000/4000

Source: `/Users/russellthomas/Docs/rapms/master_results/weighted_factors_alt3_24_26_all_rb_se_a2000_4000.csv`
Baselines: `/Users/russellthomas/Docs/pbp_rapm/nba_pipeline/results/processed_rs_parquet_averages_per100_97_26.csv` using unweighted 2024-2026 RS means from `processed_rs_parquet_averages_per100_97_26.csv`.

## Baselines

- Rim frequency: 32.813861%
- Rim FG: 62.673770%
- Midrange frequency: 26.184459%
- Midrange FG: 44.660549%
- 3P frequency: 41.036282%
- 3P FG: 36.186925%
- Implied eFG: 54.534459%

## Method

Midrange frequency is treated as the omitted category, so rim and 3P mix values are the value of shifting attempts away from midrange. Native values are the direct formula values. Calibrated values apply no-intercept regression coefficients from the five native subcomponents to better match published `ALT_EFG`. Closed values allocate the remaining residual across rim/mid/3P in proportion to absolute zone value, forcing the three closed zones to sum to `ALT_EFG`.

## Fit

- off native: R2=0.5069, MAE=0.7080, RMSE=0.9207, p95 abs residual=1.8041, max abs residual=3.7220.
- off calibrated: R2=0.8631, MAE=0.3782, RMSE=0.4851, p95 abs residual=0.9710, max abs residual=1.8081.
- def native: R2=0.4828, MAE=0.5008, RMSE=0.6446, p95 abs residual=1.2589, max abs residual=2.4798.
- def calibrated: R2=0.8668, MAE=0.2461, RMSE=0.3271, p95 abs residual=0.6733, max abs residual=1.3711.

## Calibration Coefficients

- off rim_mix: 2.041207
- off rim_fg: 2.273082
- off mid_fg: 2.781501
- off three_mix: 2.163427
- off three_fg: 2.463282
- def rim_mix: 1.879951
- def rim_fg: 2.872975
- def mid_fg: 3.779727
- def three_mix: 1.660246
- def three_fg: 2.813604

## Caveat

This is a post-hoc conversion from rounded published RAPM columns, not a true row-level additive solve. The calibrated-closed columns are the most usable zero-residual display approximation. A true zero-residual method should build these zone-value targets directly on the ALT_EFG/FIRST_CHANCE row universe and run the same ridge setup on those targets.
