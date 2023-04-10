>[PostgreSQL: Documentation: 15: Chapter 11. Indexes](https://www.postgresql.org/docs/15/indexes.html)

Indexes are data structures that allow optimizations to queries. Indexes are a common way to enhance database performance. An index allows the database server to find and retrieve specific rows much faster than it could do without an index. Indexes are a quick lookup data structure within Postgres that will tell you exactly where a specific item exists without pulling the entire Heap file into memory. Indexes also add overhead to the database system as a whole, so they should be used sensibly. 

- Automatically created on Primary Key and Unique columns by Postgres.
- Automatically created indexes not listed under PgAdmin.
- Can use query to find indexes: 
	- `SELECT relname, relkind FROM pg_class WHERE relkind = 'i';`
- Put them on table column or columns
- Support up to 32 columns
- Indexes can be on sorted columns
- List all indexes: `SELECT * FROM pg_indexes WHERE schemaname = 'public';`
-  Will show the size of the index for a table:
	- `pg_size_pretty(pg_total_relation_size('table'))`
	- `pg_indexes_size('table')`
- Will show all of the indexes:
	- `pg_stat_all_indexes`

---
### Downsides of Indexes

- As projects scale indexes can become an issue because they cost space
- They might not be used based on the input provided: If you're searching for something isn't unique enough a full table scan might be necessary.

![](assets/images/postgres/performance/Screen%20Shot%202023-01-04%20at%209.06.22%20PM.png)
![](assets/images/postgres/p=erformance/Screen%20Shot%202023-01-04%20at%209.05.21%20PM.png)

---
### Inside Indexes Example Using B-Tree

In this example the comparison of the item being is made on the "data" column. The data is compared and in this case we're using a B-Tree. So the data row is compared and the ctid says which Block/Page the data resides in.

Use extension to analyze index: 
- `CREATE EXTENSION pageinspect;` 
- The first row of table inimage (3) above is a pointer to the first item of the next leaf node. In this specific example the above table is a root node so there isn't a pointer.
- Looking at image (4, 5) - the (first number) of each page part is the block and the (second number) is the index in that block of where the item resides.
![](assets/images/postgres/performance/Screen%20Shot%202023-01-16%20at%207.09.28%20PM.png)
![](assets/images/postgres/performance/Screen%20Shot%202023-01-16%20at%207.17.56%20PM.png)
![](assets/images/postgres/performance/Screen%20Shot%202023-01-16%20at%207.17.32%20PM.png)
![](assets/images/postgres/performance/Screen%20Shot%202023-01-16%20at%208.08.54%20PM.png)
![](assets/images/postgres/performance/Screen%20Shot%202023-01-16%20at%208.40.01%20PM.png)
![](assets/images/postgres/performance/Screen%20Shot%202023-01-04%20at%208.19.17%20PM.png)
![](assets/images/postgres/performance/Screen%20Shot%202023-01-04%20at%208.51.00%20PM.png)

---
### Unique Indexes
>[PostgreSQL: Documentation: 15: 11.6. Unique Indexes](https://www.postgresql.org/docs/15/indexes-unique.html)

Indexes can also be used to enforce uniqueness of a column's value, or the uniqueness of the 
combined values of more than one column.

- Normally the `PRIMARY KEY` of a table is a unique key

---
### Multicolumn
>[PostgreSQL: Documentation: 15: 11.3. Multicolumn Indexes](https://www.postgresql.org/docs/15/indexes-multicolumn.html)

- Make the most selective column first - think about what column would be used in a `WHERE` clause first in the ordering

---
### Partial Indexes
>https://www.postgresql.org/docs/9.3/indexes-partial.html

A _partial index_ is an index built over a subset of a table; the subset is defined by a conditional expression (called the _predicate_ of the partial index).

---
### Types
>[PostgreSQL: Documentation: 15: 11.2. Index Types](https://www.postgresql.org/docs/15/indexes-types.html)

PostgreSQL provides several index types:` B-tree`, `Hash`, `GiST`, `SP-GiST`, `GIN`, `BRIN`, and the extension `bloom`. Each index type uses a different algorithm that is best suited to different types of queries.

B-Tree
>[PostgreSQL: Documentation: 15: Chapter 67. B-Tree Indexes](https://www.postgresql.org/docs/15/btree.html)

Hash
>[PostgreSQL: Documentation: 15: Chapter 72. Hash Indexes](https://www.postgresql.org/docs/15/hash-index.html)

GiSTSP-GiST
>[PostgreSQL: Documentation: 15: Chapter 69. SP-GiST Indexes](https://www.postgresql.org/docs/15/spgist.html)

GIN
>[PostgreSQL: Documentation: 15: Chapter 70. GIN Indexes](https://www.postgresql.org/docs/15/gin.html)

BRIN
>[PostgreSQL: Documentation: 15: Chapter 71. BRIN Indexes](https://www.postgresql.org/docs/15/brin.html)

---
### JSON
>[PostgreSQL: Documentation: 15: 8.14. JSON Types Indexing](https://www.postgresql.org/docs/15/datatype-json.html#JSON-INDEXING)




























  



  

  

---



---

### Derived Data

  

Don't want to store computed values that you can use a simple query to make.

  

![](assets/images/postgres/performance/Screen%20Shot%202023-01-04%20at%201.21.41%20AM.png)

  

---

### General Querying

- Generally you want to make the most impactful filtering happen first - then query it - rather than query it then filter.

- Do not create new array types

- If many to many use an edge or

- If one to many or one to one use a foreign key.