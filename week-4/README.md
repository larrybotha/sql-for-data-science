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
