# SQL query review

Reviewed: October 7, 2026  
Scope: all 11 exercise files and the repository README. Original exercise files are preserved. Suggestions below are proposed revisions, not executed changes to those files.

## Validation and limits

- Executed all 22 queries across the six files that contain SQLite-labelled schemas: marathon, salaries, Fortune, shipping, authors, and pharmaceuticals. All ran on the supplied samples. Execution success does not establish that each query answers its prompt.
- Executed the BankProducts script in SQLite. Its final savings filter returns four sample products.
- Used separate synthetic cases to test mixed shipping history, exactly 25% completion, changing product prices, repeated customer accounts, and integer division.
- Netflix was reviewed statically against its embedded PostgreSQL schema; it was not executed in PostgreSQL.
- Customer/order, Spotify, and Superstore lack their source data. No real-data results or complete runtime validation are claimed.
- Queries suggested here require testing in the intended engine. Dataset grain, join cardinality, data types, and metric definitions must be confirmed before publication of numerical conclusions.

## Review by exercise

| Exercise | Assessment | Recommended action |
| --- | --- | --- |
| Customer and Order Analytics | Several metric/grain issues; source tables missing | Correct revenue and account averages; define orders and eligibility; validate joins |
| Spotify Data Import & Analysis | Popularity query contradicts its prompt | Rank distinct artists by a defined aggregate, descending; confirm numeric types and engine |
| JOINS for Shipping Company Data | First two joins fit their prompts; third works only on simple sample history | Exclude customers with any non-Standard or unknown method for the strict Standard-only requirement |
| Fortune 500 Analysis | Runs in SQLite, but prompts and grouped output need refinement | Align ranking/filter descriptions; remove arbitrary company name from industry aggregates |
| CASE and ROUND examples in a Marathon | Percentages run; boundary issue | Include exactly 25% in the 25%+ bucket |
| BankProducts | Final filter works; earlier exploratory filter has typo/column mismatch | Clarify intended filter before replacing it |
| Comparing Salaries for Various Departments | Both queries match the stated thresholds on sample data | Add ordering for repeatable presentation; explain totals versus averages |
| Multiple JOINS to return requested data on Authors | Final joins answer the current-economics prompt on sample data | Document relationship grain and orphan textbook links |
| Pharmaceutical JOINs | Joins reproduce four doctor-medication pairs and six selected-medication fill dates | Clarify last-fill meaning and one-row-per-person sample assumption |
| Netflix Database | Static review finds queries consistent with sample questions | Identify small sample, preserve NULL meaning, address source encoding, add deterministic ordering |
| Superstore Project | Four queries are readable, but source schema and prompts are absent | Define what price totals mean and supply sample/schema |

## 1. Customer and Order Analytics

**Revenue (questions 5 and 6).** `SUM(quantity) * price` assumes the same price on every row for a product and selects an ungrouped price. Use `SUM(quantity * price)`. A synthetic group with quantities 1, 2, 1 and prices 10, 20, 30 produces 80 in line revenue; SQLite's original expression produced 40. This proves the assumption matters, not that actual course prices vary.

**Average spend per account.** `AVG(quantity * price)` averages sales rows. Aggregate spending by account first, then average those totals:

```sql
-- Proposed pattern: assumes one customer mapping per order_id.
-- Validate that assumption before using on source data.
WITH account_spend AS (
    SELECT cust.acctnum,
           SUM(feb.quantity * feb.price) AS total_spend
    FROM BIT_DB.FebSales AS feb
    INNER JOIN BIT_DB.customers AS cust
        ON feb.orderid = cust.order_id
    WHERE LENGTH(TRIM(feb.orderid)) = 6
      AND TRIM(feb.orderid) <> 'Order ID'
      AND cust.acctnum IS NOT NULL
    GROUP BY cust.acctnum
)
SELECT AVG(total_spend) AS average_spend_per_purchasing_account
FROM account_spend;
```

This denominator includes matched accounts purchasing in February, not all customer accounts. A three-row, two-account synthetic example returns 26.67 per row versus 40 per purchasing account.

**Average quantity per account.** `COUNT(cust.acctnum)` counts matched rows rather than distinct accounts. Use account-level totals followed by `AVG`, or floating-point total units divided by distinct matched purchasing accounts. In the synthetic case, the original integer expression returns 1 versus 2 units per account.

**More than two products at a time.** `quantity > 2` filters an individual line. If the requirement means total units per order, aggregate by order and use `HAVING SUM(quantity) > 2`. If it means distinct products, use `COUNT(DISTINCT product)`. Also define whether spend is averaged per qualifying order or per qualifying customer.

**Orders, joins, and cleaning.**
- `COUNT(orderid)` counts rows, which may differ from distinct orders. Confirm the table grain before choosing a count.
- The February account-number query filters the right table after a left join; it behaves as a matched join. Use an explicit inner join if only purchasers are intended.
- Qualify `orderid` with a table alias and test for duplicate customer mappings that could multiply revenue.
- Six-character order IDs are a course-specific validity assumption. Apply cleaning consistently and document excluded rows. The file has both 'Order ID' and ' Order ID' literals.
- Minimum price may need numeric conversion/header removal if imports contain text. Address this in a cleaned staging table.
- City substring matching is acceptable for this exercise, but a parsed city/state field is more reliable.

## 2. Spotify Data Import & Analysis

The “top 10 most popular artists” query sorts ascending and returns track-level artist names, potentially repeated. Define popularity as mean track popularity within the imported sample:

