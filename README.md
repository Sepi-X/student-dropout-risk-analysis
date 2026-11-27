# Student Dropout Risk Analysis

Final project for the **Foundations of Computer Science** course, MSc in Data Science, University of Milano-Bicocca (2026).

The dataset has 100,000 university students and 34 columns (demographic, economic, academic and behavioural data). The column `dropout` is 1 if the student dropped out, 0 otherwise.

## What the notebook does

Everything is in one notebook: `CS-project.ipynb`. No loops over rows are used.

| # | Task | Main pandas tools |
|---|------|-------------------|
| 1 | At-risk students: online, attendance < 50, more than 5 missed deadlines | Boolean indexing |
| 2 | Number of at-risk students and dropout rate per course type | `groupby`, `agg`, `sort_values` |
| 3 | New columns `pass_rate`, `gpa_change`, `gpa_trend` | column arithmetic, `np.select` |
| 4 | Check that `units_passed + units_failed == units_enrolled` | Boolean mask |
| 5 | Dropout rate per GPA trend, and the 10 largest GPA drops | `groupby`, `sort_values` |
| 6 | Groups by weekly work hours (0, 1-10, 11-20, 21-30, more than 30) | `np.select` |
| 7 | Risk score (0-8) and dropout rate for each score | comparisons, `groupby`, `agg` |
| 8 | GDP per capita from Our World in Data: remove aggregates, keep latest year per country | `read_csv` from URL, `isin`, `drop_duplicates` |
| 9 | Join GDP data with students, dropout rate per continent | `merge`, `groupby` |

## Main results

- **Task 1-2:** 197 at-risk students. Their dropout rate goes from 65% to 90% depending on the course.
- **Task 4:** no row breaks the rule `passed + failed = enrolled`.
- **Task 5:** dropout is 26.5% for "declining", 39.1% for "stable" and 58.9% for "improving".
- **Task 7:** dropout goes up with the risk score, from 15.3% at score 0 to 90% at score 8.
- **Task 9:** dropout is about 32% on every continent. Oceania has the fewest students (969), North America the most (38,095).

## Notes

- **Task 8:** the aggregates (World, income groups, World Bank regions, EU) are removed with a list. A check on `owid_region` shows that no aggregate is left. 199 countries remain.
- **Task 9:** the students data uses `USA` and `UK`, the GDP data uses `United States` and `United Kingdom`, so I renamed them in the students data before the join.

## How to run

1. Install [Anaconda](https://www.anaconda.com/download) (it includes Python, pandas, NumPy and Jupyter).
2. Download this repository. The dataset `student_dropout_risk_2026.csv.gz` is in the same folder as the notebook.
3. Open Jupyter Notebook, open `CS-project.ipynb` and run **Kernel → Restart & Run All**.

Task 8 downloads the GDP file from the internet, so you need an internet connection.

## Author

Sepehr Mojdehdehbaher - [GitHub](https://github.com/Sepi-X)
