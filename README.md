# Teresa Rivera | SQL Portfolio

SQL coursework demonstrating how I translate questions into filters, joins, summaries, and reporting logic.

My professional background includes client and leadership reporting, Excel and Power Query automation, and combining multiple data sources to support decisions. This repository documents my SQL learning through **Break Into Tech** exercises.

[Tableau dashboards](https://public.tableau.com/app/profile/teresa.rivera2171/vizzes) · [GitHub profile](https://github.com/TERivera21)

## Start here

| Example | Question or focus | Skills demonstrated |
| --- | --- | --- |
| [Economics curriculum and textbook authors](Multiple%20JOINS%20to%20return%20requested%20data%20on%20Authors) | Which textbooks and authors support currently taught economics courses? | Multiple joins, bridge tables, filtering, preserving missing author matches |
| [Department salary comparisons](Comparing%20Salaries%20for%20Various%20Departments) | Which departments exceed spending and average-salary thresholds? | `GROUP BY`, `SUM`, `AVG`, `HAVING` |
| [Prescription and medication joins](Pharmaceutical%20JOINs) | Which medications were prescribed by a doctor, and what fill dates are recorded for a selected medication? | `INNER JOIN`, `DISTINCT`, filtering |

These are training examples. Results apply to the supplied sample data, and do not represent employer, patient, or current company data.

## More exercises

| Example | Focus | Review note |
| --- | --- | --- |
| [Customer and order analytics](Customer%20and%20Order%20Analytics) | Revenue, customer accounts, orders, and product quantities | Metric definitions and account-level aggregation need refinement; source tables are not included |
| [Shipping customer joins](JOINS%20for%20Shipping%20Company%20Data) | Compare all customers with those who have shipping history | Standard-only eligibility needs a history-wide exclusion |
| [Fortune company sample analysis](Fortune%20500%20Analysis) | Benefits, company-size categories, and industry averages | Ranking, prompt alignment, and grouped output need refinement; sample is not an authoritative Fortune 500 dataset |
| [Marathon completion categories](CASE%20and%20ROUND%20examples%20in%20a%20Marathon) | Convert completion fractions to percentages and categories | The 25% boundary needs an inclusive comparison |
| [Bank product filters](BankProducts) | Find savings products meeting rate and fee criteria | Final savings filter works on the sample; an earlier exploratory filter contains a typo |
| [Netflix sample database](Netflix%20Database) | Explore titles, dates, directors, and release years | PostgreSQL example; supplied sample contains 20 titles |
| [Spotify data import and analysis](Spotify%20Data%20Import%20%26%20Analysis) | Table creation, track duration, and artist-level summaries | Popularity ranking and numeric typing need refinement; CSV and exact source link are not included |
| [Superstore queries](Superstore%20Project) | Sorting, filtering, and price totals | Source table and question definitions are not included |

## SQL skills shown

- **Joins:** combine related tables with `INNER JOIN` and `LEFT JOIN`.
- **Aggregation:** summarize with `COUNT`, `SUM`, `AVG`, `MIN`, and `MAX`.
- **Filtering and grouping:** use `WHERE`, `GROUP BY`, and `HAVING`.
- **Reporting logic:** use `CASE`, `ROUND`, aliases, sorting, and limits.
- **Subqueries:** compare values against an aggregate result.

## Reading and reproducing the examples

Some files contain SQL together with schema definitions and recorded result tables. Others contain queries only. For files with an embedded schema, load the schema and sample inserts into a fresh database, then run each query separately. Copy the SQL sections rather than the entire mixed-format document.

Use the dialect identified in the example: most embedded examples specify SQLite, while Netflix specifies PostgreSQL. Customer/order and Spotify use the `BIT_DB` namespace but do not identify a confirmed database engine or provide their imported data.

## Query review and quality checks

A [repository-wide query review](docs/query-review.md) records confirmed issues, assumptions, suggested corrections, and validation limits.

The review executed **22 queries across six embedded SQLite examples**, plus the BankProducts script. Separate small test cases demonstrated shipping-history, percentage-boundary, and customer-metric issues. These checks do not establish correctness on missing source datasets, and proposed changes have not been applied to the original exercise files.

## Coursework attribution

Exercises were completed through Break Into Tech. Training schemas, prompts, and sample data are course-provided where identified in the original files; those files retain their attribution notices. My SQL submissions are presented as coursework, with review suggestions documented separately. No employer datasets are included.
