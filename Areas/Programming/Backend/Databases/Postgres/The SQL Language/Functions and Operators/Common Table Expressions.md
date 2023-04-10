>[PostgreSQL: Documentation: 15: 7.8. WITH Queries (Common Table Expressions)](https://www.postgresql.org/docs/15/queries-with.html)

`WITH` provides a way to write auxiliary statements for use in a larger query. These statements, which are often referred to as Common Table Expressions or CTEs, can be thought of as defining temporary tables that exist just for one query.

- Can be used to perform calculations and join with and outer table for better display
- Can be used to do delete/insert operations simultaneously

![](assets/images/postgres/queries/Screen%20Shot%202023-01-16%20at%2011.28.58%20PM.png)

---
### Recursive
> [PostgreSQL: Documentation: 15: 7.8. WITH Queries (Common Table Expressions)](https://www.postgresql.org/docs/15/queries-with.html#QUERIES-WITH-RECURSIVE)

- Process recursively iterates using two tables: a working table and a results table

- Recursive CTE's typically follow a common structure:
```
WITH RECURSIVE recursive(x) AS
(
	-- non recursive statement

	UNION [ALL]

	-- recursive statement and exit conditions
)
```

![](assets/images/postgres/queries/Screen%20Shot%202023-01-17%20at%2012.00.29%20AM.png)

![[recursiveCTE.png]]

---
### Materialized CTE's
>[PostgreSQL: Documentation: 15: 7.8. WITH Queries (Common Table Expressions)](https://www.postgresql.org/docs/15/queries-with.html#id-1.5.6.12.7)

Commonly used CTE's can be materialized to save computation.
