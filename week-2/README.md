# Week 2 - Filtering, Sorting, and Calculating Data with SQL

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [Module Introduction](#module-introduction)
- [Basics of Filtering with SQL](#basics-of-filtering-with-sql)
  - [Why filter](#why-filter)
  - [`WHERE` clause](#where-clause)
- [Advanced Filtering: `IN`, `OR`, and `NOT`](#advanced-filtering-in-or-and-not)
  - [`IN` operator](#in-operator)
  - [`OR` operator](#or-operator)
  - [`IN` vs `OR`](#in-vs-or)
  - [`NOT` operator](#not-operator)
- [Using Wildcards in SQL](#using-wildcards-in-sql)
  - [Using wildcards](#using-wildcards)
  - [Underscore wildcard](#underscore-wildcard)
  - [Implications of using wildcards](#implications-of-using-wildcards)
- [Sorting with `ORDER BY`](#sorting-with-order-by)
  - [Sort direction](#sort-direction)
  - [Sorting by columns position](#sorting-by-columns-position)
- [Math operations](#math-operations)
- [Aggregate Functions](#aggregate-functions)
  - [`AVG` function](#avg-function)
  - [`COUNT` function](#count-function)
  - [`MAX` and `MIN` functions](#max-and-min-functions)
  - [`SUM` function](#sum-function)
  - [`DISTINCT`](#distinct)
- [Grouping Data with SQL](#grouping-data-with-sql)
  - [Using `GROUP BY`](#using-group-by)
  - [`WHERE`, `HAVING`, and `GROUP BY`](#where-having-and-group-by)
  - [`GROUP BY` and `ORDER BY`](#group-by-and-order-by)
- [Putting it all together](#putting-it-all-together)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->


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

## Sorting with `ORDER BY`

[video](https://www.coursera.org/learn/sql-for-data-science/lecture/OIQ7a/sorting-with-order-by)

Using `SELECT` will return data in the same order that it was captured in the
database.

Furthermore, the order of data can change when it is updated or deleted.

Without specifying the order of data, its order cannot be assumed.

`ORDER BY`:

- takes the name of one or more columns
- columns are delimited by commas
- data can be sorted by columns that are not retrieved in the `SELECT` statement
- must always be the last statement in a `SELECT` statement

```sql
SELECT *
FROM my_table
WHERE [criteria]
ORDER BY col1, col2;
```

### Sort direction

One can specify the direction to sort data:

- `DESC` - descending
- `ASC` - ascending

The direction keywords apply only to the column name they precede.

```sql
...
ORDER BY DESC name;
```

### Sorting by columns position

Though not readable, and prone to error, one can sort by column number:

```sql
...
ORDER BY 2, 3;
```

Columns numbers are 1-indexed.

## Math operations

[video](https://www.coursera.org/learn/sql-for-data-science/lecture/DYtOe/math-operations)

Available operators:

- `+`
- `-`
- `*`
- `/`

```sql
SELECT
  id
  ,unit_price
  ,unit_sold
  ,unit_price * units_sold AS total_sales
FROM my_table;
```

```sql
SELECT
  id
  ,unit_price
  ,units_sold
  ,unit_weight
  ,(unit_price * units_sold)/unit_weight AS weight_ratio
FROM my_table;
```

## Aggregate Functions

[video](https://www.coursera.org/learn/sql-for-data-science/lecture/O8yes/aggregate-functions)

Aggregate functios allow one to summarise data.

The aggregate functions available are:

- `AVG`
- `COUNT`
- `MIN`
- `MAX`
- `SUM`

`DISTINCT` may be used with some of the aggregate functions to include only
values that are distinct.

Aggregate functions are useful for:

- summarising data
- finding the highest and lowest values
- finding the total number of rows
- finding averages

Aggregate functions are syntactic sugar for the existing math operators.

### `AVG` function

Rows containing NULL values are ignore by the `AVG` function.

```sql
SELECT
  AVG(price) AS avg_price
FROM products;
```

### `COUNT` function

`COUNT` can be used to count rows, including those containing NULL values, or
individual columns, excluding those containing NULL values.

```sql
# count all rows, including those with NULL values
SELECT COUNT(*) AS customers
FROM customers;
```

```sql
# count customers that have emails
SELECT COUNT(email) AS customers_with_email
FROM customers;
```

### `MAX` and `MIN` functions

NULL values are ignored by both `MAX` and `MIN`.

```sql
SELECT MAX(price) AS max_price
FROM products;
```

```sql
SELECT MIN(size) AS smallest_size
FROM shoes;
```

```sql
# get a range of shoe sizes
SELECT
  MAX(size) AS shoe_size_upper
  , MIN(size) as show_size_lower
FROM shoes;
```

### `SUM` function

```sql
# simple query
SELECT SUM(price) as total_price
FROM products;
```

```sql
# less simple query
SELECT SUM(price * units_sold) total_sales
FROM products;
```

### `DISTINCT`

If `DISTINCT` is not defined, `ALL` is assumed.

`DISTINCT` can't be used in conjunction with `COUNT(*)`.

`DISTINCt` doesn't make sense to use with `MIN` or `MAX` since they only return
a single value.

```sql
SELECT COUNT(DISTINCT price) AS distinct_prices
FROM products;
```

## Grouping Data with SQL

[video](https://www.coursera.org/learn/sql-for-data-science/lecture/LWL8X/grouping-data-with-sql)

Grouping data allows one to summarise subsets of data.

Data is grouped using two statements:

- `GROUP BY`
- `HAVING`

Using these statements we can aggregate data by a particular value, e.g.
grouping shoes by shoe size.

When simply aggregating total number of customers in the aggregating module, we
didn't need to do anything specific to get a result.

If, however, another column is specified in the `SELECT` statement, we'd need to
indicate how we'd like the number of customers to be counted. This is where
`GROUP BY` is useful:

```sql
# this will error - we need to specify _how_ to count customer ids when queried
with region
SELECT
  region
  ,COUNT(customer_id) AS total_customers
FROM customers;

# so we indicate that we want to count customers grouped by region
SELECT
  region
  ,COUNT(customer_id) AS total_customers
FROM customers
GROUP BY region;
```

### Using `GROUP BY`

- can contain multiple columns, delimited by commas
- every column in the `SELECT` statement must be present in a `GROUP BY` clause,
    with the exception of the aggregated values
- NULLs can be grouped together if your `GROUP BY` column contains NULLs

### `WHERE`, `HAVING`, and `GROUP BY`

- `WHERE` filters on rows, not groups
- `WHERE` then needs to be defined before `GROUP BY`
- rows eliminated by a `WHERE` clause will not be included in a group
- `HAVING` is the equivalent to `WHERE`, but for groups
- `HAVING` must come after `ORDER BY`

```sql
# count the number of orders, by customer, where there are 2 or more orders for
# each customer
SELECT
  customer_id
  .COUNT(*) AS total_orders
FROM orders
GROUP BY customer_id
HAVING COUNT(*) >= 2;
```

### `GROUP BY` and `ORDER BY`

As with other queries, `GROUP BY` will not sort results. It's good practice to
use `ORDER BY` with `GROUP BY` to organise the results of queries.

```sql
SELECT
  supplier_id
  ,COUNT(*) AS num_products
FROM products
WHERE price >= 4
GROUP BY supplier_id
HAVING COUNT(*) >= 2
ORDER BY name ASC;
```

## Putting it all together

[video](Putting it All Together)

Filtering is useful because:

- you're narrowing down your results
- it increases query and application performance
- makes understanding data easier by:
    - finding specific values
    - finding a range of values
    - finding blank values

Order of key SQL clauses:

```sql
# always required
SELECT [columns]
# required if selecting table from a table
FROM [table]
WHERE [criteria]
# required if calculating aggregates by a group
GROUP BY [columns]
HAVING [criteria]
ORDER BY [columns [ASC|DESC]];
```
