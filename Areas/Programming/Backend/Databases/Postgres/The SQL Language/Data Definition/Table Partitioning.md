>[PostgreSQL: Documentation: 15: 5.11. Table Partitioning](https://www.postgresql.org/docs/15/ddl-partitioning.html)

Partitioning refers to splitting what is logically one large table into smaller physical pieces. Partitioning can provide several benefits:

- Query performance can be improved dramatically in certain situations, particularly when most of the heavily accessed rows of the table are in a single partition or a small number of partitions. Partitioning effectively substitutes for the upper tree levels of indexes, making it more likely that the heavily-used parts of the indexes fit in memory.
- When queries or updates access a large percentage of a single partition, performance can be improved by using a sequential scan of that partition instead of using an index, which would require random-access reads scattered across the whole table.
- Bulk loads and deletes can be accomplished by adding or removing partitions, if the usage pattern is accounted for in the partitioning design. Dropping an individual partition using `DROP TABLE`, or doing `ALTER TABLE DETACH PARTITION`, is far faster than a bulk operation. These commands also entirely avoid the `VACUUM` overhead caused by a bulk `DELETE`.
- Seldom-used data can be migrated to cheaper and slower storage media.

--- 
### Tips for Partitioning

- Understand the raw data
- Get an understanding of the history of the actual raw data
- Understand the lower and upper ranges of the data
- Think of the queries users will need from the data
- Consider if the data will be joining other tables/sets
- Rank queries rom simple to complex and put advanced queries on the top to consider
- Collect the common fields for each query and rank them
- Consider fields used in `WHERE` clauses

---
### Think of partitioning when:

- A table becomes very large - a rule of thumb is that the size of the table should exceed the physical memory of the database server i.e. several millions or billions of records. Do not partition for less
- Table can't fit into memory
- Table can be logically divided into relatively equal chunks
- Experiencing unsatisfactory or slow queries after investigating query plans first
- Always test partitions first before implementing to see if they help
- In a large table where certain columns are frequently in the `WHERE` clause - those columns are usually your candidates for partitions
- Business requirements for large tables of historical data

---
### Mistakes when partitioning

- `Constraint_Exlusion` is not enabled
- Missing indexes on partitions - they should have the indexes of the parent table
- Mismatch security on partitions or parent
- Queries not using partition field - try to utilize queries that will be useful working with the partitions
- Overhead of maintaining partitions proportional to the number of partitions - so you don't create unnecessary work and the partitions serve a purpose

---
### PostgreSQL offers built-in support for the following forms of partitioning

- Range Partitioning - The table is partitioned into “ranges” defined by a key column or set of columns, with no overlap between the ranges of values assigned to different partitions. For example, one might partition by date ranges, or by ranges of identifiers for particular business objects. Each range's bounds are understood as being inclusive at the lower end and exclusive at the upper end. For example, if one partition's range is from `1` to `10`, and the next one's range is from `10` to `20`, then value `10` belongs to the second partition not the first.
- List Partitioning - The table is partitioned by explicitly listing which key value(s) appear in each partition. Good to use for known values like months of year or country codes.
- Hash Partitioning - The table is partitioned by specifying a modulus and a remainder for each partition. Each partition will hold the rows for which the hash value of the partition key divided by the specified modulus will produce the specified remainder. Useful when you can't simply or logically parition your data. Useful for reducing table side by spreading rows into smaller partitions.
- Can create a default partition for items that don't fit in the partitions specified

---
### Inheritance and Partitioning using Inheritance
>[PostgreSQL: Documentation: 15: 5.11. Table Partitioning](https://www.postgresql.org/docs/current/ddl-partitioning.html#DDL-PARTITIONING-USING-INHERITANCE)

- Establishes an object oriented like parent/child relationship
- For declarative partitioning, partitions must have exactly the same set of columns as the partitioned table, whereas with table inheritance, child tables may have extra columns not present in the parent.    
- Table inheritance allows for multiple inheritance.
- Declarative partitioning only supports range, list and hash partitioning, whereas table inheritance allows data to be divided in a manner of the user's choosing. (Note, however, that if constraint exclusion is unable to prune child tables effectively, query performance might be poor.)

---
### Multi-Level Partitioning

- Can create partitions of a partition - for instance - create a partition by continent zones like EU, US. Partition EU further since it has more divided areas/countries than US and could grow faster - so we can create a Hash partition on the List partition to make sure we divide into the partition equally.
- It is easy to get carried away and create too many sub-partitions - which increases the time and memory required to optimize and execute queries.

---
### Pruning
>[PostgreSQL: Documentation: 15: 5.11. Table Partitioning](https://www.postgresql.org/docs/15/ddl-partitioning.html#DDL-PARTITION-PRUNING)

_Partition pruning_ is a query optimization technique that improves performance for declaratively partitioned tables.