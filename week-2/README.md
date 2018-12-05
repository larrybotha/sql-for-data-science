# Week 2 - Filtering, Sorting, and Calculating Data with SQL

## Module Introduction

[video](https://www.coursera.org/learn/sql-for-data-science/lecture/GAA9h/module-introduction)

This module introduces a number of operators and clauses to query data:

- `WHERE`
- `BETWEEN`
- `IN`
- `OR`
- `NOT`
- `LIKE`
- `ORDER BY`
- `GROUP BY`

It also introduces wildcards, and math operators which can be used to aggregate
data:

- `AVERAGE`
- `COUNT`
- `MAX`
- `MIN`

## Basics of Filtering with SQL

[video](https://www.coursera.org/learn/sql-for-data-science/lecture/ESCUo/basics-of-filtering-with-sql)

### Why filter

- allows one to be specific about what they want
- reduces the number of records you receive
- increases query performance
- reduces strain on the client application

### `WHERE` clause

The `WHERE` clause filters queries by criteria you define.

```sql
SELECT [fields]
FROM [table]
WHERE [criteria]
```

These criteria are determined through using a number of operators:

- `=` - equal
- `<>` - not equal
- `>` - greater than
- `<` - less than
- `>=` - greater than or equal
- `<=` - less than or equal
- `BETWEEN` - between an _inclusive_ range
- `IS NULL` - is a null value

To use `BETWEEN` we need to use it in conjunction with `AND`:

```sql
SELECT *
FROM my_table
WHERE size BETWEEN 5 AND 10;
```

## Advanced Filtering: `IN`, `OR`, and `NOT`

[video](https://www.coursera.org/learn/sql-for-data-science/lecture/pycB9/advanced-filtering-in-or-and-not)

### `IN` operator

The `IN` operator specifies a range of conditions, delimited by commas, and
within parentheses.

```sql
-- select only suppliers with the provided ids
SELECT *
FROM suppliers
WHERE supplierId IN (9, 10, 15);
```

To assert against string values, surround the strings in single quotes.

### `OR` operator

As with the disjunction operator in many programming languages, SQL will ignore
the second condition in an `OR` statement if the first condition is true.

```sql
SELECT *
FROM pets
WHERE name = 'sammy' OR 'hammy';
```

### `IN` vs `OR`

`IN` works similarly to `OR`, but there are benefits to using `IN`:

- one can provide a long list of options
- `IN` executes faster than `OR`
- the condition used in `IN` are not subject to a specific order
- `IN` clauses can container another `SELECT` statement to make subqueries

### `NOT` operator

`NOT` is used to exclude rows based on conditions:

```sql
SELECT *
FROM employees
WHERE NOT city = 'London' AND NOT city = 'Seattle';
```

## Using Wildcards in SQL

[video](https://www.coursera.org/learn/sql-for-data-science/lecture/xIAow/using-wildcards-in-sql)

A wildcard is a special character used to match parts of a value, such as when
you know only a portion of a value.

Search patterns are made from literal text, a wildcard character, or a
combination of both.

Wildcard queries use the `LIKE` operator. `LIKE` is actually a predicate, but is
often referred to as an operator.

Wildcards can only be used for string data.

### Using wildcards

- `%text` - matches anything ending with `text`
- `text%` - matches anything beginning with `text`
- `%text%` - matches anything containing `text`
- `a%b` - matches anything that is preceded by `a` and ends with `b`

Wildcards will not match NULL values.

### Underscore wildcard

Many RDMSs support `_` as a wildcard. It functions in the same way that `%` has
been outlined here.

### Implications of using wildcards

- wildcard queries take longer to run
- better to use other operators if possible
- statements with wildcards at the end of patterns take longer to run than
    patterns with wildcards at the beginning
- placement of wildcads is imporant