```sql
SELECT artist_name,
       AVG(popularity) AS average_track_popularity,
       COUNT(*) AS track_count
FROM BIT_DB.Spotifydata
GROUP BY artist_name
ORDER BY average_track_popularity DESC, artist_name
LIMIT 10;
```

This is a sample-based artist ranking, not Spotify's global artist ranking. Confirm that duplicates and sampling do not distort it.

Other improvements:
- `instrumentalness` is declared TEXT but averaged. Validate values and store a numeric type.
- The “highest average instrumentals” query returns every artist; add a limit or describe it as a ranking. Decide how ties should be handled.
- The longest-song query should also display duration and define tie handling.
- `#` comments are not portable to SQLite/PostgreSQL. Confirm the engine; use `--` comments and terminate table creation with a semicolon.
- Decimal precision/scale must fit actual imported ranges. Inspect loudness and other distributions before selecting types.
- Add the exact Kaggle source, import steps, schema engine, and data dictionary. They are currently missing.

## 3. Shipping customer eligibility

Filtering shipment rows to Standard can return a customer who also used Express. The supplied sample has one shipment per recorded sender, so it hides this problem. A synthetic customer with both methods is included by the original filter.

For the prompt's strict “only Standard before” interpretation:

```sql
SELECT c.customer_id, c.customer_name, c.contact_email,
       'Standard' AS shipping_method
FROM customers AS c
WHERE EXISTS (
    SELECT 1 FROM shipments AS s
    WHERE s.sender_id = c.customer_id
      AND s.shipping_method = 'Standard'
)
AND NOT EXISTS (
    SELECT 1 FROM shipments AS s
    WHERE s.sender_id = c.customer_id
      AND (s.shipping_method <> 'Standard'
           OR s.shipping_method IS NULL)
)
ORDER BY c.customer_id;
```

This also excludes unknown history and avoids repeated customer rows. If the business rule is merely “never Express,” rather than “only Standard,” eligibility for other methods and customers without history needs a separate decision.

## 4. Fortune sample analysis

- Query 1 ranks healthcare-providing companies by tenure, not employee count. Rewrite the comment to describe that behavior.
- Query 2 says “top 5” but has no ordering. Define a ranking criterion and add a tie-breaker. There are only four exact 'Finance' rows in this sample; 'Financials' is a separate label.
- Query 3's comment says more than five PTO days, while the filter uses more than 20. Clarify which is intended.
- Query 4 groups by industry while selecting an ungrouped company name. SQLite permits this but the name is arbitrary, and stricter engines can reject it. Use:

```sql
SELECT industry, ROUND(AVG(revenue), 3) AS average_revenue
FROM fortune_companies
GROUP BY industry
HAVING AVG(revenue) > 200
ORDER BY average_revenue DESC, industry;
```

Confirm whether the threshold should apply to raw or rounded averages and document revenue units. Company sizes and benefit labels are exercise-defined rules. The sample contains placeholders and must not be represented as current verified company benefits or an authoritative Fortune 500 dataset.

## 5. Marathon categories

`completion_fraction > +.25` excludes exactly 25%, placing it under 25%. Change to `>= 0.25`. The current sample has no exact 25% row, so its displayed counts do not reveal the defect. Percentage and ROUND queries execute as recorded. Use single-quoted string literals and clear, mutually exclusive range labels.

## 6. Bank products

The final Savings/rate/fee filter executes and returns Savings Account, Certificate of Deposit, IRA Account, and Money Market Account in the supplied sample.

The earlier `product_type='Checking' OR product_name='Savnigs'` has a spelling error and compares a type label to a product name. If the intended question is checking-or-savings products, use `product_type IN ('Checking', 'Savings')`. The intended question is not recorded, so do not silently assume it. Give each query its own prompt. The setup explicitly states that its database code was not authored by Teresa; preserve that attribution.

## 7–11. Remaining examples

**Department salaries:** both threshold queries execute correctly on the supplied sample. Engineering and Sales total 347,000 and 312,000, with mean salaries of 86,750 and 78,000. These are sample calculations. Order by the metric and department for presentation.

**Authors:** all seven queries execute. The final query returns four economics course/textbook/author rows. Left joins preserve textbooks lacking author mappings. The bridge table references TextbookID 22, which is absent from Textbooks; record this orphan rather than inventing a missing textbook. Repeated author names have distinct IDs, so do not deduplicate people by name.

**Pharmaceuticals:** both queries execute against the embedded training sample. Joining on person_ID works here because the sample has one row per person in each table. With multiple prescriptions per person it could create false pairings; a real schema needs a shared prescription key. “Last filled” is currently interpreted as each row's recorded date. A latest date overall needs MAX; a latest date per prescription needs the appropriate prescription identifier and grouping. These are course sample records.

**Netflix:** static review only. The movie count of eight is consistent with the embedded sample, which contains 20 titles. LIMIT without ORDER BY is fine for a quick preview but does not guarantee stable rows. The earliest movie query should define ties. NULL country/director/cast values mean missing data. Multi-value pipe-separated fields and visible mojibake in names require care if extending the analysis. Results do not describe today's Netflix catalog.

**Superstore:** source schema/data and business questions are absent. SUM(price) is a sum of listed prices, not sales revenue or inventory value. Inventory value would require a validated definition such as SUM(price * stock_quantity). Explain whether the final filter is meant to select the five highest-stock items priced above 50, and add a tie-breaker.

## Suggested next revision

Apply and validate corrections in separate, clearly labelled revised examples while retaining coursework attribution. Extract executable SQL into .sql files; keep schemas, prompts, and outputs in documentation. Retrieve missing source data or create explicitly synthetic fixtures before claiming dataset-level validation.
