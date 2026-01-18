Joins are one of the most important commands in SQL querying, since they allow us to connect different databases together. 

Consider a table with match id, date, stadium, teams, etc. There might also be a table specifically for teams, with more details such as teamid, coach, players, etc. These two tables are linked together by the "team" key, which is the same value. Using joins, we can connect their data together by matching entries with the same key together.

Some terminology:

- PK: Primary key (unique ID for a row in the table)
- FK: Foreign key (column which references the primary key of another table)

For example, teamid is a PK for teams and team in match table would be a FK which connects the two tables together.

The basic syntax for joins is SELECT () FROM (table1) JOIN (table2) ON (table1.key1 = table2.key2), where key1, key2 are the indicators of which keys/columns to use to connect the two tables together

We can join multiple tables together, just chain the join statements.

To join a row from table1 to a row from table2, they need to have matching keys. What if there is no row in table2 such that the key matches?

- INNER JOIN: returns only matching rows
- (LEFT/RIGHT) OUTER JOIN: returns everything, if element in table1 has no matching row in table2 then all the blank fields are padded with NULL

By default, JOIN means INNER JOIN.

To filter for null entries, use IS (NOT) NULL. If we want to not output null and rather output a default value (e.g. None), we can use COALESCE.

It turns out that in SQL, we can also join a table with itself. Consider a table of bus routes, which has information of stops. Suppose we want to know which stops are on the same route together. To do so, we must somehow link stops from the same table together. This motivates a self-join.

The syntax: SELECT * FROM route R1, route R2 WHERE R1.stop=R2.stop

We can also use traditional join syntax:

SELECT * FROM route a JOIN route b ON (a.company = b.company AND a.num AND b.num)

Here is an example of a query using Join:

```SQL
SELECT movie.title
FROM movie
JOIN casting ON movie.id = casting.movieid
JOIN actor ON casting.actorid = actor.id
WHERE actor.name = 'Harrison Ford';
```

This query returns movies where Harrison Ford was an actor.

