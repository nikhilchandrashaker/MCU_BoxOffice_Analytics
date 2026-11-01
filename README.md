# MCU analytics

A small dataset and dashboard covering Marvel Cinematic Universe films: worldwide box office, production budgets and critic scores, grouped by phase and saga.

Last updated: November 1, 2026.

## What's in the project

| File | What it is |
|---|---|
| `mcu_films.csv` | Plain dataset, one row per film. Use this for code, pandas or R. |
| `mcu_dataset.xlsx` | Same data in Marvel red, black and gold, with calculated columns and a phase summary sheet. |
| `mcu-analytics.html` | Interactive dashboard with phase filters, charts and a sortable table. Published as an artifact. |
| `README.md` | This file. |

## Coverage

38 theatrical films from *Iron Man* (2008) to *Spider-Man: Brand New Day* (2026).

- Infinity Saga: Phases 1 to 3 (2008 to 2019)
- Multiverse Saga: Phases 4 to 6 (2021 onward)
- Not included: Disney+ series, Marvel One-Shots, and films not yet released, including *Avengers: Doomsday*.

## Columns

### `mcu_films.csv`

| Column | Type | Description |
|---|---|---|
| `title` | text | Film title |
| `release_year` | integer | Year of first theatrical release |
| `phase` | integer, 1 to 6 | MCU phase |
| `saga` | text | `Infinity` (phases 1 to 3) or `Multiverse` (phases 4 to 6) |
| `worldwide_gross_usd_m` | number | Worldwide box office, US$ millions, rounded |
| `budget_usd_m` | number | Reported production budget, US$ millions, rounded. Excludes marketing. |
| `critic_score_pct` | integer | Rotten Tomatoes critics' score (Tomatometer), percent |

### Added in `mcu_dataset.xlsx`

| Column | Formula |
|---|---|
| Return (x) | Worldwide gross divided by budget |
| Gross minus budget ($M) | Worldwide gross minus budget |

The **Phase Summary** sheet uses formulas to calculate film count, total and average gross, average critic score, average return and best critic score for each phase. It updates when you edit the **MCU Films** sheet.

## Using it

```python
import pandas as pd

df = pd.read_csv("mcu_films.csv")
df["return_x"] = df["worldwide_gross_usd_m"] / df["budget_usd_m"]
print(df.groupby("phase")[["worldwide_gross_usd_m", "critic_score_pct", "return_x"]].mean().round(1))
```

## Data quality and caveats

- **Approximate figures.** Gross, budget and score values are rounded. Most were entered from memory and have not been checked against a source film by film. Verify before citing.
- **Checked in the latest update:** the *Fantastic Four: First Steps* worldwide gross ($520M, per The Numbers) and the details for *Spider-Man: Brand New Day*.
- ***Brand New Day* is still changing.** The film was in release at the time of the last update. Variety-level coverage confirmed $2.022B on August 17, 2026, and a box-office tracker cited roughly $2.5B in early October. The dataset uses 2,500 as an estimate, so replace it when a final total is published.
- **Budgets are production budgets.** They leave out marketing, so "return" is a simple gross-to-budget multiple, not profit. Theaters keep a share of ticket sales, so the multiple overstates how much the studio earns.
- **Gross figures are not adjusted for inflation or re-releases.**
- **Critic scores drift.** Rotten Tomatoes percentages change as reviews are added.
- **Phase assignment** follows Marvel Studios' official groupings. *Thunderbolts\** closes Phase 5 and *The Fantastic Four: First Steps* opens Phase 6.

## Suggested checks and extensions

- Replace the approximate values with figures from Box Office Mojo or The Numbers.
- Add directors, lead characters, opening weekend, domestic and international splits, and CinemaScore.
- Add the Disney+ series for a combined view.
- Update the dashboard to include *Brand New Day* (it currently shows the original 37 films).

## Notes on sources

Film data are facts about public releases. The 2026 figures came from trade and news coverage (Variety, Deadline, Screen Daily, Box Office Mojo and The Numbers as cited in those reports). Marvel, the MCU and all film titles belong to their respective owners, and this project is unofficial and not affiliated with Marvel Studios or Disney.
