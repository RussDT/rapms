# Historical Core Weighted Factors 1997-2026 ALL

Run command:

```bash
python nba_pipeline/scripts/run_historical_core_weighted_factors.py 97 26 ALL \
  --rubberband \
  --fixed-season-effects \
  --age-poly-coefficients nba_pipeline/results/rapm_14_26_all_rb_agefe/age_curve_poly3_coefficients.csv \
  --cores 14
```

Output directory:

```text
nba_pipeline/results/historical_core_97_26_all_rb_fse_agepoly3/
```

Published weighted-factor file:

```text
master_results/weighted_factors_core_97_26_all_rb_fse_agepoly3.csv
```

Downstream `rapms` commit:

```text
00246c9 Add historical core weighted factors 1997-2026
```

## Method

- Seasons: 1997 through 2026.
- Season types: ALL, meaning regular season plus playoffs where present.
- Missing 2026 playoff processed parquets were skipped because they were not present for this run.
- Core metric solves: RAPM, TS, TOV, REB, BADPASS_TOV, SCORING_TOV.
- Rubberband: fitted score-margin-by-period controls.
- Season effects: fixed constants, not fitted dummies.
- Age effects: fixed polynomial constants, not fitted dummies.
- Matrix check: metric solves used `0 season + 0 age` fitted columns; the season and age adjustments were subtracted from `y` before centering and solving.

Fixed season effect definition:

```text
season_offset = raw RS target mean for that metric and season
```

The same RS value is used for both RS and PS rows from that season.

Fixed age polynomial definition:

```text
age_offset = curve(player_age_in_season) - curve(27)
```

The lineup age offsets are summed with RAPM offense/defense signs and subtracted from the target before solving. Player results are therefore age-27 normalized.

## Final Weighted File

- Rows: 2,884 players.
- Columns: 28.
- Possession-weighted ORAPM mean in source RAPM result: approximately zero.
- Published defensive factor columns are flipped so positive defense is good.
- FT columns are intentionally blank in this historical core run.

Top 10 by `net_rapm`:

| Rank | Player | Net | Off | Def | Possessions |
|---:|---|---:|---:|---:|---:|
| 1 | Nikola Jokic | 9.00 | 6.43 | 2.57 | 117,678 |
| 2 | LeBron James | 8.89 | 6.33 | 2.56 | 279,013 |
| 3 | John Stockton | 8.55 | 5.12 | 3.43 | 66,976 |
| 4 | Victor Wembanyama | 8.46 | 1.67 | 6.78 | 22,883 |
| 5 | Kevin Garnett | 8.22 | 1.52 | 6.70 | 197,224 |
| 6 | Shaquille O'Neal | 7.85 | 4.90 | 2.95 | 140,504 |
| 7 | Tim Duncan | 7.77 | 2.10 | 5.67 | 208,873 |
| 8 | Michael Jordan | 7.45 | 5.16 | 2.29 | 47,202 |
| 9 | Joel Embiid | 7.32 | 2.73 | 4.59 | 71,236 |
| 10 | Chris Paul | 7.12 | 5.24 | 1.88 | 196,575 |

## Weighted-Factor Fit

Possession-weighted regressions of RAPM outputs on the six core factors:

```text
net_rapm ~ off_ts + def_ts + off_tov + def_tov + off_reb + def_reb
off_rapm ~ off_ts + def_ts + off_tov + def_tov + off_reb + def_reb
def_rapm ~ off_ts + def_ts + off_tov + def_tov + off_reb + def_reb
```

Fit quality:

| Target | R squared | Adjusted R squared |
|---|---:|---:|
| Net | 0.984682 | 0.984650 |
| Offense | 0.974025 | 0.973970 |
| Defense | 0.968931 | 0.968866 |

Net factor coefficients:

| Factor | Coefficient |
|---|---:|
| Intercept | -0.0007 |
| off_ts | 0.8556 |
| def_ts | -0.8581 |
| off_tov | 1.1407 |
| def_tov | -1.1503 |
| off_reb | 0.7894 |
| def_reb | -0.7877 |

Published weighted residual summary:

| Statistic | Value |
|---|---:|
| Possession-weighted mean | -0.0008 |
| Possession-weighted RMSE | 0.3406 |
| Possession-weighted MAE | 0.2664 |

TOV decomposition:

```text
off_tov ~ off_badpass_tov + off_scoring_tov
def_tov ~ def_badpass_tov + def_scoring_tov
```

| Side | R squared |
|---|---:|
| Offense | 0.999851 |
| Defense | 0.999846 |

## Main Artifacts

```text
weighted_factors_core_97_26_all_rb_fse_agepoly3.csv
fixed_rs_season_baselines.csv
regression_net_coefficients.csv
regression_off_coefficients.csv
regression_def_coefficients.csv
tov_decomposition_coefficients.csv
rapm_97_26_all_pure_rb_fse_agepoly3_results.csv
ts_97_26_all_rb_fse_agepoly3_results.csv
tov_97_26_all_rb_fse_agepoly3_results.csv
reb_97_26_all_rb_fse_agepoly3_results.csv
badpass_tov_97_26_all_rb_fse_agepoly3_results.csv
scoring_tov_97_26_all_rb_fse_agepoly3_results.csv
*_fixed_offsets.csv
*_rubberband_effects.csv
```
