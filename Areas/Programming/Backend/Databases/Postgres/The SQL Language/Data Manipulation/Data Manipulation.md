>[PostgreSQL: Documentation: 15: Chapter 6. Data Manipulation](https://www.postgresql.org/docs/15/dml.html)

### Populate 
>[PostgreSQL: Documentation: 15: 2.4. Populating a Table With Rows](https://www.postgresql.org/docs/current/tutorial-populate.html)

>[PostgreSQL: Documentation: 15: INSERT AND UPSERT](https://www.postgresql.org/docs/current/sql-insert.html)

>[PostgreSQL: Documentation: 15: SELECT INTO](https://www.postgresql.org/docs/current/sql-selectinto.html)


Create a duplicate table with no data:
`CREATE TABLE dup AS (SELECT * FROM table) WITH NO DATA`

UPSERT works similarly to using INSERT with ON CONFLICT
```
-- Lets insert a record, on conflict do nothing
INSERT INTO t_tags (tag)
VALUES ('Pen')
ON CONFLICT (tag)
DO
	NOTHING;

-- Lets insert a record, on conflict set new values
INSERT INTO t_tags (tag)
VALUES ('Pen')
ON CONFLICT (tag)
DO
	UPDATE SET
		tag = EXCLUDED.tag,
		update_date = NOW();
```

---
### Update
[PostgreSQL: Documentation: 15: 2.8. Updates](https://www.postgresql.org/docs/current/tutorial-update.html)

---
### Delete
[PostgreSQL: Documentation: 15: 2.9. Deletions](https://www.postgresql.org/docs/current/tutorial-delete.html)

```
-- Single row

DELETE FROM customers
WHERE customer_id = 9;

-- Table

DELETE FROM customers;
```

---
### Return Data after modifications
>[PostgreSQL: Documentation: 15: 6.4. Returning Data from Modified Rows](https://www.postgresql.org/docs/current/dml-returning.html)


