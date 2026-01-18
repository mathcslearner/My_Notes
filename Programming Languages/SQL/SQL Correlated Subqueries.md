As mentioned previously, we can use results from a SQL query in a different SQL query, by nesting it. Let's say we want to use a condition to compare with the value of another element in the DB, then we have to access the result first using a query, then nest that into our current query.

A **correlated subquery** is when the inner query relies on the outer query to function. As an example:

```SQL
SELECT name, continent, population
FROM world x
WHERE population >
  (SELECT AVG(population)
   FROM world y
   WHERE y.continent = x.continent);
```

We can see that here, we are referencing the outer query in world by giving it the name x. The element is then referenced as x.continent. In general, calling the outer DB x and the inner DB y we can then reference columns as x.continent, y.continent, etc.

Since the correlated subquery references columns from the outer query, it is evaluated once per outer row.