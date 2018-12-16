# Week 4 - Modifying and Analyzing Data with SQL

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->


- [Module Introduction](#module-introduction)
  - [Methods for modifying data](#methods-for-modifying-data)
  - [Case statements](#case-statements)
- [Working with Text Strings](#working-with-text-strings)
  - [Working with string variables](#working-with-string-variables)
  - [Concatenations](#concatenations)
  - [Trimming strings](#trimming-strings)
  - [Substring](#substring)
  - [Upper and Lower](#upper-and-lower)
- [Working with Date and Time Strings](#working-with-date-and-time-strings)
  - [Date formats](#date-formats)
  - [SQLite date time functions](#sqlite-date-time-functions)
  - [Timestrings](#timestrings)
  - [Modifiers](#modifiers)
- [Date and Time Strings Examples](#date-and-time-strings-examples)
  - [`STRFTIME`](#strftime)
  - [Compute current date](#compute-current-date)
  - [Compute dates and times for the current date](#compute-dates-and-times-for-the-current-date)
- [Case Statements](#case-statements)
  - [Search case statement](#search-case-statement)
- [Views](#views)
  - [Creating a view](#creating-a-view)
- [Data Governance and Profiling](#data-governance-and-profiling)
  - [What is data profiling?](#what-is-data-profiling)
    - [Object data profile](#object-data-profile)
    - [Column data profile](#column-data-profile)
  - [Governance best practises](#governance-best-practises)
- [Using SQL for Data Science, Part 1](#using-sql-for-data-science-part-1)
  - [Working through a problem](#working-through-a-problem)
  - [Data understanding](#data-understanding)
    - [Subject understanding](#subject-understanding)
  - [Business understanding](#business-understanding)
- [Using SQL for Data Science, Part 2](#using-sql-for-data-science-part-2)
  - [Profiling data](#profiling-data)
    - [Start with `SELECT`](#start-with-select)
    - [Test and troubleshoot](#test-and-troubleshoot)
    - [Format and comment](#format-and-comment)
    - [Review](#review)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Module Introduction

[video](https://www.coursera.org/learn/sql-for-data-science/lecture/V9qBD/module-introduction)

### Methods for modifying data

- concatenating
- trimming
- changing case
- substring
- date and time strings

### Case statements

SQL's version of `IF`, `THEN`, and `ELSE` statements

## Working with Text Strings

[video](https://www.coursera.org/learn/sql-for-data-science/lecture/yw49P/working-with-text-strings)

### Working with string variables

Manipulating strings on the server reduces work that needs to be done by the
client.

String functions:

- concatenate
- substring
- trim
- upper
- lower

### Concatenations

In SQLite one uses `||` to concatenate strings. SQL Server allows for `+` to
concatenate strings.

```sql
SELECT
  company_name
  ,contact_name
  ,compaany_name || ' (' || contact_name || ')'
FROM Customers
```

### Trimming strings

- `TRIM`
- `RTRIM`
- `LTRIM`

```sql
SELECT TRIM("   You the best ") AS trimmed_string;
```

### Substring

The `SUBSTR` command is 1-indexed. It accepts the string on which to operate,
the starting index, and the number of characters to return from that index.

```sql
-- SUBSTR([string name], [start pos], [num return chars])

SELECT first_name, SUBSTR(first_name, 2, 3)
FROM Employees
WHERE department_id = 69;
```

### Upper and Lower

```sql
-- make string uppercase
SELECT UPPER(first_name) FROM Customers
-- or
SELECT UCASE(first_name) FROM Customers

-- make string lowercase
SELECT LOWER(first_name) FROM Customers
```

## Working with Date and Time Strings

[video](https://www.coursera.org/learn/sql-for-data-science/lecture/kbIot/working-with-date-and-time-strings)

Dates are stored as `date` or `datetime` datatypes. The format of the date is
dependent on the RDMS you are working with.

If only the date portion of an entry is being queried, queries are generally
quite easy. As soon as the time portion is included in the query complexity
increases.

When inserting a date into a table, care must be taken to ensure that it is done
so in the correct format.

### Date formats

`DATE`: `YYYY-MM-DD`

`DATETIME`: `YYYY-MM-DD HH:MM:SS`

`TIMESTAMP`: `YYYY-MM-DD HH:MM:SS`

```sql
-- querying a DATETIME field with:
WHERE purchase_date = '2016-12-12'
-- will return no results because it's missing the _time_ portion of the data
```

### SQLite date time functions

SQLite supports 5 date and time functions:

```sql
DATE(timestring, [modifier, ...])
TIME(timestring, [modifier, ...])
DATETIME(timestring, [modifier, ...])
JULIANDAY(timestring, [modifier, ...])
STRFTIME(format, timestring, [modifier, ...])
```

### Timestrings

A time string can be in one of the following formats:

```
YYYY-MM-DD
YYYY-MM-DD HH:MM
YYYY-MM-DD HH:MM:SS
YYYY-MM-DD HH:MM:SS.SSS
# T here is a literal T delimiting the date from the time
YYYY-MM-DDTHH:MM
YYYY-MM-DDTHH:MM:SS
YYYY-MM-DDTHH:MM:SS.SSS
HH:MM
HH:MM:SS
HH:MM:SS.SSS
```

### Modifiers

Modifiers alter the date, and multiple modifiers can be applied to a date.
Modifiers are applied in the order they are provided, from left to right.

The following modifiers add a specified amount to the date:

```sql
NNN days
NNN hours
NNN minutes
NNN.NNN seconds
NNN months
NNN years
```

The following modifiers return a date "shifted back" from the provided date:

```sql
start of month
start of year
start of day
```

`weekday N` shifts the date forward to the 0-indexed day in the future, unless
the provided date is that index. Sunday is 0.

`localtime` modifies UTC dates by shifting the time to the localtime of where
the process is running. `utc` is the inverse of `localtime`.

`now` will retrieve the current date and time.

## Date and Time Strings Examples

[video](https://www.coursera.org/learn/sql-for-data-science/lecture/ayLVq/date-and-time-strings-examples)

### `STRFTIME`

`STRFTIME` extracts specific values from dates.

With a value such as a birth date, we often don't want any time information, and
it is probable that it'll be stored as `00:00:00` anyhow. We don't want to
necessarily retrieve time information, so we can use `STRFTIME` to retrieve only
portions of the date that are important to us.

```sql
-- get the year, month, and day of each employee
SELECT
  birthdate
  , STRFTIME('%Y', birthdate) AS year
  , STRFTIME('%m', birthdate) AS month
  , STRFTIME('%d', birthdate) AS day
FROM employees
```

### Compute current date

```sql
SELECT DATE('now')
```

This can be valuable to determine how long ago some event occurred, or to
retrieve records that were created or updated within a specific period of time.

### Compute dates and times for the current date

```sql
-- date
SELECT STRFTIME('%Y %m %d', 'now')

-- time
SELECT STRFTIME('%H %M %S %s', 'now')

-- compute employees' ages
SELECT
  birthdate
  ,STRFTIME('%Y', birthdate) AS year
  ,STRFTIME('%m', birthdate) AS month
  ,STRFTIME('%d', birthdate) AS day
  ,DATE(('now') - birthdate) AS age
FROM employees
```

## Case Statements

[video](https://www.coursera.org/learn/sql-for-data-science/lecture/qsHKS/case-statements)

Case statements in SQL mimic if-else statements in general programming.

Case statements can be used in any clause that accepts a valid expression, e.g.

- `WHERE`
- `ORDER BY`
- `HAVING`

and in statements:

- `SELECT`
- `INSERT`
- `UPDATE`
- `DELETE`

```sql
CASE [input_expression]
  WHEN c1 THEN e1
  WHEN c2 THEN e2
  ELSE e3
END column_name
```

e.g.

```sql
SELECT
  employee_id
  ,firstname
  ,lastname
  ,CASE city
    WHEN 'Calgary' THEN 'Calgary'
    ELSE 'Other'
  END city
FROM Employees
ORDER BY lastname, firstname;
```

As with programming languages, `CASE` in SQL will short-circuit on a true
statement, and no further statements will be evaluated. If `ELSE` is omitted and
no `WHEN` statements evaluate to true, `NULL` will be returned.

### Search case statement

By omitting the optional input expression that directly follows the `CASE`
keyword one can use the `WHEN` statements to evaluate and compare values:

```sql
CASE
  WHEN result_expression [...n]
  [ELSE else_result]
END
```

e.g.:

```sql
SELECT
  track_id
  ,name
  ,bytes
  ,CASE
    WHEN bytes < 30000 THEN 'small'
    WHEN bytes >= 30001 AND bytes <= 50000 THEN 'medium'
    WHEN bytes > 50000 THEN 'large'
    ELSE 'Other'
  END bytes_category
FROM Tracks;
```

`THEN` can return another column, not only text or hard-coded values.

## Views

[video](https://www.coursera.org/learn/sql-for-data-science/lecture/Ml5XL/views)

Views are stored queries. With a view, one can add or remove columns without
changing the schema. They can also be used to encapsulate queries.

Views persist until the database connection is closed.

```sql
CREATE [TEMP] VIEW [IF NOT EXISTS] view_name(column-name-list) AS
  SELECT ...
```

`IF NOT EXISTS` can be used to determine whether the view should be created or
not based on the name provided when creating the view.

### Creating a view

```sql
-- create a view called my_view where...
CREATE VIEW my_view AS
  -- there are 6 columns
  SELECT
    r.region_description
    ,t.territory_description
    ,e.lastname
    ,e.firstname
    ,e.hire_date
    ,e.reports_to
  -- obtained by selecting from 4 tables, joined using an INNER JOIN
  FROM Regions r
    INNER JOIN Territories t ON r.region_id = t.region_id
    INNER JOIN EmployeeTerritories et ON t.territory_id = et.territory_id
    INNER_JOIN Employees e on et.employee_id = e.employee_id
```

To actually use the view, one treats it as if it were an existing table:

```sql
SELECT *
FROM my_view
DROP VIEW my_view;
```

e.g.:

```sql
SELECT
  COUNT(territory_description)
  ,firstname
  ,lastname
FROM my_view
GROUP BY lastname, firstname;
```

If a view is no longer required, one should clean up by `DROP`ping the view.

Views can be useful in environments where you're unable to write data to tables.

## Data Governance and Profiling

[video](https://www.coursera.org/learn/sql-for-data-science/lecture/icb6W/data-governance-and-profiling)

### What is data profiling?

Data profiling is the process of examining data for completeness and accuracy in
order to understand the data before querying it.

#### Object data profile

- understanding how many rows of data are in the table
- when data was last updated

#### Column data profile

- evaluating the data types of each column
- evaluating the number of distinct values in a column
- evaluating the number of NULL vs non-NULL values
- descriptive statistics:
    - maximums and minimums
    - averages
    - standard deviation for column values

The reason one profiles their data is to understand what their data is so that
they can test it. Without profiling data first, one would find it more difficult
to determine when queries are not returning expected results.

### Governance best practises

These are the rules around data in an organisation.

- understand your read and write priveleges
- cleaning up data while experimenting and evaluating queries
- what is the process followed when changes are made, e.g. a model requires
    updating, and its associated tables

## Using SQL for Data Science, Part 1

[video](https://www.coursera.org/learn/sql-for-data-science/lecture/Mq7rr/using-sql-for-data-science-part-1)

### Working through a problem

- data understanding
- business understanding
- profiling
  - start with `SELECT`
  - test
  - format and comment
  - review

### Data understanding

Understanding the data you'll be working with is the most important step. This
includes:

- understanding relationships, i.e. entity diagrams
- evaluating columns that have NULL values
- how are string values inserted, e.g. generated, or user input

#### Subject understanding

Without understanding the subject of focus, writing queries may take more time.

- understanding where data joins are
- differentiating integers from strings
- investing time to understand the subject matter will help later during analysis

### Business understanding

It's important to be able to distinguish between business requirements and the
data required to solve that requirement.

Understanding the problem at a core level is important:

- which customers do we want to study?
- should we omit certain customers based on some criteria?
- what data is important when drilling down on a specific entity?

## Using SQL for Data Science, Part 2

[video](https://www.coursera.org/learn/sql-for-data-science/lecture/Bpbog/using-sql-for-data-science-part-2)

### Profiling data

Profiling data is about drilling down into the details of your data.

This includes:

- creating a data model and mapping the fields and tables you need
- considering joins and calculations
- understanding where there may be any data quality or format issues

#### Start with `SELECT`

Especially if you're unfamiliar with the data, start small. Every statement
starts with a `SELECT`.

If using subqueries, start with the innermost query, and work out from there.

#### Test and troubleshoot

- don't want until the end to test queries
- test queries after each join or filter
- evaluate the results
- start small and evaluate step-by-step when troubleshooting queries

#### Format and comment

- use the correct formatting and indentation
- comment strategically
- clean code and comments help when handing off or revisiting code

#### Review

- always review old queries
- evaluate that queries are in alignment with business rules
