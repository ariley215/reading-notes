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

## Database Management

In SQL, the *database schema* is what describes the structure of each table, and the datatypes that each column of the table can contain.

Use an **INSERT** statement when inserting data into a table.

- Declares which table to write into, the columns of data that we are filling, and one or more rows of data to insert

In some cases, if you have incomplete data and the table contains columns that support default values, you can insert rows with only the columns of data you have by specifying them explicitly.

```SQL

INSERT INTO mytable
(column, another_column, …)
VALUES (value_or_expr, another_value_or_expr, …),
      (value_or_expr_2, another_value_or_expr_2, …),
      …;

```

**UPDATE** statement is used to update existing data in a table.

- This works by taking multiple column/value pairs, and applying those changes to each and every row that satisfies the constraint in the **WHERE** clause
- Leaving out the **WHERE** clause causes all the data to be updated.
    - Write the constraint first and test it in a **SELECT** query to make sure you are updating the right rows

```SQL

UPDATE mytable
SET column = value_or_expr, 
    other_column = another_value_or_expr, 
    …
WHERE condition;

```

**DELETE** statement are similar to **UPDATE** it needs a **WHERE** condition or all the rows will be deleted.

**CREATE TABLE** statement:

```SQL
CREATE TABLE IF NOT EXISTS mytable (
    column DataType TableConstraint DEFAULT default_value,
    another_column DataType TableConstraint DEFAULT default_value,
    …
);

```

*Example using movies table, showing constraints*

```SQL

CREATE TABLE movies (
    id INTEGER PRIMARY KEY,
    title TEXT,
    director TEXT,
    year INTEGER, 
    length_minutes INTEGER
);

```

- If there already exists a table with the same name, the SQL implementation will usually throw an error, so to suppress the error and skip creating a table if one exists, you can use the IF NOT EXISTS clause.

**ALTER TABLE** statements add, remove, or modify columns and table constraints.

- The syntax for adding a new column is similar to the syntax when creating new rows in the **CREATE TABLE** statement
  - In some databases like MySQL, you can even specify where to insert the new column using the FIRST or AFTER clauses

**DROP** can be used to remove columns in some databases

**RENAME TO** will rename the table

Each database implementation supports different methods of altering their tables, so it's always best to consult your database docs before proceeding:

MySQL, Postgres, SQLite, Microsoft SQL Server.

**DROP TABLE** statement, which differs from the **DELETE** statement in that it also removes the table schema from the database entirely.

## Tutorial Completion Images

![First-Lesson](401-Notes/screenshots/Screenshot2024-01-02at3.23.46 PM(2).jpg)
![second-lesson](/Users/andrea/projects/courses/code-401/reading-notes/401-Notes/screenshots/Screenshot 2024-01-02 at 7.15.03 PM.png)


### Sources

[SQL Bolt](https://sqlbolt.com/)

[SQL Tutorial](https://www.computer-pdf.com/3-sql-database-tutorial-for-beginners)