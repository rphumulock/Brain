>[PostgreSQL: Documentation: 15: 9.23. Subquery Expressions](https://www.postgresql.org/docs/15/functions-subquery.html)

Subqueries are queries that other parent queries refer to. It is vital to understand the shape of the subquery based off what it will return from it's verbs.

![](assets/images/postgres/queries/Screen%20Shot%202023-01-02%20at%206.32.30%20PM.png)
![](assets/images/postgres/queries/Screen%20Shot%202023-01-02%20at%206.31.46%20PM.png)

---
### SELECT Shape
![](assets/images/postgres/queries/Screen%20Shot%202023-01-02%20at%206.37.14%20PM.png)

---
### FROM Shape
![](assets/images/postgres/queries/Screen%20Shot%202023-01-02%20at%206.48.00%20PM.png)

---
### Join Shape
![](assets/images/postgres/queries/Screen%20Shot%202023-01-02%20at%207.14.24%20PM.png)

---
### WHERE Shape
![](assets/images/postgres/queries/Screen%20Shot%202023-01-02%20at%207.55.16%20PM.png)
![](assets/images/postgres/queries/Screen%20Shot%202023-01-02%20at%208.00.00%20PM.png)

---
### Correlated Queries

Correlated queries are queries where the parent query is referred to in subquery. These are nested n$^2$ queries.

- CTE's can often be used instead and typically perform better
- It is useful to typically design the inner query first

###### Steps in below image:
1. Get all rows in first query and then begin to iterate over them
	- Start with first row from first query
2. Get all rows in second query and then begin to iterate over them
	- Compare first row to each row of second query and see if you keep that row
3. Restart with next row until all rows finished

![[correlatedSubqueries.png]]

---
### Subquery without a from

These are used for calculations typically

![](assets/images/postgres/queries/Screen%20Shot%202023-01-02%20at%209.34.40%20PM.png)