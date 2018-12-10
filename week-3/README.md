# Week 3 - Subqueries and Joins in SQL

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [Modle introduction](#modle-introduction)
  - [Joins](#joins)
- [Using subqueries](#using-subqueries)
  - [What are subqueries?](#what-are-subqueries)
  - [Problem: Get regions for customers who have ordered freight >= 100](#problem-get-regions-for-customers-who-have-ordered-freight--100)
    - [Combined with a subquery](#combined-with-a-subquery)
  - [Working with subquery statements](#working-with-subquery-statements)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Modle introduction

[video](https://www.coursera.org/learn/sql-for-data-science/lecture/NDsRj/module-introduction)

Subqueries and joins are used to combine data from multiple tables.

### Joins

We'll learn about different types of joins:

- cartesian joins
- inner joins
- left and right joins
- full outer joins
- self joins

We'll also learn how to make queries cleaner and more efficient using aliases
and pre-qualifiers.

## Using subqueries

[video](https://www.coursera.org/learn/sql-for-data-science/lecture/FChaS/using-subqueries)

Using one table is useful, but a lot of value comes from using data from
multiple tables.

One of the ways to combine data from multiple tables is by using subqueries.

### What are subqueries?

Subqueries are queries embedded in other queries.

Relational databases store data in multiple tables. Subqueries merge data from
multiple sources together.

Subqueries are useful for adding additional filtering criteria.

### Problem: Get regions for customers who have ordered freight >= 100

With our currently limited knowledge of single table queries we would have to:

1. retrieve customer IDs for orders with freight over 100 from the `orders`
   table
2. retrieve customer details from the `customers` table
3. combine the two queries

e.g.

```sql
-- get customer ids
SELECT customer_id
FROM orders
WHERE freight >= 100;

-- get regions
SELECT customer_id, company_name, region
FROM customers
WHERE customer_id in (1,2,3,...);
```

#### Combined with a subquery

```sql
SELECT customer_id, comppany_name, region
FROM customers
WHERE customer_id IN (
  SELECT customer_id
  FROM orders
  WHERE freight >= 100
);
```

### Working with subquery statements

With subqueries, SQL will be performing two operations. The subquery is always
performed first.

When troubleshooting, always evaluate the innermost query first, as that is what
SQL will be evaluating first.
