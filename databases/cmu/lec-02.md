# Advanced SQL

## Nested Queries

Most optimizer will try to rewrite using Joins.

ALL - all rows in the inner query must satisfy the condition (Used for finding the min/max value)
ANY - at least one row in the inner query must satisfy the condition
IN - =ANY()
EXISTS - =ANY()

## Window Functions

Window functions are used to perform calculations on a set of rows that are somehow related to the current row.

```sql
SELECT *, ROW_NUMBER() OVER () row_num as FROM enrolled;

SELECT *, SUM(salary) OVER (PARTITION BY dept) FROM employees; -- partition is similar to group by
```

Special window functions:
- RANK() - rank of the current row
- ROW_NUMBER() - row number of the current row

## Common Table Expressions (CTE)

Provides a way to write auxillary statements for use in the main query.
Temp table for just 1 query. Alternative to nested queries & views.

```sql
WITH cte_name AS (
  SELECT * FROM table_name
)
```