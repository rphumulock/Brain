> [PostgreSQL: Documentation: 15: Chapter 14. Performance Tips](https://www.postgresql.org/docs/15/performance-tips.html)

---
### Explain Analyze
>[PostgreSQL: Documentation: 15: 14.1. Using EXPLAIN](https://www.postgresql.org/docs/15/using-explain.html)

PostgreSQL devises a _query plan_ for each query it receives. Choosing the right plan to match the query structure and the properties of the data is absolutely critical for good performance, so the system includes a complex _planner_ that tries to choose good plans.

- Can be used to plan queries for best performance
- Can analyze query speeds
- Explain Analyze can also be found with more tools in PGAdmin
- Can run as formatted JSON

![](assets/images/postgres/performance/Screen%20Shot%202023-01-16%20at%209.32.55%20PM.png)

Here we can see an example output of `EXPLAIN ANALYZE`.
- If cost is shown that means that line is a node
- Read from bottom up

![](assets/images/postgres/performance/Screen%20Shot%202023-01-16%20at%209.40.24%20PM.png)

---
### Internals of a Query
>[PostgreSQL: Documentation: 15: Chapter 52. Overview of PostgreSQL Internals](https://www.postgresql.org/docs/15/overview.html)

SQL Is a declarative language. You don't tell Postgres how to get your data explicitly. It's your job to provide as much information to the database engine to take the best route to get the data.

Here we give a short overview of the stages a query has to pass to obtain a result.
1.  A connection from an application program to the PostgreSQL server has to be established. The application program transmits a query to the server and waits to receive the results sent back by the server.
2.  The _parser stage_ checks the query transmitted by the application program for correct syntax and creates a _query tree_.
3.  The _rewrite system_ takes the query tree created by the parser stage and looks for any _rules_ (stored in the _system catalogs_) to apply to the query tree. It performs the transformations given in the _rule bodies_. One application of the rewrite system is in the realization of _views_. Whenever a query against a view (i.e., a _virtual table_) is made, the rewrite system rewrites the user's query to a query that accesses the _base tables_ given in the _view definition_ instead.
4. The _planner/optimizer_ takes the (rewritten) query tree and creates a _query plan_ that will be the input to the _executor_. It does so by first creating all possible _paths_ leading to the same result. For example if there is an index on a relation to be scanned, there are two paths for the scan. One possibility is a simple sequential scan and the other possibility is to use the index. Next the cost for the execution of each path is estimated and the cheapest path is chosen. The cheapest path is expanded into a complete plan that the executor can use.
5. The executor recursively steps through the _plan tree_ and retrieves rows in the way represented by the plan. The executor makes use of the _storage system_ while scanning relations, performs _sorts_ and _joins_, evaluates _qualifications_ and finally hands back the rows derived.

- Can be multiple threaded to find path and also execute query at same time
![](assets/images/postgres/performance/Screen%20Shot%202023-01-16%20at%209.17.43%20PM.png)

---
### Query Optimizer
>[PostgreSQL: Documentation: 15: 52.5. Planner/Optimizer](https://www.postgresql.org/docs/15/planner-optimizer.html)

The structure of a query plan is a tree of _plan nodes_. Nodes at the bottom level of the tree are scan nodes: they return raw rows from a table. There are different types of scan nodes for different table access methods: sequential scans, index scans, and bitmap index scans. There are also non-table row sources, such as `VALUES` clauses and set-returning functions in `FROM`, which have their own scan node types. If the query requires joining, aggregation, sorting, or other operations on the raw rows, then there will be additional nodes above the scan nodes to perform these operations.

Parent Node (Cost-..)
	Child Node 1 (Cost..)
		Child Node 2 (Cost...)

- To get node types :  `SELECT * FROM pg_am`
- What is the fastest path and what is the time spent reasoning about this fastest path? Every operation has it's own cost that is calculated `SELECT`, `INSERT`, `SORTING` etc.
- Query optimizer dissects statements into various kinds of nodes to work with that comes with their costs

Sequential Nodes: Used when filtering is limited and you're mostly getting the entire table

Index Nodes: [[Indexes]]
- Index Scan - seeking the tuples from the table heap with the index
- Index Only Scan - getting the data directly from the index file and not needing to access the heap - this happens if we index on a column and search on that specific column
- Bitmap Index Scan - builds a memory bitmap of where tuples that satisfy statements are located

Join Nodes
- Hash
	- Hash based aggregation and hash based processing in subqueries - typically things that result to a boolean
	- Hash joins can take up a lot of space and go over the work_memory that's configured for this server and will become batched: `SHOW work_mem`
- Merge
	- Merge Join - Joins two children already sorted by their shared join key
- Nested Loop
	- Nested Loop - Can be inefficient


---

EDIT


  

## Overview

> https://www.postgresql.org/docs/15/performance-tips.html

  

---

### The Rule System

> https://www.postgresql.org/docs/15/rules.html

  

---

### Full Table Scans

We want to limit how much data is being moved from our Hard Drive (Heap File) and into memory. When you query - the binary has to be pulled from the Heap files and into memory.

  

How can we limit how much data is moved from HDD (Heap File) and into memory?

  

![](assets/images/postgres/performance/Screen%20Shot%202023-01-04%20at%208.08.34%20PM.png)

  

After we move the data into memory - we're also performing a search record by record on that data. Like a for loop (Full Table Sequential Scan).

  

![](assets/images/postgres/performance/Screen%20Shot%202023-01-04%20at%208.09.05%20PM.png)

  

Full Table scan isn't ALWAYS bad but if it is happening we want to investigate to see if we can do things better.

  

---

### Explain Analyze

> https://www.postgresql.org/docs/15/using-explain.html

  

> https://www.postgresql.org/docs/15/using-explain.html#USING-EXPLAIN-ANALYZE

  

Can be used to plan queries for best performance.

  

Can analyze query speeds.

  

![](assets/images/postgres/performance/Screen%20Shot%202023-01-16%20at%209.32.55%20PM.png)

  

![](assets/images/postgres/performance/Screen%20Shot%202023-01-16%20at%209.40.24%20PM.png)

  

Explain Analyze can also be found with more tools in PGAdmin.

  

---

### Basic Query Tuning

  

Steps that take place when querying.

  

![](assets/images/postgres/performance/Screen%20Shot%202023-01-16%20at%209.17.43%20PM.png)

  

1. Parser builds a query tree from your query.

1. Rewrites the query to be more efficient if it can. Possibly using Views.

1. Planner tris to pick the fastest way to fetch data: Could use indexes, could just grab data nd search one by one.

  

---

### Query Planning

> https://www.postgresql.org/docs/15/runtime-config-query.html

  

Example of calculations to come up with the best plan. Postgres saves some statistics for items in our database.

  

Can make rough calculations to see what the best plan is.

  

![](assets/images/postgres/performance/Screen%20Shot%202023-01-16%20at%2010.29.07%20PM.png)

  

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

### Derived Data

  

Don't want to store computed values that you can use a simple query to make.

  

![](assets/images/postgres/performance/Screen%20Shot%202023-01-04%20at%201.21.41%20AM.png)

  

---

### General Querying

- Generally you want to make the most impactful filtering happen first - then query it - rather than query it then filter.

- Do not create new array types

- If many to many use an edge or

- If one to many or one to one use a foreign key.