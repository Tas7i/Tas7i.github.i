# Motorcycle Crashes in Eastern Province: What Is Associated with Fatal Outcomes?

An exploratory analysis and interactive dashboard of 520 recorded motorcycle crashes in Saudi Arabia's Eastern Province (2023–2025).

**Live dashboard:** [tas7i.github.io/Tas7i.github.i](https://tas7i.github.io/Tas7i.github.i/)

## Question

Which factors are associated with a recorded motorcycle crash being **fatal** rather than injury-only?

## Key finding

Crashes recorded in **Al-Ahsa** were fatal far more often than in the rest of the region.

| Group | Crashes | Fatal | Share fatal | 95% CI |
|---|---|---|---|---|
| Al-Ahsa | 78 | 30 | 38.5% | 28.4–49.6% |
| Rest of region | 442 | 46 | 10.4% | 7.9–13.6% |

- Fisher's exact test: p < 0.001.
- The gap appears in each of the three years.
- Logistic regression adjusting for year, night-time and crash type gives an odds ratio of **5.5** (95% CI 3.2–9.6). Odds ratios are not the same as risk ratios; the raw share is about 3.7× higher.
- Night-time (OR 1.4) and crash type (OR 0.7) were not statistically distinguishable from no effect, and there was no clear trend across years.

## Data

| File | Rows | Content |
|---|---|---|
| Accidents | 520 | Year, month, hour, cause, type, city, severity, coordinates |
| Parties | 1,004 | People involved: age, sex, condition, nationality, role |
| Vehicles / Vehicles2 | 912 / 942 | Vehicles involved (Vehicles2 adds model and make) |

Source: Eastern Province Traffic Safety Committee (لجنة السلامة المرورية بالمنطقة الشرقية). The original files are not published in this repository. They contain personal fields, so only an anonymized extract is embedded in the dashboard.

## Method

1. **Join** the four files by crash ID. All party and vehicle IDs match the crash table.
2. **Choose the vehicle file.** Vehicles2 matched the driver count in 23 of 31 crashes where the two files differed, versus 4 for Vehicles, so Vehicles2 was used. Two crashes missing from Vehicles2 were filled from Vehicles. Seven crashes whose counts stay inconsistent were flagged and kept.
3. **Clean parties.** Age 0 (14 rows) was treated as unknown, along with 8 missing ages; blank categories became "unknown".
4. **Fix coordinates.** The columns named X and Y hold latitude and longitude respectively and were renamed.
5. **Validate.** The 76 fatal crashes match between the crash and party tables with zero mismatches.
6. **Analyze.** Fatality shares by city, cause, type and hour (groups under 15 crashes excluded), Fisher's exact test, Wilson confidence intervals, and a logistic regression with four predictors (only 76 fatal events, so the model is kept small).
7. **Visualize.** A single-page dashboard with filters, charts and a map.

## Limitations

- The data contains only crashes with an injury or death, so results are shares of **recorded** crashes, not risk per rider or per kilometre.
- "More fatal" does not mean "more dangerous". Differences in how minor crashes are recorded between cities could contribute to the Al-Ahsa gap, and this data cannot settle it.
- Small groups (for example, 18 Al-Ahsa crashes in 2023) give unstable percentages.
- Large "other" categories in cause, make and model limit detail.
- This is an association study. It does not establish causes.

## Tech

Python (pandas, SciPy, statsmodels) for analysis; HTML, CSS and JavaScript with [Leaflet](https://leafletjs.com/) for the dashboard; map data © [OpenStreetMap](https://www.openstreetmap.org/copyright) contributors.

## Repository structure

```
├── index.html          # the dashboard (data embedded)
├── analysis.ipynb      # cleaning and analysis notebook
└── README.md
```

## Author

Tamim Alsuhibani: [GitHub](https://github.com/Tas7i) · [LinkedIn](https://linkedin.com/in/tamim-alsuhibani-10b53a2b1)
