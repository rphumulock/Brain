### The Rule System
>[PostgreSQL: Documentation: 15: Chapter 41. The Rule System](https://www.postgresql.org/docs/15/rules.html)

---
### Query Planning
> [PostgreSQL: Documentation: 15: 20.7. Query Planning](https://www.postgresql.org/docs/15/runtime-config-query.html)

Example of calculations to come up with the best plan. Postgres saves some statistics for items in our database.

- Can make rough calculations to see what the best plan is
- Pages loaded randomly can be slower than pages loaded sequentially
  
![](assets/images/postgres/Screen%20Shot%202023-01-16%20at%2010.29.07%20PM.png)

Pages loaded randomly can be slower than pages loaded sequentially.

![](assets/images/postgres/performance/Screen%20Shot%202023-01-16%20at%2010.29.23%20PM.png)
![](assets/images/postgres/performance/Screen%20Shot%202023-01-16%20at%2010.29.52%20PM.png)

Come up with a cost analysis. Loading pages from hard drive slower than doing operations on rows.

![](assets/images/postgres/performance/Screen%20Shot%202023-01-16%20at%2010.30.33%20PM.png)
![](assets/images/postgres/performance/Screen%20Shot%202023-01-16%20at%2010.35.34%20PM.png)

Sequential page loading cost is the "baseline" measurement for time.

![](assets/images/postgres/performance/Screen%20Shot%202023-01-16%20at%2010.46.16%20PM.png)
![](assets/images/postgres/performance/Screen%20Shot%202023-01-16%20at%2011.00.04%20PM.png)
![](assets/images/postgres/performance/Screen%20Shot%202023-01-16%20at%2011.00.25%20PM.png)

Postgres might not always use index. Explain Analyse can help you show this.

![](assets/images/postgres/performance/Screen%20Shot%202023-01-16%20at%2011.10.40%20PM.png)

---
### Full Table Scans
>[Full table scan - Wikipedia](https://en.wikipedia.org/wiki/Full_table_scan)

A **full table scan** (also known as a **sequential scan**) is a scan made on a database where each row of the table is read in a sequential (serial) order and the columns encountered are checked for the validity of a condition.

- We want to limit how much data is being moved from our Hard Drive (Heap File) and into memory. When you query - the binary has to be pulled from the Heap files and into memory.
- Full Table scan isn't always bad but if it is happening we want to investigate to see if we can do things better.

How can we limit how much data is moved from HDD (Heap File) and into memory?

![](assets/images/postgres/performance/Screen%20Shot%202023-01-04%20at%208.08.34%20PM.png)

After we move the data into memory - we're also performing a search record by record on that data. Like a for loop (Full Table Sequential Scan).

![](assets/images/postgres/performance/Screen%20Shot%202023-01-04%20at%208.09.05%20PM.png)

---
### Where does Postgres store data?
>https://www.postgresql.org/docs/15/runtime-config-file-locations.html

Can find the directory where Postgres lives. Postgres stores files in this directory. Under base folder which lists each database files exist and each file represents something in Postgres.
- `SHOW directory`

Will show each database ID with the respective database.
- `SELECT oid, datname FROM pg_database`

Shows all the different items in the database.
> https://www.postgresql.org/docs/15/catalog-pg-class.html

`SELECT * FROM pg_class`
  
---

### Nitty Gritty
>[PostgreSQL: Documentation: 15: Chapter 73. Database Physical Storage](https://www.postgresql.org/docs/current/storage.html)

Tables are stored in Heap Files. Each Heap file contains blocks/pages which each contain many tuples/items.

- Each Heap file represents a table.
- Each Block/Page represents stores some number of rows.
- Each tuple represents an item or row.

![](assets/images/postgres/internals/Screen%20Shot%202023-01-04%20at%205.49.47%20PM.png)

![](assets/images/postgres/internals/Screen%20Shot%202023-01-04%20at%205.51.28%20PM.png)

![](assets/images/postgres/internals/Screen%20Shot%202023-01-04%20at%205.55.05%20PM.png)

![](assets/images/postgres/internals/Screen%20Shot%202023-01-04%20at%206.04.29%20PM.png)

---
### View of a table

- Each hex value is one byte (8 bits binary).
- 8000 bytes per block/page.

![](assets/images/postgres/internals/Screen%20Shot%202023-01-04%20at%207.02.17%20PM.png)

![](assets/images/postgres/internals/Screen%20Shot%202023-01-04%20at%207.11.30%20PM.png)

![](assets/images/postgres/internals/Screen%20Shot%202023-01-04%20at%207.04.26%20PM.png)

![](assets/images/postgres/internals/Screen%20Shot%202023-01-04%20at%207.10.23%20PM.png)

---
### PageHeaderData
![](assets/images/postgres/internals/Screen%20Shot%202023-01-04%20at%207.19.50%20PM.png)

Example:
pd_lower represents from start of block to when Free Space! starts. E4 and the next byte represents this - we can highlight E4 since that's the start of the memory holding the information. 228 bytes is where free space starts.

![](assets/images/postgres/internals/Screen%20Shot%202023-01-04%20at%207.14.16%20PM.png)

ItemId storing the starting byte to the item and the length of the Item. NOT the id of the item (row).
![](assets/images/postgres/internals/Screen%20Shot%202023-01-04%20at%207.24.22%20PM.png)