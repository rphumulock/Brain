>[PostgreSQL: Documentation: 15: 5.8. Row Security Policies](https://www.postgresql.org/docs/15/ddl-rowsecurity.html)

- Not enabled by default.
- Is essentially a permanent global `WHERE` clause and in essence can add some overhead to performance when comparison occurs.
- The larger your policies the more performance hit you can take.