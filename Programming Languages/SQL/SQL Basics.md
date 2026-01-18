# SQL Basics

Basic format: SELECT (column names) FROM (database name) WHERE (conditions)

To show all columns, pass '\*' into column names

When we select, we can also select calculations involving column names, such as population/area.

Condition types:

- Exact string: (column) = 'string' or IN ('string1', 'string2')
- Number comparison: (column) >/</= number, BETWEEN x1 AND x2
- Pattern/substring matching: % is a placeholder, eg "%united%" means a substring of "united". The query is formatted as (column) LIKE (pattern)

Note that **must use** '' and not "" for strings.

Logical operators can be used for conditions: AND, OR, NOT, XOR, =, !=

To order the results, use ORDER BY (condition) (ASC/DESC). We can also do ordering using boolean values, 1 meaning yes. To get booleans, can use IN.

We can also use IN/NOT IN on sql query results: 
- Ex: yr NOT IN (SELECT yr FROM table WHERE (conditions)) returns years where the condition didn't happen

Some useful commands:

- ROUND(input, number of decimals) rounds output. Number of decimals can also be negative, eg -1 rounds to nearest ten.
- LENGTH() returns num of chars
- LEFT(str, n) gives first n chars of str
- DISTINCT returns distinct results
- CASE WHEN for casework

Here is an example of a SQL query using the basic commands:

```SQL
SELECT name, population
FROM world
WHERE continent = 'Europe'
  AND population > 10000000
ORDER BY population DESC;

```

This query returns countries in Europe which have population above 10 million, ordered in descending order by population.
