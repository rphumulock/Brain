>[PostgreSQL: Documentation: 15: 9.26. System Information Functions and Operators](https://www.postgresql.org/docs/15/functions-info.html)

>[PostgreSQL: Documentation: 15: 9.27. System Administration Functions](https://www.postgresql.org/docs/15/functions-admin.html)

Random helpful queries for internal Postgres tables.

All users:
`SELECT * from pg_user`

Database sizes:
`SELECT db, pg_size_pretty(pg_database_size(db) from pg_database`

Schemas Tables Views etc:
`SELECT * FROM information_schema.{views, table, schemata, columns}`

To see all running queries (can filter to show only running quieries as well):
`SELECT * from pg_stat_activity*`

To terminate idle processes/queries get PID from above and then:
`SELECT pg_cancel_backend("PID)"`

To get the number of live and dead rows (marked deleted but not fully removed):
`SELECT relname, n_live,tup, n_dead_tup, FROM pg_stat_user_tables`

```
SELECT 
	CURRENT_CATAOLG, 
	CURRENT_DATABASE(), 
	CURRENT_SCHEMA, 
	CURRENT_USER,
	SESSION_USER,
	VERSION(),
	
	hasDatabasePrivilege('database', 'ACTION'),
	hasSchemaPrivilege('database', 'ACTION')
	hasTablePrivilege('database', 'ACTION')
	hasAnyColumnPrivilege('database', 'ACTION')
	hasColumnPrivilege('user', 'database', 'column_name', 'ACTION')
	

ACTION = Query or CRUD actions (SELECT/CREATE/INSERT/USAGE) etc
```

---
### Import/Export to CSV

Import:
`\copy table_name(column_headers1, column_headers2...) FROM \location\file_name DELIMETER ',' CSV HEADER

Export:
`COPY table TO \location\file_name` DELIMETER ',' CSV HEADER
`\copy table TO \location\file_name` DELIMETER ',' CSV HEADER

