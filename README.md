# IPL Data Analysis — Cricket Performance Analytics

An end-to-end data analytics project analysing a decade of Indian Premier League cricket — from raw ball-by-ball data to an interactive Power BI dashboard.

**Stack:** Power BI Desktop · Power Query · DAX

---

## Overview

The Indian Premier League generates an enormous volume of data every season — behind every match sits a record of each individual ball bowled. This project covers the full analytics lifecycle: cleaning two raw datasets, modelling the relationship between them, writing analytical DAX measures, and surfacing insights in a three-page Power BI dashboard.

| Metric | Value |
|-|-|
| Seasons Analysed | 10 (2008–2017) |
| Total Matches | 636 |
| Total Deliveries | 150,460 |
| Total Runs Scored | 194,314 |
| Total Wickets | 7,438 |
| Boundaries (4s / 6s) | 17,033 / 6,523 |

---

## Data Model

| Table | Type | Rows | Description |
|-|-|-|-|
| `deliveries` | Fact | 150,460 | One row per ball — batsman, bowler, runs, extras, dismissal |
| `matches` | Dimension | 636 | One row per match — teams, toss, result, venue |
| `Teams` | Dimension | 14 | Derived team master, drives a single cross-page team filter |
| `Measures` | Calculation | — | Dedicated table holding all DAX measures |

The model is built on a one-to-many relationship (`matches[id]` → `deliveries[match_id]`), so a filter applied at match level cascades instantly to every ball-level visual.

---

## Dashboard

A three-page interactive Power BI report, each page with its own theme and focus.

### Teams, Toss & Venues

The strategic view — team success, toss behaviour, and venue effects. Tracks total wins
by franchise, tosses won and converted, the overall bat-versus-field decision split,
matches hosted by city, and the venue where the leading team wins most often.

### Batting Performance

A deep dive into who scores and how. Compares the highest and lowest run scorers,
separates boundary-hitting into fours and sixes at both individual and team level,
and applies a minimum-balls threshold so low-scorer rankings reflect genuine
performance rather than small sample sizes.

### Bowling & Fielding

Measures the bowling attack and the work behind the stumps. Combines leading
wicket-takers, a full statistical breakdown of the top five bowlers, the bowlers who
never took a wicket, and a fielding leaderboard built from catches, run-outs, and
stumpings.

---

## Key Analytical Insights

Each insight pairs a finding in the data with what it actually means.

**Winning the toss is close to worthless.**
Across 636 matches, the toss winner went on to win the match only 51.1% of the time — statistically indistinguishable from a coin flip, despite the toss being treated as a decisive moment in broadcast coverage. *Implication: pre-match narratives built around the toss have almost no predictive value.*

**Teams overwhelmingly choose to field first, and the data supports them.**
Captains elected to field in 363 of 636 tosses versus 273 to bat, and chasing sides won 54.2% of all decided matches. *Implication: the preference for chasing is an evidence-based strategy, not superstition — knowing the target is a measurable advantage.*

**Sustained consistency, not peak seasons, defines the top teams.**
Mumbai Indians lead on volume with 92 wins, but Chennai Super Kings convert at a higher rate — roughly 60% versus Mumbai's 58.6% from fewer matches. *Implication: ranking by total wins alone favours longevity; win rate is the fairer measure of team quality.*

**Run totals reward longevity, boundary counts reveal style.**
Suresh Raina tops the run charts with 4,548, but the boundary split tells a different story — Chris Gayle dominates sixes with 266 while Gautam Gambhir leads fours with 484. *Implication: aggregate runs mask playing style; separating fours from sixes exposes power hitters versus accumulators.*

**Elite T20 bowling means wickets and economy together.**
Lasith Malinga took the most wickets (154) while conceding just 6.88 runs per over — most bowlers excel at one dimension or the other, and roughly 46 bowlers in the dataset bowled without ever taking a wicket. *Implication: single-metric bowling rankings are misleading; attack and control must be assessed jointly.*

**Fielding statistics are structurally dominated by wicket-keepers.**
Dinesh Karthik (127 dismissals) and MS Dhoni (126) top the fielding table, driven by catches and stumpings that pass through the keeper by definition. *Implication: raw dismissal counts overstate keeper impact — outfield contribution needs to be measured separately.*

---

## Data Preparation

All cleaning was performed in Power Query, leaving the source files untouched and the transformation steps fully repeatable.

| Issue | Resolution |
|-|-|
| Inconsistent franchise naming | Merged "Rising Pune Supergiants" into "Rising Pune Supergiant" across all six team columns |
| Matches with no winner | Three tied or abandoned matches labelled "No Result" to prevent miscounting wins |
| Missing city values | Seven blank cities filled from their venue (all Dubai) |
| Duplicate venue names | Two variants of the Mohali stadium merged into a single entry |
| Incorrect data types | Season and win-margin columns cast to whole numbers, date cast to date |
| Sparse dismissal columns | Blanks deliberately retained — a blank correctly represents "no wicket on this ball" (~95% of deliveries) |

---

## Power BI

* Three-page report with consistent theming, custom page navigation, and synced slicers (Season, Team, Player)
* 25+ DAX measures spanning team performance, batting, bowling, fielding, toss, and venue analysis
* Measures encode real cricket scoring rules — run-outs excluded from bowler wickets, wides and no-balls excluded from legal deliveries, byes and leg-byes not charged to the bowler
* KPI cards, ranked bar charts, donut charts, geographic map, and detail tables with Top-N and minimum-threshold filters

**Techniques demonstrated:** `CALCULATE` with multi-condition filters, `SELECTEDVALUE` with `REMOVEFILTERS` for context-aware team measures, `AVERAGEX` iterators, `DIVIDE` with safe fallbacks, and visual-level threshold filtering for fair rankings.

---

## Project Structure

```
IPL-Data-Analysis/
├── README.md
├── dashboard/
│   ├── IPL_Dashboard.pbix
│   └── screenshots/
│       ├── 01_teams_toss_venues.png
│       ├── 02_batting.png
│       └── 03_bowling_fielding.png
├── report/
│   └── IPL_Project_Report.pdf
└── data/
    ├── matches.xlsx
    └── deliveries.xlsx
```

---

## Notes

The source files in `data/` are raw and uncleaned by design — all transformations live inside the `.pbix` and re-run automatically on load. When opening the dashboard, update the data source path to your local `data/` folder if prompted. All figures quoted in this README were verified directly against the dataset.

---

**Kunal Shrivastava** — Data Analyst Intern
