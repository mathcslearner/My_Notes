Aggregate functions are functions which take in many values and output a single value back. They are used to summarize data over certain columns. Here are the core aggregate functions in SQL:

- COUNT()
- SUM()
- AVG()
- MAX()
- MIN()

The use of all these functions is straightforward. To remove duplicates, we can use DISTINCT(). 

Recall that for normal conditions, we use WHERE. If we want to run a condition on an aggregate function, we would have to use HAVING. 

Ex: HAVING COUNT(list) > 1

Furthermore, when using aggregate functions, you have to tell the DB how to combine the different values by using GROUP BY on the non-aggregated columns. All columns must either be used in an aggregated function or in GROUP BY.

If you just want the number of elements of something, COUNT(1) or COUNT(\*) does the trick.

Here is a clean example of a SQL query using aggregate functions:

```SQL
SELECT
  continent,
  COUNT(*)        
  SUM(population)
  AVG(population)
FROM world
GROUP BY continent;
```

This query returns a list of continents, and for each continent, we get the following information back: the number of countries, the total population in the continent, and the average population.