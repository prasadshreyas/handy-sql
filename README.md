# Handy SQL

This guide aims to be the handy-only-guide-you'll-ever-need resource for SQL concepts and techniques. 

## Joins

1. **INNER JOIN**: Returns records that have matching values in both tables.

```sql
SELECT * FROM table1
INNER JOIN table2 ON table1.key = table2.key;
```

2. **FULL JOIN (OUTER)**: Returns all records from both tables, with unmatched records replaced by NULL values.

```sql
-- MySQL full join logic:
SELECT * FROM table1
LEFT JOIN table2 ON table1.key = table2.key
UNION
SELECT * FROM table1
RIGHT JOIN table2 ON table1.key = table2.key;
```

3. **LEFT JOIN**: Returns all records from the left table, and matched records from the right table. Unmatched records from the right table are replaced by NULL values.

```sql
SELECT * FROM table1
LEFT JOIN table2 ON table1.key = table2.key;
```

4. **RIGHT JOIN**: Returns all records from the right table, and matched records from the left table. Unmatched records from the left table are replaced by NULL values.

```sql
SELECT * FROM table1
RIGHT JOIN table2 ON table1.key = table2.key;
```

5. **ANTI JOIN**: Returns records from the left table that do not have a match in the right table.

```sql
SELECT * FROM table1
LEFT JOIN table2 ON table1.key = table2.key
WHERE table2.key IS NULL;
```

6. **SEMI JOIN**: Returns records from the left table that have a match in the right table.

```sql
SELECT * FROM table1
WHERE EXISTS (SELECT * FROM table2 WHERE table1.key = table2.key);
```

## Subqueries

Subqueries are queries embedded within other queries. They can be used in SELECT, FROM, WHERE, and HAVING clauses.

Examples:

```sql
SELECT sub.*
FROM (
    -- results from inner query go here
) sub
WHERE sub.column = 'value';
```

```sql
SELECT *
FROM table1
WHERE column1 = (SELECT column2
                 FROM table2);
```

## SQL Window functions

Window functions perform calculations across a set of rows that are related to the current row. They are used with the `OVER()` clause.

```sql
-- Creates a window over the entire table and calculates the max(column) for each record
SELECT *,
MAX(column) OVER () AS max_column
FROM table_name;
```

```sql
-- Creates a window partitioned by another column and calculates the max(column) for each record within the partition
SELECT *,
MAX(column) OVER (PARTITION BY partition_column) AS max_column
FROM table_name;
```

### Rank

`ROW_NUMBER()`, `RANK()`, and `DENSE_RANK()` functions assign a unique rank to each row within a result set.

```sql
-- Assigns a unique rank number to each row
SELECT *,
ROW_NUMBER() OVER () AS row_number
FROM table_name;
```

```sql
-- Assigns a unique rank number to each row within a partition
SELECT *,
ROW_NUMBER() OVER (PARTITION BY partition_column) AS row_number
FROM table_name;
```

```sql
-- Assigns a unique rank number to each row within a partition, ordered by another column
SELECT *,
ROW_NUMBER() OVER (PARTITION BY partition_column ORDER BY order_column) AS row_number
FROM table_name;
```

### Dense Rank

`RANK()` assigns the same rank to ties, with the next ranking(s) skipped. In contrast, `DENSE_RANK()` assigns consecutive ranks to all items, including ties.

### Lag/Lead

`LAG()` and `LEAD()` functions access data from a previous or following row in the same result set.

```sql
-- Retrieves the value of a column from the previous row
SELECT *,
LAG(column) OVER (PARTITION BY partition_column ORDER BY order_column) AS previous_value
FROM table_name;
```

### NTH_VALUE()

`NTH_VALUE()` function returns the value of a column for the Nth row in the ordered result set.

```sql
SELECT
    column1,
    column2,
    NTH_VALUE(column1, N) OVER  (
        ORDER BY column2 DESC
    ) Nth_highest_value
FROM
    table_name;
```
