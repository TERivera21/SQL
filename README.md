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

These are training examples. I verified my coursework queries against the supplied class data and expected results when completing the exercises. Those results apply to that training context, rather than employer, patient, or current company data.

## More exercises

| Example | Focus | Skills demonstrated |
| --- | --- | --- |
| [Customer and order analytics](Customer%20and%20Order%20Analytics) | Revenue, customer accounts, orders, and product quantities | Aggregation, joins, subqueries, and filtering |
| [Shipping customer joins](JOINS%20for%20Shipping%20Company%20Data) | Compare all customers with those who have shipping history | Left and inner joins, distinct results, and shipping-method filters |
| [Fortune company sample analysis](Fortune%20500%20Analysis) | Benefits, company-size categories, and industry averages | CASE expressions, grouping, filtering, and ranking |
| [Marathon completion categories](CASE%20and%20ROUND%20examples%20in%20a%20Marathon) | Convert completion fractions to percentages and categories | CASE, ROUND, and grouped counts |
| [Bank product filters](BankProducts) | Find savings products meeting rate and fee criteria | Multiple conditions and product filtering |
| [Netflix sample database](Netflix%20Database) | Explore titles, dates, directors, and release years | PostgreSQL joins, date functions, and sorting |
| [Spotify data import and analysis](Spotify%20Data%20Import%20%26%20Analysis) | Table creation, track duration, and artist-level summaries | Schema creation, grouping, and averages |
| [Superstore queries](Superstore%20Project) | Sorting, filtering, and price totals | SUM, WHERE, ORDER BY, and LIMIT |

## SQL skills shown

- **Joins:** combine related tables with `INNER JOIN` and `LEFT JOIN`.
- **Aggregation:** summarize with `COUNT`, `SUM`, `AVG`, `MIN`, and `MAX`.
- **Filtering and grouping:** use `WHERE`, `GROUP BY`, and `HAVING`.
- **Reporting logic:** use `CASE`, `ROUND`, aliases, sorting, and limits.
- **Subqueries:** compare values against an aggregate result.

## Reading and reproducing the examples

Some files contain SQL together with schema definitions and recorded result tables. Others contain queries only. For files with an embedded schema, load the schema and sample inserts into a fresh database, then run each query separately. Copy the SQL sections rather than the entire mixed-format document.

Use the dialect identified in the example: most embedded examples specify SQLite, while Netflix specifies PostgreSQL. Customer/order and Spotify use the `BIT_DB` namespace but do not identify a confirmed database engine or provide their imported data.

## Coursework verification and further review

**Coursework verification:** I checked these queries against the class datasets and expected results when completing the exercises. This describes verification in the original learning context, not a guarantee for every possible dataset or database engine.

**Independent sample checks:** A later review executed **22 queries across six embedded SQLite examples**, plus the BankProducts script. These checks confirm execution on the included samples. The original imported datasets and grading criteria were not available for every exercise, so the review does not independently revalidate all coursework answers.

**Further development:** The [query review](docs/query-review.md) includes suggestions for handling broader datasets, clarifying metric definitions, and checking possible query/prompt mismatches. For example, varying product prices, customers with multiple shipping methods, and exact category boundaries may require additional logic even when an original sample returns the expected results.

The review's references to “corrections” or “needs refinement” should be read in light of these distinctions: some findings concern new edge cases or unconfirmed assumptions, while others identify specific code/prompt discrepancies worth checking against the original instructions. They do not establish that all course-verified answers were incorrect. Original exercise files are preserved, and proposed revisions have not been applied.

## Coursework attribution

Exercises were completed through Break Into Tech. Training schemas, prompts, and sample data are course-provided where identified in the original files; those files retain their attribution notices. My SQL submissions are presented as coursework, with review suggestions documented separately. No employer datasets are included.
