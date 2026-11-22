# Student Dropout Risk Analysis

Final project for the **Foundations of Computer Science** course, **MSc in Data Science, University of Milano-Bicocca** (2026). Written in Python with pandas and NumPy.

The project studies a dataset of **100,000 university students** (34 columns: demographic, economic, academic and behavioural data). The goal is to understand which factors are linked to **dropout** (`dropout = 1`) and to build a simple risk score.

Overall, **32.0%** of the students in the dataset dropped out.

## What the notebook does

Everything is in one notebook: [`foundations_cs_project.ipynb`](foundations_cs_project.ipynb). All steps use vectorised pandas operations (no loops over rows).

| # | Task | Main pandas tools |
|---|------|-------------------|
| 1 | Select "at-risk" students: online, attendance < 50%, more than 5 missed deadlines | Boolean indexing |
| 2 | Number of at-risk students and dropout rate per course type | `groupby`, `agg`, `sort_values` |
| 3 | New columns `pass_rate`, `gpa_change`, `gpa_trend` | vectorised arithmetic, `np.where` |
| 4 | Data check: `units_passed + units_failed == units_enrolled` | Boolean mask |
| 5 | Dropout rate by GPA trend; the 10 largest GPA drops | `groupby`, `sort_values` |
| 6 | Groups by weekly work hours (0, 1-10, 11-20, 21-30, more than 30) | `pd.cut` |
| 7 | Manual risk score (0-8) and dropout rate for each score | Boolean masks, `agg` |
| 8 | Load GDP per capita from Our World in Data, keep countries only, latest year | `read_csv` from URL, `notna`, `drop_duplicates` |
| 9 | Join GDP/continent data with students; dropout rate per continent | `merge`, `groupby` |

## Main results

- **At-risk students (task 1):** 197 students. Their dropout rate is high in every course (from 65% to 90%), but each course has only 29-47 of them, so the differences between courses are not reliable.
- **Data quality (task 4):** no row breaks the rule `passed + failed = enrolled`.
- **GPA trend (task 5):** dropout is 26.5% for "declining", 39.1% for "stable" and 58.9% for "improving". The "improving" group is small (about 700 students), so GPA change alone is a weak signal.
- **Risk score (task 7):** the dropout rate rises with every point, from 15.3% at score 0 to 90% at score 8. The weights are chosen by hand, not learned from data, so this is only a rough indicator.
- **Continents (task 9):** dropout is about 32% everywhere (31.7%-32.3%). Continent does not explain dropout in this dataset.

## Data-quality details handled in the notebook

- **Floating-point comparison:** GPAs have two decimals, but subtracting floats can give values like `-0.5000000000000004`. Without `round(2)`, the strict comparison with `±0.5` put 76 students in the wrong `gpa_trend` group.
- **Ties:** the 10th largest GPA drop is shared by 4 students. The top-10 list breaks the tie by `person_id`, so the result is reproducible.
- **Join check:** after the join with the GDP data, the notebook checks that no student is duplicated and that every nationality got a continent (`UK` and `USA` are renamed to match Our World in Data).

## How to run

The project was developed with **Anaconda** and **Jupyter Notebook**. Anaconda already includes Python, pandas, NumPy and Jupyter, so there is nothing else to install.

1. Install [Anaconda](https://www.anaconda.com/download) if you do not have it.
2. Download the dataset `student_dropout_risk_2026.csv.gz` from the course website and put it in the same folder as the notebook (no need to unzip it, pandas reads `.gz` files). The data file is **not** included in this repository.
3. Open **Anaconda Navigator** and launch **Jupyter Notebook**. Alternatively, open the **Anaconda Prompt**, go to the project folder and run:
   ```bash
   jupyter notebook
   ```
4. Open `foundations_cs_project.ipynb` and run everything with **Kernel -> Restart & Run All**.

If your data file has a different name or location, change the path in the first `pd.read_csv(...)` cell.

Task 8 downloads the GDP file from [Our World in Data](https://ourworldindata.org/grapher/gdp-per-capita-worldbank.csv?v=1&csvType=full&useColumnShortNames=true), so it needs an internet connection. The request sends a browser-like `User-Agent`, because the site refuses pandas' default one.

## Tech

Python 3 (Anaconda), pandas, NumPy, Jupyter Notebook.

## Author

Sepehr Mojdehdehbaher - [GitHub](https://github.com/Sepi-X)
