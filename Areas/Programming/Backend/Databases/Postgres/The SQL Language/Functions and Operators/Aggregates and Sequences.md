>[PostgreSQL: Documentation: 15: Chapter 9. Functions and Operators](https://www.postgresql.org/docs/15/functions.html)

---
### Sequences
>[PostgreSQL: Documentation: 15: 9.17. Sequence Manipulation Functions](https://www.postgresql.org/docs/15/functions-sequence.html)
>[PostgreSQL: Documentation: 15: CREATE SEQUENCE](https://www.postgresql.org/docs/15/sql-createsequence.html)
>[PostgreSQL: Documentation: 15: ALTER SEQUENCE](https://www.postgresql.org/docs/15/sql-altersequence.html)

Sequence objects are special single-row tables created with CREATE SEQUENCE. Sequence objects are commonly used to generate unique identifiers for rows of a table.

- Can be used between multiple tables

---
### Aggregate Functions
>[PostgreSQL: Documentation: 15: 9.21. Aggregate Functions](https://www.postgresql.org/docs/15/functions-aggregate.html)>

Aggregate functions compute a single result from a set of input values.

>List of Aggregate Functions: https://www.postgresql.org/docs/15/functions-aggregate.html

>https://www.postgresql.org/docs/15/sql-expressions.html#SYNTAX-AGGREGATES

An _aggregate expression_ represents the application of an aggregate function across the rows selected by a query. An aggregate function reduces multiple inputs to a single output value, such as the sum or average of the inputs.

- Can apply extra conditionals to an aggregate function (such as `FILTER`)
- `COUNT(field)` won't count null values
- `COUNT(*)` counts the number of rows - which includes null fields

![](assets/images/postgres/quries/Screen%20Shot%202023-01-02%20at%2012.31.25%20AM.png)

