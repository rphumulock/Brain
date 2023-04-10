>[PostgreSQL: Documentation: 15: 7.2. Table Expressions](https://www.postgresql.org/docs/15/queries-table-expressions.html)


---
### Group-By Having
> [PostgreSQL: Documentation: 15: 7.2. The GROUP BY and HAVING Clauses](https://www.postgresql.org/docs/15/queries-table-expressions.html#QUERIES-GROUP)

After passing the `WHERE` filter, the derived input table might be subject to grouping, using the `GROUP BY` clause, and elimination of group rows using the `HAVING` clause.

- HAVING filters after a GROUP-BY aggregation and must refer to that aggregation
- Works on a result group - not like WHERE that works on SELECT columns
- Cannot use aliases in the HAVING clause due to order of operations
- To select a column, that column must be  part of the "GROUP-BY"

![](assets/images/postgres/queries/Screen%20Shot%202023-01-02%20at%2012.14.15%20AM.png)
![](assets/images/postgres/queries/Screen%20Shot%202023-01-02%20at%2012.14.02%20AM.png)

---
### Grouping Sets Rollup and Cube

> https://www.postgresql.org/docs/15/queries-table-expressions.html#QUERIES-GROUPING-SETS

More complex grouping operations than those described above are possible using the concept of _grouping sets_. The data selected by the `FROM` and `WHERE` clauses is grouped separately by each specified grouping set, aggregates computed for each group just as for simple `GROUP BY` clauses, and then the results returned.

- Great for finding things like "subtotal amounts per department"