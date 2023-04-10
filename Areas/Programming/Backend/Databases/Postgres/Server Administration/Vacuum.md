>[PostgreSQL: Documentation: 15: Chapter 25. Routine Database Maintenance Tasks](https://www.postgresql.org/docs/15/maintenance.html)

Can see vacuuming for tables from this query:
`SELECT * FROM pg_stat_all_tables`

auto-vacuum wont reclaim unused space - must manually vacuum