>[PostgreSQL: Documentation: 15: 5.4. Constraints](https://www.postgresql.org/docs/current/ddl-constraints.html)

Data types are a way to limit the kind of data that can be stored in a table. For many applications, however, the constraint they provide is too coarse. For example, a column containing a product price should probably only accept positive values.

- Constraints can allow for some validation on your database. Typically bulk of validation goes on server level but critical rules should go on the database level.
![](datadef8.png)
![](datadef9.png)
- Constraints can be set on columns and tables.
- Can set constraints on `CREATE` or `ALTER`.
- Types:
	- Check Constraints
	- Not-Null Constraints
	- Unique Constraints
	- Primary Keys
	- Foreign Keys
	- Exclusion Constraints
- Can do multi-column for Check /Unique constraints.
![](datadef7.png)

---
### Primary /Foreign Keys

![[datadef1.png]]

![](datadef3.png)
![](datadef4.png)

---
### Primary
>[PostgreSQL: Documentation: 15: 5.4. PK Constraints](https://www.postgresql.org/docs/15/ddl-constraints.html#DDL-CONSTRAINTS-PRIMARY-KEYS)

A primary key constraint indicates that a column, or group of columns, can be used as a unique identifier for rows in the table. This requires that the values be both unique and not null. 

- Primary keys can be a single field or multiple fields which is a compostite primary key.
- Order matters on composite primary keys.

---
### Foreign
>[PostgreSQL: Documentation: 15: 5.4. FK Constraints](https://www.postgresql.org/docs/15/ddl-constraints.html#DDL-CONSTRAINTS-FK)

A foreign key constraint specifies that the values in a column (or a group of columns) must match the values appearing in some row of another table. We say this maintains the _referential integrity_ between two related tables.
 
![[datadef1.png]]

###### Insertion
![](datadef6.png)

###### Deletion
![](datadef5.png)
