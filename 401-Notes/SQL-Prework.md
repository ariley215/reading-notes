# SQL Practice

SELECT statements = queries.

- A query in itself is just a statement which declares what data we are looking for, where to find it in the database, and optionally, how to transform it before it is returned.

*Select query for a specific columns*

```SQL
SELECT column, another_column, …
FROM mytable;

```

*Select query for all columns*

```SQL
SELECT * 
FROM mytable;
```

## Queries with constraints

To filter certain results from being returned, we need to use a **WHERE clause** in the query.

- The clause is applied to each row of data by checking specific column values to determine whether it should be included in the results or not.

```SQL
SELECT column, another_column, …
FROM mytable
WHERE condition
    AND/OR another_condition
    AND/OR …;
```

| Operator      | Condition | Example |
| ----------- | ----------- | ------- |
| =      | Case sensitive exact string comparison (notice the single equals)    | col_name = "abc"
| != or <> |  Case sensitive exact string inequality comparison  | col_name != "abcd"|
| LIKE | Case insensitive exact string comparison| col_name LIKE "ABC" 
| NOT LIKE | Case insensitive exact string inequality comparison | col_name NOT LIKE "ABCD"
| % | Used anywhere in a string to match a sequence of zero or more characters (only with LIKE or NOT LIKE)| col_name LIKE "%AT%"(matches "AT", "ATTIC", "CAT" or even "BATS")|
| - | Used anywhere in a string to match a single character (only with LIKE or NOT LIKE)| col_name LIKE "AN_"(matches "AND", but not "AN")|
| IN (…)| String exists in a list | col_name IN ("A", "B", "C")|
|NOT IN (…)| String does not exist in a list| 	col_name NOT IN ("D", "E", "F")|

## Filter and Sort Query Results

**DISTINCT** keyword will blindly remove duplicate rows

Most data in real databases are added in no particular column order.

**ORDER BY** clause is specified, each row is sorted alpha-numerically based on the specified column's value.

The **LIMIT** will reduce the number of rows to return, and the optional **OFFSET** will specify where to begin counting the number rows from.

*Select query with limited rows*

```SQL
SELECT column, another_column, …
FROM mytable
WHERE condition(s)
ORDER BY column ASC/DESC
LIMIT num_limit OFFSET num_offset;
```

## Multi-table queries with JOINs

Entity data in the real world is often broken down into pieces and stored across multiple orthogonal tables using a process known as normalization.

Tables that share information about a single entity need to have a *primary key* that identifies that entity uniquely across the database

- Using the JOIN clause in a query, we can combine row data across two separate tables using this unique key

- The INNER JOIN is a process that matches rows from the first table and the second table which have the same key

*Select query with INNER JOIN on multiple tables*

``` SQL
SELECT column, another_table_column, …
FROM mytable
INNER JOIN another_table 
    ON mytable.id = another_table.id
WHERE condition(s)
ORDER BY column, … ASC/DESC
LIMIT num_limit OFFSET num_offset
```

### Sources:
[SQL Bolt](https://sqlbolt.com/)

[SQL Tutorial](https://www.computer-pdf.com/3-sql-database-tutorial-for-beginners)